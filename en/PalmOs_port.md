# PalmOS port

│ **English (en)** │  **[español (es)](</PalmOS_port/es> "PalmOS port/es")** │  **[português (pt)](</PalmOS_port/pt> "PalmOS port/pt")** │    
****

Currently, the PalmOS port is a retro, "just for fun" port of the compiler and runtime libraries, based on an earlier, incomplete effort from over a decade ago. The port was originally started by Mazen Neifer. Peter Vreman ported PalmOS API headers. m68k port and 3.1.x+ compiler support and maintenance by [Károly Balogh](</User:Chain-Q> "User:Chain-Q"). The PalmOS port is a crosscompiler-only target. 

## Contents

  * 1 Status
  * 2 CPU Defaults
  * 3 Identification
  * 4 Calling conventions
    * 4.1 Registers
    * 4.2 SysCalls
  * 5 Building Tutorial
    * 5.1 Cross binutils
    * 5.2 Cross compiler
  * 6 Examples
  * 7 Running binaries
    * 7.1 Emulator
    * 7.2 Real Device
  * 8 Debugging PalmOS applications
  * 9 Links
  * 10 Contacts



## Status

  * The 3.1.x compiler has compiler support (very experimental) for PalmOS.
  * [m68k](<m68k.md> "m68k") CPU is supported, including syscall generation support. Only small code is supported at the moment.
  * [ARM](<ARM.md> "ARM") CPU is not yet supported
  * Base RTL units are buildable but functionality is minimal.
  * There's a **palmunits** package, basic functionality was tested successfully.
  * Resource compilation is broken at the moment.



## CPU Defaults

The [m68k](<m68k.md> "m68k") PalmOS port defaults to 68000 CPU subarchitecture and no FPU. Note that by default, the PalmOS port doesn't include the RTL's SoftFPU. For floating point calculations, one can use the OS supplied floating point unit helper API, or recompile the RTL with the SoftFPU included, using argument **-CfSOFT**. 

## Identification

To identify PalmOS exclusively during compile-time, use **{$IFDEF PALMOS}**. 

## Calling conventions

### Registers

As a difference to the standard [m68k register layout](<m68k.md> "m68k"), on PalmOS also **d2** and **a2** registers are used as scratch registers, and their contents are not preserved. This is dictated by the ABI used by the C compilers and the syscall convention of the platform. Additionally, register **a5** is used as a pointer to the global data. On PalmOS, register **a5** points to the end of the global data. 

### SysCalls

It's not required to use inline assembly to do system calls. Any trap function can be defined the following way: 
    
    
    function MemPtrNew(size: UInt32): MemPtr; syscall $A013;
    

Note the syscall modifier in the function declaration. The argument to the syscall modifier is the trap number to call. The arguments to a syscall function are always passed on the stack and they're word (2 byte) aligned. Optionally, the numerical trap ID (here shown in hexadecimal) can be defined by a constant. For further examples, see the file _rtl/palmos/palmos.inc_ in the RTL source or the **palmunits** package. 

## Building Tutorial

The section below details building an m68k-palmos Free Pascal cross compiler. The ARM building process is the same with the different CPU name. Note that the ARM PalmOS support is non functional at the moment. The tutorial below uses PATHs tuned for Linux and macOS systems. The FPC directory structure on Windows can be slightly different to these. 

### Cross binutils

