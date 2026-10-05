# Cross compiling

│ **English (en)** │  **[русский (ru)](<../ru/Cross_compiling.md>)** │

## Contents

  * 1 Introduction
    * 1.1 Host and target on the same CPU
    * 1.2 Host and target on different CPUs
    * 1.3 Supported targets
    * 1.4 Assembler and linker
    * 1.5 Units for target
    * 1.6 Configuration
    * 1.7 Basic Steps
  * 2 From Linux
    * 2.1 From Linux x64 to Linux i386
      * 2.1.1 Install the 32bit libraries
      * 2.1.2 Create ld and as files
      * 2.1.3 Compile The Cross Compiler
      * 2.1.4 Testing
    * 2.2 From Linux to ARM Linux
      * 2.2.1 Maemo 5 OS
      * 2.2.2 Raspberry Pi
    * 2.3 From Linux to Windows
    * 2.4 From Linux to macOS
  * 3 From Windows
    * 3.1 From Windows to Android
    * 3.2 From Windows to Linux
    * 3.3 From Windows to GO32v2
    * 3.4 From Windows to winCE
    * 3.5 From win32 to win64
    * 3.6 From win64 to win32
    * 3.7 Lazarus guide to cross-compile Win 32/64-bit
  * 4 From macOS (Darwin)
    * 4.1 From macOS to Win32
    * 4.2 From macOS to Win64
    * 4.3 From macOS i386 to PowerPC
    * 4.4 From macOS i386 to x86_64
    * 4.5 From macOS x86_64 to i386
    * 4.6 From macOS to any using fink
  * 5 From FreeBSD
    * 5.1 FreeBSD to SPARC
  * 6 From within Docker container
  * 7 General Unix/Linux notes
  * 8 Cross compiling the LCL
    * 8.1 Test cross compiler
    * 8.2 Cross compiling the LCL in Lazarus 0.9.30 and below
    * 8.3 Command line
  * 9 Cross compiling LCL applications
  * 10 Cross compile FAQ
    * 10.1 Why cross compile?
    * 10.2 Why not to cross compile?
    * 10.3 Why cross compile from Unix to Windows and not the other way around?
    * 10.4 I want more information on building Free Pascal. Where is it?
    * 10.5 Errors like compiler "/usr/bin/fpc" does not support target arm-linux
  * 11 References



## Introduction

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Before going any further, consider [Why not to cross compile?](<Cross_compiling.md> "Cross compiling") below

This is a short introduction for users new to cross compiling. The following sections describe how to setup a system to cross compile, that means creating binaries (executables) for a platform different from the one used for compilation - e.g. working under Linux and creating Win32 executables (or those for FreeBSD or Darwin, etc.). In this case, the platform used for compilation is usually referred to as "host" (Linux in the example above) and the platform where you want to run your created binaries is your "target". 

Free Pascal is a compiler and basically converts source into binaries (machine language). These binaries also contain information on how the operating system starts the executables. Moreover, the binaries refer to the APIs provided by the particular operating system, that's why a different implementation of our Run-Time Library is necessary for different operating systems. Therefore these binaries are platform specific. Free Pascal itself does not need much setup. It can create binaries for many platforms. Just tell it to do so. 

### Host and target on the same CPU

FPC is designed so that the distributed compiler can create machine code for a certain CPU (because different CPUs need different machine code) and it knows specific requirements for all supported platforms (operating systems) available on that particular CPU. This means that you can perform cross-compilation with the same compiler used for native compilation as long as you stick to the same CPU. 

### Host and target on different CPUs

If you need to create binaries for a different CPU, you need a special cross-compiler, i.e. compiler running on the host platform, but able to create machine code for a different CPU (in the case of FPC, such a cross-compiler would be again able to target all supported platforms available on the _target_ CPU). This cross-compiler is then usually stored in the same directory as the native compiler. Such a cross-compiler may be either compiled by yourself, or you can use a ready made distributed cross-compiler provided for some platforms directly by the FPC team (usually platforms mostly used in portable devices like arm-linux or arm-wince, because these are usually not used as host platforms). FPC binary can then select the right compiler (either the native compiler or the cross-compiler) for the target CPU selected using the -P parameter. 

### Supported targets

See the [Supported target operating systems](<Free_Pascal_supported_targets.md> "Free Pascal supported targets") article for the full list. 

### Assembler and linker

The compiler is only one part. We also need the assembler and the linker. FPC provides an internal assembler and/or linker for some platforms, but other platforms need external tools. Usually these tools are not able to create binaries for different platforms. That's why we have to use different special linker 'ld' and assembler 'as' for every target platform. These are the binutils. 

See [Binutils](<Binutils.md> "Binutils"). 

### Units for target

After creating (or having/installing) the cross tools, one needs FPC RTL and other units compiled for the chosen target platform. For example, every target platform needs a different file system.ppu (System unit), etc. These units may be either compiled using your compiler set up for compilation to the target platform, or you may potentially use officially distributed units compiled (and distributed) with exactly the same FPC version (if available in format usable under the particular host platform). 

