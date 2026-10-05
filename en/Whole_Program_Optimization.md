# Whole Program Optimization

│ **English (en)** │

## Contents

  * 1 Overview
  * 2 General principles
  * 3 How to use
    * 3.1 Generate WPO feedback file
    * 3.2 Use generated WPO feedback file
    * 3.3 Concrete example
    * 3.4 Concrete Lazarus IDE example
  * 4 When to use
  * 5 Available whole program optimizations
    * 5.1 All optimizations
    * 5.2 Whole program devirtualization
    * 5.3 Optimise virtual method tables
    * 5.4 Symbol liveness
  * 6 Format of the WPO feedback file



## Overview

Traditionally, compilers optimize a program procedure by procedure, or at best compilation unit per compilation unit. Whole program optimization, abbreviated WPO, means that the compiler considers all compilation units that make up a program or library and optimizes them using the comprehensive knowledge of how they are used together in this particular case. 

The way WPO generally works is as follows: 

  * you compile the program normally, telling the compiler to store various bits of information into a _feedback file_
  * you _recompile_ the program (and optionally all units that it uses) with WPO, providing the feedback file as extra input to the compiler



In some implementations, the compiler generates some kind of intermediary code (e.g., byte code) and the linker performs all WPO along with the translation to the target ISA. In case of FPC however, the scheme followed is the one described above. 

## General principles

A few general principles have been followed when designing the FPC implementation of WPO: 

  * All information necessary to generate a WPO feedback file for a program is always stored in the PPU files. This means that you can e.g. use a generic RTL for WPO (even though the RTL itself will then not be optimized, your program and its units can be correctly optimized because the compiler knows everything it has to know about all RTL units);
  * The generated WPO feedback file is plain text. The idea is that it should be easy to inspect this file by hand, and to add information to it produced by external tools if desired (e.g., profile information);
  * The implementation of the WPO subsystem in the compiler is very modular, so it should be easy to plug in additional WPO information providers, or to choose at run time between different information providers for the same kind of information. At the same time, the interaction with the rest of the compiler is kept to a bare minimum to improve maintainability;
  * It is possible to generate a WPO feedback file while at the same time using another one as input. In some cases, using this second feedback file as input during a third compilation can further improve the results.



## How to use

### Generate WPO feedback file

First of all, compile your program (or library) and all of its units as you would normally do, except that when compiling the main program/library you add 
    
    
    -FW/path/to/feedbackfile.wpo -OW<selected_wpo_options>
    

. 

The compiler will then, right after your program has been linked, collect all necessary information to perform the requested WPOs during a successive compilation run, and store this information in `/path/to/feedbackfile.wpo`. 

### Use generated WPO feedback file

To actually apply the WPOs, recompile the program/library and all or some of the units that it uses, using 
    
    
    -Fw/path/to/feedbackfile.wpo -Ow<selected_wpo_options>
    

, thereby pointing the compiler to the feedback file generated in the previous step. 

The compiler will then read the information collected about the program during the previous compiler run, and use it during the current compilation of units and/or program/library. 

Units not recompiled during the second pass will obviously not be optimized, but they will still work correctly when used together with the optimized units and program/library. 

### Concrete example

