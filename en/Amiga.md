# Amiga

[![Amiga-checkmark.png](https://wiki.freepascal.org/images/f/f2/Amiga-checkmark.png)](</File:Amiga-checkmark.png>)

This article applies to [Amiga](</Category:AmigaOS> "Category:AmigaOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

This page is about the so called 'classic Amiga' version of Free Pascal, which means [Motorola 680x0](<m68k.md> "m68k") CPU based systems running AmigaOS 3.x and below. For [MorphOS](<MorphOS.md> "MorphOS"), [AROS](<AROS.md> "AROS") and [AmigaOS4](<AmigaOS.md> "AmigaOS") versions, see the relevant pages. 

_Please note, that the information detailed here is specific to version Free Pascal 2.x and above, and**does *not* apply** to the old 0.99.x and 1.0.x versions of Free Pascal for Amiga, still available on various Amiga download sites._

After a long break, [Motorola 68k](<m68k.md> "m68k") CPU series and classic Amiga is supported again in Free Pascal 3.0. Please note that to successfully use the compiler natively, a very fast 68k system is required, at least expanded with a Motorola 68060 processor and 128MB of memory or more and a Hard Disk controller with DMA support, or an emulator with a fast JIT-compiler like [WinUAE](<http://winuae.net>), or a next-gen Amiga-compatible system. Large parts of the classic Amiga support was developed under [MorphOS](<MorphOS.md> "MorphOS"). Cross compiling from other architectures and operating systems is supported. 

## Contents

  * 1 Requirements
  * 2 Identification
  * 3 Stack
  * 4 SysCalls
    * 4.1 Introduction to SysCalls
    * 4.2 Other hints about libraries and Syscalls
  * 5 Naming conventions
    * 5.1 Constants
    * 5.2 Structures
  * 6 Alignment
  * 7 Threading
  * 8 Hooks
  * 9 Things good to know
  * 10 Linker
  * 11 Assembler
    * 11.1 Building vasm for FPC
  * 12 Building
  * 13 See also
  * 14 More information



## Requirements

By default, Free Pascal and its runtime library requires AmigaOS 3.0 (v39+ or newer). If you want to target older systems, please see [Amiga Legacy Support](<Amiga_Legacy_Support.md> "Amiga Legacy Support"). 

## Identification

To identify classic Amiga exclusively during compile-time, use **{$IFDEF AMIGA68K}**. To identify classic Amiga or [AmigaOS4](<AmigaOS.md> "AmigaOS"), use **{$IFDEF AMIGA}**. To identify any Amiga-like system including [AROS](<AROS.md> "AROS") and [MorphOS](<MorphOS.md> "MorphOS"), use **{$IFDEF HASAMIGA}**. This is similar to **HASUNIX** which is defined across Unix systems. Please note that **HASAMIGA** define is only available in Free Pascal 3.0 and up, and **AMIGA68K** is only available in Free Pascal 3.0.2 and up. 

## Stack

Under Amiga, the default stack is set to 256 KiB. Stack size can be set directly by the programmer with a compiler switch (-Cs), or with the **{$MEMORY}** directive. The memory area for the stack is dynamically allocated on program startup, so if you set this value too high, in low memory situations your program may exit immediately on startup without any further notice. 

Please note that increasing stack size with the 'stack' utility **will *NOT* work** for your app, because the RTL startup code will replace the existing stack with the size specified by the directive, using the _exec.library/StackSwap_ function. 

## SysCalls

### Introduction to SysCalls

Free Pascal supports generating Amiga-style library calls. You don't need to use additional hand-written assembly to call any library function. However, you must declare every function you're going to use in the following way: 
    
    
     Var 
       my_LibBase: Pointer;
     
     function my_OldCall(param1: LongInt location 'd0',
                         param2: LongInt location 'd1'): LongInt; 
              SysCall my_LibBase 1234; { Amiga-style call }
    

Where my_LibBase is the library base returned by exec's OpenLibrary() call, and 1234 is the call offset. Please note that offset values in Free Pascal must be specified as **positive values** , and not negative as shown in the Amiga SDK. my_LibBase can be a typed pointer, a dword or a void pointer. It's recommended to use PLibrary or the library's own type if one exists or possible. 

### Other hints about libraries and Syscalls

**MOST IMPORTANT:** While creating your own interface units for libraries, you must open the libraries you're going to use explicitly **before** using any function from them. Don't forget to close all libraries before you exit. The interface units bundled with the compiler will auto open and close the respective libraries (unless otherwise noted). 

## Naming conventions

This section describes the differences between the official Amiga SDK, which is for C and assembly language, and Amiga-specific units in FPC RTL. 

### Constants

System constants are named exactly like in Amiga SDK. 

### Structures

System structures are named similar to those in the Amiga SDK, but follow the Pascal convention about type naming, so: 

  * structure names has a T before the name
  * each type has a pointer to the type with P before the name.



The following example should make things trivial to understand: 
    
    
     struct Task is equal to TTask
     struct Task * is equal to PTask
    

## Alignment

Record elements are aligned to WORD (2 bytes) under Amiga by default. Use **{$PACKRECORDS 4}** if you need DWORD aligned structures. For byte aligned records, you can use a packed record, however this is not recommended when targeting the plain 68000, which has [special alignment requirements](<m68k.md> "m68k"). 

## Threading

Threading is supported on Amiga since Free Pascal 3.0, using the [AThreads](<AThreads.md> "AThreads") unit. Read there for possible caveats and unsupported features. 

## Hooks

Hook functions passed to some Amiga API calls must be declared with the **cdecl** modifier to be future proof. This is because differences in calling convention between some versions of the Free Pascal Compiler and the operating system. More specifically, recent versions of Free Pascal Compiler use the **register** calling convention by default on [m68k](<m68k.md> "m68k"), while Hooks are expected to use **stdcall** or **cdecl**. 

If you still want to use a function with the **register** calling convention, use the _HookEntryPas_ helper function instead of _HookEntry_ in the AmigaLib unit. 

## Things good to know

Here are a few hints, which I think are good to remember, when using FPC on Amiga. 

  * A filesystem which supports file name length above 30 characters is highly recommended. Lack of it may cause unexpected results or compilation failures, especially in case of large projects like the compiler itself.
  * On program exit, System unit will close all files opened via its own functions, so it's recommended to use them in most cases.
  * Do not mix FPC and OS file functions on a single file. It may work in some cases, but that's purely a coincidence. It's bad programming and can stop working anytime without notice.
  * On program exit, the heap manager will free all memory allocated via its own functions. It uses a memory pool to avoid fragmentation. It also makes debugging easier when using heaptrc unit. Using OS memory functions directly is not recommended, unless you explicitly require memory areas which should stay in memory after your process has exited (like pointers passed to other processes).
  * Do not mix FPC and OS memory functions on a pointer. It won't work and will only cause crashes and/or memory leaks.



## Linker

Some versions of Free Pascal on Amiga, including latest SVN trunk versions default to **vlink** by Frank Wille as the default linker, when running natively on Amiga. The cross-compilers still default to **GNU ld**. **vlink** is open source, and it is available [here](<http://sun.hasenbraten.de/vlink>). Binaries are available as part of the [vbcc compiler package](<http://sun.hasenbraten.de/vbcc>). 

You can change the linker back to **GNU ld** by adding _-XV-_ argument when compiling. For cross compilers _-XV_ argument enables vlink. 

Please make sure you use **vlink** version 0.16 or newer with Free Pascal on Amiga. Earlier versions might have issues when used with Free Pascal or might not have features the compiler now requires. 

## Assembler

The Amiga port supports both **GNU as** assembler and **vasm**. Advanced features like named sections support are only available with **vasm**. To compile with vasm, you need to specify the argument _-Avasm_ compile time. Please note that mixing **GNU as** and **vasm** compiled objects might cause issues during linking. Please also note that linking **vasm** generated object files with **GNU ld** is not supported, as **vasm** outputs objects in ELF format, while **GNU LD** for Amiga only supports aout objects. This means using **vasm** with FPC, requires **vlink** as linker

**vasm** is open source, and it is available [here](<http://sun.hasenbraten.de/vasm>). Binaries are available as part of the [vbcc compiler package](<http://sun.hasenbraten.de/vbcc>). Only **vasm** versions 1.7h and newer were tested, older versions might not work. 

**WARNING:** **vasm** versions between 1.9a and 1.9c - these versions inclusive - contain a bug, which makes it impossible to use these versions with Free Pascal on any [m68k](<m68k.md> "m68k") platform. **Please stick to vasm versions 1.8 (any letter), 1.9 (no letter) or 1.9d, or newer.**

#### Building vasm for FPC

Note that **vasm** for m68k needs to be built with the _standard_ syntax module to work with FPC. This is not the default, as most coders and compilers (like vbcc itself) prefer the _Motorola_ syntax module. A **vasm** version with the _standard_ syntax module can be built with the following command, issued in the root of the **vasm** source tree: 
    
    
     make CPU=m68k SYNTAX=std
    

The resulting _vasmm68k_std_ executable file is the assembler is the one FPC needs. 

## Building

To build an Amiga cross-compiler, use the following steps: 

  1. Install the latest stable FPC version for your host platform, at the time of the writing of this article this is FPC 3.0.2. This will be used as the startup compiler.
  2. Check out FPC SVN trunk into a directory.
  3. Make sure you have Amiga cross-binutils and/or vasm and vlink in the PATH.
  4. Go to the trunk's main directory and use the following command to build an FPC cross-compiler:


    
    
      make clean crossall crossinstall OS_TARGET=amiga CPU_TARGET=m68k CROSSOPT="-Cp68020" INSTALL_PREFIX="<path/to/install>"
    

If you use vasm and vlink, the CROSSOPT option in the make line should additionally include the options **"-Avasm -XV"**. 

If your binutils has different prefix than **m68k-amiga** , you add the **BINUTILSPREFIX** parameter to the above command line, for example: **BINUTILSPREFIX=m68k-amigaos-**

If you've done everything right, you should find a working Amiga cross-compiler with all the prebuilt RTL and Packages units in the install path you've specified. 

Now, lets create a default **fpc.cfg** for Amiga cross compiling. Create a file called **< path/to/install>/lib/fpc/etc/fpc.cfg**. Put the following lines into that file, and fix up the paths: 
    
    
     #IFDEF CPUM68K
     -Fu<path/to/install>/lib/fpc/$fpcversion/units/$FPCTARGET
     -Fu<path/to/install>/lib/fpc/$fpcversion/units/$FPCTARGET/*
     #IFDEF AMIGA
     -FD</path/to/amiga-cross-binutils>
     #ENDIF
     #ENDIF
    

Optionally also add an **-XP** parameter, to specify the correct prefix of your cross-binutils, for example: **-XPm68k-amigaos-**

Optionally add **< path/to/install>/lib/fpc/3.3.1/** directory to the PATH, so you'll have direct access to the cross-compiler. If you've done everything right, you now should be able to build Amiga executables: 
    
    
     ppcross68k -Tamiga <source.pas>
    

## See also

  * [List of Amiga software written in Free Pascal](<List_of_Amiga_software_written_in_Free_Pascal.md> "List of Amiga software written in Free Pascal")
  * [Amiga TagList](<Amiga_TagList.md> "Amiga TagList") \- A proposal
  * [Amiga Legacy Support](<Amiga_Legacy_Support.md> "Amiga Legacy Support") \- special support for very old (1.2+/v33+) AmigaOS versions



## More information

More information regarding Free Pascal and Amiga can be read on the [Free Pascal 4 Amiga wiki](<http://fpcamigawiki.alb42.de>). This wiki also contains links to (unofficial) nightly downloads and other (additional) information for Amiga.

---

_Source: [https://wiki.freepascal.org/Amiga](https://web.archive.org/web/20241209234748/https://wiki.freepascal.org/Amiga)_