Its worth noting that you don't necessarily need a new cross compiler for every CPU-OS combination, but you do need the FPC Units. For example, on a Linux host setup to cross compile to Windows and Raspberry Pi, the /usr/lib/fpc/3.2.2/units directory may contain the following directories- 
    
    
    arm-linux
    i386-linux
    i386-win32
    x86_64-linux
    x86_64-win64
    

Each of those directories will contain a directory for each of the FPC units, each one with .o and .ppu files. 

### Configuration

Then your FPC config file will be setup, so that cross compilation becomes so easy, that you can forget all the boring details. The same will be done for the LCL - the Lazarus Component Library (if using Lazarus). And after this you can cross compile Pascal programs for the (different) target platform. The resulting binaries may then be copied to a machine running the target platform, or run under an emulator (e.g. Wine for Win32 binaries under Linux, etc.). 

### Basic Steps

There are a few common steps involved in cross compiling that you must do in every case: 

  * Have already a Free Pascal compiler for the platform you wish to compile from.
  * You need to have the Free Pascal source code (except for the special case of having everything prepared by someone else).
  * You need to either build from source or obtain binaries of the cross-binutils (See [Binutils](<Binutils.md> "Binutils")) that run on the platform you are on and are designed to build programs for your desired target platform.
  * Sometimes you will need some files from the target you are compiling to.



## From Linux

### From Linux x64 to Linux i386

Chances are that your 64 bit Linux distribution is already capable of compiling 32 bit programs but due to the way the FPC build process is designed there are a couple of things you might have to do. 

#### Install the 32bit libraries

On a 64bit Debian/Ubuntu you need to install a number of 32bit libraries, first enable the architecture as shown below. Note that just installing the particular :i386 packages (without enabling the architecture) is no longer reliable, see <https://wiki.debian.org/Multiarch/HOWTO> . Other distributions will require a similar install. 
    
    
    sudo dpkg --add-architecture i386
    sudo apt update
    sudo apt install libc6-dev-i386  libx11-6:i386  libx11-dev:i386  libgdk-pixbuf2.0-0:i386  libatk1.0-dev:i386 libgdk-pixbuf2.0-dev:i386  libpango1.0-dev:i386  libgtk2.0-dev:i386 libnotify-dev:i386
    

Note how they have :i386 appened to the names, thats to tell apt to get the 32bit ones on a 64bit system. If you later get missing library error messages when cross compiling 32bit to 64bit, don't forget to add the :i386 to the names when installing what is needed. 

This will typically put those 32bit libraries in /usr/lib/i386-linux-gnu/. You might need to check they are there and ensure your fpc.cfg knows where they are. Further, to ensure you find the crt* libraries, you may need to add (or, more likely, edit) something like - 
    
    
    # path to the gcclib, 
    #ifdef cpui386
    -Fl/usr/lib/gcc/x86_64-linux-gnu/9/32
    #endif
    

Check that location first, the '9' varies with your distribution's age. In Bullseye for example, its /usr/lib/gcc/x86_64-linux-gnu/10/32 

#### Create ld and as files

Check if you already have the files i386-linux-ld and i386-linux-as by doing the following commands: 
    
    
    $ which i386-linux-ld
    $ which i386-linux-as
    

If you get nothing, it means you need to create them. If you have these files skip to the **Compile FPC** (see below). 

Those are executable files. They must be in you PATH, if you are installing the compiler in root space, then probably /usr/bin but if you are using a compiler in user space (ie you used a FPC Tarball) then put them in the same user directory your other FPC compiler tools live in. We will assume /usr/bin here. **/usr/bin/i386-linux-ld** must contain: 
    
    
    #!/bin/bash
    ld -A elf32-i386 $@
    

**/usr/bin/i386-linux-as** must contain: 
    
    
    #!/bin/bash
    as --32 $@
    

Both can be created using following commands: 
    
    
    # Create /usr/bin/i386-linux-ld
    $ cat >/usr/bin/i386-linux-ld << EOF
    #!/bin/bash
    ld -A elf32-i386 \$@
    EOF
    $ chmod +x /usr/bin/i386-linux-ld
    
    # Create /usr/bin/i386-linux-as
    $ cat >/usr/bin/i386-linux-as << EOF
    #!/bin/bash
    as --32 \$@
    EOF
    $ chmod +x /usr/bin/i386-linux-as
    

#### Compile The Cross Compiler

You may be installing a cross compiler to suit either a root installed existing compiler (perhaps installed with a .deb) or your existing compiler was installed using a Tarball (an excellent idea incidentally). Further, your system may like to keep such things in /usr or, perhaps /usr/local, so adjust accordingly. Firstly, if installing in root space - 
    
    
     
    $ cd /usr/share/fpcsrc/<version>
    $ sudo make all OS_TARGET=linux CPU_TARGET=i386
    $ sudo make crossinstall OS_TARGET=linux CPU_TARGET=i386 INSTALL_PREFIX=/usr
    $ ls -la /usr/lib/fpc/<version>/ppcross386
    

If the last command there shows you a nice file called ppcross386, great, proceed to the next command 
    
    
    $ sudo ln -s /usr/lib/fpc/<version>/ppcross386  /usr/bin/ppcross386
    

