# Win16

## Contents

  * 1 Overview
  * 2 Requirements
    * 2.1 FPC trunk
    * 2.2 Building a snapshot manually
  * 3 Status



## Overview

A Win16 cross compiler is currently being developed in FPC trunk (the development version). It targets Windows 3.0 or later. 

## Requirements

### FPC trunk

The compiler is a cross compiler that runs at least on Windows (both x86 and x64), Linux, and Mac OS X. For compiling programs, it needs: 

  * the Open Watcom linker - WLINK
  * the Open Watcom library manager - WLIB
  * the Netwide Assembler - [NASM](<http://www.nasm.us/>)



In theory it should be able to run on any platform, supported by FPC, where NASM and the Open Watcom tools are available. This includes DOS via the GO32V2 extender. However, because the Watcom tools for DOS are compiled with a different extender, there are some issues related to long file names and the passing of long command line arguments. 

### Building a snapshot manually

  * make sure **nasm** , **wlink** and **wlib** are in your path
  * checkout fpc trunk:


    
    
    svn checkout <http://svn.freepascal.org/svn/fpc/trunk> fpc
    

  * enter the fpc directory and build the compiler with the following command (replace **/usr/bin/ppc386** with the full path to the stable (3.0.4) FPC compiler binary; replace **-WmLarge** with **-WmMedium** , **-WmCompact** or **-WmSmall** if you want to use another memory model, although using **large** is recommended):


    
    
    make clean all OS_TARGET=win16 CPU_TARGET=i8086 OPT="-CX -XXs" CROSSOPT=**-WmLarge** BINUTILSPREFIX= PP=**/usr/bin/ppc386**
    

  * install the snapshot (replace **linux** and **i386** with the OS and CPU you're using; replace **INSTALL_PREFIX** with the directory you want the snapshot installed):


    
    
    make crossinstall OS_SOURCE=**linux** CPU_SOURCE=**i386** OS_TARGET=win16 CPU_TARGET=i8086 PP=compiler/ppcross8086 CROSSOPT=**-WmLarge** \
        BINUTILSPREFIX= INSTALL_PREFIX=**/home/blablabla/fpc-i8086/snapshot-win16/large** OPT="-CX -XXs"
    

## Status

The RTL compiles. Not that not all packages work in memory models, other than **large** or **medium**. This is because, currently the **small** and **compact** memory models don't support generating far procedures (the _far_ directive is ignored in these models), and that's what the Win16 API expects to use for callback functions. The RTL supports long file names when run under Windows 95+/2000+. The **dos** unit works (unlike Borland Pascal 7, it is still called **dos** and not **windos**). Most of the core win16 api is available through the **wintypes** and **winprocs** units, similarly to Borland Pascal 7. The **win31** unit is also available, providing support for the extra APIs, added in Windows 3.1. The **crt** , **video** , **keyboard** and **mouse** units compile and work (although there's no mouse supported implemented yet), by providing an emulated text console (since Win16 applications can be GUI only). Note that **crt** is also called **crt** (and not **wincrt** like in BP7). FPC demo program _fpctris_ works. _Samegame_ should also work, as soon as mouse input support is added to the kvm units. **FV** (_Free Vision_) also compiles and mostly works, but is not well tested and may have bugs. The **sysutils** unit compiles, but does not yet work. The **graph** unit is **not** yet implemented. However, the **objects** and **objpas** units are available and work. 

Creating DLLs is not yet supported. 

Even though created EXE files are marked to support Windows 3.0 real mode, the real mode support is not stable yet (it needs all functions, including internal ones to be exported, which is not yet implemented). Therefore, running under protected mode Windows is highly recommended. 

Screenshot of fpctris, working under Windows for Workgroups 3.11: 

[![fpctris wfwg311.png](https://wiki.freepascal.org/images/c/c0/fpctris_wfwg311.png)](</File:fpctris_wfwg311.png>)

---

_Source: [https://wiki.freepascal.org/Win16](https://web.archive.org/web/20210304003441/https://wiki.freepascal.org/Win16)_
