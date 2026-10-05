# DOS

│ **[Deutsch (de)](</DOS/de> "DOS/de")** │  **English (en)** │ 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This page has outdated information. Somebody is updating the information but you can help.

## Contents

  * 1 Overview
    * 1.1 Advantages over FPC for GO32V2
    * 1.2 Advantages over Turbo Pascal
    * 1.3 Limitations
  * 2 Requirements
    * 2.1 FPC 3.0.x
    * 2.2 FPC trunk
  * 3 Releases
    * 3.1 3.0.4
    * 3.2 3.0.2
    * 3.3 3.0.0
    * 3.4 3.0.0rc2
    * 3.5 3.0.0rc1
  * 4 Snapshots
    * 4.1 Building a snapshot manually
    * 4.2 Updating your fpc.cfg
  * 5 Status
    * 5.1 Internal linker
    * 5.2 Floating point support
    * 5.3 Nil pointer assignment checking
    * 5.4 New pointer types
      * 5.4.1 Near pointers
      * 5.4.2 Special versions of near pointers
      * 5.4.3 Far pointers
      * 5.4.4 Huge pointers
        * 5.4.4.1 Huge pointer normalization
          * 5.4.4.1.1 $HugePointerArithmeticNormalization
          * 5.4.4.1.2 $HugePointerComparisonNormalization
          * 5.4.4.1.3 $HugePointerNormalization
    * 5.5 Tested machines
    * 5.6 Supported memory models
      * 5.6.1 Tiny
      * 5.6.2 Small
      * 5.6.3 Medium
      * 5.6.4 Compact
      * 5.6.5 Large
      * 5.6.6 Huge
    * 5.7 Units and programs exceeding 64KB of code
    * 5.8 Supported calling conventions
      * 5.8.1 Pascal
  * 6 Troubleshooting
    * 6.1 Compile error: missing .o files
  * 7 Running the testsuite
    * 7.1 Using DOSBox
  * 8 See also



## Overview

A DOS (8086) cross compiler is currently being developed in FPC trunk (the development version). It started as a hobby project meant to explore how to port FPC to a new platform. 

The number of (FPC on) DOS users is probably quite low, especially since there is an FPC compiler for the more capable [GO32V2](<GO32V2.md> "GO32V2") DOS extender available which runs on 80386+ processors. 

