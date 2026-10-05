# Atari

This page is about the Atari TOS version of Free Pascal, which means [Motorola 680x0](<m68k.md> "m68k") CPU based Atari systems running TOS and compatibles. 

## Contents

  * 1 Status
  * 2 Identification
  * 3 AppType directive
  * 4 SysCalls
  * 5 Linker
  * 6 Assembler
    * 6.1 Building vasm for FPC
  * 7 Cross compiling
    * 7.1 Dependencies
    * 7.2 Building
  * 8 OS Bindings
    * 8.1 Naming differences
    * 8.2 Examples
  * 9 Missing features
    * 9.1 Truncate



## Status

The initial port was done by [Károly Balogh](</User:Chain-Q> "User:Chain-Q"), maintainer of the [m68k](<m68k.md> "m68k") and [Amiga](<Amiga.md> "Amiga") ports, based on some existing bits from FPC 0.x and 1.x times done by Carl Eric Codere and others. The port was further improved thanks to the extensive contributions by Thorsten Otto. The Atari TOS port lacks a maintainer currently. This means although we try to keep it working and do minor fixes to it, the port receives no new features. **Contact the[m68k](<m68k.md> "m68k") port maintainer, if you are interested in stepping up as an Atari maintainer.**

**The Atari port is in a low support tier, which means we don't really support "stable" compiler versions.** This document reflects the status in the "main" branch. 

The RTL support is reasonably advanced, and it's sufficient to run the compiler itself on Atari TOS, but - given the real hardware's constraints - it's not extensively tested. Most of the System, Sysutils and DOS units are implemented, some parts of it are not, or not very well tested though. The "tosunits" package is available for direct OS call bindings, and are fairly complete at this point. See section OS Bindings for details. The Atari TOS port doesn't support MiNT long file names at this point. 

## Identification

To identify Atari TOS during compile-time, use **{$IFDEF ATARI}**. 

## AppType directive

Atari is among the few platforms actually support the $APPTYPE directive, originating from Delphi and Windows. There are currently two states: 

  * **$APPTYPE CONSOLE** \- the resulting executable will have .TTP extension
  * **$APPTYPE GUI** \- the resulting executable will have .PRG extension



The default is CONSOLE, so compiled programs will have .TTP extension. The directive doesn't affect any other code generation or run-time behavior, except the resulting executable name. 

## SysCalls

Free Pascal supports generating Atari TOS-style system calls, also known as traps. It's not required to use inline assembly to do system calls. However, you must declare every function you're going to use in the following way: 
    
    
    function gemdos_fwrite(handle: smallint; count: longint; buf: pointer): longint; syscall 1 64;
    

Note the syscall modifier in the function declaration. The first argument to the syscall modifier is the trap number to call, and the second one is the trap opcode. All syscall parameters are passed on the stack and they're word (2 byte) aligned. For further examples, see _rtl/atari/gemdos.inc_ in the RTL source. 

## Linker

For Atari, some versions of **GNU ld** linker available online is known to be problematic when working together with Free Pascal. It's recommended to use **vlink** by Frank Wille, while compiling to Atari TOS with FPC. Specifying _-XV_ argument enables **vlink** for cross compilers. The native compiler defaults to **vlink** , to switch the linker back to **GNU ld** use _-XV-_ argument. 

