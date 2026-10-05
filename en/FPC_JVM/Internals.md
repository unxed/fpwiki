# FPC JVM/Internals

│ **English (en)** │  **[русский (ru)](<../../ru/FPC_JVM/Internals.md>)** │

## Contents

  * 1 Compiler
    * 1.1 Compile-time code generation
    * 1.2 Typed constants
    * 1.3 Nested routines
    * 1.4 Code generator
      * 1.4.1 Overview
      * 1.4.2 ncgutil
      * 1.4.3 ngenutil
      * 1.4.4 Limitations
  * 2 RTL
    * 2.1 General
    * 2.2 Debugging



# Compiler

## Compile-time code generation

The implementation of several language features on the JVM target require the compiler to generate wrappers of all sorts: 

  * records must be wrapped by classes that contain several helpers (initialization, deep copy)
  * if a public property accesses a private/protected field or method, the compiler has to generate a public method that wraps these accesses since the property can be used from code that does not have access to the original symbol (resulting in class verification errors)
  * enumerations are classes in the JVM and several default fields and methods must be implemented
  * constructors are not automatically inherited in the JVM class model, so the compiler has to do that
  * procedure variables are not supported by the JVM and must be emulated by classes. These classes contain a method that boxes all primitive parameters types and then dispatches them via _java.lang.reflect.Method.invoke()_.
  * virtual constructors and virtual class methods are not supported and must be wrapped by the compiler
  * ...



While it is possible to generate all the internal definitions/nodes to implement all this functionality, plain Pascal code is much quicker and easier to write and maintain. For this reason the new _symcreat_ unit supports parsing Pascal code injected into the source code stream at compile time. There is currently functionality to parse both method declarations and implementations, and also for typed const declarations. 

Once a method declaration has been parsed or otherwise generated, you can set the corresponding _procdef'_ s _synthetickind_ field to any of the _tsk_*_ constants defined in the _symdef_ unit to indicate what kind of code should be generated for it. The _symcreat.add_synthetic_method_implementations_for_struct()_ routine handles calling the appropriate helper routine to generate the actual Pascal code, parse it and generate the machine/byte code. 

Caveats: 

  * the automatically generated Pascal code is always parsed in _objfpc_ mode, regardless of the syntax mode of the current unit
  * the automatically generated code cannot do anything that is not allowed in regular source code. This means that all used identifiers must be valid Pascal identifiers, regular scoping rules apply, etc. In particular, if you duplicate a _procdef_ from another unit and give it a new implementation, some of its parameter types may actually not be visible in the current unit (and hence you cannot use those type names in the Pascal code).



## Typed constants

There are no generic data sections in Java class files. As a result, data initialization must be performed using explicit assignments in the class initialization (~unit initialization). 

Most of the functionality of the _ptconst_ unit has been split off into _ngtcon_. Generating the typed constant initialization is now handled by a _ttypedconstbuilder_ class. This abstract class has two child classes. _tasmlisttypedconstbuilder_ handles the typed constant initialization for native targets. _tnodetreetypedconstbuilder_ handles the typed constant generation for the JVM target and possible other future targets where typed constants have to be initialized via explicit assignments. 

Both classes can still be further overridden by target-specific classes. Depending on whether or not the current target is in the _systems.systems_typed_constants_node_init_ set, the asmlist or nodetree class is instantiated. 

## Nested routines

The JVM has no stack or frame pointer that can be passed around, so another way is required to support accessing local variables or parameters from an outer routine in an inner routine. This has been solved by moving such parameters and local variables in a local record in the outer routine. This record is called the _parentfpstruct_. 

A pointer to this _parentfpstruct_ is passed as hidden parameter to any child routine, so they can access the variables and parameters that way. In case a nested routine calls a more deeply nested nested routine, the _parentfpstruct_ parameter itself will also be migrated to the new _parentfpstruct_ , so that via daisy chaining all _parentfpstruct_ s are reachable from everywhere. 

If a target requires that nested routines are handled this way rather than via the default framepointer technique, all it has to do is use the _ncgnstld_ (instead of _ncgld_) and _ncgnstmm_ (instead of _ncgmem_) units, and/or derive from the classes defined in those units rather than those defined in their generic equivalents. 

## Code generator

### Overview

There is now a high level code generator class, _thlcgobj_ (hlcgobj.pas). The difference with the existing code generator is that all size descriptions are _tdef_ s rather _tcgsize_ s. 

The low level and high level code generators can be seamlessly mixed for targets that have a complete implementation of the low level one (all but the JVM target for now). There is a _tcghl2ll_ (hlcg2ll.pas) class that translates all high level _hlcgobj_ calls into low level _tcg_ calls, which is used for all non-JVM targets. 