The older version of GNU binutils included in [prc-tools](<http://prc-tools.sourceforge.net>) is difficult to build on modern systems, like macOS/Darwin 10.12+ or any 64-bit Linux. It is recommended to use the [prc-tools-remix](<https://github.com/chainq/prc-tools-remix>) repository instead. This supports both m68k and ARM prc tools, and it is fixed to build and work on current systems. It also provides up to date installation instructions, see there. 

From prc-tools, Free Pascal uses the followings: 

  * as
  * ld
  * ar
  * build-prc



Make sure they're all on the path or in the specified tools directory, and they all share the same prefix, like **m68k-palmos-**. 

### Cross compiler

PalmOS is a cross-compiler only target. The following steps can be used build a PalmOS cross-compiler: 

  1. Install the latest stable Free Pascal Compiler. This will be used as the startup compiler.
  2. Check out FPC SVN trunk into a directory.
  3. Make sure the cross-binutils are properly built, and its binaries are actually in the **PATH**.
  4. Go to the SVN trunk root directory and use the following command to build an m68k-palmos cross-compiler:


    
    
      make clean crossall crossinstall OS_TARGET=palmos CPU_TARGET=m68k CROSSOPT="-XX -CX" INSTALL_PREFIX="<path/to/install>"
    

If everything went correctly, you should find a working PalmOS cross-compiler at the install path you specified. 

Now, lets create a default fpc.cfg for PalmOS cross compiling. Create a file called **< path/to/install>/lib/fpc/etc/fpc.cfg**. Put the following lines into that file, and fix up the paths: 
    
    
    #IFDEF CPUM68K
    -Fu<path/to/install>/lib/fpc/$fpcversion/units/$FPCTARGET
    -Fu<path/to/install>/lib/fpc/$fpcversion/units/$FPCTARGET/*
    #IFDEF PALMOS
    -FD</path/to/m68k-palmos-binutils>
    -XPm68k-palmos-
    -XX
    -CX
    #ENDIF
    #ENDIF
    

Note the **-CX** and **-XX** options used for both the build and the config file. They enable smartlinking by default. 

**Due to the size constraints PalmOS puts on the executables, and the limitations of the old prc-tools binutils, building and compiling without smartlinking is not supported.**

Optionally add **< path/to/install>/lib/fpc/3.1.1/** directory to the PATH, so you'll have direct access to the cross compiler. If you've done everything right, you now should be able to build PalmOS executables from a Pascal source the following way: 
    
    
    ppcross68k -Tpalmos <source.pas>
    

Copy the resulting **.prc** executable to your PalmOS device using HotSync or your preferred method. 

## Examples

The '_palmunits_ package contains a few examples, which are a good starting point. They should be directly buildable from the command line. They're all GUI applications. Console applications are not supported at this moment. 

## Running binaries

### Emulator

The old POSE Emulator of 68k-based Palm devices is Windows only, but works great with WINE on macOS and Linux. You will also need a set of Palm ROMs and skins for the emulator. These are available from various locations over the internet, preserving old Palm software and developer tools. Due to the uncertain nature of these redistributions, we cannot directly link these here, sadly. 

[![](https://wiki.freepascal.org/images/3/32/fpc_cube_on_pose.png)](</File:fpc_cube_on_pose.png>)

[](</File:fpc_cube_on_pose.png> "Enlarge")

FPC Cube! example program running on POSE

### Real Device

On Linux and macOS, the open source command line **pilot-link** was tested and works for uploading FPC generated binaries. **pilot-link** is still available from most distributions' package manager, [or in source from here](<https://github.com/jichu4n/pilot-link>). There are also various GUI front-ends for **pilot-link** , like [JPilot](<http://jpilot.org>). 

On Windows or macOS, the original Palm Desktop software should work as well. 

## Debugging PalmOS applications

This section is not yet available 

## Links

  * [Buildfaq](<http://www.stack.nl/~marcov/buildfaq.pdf>) is a general FAQ about how to build and configure FPC.



Various Palm related information sources 

  * [PRC format](<http://web.mit.edu/tytso/www/pilot/prc-format.html>) describes the Palm executable and resource format
  * [pilot-link howto](<http://www.tldp.org/HOWTO/PalmOS-HOWTO/pilotlink.html>) describes the usage of pilot-link and transferring files and data to/from the device



Here are some links related to ARM CPU Architecture 

  * [GCC ARM Improvement Project](<http://www.inf.u-szeged.hu/gcc-arm/>)
  * [ARM ASSEMBLER](<http://www.heyrick.co.uk/assembler/index.html>) Good information and codes related to arm assembly language.
  * [ARM Instruction Sets & Programs](<http://soc.csie.ndhu.edu.tw/source/intro_embedded/ch2-arm-2.ppt>) Very good and consice information about arm architecture
  * [The ARM Instruction Set ](<http://web.njit.edu/~baltrush/arm_stuff/ARMInst.ppt>) Another fine power point file about arm



## Contacts

Current maintainer: [Károly Balogh](</User:Chain-Q> "User:Chain-Q")

Original author: [Mazen NEIFER](<mailto:mazen@freepascal.org>)

---

_Source: [https://wiki.freepascal.org/PalmOs_port](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/PalmOs_port)_