### Advantages over FPC for GO32V2

  * Generated code can run on 16-bit processors. Since these processors are obsolete and out of production, this is mostly useful for [retrocomputing](<http://en.wikipedia.org/wiki/Retrocomputing>) purposes. Note also that the 80186 is still in use in embedded devices: [[1]](<http://www.cpushack.com/2013/01/12/the-intel-80186-gets-turbocharged-vautomation-turbo186/>)
  * It's possible to write [TSRs](<http://en.wikipedia.org/wiki/Terminate_and_Stay_Resident>).
  * It's possible to write DOS .SYS device drivers. (Not ready yet, but should be easy to implement)
  * It allows writing a bootloader in Pascal.
  * It allows writing 16-bit BIOS code in Pascal (e.g. you can now do a project like [SeaBIOS](<http://www.seabios.org>) but in Pascal).
  * It can compile Turbo Pascal 7 code with less changes needed, compared to GO32V2 (no need to port the inline assembly code, for example).
  * Real mode DOS programs are less sensitive to bugs in virtual DOS environments, since they're immune to DPMI bugs and in fact require no DPMI at all. (Provided that your program fits within the memory constraints, of course.)
  * Compiled binaries are smaller and you don't have to ship CWSDPMI.EXE in order for your program to work under real DOS.



### Advantages over Turbo Pascal

  * Compiler is Free/Open Source software and is in active development.
  * Supports long file names.
  * Not affected (just like FPC for GO32v2) by the infamous CRT unit bug, that causes runtime error 200 on startup when the program is run on a fast CPU.
  * Multiple memory models are supported.
  * Supports huge pointers.
  * Heap manager supports allocating blocks larger than 64 KB (only in the memory models with far data, such as compact, large or huge).
  * All the extra Object Pascal features supported by FPC should work. This includes ansistrings, classes, interfaces, exceptions, generics, etc.
  * Crosscompilation is doable without running the development system in a dosbox/VM (compile/build on Windows 64-bit, only need 16-bit capable OS for running)



### Limitations

The DOS platforms brings some limitations, like 

  * data structures cannot be larger than 64KB
  * no simple way of pre-emptive multitasking.
  * it is unlikely the Lazarus LCL GUI will be ported to the DOS environment 
    * However, an [OpenGEM](<http://en.wikipedia.org/wiki/OpenGEM>) widget set is possible
    * Since the large memory model of the i8086 code generator have matured, [Win16](<Win16.md> "Win16") support has also been started in FPC trunk



## Requirements

### FPC 3.0.x

The compiler is a cross compiler that runs at least on Windows (both x86 and x64), Linux, and macOS. For compiling programs, it needs: 

  * the Open Watcom linker - WLINK
  * the Open Watcom library manager - WLIB
  * the Netwide Assembler - [NASM](<http://www.nasm.us/>)



In theory it should be able to run on any platform, supported by FPC, where NASM and the Open Watcom tools are available. This includes DOS via the GO32V2 extender. However, because the Watcom tools for DOS are compiled with a different extender, there are some issues related to long file names and the passing of long command line arguments. This is resolved in FPC trunk, where there's an internal assembler and linker implemented in the compiler itself. 

### FPC trunk

FPC trunk contains an internal assembler and linker for DOS, so the Open Watcom tools are no longer required. NASM is still required for building an i8086-msdos snapshot, because of the msdos startup code in the rtl. 

## Releases

### 3.0.4

The crosscompiler packages for Windows, Linux - i386 and Linux - x86_64 are also available at the official Free Pascal website: <http://freepascal.org/download.var>

  * Crosscompiler package for Windows: 
    * <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.4/i386-win32/fpc-3.0.4.i386-win32.cross.i8086-msdos.exe>
  * Crosscompiler package for Linux - i386 
    * .tar.xz: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.4/i386-linux/fpc-3.0.4.i386-linux.cross.i8086-msdos.tar.xz>
    * RPM: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.4/i386-linux/fpc-i8086-msdos-3.0.4-1.i686.rpm>
    * SRPM: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.4/i386-linux/fpc-i8086-msdos-3.0.4-1.src.rpm>
  * Crosscompiler package for Linux - x86_64 
    * .tar.xz: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.4/x86_64-linux/fpc-3.0.4.x86_64-linux.cross.i8086-msdos.tar.xz>
    * RPM: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.4/x86_64-linux/fpc-i8086-msdos-3.0.4-1.x86_64.rpm>
    * SRPM: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.4/x86_64-linux/fpc-i8086-msdos-3.0.4-1.src.rpm>



### 3.0.2

  * Crosscompiler package for Windows: 
    * <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.2/i386-win32/fpc-3.0.2.i386-win32.cross.i8086-msdos.exe>
  * Crosscompiler package for Linux - i386 
    * .tar.xz: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.2/i386-linux/fpc-3.0.2.i386-linux.cross.i8086-msdos.tar.xz>
    * RPM: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.2/i386-linux/fpc-i8086-msdos-3.0.2-1.i686.rpm>
    * SRPM: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.2/i386-linux/fpc-i8086-msdos-3.0.2-1.src.rpm>
  * Crosscompiler package for Linux - x86_64 
    * .tar.xz: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.2/x86_64-linux/fpc-3.0.2.x86_64-linux.cross.i8086-msdos.tar.xz>
    * RPM: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.2/x86_64-linux/fpc-i8086-msdos-3.0.2-1.x86_64.rpm>
    * SRPM: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.2/x86_64-linux/fpc-i8086-msdos-3.0.2-1.src.rpm>



### 3.0.0

  * Crosscompiler package for Windows: 
    * <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.0/i8086-msdos/fpc-3.0.0.i386-win32.cross.i8086-msdos.exe>
  * Crosscompiler package for Linux - i386 
    * .tar.xz: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.0/i386-linux/fpc-3.0.0.i386-linux.cross.i8086-msdos.tar.xz>
    * RPM: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.0/i386-linux/rpm/fpc-i8086-msdos-3.0.0-1.i686.rpm>
    * SRPM: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.0/i386-linux/rpm/fpc-i8086-msdos-3.0.0-1.src.rpm>
  * Crosscompiler package for Linux - x86_64 
    * .tar.xz: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.0/x86_64-linux/fpc-3.0.0.x86_64-linux.cross.i8086-msdos.tar.xz>
    * RPM: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.0/x86_64-linux/rpm/fpc-i8086-msdos-3.0.0-1.x86_64.rpm>
    * SRPM: <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.0/x86_64-linux/rpm/fpc-i8086-msdos-3.0.0-1.src.rpm>



### 3.0.0rc2

  * Crosscompiler package for Windows: 
    * <ftp://ftp.freepascal.org/pub/fpc/beta/3.0.0-rc2/i386-win32/fpc-3.0.0rc2.i386-win32.cross.i8086-msdos.exe>
  * Crosscompiler package for Linux - i386 
    * .tar.xz: <ftp://ftp.freepascal.org/pub/fpc/beta/3.0.0-rc2/i386-linux/fpc-3.0.0rc2.i386-linux.cross.i8086-msdos.tar.xz>
    * RPM: <ftp://ftp.freepascal.org/pub/fpc/beta/3.0.0-rc2/i386-linux/rpm/fpc-i8086-msdos-3.0.0-0.rc2.i686.rpm>
  * Crosscompiler package for Linux - x86_64 
    * .tar.xz: <ftp://ftp.freepascal.org/pub/fpc/beta/3.0.0-rc2/x86_64-linux/fpc-3.0.0rc2.x86_64-linux.cross.i8086-msdos.tar.xz>
    * RPM: <ftp://ftp.freepascal.org/pub/fpc/beta/3.0.0-rc2/x86_64-linux/rpm/fpc-i8086-msdos-3.0.0-0.rc2.x86_64.rpm>



### 3.0.0rc1

  * Crosscompiler package for Windows: 
    * <ftp://ftp.freepascal.org/pub/fpc/beta/3.0.0-rc1/i386-win32/fpc-3.0.0rc1.i8086-msdos.exe>
  * Crosscompiler package for Linux - i386 
    * .tar.xz: <ftp://ftp.freepascal.org/pub/fpc/beta/3.0.0-rc1/i386-linux/i8086-msdos-fpc-3.0.0rc1.i386-linux.tar.xz>
    * RPM: <ftp://ftp.freepascal.org/pub/fpc/beta/3.0.0-rc1/i386-linux/rpm/fpc-i8086-msdos-3.0.0-0.rc1.i686.rpm>
  * Crosscompiler package for Linux - x86_64 
    * .tar.xz: <ftp://ftp.freepascal.org/pub/fpc/beta/3.0.0-rc1/x86_64-linux/i8086-msdos-fpc-3.0.0rc1.x86_64-linux.tar.xz>
    * RPM: <ftp://ftp.freepascal.org/pub/fpc/beta/3.0.0-rc1/x86_64-linux/rpm/fpc-i8086-msdos-3.0.0-0.rc1.x86_64.rpm>



## Snapshots

There are daily snapshots available here: 

<ftp://ftp.freepascal.org/pub/fpc/snapshot/trunk/i8086-msdos/>

There's also a snapshot available here, but it's quite outdated, so it's better to simply use the above snapshot or to build your own: 

<http://www.bttr-software.de/forum/forum_entry.php?id=12985>

### Building a snapshot manually

  * make sure **nasm** is in your path (the fixes-3.0 branch also requires **wlink** and **wlib**)
  * checkout fpc trunk:


    
    
    svn checkout <http://svn.freepascal.org/svn/fpc/trunk> fpc
    

  * enter the fpc directory and build the compiler with the following command (replace **/usr/bin/ppc386** with the full path to the stable (3.0.4) FPC compiler binary; replace **-WmSmall** with **-WmTiny** , **-WmMedium** , **-WmCompact** , **-WmLarge** or **-WmHuge** if you want to use another memory model):


    
    
    make clean all OS_TARGET=msdos CPU_TARGET=i8086 OPT="-CX -XXs" CROSSOPT=**-WmSmall** BINUTILSPREFIX= PP=**/usr/bin/ppc386**
    

  * install the snapshot (replace **linux** and **i386** with the OS and CPU you're using; replace **INSTALL_PREFIX** with the directory you want the snapshot installed):


    
    
    make crossinstall OS_SOURCE=**linux** CPU_SOURCE=**i386** OS_TARGET=msdos CPU_TARGET=i8086 PP=compiler/ppcross8086 CROSSOPT=**-WmSmall** \
        BINUTILSPREFIX= INSTALL_PREFIX=**/home/blablabla/fpc-i8086/snapshot/small** OPT="-CX -XXs"
    

### Updating your fpc.cfg

Add the following to your fpc.cfg to enable smartlinking and disable the default binutils prefix (so that you don't have to rename nasm, wlink and wlib to msdos-nasm, msdos-wlink and msdos-wlib): 
    
    
    #ifdef cpui8086
    -CX
    -XX
    -XP
    #endif
    

To enable building for all the memory models, build snapshots for the tiny, small, medium, compact, large and huge memory model, and then find the lines in your fpc.cfg that specify the path to the RTL: 
    
    
    -Fu/usr/lib/fpc/$fpcversion/units/$fpctarget
    -Fu/usr/lib/fpc/$fpcversion/units/$fpctarget/*
    -Fu/usr/lib/fpc/$fpcversion/units/$fpctarget/rtl
    

And update them to something similar to: 
    
    
    #IFDEF CPUI8086
    -Fu/home/blablabla/fpc-i8086/snapshot/$fpcmemorymodel/lib/fpc/$fpcversion/units/$fpctarget
    -Fu/home/blablabla/fpc-i8086/snapshot/$fpcmemorymodel/lib/fpc/$fpcversion/units/$fpctarget/*
    -Fu/home/blablabla/fpc-i8086/snapshot/$fpcmemorymodel/lib/fpc/$fpcversion/units/$fpctarget/rtl
    #ELSE
    -Fu/usr/lib/fpc/$fpcversion/units/$fpctarget
    -Fu/usr/lib/fpc/$fpcversion/units/$fpctarget/*
    -Fu/usr/lib/fpc/$fpcversion/units/$fpctarget/rtl
    #ENDIF
    

This should enable you to build programs in any of the supported memory models with just a compiler switch: 
    
    
    ppcross8086 -WmTiny -Wtcom hello.pas
    ppcross8086 -WmTiny -Wtexe hello.pas
    ppcross8086 -WmSmall hello.pas
    ppcross8086 -WmMedium hello.pas
    ppcross8086 -WmCompact hello.pas
    ppcross8086 -WmLarge hello.pas
    ppcross8086 -WmHuge hello.pas
    

And in case you're wondering - it doesn't matter which ppcross8086 compiler binary you use (i.e. from the tiny, small, medium, compact, large or huge model snapshot). They are identical. Only the compiled units differ. 

## Status

The compiler has been enabled as a CPU/OS target in FPC.exe since revision 25792 (CPU: i8086, OS: msdos). 

The RTL compiles. Most units from Go32v2 have been ported. Just like Go32v2, the RTL supports long file names when run under a Windows 95+/2000+ DOS box or under plain DOS with a long file names driver such as doslfn.exe. FPC demo programs such as fpctris and samegame work in all supported memory models. 

### Internal linker

The internal linker does not support all records and features of the OMF object file format. It was designed to support only the records produced by the NASM assembler and FPC's own internal object writer. You may encounter problems if you try to link object modules produced by other compilers and assemblers. 

### Floating point support

Floating point operations require an FPU. Software FPU emulation is not yet implemented and using floating point operations on a real machine without an FPU will lead to a hang. 

### Nil pointer assignment checking

The **small** and **medium** memory models offer rudimentary nil pointer assignment checking. The system RTL puts a pattern of 32 bytes equal to $01 at the beginning of the data segment (DS:0000). At the end of program execution, this pattern is checked and if turns out it has changed, the message: 
    
    
    Nil pointer assignment
    

is printed on the screen. This indicates that a nil pointer assignment has happened at some time during the program execution. Unfortunately, we can't tell when exactly, only that it has happened. 

### New pointer types

To reflect the i8086 segmented memory model, FPC supports several pointer types: 

#### Near pointers

This is the default pointer type in the tiny, small and medium memory models. 

Near pointers can be declared in any memory model, using the _near_ keyword. For example: 
    
    
    type
      PNearInteger = ^Integer; near;
    

There's also an untyped near pointer, predefined in the system unit, called _NearPointer_. 

#### Special versions of near pointers

Near pointers are declared using the _near_ keyword, followed by a segment register, enclosed in single quotes. For example: 
    
    
    type
      PNearCSInteger = ^Integer; near 'CS';
      PNearDSInteger = ^Integer; near 'DS';
      PNearESInteger = ^Integer; near 'ES';
      PNearSSInteger = ^Integer; near 'SS';
      PNearFSInteger = ^Integer; near 'FS';
      PNearGSInteger = ^Integer; near 'GS';
    

  
There are also untyped versions, predefined in the system unit: 
    
    
     NearCSPointer
     NearDSPointer
     NearESPointer
     NearSSPointer
     NearFSPointer
     NearGSPointer
    

#### Far pointers

This is the default pointer type in the compact, large and huge memory models. 

Far pointers can be declared in any memory model, using the _far_ keyword. For example: 
    
    
    type
      PFarInteger = ^Integer; far;
    

There's also an untyped far pointer, predefined in the system unit, called _FarPointer_. 

#### Huge pointers

Huge pointers are like far pointers that support pointer arithmetic without the 64kb segment wraparound. That means they will automatically switch to a different segment if the offset exceeds 64k or becomes less than 0. Thus, they are very useful for accessing blocks of memory that exceed 64kb. Additionally, they support normalization, which is explained in the next section. Their main disadvantage is that they are quite a bit slower compared to far pointers. 

Huge pointers are declared using the _huge_ keyword. For example: 
    
    
    type
      PHugeInteger = ^Integer; huge;
    

There's also an untyped huge pointer, predefined in the system unit, called _HugePointer_. 

##### Huge pointer normalization

In real mode it is possible for several different pointers to point to the same memory address. For example, the following different pointers point to the same memory location: 
    
    
    Ptr($1000, $2345)
    Ptr($1200, $0345)
    Ptr($1234, $0005)
    

This is because they resolve to the same linear address via the formula (segment * 16 + offset): 
    
    
    Ptr($1000, $2345) // -> $1000 * 16 + $2345 = $12345
    Ptr($1200, $0345) // -> $1200 * 16 + $0345 = $12345
    Ptr($1234, $0005) // -> $1234 * 16 + $0005 = $12345
    

A Pointer whose offset is between $0 and $F is called a _normalized_ pointer. That's the pointer with the lowest possible offset among all the pointers that point to the same linear address. 

Huge pointers can be automatically normalized by the compiler when doing pointer arithmetic on them or when comparing them. This is controlled by the _$HugePointerNormalization_ , _$HugePointerArithmeticNormalization_ and _$HugePointerComparisonNormalization_ directives. All three of them are local switches, so you can change the modes in the middle of the program. 

###### $HugePointerArithmeticNormalization

This directive controls whether huge pointers are automatically normalized by the compiler after doing pointer arithmetic on them. 

###### $HugePointerComparisonNormalization

This directive controls whether huge pointers are normalized before comparing them. 

###### $HugePointerNormalization

This sets both $HugePointerArithmeticNormalization and $HugePointerComparisonNormalization: 

mode | arithmetic normalization | comparison normalization | description   
---|---|---|---  
{$HugePointerNormalization BorlandC} | On | On | Like Borland C   
{$HugePointerNormalization MicrosoftC} | Off | Off | Like Microsoft C   
{$HugePointerNormalization WatcomC} | Off | On | Like Watcom C   
  
### Tested machines

Compiled programs have been tested and known to work on the following machines: 

  * [IBM PC 5150](<http://oldcomputers.net/ibm5150.html>) (the first PC model ever), with a 4.77 MHz 8088 CPU, 512 KB RAM and a CGA card, running IBM DOS 3.30
  * [HP 200LX](<http://en.wikipedia.org/wiki/HP_200LX>), with a 7.91 MHz 80186 CPU, running MS-DOS 5.0
  * IBM PS/2 Model 30-286, with a 10 MHz 80286 CPU and an AMD 287 FPU, running MS-DOS 6.22
  * various boring 32-bit and 64-bit machines :)
  * [DOSBox](<http://en.wikipedia.org/wiki/DOSBox>)



### Supported memory models

#### Tiny

  * Activated by the `-WmTiny` compiler option
  * Code + Data + Heap + Stack <= 64KB
  * CS = DS = SS
  * Pointer = NearPointer
  * CodePointer = NearPointer
  * Code starts at offset $100
  * Can produce both .com and .exe files. The binary format can be chosen with the `-Wtcom` and `-Wtexe` compiler options. The default format is .exe
  * The compiler defines: FPC_MM_TINY



#### Small

  * Activated by the `-WmSmall` compiler option. This is the default memory model, so it is chosen if you don't specify a memory model.
  * Code <= 64KB, Data + Heap + Stack <= 64KB (Code and data are in separate segments, so programs can use up to 128KB in total)
  * DS = SS
  * Pointer = NearPointer
  * CodePointer = NearPointer
  * Can produce only .exe files
  * The compiler defines: FPC_MM_SMALL



#### Medium

  * Activated by the `-WmMedium` compiler option
  * Code <= 1MB, Data + Heap + Stack <= 64KB
  * DS = SS
  * Pointer = NearPointer
  * CodePointer = FarPointer
  * Can produce only .exe files
  * The compiler defines: FPC_MM_MEDIUM



#### Compact

  * Activated by the `-WmCompact` compiler option
  * Code <= 64KB, Data <= 64KB, Stack <= 64KB, Heap <= 1MB
  * Data structures cannot exceed 64KB
  * Pointer = FarPointer
  * CodePointer = NearPointer
  * Can produce only .exe files
  * The compiler defines: FPC_MM_COMPACT



#### Large

  * Activated by the `-WmLarge` compiler option. This is the memory model used by Turbo Pascal version 4 and above
  * Code <= 1MB, Data <= 64KB, Stack <= 64KB, Heap <= 1MB
  * Data structures cannot exceed 64KB
  * Pointer = FarPointer
  * CodePointer = FarPointer
  * Can produce only .exe files
  * The compiler defines: FPC_MM_LARGE



#### Huge

  * Activated by the `-WmHuge` compiler option
  * Code <= 1MB, Data <= 1MB, Stack <= 64KB, Heap <= 1MB
  * Static data of a single pascal module (i.e. unit or program) cannot exceed 64KB
  * Data structures cannot exceed 64KB
  * Pointer = FarPointer
  * CodePointer = FarPointer
  * Can produce only .exe files
  * The compiler defines: FPC_MM_HUGE



### Units and programs exceeding 64KB of code

Although the medium, large and huge memory models support up to 1MB of code for the whole program+units, there's still a limit of 64kb code per pascal module (i.e. unit or program). If this limit is exceeded, you can overcome this by splitting the unit or program that exceeded 64KB into smaller units, so that each of them stays within the 64KB limit. But there's also an easier solution. You can put the following directive in the module that exceeds 64KB: 
    
    
    {$HUGECODE on}
    

This forces each procedure and function to be in a separate segment. Note that since the code of the unit is no longer in a single segment, this also disallows mixing near and far procedures in the same unit, so in this mode, 'near' and {$F-} are ignored. 

Using {$HUGECODE on} does not count as using a different memory model, because you can freely mix units with {$HUGECODE on} and {$HUGECODE off} in the same program. 

### Supported calling conventions

Currently, only the Pascal calling convention is supported. All the other calling conventions are entirely untested and should not be used, because they may or may not work and may change at any time. 

#### Pascal

This is the default calling convention. It strives for compatibility with Turbo Pascal 7. 

  * Parameters are pushed on the stack left to right.
  * The callee cleans the parameters from the stack.
  * Procedures and functions must preserve DS, SS and BP and may destroy AX, BX, CX, DX, SI, DI and ES.
  * 16-bit results are returned in AX, 32-bit results in DX:AX
  * 64-bit ints are returned in AX:BX:CX:DX. Note that Turbo Pascal doesn't support int64, so this behavior is borrowed from Open Watcom's 'pascal' calling convention.



## Troubleshooting

### Compile error: missing .o files

Problem: If you see errors/warnings like this during compiling & linking: 
    
    
    test.pas(12,24) Warning: Object system.o not found, Linking may fail !
    Error! E2008: cannot open system.o : No such file or directory
    

(and indeed there don't appear to be any .o files in your msdos units directory) 

Solution: You haven't used smartlinking. You should always compile i8086-msdos programs with smartlinking, i.e. use the -XX and -CX options. Smartlinking uses .a files, nonsmartlinking uses .o files. 

The reason the .o files aren't generated is because the system unit exceeds the 64kb code limit for the small and tiny memory models and is therefore impossible to compile even 'hello world' without smartlinking, so creating the .o files is a waste of time. It might work for the medium model, but then you have to build the snapshot without -CX. However, with the tight memory constraints of real mode DOS, there is little reason not to use smartlinking in every memory model. Also, the medium model still has a limit of 64k code per unit, so even there it might not work without smartlinking. It certainly hasn't been tested. 

Hint: You can add these options to your fpc.cfg file, as described in the "Updating your fpc.cfg" section. 

## Running the testsuite

### Using DOSBox

  * build a snapshot for the memory model you want to test
  * it is recomended to use a 32-bit x86 build of DOSBox, because the 64-bit one has inaccurate FPU emulation
  * compile fpc/tests/utils/dosbox/dosbox_wrapper.pas for your native platform
  * prepare and run a script like this:


    
    
    #!/bin/sh
     
    FPCDIR=/home/blablabla/fpc-i8086/fpc
    MEMMODEL=Small
     
    export FPCMAKE=$FPCDIR/utils/fpcm/fpcmake
    export DOSBOX=/usr/bin/dosbox
    export SDL_VIDEODRIVER=dummy
    export SDL_AUDIODRIVER=dummy
     
    cd $FPCDIR/tests/
     
    ulimit -m 256000
    #ulimit -v 256000
    ulimit -d 256000
     
    echo running testsuite
    make full FPCFPMAKE=$FPCDIR/compiler/ppcx64 TEST_FPC=$FPCDIR/compiler/ppcross8086 TEST_OPT=-Wm$MEMMODEL EMULATOR=$FPCDIR/tests/utils/dosbox/dosbox_wrapper
    

## See also

  * [GO32V2](<GO32V2.md> "GO32V2") \- DOS Extender
  * [go32v2 development](<go32v2_development.md> "go32v2 development")

---

_Source: [https://wiki.freepascal.org/DOS](https://web.archive.org/web/20250120071724/https://wiki.freepascal.org/DOS)_