**vlink** is open source, and it is available [here](<http://sun.hasenbraten.de/vlink>). Binaries are available as part of the [vbcc compiler package](<http://sun.hasenbraten.de/vbcc>). For advanced features like named sections and section garbage collection, use vlink 0.15d or newer. 

Due to TOS executable format limitations, code compiled for 68020 and newer CPUs might not work at this time, so use the option _-Cp68000_ to force the compiler to 68000 only mode. See the additional notes about 68000 constraints on the [m68k](<m68k.md> "m68k") page. 

## Assembler

The Atari TOS port supports both **GNU as** assembler and **vasm**. Advanced features like named sections support are only available with **vasm**. To compile with vasm, you need to specify the argument _-Avasm_ compile time. Please note that mixing **GNU as** and **vasm** compiled objects might cause issues during linking. 

**vasm** is open source, and it is available [here](<http://sun.hasenbraten.de/vasm>). Binaries are available as part of the [vbcc compiler package](<http://sun.hasenbraten.de/vbcc>). Only vasm versions 1.7h and newer were tested, older versions might not work. 

**WARNING:** **vasm** versions between 1.9a and 1.9c - these versions inclusive - contain a bug, which makes it impossible to use these versions with Free Pascal on any [m68k](<m68k.md> "m68k") platform. **Please stick to vasm versions 1.8 (any letter), 1.9 (no letter) or 1.9d, or newer.**

#### Building vasm for FPC

Note that **vasm** for m68k needs to be built with the _standard_ syntax module to work with FPC. This is not the default, as most coders and compilers (like vbcc itself) prefer the _Motorola_ syntax module. A **vasm** version with the _standard_ syntax module can be built with the following command, issued in the root of the **vasm** source tree: 
    
    
     make CPU=m68k SYNTAX=std
    

The resulting _vasmm68k_std_ executable file is the assembler is the one FPC needs. 

## Cross compiling

### Dependencies

Free Pascal requires cross-binutils for the target platform. Additionally, the TOS version also needs **vlink** and optionally **vasm**. For Atari TOS/MiNT cross-binutils for various platforms, also as binary downloads, [see this page by Vincent Rivière](<http://vincent.riviere.free.fr/soft/m68k-atari-mint/>). For vlink and vasm see the relevant sections above. 

### Building

To build an Atari cross-compiler, use the following steps: 

  1. Install the latest stable FPC version, at the time of the writing of this article this is FPC 3.2.2. This will be used as the startup compiler.
  2. Clone the FPC git repository.
  3. Make sure you have Atari TOS cross-binutils and vlink, and optionally vasm in the PATH.
  4. Go to the repository's directory, make sure you are on the **main** branch and use the following command to build an FPC cross-compiler:


    
    
      make clean crossall crossinstall OS_TARGET=atari CPU_TARGET=m68k CROSSOPT="-Cp68000" INSTALL_PREFIX="<path/to/install>"
    

Or, to build everything with vasm instead, use: 
    
    
      make clean crossall crossinstall OS_TARGET=atari CPU_TARGET=m68k CROSSOPT="-Avasm -Cp68000" INSTALL_PREFIX="<path/to/install>"
    

If you've done everything right, you should find a working Atari cross-compiler in the install path you've specified. 

Now, lets create a default fpc.cfg for Atari cross compiling. Create a file called **< path/to/install>/lib/fpc/etc/fpc.cfg**. Put the following lines into that file, and fix up the paths: 
    
    
    #IFDEF CPUM68K
    -Fu<path/to/install>/lib/fpc/$fpcversion/units/$FPCTARGET
    -Fu<path/to/install>/lib/fpc/$fpcversion/units/$FPCTARGET/*
    #IFDEF ATARI
    -FD</path/to/atari-cross-binutils>
    -Cp68000
    -XV
    #ENDIF
    #ENDIF
    

Optionally add **< path/to/install>/lib/fpc/3.3.1/** directory to the PATH, so you'll have direct access to the cross-compiler. If you've done everything right, you now should be able to build Atari executables: 
    
    
    ppcross68k -Tatari <source.pas>
    

## OS Bindings

The "tosunits" package contains the direct OS bindings, and it's fairly complete at this point. It provides **AES** , **VDI** , **GEMDOS** and **XBIOS** bindings. The bindings were developed using the [tos.hyp documentation](<http://toshyp.atari.org/en/index.html>), and various other bits of information available around the net. The still missing bindings should be very easy to add, using the existing ones as a template. Some work has been included to be compatible with Pure Pascal OS bindings, therefore support compiling of existing code. 

### Naming differences

The bindings try to follow the established function names for OS functions. However, there are some differences compared to the C language, to avoid confusion caused by collisions in the global namespace. The most important are: 

  * All **GEMDOS** functions start with the prefix _gemdos__. For example _fwrite()_ becomes _gemdos_fwrite()_.
  * All **XBIOS** functions start with the prefix _xbios__. For example _random()_ becomes _xbios_random()_.
  * As a de-facto convention with Pascal, all type names are prefixed with _T_ and all pointers to those types are prefixed with _P_. For example the **AES** _MFORM_ structure is named _TMFORM_ in Free Pascal, and a pointer to _TMFORM_ is defined as _PMFORM_. Additionally, the **AES** _OBJECT_ structure is named _TAESOBJECT_ to avoid conflict with the _TObject_ class type defined in the _System_ unit.



Note that most of these naming differences are not mandated, but simply there for practical reasons: to avoid confusion and cryptic error messages during compilation, in case a wrong type or function gets referenced with an overlapping name. If necessary to compile some older code, aliases with the regular naming can be added easily. 

### Examples

The "tosunits" package provides a few examples which show the bindings and **TOS/GEM** API used natively. Further contributions are welcomed. 

## Missing features

### Truncate

It's not possible to trivially implement a _truncate()_ procedure on the original Atari TOS. This function is not supported by FPC on TOS. It might be implemented for a future MiNT support though.

---

_Source: [https://wiki.freepascal.org/Atari](https://web.archive.org/web/20241207124546/https://wiki.freepascal.org/Atari)_