If, however, **that file is not there** , its probably in /usr/share/fpcsrc/<version>/compiler/ppcross386, check there and if it is, use the next command - 
    
    
    $ sudo ln -s  /usr/share/fpcsrc/<version>/compiler/ppcross386  /usr/bin/ppcross386
    
    

On the other hand perhaps you are installing in user space and assuming your existing compiler is in in $HOME/bin/FPC you might do - 
    
    
     
    $ cd $HOME/bin/FPC/SRC/fpc-3.2.0
    $ make all CPU_TARGET=i386
    $ make crossinstall CPU_TARGET=i386 INSTALL_PREFIX=$HOME/bin/FPC
    $ ln -s $HOME/bin/FPC/lib/fpc/3.2.0/ppcross386 $HOME/bin/FPC/bin/ppcross386
    

  
Thats it ! You may need to edit your fpc.cfg (it lives in either /etc/fpc.cfg or $HOME/.fpc.cfg). 

Note that if you see messages about failing to find files such as crtn.o and predicted failures to link, its probably because your Linux distro likes to keep its 32bit gcc libraries in /usr/lib32. Check if the missing files are in fact, in /usr/lib32 and, if so, make a small change to your (eg) /etc/fpc.cfg file, around line #177 there is a section that looks like this - 
    
    
    #ifdef cpui386
    -Fl/usr/lib/gcc/x86_64-linux-gnu/9/32
    #endif
    

Make it look like this, one additional line - 
    
    
    #ifdef cpui386
    -Fl/usr/lib/gcc/x86_64-linux-gnu/9/32
    -Fl/usr/lib32
    #endif
    

