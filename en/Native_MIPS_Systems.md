# Native MIPS Systems

The MIPS architecture is unusual in that it was designed for desktop computers, is now overwhelmingly used for embedded systems, but can still occasionally be found in desktops or small servers. The only comparable architecture is [ARM](<Native_ARM_Systems.md> "Native ARM Systems"), which is the platform of choice for a multitude of mobile phones, tablets etc. 

Like almost all computers, MIPS-based systems will contain an internal loader, usually in flash memory. However there are three basic categories of system: 

  * Those that boot an operating system from internal flash, such as the Linksys/Cisco WRT54G [[1]](<http://wiki.openwrt.org/>).


  * Those that boot an operating system from a conventional disc, connected via IDE, SCSI or USB, such as an SGI server or workstation, or a Cobalt/Sun Qube or Raq.


  * Those running as guests using an emulator such as [Qemu](<Qemu_and_other_emulators.md> "Qemu and other emulators") on a general-purpose computer.



In any of these cases it is necessary to be able to install a general-purpose operating system such as Linux in order to be able to run development tools. This writer (MarkMLl) favours Debian, since he finds that using it results in a very similar system on a wide range of hardware (x86, SPARC, PPC, MIPS, ARM, zSeries, 68K). 

**If you intend to build Lazarus, make sure you have at least 512Mb memory (RAM + swap) available.**

## Contents

  * 1 Microchip microcontrollers
  * 2 FPC Implementations for MIPS
    * 2.1 David Zhang's Compiler
    * 2.2 Mainline MIPS Port
      * 2.2.1 Stall point
    * 2.3 Big-endian subarchitecture
  * 3 See also



## Microchip microcontrollers

In 2013, Microchip enhanced their series of "PIC24" and "PIC33" chips with the "PIC32" variants, providing compatible (to a certain extent even pin-compatible) alternatives to the 16 bit chips (PIC32MX..."), as well as much more powerful ones ("PIC32MZ..."). The PIC32 chips are both cheaper and more versatile than the corresponding PIC24s. Hence they include both cheap and powerful controllers. The PIC32 chips feature a "M4K" MIPS CPU and so they could run software done with the FPC MIPS cross compiler. As usually these chips are used for doing "deeply embedded" systems: in most cases there will be no formal OS and the user code programmer will need to adapt the RTL to fit his needs. But as the C-based "MPLAB-X" IDE by Microchip comes with a huge library that provides basic functions (such as heap management), the "standard" RTL for such purpose should take this into account. 

## FPC Implementations for MIPS

As of 2011 there are two implementations of FPC for the MIPS architecture, both targeting Linux. 

  * A fork of the mainline compiler at 2.0.0 by David Zhang. _Now deprecated due to mainline integration._


  * An ongoing effort to reconcile this with the mainline project, in SVN trunk and partially completed in 2.7.x.



### David Zhang's Compiler

The "unofficial" MIPS compiler at [[2]](<http://sourceforge.net/projects/fpc-mips/develop>) was written by David Zhang (possibly [[3]](<http://www4.comp.polyu.edu.hk/~csdzhang/>)) and is licensed under GPL Version 2, it cites but does not include the COPYING.FPC file. The compiler appears to have been originally based on the SPARC port, and there are significant differences between it and the mainline sources. It is discussed for historical interest, and as reference until the mainline compiler is at least as complete and robust. 

There are .zip files of both sources and a minimal executable at Sourceforge (link above). The sources are basically a complete reimplementation of fpcsrc/compiler/mips based on FPC 2.0.0, the compiler targets mipsel and is described as being operational on Qemu but not yet supporting shared libraries. 

Since this is specifically a little-endian compiler it should presumably be compatible with Linux on a MIPS-based Qube or Raq, or with the Chinese "Loongson" processor. It will not be compatible with SGI MIPS systems, which are big-endian. 

There is a system library bundled with the compiler, but no indication of how far the compiler is capable of handling the remainder of the standard libraries. The released files may be downloaded from: 

  * <http://sourceforge.net/projects/fpc-mips/files/fpc-mips/0.90/fpc_mips_bin_090.zip/download>



This comprises the binary compiler pp_mipsel32. It successfully compiles a minimal test program which appears to run. 

  * <http://sourceforge.net/projects/fpc-mips/files/fpc-mips/0.90/fpc_mips_all_090a.zip/download>



This is both source and binary, dumped into a single directory which appears to correspond to the standard fpc_200/compiler, i.e. there's no separate compiler/mips directory; it also partially reimplements some standard libraries etc. 

### Mainline MIPS Port

This is part of the trunk development files, but does not yet appear in standard releases; i.e. it's only available via an svn download. 

The host is assumed to be a non-MIPS system, for example x86 running Linux (e.g. Debian "Squeeze"). It needs a copy of binutils, and it is probably expedient to make this the same version as is running natively, i.e. 2.20.1 in the above case, downloaded from [[4]](<http://ftp.gnu.org/gnu/binutils/binutils-2.20.1.tar.bz2>). In any event, no version older than 2.18 will work since symbolic register names are a prerequisite. 

Build and install cross-binutils: 
    
    
    tar xjf binutils-2.20.1.tar.bz2
    cd binutils-2.20.1
    ./config.sub mipsel-linux-gnu
    ./configure --prefix=/usr/local/mipsel-linux mipsel-linux-gnu
    make
    sudo make install
    sudo ln -s /usr/local/mipsel-linux/bin/as /usr/local/bin/mipsel-linux-as
    sudo ln -s /usr/local/mipsel-linux/bin/ld /usr/local/bin/mipsel-linux-ld
    sudo ln -s /usr/local/mipsel-linux/bin/ar /usr/local/bin/mipsel-linux-ar
    sudo ln -s /usr/local/mipsel-linux/bin/objdump /usr/local/bin/mipsel-linux-objdump
    sudo ln -s /usr/local/mipsel-linux/bin/objcopy /usr/local/bin/mipsel-linux-objcopy
    sudo ln -s /usr/local/mipsel-linux/bin/strip /usr/local/bin/mipsel-linux-strip
    mipsel-linux-ld -V
    

Particularly if working with the trunk sources, consider doing the same for a different CPU (e.g. powerpc-linux-gnu) so as not to be caught out if they won't build due to a temporary inconsistency. 

Build a compiler which runs on the host but targets mipsel, note debug options which are needed later: 
    
    
    cd /usr/local/src/fpc-trunk
    export PP=
    make "OPT=-O- -g" -C compiler mipsel
    mv compiler/ppcmipsel compiler/ppcXmipsel
    

Renaming the compiler prevents it from being deleted by make clean etc., consider extending that OPT setting with -dEXTDEBUG. 
    
    
    export PP=/usr/local/src/fpc-trunk/fpc/compiler/ppcXmipsel
    make CPU_TARGET=mipsel OS_TARGET=linux rtl
    make CPU_TARGET=mipsel OS_TARGET=linux compiler
    

Move the newly-generated compiler and the RTL units to the target system, which should now be able to do a full build. 

#### Stall point

As of October 2012 there are still some code-generation tests failing. 

To be continued (hopefully :-) 

As a general point, there's some useful thoughts on binary disassembly at [[5]](<http://chdk.wikia.com/wiki/GPL_Disassembling>) for situations where IDA or equivalent aren't available. 

### Big-endian subarchitecture

_Working notes, no permanent links._

This is a highly-experimental investigation of how much work it would take to port the existing little-endian compiler (mipsel) to big-endian architectures. Note that the GNU triplet is mips-linux-gnu, there is no such thing as mipseb-linux-gnu etc. 

Build and install cross-binutils: 
    
    
    tar xjf binutils-2.20.1.tar.bz2
    cd binutils-2.20.1
    ./config.sub mips-linux-gnu
    ./configure --prefix=/usr/local/mips-linux mips-linux-gnu
    make
    sudo make install
    sudo ln -s /usr/local/mips-linux/bin/as /usr/local/bin/mips-linux-as
    sudo ln -s /usr/local/mips-linux/bin/ld /usr/local/bin/mips-linux-ld
    sudo ln -s /usr/local/mips-linux/bin/ar /usr/local/bin/mips-linux-ar
    sudo ln -s /usr/local/mips-linux/bin/objdump /usr/local/bin/mips-linux-objdump
    sudo ln -s /usr/local/mips-linux/bin/objcopy /usr/local/bin/mips-linux-objcopy
    sudo ln -s /usr/local/mips-linux/bin/strip /usr/local/bin/mips-linux-strip
    mips-linux-ld -V
    

Build a compiler which runs on the host but targets mipseb, note debug options which are needed later: 
    
    
    cd /usr/local/src/fpc-trunk
    export PP=
    make "OPT=-O- -g" -C compiler mips
    mv compiler/ppcmips compiler/ppcXmips
    

For my own reference, actually using /usr/local/src/fpc/zhang/fpc-trunk/fpc 
    
    
    export PP=/usr/local/src/fpc-trunk/fpc/compiler/ppcXmips
    make CPU_TARGET=mips OS_TARGET=linux rtl
    # make CPU_TARGET=mips OS_TARGET=linux compiler
    

Build a compiler which runs on the target: 
    
    
    export PP=/usr/local/src/fpc-trunk/fpc/compiler/ppcXmips
    make "OPT=-O- -g" -C compiler mips
    
    
    
    $PP -i |head
    Free Pascal Compiler version 2.7.1
    
    Compiler Date      : 2012/11/02
    Compiler CPU Target: mipseb
    
    Supported targets:
      Linux for MIPSEB
    
    file compiler/ppcmips
    compiler/ppcmips: ELF 32-bit MSB executable, MIPS, MIPS-II version 1 (SYSV), statically linked, stripped
    
    file rtl/units/mips-linux/system.o
    rtl/units/mips-linux/system.o: ELF 32-bit MSB relocatable, MIPS, MIPS-II version 1 (SYSV), not stripped
    

## See also

  * [TARGET Embedded Mipsel](<TARGET_Embedded_Mipsel.md> "TARGET Embedded Mipsel")

---

_Source: [https://wiki.freepascal.org/Native_MIPS_Systems](https://web.archive.org/web/20250114082848/https://wiki.freepascal.org/Native_MIPS_Systems)_
