# MorphOS

[![MorphOS-butterfly.png](https://wiki.freepascal.org/images/f/f0/MorphOS-butterfly.png)](</File:MorphOS-butterfly.png>)

This article applies to [MorphOS](</Category:AmigaOS> "Category:AmigaOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

This page contains platform specific notes about the MorphOS port of Free Pascal. MorphOS is a massively enhanced, independent AmigaOS-clone, featuring ABI and API compatibility with classic [Amiga](<Amiga.md> "Amiga"), running on various PowerPC platforms. For more information, [see its homepage](<http://www.morphos-team.net>). 

## Contents

  * 1 Identification
  * 2 Stack
  * 3 SysCalls
    * 3.1 Introduction to SysCalls
    * 3.2 What different kinds of SysCalls do?
      * 3.2.1 Legacy
      * 3.2.2 SysV
      * 3.2.3 BaseSysV
      * 3.2.4 SysVBase
      * 3.2.5 R12Base
    * 3.3 Other hints about libraries and Syscalls
  * 4 Naming conventions
    * 4.1 Constants
    * 4.2 Structures
    * 4.3 Function calls
      * 4.3.1 MUI
  * 5 Alignment
  * 6 Threading
  * 7 Things good to know
  * 8 Cross compiling
    * 8.1 Cross compiling from Linux
    * 8.2 Cross compiling from OSX
    * 8.3 Cross compiling from Windows
  * 9 Bootstrapping a compiler from Linux
  * 10 Linker
  * 11 Assembler
  * 12 More information
  * 13 Thanks



## Identification

To identify MorphOS exclusively during compile-time, use **{$IFDEF MORPHOS}**. To identify any Amiga-like systems, use **{$IFDEF HASAMIGA}**. This is similar to **HASUNIX** which is defined across Unix systems. Please note that **HASAMIGA** define is only available in Free Pascal 3.0 and up, and the **AMIGA** define is *NOT* defined on MorphOS. 

## Stack

Under MorphOS, the default stack is set to 256 KiB. Stack size can be set directly by the programmer with a compiler switch (-Cs), or with the **{$MEMORY}** directive. The memory area for the stack is dynamically allocated on program startup, so if you set this value too high, in low memory situations your program may exit immediately on startup without any further notice. 

Note to classic Amiga programmers: Increasing stack size with the 'stack' utility provided with MorphOS **will not work** because it sets the size of the 68k stack for applications running in 68k emulation. 

## SysCalls

### Introduction to SysCalls

Free Pascal supports ABox style legacy, and four new calling conventions based on SysV ABI on MorphOS. Before that, only ABox style calls were supported. You don't need to use additional hand-written assembly to call any library function. However, you must declare every function you're going to use in the following way: 
    
    
     Var 
       my_LibBase: Pointer;
     
     function my_OldCall(param1: LongInt location 'd0',
                         param2: LongInt location 'd1'): LongInt; 
              SysCall Legacy my_LibBase 1234; { ABox-style call }
     
     function my_SysVCall(param1: LongInt;
                          param2: LongInt): LongInt;
              SysCall SysV my_LibBase 1240; { SysV ABI call }
     
     function my_BaseSysVCall(param1: LongInt;
                              param2: LongInt): LongInt;
              SysCall BaseSysV my_LibBase 1246; { Base,SysV ABI call }
     
     function my_SysVBaseCall(param1: LongInt;
                              param2: LongInt): LongInt;
              SysCall SysVBase my_LibBase 1252; { SysV,Base ABI call }
     
     function my_R12BaseCall(param1: LongInt;
                             param2: LongInt): LongInt;
              SysCall R12Base my_LibBase 1258; { SysV,r12Base ABI call }
    

Where my_LibBase is the library base returned by exec's OpenLibrary() call, and 1234 is the call offset. Please note that offset values in Free Pascal must be specified as **positive values** , and not negative as shown in MorphOS SDK. Since compiler version 2004/08/22, my_LibBase can be a typed pointer, beside a dword and a void pointer. It's recommended to use PLibrary or the library's own type if one exists or possible. 

In case of ABox style calls, the value after location can be any 68k register from d0-d7/a0-a5. Register a6 is used internally to pass my_LibBase in it. Register a7/sp cannot be used. Register names are not case sensitive. In ABox-style SysCalls you must specify a location for every argument explicitly. 

If you don't want to specify SysCall method explicitly for every call, you can set a default SysCall method, using the {$SYSCALL} directive. {$SYSCALL LEGACY} (which is the built-in default) sets ABox-style, {$SYSCALL SYSV} sets SysV style calls as default. Other possible arguments for {$SYSCALL} directive are SYSVBASE, BASESYSV and R12BASE. Please note that the calling convention rules are not strict inside a library, so you might found different kinds of it inside a single library. 

### What different kinds of SysCalls do?

Old libraries like exec and dos mostly using Legacy and SysV, newer PowerPC-native libs using SysV and BaseSysV, most current libs are using SysV and R12Base calls. SysVBase calls are rare, but do exist, so check for them when you create your own units for external libraries. If you want to figure out which calling convention a library uses, you can search its SDK for some fd/*.fd files, where this information is stored. 

#### Legacy

Legacy calling convention puts arguments into the 68k emulation structure, which is pointed by register r2. 0(r2) equals to 68k register d0, 4(r2) to d1, and so on. It puts libbase into 56(r2) which equals to 68k register a6, then puts call offset into r3, and jumps to address stored at $64(r2), which is the emulation entry point. On return it returns arguments in register r3. This calling convention is used to provide compatibility with existing 68k and classic Amiga PowerPC (WarpOS and PowerUP) apps. 

#### SysV

SysV calling convention stores arguments according to SysV.4 ABI, then jumps to address stored at (LibBase - Offset). This calling convention is often used where no LibBase is needed. 

#### BaseSysV

BaseSysV is a variation of SysV. It stores LibBase as first argument according to SysV.4 ABI, then puts all other arguments, then jumps to address stored at (LibBase - Offset). This calling convention also can be considered as legacy, R12Base convention replaced it, but still many libraries using it. 

#### SysVBase

SysVBase is a variation of SysV. It stores all arguments according to SysV.4 ABI, then stores LibBase as last argument, then jumps to address stored at (LibBase - Offset). This calling convention also can be considered as legacy, as R12Base convention replaced it. This one is rare, not many libraries using it. 

#### R12Base

R12Base is a variation of SysV. It stores all arguments according to SysV.4 ABI, then stores LibBase into register r12, then jumps to address stored at (LibBase - Offset). This calling convention superseeded BaseSysV and SysVBase, and should be used in new libraries. 

### Other hints about libraries and Syscalls

**MOST IMPORTANT:** You must open the libraries you're going to use explicitly **before** using any function from them, because the compiler will not do it for you. Don't forget to close all libraries before you exit. 

Before declaring a SysCall function, please check the units provided with FPC, the function you want to use might already be declared in one of the units, ready to be used. 

## Naming conventions

This section describes the differences between the official MorphOS SDK, which is for C language, and MorphOS-specific units in FPC RTL. 

### Constants

System constants are named exactly like in MorphOS SDK. 

### Structures

System structures are named similar like in MorphOS SDK, but follows the Pascal convention about type naming, so structure names has a T before, and each type has a pointer to type with P before the name. The following example should make things trivial: 
    
    
     struct Task equals to TTask
     struct Task * equals to PTask
    

### Function calls

System calls are named like in MorphOS SDK, but there are a few exceptions. Unfortunately MorphOS legacy (Amiga compatible) API has some name conflicts with Free Pascal RTL functions, and this issue cannot be solved with function overloading. These calls are available with a name prefixed by their library's name. So for example, exec.library's FreeMem() is available under the name execFreeMem(), and FreeMem() means the FreeMem() call of FPC's own heap manager. The following functions are known to be affected: 

  * exec.library 
    * AllocMem() (only causes trouble inside System unit),
    * FreeMem()
    * Insert()
  * dos.library 
    * Close()
    * Read()
    * Write()
    * Input()
    * Output()
    * Seek()
    * Rename()
    * Exit()
    * Flush()
    * System()
    * DeleteFile() (conflicts with SysUtils)
    * CreateDir() (conflicts with SysUtils)
  * graphics.library 
    * Move() (conflicts with Video)



Be prepared for more... 

#### MUI

In the following section there are a few hints about using MUI with Free Pascal. 

  1. set() and get() MUI macros available under name MUIset() and MUIget(), to avoid name conflicts with Pascal language. These are wrappers to the appropriate Intuition functions just like in C.
  2. To avoid unnecessary typecasting, MUI_TRUE and MUI_FALSE declared instead of plain True and False. Using these is recommended.
  3. There are various helper functions in MUIHelper unit, to make Pascal code using MUI much simpler and more readable. Use them.
  4. Pascal preprocessor won't preparse strings like C does. So text formatting sequences widely used in MUI applications like \033c and so won't work. You have to use the defined constants instead. Example:



C version: 
    
    
    "\033cCentered \033bBold Text"
    

Pascal version: 
    
    
    MUIX_C+'Centered '+MUIX_B+'Bold Text'
    

or 
    
    
    #27'cCentered '#27'bBold Text'
    

  * Note: these constants are there in official mui.h includes as well, they're just rarely used.



## Alignment

Record elements are aligned to DWORD (4 bytes) under MorphOS by default. Use {$PACKRECORDS 2} if you need word aligned structures. For byte aligned records, use a packed record. 

## Threading

Threading is supported on MorphOS since Free Pascal 3.0, using the [AThreads](<AThreads.md> "AThreads") unit. Read there for possible caveats and unsupported features. 

## Things good to know

Here are a few hints, which I think are good to remember, when using FPC on MorphOS. 

  * On program exit, System unit will close all files opened via its own functions, so it's recommended to use them in most cases.
  * Do not mix FPC and OS file functions on a single file. It may work in some cases, but that's purely a coincidence. It's bad programming and can stop working anytime without notice.
  * On program exit, the heap manager will free all memory allocated via its own functions. It uses a memory pool to avoid fragmentation. It also makes debugging easier when using heaptrc unit. Using OS memory functions directly is not recommended.


  * Scribble, which is a programmer's editor based on [Scintilla](<http://www.scintilla.org>) is included in MorphOS SDK 2.x and part of MorphOS 3.x itself, and **supports Pascal syntax highlighting out of box**.
  * Another option for syntax highlighted editing is GoldEd, [also known as Cubic IDE](<http://www.softwareandcircuits.com/division/amiga/products/cubic/index.html>) (or the lite version MorphEd, which was available as part of older MorphOS SDKs). A very nice Pascal syntax highlight addon was made for it by Rafal Zabdyr. The original site is down, but a backup is available here: <http://obligement.free.fr/files/ged5morphed_pas075.lha>


  * You can find some MorphOS specific examples in the SVN, directly accessible here: <http://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/demo/morphos/?root=fpcbuild>



## Cross compiling

Cross compiling to MorphOS is supported since version 3.0.0 and up. 

### Cross compiling from Linux

It's possible to cross-build the MorphOS compiler from Linux/i386 and Linux/x86-64 (probably other CPUs too, but it was not tested). To do that, you need Linux to MorphOS cross-binutils, mainly GNU as, ld, and optionally ar and strip. It's best if you build these from GNU sources, along with the rest of cross-build GNU toolchain. Mark "bigfoot" Olsen kindly provided a script for this, [available here](<http://bigfoot.morphos-team.net/files/setup-cross-sdk.sh>). 

Optionally, the binutils required by FPC are [available here as binary](<http://charlie.amigaspirit.hu/software/fpc/morphos/cross-binutils-linux-i386.tar.gz>) for Linux/i386 (works on Linux/x86-64 too, if you install the required 32 bit environment). It was only tested on Ubuntu and Debian Linuxes, but may work on other distributions too. **This package is provided "as-is" without any kind of support. Use it at your own risk.**

To build a MorphOS cross-compiler, use the following steps: 

  1. Install FPC the latest stable compiler, at the time of the writing of this article this is FPC 3.0.0. This will be used as the startup compiler.
  2. Check out FPC SVN trunk into a directory (see version info above)
  3. Make sure the aforementioned cross-binutils are actually in the PATH.
  4. Go to the trunk's main directory and use the following command to build an FPC to MorphOS cross-compiler running on Linux:


    
    
      make clean crossall crossinstall OS_TARGET=morphos CPU_TARGET=powerpc INSTALL_PREFIX="<path/to/install>"
    

If you've done everything right, you should find a working MorphOS cross-compiler in the install path you've specified. 

Now, lets create a default fpc.cfg for MorphOS cross compiling. Create a file called **< path/to/install>/lib/fpc/etc/fpc.cfg**. Put the following lines into that file, and fix up the paths: 
    
    
    #IFDEF CPUPOWERPC
    -Fu<path/to/install>/lib/fpc/$fpcversion/units/$FPCTARGET
    -Fu<path/to/install>/lib/fpc/$fpcversion/units/$FPCTARGET/*
    #IFDEF MORPHOS
    -FD</path/to/morphos-cross-binutils>
    -XPpowerpc-morphos-
    #ENDIF
    #ENDIF
    

Optionally add **< path/to/install>/lib/fpc/3.1.1/** directory to the PATH, so you'll have direct access to the cross compiler. If you've done everything right, you now should be able to build MorphOS executables from Pascal source on Linux the following way: 
    
    
    ppcrossppc -Tmorphos <source.pas>
    

Copy the resulting executable to MorphOS, and that's it! 

### Cross compiling from OSX

In general, the same rules and steps apply as for Linux cross compiling. To build the binutils from sources using Mark Olsen's script (see in the previous section) some slight modifications to the script and/or additional GNU tools might be required. 

For convenience, a binutils package required by FPC are [available here as binary](<http://charlie.amigaspirit.hu/software/fpc/morphos/cross-binutils-darwin-i386.tar.gz>) for Darwin/i386. **This package is provided "as-is" without any kind of support. Use it at your own risk.**

### Cross compiling from Windows

In general, the same rules and steps apply as for Linux cross compiling. It's also possible to cross-build the binutils with [Cygwin](<http://cygwin.com>) using Mark Olsen's script, as linked in the previous section. 

For convenience, a binutils package required by FPC are [available here as binary](<http://charlie.amigaspirit.hu/software/fpc/morphos/cross-binutils-cygwin-i686.zip>) for Windows/x86 (Cygwin/i686). **This package is provided "as-is" without any kind of support. Use it at your own risk.**

As FPC on Windows use a bit different directory structure the place to put our fpc.cfg is: **< path/to/install>/bin/i386-win32/fpc.cfg**. Also, for easy access, make sure directory **< path/to/install>/bin/i386-win32/** is part of the PATH. 

The cross-build from Windows/x64 was not tested, but should work with the obvious differences in paths and tools. 

## Bootstrapping a compiler from Linux

To bootstrap a Free Pascal Compiler from Linux to MorphOS, first please complete the previous section. In order to have all the tools available, it's easiest to install the latest FPC compiler archive [available here.](<http://charlie.amigaspirit.hu/software/fpc/morphos/>)

After you have a working cross compiler, go to the SVN **trunk/compiler** directory, and issue the following command: 
    
    
    make compiler OS_TARGET=morphos CPU_TARGET=powerpc FPC="<path/to/install>/lib/fpc/2.7.1/ppcrossppc"
    

Copy the resulting **ppcppc** executable to MorphOS. On the MorphOS box, try to cycle a compiler the following way in the SVN's **trunk/compiler** directory: 
    
    
    make cycle FPC=/path/to/ppcppc
    

  
Congratulations, you've just successfully bootstrapped Free Pascal on MorphOS! Now, copy everything in place by hand to your FPC: assign, build more stuff, etc... :) 

## Linker

Some version of Free Pascal on MorphOS, including latest SVN trunk versions default to **vlink** by Frank Wille as the default linker, when running natively on MorphOS. The cross-compilers still default to **GNU ld**. **vlink** is open source, and it is available [here](<http://sun.hasenbraten.de/vlink>). Binaries are available as part of the [vbcc compiler package](<http://sun.hasenbraten.de/vbcc>). 

You can change the linker back to **GNU ld** by adding _-XV-_ argument when compiling. For cross compilers _-XV_ argument enables vlink. 

## Assembler

The MorphOS port supports both **GNU as** assembler and **vasm**. Advanced features like named sections support are only available with **vasm**. To compile with vasm, you need to specify the argument _-Avasm_ compile time. Please note that mixing **GNU as** and **vasm** compiled objects might cause issues during linking. 

**vasm** is open source, and it is available [here](<http://sun.hasenbraten.de/vasm>). Binaries are available as part of the [vbcc compiler package](<http://sun.hasenbraten.de/vbcc>). Only vasm versions 1.7h and newer were tested, older versions might not work. 

If _-Avasm_ is set as default, for example in the config file, one can switch back to **GNU as** by using the _-Aas-legacy_ command line argument. Please note that since the MorphOS SDK has a very old **GNU as** bundled, there could be problems with some of the assembly code generated by using the normal **GNU as** argument _-Aas_. 

## More information

More information regarding Free Pascal and MorphOS can be read on the [Free Pascal 4 Amiga wiki](<http://fpcamigawiki.alb42.de>). This wiki also contains links to (unofficial) nightly downloads and other (additional) information for MorphOS. 

## Thanks

The initial [MorphOS](<http://www.morphos-team.net>) port of Free Pascal Compiler was possible, because [Genesi](<http://genesi.pegasosppc.com>) provided a free [Pegasos II/G4](<http://en.wikipedia.org/wiki/Pegasos>) machine to do the development on.

---

_Source: [https://wiki.freepascal.org/MorphOS](https://web.archive.org/web/20250324155300/https://wiki.freepascal.org/MorphOS)_
