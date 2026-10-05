# Human 68k

The Human 68k port is an experimental [m68k](<m68k.md> "m68k") target, mainly aimed at retro computer enthusiasts. It targets the Sharp X68000 line of computers, that existed from the mid-80s to the mid-90s as a Japanese-only home computer and gaming console, and their operating system, the MS-DOS-alike Human 68k by Hudson Soft. The initial version of the Human 68k port was implemented by [Karoly Balogh](</User:Chain-Q> "User:Chain-Q"). 

## Contents

  * 1 Status
  * 2 Requirements
  * 3 Identification
  * 4 SysCalls
  * 5 Defaults
    * 5.1 CPU
    * 5.2 Alignment
    * 5.3 Stack
  * 6 External Dependencies
    * 6.1 Assembler
      * 6.1.1 Building vasm
    * 6.2 Linker
    * 6.3 Licensing
  * 7 Cross compiling
    * 7.1 Dependencies
    * 7.2 Building
  * 8 OS Bindings
  * 9 Other resources



# Status

This port is currently a cross-compiler only target. This is mostly due to hardware limitations of the original unexpanded platform. 

# Requirements

The compiled binaries - depending on the size - should be able to run on any Sharp X68000. Memory expansion for more complex programs is highly recommended. 

# Identification

To identify the Human 68k target compile-time, use **{$IFDEF HUMAN68K}**. 

# SysCalls

Free Pascal supports generating Human 68k style system calls. Inline assembly code is not needed to call a system function. The syntax to declare Human 68k-style system calls is the following: 
    
    
    function h68kdos_write(fileno: word; buffer: pointer; len: longint): longint; syscall $ff40;
    

Note the syscall modifier in the function declaration. The argument to the syscall modifier is the DOS function number to call. All syscall parameters are passed on the stack in right-to-left order, and they're word (2 byte) aligned. For further examples, see _rtl/human68k/h68kdos.inc_ in the RTL source. 

  


# Defaults

This section documents several compiler default settings, which are specific to the Human 68k target. Settings not documented here are usually the default for the m68k CPU architecture. 

## CPU

The default CPU target for the Human 68k target is the 68000. This is different from most other [m68k](<m68k.md> "m68k") targets where the default is the 68020. SoftFPU is enabled by default. 

## Alignment

Record elements are aligned to WORD (2 bytes) by default. Use the **{$PACKRECORDS n}** compiler directive, if you want to change the default aligment. For byte aligned records, a packed record is also possible. Free Pascal also has support for the 68000 [special alignment requirements](<m68k.md> "m68k"). 

## Stack

The default stack size is set to 16 KiB, which is quite small for a complex Pascal program. For more complex programs you'll probably need to increase it. To change the stack size, use the **{SMEMORY <stacksize>,<heapsize>}** directive, or the **-Cs** command line argument. See the details in the documentation. 

  


# External Dependencies

The Human 68k port depends on **vasm** and **vlink** , as external assembler and linker. 

## Assembler

The only supported assembler for the Human 68k port is **vasm** , which is also the default. The Human 68k port uses **ELF** as the default object format. This means using **vasm** also requires **vlink** as linker. **vasm** is open source, and it is available [here](<http://sun.hasenbraten.de/vasm>). Only **vasm** versions 1.9f and newer were tested with the Human 68k port, older versions might not work. 

### Building vasm

Note that **vasm** for [m68k](<m68k.md> "m68k") needs to be built with the _standard_ syntax module to work with Free Pascal. This is not the default, as most programmers and compilers prefer the _Motorola_ syntax module. A **vasm** version with the _standard_ syntax module can be built with the following command, issued in the root of the **vasm** source tree: 
    
    
     make CPU=m68k SYNTAX=std
    

The resulting _vasmm68k_std_ executable file is the assembler Free Pascal needs. 

## Linker

The Human 68k port defaults to **vlink** by Frank Wille as the default linker, which is the only supported linker for this target. **vlink** is open source, and it is available [here](<http://sun.hasenbraten.de/vlink>). 

Please make sure you use **vlink** version 0.17a or newer. Earlier versions were never tested. 

## Licensing

Please note that the license of **vasm** and **vlink** does not allow development of commercial applications. If you plan to develop and sell a commercial application using these tools, you need their author's written consent. Please see the documentation of [vasm](<http://sun.hasenbraten.de/vasm/release/vasm.html>) and [vlink](<http://sun.hasenbraten.de/vlink/release/vlink.pdf>) for details. 

  


# Cross compiling

## Dependencies

Free Pascal needs **vlink** and **vasm** for the Human 68k target. Using GNU binutils is not supported for now. To get vlink and vasm built, see the relevant sections above. 

## Building

To build a Human 68k cross-compiler, use the following steps: 

  1. Install the latest stable FPC version, at the time of the writing of this article this is FPC 3.2.2. This will be used as the startup compiler.
  2. Clone the FPC git repository.
  3. Make sure you have vlink and vasm in the path
  4. Go to the repository's directory, make sure you are on the **main** branch and use the following command to build an FPC cross-compiler:


    
    
      make clean crossall crossinstall OS_TARGET=human68k CPU_TARGET=m68k INSTALL_PREFIX="<path/to/install>"
    

If you've done everything right, you should find a working Human 68k cross-compiler in the install path you've specified. 

Now, lets create a default fpc.cfg for Human 68k cross compiling. Create a file called **< path/to/install>/lib/fpc/etc/fpc.cfg**. Put the following lines into that file, and fix up the paths: 
    
    
    #IFDEF CPUM68K
    -Fu<path/to/install>/lib/fpc/$fpcversion/units/$FPCTARGET
    -Fu<path/to/install>/lib/fpc/$fpcversion/units/$FPCTARGET/*
    #IFDEF HUMAN68K
    -FD</path/to/human68k-binutils>
    -Cp68000
    #ENDIF
    #ENDIF
    

Optionally add **< path/to/install>/lib/fpc/3.3.1/** directory to the PATH, so you'll have direct access to the cross-compiler. If you've done everything right, you now should be able to build Human 68k executables: 
    
    
    ppcross68k -Thuman68k <source.pas>
    

  


# OS Bindings

There's a **h68units** package, that contains support code for calling OS functions and addressing the X68000 hardware directly. Currently this is mostly limited to DOS functions. The package is named **h68units** , and not **h68kunits** to support the 8+3 filename limitations of the target platform. This allows a future native compiler run, even if this is not possible at the moment. 

# Other resources

  * [GCC, binutils, and newlib port for Human 68k on GitHub by Lyderic "Lydux" Maillet](<https://github.com/lydux>)
  * [X68000 C programming tutorials on GitHub by Federico Roviralta](<https://github.com/FedericoTech/X68KTutorials>)

---

_Source: [https://wiki.freepascal.org/Human_68k](https://web.archive.org/web/20250122113421/https://wiki.freepascal.org/Human_68k)_