Again, the above says /usr/lib/gcc/x86_64-linux-gnu/**9** /32, the '9' will change over time, on Bullseye its '10'. 

#### Testing

Before you proceed any further, its a good idea to test what you have done so far. 

With a simple hello.pas file 
    
    
    $> fpc -Pi386 hello.pas <enter>
    $> file hello <enter>
    

### From Linux to ARM Linux

#### Maemo 5 OS

Information about targeting Linux running on ARM (e.g. Zaurus) may be found in [Setup Cross Compile For ARM](<Setup_Cross_Compile_For_ARM.md> "Setup Cross Compile For ARM"). 

#### Raspberry Pi

Information about targeting Linux running on a Raspberry Pi may be found in [Cross Compile to RasPi from Linux](<Cross_Compile_to_RasPi_from_Linux.md> "Cross Compile to RasPi from Linux"). 

### From Linux to Windows

Information on cross-compilation with Lazarus can be found in [Cross compiling for Windows under Linux](<Cross_compiling_for_Windows_under_Linux.md> "Cross compiling for Windows under Linux")

### From Linux to macOS

Please see [Cross compiling macOS on Linux](<Cross_compiling_OSX_on_Linux.md> "Cross compiling OSX on Linux"). 

## From Windows

### From Windows to Android

For a tutorial on how to create Android apps with Lazarus on Windows, see [Android tutorial](<Android_tutorial.md> "Android tutorial"). 

### From Windows to Linux

This is less trivial, there is some info in the [buildfaq](<buildfaq.md> "buildfaq")

See also [fpcup](<fpcup.md> "fpcup") for descriptions on which binutils work and what libraries/files to copy. 

As the Build FAQ explains, you will need libs (.so files) from the target system, e.g. from /lib and /user/lib (but could be more locations). On some systems, some .so files are actually scripts; check with: 
    
    
    $ grep -i "ld script" *
    

Remove those .so and copy over (or symlink) the .so.x files that you should have to .so in order for the linker to find them. 

### From Windows to GO32v2

Detailed information may be found in [Cross-compilation from Win32 to GO32v2](<Cross-compilation_from_Win32_to_GO32v2.md> "Cross-compilation from Win32 to GO32v2"). 

### From Windows to winCE

  * [arm-wince](<arm-wince.md> "arm-wince") describes how to setup a crosscompiler for arm CPU
  * [i386-wince](<i386-wince.md> "i386-wince") describes how to setup compiling for i386 CPU (no crosscompiling)



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Lazarus installers have an installer that adds Windows to WinCE cross compilation options automatically

### From win32 to win64

If you are compiling the 2.1.1 or greater branch of fpc you can just do: 
    
    
    $ make all OS_TARGET=win64 CPU_TARGET=x86_64
    

and then 
    
    
    $ make crossinstall OS_TARGET=win64 CPU_TARGET=x86_64
    

### From win64 to win32

Assuming that Lazarus 2.0.10 was previously installed, now install 
    
    
    **lazarus-2.0.10-fpc-3.2.0-cross-i386-win32-win64.exe** 
    

in the same base directory and then setup your project options to: 

  * Target OS : Win32
  * Target CPU : i386



Failing to set the CPU (Default) lead to incorrect win32-x86_64 platform! 

### Lazarus guide to cross-compile Win 32/64-bit

Download the installers from sourceforge.net (click on the Files tab): 

**<https://sourceforge.net/projects/lazarus/>**

The installers include FPC and the Lazarus help files _(as of Oct. 20, 2019)_ : 

  * Lazarus 32-bit 
    * lazarus-2.0.4-fpc-3.0.4-win32.exe
    * lazarus-2.0.4-fpc-3.0.4-cross-x86_64-win64-win32.exe


  * Lazarus 64-bit 
    * lazarus-2.0.4-fpc-3.0.4-win64.exe
    * lazarus-2.0.4-fpc-3.0.4-cross-i386-win32-win64.exe



To install Lazarus 32-bit, and cross-compile a Windows 64-bit exe: 

  1. Install the win32.exe
  2. Install the cross-x86_64-win64-win32.exe in the same folder



To install Lazarus 64-bit, and cross-compile a Windows 32-bit exe: 

  1. Install the win64.exe
  2. Install the cross-i386-win32-win64.exe in the same folder



  
(The following assumes Lazarus 64-bit with cross-i386-win32-win64) 

To cross-compile 32-bit or 64-bit without creating Build Modes: 
    
    
      Project, Options, Compiler Options, Config and Target:
         Target OS         =Default=Win64  or Win32 for 32-bit
         Target CPU family =Default=x86_64 or i386  for 32-bit
         Target processor  =Default
      Press OK to exit Project Options
      Build and run the project
      Use Windows Task Manager, Details tab, to see if Project exe is 32/64-bit
      
    

To Configure Build Modes: 
    
    
      Project, Options, Compiler Options, Config and Target:
         Check 'Build Modes', then press the [...] button
            Press [Create Debug and Release Modes] button
            Press OK
            
      Create a Debug 32-bit Build Mode:
         Lazarus menu: Project, Options, Compiler Options, Config and Target
         Select Build Mode = Debug
         Press the Build Modes [...] button
            Click to select Debug on the list
            Press the [ + ] button to add a new build mode
            Rename to 'Debug 32-bit', press OK
         Set Target Platform to 32-bit:
            Target OS         =Win32
            Target CPU family =i386
            Target processor  =Default
         Press OK to exit Project Options and save Build Mode 'Debug 32-bit'
         
      Create a Debug 64-bit Release Mode:
         Lazarus menu: Project, Options, Compiler Options, Config and Target
         Select Build Mode = Release
         Press the Build Modes [...] button
            Click to select Release on the list
            Press the [ + ] button to add a new build mode
            Rename to 'Release 32-bit', press OK
         Set Target Platform to 32-bit:
            Target OS         =Win32
            Target CPU family =i386
            Target processor  =Default
         Press OK to exit Project Options and save Build Mode 'Release 32-bit'      
      Build and run the project
      Use Windows Task Manager, Details tab, to see if Project exe is 32/64-bit
    

To use Build Modes: 
    
    
      Lazarus IDE Menu: Change Build Mode dropdown (left of Run button)
      Build and run the project
      Use Windows Task Manager, Details tab, to see if Project exe is 32/64-bit
    

## From macOS (Darwin)

You must have the Free Pascal Compiler (FPC) source code already installed. Recent FPC dmg downloads store the source code on macOS in the `/usr/local/share/fpcsrc` directory. 

### From macOS to Win32

![<translate> Warning: </translate>](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/OOjs_UI_icon_notice-destructive.svg/18px-OOjs_UI_icon_notice-destructive.svg.png) **Warning**|  32 bit software can only be compiled up to and including macOS 10.14 (Mojave). Apple removed the 32 bit frameworks from macOS 10.15 (Catalina) in 2019 - that operating system and future versions will no longer compile or run any 32 bit software.  
---|---  
  
To be able to compile your Lazarus/Pascal source code on macOS for Windows 32 bit, compile and install the Windows 32 bit cross compiler. To do this you need a bootstrap i386 compiler (ppc386). A Free Pascal Compiler release can always be built by the previously released FPC version, and FPC trunk can always be built by the currently released FPC version. So, as of July 2020: 

  * to build FPC v3.2.0, you need an FPC v3.0.4 ppc386 i386 bootstrap compiler;
  * to build trunk (v3.3.1) you need an FPC v3.2.0 ppc386 i386 bootstrap compiler.



Once you have a bootstrap i386 compiler, then to use that compiler you pass it on the command line with _FPC=_. For example, to build the trunk (v3.3.1) FPC version cross compiler for Win32: 
    
    
    # Open a Terminal and navigate to the FPC source code directory.
    $ cd /usr/local/share/fpcsrc/
    
    
    
    # Compile the FPC trunk cross compiler using the released FPC 3.2.0 compiler.
    $ sudo make clean all FPC=/usr/local/lib/fpc/3.2.0/ppc386 OS_TARGET=win32 CPU_SOURCE=i386
    
    
    
    # Install the cross compiler.
    $ sudo make crossinstall OS_TARGET=win32 CPU_TARGET=i386
    
    
    
    # Soft link the cross compiler to the bin directory with the other compiler binaries.
    $ sudo ln -s /usr/local/lib/fpc/3.3.1/ppcross386 /usr/local/bin/ppcross386
    

Now go to the Lazarus IDE and setup the Project Options and Tools Options as follows: 

  * Project Options 
    * Config and Target 
      * Target OS Win32
      * Target CPU i386
      * Target Processor (Default)
      * Target specific options [X] Win32 gui application
      * Current LCL widgetset: "win32"


  * Tools 
    * Options 
      * Environment 
        * Files
        * Compiler executable: /usr/local/bin/ppcross386



Once you have setup the Project Options and Tools Options, Lazarus will now be able to cross compile Windows 32 bit applications on your macOS computer. The first time you compile a Windows 32 bit application, Lazarus will also recompile its component source code; so don't be alarmed, it will just take a little longer. 

To use the FPC compiler without Lazarus, I use the following shell script: 
    
    
    #!/bin/sh
    /usr/local/bin/ppcross386 -FUwin32 -FEwin32 -Twin32 ${1}
    

as follows: 
    
    
    win32 program.pas
    

This will create the 32 bit Windows executable in the win32 subdirectory to keep it separate from macOS and Win64 executables. If you do not want to use subdirectories, you can omit the _-FEwin32_ parameter from the script. 

### From macOS to Win64

To be able to compile your Lazarus/Pascal source code on macOS for Windows 64 bit on either an Intel or Apple Silicon Mac, compile and install the Windows 64 bit cross compiler. To do this you need a bootstrap x86_64 compiler (ppcx64). A Free Pascal Compiler release can always be built by the previously released FPC version, and FPC trunk can always be built by the currently released FPC version. So, as at May 2022: 

  * to build FPC v3.2.2, you need an FPC v3.2.0 ppcx64 x86_64 bootstrap compiler;
  * to build trunk (v3.3.1) you need an FPC v3.2.2 ppcx64 x86_64 bootstrap compiler.



Once you have a bootstrap x86_64 compiler, then to use that compiler you pass it on the command line with _FPC=_. For example, to build the trunk (v3.3.1) FPC version cross compiler for Win64: 
    
    
    # Open a Terminal and navigate to the FPC source code directory
    $ cd /usr/local/share/fpcsrc/
    
    
    
    # Compile the FPC trunk cross compiler using the released FPC 3.2.2 compiler.
    $ sudo make clean all OS_TARGET=win64 CPU_SOURCE=x86_64 FPC=/usr/local/lib/fpc/3.2.2/ppcx64 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk"
    
    
    
    # Install the cross compiler.
    $ sudo make crossinstall OS_TARGET=win64 CPU_TARGET=x86_64
    
    
    
    # Soft link the cross compiler to the bin directory with the other compiler binaries.
    $ sudo ln -s /usr/local/lib/fpc/3.3.1/ppcrossx64 /usr/local/bin/ppcrossx64
    

Now go to the Lazarus IDE and setup the Project Options and Tools Options as follows: 

  * Project Options 
    * Config and Target 
      * Target OS Win64
      * Target CPU x86_64
      * Target Processor (Default)
      * Target specific options [X] Win32 gui application [win32 is not a mistake]
      * Current LCL widgetset: "win32" [win32 is not a mistake]


  * Tools 
    * Options 
      * Environment 
        * Files
        * Compiler executable: /usr/local/bin/ppcrossx64



Once you have setup the Project Options and Tools Options, Lazarus will now be able to cross compile Windows 64 bit applications on your macOS computer. The first time you compile a Windows 64 bit application, Lazarus will also recompile its component source code; so don't be alarmed, it will just take a little longer. 

To use the FPC compiler without Lazarus, I use the following shell script located in `/usr/local/bin`: 
    
    
    #!/bin/sh
    /usr/local/bin/ppcrossx64 -FUwin64 -FEwin64 -Twin64 ${1}
    

as follows: 
    
    
    win64 program.pas
    

This will create the 64 bit Windows executable in the win64 subdirectory to keep it separate from macOS and Win32 executables. If you do not want to use subdirectories, you can omit the _-FEwin64_ parameter from the script. 

### From macOS i386 to PowerPC

The official FPC installer for macOS/i386 includes a PowerPC cross-compiler and all units necessary to compile PowerPC programs (use _ppcppc_ instead of _ppc386_ to compile your programs). The instructions below are only necessary if you want to compile and install a new version from svn. 

  * Compile FPC:


    
    
    $ cd fpc
    $ make all CPU_TARGET=powerpc -j 2
    

This creates the powerpc cross-compiler compiler (fpc/compiler/ppcrosspcc) and all units. You can install them using the following commands: 
    
    
    $ sudo make FPC=`pwd`/compiler/ppc386 install CPU_TARGET=powerpc CROSSINSTALL=1
    $ INSTALLED_VERSION=`./compiler/ppc386 -iV`
    $ sudo mv /usr/local/lib/fpc/$INSTALLED_VERSION/ppcrossppc /usr/local/lib/fpc/$INSTALLED_VERSION/ppcppc
    

Reminder: Universal binaries are created from the individual (i386 and powerpc) binaries using lipo. 

### From macOS i386 to x86_64

The official FPC installer for macOS/i386 includes an x86_64 compiler and all units necessary to compile x86_64 programs (use _ppcx64_ instead of _ppc386_ to compile your programs or use fpc -Px86_64). The instructions below are only necessary if you want to compile and install a new version from svn. 

  * Compile FPC:


    
    
    $ cd fpc
    $ make all CPU_TARGET=x86_64
    

This creates the x86_64 cross-compiler (fpc/compiler/ppcrossx64) and all units. You can install them using the following commands: 
    
    
    $ sudo make crossinstall CPU_TARGET=x86_64
    $ sudo mv /usr/local/lib/fpc/2.7.1/ppcrossx64 /usr/local/lib/fpc/2.7.1/ppcx64
    

If you want to make this newly installed compiler the default version (this is _**not**_ recommended, and will break your ability to build new FPC 2.7.1 versions without explicitly specifying the path to the latest official FPC release), also execute the following command: 
    
    
    $ sudo ln -sf /usr/local/lib/fpc/2.7.1/ppcx64 /usr/local/bin/ppcx64
    

Assuming all of the LCL components your project uses are supported by the [Cocoa widgetset](<http://wiki.lazarus.freepascal.org/Roadmap#Status_of_features_on_each_LCL_Interface>), you can compile a Lazarus project from the command line, using a command like this (full path to the compiler seems required): 
    
    
    $ lazbuild -B project1.lpr --ws=cocoa --cpu=x86_64 --os=darwin --compiler=/usr/local/bin/ppcx64
    

You can check that your executable is 64-bit uses the "file" command: 
    
    
    $ file ./project1
    $ ./project1: Mach-O 64-bit executable x86_64
    

Alternatively, you can set your project to compile as 64-bit using the Lazarus graphical interface: 

  * Choose the Project/ProjectOptions menu item
  * For CompilerOptions/ConfigAndTarget set the "Target CPU family" to "x86_64"
  * For CompilerOptions/AdditionsAndOverrides store the "LCLWidgetType := cocoa" in the LPI



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Before macOS 10.15 (Catalina), a 64-bit Mac computer could run 32-bit executables. With macOS 10.15 and higher, 32-bit executables can no longer run.

If you want to create a universal binary that executes on a 32-bit Intel as well as in 64-bit mode on a pre-Catalina 64-bit Intel computer you can use the "lipo" command. This would allow the 64-bit computer to access more memory and in theory might perform a bit better (e.g. different compiler optimizations, more registers though with larger pointers). Assuming you have generated separate 32-bit ("project32") and 64-bit ("project64") executables with Lazarus. 
    
    
    $ lipo -create project32 project64 -o projectUniversal
    

### From macOS x86_64 to i386

![<translate> Warning: </translate>](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/OOjs_UI_icon_notice-destructive.svg/18px-OOjs_UI_icon_notice-destructive.svg.png) **Warning**|  32 bit software can only be compiled up to and including macOS 10.14 (Mojave). Apple removed the 32 bit frameworks from macOS 10.15 (Catalina) in 2019 - that operating system and future versions will no longer compile or run any 32 bit software.  
---|---  
  
The official Free Pascal Compiler installer for macOS includes an i386 32 bit compiler and all units necessary to compile i386 programs (use ppc386 instead of ppcx64 to compile your programs or use fpc -Pi386). **The instructions below are only necessary if you want to compile and install a new version from the git repository.**

You need a bootstrap i386 compiler (ppc386). A Free Pascal Compiler release can always be built by the previously released FPC version, and FPC trunk can always be built by the current FPC version. So, as at May 2022: 

  * to build FPC v3.2.2, you need an FPC v3.2.0 ppc386 i386 bootstrap compiler;
  * to build trunk you need an FPC v3.2.2 ppc386 i386 bootstrap compiler.



Once you have a bootstrap i386 compiler, then to use that compiler you pass it on the command line with FPC= as below: 
    
    
    $ make FPC=/usr/local/lib/fpc/3.2.2/ppc386 distclean CPU_SOURCE=i386
    

And install with: 
    
    
    $ make install CPU_TARGET=i386
    

### From macOS to any using fink

The FPC package of the package manager [fink](<http://finkproject.org/>) has a 64-bit compiler for macOS and cross compilers to Windows, Linux, FreeBSD and others on various CPUs. 

Examples for installing a cross compiler are: 
    
    
    $ fink install fpc-cross-i386-win32
    

or 
    
    
    $ fink install fpc-cross-arm-linux
    

To compile use these commands: 
    
    
    fpc -Pi386 -Twin32 FILENAME
    fpc -Parm -Tlinux FILENAME
    

This command gives the list of cross compilers: 
    
    
    $ fink list fpc-cross
    

For other platforms (processors and systems) you have to do the setup by yourself. It is basically the same scheme: First, you need the corresponding binutils (see [Binutils](<Binutils.md> "Binutils")) and second, the cross compiler and the run time library. Some more details of the building procedure can be learned from the fink package description files of the cross compilers from above. 

## From FreeBSD

### FreeBSD to SPARC

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** This section appears to date from around 2005 and may not be relevant anymore. Updates are welcome.

I managed to crosscompile from x86 to Sparc Solaris 9. However the result doesn't work very well, but here is my command-line: 

in compiler/ execute: 
    
    
    $ gmake cycle CPU_TARGET=sparc OS_TARGET=solaris CROSSBINUTILPREFIX=solaris-sparc- CROSSOPT='-Xd -Fl~/src/sollib'
    

~/src/sollib is a directory that contains: 

  * a set of .o's from /usr/local/gcc-3.3-32bit/lib/gcc-lib/sparc-sun-solaris/3.3
  * libgcc.a from /usr/local/gcc-3.3-32bit/lib/gcc-lib/sparc-sun-solaris/3.3
  * a set of lib*.so from /usr/lib: libaio.so libmd5.so libc.so libelf.so librt.so libdl.so libm.so



Problem is illustrated by the following binary. 
    
    
     Free Pascal Compiler version 2.1.1 [2006/03/17] for sparc
     Copyright (c) 1993-2005 by Florian Klaempfl
     Target OS: Solaris for SPARC
     Compiling system.pp
     system.pp(15,1) Fatal: Syntax error, "BEGIN" expected but "identifier UNIT" found
    

I suspect wrong .o's are taken. 

## From within Docker container

Cross compiling hello world with Docker image and Free Pascal Compiler 3.0.4 

1\. Pull docker image: 
    
    
    $ sudo docker pull taraworks/lazarus-cross:0.0.2 # (do not use latest)
    

2\. Create container with virtual volume based on the pulled image: 

a. Identify image ID (first 4 digits are needed): 
    
    
    $ sudo docker images
    

b. Use the image for the container, map virtual volume from host: 
    
    
    $ sudo docker create --name pascalContainer -v /home/tudi/pascal_files:/home/tudi/pascal_files -ti image ID
    

3\. Start the container 

a. Identify container ID (first 4 digits are needed): 
    
    
    $ sudo docker ps -a
    

b. Start container: 
    
    
    $ sudo docker start container ID
    

4\. Compile file 

a. Copy Pascal file hello.pas from <https://github.com/taraworks/lazarus-cross> to folder /home/tudi/pascal_files on host 

b. Run on host: 
    
    
    $ cd /home/tudi/pascal_files
    
    
    
    $ git clone https://github.com/taraworks/lazarus-cross.git
    

b. Attach to container to check the cloned repo is presented: 

c. Run on host: 
    
    
    $ sudo docker attach container ID
    

Press 'Enter' one more time and check hello.pas is in container folder. Do not detach with exit from container as container will stop. 

c. Compile from container. Change directory to the folder containing hello.pas: 

For Windows: 
    
    
    PATH=$PATH:/opt/clang/bin:/opt/osxcross/target/bin /opt/windows/lib/fpc/3.0.4/ppcross386 -Twin32 -va hello.pas
    

For macOS: 
    
    
    PATH=$PATH:/opt/clang/bin:/opt/osxcross/target/bin /opt/darwin/lib/fpc/3.0.4/ppcross386 -Tdarwin -XR/opt/osxcross/target/SDK/MacOSX10.11.sdk -va hello.pas
    

d. Compile from host. 
    
    
    $ sudo docker exec pascalContainer bash -c "cd ${CI_PROJECT_DIR} && PATH=$PATH:/opt/clang/bin:/opt/osxcross/target/bin /opt/windows/lib/fpc/3.0.4/ppcross386 -Twin32 -va /home/tudi/pascal_files/lazarus-cross/hello.pas"
    

## General Unix/Linux notes

Option -XLA is used to rename library dependencies specified in Pascal units. Format is -XLAold=new, to modify ld link option -l<old> to -l<new>. 

Option -XR<sysroot> (recent trunk) that can be used to specify the target system root. It's used for: 

  * adding a prefix to the default added library paths; in the past you used to specify -Xd and these paths manually. E.g. for i386-linux instead of passing /lib, /usr/lib, and /usr/X11R6/lib to ld, it will pass <sysroot>/lib, <sysroot>/usr/lib, and <sysroot>/usr/X11R6/lib to ld.
  * detecting the C library (linux specific): glibc or uclibc. E.g. for uclibc detection '<sysroot>/lib/ld-uClibc.so.0' is tried.



## Cross compiling the LCL

Since 0.9.31 the LCL is a normal Lazarus package and the IDE will automatically cross compile all needed packages, when you change the target platform of your project. 

If something goes wrong, here are some hints that might help to find out why: 

### Test cross compiler

Test if you have installed the cross compiled FPC correctly: 

Create a hello world program test.pas: 
    
    
    program test;
    begin
      writeln('DATE ',{$i %DATE%});
      writeln('FPCTARGET ',{$i %FPCTARGET%});
      writeln('FPCTARGETCPU ',{$i %FPCTARGETCPU%});
      writeln('FPCTARGETOS ',{$i %FPCTARGETOS%});
      writeln('FPCVERSION ',{$i %FPCVERSION%});
    end.
    

And compile it with your source/original platform. Example for x86 Windows: 
    
    
    fpc -Twin32 -Pi386 test.pas
    

Then test source compiler: 
    
    
    test
    

Replace _win32_ and _i386_ with your targets. Example for target Windows 64 bit: 
    
    
    fpc -Twin64 -Px86_64 test.pas
    

Then test cross compiler: 
    
    
    test
    

The program **fpc** is a wrapper that searches the right compiler (eg ppcrossx64) for the target and executes it. 

If this does not work, your cross compiler was not installed correctly. When this works you can cross compile the LCL. 

### Cross compiling the LCL in Lazarus 0.9.30 and below

If you are sure your cross compiler works, you can do the actual cross compile. 

Perform the following steps in the Lazarus IDE to do an LCL cross compile: In older IDEs: 

  * Set in _Tools - > Options -> Environment -> Files_ the _Compiler path_ to the path to _fpc_. Normally this is already done.
  * Then open _Tools - > Configure Build Lazarus_.
  * Set _Target OS_ (e.g. to _Win64_) and _Target CPU_ (e.g. to _x86_64_)
  * Click the _Build_ button.



### Command line

Apart from the IDE, the command line also allows you to build a cross compiled LCL. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Since 0.9.31 you should use lazbuild to cross compile Lazarus packages. See lazbuild -h for a list of options.

An example: you would cross compile a Windows 64 cross compiler using: 

  * First thoroughly clean any 64 bit leftovers. This does not touch your 32 bit environment:


    
    
    make distclean LCL_PLATFORM=win32 CPU_TARGET=x86_64 OS_TARGET=win64
    

  * Then build LCL and its required dependencies. We're using LCL_PLATFORM as that presumably corresponds to the widgetset, which is still Windows 32 even on Windows 64 (the widgetsets are the same).


    
    
    make packager/registration lazutils lcl LCL_PLATFORM=win32 CPU_TARGET=x86_64 OS_TARGET=win64
    

The LCL for your normal OS is untouched. 

## Cross compiling LCL applications

You first need to cross compile the LCL. See above. 

Cross compiling applications means: compiling plus linking. When you have cross compiled the LCL the compilation part is easy. Just set in the compiler options of the IDE the Target OS and Target CPU. The tricky part is the linking. If you cross compile a project you may see something like this: 
    
    
    /usr/local/bin/arm-linux-ld: cannot find -lX11
    

This means you have to install the graphical libraries of the target system. This has nothing to do with FPC/Lazarus, but with cross compiling a library. Some distributions provides pre compiled packages for this. For example Microsoft provides cross compiled libraries for WinCE for Windows. Under Linux you can install Wine to cross compile from Linux to Windows. Some Linux distributions provide 64bit libraries for 32bit systems. 

## Cross compile FAQ

### Why cross compile?

So you can develop a program for one OS/CPU and compile it for another OS/CPU without rebooting or switching computers. 

### Why not to cross compile?

In many cases, you will want to test the application which you have cross compiled on its native platform. Compiling the application on that platform in the first place may be easier to set up, especially for new users. 

One way of being able to compile and test your application on a non-native platform is to use a Virtual Machine (VM) of that platform on your computer. For example, you could setup a Linux/FreeBSD/Windows VM running on an Apple computer running its native macOS operating system. 

There are a number of commercial and free Virtual Machine solutions: 

  * [VMware Fusion Pro for Mac](<https://www.vmware.com/products/fusion.html>) \- Host system must be running macOS (Commercial).
  * [VMware Fusion Player for Mac](<https://www.vmware.com/products/fusion.html>) \- Host system must be running macOS (dual-licence: Commercial/Free for Personal Use). Do not be fooled by the name; this is able to create VMs. The difference between Pro and Player is that Player lacks some of the advanced features (eg encrypted VMs) which can be found in the Pro version.
  * [VMware Workstation Player](<https://www.vmware.com/products/workstation-player.html>) \- Host system running Windows 64 bit or Linux 64 bit (Commercial/Free for personal use).
  * [VMware Workstation Pro](<https://www.vmware.com/products/workstation-pro.html>) \- Host system running Windows 64 bit or Linux 64 bit (Commercial/Free trial).
  * [Parallels Desktop for Mac](<https://www.parallels.com/>) \- Host system must be running macOS (Commercial/Free trial).
  * [VirtualBox](<https://www.virtualbox.org/>) \- Host system can be running macOS, Windows, Linux or Solaris (Open Source).



Some, eg Parallels on the Mac, make it trivial to install a desktop Linux operating system with a single click. 

### Why cross compile from Unix to Windows and not the other way around?

See [Why Unix to Windows and not the other way around](<Cross_compiling_for_Win32_under_Linux.md> "Cross compiling for Win32 under Linux")

### I want more information on building Free Pascal. Where is it?

There is a general FAQ in pdf format about how to build and configure FPC: [buildfaq](<buildfaq.md> "buildfaq"). 

### Errors like compiler "/usr/bin/fpc" does not support target arm-linux

Apart from other causes, this error may occur if you edited fpc.cfg with incorrect options for the cross compiler (e.g. specifying parameters all on one line instead of one per line). 

## References

  1. S. Ribić and M. Beganović, "Cross compilation under Lazarus IDE," 2013 21st Telecommunications Forum Telfor (TELFOR), Belgrade, 2013, pp. 1007-1010, [doi 10.1109/TELFOR.2013.6716402](<https://doi.org/10.1109/TELFOR.2013.6716402>).

---

_Source: [https://wiki.freepascal.org/Cross_compiling](https://web.archive.org/web/20250418194752/https://wiki.freepascal.org/Cross_compiling)_
