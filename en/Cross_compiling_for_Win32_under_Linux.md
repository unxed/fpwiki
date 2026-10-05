# Cross compiling for Windows under Linux

│ **English (en)** │    
****

## Contents

  * 1 General
    * 1.1 Install FPC/Lazarus
    * 1.2 Setting up a 64bit Linux to Windows Cross Compile.
    * 1.3 Testing
  * 2 Lazarus/LCL
    * 2.1 Cross compiling the LCL and Lazarus components
    * 2.2 Cross compiling a project
    * 2.3 Hints for Cross compiling and Lazarus
    * 2.4 Cross compiling and Lazarus Packages
  * 3 Legacy (for now)
    * 3.1 Introduction - knowing, what you are doing
    * 3.2 Free Pascal
      * 3.2.1 Why *nix to Windows and not the other way around
      * 3.2.2 Newer FPCs - 2.1.1 and newer
      * 3.2.3 An example under Bunsen Labs (Debian 8)
      * 3.2.4 FPC older than 2.1.1
  * 4 See also



## General

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Rewrite in progress, original content is down below, will pop up here again as its reviewed and updated as necessary

One of the FPC features is is ability to cross compile. Its very useful to be able to make Windows (32bit and 64bit) from your Linux workstation. Everything we do here relates to FPC, no changes are needed to Lazarus. 

Its very important to note that these instructions apply to a FPC that was installed from packages from SourceForge, if you are using packages from your Linux Distribution then they will almost certainly not work, in fact, in many cases, Linux Distribution Repository packages may not support building cross compiles. Please consider uninstalling the Distribution's packages and replacing them with ones from the official FPC/Lazarus SourceForge repository. <https://sourceforge.net/projects/lazarus/files/Lazarus>

These instructions have been tested on Linux Ubuntu 21.04 and SUSE Leap 15.3. 

### Install FPC/Lazarus

