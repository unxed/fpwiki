# Cross compiling for Windows under Linux

│ **English (en)** │    
****

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Rewrite in progress, original content is retained inside comment tags; will pop up here again as it's reviewed and updated as necessary.

## Contents

  * 1 General
    * 1.1 Install FPC/Lazarus
    * 1.2 Setting up x86_64 Linux to Windows Cross-compile
    * 1.3 Testing
  * 2 Lazarus/LCL
    * 2.1 Cross-compiling the LCL and Lazarus components
    * 2.2 Cross-compiling a project
    * 2.3 Hints for cross-compiling with Lazarus
      * 2.3.1 Example 1: Cross-compiling a project for Linux and Win32
      * 2.3.2 Example 2: Cross-compiling a project for various platforms and widget sets
    * 2.4 Cross-compiling and Lazarus packages
  * 3 See also



## General

One of the FPC's features is its ability to cross-compile. It's very useful to be able to make Windows (32-bit and 64-bit) from your Linux workstation. Everything we do here relates to FPC, no changes are needed to Lazarus. 

It's very important to note that these instructions apply to a FPC that was installed from packages from SourceForge; if you are using packages from your Linux distribution then they will almost certainly not work. In fact, in many cases, Linux distribution repository packages may not support building cross-compiles. Please consider uninstalling the distribution's packages and replacing them with ones from the [official FPC/Lazarus SourceForge repository](<https://sourceforge.net/projects/lazarus/files/Lazarus>). 

These instructions have been tested on Ubuntu Linux 21.04 "The Hirsute Hippo" and openSUSE Leap 15.3. 

### Install FPC/Lazarus

If you already have a working FPC/Lazarus install obtained from SourceForge, then skip this step. Install FPC and Lazarus using the SourceForge method as detailed in [Installing Lazarus on Linux § Build Lazarus from Source](<Installing_Lazarus_on_Linux.md> "Installing Lazarus on Linux") It's a good idea to fire it up and make sure you can build and compile a simple app first. 

### Setting up x86_64 Linux to Windows Cross-compile

Note, we assume the use of FPC 3.2.2, if you're using a different version, put the right numbers in yourself! 

The process here is to build the pre-compiled FPC units to suit Windows (both 32-bit and 64-bit) and, of course, the Windows compilers, ppcross386 and ppcross64. We then put a symlink to the compilers in the right place and make sure your FPC config files know about the Windows pre-compiled units. 

Now to make a cross-compiler, the following needs to be done as root. It's easier (but slightly riskier) to `sudo` (or `su`) and stay as root for the whole process, so please be careful. We will set a temporary environment variable containing the FPC version number to make copy and pasting easy (assuming you are using FPC 3.2.2). 
    
    
    sudo -i 
    export FPCVER="3.2.2"
    cd "/usr/share/fpcsrc/${FPCVER}"
    make clean all OS_TARGET=win64 CPU_TARGET=x86_64
    make clean all OS_TARGET=win32 CPU_TARGET=i386
    

Those steps take awhile each. Watch for errors as the compile report scrolls by. It builds Windows versions of the units in the FPC-SRC tree. The next lines are pretty quick; they copy the units to `/usr/lib/fpc/${FPCVER}/units` and the ones after that make symlinks to the cross-compilers. 
    
    
    make crossinstall OS_TARGET=win64 CPU_TARGET=x86_64 INSTALL_PREFIX=/usr
    make crossinstall OS_TARGET=win32 CPU_TARGET=i386 INSTALL_PREFIX=/usr
    ln -sf "/usr/lib/fpc/${FPCVER}/ppcrossx64" /usr/bin/ppcrossx64
    ln -sf "/usr/lib/fpc/${FPCVER}/ppcross386" /usr/bin/ppcross386
    

Assuming you did not see any errors, does your `fpc.cfg` file need attention? 
    
    
    grep 'Fu' /etc/fpc.cfg
    

Does the result listed include `-Fu/usr/lib/fpc/${fpcversion}/units/${fpctarget}/*`? If not, you need to fire up your favorite editor and add it. Look for the first line in the sample below, and when you find it, add the second line as shown. 
    
    
    # searchpath for units and other system dependent things
    -Fu/usr/lib/fpc/${fpcversion}/units/${fpctarget}/*
    

That's it, press `Ctrl`+`D` to get out of the risky root mode, and test it out. 

### Testing

Fire up Lazarus and open a new project, then: 

  * Project ⇒ ProjectOptions ⇒ ConfigAndTarget, and set Target OS (-T) to Win64
  * Project ⇒ ProjectOptions ⇒ ConfigAndTarget, and set LCLWidgetType to Win32
  * Run ⇒ Build
  * Project ⇒ ProjectOptions ⇒ ConfigAndTarget, and set Target OS (-T) to Win32
  * Project ⇒ ProjectOptions ⇒ ConfigAndTarget, and set Target CPU Family (-P) to i386
  * Project ⇒ ProjectOptions ⇒ AdditionsAndOverRides, and set LCLWidgetType to Win32
  * Run ⇒ Build



Obviously, you cannot 'run' that binary, but if one is made, it's almost certainly okay. 

## Lazarus/LCL

### Cross-compiling the LCL and Lazarus components

The IDE automatically cross-compiles all used packages when you change the target of your project and build it. 

### Cross-compiling a project

Lazarus projects can have 'Modes', and it's convenient to set a mode for each compilation you want to perform. Here you can make Linux64 binaries and Windows 32-bit and 64-bit executables, so make a mode for each and apply the appropriate settings to each mode. 

In Project ⇒ ProjectOptions ⇒ ConfigAndTarget, set the Target OS to 'win64' and in "Additions and Overrides" click "Set LCL WidgetType" and select win32. That's all. The next time you build, you will create a win64 executable. 

Similarly, to make a Win32 executable, do the above but also set Project ⇒ ProjectOptions ⇒ ConfigAndTarget and set Target CPU Family (-P) to i386. 

The IDE will rescan for the Windows units, so that 'Find declaration' and code completion features will now work with the win32 rtl instead of the Linux rtl. 

### Hints for cross-compiling with Lazarus

If you create an application/package for multiple targets, you will often do the following: 

  * Fix a bug
  * Compile and test it under Linux, then
  * Compile and test it under Win32



Because normally you overwrite your .ppu files you have to recompile everything, every time you switch. However, this is not necessary; the Lazarus IDE supports macros. 

#### Example 1: Cross-compiling a project for Linux and Win32

Set Project ⇒ Compiler Options ⇒ Paths ⇒ Unit Output directory to $(TargetOS). This macro will be replaced by the value in Code ⇒ TargetOS in lowercase (i.e. "linux" for Linux and "win32" for Win32). The output directory is relative to your project directory (the directory where your .lpi file is). Create `linux` and `win32` directories in your project directory. 

When you click on the "Show Options" button at the bottom of the compiler options, you will see a `-FElinux/` or `-FEwin32/`. This option tells the compiler where to write the output (e.g. .ppu/.o files). 

#### Example 2: Cross-compiling a project for various platforms and widget sets

Set the Unit output directory to $(TargetCPU)/$(TargetOS)/$(LCLWidgetType) and create the sub-directories for all targets. This path construction is also used by the LCL. 

The same can be done for packages. 

### Cross-compiling and Lazarus packages

Lazarus packages are not limited to libraries, they can be used to compile nearly everything and the IDE automatically recompiles them, if needed. 

Packages can inherit compiler options. For example, a project that uses a package inherits the output directory of the package. In other words, the output directory of the package is added to the unit search path of the project. See for yourself in IDE: Project ⇒ Compiler options ⇒ Inherited. 

Inheritance normally works only one way, but there are exceptions: 

  * The target platform (OS and CPU) of the project override the target for all used packages. That means if you set the Target OS of the project to "win32" and compile the project, the IDE will check if the used packages need to be recompiled for this Target OS. For example: 
    * Package A has as output directory: lib/$(TargetOS). Project uses A.
    1. The project is built for linux. The IDE compiles A for linux in <PackageDirOfA>/lib/linux/, then it compiles the project for linux.
    2. The project is built for win32. The IDE compiles A for win32 in <PackageDirOfA>/lib/win32/, then it compiles the project for win32.
    3. The project is built again for linux. The IDE checks A for linux and does not recompile it, then it compiles the project for linux.



So using the macros saves a lot of time. 

  


## See also

  * [Cross compiling](<Cross_compiling.md> "Cross compiling")
  * [buildfaq](<buildfaq.md> "buildfaq")

---

_Source: [https://wiki.freepascal.org/Cross_compiling_for_Windows_under_Linux](https://web.archive.org/web/20231209021157/https://wiki.freepascal.org/Cross_compiling_for_Windows_under_Linux)_