Apart from _tcgsize_ -> _tdef_ changes, a number of _thlcgobj_ methods also received extra size parameters in case _tlocation_ s are involved. The reason is that a _tlocation_ has a _tcgsize_ member, and replacing it with a _tdef_ is not possible without messing up the layer separations in the compiler (a different kind of high level location would be required). 

Several ncg*.pas units have been wholly or partly converted to use the new high level code generator infrastructure, so they can be used by both the JVM target and the existing targets. 

### ncgutil

A number of routines from ncgutil.pas has been moved to thlcgobj.pas so they can be overridden by different targets. 

### ngenutil

A new unit _ngenutil_ has been added that contains node-related routines from other units (mainly from _nutils_ and _pmodule_) that can now be overridden with target-specific versions (e.g., _jvm/njvmutil_) 

### Limitations

The high level code generator currently does not support the following features due to the inability of expressing them using high level types (_tdef_ s): 

  * LOC_(C)SUBSETREF, LOC_(C)SUBSETREG: would need additional type information
  * LOC_(C)MM(X)REGISTER: the compiler currently has no way to specify vector types using tdef(s) (no difference between regular arrays and vector types)



Since the JVM target does not require these features and since the other targets use the existing low level code generator for them, this can be solved in the future. 

# RTL

## General

Much of the existing RTL code is not usable for the JVM target, because it makes heavy use of pointers to arbitrary data, move and fillchar, none of which are available on the JVM platform. 

Rather than immediately putting the existing RTL files full of ifdefs, several files from _rtl/inc_ have been duplicated into the _rtl/java_ directory and modified there. As development progressed, some became more like the original ones and in one case (generic.inc) an original file is even already used. 

Some details: 

  * _inc/systemh.inc_ has been split into _java/systemh_types.inc_ and _javasystemh.inc_. The reason is that the function/procedure definitions from _inc/systemh.inc_ require that the Java wrappers (for ShortString, AnsiString) and base types (for records, sets, procvars) are already declared. However, these wrappers and base types in turn depend on types that are declared earlier on in _inc/systemh.inc_. Hence the split. Maybe this can be done to the generic version as well over time -- it doesn't even require changes to the RTLs of other targets if _inc/systemh.inc_ afterwards simply includes both files resulting from the split. There are also some other changes to those files, mostly commented out things that are not relevant or not (yet) supported on the JVM target.
  * _java/jsystem.inc_ is a (not very heavily) modified version of _inc/system.inc_. This one can probably also be unified with the generic version over time without too much trouble.
  * _java/compproc.inc_ mainly has a bunch of routines commented out, and some extra JVM-specific added. Over time, the Java-specific ones should be moved to a Java-specific include file and with possibly a few extra ifdefs the generic one from _inc_ should be usable
  * _java/jstrings.inc_ contains the implementation of the _ShortstringClass_ type along with some shortstring helpers. Where possible, the JVM RTL uses the existing helpers from _inc/sstrings.inc_. The only changes where that in some cases calls to _move_ were replaced with calls to helpers that are replaced with JVM-specific helpers, like in [this example](<http://svn.freepascal.org/cgi-bin/viewvc.cgi/branches/jvmbackend/rtl/inc/generic.inc?r1=18907&r2=18906&pathrev=18907>).
  * _java/jastrings.inc_ and _java/justrings.inc_ are basically the same story as _java/jstrings.inc_.
  * _java/jpvar(h).inc_ contains the base class used for dispatching procedure variables
  * _java/jset(h).inc_ contains the parent class for all integer-based sets and a number of set helpers
  * _java/objpas(h).inc_ contains the skeleton _TObject_ definition and the _TVarrec_ record. Since variant records make no real sense in Java (fields cannot overlap the same memory area), _TVarrec_ has been implemented by boxing the value and storing the object into a single _Value_ field. Extracting the value again happens via simple accessor functions. Since you're not supposed to write into _array of const_ elements, this should work fine for most code that does not rely on the overlay behaviour.
  * _java/jrec(h).inc_ contains the parent class for all record types
  * _java/jdynarr(h).inc_ contains the support code for dynamic arrays
  * _java/rtti.inc_ contains the helpers for initializing arrays. There are more than on native targets, because e.g. arrays of records on the JVM target have to be initialized similarly to arrays of ansistring on native targets: all record instances in the array must be explicitly allocated.
  * _java/jtvar(h).inc_ contains the support for _threadvar_



## Debugging

Because the plain Java class format does not support line number information for include files, it is currently not possible to generate line number information for any of the code in the RTL include files. If you really need to debug some code in an include file, the only workaround right now is to temporarily copy its contents into _java/system.pp_

---

_Source: [https://wiki.freepascal.org/FPC_JVM/Internals](https://web.archive.org/web/20250514175316/https://wiki.freepascal.org/FPC_JVM/Internals)_
