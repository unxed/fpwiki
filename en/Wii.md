# Wii

_This is a work in progress porting. No download available yet._

## Contents

  * 1 Wii specs
  * 2 Status
  * 3 How to build the compiler from SVN
    * 3.1 Pre requisites
      * 3.1.1 fpc sources from SVN trunk
      * 3.1.2 devkitPPC
      * 3.1.3 fpc binaries
    * 3.2 Updating fpcmake
    * 3.3 Building the compiler
    * 3.4 Building the runtime library
    * 3.5 Adding fpc.cfg file
    * 3.6 Building libogcfpc
  * 4 Building and running the examples



## Wii specs

_(from Wikipedia)_

**Processors**

  * **CPU** : PowerPC-based '[Broadway](<http://en.wikipedia.org/wiki/Broadway_\(microprocessor\)>)' processor, made with a 90 nm SOI CMOS process, clocked at 729 MHz
  * **GPU** : ATI '[Hollywood](<http://en.wikipedia.org/wiki/Hollywood_\(graphics_chip\)>)' GPU made with a 90 nm CMOS process, clocked at 243 MHz 
    * **Boot CPU** : 'Starlet', part of the Hollywood package, an [ARM926EJ-S](<http://en.wikipedia.org/wiki/ARM9>) processor clocked at 243 MHz. It handles I/O and security; it is set to Big endian for compatibility with the Broadway core and it supports ARM and thumb instruction set.



**Memory**

  * 88 MB of main memory (24 MB of 'internal' [1T-SRAM](<http://en.wikipedia.org/wiki/1T-SRAM>), integrated into graphics package; 64 MB of 'external' [GDDR3](<http://en.wikipedia.org/wiki/GDDR3>) SDRAM)
  * 3 MB of embedded memory for GPU texture and framebuffer.



**Storage**

  * 512 MB built-in NAND flash memory
  * Expanded storage via SD and SDHC card memory (up to 32 GB)
  * Nintendo GameCube Memory Card (required for GameCube game saves)
  * Slot-loading disc drive compatible with 8 cm Nintendo GameCube Game Disc and 12 cm Wii Optical Disc
  * Mask ROM by Macronix



**Video Resolution**

  * 480p (PAL/NTSC), 480i (PAL/NTSC) or 576i (PAL/SECAM), standard 4:3 and 16:9 anamorphic widescreen



**Audio**

  * Main: Stereo – Dolby Pro Logic II-capable
  * Controller: Built-in speaker



  


## Status

Compiler and RTL are working. libogc library needs some work here and there, but it seems working fine too. 

  


## How to build the compiler from SVN

### Pre requisites

#### fpc sources from SVN trunk

Get fpc sources from SVN trunk repository: 
    
    
     svn checkout <http://svn.freepascal.org/svn/fpc/trunk> fpc
    

#### devkitPPC

Download the [devkitPro updater](<http://sourceforge.net/projects/devkitpro/>), then execute it to install all devkitPPC related stuff. In case, there is a [wiki page](<http://devkitpro.org/wiki/Getting_Started>) that explains how to manually install things. 

#### fpc binaries

Install the last stable release. 

### Updating fpcmake

The fpcmake shipped with the stable release needs to be updated in order to handle powerpc-wii target. Go to trunk/utils/fpcm folder and do 
    
    
     make clean all
    

You will get a new fpcmake.exe, that should replace the one shipped with the fpc stable release, e.g. in c:/fpc/bin/i386-win32. 

  


### Building the compiler

Make a .bat file (e.g. build_compiler.bat) in a directory of your choice, replacing the paths accordingly to yours: 
    
    
    :: build_compiler.bat
    @ECHO off
    
    :: FPC installation directory
    SET FPC_INST_DIR=C:\fpc4wii
    :: FPC sources directory
    SET FPC_SRC_DIR=C:\fpc_sources_from_trunk
    :: Binutils directory
    SET BINUTILS_DIR=C:\devkitPro\devkitPPC\bin
    :: Binutils prefix
    SET TOOL_PREFIX=powerpc-eabi-
    
    SET PATH=%BINUTILS_DIR%;%PATH%
    
    CD /D %FPC_SRC_DIR%\compiler
    
    :: Recreate makefile...
    fpcmake -r -w -Tall
    
    :: Cleaning...
    make distclean
    
    :: Building the compiler...
    make PPC_TARGET=powerpc
    
    pause
    
    :: Making install directories and copying files...
    MKDIR %FPC_INST_DIR%\bin\powerpc-wii
    :: copying the compiler...
    COPY /Y %FPC_SRC_DIR%\compiler\ppcppc.exe %FPC_INST_DIR%\bin\powerpc-wii\ppcppc.exe
    :: copying fpc utils...
    XCOPY /Y %FPC_SRC_DIR%\compiler\utils\*.exe %FPC_INST_DIR%\bin\powerpc-wii\
    :: copying binutils...
    XCOPY /Y %BINUTILS_DIR%\*.* %FPC_INST_DIR%\bin\powerpc-wii\
    :: copying messages...
    MKDIR %FPC_INST_DIR%\msg
    XCOPY /Y %FPC_SRC_DIR%\compiler\msg\*.* %FPC_INST_DIR%\msg\
    
    :: Done!
    pause
    
    

Now execute it to build and install the compiler. 

### Building the runtime library

Make another .bat file (e.g. build_rtl.bat) and put in a directory of your choice, replacing the paths accordingly to yours: 
    
    
    :: build_rtl.bat
    @ECHO off
    
    :: FPC installation directory
    SET FPC_INST_DIR=C:\fpc4wii
    
    :: FPC sources directory
    SET FPC_SRC_DIR=C:\fpc_sources_from_trunk
    
    :: Binutils directory
    SET BINUTILS_DIR=C:\devkitPro\devkitPPC\bin
    
    :: Binutils prefix
    SET TOOL_PREFIX=powerpc-eabi-
    
    SET PATH=%BINUTILS_DIR%;%PATH%
    
    CD %FPC_SRC_DIR%\rtl\wii
    
    fpcmake -r -w -Tall
    
    make clean
    make CPU_TARGET=powerpc OS_TARGET=wii PP="%FPC_INST_DIR%\bin\powerpc-wii\ppcppc.exe" OPT="-Twii -CX -XX -O2" BINUTILSPREFIX=%TOOL_PREFIX%
    MKDIR %FPC_INST_DIR%\units\powerpc-wii
    XCOPY /Y %FPC_SRC_DIR%\rtl\units\powerpc-wii\*.* %FPC_INST_DIR%\units\powerpc-wii\
    
    :: Create Package.fpc...
    fpcmake -pv -Twii
    COPY /Y %FPC_SRC_DIR%\rtl\wii\Package.fpc %FPC_INST_DIR%\units\powerpc-wii\Package.fpc
    
    :: Done!
    pause
    
    

Execute it to build and install the RTL. 

### Adding fpc.cfg file

Go to the new binaries directory (e.g. C:\fpc4wii\bin\powerpc-wii), make a new fpc.cfg file and edit it: 
    
    
    -Twii 
    -XPpowerpc-eabi- 
    -FuC:\fpc4wii\units\powerpc-wii 
    -FuC:\fpc4wii\units\powerpc-wii\libogcfpc 
    -FuC:\fpc4wii\units\powerpc-wii\mad 
    -FDC:\fpc4wii\bin\powerpc-wii
    -FlC:\devkitPro\libogc\lib\wii 
    -FlC:\devkitPro\devkitPPC\lib\gcc\powerpc-eabi\4.5.1 
    -FlC:\devkitPro\devkitPPC\powerpc-eabi\lib 
    

  


### Building libogcfpc

Make a last .bat file (e.g. build_libs.bat) and put in a directory of your choice, replacing the paths accordingly to yours: 

  

    
    
    :: build_libs.bat
    @ECHO off
    
    :: FPC installation directory
    SET FPC_INST_DIR=C:\fpc4wii
    :: FPC sources directory
    SET FPC_SRC_DIR=C:\fpc_sources_from_trunk
    :: Binutils directory
    SET BINUTILS_DIR=C:\devkitPro\devkitPPC\bin
    :: Binutils prefix
    SET TOOL_PREFIX=powerpc-eabi-
    
    SET PATH=%PATH%;%FPC_INST_DIR%\bin\powerpc-wii
    
    :: libogcfpc requires "mad" package, so move to trunk/packages/mad and build it
    CD %FPC_SRC_DIR%\packages\mad
    
    :: Recreate makefile...
    fpcmake -r -w -Twii
     
    :: Cleaning...
    make distclean all OS_TARGET=wii CPU_TARGET=powerpc BINUTILSPREFIX=powerpc-eabi- PP=ppcppc.exe 
    MKDIR %FPC_INST_DIR%\units\powerpc-wii\mad
    XCOPY /Y %FPC_SRC_DIR%\packages\mad\units\powerpc-wii\*.* %FPC_INST_DIR%\units\powerpc-wii\mad
    
    :: Create Package.fpc...
    fpcmake -pv -Twii
    COPY /Y %FPC_SRC_DIR%\packages\mad\Package.fpc %FPC_INST_DIR%\units\powerpc-wii\mad\Package.fpc
    
    pause
    
    :: now build libogcfpc...
    CD %FPC_SRC_DIR%\packages\libogcfpc
    
    :: Recreate makefile...
    fpcmake -r -w -Twii
    
    :: Cleaning...
    make distclean all OS_TARGET=wii CPU_TARGET=powerpc BINUTILSPREFIX=powerpc-eabi- PP=ppcppc.exe 
    MKDIR %FPC_INST_DIR%\units\powerpc-wii\libogcfpc
    XCOPY /Y %FPC_SRC_DIR%\packages\libogcfpc\units\powerpc-wii\*.* %FPC_INST_DIR%\units\powerpc-wii\libogcfpc
    
    :: Create Package.fpc...
    fpcmake -pv -Twii
    COPY /Y %FPC_SRC_DIR%\packages\libogcfpc\Package.fpc %FPC_INST_DIR%\units\powerpc-wii\libogcfpc\Package.fpc
    
    :: Done!
    pause
    

Execute it to build and install the required packages. 

  


## Building and running the examples

The examples can be built by calling 
    
    
     SET PATH=%PATH%;%FPC_INST_DIR%\bin\powerpc-wii
     make OS_TARGET=wii CPU_TARGET=powerpc BINUTILSPREFIX=powerpc-eabi- PP=ppcppc.exe 
    

where %FPC_INST_DIR% is the directory where the powerpc crosscompiler is installed. 

The examples are running fine on a real Wii with the [Homebrew Channel](<http://wiibrew.org/wiki/Homebrew_Channel>) installed. As alternative, you can use the [Dolphin emulator](<http://dolphin-emu.org/>). In this case, please read carefully the [notes](<http://wiibrew.org/wiki/Dolphin#Compatibility>) on <http://wiibrew.org>

---

_Source: [https://wiki.freepascal.org/Wii](https://web.archive.org/web/20241212100718/https://wiki.freepascal.org/Wii)_