If you already have a working FPC/Lazarus install obtained from SourceForge, skip the this step. Install FPC and Lazarus Using the SourceForge method as detailed in [https://wiki.freepascal.org/Installing_Lazarus_on_Linux#Build_Lazarus_from_Source](<Installing_Lazarus_on_Linux.md>) Good idea to fire it up and make sure you can build and compile a simple app. 

### Setting up a 64bit Linux to Windows Cross Compile.

Note, we, assume fpc 3.2.2, if not, put the right numbers in yourself ! 

The process here is to build the pre compiled FPC units to suit Windows (both 32bit and 64bit) and, of course the Windows compilers, ppcross386 and ppcross64. We then put a symlink to the compilers in the right place and make sure your FPC config files knows about the Windows pre compiled units. 

Now to make a cross compiler, the following needs to be done as root, its easier (but slightly riskier) to sudo (or su) and stay as root for the whole process. So, please be careful. We will set a temporary env variable containing the FPC version number to make copy and pasting easy (assuming you are using FPC3.2.2) - 
    
    
    sudo -i 
    export FPCVER="3.2.2"
    cd /usr/share/fpcsrc/"$FPCVER"/
    make clean all OS_TARGET=win64 CPU_TARGET=x86_64
    make clean all OS_TARGET=win32 CPU_TARGET=i386
    

Those steps take awhile each, watch for errors as the compile report scrolls by. It builds Windows versions of the units in the FPC-SRC tree. The next lines are pretty quick, they copies the units to /usr/lib/fpc/3.2.2/units and the ones after that makes a symlinks to the crosscompilers. 
    
    
    make crossinstall OS_TARGET=win64 CPU_TARGET=x86_64 INSTALL_PREFIX=/usr 
    make crossinstall OS_TARGET=win32 CPU_TARGET=i386 INSTALL_PREFIX=/usr
    ln -sf /usr/lib/fpc/"$FPCVER"/ppcrossx64 /usr/bin/ppcrossx64
    ln -sf /usr/lib/fpc/"$FPCVER"/ppcross386 /usr/bin/ppcross386
    

Right, assuming you did not see any errors, does your fpc.cfg file need attention ? 
    
    
    grep Fu /etc/fpc.cfg
    

Does the result listed include "-Fu/usr/lib/fpc/$fpcversion/units/$fpctarget/*" ? If not, you need to fire up your favorite editor and add it. Look for the first line in the sample below, when you find it, add the second line as shown. 
    
    
    # searchpath for units and other system dependent things
    -Fu/usr/lib/fpc/$fpcversion/units/$fpctarget/*

Thats it, press Control-D, to get out of the risky root mode. And test it out. 

### Testing

Fire up Lazarus, open a new project, then 

  * Project->ProjectOptions->ConfigAndTarget and Set Target OS (-T) to Win64
  * Project->ProjectOptions->ConfigAndTarget, Set LCLWidgetType" to Win32
  * Run->Build
  * Project->ProjectOptions->ConfigAndTarget and Set Target OS (-T) to Win32
  * Project->ProjectOptions->ConfigAndTarget and Set Target CPU Family (-P) to i386
  * Project->ProjectOptions->AdditionsAndOverRides, Set LCLWidgetType" to Win32
  * Run->Build



  
Obviously, you cannot 'run' that binary but if one is made, its almost certainly OK. 

## Lazarus/LCL

### Cross compiling the LCL and Lazarus components

The IDE automatically cross compiles all used packages when you change the target of your project and build it. 

### Cross compiling a project

Lazarus Projects can have 'Modes' and its convenient to set a mode for each compile you want to do. So, here, now you can make Linux64 binaries and Windows 32bit and 64bit executables, so make a mode for each and apply the appropriate settings to each mode. 

In Project->ProjectOptions->ConfigAndTarget, set the Target OS to 'win64' and in "Additions and Overrides" click Set LCL WidgetType and select win32. That's all. The next time you build, you will create a win64 executable. 

Similarly, to make a Win32 executable, do the above but also set Project->ProjectOptions->ConfigAndTarget and Set Target CPU Family (-P) to i386 

The IDE will rescan for the Windows units, so that 'Find declaration' and code completion features will now work with the win32 rtl instead of the linux rtl. 

  


### Hints for Cross compiling and Lazarus

If you create an application/package for multiple targets, you will often do the following: Fix a bug, compile and test it under Linux, then compile and test it under win32, .. . Because normally you overwrite your .ppu files, you have to recompile everything, everytime you switch. This is not necessary. The Lazarus IDE supports macros. 

Example 1: Cross compiling a project for linux and win32. 

Set Project -> Compiler Options -> Paths -> Unit Output directory to $(TargetOS). This macro will be replaced by the value in Code -> TargetOS in lowercase (i.e. "linux" for Linux and "win32" for Win32). The output directory is relative to your project directory (the directory where your .lpi is). Create a linux and win32 directory in your project directory. 

When you click on the "Show Options" button at the bottom of the compiler options, you will see a -FElinux/ or -FEwin32/. This option tells the compiler where to write the output (e.g. .ppu/.o files). 

  
Example 2: Cross compiling a project for various platforms and widget sets. 

Set the Unit output directory to $(TargetCPU)/$(TargetOS)/$(LCLWidgetType) and create the sub directories for all targets. This path construction is also used by the LCL. 

The same can be done for packages. 

### Cross compiling and Lazarus Packages

Lazarus packages are not limited to libraries. They can be used to compile nearly everything. And the IDE automatically recompiles them if needed. 

Packages can inherit compiler options. For example: A project that uses a package inherits the output directory of the package. In other words: the output directory of the package is added to unit search path of the project. See in the IDE: Project -> Compiler options -> Inherited. 

Inheritance normally works only one way, but there are exceptions: The target platform (OS and CPU) of the project overrides the target for all used packages. That means, if you set the Target OS of the project to "win32" and compile the project, the IDE will check if the used packages need to be recompiled for this Target OS. 

For example: 

Package A has as output directory: lib/$(TargetOS) Project uses A. 

  1. The project is built for linux. The IDE compiles A for linux in <PackageDirOfA>/lib/linux/, then it compiles the project for linux.
  2. The project is built for win32. The IDE compiles A for win32 in <PackageDirOfA>/lib/win32/, then it compiles the project for win32.
  3. The project is built again for linux. The IDE checks A for linux and does not recompile it. Then it compiles the project for linux.



So, using the macros saves a lot of time. 

  


## Legacy (for now)

**This is information that is possibly out of date and unnecessary. It might be deleted some time in the future so, if you find something still valuable, consider moving it up above the above heading.**

  


### Introduction - knowing, what you are doing

**Note :** If your FPC has come from your Linux Distribution Repository, it is likely it won't have everything needed to be turned into a cross compiler. Please see [Installing the Free Pascal Compiler - Linux](<Installing_the_Free_Pascal_Compiler.md> "Installing the Free Pascal Compiler"). 

This is a short introduction for newbies. The following sections describe how to set up a Linux system to cross compile, creating Win32 executables (or FreeBSD or Darwin/macOS, or ...). Why cross compiling? Free Pascal is a compiler and basically converts source into binaries (machine language). These binaries also contain information about how the operating system should start the executable. Therefore these binaries are platform specific. 

Free Pascal itself does not need much setup. It can create binaries for many platforms. Just tell it to do so. But the compiler is only one part. 

There is also the assembler and the linker. And these tools are not able to create cross-platform code. That's why we have to create a special linker 'ld' and assembler 'as' for every target platform. These are the binutils. 

After creating the cross tools, all the FPC Pascal units will be cross compiled. For example, there will then be one system.ppu file for every target platform. Next, your FPC config file (fpc.cfg) will be set up, so that cross compilation becomes so easy, that you can forget all the boring details. The same will be done for the LCL - the Lazarus Component Library. And after this you can cross compile Pascal programs for Win32. Either start them with wine or copy them to a Windows machine and test them there. 

### Free Pascal

#### Why *nix to Windows and not the other way around

The main reason for this is that generating Linux/Unix binaries on a foreign platform (even another Unix or Linux system) is more complicated. Static linking is already complicated, let alone shared. 

You would need the used libraries from the target platform (gtk, glib, libc etc), and a lot of additional configuring for ld (library paths, dynlinker path etc). 

This has been partially done (for the static case), but it is hard since it needs manual postediting of linker files and linker commandline, and a deep understanding about what makes Unix binaries tick. 

#### Newer FPCs - 2.1.1 and newer

If you are compiling a 2.1.1 or newer version of FPC you can just do: 
    
    
    $ make all OS_TARGET=win32 CPU_TARGET=i386
    

and then 
    
    
    $ su -c "make crossinstall OS_TARGET=win32 CPU_TARGET=i386"
    

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:**   
make crossinstall by default installs all files in **/usr/local/lib/fpc/$fpcversion/units** directory, that's why you will need to add to **/etc/fpc.cfg file** a new search path:  
**-Fu/usr/local/lib/fpc/$fpcversion/units/$fpctarget/***.  
  
Another option is to use INSTALL_PREFIX=**/usr** while performing crossinstall. In this case, you will not need to change anything in **/etc/fpc.cfg** file because all will be installed in **/usr/lib/fpc/$fpcversion/units** which is already there

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:**   
To build for win64 the make command is: make all OS_TARGET=win64 CPU_TARGET=x86_64

The reason for this simplicity is the internal linker included in this version of fpc. 

#### An example under Bunsen Labs (Debian 8)

\- Cross-compile to Win32 and Win64 using FPC 3.0.0 and Lazarus 1.6.2 

\- Open up your terminal and execute the following commands (Many thanks to Handoko and Leledumbo for this, you are awesome). 
    
    
    # Navigate to the fpc source folder.
    cd /usr/share/fpcsrc/3.0.0
    
    # Compile the cross-compiler.
    sudo make clean all OS_TARGET=win32 CPU_TARGET=i386
    
    # Install the cross-compiler.
    sudo make crossinstall OS_TARGET=win32 CPU_TARGET=i386 INSTALL_PREFIX=/usr
    
    # Link the cross-compiler and place the link where Lazarus can see it.
    sudo ln -sf /usr/lib/fpc/3.0.0/ppcross386 /usr/bin/ppcross386
    
    # Do the same using x64 as target
    sudo make clean all OS_TARGET=win64 CPU_TARGET=x86_64
    sudo make crossinstall OS_TARGET=win64 CPU_TARGET=x86_64 INSTALL_PREFIX=/usr
    sudo ln -sf /usr/lib/fpc/3.0.0/ppcrossx64 /usr/bin/ppcrossx64
    

\- Make sure your cross-compilers are alive: (Note, the links to the cross compiling binaries appear in /usr/bin rather than /usr/lib as shown in the images below). 

[![000-cross-compilers-compiled.png](https://wiki.freepascal.org/images/e/e6/000-cross-compilers-compiled.png)](</File:000-cross-compilers-compiled.png>)

\- Make sure your cross-compilers were properly linked: 

[![000-cross-compilers-linked.png](https://wiki.freepascal.org/images/7/76/000-cross-compilers-linked.png)](</File:000-cross-compilers-linked.png>)

\- Now open Lazarus. 

\- Find the "Paths" item in the right list of the "Project Options" window. Press Ctrl+Shift+F11 or navigate through the Projects->Project Options->Paths menus. 

\- For each build, configure your paths so that all necessary libraries are reacheable and all output files can be generated by Lazarus/FPC with no overriding. I have chosen to use macros in the "Unit output path" and "Target file name". 

[![002-paths-win32.png](https://wiki.freepascal.org/images/5/57/002-paths-win32.png)](</File:002-paths-win32.png>)

\- Create builds and edit build names in the "Build Mode: [BuildName]" window (click in the upper [...] button to open it). 

[![001-build-modes.png](https://wiki.freepascal.org/images/7/7b/001-build-modes.png)](</File:001-build-modes.png>)

\- Click ok when your done. 

\- Now go to Run -> "Build Many Modes". Press ok. Wait until Lazarus and FPC finishes their work. 

[![003-build-many-modes.png](https://wiki.freepascal.org/images/0/07/003-build-many-modes.png)](</File:003-build-many-modes.png>)

\- Go to your project folder and enjoy! 

[![Write once, compile anywhere.](https://wiki.freepascal.org/images/6/62/004-executables.png)](</File:004-executables.png> "Write once, compile anywhere.")

#### FPC older than 2.1.1

For FPC versions older than 2.1.1, please see the History link (version before 19 August 2014) to see the steps needed 

## See also

  * [Cross compiling](<Cross_compiling.md> "Cross compiling")
  * [crossnotes](<http://www.stack.nl/~marcov/crossnotes.txt>) Notes about cross compiling
  * [buildfaq](<buildfaq.md> "buildfaq")

---

_Source: [https://wiki.freepascal.org/Cross_compiling_for_Win32_under_Linux](https://web.archive.org/web/20220807212953/https://wiki.freepascal.org/Cross_compiling_for_Win32_under_Linux)_
