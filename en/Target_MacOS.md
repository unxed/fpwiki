# Target MacOS

│ **English (en)** │

[![Logo OSX.png](https://wiki.freepascal.org/images/7/7a/Logo_OSX.png)](</File:Logo_OSX.png>)

This article applies to [Mac OS Classic](</Category:Mac_OS_Classic> "Category:Mac OS Classic") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

_MacOS_ is the target for Mac OS Classic, i.e. the predecessor to macOS. macOS itself was previously marketed as Mac OS X and OS X. For macOS, see [Target Darwin](<Target_Darwin.md> "Target Darwin"). 

## Contents

  * 1 Compiling in MPW
  * 2 Some tips
    * 2.1 Choosing apptype
    * 2.2 Resources
    * 2.3 Error messages
    * 2.4 Dos.Exec
    * 2.5 Assembler symbols
    * 2.6 QuickDraw globals
  * 3 Debugging
  * 4 Global variables
  * 5 Cross compiling from Mac OS X to Mac OS
    * 5.1 Crosscompiling step on macOS
    * 5.2 Assembling and linking step on Mac OS Classic
    * 5.3 Working notes: situation as of late 2012



## Compiling in MPW

Free Pascal can now be used in the [MPW](<MPW.md> "MPW") development environment. 

Free Pascal for MPW call assembler and linker via ToolServer. This means that you need ToolServer installed. This will normally not be an issue, since ToolServer is included in the MPW distribution. 

In rare instances, you might get the error message "Can't call the assembler, switching to external assembler." I do not know the origin of this, but it will help to restart. On Mac OS X you might need to restart even OSX. It might also indicate that you need to increase the memory setting for ToolServer. 

Another workaround for such problems is to let the compiler write out an MPW script (option -s). This will assemble and link when executed in MPW. This method can also be used if the compiler is to be called from a third program, to overcome the deadlock situation mentioned below under Dos.Exec. 

For crosscompiling e.g. from macOS to Mac OS Classic, see below. 

## Some tips

### Choosing apptype

For casual users, copying Pascal text only programs from a book, `{$APPTYPE CONSOLE}` is appropriate. 

If you want an MPW tool, use `{$APPTYPE TOOL}`. 

For maximum portability, the following: 
    
    
     {$IFDEF MACOS}
       {$APPTYPE TOOL}
     {$ELSE}
       {$APPTYPE CONSOLE}
     {$ENDIF}
    

... ensures it will be an MPW tool on Mac OS, and a console application in Delphi where {$APPTYPE TOOL} is not recognized. 

### Resources

Since 24-01-2004 mac style resources can be included with `{$R <resfile>}`. If the resource file ends with .r it is considered a Rez type resource file (in text form). For all other files (including .rsrc), binary resources are assumed. Several resource files can be included. 

### Error messages

Errors, warnings, hints and other messages written by the compiler are in MPW format - that is you can execute them and the error location will be displayed. 

### Dos.Exec

The procedure Dos.Exec is used to execute other programs. In MacOS this is supported, and the program(s?) which can be executed is MPW tools. An AppleEvent is sent to ToolServer which in turn executes the tool. A limitation is that ToolServer is not reentrant, so if a program which is called via Dos.Exec, in turn call Dos.Exec, it will be deadlocked. 

### Assembler symbols

Internal symbols in the generated assembler files will have a lowercase "s" as part separator instead of "$". The reason is that PPCAsm does not support $. 

### QuickDraw globals

There is a variable **qd** defined in System.pp for use as the QuickDraw global. 

For `{$APPTYE TOOL}` and `{$APPTYPE CONSOLE}` it is initialized, but for `{$APPTYPE GUI}` you have to initialize it yourself. 

## Debugging

See [MPW debugging](<MPW_debugging.md> "MPW debugging")

## Global variables

Currently all global variables are indirect, i.e. the entry in the TOC is always a pointer to data. (In the future small data items may be stored directly in the TOC) 

Thus all references to globals are via a construct like: `lwz rX, yyy[TC](r2) ;loads a pointer to a global into rX`

## Cross compiling from Mac OS X to Mac OS

Although not necessary nowadays when there exists a native compiler for target MacOS, here is some info on how to cross compile. See also [Link on target](<Link_on_target.md> "Link on target"). 

### Crosscompiling step on macOS

When compiling, add these options: 
    
    
     -Tmacos # target macos
     -st	  # do not generate executable, link on target
    

If make is used, add `OPT=-st OS_TARGET=macos`

Note that one must first build the RTL by issuing `make` in rtl/macos, with the above options, to be able to build other programs. The RTL must then be assembled and linked (see below) before compiling any program, otherwise the search paths in the link script will not be correct. 

You might also want to add option -a, then the link script will not delete unneeded files, in particular the assembler files (*.s). This can be useful if there are problems with the link scripts and you want to rerun it. 

### Assembling and linking step on Mac OS Classic

In case that host and target machine is different, transfer the produced files (assembler files (*.s) and link script *_ppas) to the host. 

In Mac OS Classic, use MPW to assemble and link the output from FreePascal by executing the link script (which is an MPW script), with its directory as current directory. Unfortunately the link script does not(?) obtain the proper mac file type, so this has to be fixed first e.g. with the MPW command SetFile. 

Example: To build Hello World (together with the rtl unit system.pp), execute: 
    
    
     Directory HD:Projekt:FreePascal:fpc:rtl:macos
     SetFile -c 'MPS ' -t 'TEXT' system_ppas
     system_ppas	#executes the script
     Directory HD:Projekt:FreePascal:fpc:demo:text
     SetFile -c 'MPS ' -t 'TEXT' hello_ppas
     hello_ppas	#executes the script
    

* * *

_Above circa 2004/5._

### Working notes: situation as of late 2012

The objective here is to build PPC and possibly 68K compilers, first as cross-compilers to run on a PC and then to run natively on Mac OS 9. Part of the incentive for this is to investigate whether a PPC Mac, e.g. my (MarkMLl) G3 beige with "Old World" ROMs, is a usable testbed for the fixed 68K compiler which Sven has added to trunk at around 2.8. 

On e.g. Debian Linux, build and install cross-binutils: 
    
    
    tar xjf binutils-2.20.1.tar.bz2
    cd binutils-2.20.1
    ./configure --prefix=/usr/local/powerpc-linux powerpc-linux-gnu
    make
    sudo make install
    sudo ln -s /usr/local/powerpc-linux/bin/as /usr/local/bin/powerpc-linux-as
    sudo ln -s /usr/local/powerpc-linux/bin/ld /usr/local/bin/powerpc-linux-ld
    sudo ln -s /usr/local/powerpc-linux/bin/ar /usr/local/bin/powerpc-linux-ar
    sudo ln -s /usr/local/powerpc-linux/bin/objdump /usr/local/bin/powerpc-linux-objdump
    sudo ln -s /usr/local/powerpc-linux/bin/objcopy /usr/local/bin/powerpc-linux-objcopy
    sudo ln -s /usr/local/powerpc-linux/bin/strip /usr/local/bin/powerpc-linux-strip
    powerpc-linux-ld -V
    

Do the same for m68k-linux-gnu. 

Build a cross-compiler to run on a PC but targeting PPC: 
    
    
    cd /usr/local/src/fpc/fpc-trunk
    export PP=
    make "OPT=-O- -g" -C compiler powerpc
    mv compiler/ppcppc compiler/ppcXppc
    

Renaming the compiler prevents it from being deleted by make clean etc., consider extending that OPT setting with -dEXTDEBUG. 

Build a native RTL and compiler: 
    
    
    export PP=/usr/local/src/fpc/fpc-trunk/compiler/ppcXppc
    make CPU_TARGET=powerpc OS_TARGET=macos "CROSSOPT=-st" rtl
    make CPU_TARGET=powerpc OS_TARGET=macos "CROSSOPT=-st" compiler
    

Bringing macos/sysdir.inc up to date shows that the amiga, embedded, watcom and symbian OS targets are similarly falling behind. It's instructive to compare the amiga target (which has not been updated) with the morphos target (which has). 

There's also a problem inside the compiler itself where entries of type AT_NONE are being generated but not handled. 

To be continued.

---

_Source: [https://wiki.freepascal.org/Target_MacOS](https://web.archive.org/web/20230207071608/https://wiki.freepascal.org/Target_MacOS)_
