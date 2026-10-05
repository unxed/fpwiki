# MSX-DOS

│ **English (en)** │    
****

MSX-DOS is a CP/M-DOS-like operating system developed by Microsoft for the Z80 based MSX computer. 

Starting with revision r45600 Free Pascal has initial support for generating code for MSX-DOS. 

## Contents

  * 1 Overview
  * 2 Building the compiler
  * 3 Building a program
  * 4 Setting heap and stack size
  * 5 Known problems
  * 6 Differences to Turbo Pascal 3.0
    * 6.1 Support for units
    * 6.2 Inline assembly
    * 6.3 Inlining



## Overview

The compiler generates flat COM files which reside in the Transient Program Area (TPA) of the memory that reaches from $100 to the address of the DOS (which might start at something like $DEF0 or so). The top of the TPA is the place of the stack (at program start the top of the stack points below the start of the DOS) and FPC reserves an area for the heap inside the TPA as well. Thus the size for the program code and data is currently restricted to the remaining size. Mechanisms for (transparently) utilizing the slot mechanisms of the MSX need yet to be researched. Due to this restriction extreme care needs to be taken when linking in code as the limit can be reached quickly and quietly. 

At least MSX-DOS 2.0 or newer is required. 

In addition FPC produces rather verbose code compared to e.g. Turbo Pascal 3.0. It needs to be seen whether this can be improved in the future. 

As of revision r45600 the _System_ unit is supported, but neither the _DOS_ nor the _SysUtils_ unit are. The _ObjPas_ and _ISO7185_ units are compiled though not yet tested. Console as well as File I/O are implemented, though only the former is tested due to apparent bugs in the code generator. 

## Building the compiler

First of you need the _ihxutil_ utility. For this simply do a build of FPC for your host target which will build the utility as well and the resulting binary will then reside in _utils/ihxutil/bin/ <HOSTCPU>-<HOSTOS>_. 

Currently a _make all_ in the top level directory of the FPC sources does not fully succeed, but it works enough that the resulting compiler and RTL can be used, so use the following: 
    
    
    make all OS_TARGET=msxdos CPU_TARGET=z80 OPT=-CX
    

## Building a program

Once you've build the compiler and RTL you can then build programs using the following command: 
    
    
    ./compiler/ppcrossz80 -n -Tmsxdos -Furtl/units/z80-msxdos -viwn -FE<OUTPUTDIR> -FDutils/ihxutil/bin/<HOSTCPU>-<HOSTOS> -CX -XX file.pp
    

The options _-CX -XX_ are very important to enable smartlinking to have even remotely a chance to fit the program into the available space. 

If you place the _ihxutil_ utility into a directory reachable by the PATH environment variable you can leave out the _-FD..._ parameter. 

## Setting heap and stack size

The default heap size is 256 Byte and the default stack size is 1024. This can be changed using the _$MEMORY_ directive as documented [here](<https://www.freepascal.org/docs-html/current/prog/progsu102.html#x110-1110001.3.19>). The format of the directive is 
    
    
    {$MEMORY StackSize,HeapSize}
    

## Known problems

This is a list of currently known problems: 

  * Parameters are not correctly initialized (code generator problem?)
  * [Assign](<Assign.md> "Assign")() does not correctly set the filename (code generator problem?)
  * HexStr() does not result in correct hex strings (code generator problem?)
  * MkDir(), ChDir(), RmDir() are not implemented (MSX-DOS does not seem to support these?)



## Differences to Turbo Pascal 3.0

A common Pascal compiler for MSX is Turbo Pascal 3.0. There are a few significant differences between FPC and that version of Turbo Pascal (even if mode _TP_ is used). In general a look at the [Language Reference Guide](<https://www.freepascal.org/docs-html/current/ref/ref.html>) and maybe also the [Programmer's Guide](<https://www.freepascal.org/docs-html/current/prog/prog.html>) of FPC is suggested. 

### Support for units

Turbo Pascal 3.0 did not yet support the concept of units that premiered in newer versions of Turbo Pascal. FPC however does support them, so you can put your code into separate units. Though you can still use include files if you want to, but units like _System_ will be used automatically. 

See also [here](<https://www.freepascal.org/docs-html/current/ref/refse106.html#x216-23800016.2>)

### Inline assembly

FPC does not support Turbo Pascal's _Inline()_ intrinsic to provide inline assembly. Instead FPC provides a mnemonic based inline assembly that is started with **asm** and ended with **end**. Whole functions can be declared such if they're marked with the **assembler** directive (plus probably **nostackframe** to avoid the compiler setting up a stackframe). 

See also [here](<https://www.freepascal.org/docs-html/current/ref/refch18.html#x232-25400018>)

### Inlining

FPC can inline functions declared with the **inline** directive (there are some restrictions though, e.g. no assembly blocks allowed, no open array parameters and a few others), meaning that a call is avoided at the expense of potentially bloating the binary size, though it might lead to better optimized code (especially if constant parameters are used). 

See also [here](<https://www.freepascal.org/docs-html/current/ref/refsu73.html#x191-21300014.10.4>)

---

_Source: [https://wiki.freepascal.org/MSX-DOS](https://web.archive.org/web/20250114063214/https://wiki.freepascal.org/MSX-DOS)_
