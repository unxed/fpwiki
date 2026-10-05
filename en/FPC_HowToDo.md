# FPC HowToDo

This page provides blueprints of features, both mandatory and optional, which have been implemented/tested on at least one platform, but that still have to or could be added for other platforms. When adding a new entry, please add as much technical information as needed about the existing implementations for other platforms and the generic support routines which are available. Do not just add bug fix requests, that is what [Mantis](<http://bugs.freepascal.org>) is for. 

Note that description of [System unit structure](<System_unit_structure.md> "System unit structure") (especially the internal platform-specific implementation) provides another useful source of information for porting to new platforms (and aligning behaviour for the existing ports). In general, descriptions appearing here and related to System unit are expected to move to [System unit structure](<System_unit_structure.md> "System unit structure") sooner or later. 

## Contents

  * 1 Mandatory
    * 1.1 Widestring manager
    * 1.2 i386/arm fpu/sse initialisation
    * 1.3 Dynamic library initialization/finalization
    * 1.4 Weak linking
  * 2 Optional
    * 2.1 Compiler optimizations
      * 2.1.1 (u)int32x(u)int32 to (u)int64 multiplication
      * 2.1.2 Bit(field) getting/setting primitives



# Mandatory

## Widestring manager

  * **Required for** : proper functioning of locale-dependent widestring and ansistring operations (assignments, comparisons)
  * **Platforms implemented** : Windows, unix
  * **Platforms Todo** : OS/2, symbian, macos9, morphos, netware
  * **Generic usable routines** : rtl/inc/wstringh.inc, rtl/inc/wustring.inc
  * **What** : implement widestring manager helpers. Add them to your system unit if they can be linked into any binary, or into a unit called "cwstring" if they depend on an external library on which programs should not depend by default. If added to the system unit, also define _FPC_NOGENERICANSIROUTINES_ at the top of your platform's sysutils unit.
  * **Existing implementations** : win{32,64,ce}/system.pp (complete? -- search for _widestringmanager_), rtl/unix/cwstring.pp
  * **Tests** : tests/test/twide*.pp, tests/test/units/sysutils/tastrcmp.pp, and some in tests/tbs and tests/webtbs. Mostly not available and still need to be written.



## i386/arm fpu/sse initialisation

  * **Required for** : initialising global variables indicating the presence of certain fpu types, so that the fpu exception masking routines also adjust the exception masks of these fpus.
  * **Platforms implemented** : win32 (i386), Linux (i386 and arm), FreeBSD (i386), Darwin (i386)
  * **Platforms Todo** : wince (arm), OS/2 (i386), BeOS (i386)
  * **Generic usable routines** : rtl/i386/i386.inc (_fpc_cpucodeinit_), rtl/arm/arm.inc (_fpc_cpucodeinit_)
  * **What** : call _fpc_cpucodeinit_ in your system unit's initialisation code, and catch the potential "illegal instruction" exception in your exception/signal handler, checking/setting a global variable in the process indicating lacking support for the tested fpu type
  * **Existing implementations** : rtl/win32/system.pp (search for _os_supports_sse_ and _fpc_cpucodeinit_), rtl/linux/system.pp and rtl/linux/arm/sighnd.inc and rtl/linux/i386/sighnd.inc (idem), rtl/bsd/system.pp and rtl/freebsd/i386/sighnd.inc (idem)
  * **Tests** : tests/test/units/math/tmask*.pp compiled with -Cfsse2 (i386) or with -Cf{fpa,fpa10,fpa11} (arm)



## Dynamic library initialization/finalization

  * **Required for** : Working initialization/finalization sections in dynamic libraries
  * **Platforms implemented** : windows all, Darwin, Linux
  * **Platforms Todo** : All other platforms which support dynamic libraries
  * **Generic usable routines** : compiler/ncgutil.pas (_gen_proc_symbol_end_)
  * **What** : When _current_module.islibrary_ is true, 
    * call the procedure whose _procdef.potype_proginit=potype_proginit_ when the library is initialised
    * when _(current_module.flags and uf_finalize) <>0_ call the _current_procinfo.procdef_ when the library is finalised
  * **Existing implementations** : The darwin implementation can be found in compiler/rtl/ncgutil.pas (_gen_proc_symbol_end_).
  * **Tests** : tests/webtbs/tw7838*.pp, tests/webtbs/tw9089*.pp



## Weak linking

  * **Required for** : supporting weakly linking symbols (symbols which may not be available at run time, and which map to NIL in that case)
  * **Platforms implemented** : Darwin, Linux (with external assembler), Solaris
  * **Platforms Todo** : All other platforms that support dynamic libraries, and the internal assembler/linker
  * **Generic usable routines** : `svn diff -c 12009 compiler/aggas.pas` (all the rest are generic changes required for all platforms)
  * **What** : when an asmsymbol's "bind" field equals AB_WEAK_EXTERNAL, mark it as a "weak" symbol in the assembler or object file, and add the newly supported platforms to the `system_weak_linking` set in compiler/systems.pas
  * **Tests** : tests/test/tweaklib1.pp and tests/test/tweaklib2.pp (tweaklib3.pp and tweaklib4.pp are probably only necessary for Darwin, see the comments in tweaklib1.pp for more info)



# Optional

## Compiler optimizations

### (u)int32x(u)int32 to (u)int64 multiplication

  * **Required for** : optimizing 32x32->64 bit multiplications (several 32 bit architectures can do this without needing to perform a full 64 bit multiplication)
  * **Platforms implemented** : PowerPC 32 and i386
  * **Platforms Todo** : all other 32 bit architectures
  * **Generic routines** : compiler/nadd.pas (_use_generic_mul32to64_ /_first_add64bitint_ /_try_make_mul32to64_)
  * **What** : override _taddnode.use_generic_mul32to64_ and let it return false, then add support to your architecture's muln handler for performing signed and unsigned 32 bit int to signed/unsigned 64 bit int multiplications (i.e., left.resultdef and right.resultdef = 32 bit ints, self.resultdef = 64 bit int)
  * **Existing implementations** : compiler/powerpc/ncpuadd.pas (_second_add64bit_ , search for _MULHW_ /_MULHWU_)
  * **Tests** : tests/test/cg/tmul3264.pp
  * **Notes** : does not occur much in at least the rtl/compiler code base



### Bit(field) getting/setting primitives

  * **Required for** : being able to use the generic tcginnode code and still getting (close to) optimal code, faster record register variables and faster bitpacked records/arrays
  * **Platforms implemented** : PowerPC 32/64
  * **Platforms Todo** : all other architectures
  * **Generic routines** : compiler/cgobj.pas (_a*subsetreg_ , _a*subsetref_ , _a_bit_test*_)
  * **What** : override (some of) the subsetreg/subsetref routines to use architecture-specific opcodes for setting/getting bits and/or bitfields (the _a_bit_test*_ routines mostly use the subsetref/reg routines under the hood, except for _a_bit_test_reg_reg_reg_ , so in general it's better to implement special cases for the subsetreg/ref routines as these are used in more cases)
  * **Existing implementations** : compiler/ppcgen/cgppc.pas (_a_load_subsetref_regs_noindex_), compiler/powerpc/cgcpu.pas (_a_load_subsetreg_reg_ , _a_load_subsetreg_subsetreg_ , _a_load_regconst_subsetreg_intern_), compiler/powerpc64/cgcpu.pas (_a_load_subsetreg_reg_ , _a_load_const_subsetreg_ , _a_load_regconst_subsetreg_intern_)
  * **Tests** : tests/test/tparray*.pp, tests/test/tprec*.pp, tests/test/trecreg*.pp

---

_Source: [https://wiki.freepascal.org/FPC_HowToDo](https://web.archive.org/web/20220629212225/https://wiki.freepascal.org/FPC_HowToDo)_