The example below refers to [`twpo4.pp`](<https://gitlab.com/freepascal.org/fpc/source/-/blob/main/tests/test/opt/twpo4.pp>). 

Compile the program a first time, collecting feedback in `twpo4-1.wpo` in the current directory. The compiler will record which classes are created in the program (`tchild1` and `tchild2`), and after the linking step also that the `notcalled` procedure is in fact not called. 

Afterwards, save the generated assembler code in `twpo4.s1` for later comparison. 
    
    
    $ ppn37 -FWtwpo4-1.wpo -OWall -CX -XX -Xs- -al twpo4
    Free Pascal Compiler version 2.3.1 [2008/12/11] for i386
    Copyright (c) 1993-2008 by Florian Klaempfl
    Target OS: Darwin for i386
    Compiling twpo4.pp
    Assembling program
    Linking twpo4
    66 lines compiled, 0.6 sec
    $ mv twpo4.s twpo4.s1
    

Now compile the program a second time, using the information in `twpo4-1.wpo` and collecting new information in `twpo4-2.wpo`. At this point, the compiler knows that no instance of the `tbase` class is created in the program, and therefore it replaces all regular entries in its virtual method table with references to `FPC_ABSTRACTERROR`. Virtual class methods cannot be eliminated this way, since they can also be called without constructing an instance of this type. 

Because the compiler knows from the previously generated feedback file that the `notcalled` procedure is never called, it will also record that only an instance of `tchild1` is created, as the `tchild2` instance was only created inside this `notcalled` procedure. For optimization purposes, it will however still consider that an instance of `tchild2` may be created, as for optimization purposes it still uses the feedback file generated during the previous compilation. 

Afterwards, we again save the generated assembler code for later comparisons. 
    
    
    $ ppn37 -FWtwpo4-2.wpo -OWall -Fwtwpo4-1.wpo -Owall -CX -XX -Xs- -al twpo4
    Free Pascal Compiler version 2.3.1 [2008/12/11] for i386
    Copyright (c) 1993-2008 by Florian Klaempfl
    Target OS: Darwin for i386
    Compiling twpo4.pp
    Assembling program
    Linking twpo4
    66 lines compiled, 0.1 sec
    $ mv twpo4.s twpo4.s2
    

Compile the program a final time, using the information collected in `twpo4-2.wpo`. This time, the compiler knows that only a `tchild1` instance is created, and will therefore turn the `bb.test` call from a virtual method call into a static method call (because it knows that even though the type of `bb` is `tbase`, in practice it can only be a `tchild1` since no `tbaseh` or other `tbase`-descendent class instance is created). 
    
    
    $ ppn37 -Fwtwpo4-2.wpo -Owall -CX -XX -al twpo4
    Free Pascal Compiler version 2.3.1 [2008/12/11] for i386
    Copyright (c) 1993-2008 by Florian Klaempfl
    Target OS: Darwin for i386
    Compiling twpo4.pp
    Assembling program
    Linking twpo4
    66 lines compiled, 0.1 sec
    

Now, let's have a look at the differences between the generated assembler files. After the first recompilation: 
    
    
    $ diff -u twpo4.s1 twpo4.s2
    
    
    
    --- twpo4.s1	2008-12-11 18:59:55.000000000 +0100
    +++ twpo4.s2	2008-12-11 19:00:15.000000000 +0100
    @@ -214,15 +214,15 @@
     	.long	0,0,0
     	.long	FPC_EMPTYINTF
     	.long	0
    -	.long	_SYSTEM_TOBJECT_$__DESTROY
    +	.long	FPC_ABSTRACTERROR
     	.long	_SYSTEM_TOBJECT_$__NEWINSTANCE$$TOBJECT
    -	.long	_SYSTEM_TOBJECT_$__FREEINSTANCE
    -	.long	_SYSTEM_TOBJECT_$__SAFECALLEXCEPTION$TOBJECT$POINTER$$LONGINT
    -	.long	_SYSTEM_TOBJECT_$__DEFAULTHANDLER$formal
    -	.long	_SYSTEM_TOBJECT_$__AFTERCONSTRUCTION
    -	.long	_SYSTEM_TOBJECT_$__BEFOREDESTRUCTION
    -	.long	_SYSTEM_TOBJECT_$__DEFAULTHANDLERSTR$formal
    -	.long	_P$PROGRAM_TBASE_$__TEST
    +	.long	FPC_ABSTRACTERROR
    +	.long	FPC_ABSTRACTERROR
    +	.long	FPC_ABSTRACTERROR
    +	.long	FPC_ABSTRACTERROR
    +	.long	FPC_ABSTRACTERROR
    +	.long	FPC_ABSTRACTERROR
    +	.long	FPC_ABSTRACTERROR
     	.long	0
    
     .const_data
    

As you can see, all references to the virtual methods in the virtual method table of `tbase` have been removed, since the compiler knows they can never be referenced. This allows the linker to throw away these methods if they are not referenced anywhere else either. In our example, that is only true for `tbase.test`. 

Now let's look at the effect of the second recompilation: 
    
    
    $ diff -u twpo4.s2 twpo4.s
    
    
    
    --- twpo4.s2	2008-12-11 19:00:15.000000000 +0100
    +++ twpo4.s	2008-12-11 19:00:29.000000000 +0100
    @@ -103,9 +103,7 @@
     	movl	%eax,-4(%ebp)
     # [54] bb.test;
     	movl	-4(%ebp),%eax
    -	movl	-4(%ebp),%edx
    -	movl	(%edx),%edx
    -	call	*80(%edx)
    +	call	L_P$PROGRAM_TCHILD1_$__TEST$stub
     # [55] bb.free;
     	movl	-4(%ebp),%eax
     	call	L_SYSTEM_TOBJECT_$__FREE$stub
    @@ -139,10 +137,7 @@
     # [63] bb.test;
     	movl	L_U_P$PROGRAM_BB$non_lazy_ptr-Lj2(%ebx),%eax
     	movl	(%eax),%eax
    -	movl	L_U_P$PROGRAM_BB$non_lazy_ptr-Lj2(%ebx),%edx
    -	movl	(%edx),%edx
    -	movl	(%edx),%edx
    -	call	*80(%edx)
    +	call	L_P$PROGRAM_TCHILD1_$__TEST$stub
     # [64] a:=2;
     	movl	L_U_P$PROGRAM_A$non_lazy_ptr-Lj2(%ebx),%eax
     	movl	$2,(%eax)
    

As you can see, the two virtual method calls to _bb.test_ (one inside the never called _notcalled_ procedure, and one in the main program) have been replaced with calls to _tchild1.test_. 

Note that you could also recompile the entire run time library for this particular program, if you want. 

### Concrete Lazarus IDE example

To setup Whole Program Optimization in Lazarus IDE, add two new Build Modes for each platform. For example: 

  * "Linux WPO Pass1"
  * "Linux WPO Pass2"
  * "Windows WPO Pass1"
  * "Windows WPO Pass2"



[![01 Add New Build Modes.png](https://wiki.freepascal.org/images/9/9d/01_Add_New_Build_Modes.png)](</File:01_Add_New_Build_Modes.png>)

See: [IDE Window: Compiler Options#Build modes](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options"). 

Switch to Build mode "Linux WPO Pass1". Under "Compiler Options | Custom Options", add the Custom Options "-OWall -FWtemplinux.wpo -Xs- -CX" (case sensitive, should be UPPERCASE "W"). 

[![02 Linux WPO Pass1.png](https://wiki.freepascal.org/images/8/8f/02_Linux_WPO_Pass1.png)](</File:02_Linux_WPO_Pass1.png>)

Switch to Build mode "Linux WPO Pass2". Under "Compiler Options | Custom Options", add the Custom Options "-Owall -Fwtemplinux.wpo -Xs- -CX" (case sensitive, should be lowercase "w"). 

[![03 Linux WPO Pass2.png](https://wiki.freepascal.org/images/5/59/03_Linux_WPO_Pass2.png)](</File:03_Linux_WPO_Pass2.png>)

Switch to Build mode "Windows WPO Pass1". Under "Compiler Options | Custom Options", add the Custom Options "-OWDEVIRTCALLS,OPTVMTS -FWtempwindows.wpo -CX -XX -Xs-" (case sensitive, should be UPPERCASE "W"). 

[![04 Windows WPO Pass1.png](https://wiki.freepascal.org/images/0/07/04_Windows_WPO_Pass1.png)](</File:04_Windows_WPO_Pass1.png>)

Switch to Build mode "Windows WPO Pass2". Under "Compiler Options | Custom Options", add the Custom Options "-OwDEVIRTCALLS,OPTVMTS -Fwtempwindows.wpo -CX -XX -Xs-" (case sensitive, should be lowercase "w"). 

[![05 Windows WPO Pass2.png](https://wiki.freepascal.org/images/e/e4/05_Windows_WPO_Pass2.png)](</File:05_Windows_WPO_Pass2.png>)

When you are happy with your Project Options settings, you might want to click the Export button, to save them to an XML file, which you can import into other projects later. 

When you are ready to compile your program, from the IDE menu, select "Run | Compile Many Modes". Select both Pass1 and Pass2 to compile in exactly that proper order. 

[![06 Build Many Modes.png](https://wiki.freepascal.org/images/0/04/06_Build_Many_Modes.png)](</File:06_Build_Many_Modes.png>)

If your Build Modes are out of sequence, you could simply compile each Pass separately, in proper order. Alternatively, you could very carefully manually edit in a text editor your LPI project file (after first making a backup copy), to put the Passes into correct order. 

## When to use

Since whole program optimization requires multiple compilations, it is advisable to only use this functionality when compiling a final release version. 

Also keep in mind that once a unit has been compiled using _wpo_ for a particular program, it has to be recompiled if you want to use it in another program. 

## Available whole program optimizations

### All optimizations

parameter
    
    
    
    -OWall/-Owall
    

effect
    Enables all whole program optimizations described below.
limitations
    The combined limitations of all optimizations described below.

### Whole program devirtualization

parameter
    
    
    
    -OWdevirtcalls/-Owdevirtcalls
    

effect
    Changes virtual method calls into normal (static) method calls when the compiler can determine that a virtual method call will always go to the same static method. This makes such code both smaller and faster. In general, it's mainly an enabling optimization for other optimizations, because it makes the program easier to analyse due to the fact that it reduces indirect control flow.
limitations

  * The current implementation is context-insensitive. This means that the compiler only looks at the program as a whole and determines for each class type which methods can be devirtualised, rather than that it looks at each call statement and the surrounding code to determine whether or not this call can be devirtualised;
  * The current implementation does not yet devirtualise interface method calls (not when calling them via an interface instance, nor when calling them via a class instance).



  


### Optimise virtual method tables

parameter
    
    
    
    -OWoptvmts/-Owoptvmts
    

effect
    This optimization looks at which class types can be instantiated and which virtual methods can be called in a program, and based on this information it replaces virtual method table (VMT) entries that can never be called with references to FPC_ABSTRACTERROR. This means that such methods, unless they are called directly via an _inherited_ call from a child class/object, can be removed by the linker. It has little or no effect on speed, but can help reducing code size.
limitations

  * Methods that are _published_ , or getters/setters of published properties, can never be optimized in this way, because they can always be referred to and called via the RTTI (which the compiler cannot detect).
  * Such optimizations are not yet done for virtual class methods (but it should not be that difficult to add)



### Symbol liveness

parameter
    
    
    
    -OWsymbolliveness/-Owsymbolliveness
    

effect
    This parameter does not perform any optimization by itself. It simply tells the compiler to record which functions/procedures were kept by the linker in the final program. During a subsequent _wpo_ pass, the compiler can then ignore the removed functions/procedures as far as _wpo_ is concerned (e.g., if a particular class type is only constructed in one unused procedure, then ignoring this procedure can improve the effectiveness of the previous two optimizations).
limitations

  * This optimization requires that the `nm(1)` utility is installed on the system. For Linux binaries, `objdump(1)` will also work. In the future, this information could also be extracted from the internal linker for the platforms that it supports.
  * Collecting information for this optimization (using -OWsymbolliveness) requires that smart linking is enabled (-XX) and that symbol stripping is disabled (-Xs-). When only using such previously collected information (using -OwSymbolliveness or -Owall), these limitations do not apply.



## Format of the WPO feedback file

This information is mainly interesting if you want to add external data to the WPO feedback file, e.g. from a profiling tool. If you are just a user of the WPO functionality, you can ignore what follows. 

The file consists of comments and a number of sections. Comments are lines that start with a `#`. Each section starts with `%` (percent sign and space) followed by the name of the section (e.g.,`% contextinsensitive_devirtualization`). After that, until either the end of the file or until the next line starting with with "`% `", first a human readable description follows of the format of this section (in comments), and then the contents of the section itself. 

There are no rules for how the contents of a section should look, except that lines starting with `#` are reserved for comments and lines starting with `%` are reserved for section markers.

---

_Source: [https://wiki.freepascal.org/Whole_Program_Optimization](https://web.archive.org/web/20250219061636/https://wiki.freepascal.org/Whole_Program_Optimization)_
