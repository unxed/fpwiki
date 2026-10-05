# AmigaOS

[![AmigaOS-boingball.png](https://wiki.freepascal.org/images/c/ce/AmigaOS-boingball.png)](</File:AmigaOS-boingball.png>)

This article applies to [AmigaOS 4.x](</Category:AmigaOS> "Category:AmigaOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

This page contains information about the AmigaOS4 on PowerPC port, which is in an **experimental** stage. About classic [Amiga](<Amiga.md> "Amiga") (OS 3.x, Motorola 680x0) port, please see the [relevant page](<Amiga.md> "Amiga"). About other Amiga-like systems supported by Free Pascal, see [AROS](<AROS.md> "AROS") and [MorphOS](<MorphOS.md> "MorphOS") pages. **The AmigaOS4 port has no official maintainer at this point, which means no new features are being added.**

The initial AmigaOS4 port was done by [Karoly 'Chain-Q' Balogh](</User:Chain-Q> "User:Chain-Q"), maintainer of [MorphOS](<MorphOS.md> "MorphOS") and classic [Amiga](<Amiga.md> "Amiga") ports. 

## Contents

  * 1 Identification
  * 2 Stack
  * 3 Naming conventions
    * 3.1 Constants
    * 3.2 Structures and types
  * 4 Threading
  * 5 Cross compiling
    * 5.1 Cross compiling from Linux
    * 5.2 Bootstrapping a compiler from Linux
  * 6 Assembler and Linker
    * 6.1 Linker problems
    * 6.2 Assembler problems
  * 7 Preliminary information about language elements to support the port
    * 7.1 Library interfaces and Syscalls
  * 8 More information



## Identification

To identify AmigaOS4 exclusively during compile-time, use **{$IFDEF AMIGAOS4}**. To identify classic Amiga or AmigaOS4, use **{$IFDEF AMIGA}**. To identify any Amiga-like system, including [AROS](<AROS.md> "AROS") and [MorphOS](<MorphOS.md> "MorphOS"), use **{$IFDEF HASAMIGA}**. This is similar to **HASUNIX** which is defined across Unix systems. Please note that **HASAMIGA** define is only available in Free Pascal 3.0 and up. 

## Stack

On AmigaOS4, the default stack is set to 256 KiB. Stack size can be set directly by the programmer with a compiler switch (-Cs), or with the **{$MEMORY}** directive. On AmigaOS4 this feature just sets the "stack cookie" in the executable, [as described here](<http://wiki.amigaos.net/wiki/Controlling_Application_Stack>). You can also reduce the stack size, but note that AmigaOS4 won't allow stack sizes below 32 KiB. 

## Naming conventions

This section describes the differences between the official AmigaOS4 SDK, which is for C language, and the AmigaOS4-specific units in FPC RTL. 

### Constants

System constants are named exactly like in the AmigaOS4 SDK. 

### Structures and types

System structures are named similar like in the AmigaOS4 SDK, but follows the Pascal convention about type naming, so structure names has a T before, and each type has a pointer to type with P before the name. The following example should make things trivial: 
    
    
     struct Task equals to TTask
     struct Task * equals to PTask
    

## Threading

Threading is supported on Amiga-like systems since Free Pascal 3.0, using the [AThreads](<AThreads.md> "AThreads") unit. Read there for possible caveats and unsupported features. 

## Cross compiling

While the following sections describe cross-compiling from Linux, you can use Mac OS X and Windows as well, as long as you can provide the required cross-binutils. Also on other systems some paths might be different from the ones described below. 

### Cross compiling from Linux

It's possible to cross-build the AmigaOS4 compiler from Linux/i386 and Linux/x86-64 (probably other CPUs too, but it was not tested). To do that, you need Linux to AmigaOS4 cross-binutils, mainly GNU as, ld, and optionally ar and strip. It's best if you build these from GNU sources, along with the rest of cross-build GNU toolchain. 

To build an AmigaOS4 cross-compiler, use the following steps: 

  1. Install FPC the latest stable compiler, at the time of the writing of this article this is FPC 3.0.0. This will be used as the startup compiler.
  2. Check out FPC SVN trunk into a directory.
  3. Make sure the aforementioned cross-binutils are actually in the PATH.
  4. Go to the trunk's main directory and use the following command to build an FPC to AmigaOS4 cross-compiler running on Linux:


    
    
      make clean crossall crossinstall OS_TARGET=amiga CPU_TARGET=powerpc BINUTILSPREFIX=ppc-amigaos- INSTALL_PREFIX="<path/to/install>"
    

If you've done everything right, you should find a working AmigaOS4 cross-compiler in the install path you've specified. 

Now, lets create a default fpc.cfg for AmigaOS4 cross compiling. Create a file called **< path/to/install>/lib/fpc/etc/fpc.cfg**. Put the following lines into that file, and fix up the paths: 
    
    
    #IFDEF CPUPOWERPC
    -Fu<path/to/install>/lib/fpc/$fpcversion/units/$FPCTARGET
    -Fu<path/to/install>/lib/fpc/$fpcversion/units/$FPCTARGET/*
    #IFDEF AMIGAOS4
    -FD</path/to/amigaos4-cross-binutils>
    -XPppc-amigaos-
    #ENDIF
    #ENDIF
    

Optionally add **< path/to/install>/lib/fpc/3.1.1/** directory to the PATH, so you'll have direct access to the cross compiler. If you've done everything right, you now should be able to build AmigaOS4 executables from Pascal source on Linux the following way: 
    
    
    ppcrossppc -Tamiga <source.pas>
    

Copy the resulting executable to AmigaOS4, and that's it! 

### Bootstrapping a compiler from Linux

To bootstrap a Free Pascal Compiler from Linux to AmigaOS4, first please complete the previous section. 

After you have a working cross compiler, go to the SVN **trunk/compiler** directory, and issue the following commands: 
    
    
     make compiler OS_TARGET=amiga CPU_TARGET=powerpc FPC="<path/to/install>/lib/fpc/3.1.1/ppcrossppc"
    

Copy the resulting **ppcppc** executable to AmigaOS4. On the OS4 box, try to cycle a compiler the following way in the SVN's **trunk/compiler** directory: 
    
    
    set PATH=/SDK/Local/C
    gmake cycle FPC=/path/to/ppcppc
    

Please note you need to use **gmake** not **make** , on AmigaOS4. The resulting required compiler is **trunk/compiler/ppcppc** , the RTL files are in **rtl/units/powerpc-amiga**. 

At the end of the cycle, you might an error, but this is caused by a linker or an assembler problem and it doesn't affects the compiler's functionality. Read the relevant section below. Congratulations, you've just successfully bootstrapped Free Pascal on AmigaOS4! 

  


## Assembler and Linker

Free Pascal for AmigaOS4 uses **vlink** by Frank Wille as the default linker, when running natively on AmigaOS4. The cross-compilers still default to **GNU ld**. **vlink** is open source, and it is available [here](<http://sun.hasenbraten.de/vlink>). Binaries are available as part of the [vbcc compiler package](<http://sun.hasenbraten.de/vbcc>). 

You can change the linker back to **GNU ld** by adding _-XV-_ argument when compiling. For cross compilers _-XV_ argument enables vlink. 

### Linker problems

The native GNU LD on AmigaOS4 has a problem, which prevents FPC's **make cycle** to succeed natively. To fulfill some requirements of the system, it pads sections in unstriped executables, but it does so using random memory data. This means the binaries will be different, depending on the memory state during compile time. This makes the final binary verification of **make cycle** to fail. Note that the resulting _ppcppc_ binary is still fully functional. Note that the AmigaOS4 developers know about this problem, but they consider it "harmless". For us, it's a blocker. Also the AmigaOS4 port doesn't support striping with GNU LD currently, which is a missing feature on our side. 

### Assembler problems

Some native GNU AS versions distributed in binary might have an issue, which causes the branch-prediction hint bits of some PowerPC branch instructions to change randomly. This causes no directly observable functional differences, but might cause a penalty hit with time critical code, and might cause FPC's own build process to fail (due to binary sanity checks). GNU AS 2.18 as distributed with Hyperion's SDK is not affected. 

## Preliminary information about language elements to support the port

**BIG FAT WARNING!** All information below this line is preliminary, and subject to change without any further notice. OK, you've been warned. 

### Library interfaces and Syscalls

Syscalls are used in AmigaOS4, to provide calls to libraries through OS4 interfaces. Syntax is proposed to be the following: 
    
    
    var
      myLibBase: PLibrary;
     
    type
      TmyLibIface = record
        Data: TInterface;
     
        Obtain: function(self as hidden): longword; SysCall;
        Release: function(self as hidden): longword; SysCall;
        Expunge: procedure(self as hidden); SysCall;
        Clone: function(self as hidden): POS4Interface; SysCall;
         
        myCallFoo: function(self as hidden; a: longint; b: pointer): pointer; SysCall;
        myCallBar: procedure(self as hidden; c: pchar; d: byte); SysCall;
      end;
      PmyLibIface = ^TmyLibIface;
     
    var
      ImyLib: PmyLibIface;
    begin
      // here the opening the library and interface, blah...
      
      ImyLib^.myCallFoo(0,nil);
      ImyLib^.myCallBar('a',1);   
    
      // releasing stuff
    end;
    

The above example calls will pass pointer pointer to the interface in **r3** , pass the remaining arguments according to SysV ABI. Syscall is needed to enable the passing of the hidden argument, which is only supported for syscalls. A hidden argument can be any integer or pointer variable. They're not need to be specified in the actual calls in the code, but they're still passed to called function. On AmigaOS4, identifier "Self" has a special meaning, and means a pointer to the actual interface. 

An utility which helps creating a Pascal headers from interface definition files for existing libraries, similar to what IDLTool does for C language, would be highly recommended. 

## More information

More information regarding Free Pascal and AmigaOS can be read on the [Free Pascal 4 Amiga wiki](<http://fpcamigawiki.alb42.de>). This wiki also contains links to (unofficial) nightly downloads and other (additional) information for AmigaOS.

---

_Source: [https://wiki.freepascal.org/AmigaOS](https://web.archive.org/web/20250421153745/https://wiki.freepascal.org/AmigaOS)_
