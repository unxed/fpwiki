# Porting the Unix rtl to a new target

## Contents

  * 1 Porting FPC
    * 1.1 Setting up a new target in the compiler
    * 1.2 Porting the RTL
      * 1.2.1 startup code



# Porting FPC

Very global instructions to porting FPC to a *nix clone on a supported architecture: 

## Setting up a new target in the compiler

(typically only done when the RTL already works, and the first program runs, using a similar target) 

A few things must be done for a new target: (ARCH-OS combination, assuming ARCH is supported) 

  * Define enumerations (tsystem) for this target in systems.pas. Only append, changing the order breaks compability.
  * A record with some parameters for the ARCH-OS combination must be defined in compiler/systems/i_*. If you only ported the OS to a new architecture, you can add it to the existing i_OS file.
  * An object that handles calling the linker must be registered. These objects reside in compiler/systems/t_*. Reuse an existing one, or create a new one. Registering must be done in $ARCH/cputarg.pas file, by including the t_ unit. Also don't forget to review the initialisation code of the t_ unit if you adapt an existing one.
  * the startupcode of systems.pas must be adapted to select your target as default, under ifdef for the default OS-ARCH.



If you get stuck on some linking problem, try to link a simple C program with some verbosity options turned on. (either static or linking to libc, depending on what you are trying). 

A more detailed list of needed changes inside the compiler can be found at [Porting Free Pascal](<Porting_Free_Pascal.md> "Porting Free Pascal"). 

## Porting the RTL

Porting the Unix rtl is typically more work then the stuff above. 

Typically it boils down to take the closest relative (rtl/freebsd/, and start checking and converting) 

There are a few typical problem points: 

  * ftruncate, lseek, mmap and other calls that have 64-bits arguments and returnvalues. The exact handling and calling conventions of these changes per unix flavour and even per architecture.
  * The startup code must be adapted. Twice. One for linking without libc (prt0), one for with libc (cprt0). Others (like gprt0) are only for profiling and basically a slightly modded cprt0
  * signal to runtime error procedure must be reviewed. The way to get access to the registerstate is highly arch + OS dependant. The texception* tests in the testsuite are good tests for this.



### startup code

The startup code mods are typically done by compiling the crt0 sources with -S to generate assembler AND compare with a objdumped small static binary. (the latter will reveal stuff like ELF identifiers), and checking with a comparable FPC existing startup code. 

Typical modifications both prt0 and cprt0.as: 

  * reserve room for a few variables (FPC_*, operatingsystem*)
  * fixup copro status word (x86)
  * move argc,argv,envp to FPC namespace.
  * Empty framepointer before calling main (xorl %ebp,%ebp)
  * beware of ELF identifiers.



prt0 can strip the whole libc init/finalisation, cprt0 must preserve it.

---

_Source: [https://wiki.freepascal.org/Porting_the_Unix_rtl_to_a_new_target](https://web.archive.org/web/20210125165120/https://wiki.freepascal.org/Porting_the_Unix_rtl_to_a_new_target)_
