# Installing Lazarus on MacOS X

[![Logo OSX.png](https://wiki.freepascal.org/images/7/7a/Logo_OSX.png)](</File:Logo_OSX.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │


Installing Lazarus on a Mac is not particularly difficult but it is critical that you do the install in the correct order. Skipping steps will almost certainly lead to tears. In brief, here is what you do - 

  1. Download and install Xcode. 
  2. Install the global command line tools for Xcode. 
  3. Install Free Pascal Compiler and FPC Source 
  4. Install Lazarus 
  5. Configure LLDB - the Apple supplied (and signed) debugger from within Lazarus. 



  


  1. Note : if installing versions of Lazarus prior to 2.0.0 you almost certainly also need gdb, see in the Legacy section. 



## Contents

  * 1 Installation
    * 1.1 Step 1. Download Xcode.
    * 1.2 Step 2. Xcode Command Line Tools
    * 1.3 Step 3 FPC, FPC Source
      * 1.3.1 Source Forge
      * 1.3.2 Other FPC Options
    * 1.4 Install Lazarus
    * 1.5 Configure the Debugger
      * 1.5.1 Extra Information on using LLDB
      * 1.5.2 Installing LazDebuggerFpLLdb"
  * 2 Compatibility
  * 3 Carbon and Cocoa
  * 4 Download the sources via svn
  * 5 Download stable sources
  * 6 Legacy Information
    * 6.1 Install gdb
    * 6.2 Xcode Compatability
      * 6.2.1 Compatibility with Xcode 5.0 and newer versions (Mac OS X 10.8, 10.9, 10.10 and 10.11)
      * 6.2.2 Xcode 4.3+ compatibility (Mac OS X 10.7/10.8)
      * 6.2.3 Xcode 3.2.x compatibility (Mac OS X 10.6)
      * 6.2.4 Xcode 3.1.x compatibility (Mac OS X 10.5)
      * 6.2.5 Xcode 2.x compatibility (Mac OS X 10.4)
    * 6.3 Install Packages / Released version
      * 6.3.1 Installation from disk images
    * 6.4 Install from Source / Development version
      * 6.4.1 Download and install a compiler
  * 7 Uninstalling Lazarus and Free Pascal
    * 7.1 Installed using fink
    * 7.2 Installed from disk images or sources
      * 7.2.1 Lazarus preference folder .lazarus in the home directory
  * 8 Known issues
  * 9 See Also
    * 9.1 Other Interfaces
      * 9.1.1 Platform specific Tips
      * 9.1.2 Mac How To Articles
      * 9.1.3 Interfaces Development Articles



## Installation

The detailed instructions assume a recent version of macOS on you Mac, a recent version of Xcode from Apple and recent version of Lazarus. Further down the page, under Legacy Documentation, you will see older information that may be relevant if you are using older components. You can assist by replacing out of date info, either deleting it, or, if it may help someone working with a legacy project, moving it to the bottom of the page. 

In general, this is about using both the **Carbon** and **Cocoa** Widget Set. While Carbon may still be seen as a little more stable, as of release 2.0.0 the 64bit Cocoa is very close and certainly should be considered. Carbon is intentionally (by Apple) limited to 32 bits and we know the next release of MacOSX will probably not support it. 

### Step 1. Download Xcode.

You need the Apple Developer tools, which are a part of the Xcode development environment. They can be installed from the original macOS installation disks or a newer copy downloaded from the Apple Developer Connection (ADC), which requires free registration: <http://developer.apple.com/> Download the Xcode file, it will end up in your Downloads directory as a zip file. September 2017 it is Xcode_9_beta_6.xip, about 5G. Click it. It is unarchived into your Downloads directory. You may be happy with it there but maybe not. Other users will see the path to it but be unable to use it. And it is untidy there. So I moved mine and then told xcode-select where it was moved to (in a terminal) - 
    
    
    mv Downloads/Xcode-beta.app /Developer/.
    sudo xcode-select -s /Developer/Xcode-beta.app/Contents/Developer

### Step 2. Xcode Command Line Tools

This is shown here as a separate step because it really is a separate step in addition to Step 1. Don't confuse this with the internal Xcode command line tools that the Xcode GUI will tell you are already installed. Lazarus cannot use those Xcode internal command line tools, so do the following (it is quick and easy)- 
    
    
    xcode-select --install

Since macOS Mojave you need to install the SDK headers as well: 
    
    
    open /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg
    

  


### Step 3 FPC, FPC Source

Get and install FPC and FPC Source. A compatible fpc (and source) must be installed before you install Lazarus, you have a number of options. 

#### Source Forge

Get release version from Link <https://sourceforge.net/projects/lazarus/files> . These binary install kits are built by the FPC/Lazarus developers and track formal releases. As these install kits are not approved by Apple, you need to hold down the Control key, click the package and choose Open and confirm you want to install from an Unknown Developer (i.e. one who does not pay Apple a kickback). 

For authoritative/more detailed instructions for FPC, please go to [[1]](<http://freepascal.stack.nl/down/i386/macosx-netherlands.html>)

You might like to apply a simple and quick test of fpc at this stage - [http://wiki.freepascal.org/Installing_Lazarus_on_Linux#Testing_FPC_installation](<Installing_Lazarus_on_Linux.md>)

#### Other FPC Options

You could also consider using fpcupdeluxe to install fpc and, perhaps Lazarus, [ fpcupdeluxe](<fpcupdeluxe.md> "fpcupdeluxe")

Alternatively, you can use [fink](<http://finkproject.org/>), a package manager for macOS to install fpc. Note at the time of writing (Early 2019) fink offers Lazarus 1.8.4 and its generally agreed that versions later than 2.0.0 (ie non-release) are required so probably unsuited for Cocoa development. The extra bonus of fink is easy installation as well as clean removal of a huge number of other open source software packages, including [ FreePascal crosscompilers ](<Cross_compiling.md> "Cross compiling") for many processors and systems. The choice for lazarus is between a carbon look of lazarus (preferred by most), a gtk2 look, a Qt4-based and a Qt5-based version: 
    
    
    $ fink install lazarus-aqua
    // or
    $ fink install lazarus-gtk2
    // or
    $ fink install lazarus-qt4
    // or
    $ fink install lazarus-qt5

You will be asked, whether to install a number of dependencies, including the FreePascal compiler, the lazarus sources. Simply hit RETURN at the prompt and go for a coffee. It may take quite some time to build all packages, in particular on older Macs. 

After installation, Lazarus can be started from the folder /Applications/Fink/. The actual details of fpc and lazarus are in subdirectories of /sw 

With any lazarus package from fink, these widget sets are supported for your program on macOS: 

carbon (Aqua), gtk2, qt4 (Aqua), qt5 (Aqua), nogui, win32, win64 and wince. 

If you install fpc from fink, you will be fine downloading Lazarus source and compiling as detailed below. However, its possibly not a good idea to mix fink fpc with the sourceforge binary install of Lazarus. 

### Install Lazarus

At time of writing (early 2019), there are a number of reasons why you may be better off using a non release version of Lazarus, specifically, fixes_2_0. Particularly - 

  * You almost certainly need to target Cocoa, its likely the next release of MacOSX will not support 32bit Carbon. 
  * You almost certainly want to use lldb instead of gdb - far easier install. 
  * The Cocoa is steadily improving and lldb interface is rapidly improving since even 2.0.0. 
  * Fixes_2_0 is a safer and more stable than trunk but gets the new stuff much faster than release version. 



Precompiled binaries based on fixes_2_0 are not available from SourceForge but can be obtained from [fpcupdeluxe](<fpcupdeluxe.md> "fpcupdeluxe") . But here we will discuss downloading source and building. An little slow initially but very reliable and a great test of your compiler install ! You will need svn (its not pre installed on early MacOS, use brew or fink to install). 
    
    
    cd ~
    mkdir -p bin/lazarus
    cd bin/lazarus
    svn checkout https://svn.freepascal.org/svn/lazarus/branches/fixes_2_0 laz_fixes
    // wait some time ....
    cd laz_fixes
    make LCL_PLATFORM=cocoa CPU_TARGET=x86_64 bigide
    // wait some time .....
    open startlazarus.app --args "--pcp=~/.laz_fixes"

  * Note I pass a parameter to use a config directory thats based on the name of the actual install directory. It makes some scripting easy. 


  * Its also quite feasible at present (and perhaps a touch safer) to build a 32bit Carbon Lazarus IDE and use it to build 64bit Cocoa applications. Replace above make line with "_make LCL_PLATFORM=carbon CPU_TARGET=i386 bigide_ " and setup your project as mentioned in the [Carbon_and_Cocoa](<Installing_Lazarus_on_MacOS_X.md> "Installing Lazarus on MacOS X") section below. 



You might like to put a small script in your $HOME/bin directory and even set a path to it (very unix!) 
    
    
    #!/bin/bash
    LAZDIR="laz_fixes"
    cd ~/bin/lazarus/"$LAZDIR"
    open ~/bin/lazarus/"$LAZDIR"/lazarus.app --args "--pcp=~/.$LAZDIR"

### Configure the Debugger

In versions of Lazarus 1.8.4 and earlier, you needed to use gdb as a debugger, slow to install and hard to sign. Since Lazarus 2.0.0 you can (and should) use LLDB, a debugger provided by Apple, no signing required. 

Assuming you have installed whats necessary and started Lazarus, all that remains is configuring the debugger. If you don't do this now, Lazarus will try to use GDB and fail. 

First, click Tools->Options->Debugger. Top right of the window now open has a label, "Debugger type and path", you must set both. Select "LLDB debugger (with fpdebug) (Beta)". 

[![Set Debugger.png](https://wiki.freepascal.org/images/8/8e/Set_Debugger.png)](</File:Set_Debugger.png>)

If its not show in the selection list, see below (Installing LazDebuggerFpLLdb".) Note that the image above has a path to LLDB that may well be different to yours, depends on where the XCode tools were installed, in my case /Library . Save those settings and you can now try to compile the almost nothing programme that Lazarus has kindly provided for you. (click the small green triangle near top left.) 

Next you see a puzzeling question, see image below. Choose a "Debug Format" from one of the offered - 

[![Set Dwarf.png](https://wiki.freepascal.org/images/0/03/Set_Dwarf.png)](</File:Set_Dwarf.png>)

Martin_fr, the person who has given us this interface between Lazarus and LLDB, suggests you use "dwarf3". Then, you need to enter your password, a MacOSX cuteness because one application appears to be interfering with another. In this case, thats fine ! 

#### Extra Information on using LLDB

A great deal of information about using LLDB appears in this forum thread <https://forum.lazarus.freepascal.org/index.php/topic,42869.0.html> Here is a few gems, again, from Martin_fr : 

In the unexpected case of problems, it may be worth trying "dwarf with sets" instead of just "dwarf3". 

The "debug info" setting only affects the units directly in your project. However units in packages may have debug info too. This can be 

  * set per package 
  * for many, but not all packages in menu Tools > Configure build Lazarus 
  * project settings > [Additions and Overrides](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options")



If you change settings for package, you may as well check into which package you expect to step. Package you do not step into, do not need debug info. If you use a type from a package (such as TForm from LCL) it is enough that your unit (in which you declare the variable / must declare and use a variable to include the type) has debug info. Reducing the amount of packages with debug info (including those that default to have debug info), can shorten the debuggers start-up time. 

Also it may be worth comparing (its not been tested) the debuggers start up time for the same settings, only changing the checkbox "use external debug info". This needs to be only set in your project. If set in your project it will affect all packages. (If set in a package it will do nothing / at least should...) 

#### Installing LazDebuggerFpLLdb"

If you installed from source and used the bigide parameter to make, then the correct debugger will be installed, as a package, and ready to go. If, however, you installed in another way, it may, or may not be there. From the main IDE screen, click Packages->Install/UninstallPackages. Show is two lists of packages, the list on the left is installed, the list on the right is available to install. Look for LazDebuggerFpLldb (exactly that, there are some similar named but less suitable packages). If its on the right hand side, click it, press "Install Selection" and then "Save and rebuild IDE". This will take a little time, the IDE will shutdown and restart and all should be well. Now jump back up the page and continue configuring the debugger. 

## Compatibility

Not every combination of Lazarus and Free Pascal is compatible with every installation of macOS. Please refer to the following table in order to find the correct version for your development environment: 

Lazarus Compatibility Matrix  |  Lazarus 1.0.0  |  Lazarus 1.0.2  |  Lazarus 1.0.4  |  Lazarus 1.0.6  |  Lazarus 1.0.8  |  Lazarus 1.0.10  |  Lazarus 1.0.12  |  Lazarus 1.0.14  |  Lazarus 1.2.0   
---|---|---|---|---|---|---|---|---|---  
|  Free Pascal (FPC) 2.6.0  |  FPC 2.6.2   
PPC processors   
Mac OS X 10.4 (Tiger)  |  Incompatible  |  Incompatible  |  N/A  |  Compatible  |  Incompatible  |  Incompatible  |  Compatible*  |  Compatible*  |  Not tested   
Mac OS X 10.5 (Leopard)  |  Compatible  |  Compatible  |  N/A  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible**   
Intel processors   
Mac OS X 10.4 (Tiger)  |  Incompatible  |  Incompatible  |  Compatible  |  Compatible  |  Incompatible  |  Incompatible  |  With hack only  |  Not tested  |  Not tested   
Mac OS X 10.5 (Leopard)  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Not tested  |  Not tested   
Mac OS X 10.6 (Snow Leopard)  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible   
Mac OS X 10.7 (Lion)  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible   
Mac OS X 10.8 (Mountain Lion)  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible   
Mac OS X 10.9 (Mavericks)  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Compatible*  |  Compatible*  |  Compatible*   
Mac OS X 10.10 (Yosemite)  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested   
Mac OS X 10.11 (El Capitan)  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested   
macOS X 10.12 (Sierra)  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested   
macOS X 10.13 (High Sierra)  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested   
macOS X 10.14 (Mojave)  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested   
|  Lazarus 1.2.2  |  Lazarus 1.2.4  |  Lazarus 1.2.6  |  Lazarus 1.4.0  |  Lazarus 1.4.2  |  Lazarus 1.4.4  |  Lazarus 1.6.0  |  Lazarus 1.6.2  |  Lazarus 1.6.4  |  Lazarus 1.8.x  |  Lazarus 2.0.y   
---|---|---|---|---|---|---|---|---|---|---|---  
|  FPC 2.6.4  |  FPC 3.0.0  |  FPC 3.0.2  |  FPC 3.0.4  |  FPC 3.0.4   
PPC processors   
Mac OS X 10.4 (Tiger)  |  Compatible  |  N/A  |  Compatible  |  Incompatible  |  Incompatible  |  Incompatible  |  Incompatible  |  Incompatible  |  Incompatible  |  Incompatible  |  Incompatible   
Mac OS X 10.5 (Leopard)  |  Compatible  |  Compatible**  |  Compatible  |  Compatible**  |  Compatible**  |  Compatible**  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Incompatible   
Intel processors   
Mac OS X 10.4 (Tiger)  |  Not tested  |  Not tested  |  Not tested  |  Incompatible  |  Incompatible  |  Incompatible  |  Incompatible  |  Incompatible  |  Incompatible  |  Incompatible  |  Incompatible   
Mac OS X 10.5 (Leopard)  |  Compatible  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested   
Mac OS X 10.6 (Snow Leopard)  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible  |  Compatible*  |  Not tested   
Mac OS X 10.7 (Lion)  |  Compatible  |  Compatible  |  Compatible  |  Not tested  |  Not tested  |  Not tested  |  Compatible  |  Compatible  |  Compatible  |  Not tested  |  Not tested   
Mac OS X 10.8 (Mountain Lion)  |  Compatible  |  Compatible  |  Compatible  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Not tested  |  Not tested   
Mac OS X 10.9 (Mavericks)  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Not tested  |  Not tested   
Mac OS X 10.10 (Yosemite)  |  Not tested  |  Not tested  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Not tested  |  Not tested   
Mac OS X 10.11 (El Capitan)  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Not tested  |  Not tested   
macOS X 10.12 (Sierra)  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible*  |  Compatible***   
macOS X 10.13 (High Sierra)  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested   
macOS X 10.14 (Mojave)  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Not tested  |  Compatible***   
  
*[Restrictions](<GDB_on_OS_X_Mavericks_and_Xcode_5.md> "GDB on OS X Mavericks and Xcode 5") apply to debugging with [GDB](<GDB.md> "GDB"). **Not available as pre-built installer. [Compiling from source](<Installing_Lazarus_on_MacOS_X.md> "Installing Lazarus on MacOS X") required. ***[LLDB](<lldb.md> "lldb") debugger required  
x = 0, 2 or 4; y = 0 or 2 

## Carbon and Cocoa

At present (early 2019) Lazarus using Cocoa will meet many users needs. Carbon work pretty much as expected but you are advised to try Cocoa first, especially considering that Apple has indicated releases after Mojave will not support 32bit Carbon. Alternatives include QT and GTK2+, both requiring additional libraries and GTK2+ on the Mac appears to get little attention these days. QT on the Mac does have some strong supporters on the forum. 

However, we should assume, **Cocoa** is the future on Mac. It is possible to build an all Cocoa version of Lazarus with release 2.0.0 and later. It is also possible to make a Carbon install of Lazarus and use that to generate Cocoa 64bit binaries. In a Carbon IDE, you need to set the Target to the 64bit processor and select the Cocoa Widget set: 

  * Open your project with Lazarus and click Project/ProjectOptions 
  * In the "Config and Target" panel set the "Target CPU family" to be "x86_64" 
  * In the "Additions and Overrides" panel click on "Set LCLWidgetType" pulldown and set the value to "Cocoa" 
  * In the past, for some reason Lazarus kept setting the compiler to "/usr/local/bin/ppc386" - which results in 32 bit apps. Make sure under Tools->Options that "Compiler Executable" is set to "/usr/local/bin/fpc" to get 64 bit apps. 
  * Now compile your project - and please feed back any problems you experience. 



## Download the sources via svn

The sources are kept in a version control system called subversion or short **svn** : 

  * 10.5 and higher already contains svn clients. Users of earlier versions must install SVN for macOS. A good package is provided by [Martin Ott](<http://www.codingmonkeys.de/mbo/>). You can also use fink. SVN clients with GUI (graphical user interface) are available from [Versiontracker](<http://www.versiontracker.com>). A quite handy client, which integrates in Finder, is [SCPlugin](<http://scplugin.tigris.org>). 



Create a directory, where you would like to put the sources. You don't need to be root to do this. Any normal user can do this. First create a directory for fpc 

     (e.g. /Users/username/freepascal) 

then open a terminal and do the following: 
    
    
    []$ svn co <http://svn.freepascal.org/svn/fpc/trunk> fpc
    

This will create a directory called 'fpc', which can be later used in the IDE. Hint: To download/update the latest changes you can simply do 
    
    
    []$ cd /Users/username/freepascal/fpc
    []$ svn up
    

Building fpc 
    
    
    []$ make clean all
    []$ sudo make install
    

Then download lazarus 
    
    
    []$ svn co <http://svn.freepascal.org/svn/lazarus/trunk> lazarus
    

This will create a directory called 'lazarus'. To update the latest changes: 
    
    
    []$ cd /Users/username/freepascal/lazarus
    []$ svn up
    

Building lazarus 
    
    
    []$ make clean all
    

Then start lazarus either via command line or by double click in the finder: 
    
    
     open lazarus.app
    

## Download stable sources

Source code of stable versions is available in the [Lazarus Zip - GZip](<http://sourceforge.net/projects/lazarus/files/Lazarus%20Zip%20_%20GZip/>) archives. This is e.g. an option for users, who want to run the latest official Lazarus versions on "unsupported" machines, e.g. PPC-based Macs. Provided that you have installed the latest stable FPC version together with sources, it is sufficient to download and unpack the zip archive. Move it to the desired location (e.g. your /Developer folder), open a terminal window and change the shell's path to this folder (the easiest way to do this is to type "cd " and then to drag the icon of the freshly unpacked Lazarus folder to the position after this command in the Terminal window). If necessary enter <return> and then 
    
    
    []$ make clean bigide
    

and <return> again. Subsequently, FreePascal will use the makefile in this folder to build the Lazarus IDE. This may take a few minutes, but the process is fully automated. Therefore, you might want to look for a cup of coffee, while FPC is working. If the compiler is ready it prints the number of processed lines and the required time to the shell's output. 

The last step is to right-klick on the Lazarus bundle icon and to select "Show Package Contents" from the context menu. Then drag the executable "lazarus" to the folder "Contents/MacOS" of the bundle. That's it. 

# Legacy Information

### Install gdb

As of the Lazarus 2.0.0 release this is entirely optional. The IDE now supports LLDB directly. See <http://forum.lazarus-ide.org/index.php/topic,42869.0.html> and <http://wiki.lazarus.freepascal.org/Lazarus_2.0.0_release_notes#LLDB_based_Debugger_.28New.29>

Getting gdb to run can be a bit tedious but is worthwhile in the end if using Lazarus < V2.0 or have some specific requirement for gdb. You probably don't ! 

This step should be done after installing XCode and before installing FPC/Lazarus. 

  1. Install fink or brew and use them to install gdb- [http://wiki.freepascal.org/GDB_on_OS_X_Mavericks_and_Xcode_5](<GDB_on_OS_X_Mavericks_and_Xcode_5.md>)
  2. Self sign the gdb binary using tools already on your Mac - <https://sourceware.org/gdb/wiki/BuildingOnDarwin> This is the tedious part. The instructions are clear except, perhaps they could have mentioned that after you have set the Keychain to ’System’ you need click ‘Create’ and ‘Done’ on the next screen. Then go and find the cert in your overall list of certs to do the next instruction “using the contextual menu…”. Don’t worry about step “2.1.4 Disable starting the debuggee…”, we’ll address that once Lazarus itself is installed and working. 
  3. Once Lazarus is installed and fired up, tell it how it should start gdb, that is step 5. 



**Note** that as of April 2018, there may be issues with newest gdb and High Sierra. <https://forum.lazarus.freepascal.org/index.php/topic,40874.0.html> _According to<https://stackoverflow.com/questions/49001329/gdb-doesnt-work-on-macos-high-sierra-10-13-3>, gdb 8.1 does not work on High Sierra, but 8.0.1 does._

**Part 2 of gdb install, Tell Lazarus about starting gdb**

Again, this step is not necessary with Lazarus V2.0 and Later. See <http://wiki.lazarus.freepascal.org/Lazarus_2.0.0_release_notes#LLDB_based_Debugger_.28New.29> If you do install (and use) gdb, you must tell Lazarus about how to call gdb. Obviously, this step happens after Lazarus is installed. 

OK, now you should be able to start Lazarus, it should display the various parts and paths it needs (including gdb) in the little opening screen. You will get an alert there if you chose to not use a debugger. Proceed to the IDE and we now need to tell Lazarus to tell gdb to not open a shell, as noted by a number of Forum people (<http://forum.lazarus.freepascal.org/index.php/topic,37310.0.html>). Within Lazarus, click Tools->Options->Debugger->General In the “Debugger specific Options” there is a field labeled Debugger_Startup_Options. Type in "--eval-command=set startup-with-shell off" (including the inverted commas) AND press the return key, click OK. Note you cannot copy and paste into this box (##$!). See screen shot. 

  * [![Debug Startup Options 01.png](https://wiki.freepascal.org/images/6/6e/Debug_Startup_Options_01.png)](</File:Debug_Startup_Options_01.png>)




  


### Xcode Compatability

Information relating to older versions of FPC, Lazarus and Apple Developer Tools 

#### Compatibility with Xcode 5.0 and newer versions (Mac OS X 10.8, 10.9, 10.10 and 10.11)

Like Xcode 4.3+, Xcode 5 and subsequent versions no longer install the command line tools by default. These are required by FPC/Lazarus, however. Unlike in Xcode 4 the installation option is no longer accessible in Xcode 5 via the Downloads tab. Additionally, the "Locations" tab of the preferences dialog may be confusing, as it mentions that the command line tools are installed: 

  * [![](https://wiki.freepascal.org/images/c/cf/xcode5_misleading_screenshot.png)](</File:xcode5_misleading_screenshot.png>)

Default location of command line tools in Xcode 5. 

  * [![](https://wiki.freepascal.org/images/7/75/xcode_select.png)](</File:xcode_select.png>)

Installation of command line tools with the xcode-select command part 1... 

  * [![](https://wiki.freepascal.org/images/1/1e/command_line_tools_installation_on_mavericks.png)](</File:command_line_tools_installation_on_mavericks.png>)

...and part 2. 




The first dialog box shows that by default the command line tools are only installed inside the Xcode.app bundle itself. To install them globally, as required by FPC/Lazarus, execute the following terminal command: 
    
    
    xcode-select --install

Depending from your Xcode version alternative steps to install the command line developer tools may be to download them directly in the Xcode preferences window (see screenshots above) or to download them from [developer.apple.com](<http://developer.apple.com>) (please take care to install the command line tools that are correct exactly for your Xcode version). 

  * You can compile and install the normal gdb. See [GDB on OS X Mavericks and Xcode 5](<GDB_on_OS_X_Mavericks_and_Xcode_5.md> "GDB on OS X Mavericks and Xcode 5"). 



#### Xcode 4.3+ compatibility (Mac OS X 10.7/10.8)

Xcode 4.3 and later no longer install the command line tools by default. These are required by FPC/Lazarus. 

To install them manually, open Xcode, go to Preferences, select "Downloads;" and install the "Command Line Tools". Afterwards, FPC will install and function correctly. 

  * [![install command line tools 1.png](https://wiki.freepascal.org/images/a/a5/install_command_line_tools_1.png)](</File:install_command_line_tools_1.png>)

  * [![install command line tools 2.png](https://wiki.freepascal.org/images/d/dd/install_command_line_tools_2.png)](</File:install_command_line_tools_2.png>)

  * [![install command line tools 3.png](https://wiki.freepascal.org/images/d/d0/install_command_line_tools_3.png)](</File:install_command_line_tools_3.png>)




It will be necessary to reinstall the command line tools after each upgrade of Xcode (especially when it's a new major version, the installer may delete the old copies of the command line tools), and also after switching to a new major version of the system software (idem). You may also download the command line tools from [Apple Developer](<https://developer.apple.com/download/more/?=command%20line%20tools>). 

#### Xcode 3.2.x compatibility (Mac OS X 10.6)

FPC 2.6.0: there is an issue when compiling dynamic libraries with FPC under Mac OS X 10.6 due to a bug in the Xcode 3.2.x linker. This bug has been fixed in Xcode 4. You can work around the bug in Xcode 3.2.x by using the -k-no_order_inits command line parameter when compiling a dynamic library. 

#### Xcode 3.1.x compatibility (Mac OS X 10.5)

Most versions of Lazarus and FPC are compatible with Xcode 3.1 (see the compatibility matrix for detailed reference). The Xcode integration kit is provided as an optional install to allow for developing with Free Pascal directly from Xcode. It is not required for development with Lazarus, however. 

#### Xcode 2.x compatibility (Mac OS X 10.4)

Most versions of Lazarus and FPC are compatible with Xcode 2.3 or newer (see the compatibility matrix for detailled reference). The Xcode integration kit is provided as an optional install to allow for developing with Free Pascal directly from Xcode. It is not required for development with Lazarus, however. Xcode 2.0 was compatible with Lazarus 0.9.x, but it is no longer supported by newer versions of the Lazarus IDE. 

## Install Packages / Released version

### Installation from disk images

Download the three disk images (.dmg files) for fpc, fpcsrc and lazarus from either of the following links: 

  * [Intel-based Macs](<http://sourceforge.net/projects/lazarus/files/Lazarus%20Mac%20OS%20X%20i386/>)
  * [PowerPC-based Macs](<http://sourceforge.net/projects/lazarus/files/Lazarus%20Mac%20OS%20X%20powerpc/>)



Open up each disk image and install in this order: 

  1. fpc 
  2. fpcsrc 
  3. Lazarus 



After installation the Lazarus application can be found in /Developer/lazarus/, the FPC source files in /usr/local/share/fpcsrc. 

If you receive a "Can't find unit Interfaces used by Project1" error on trying to compile a blank form, check the following settings in Lazarus (should be set by default): 

Environment Options 
    
    
      Lazarus directory: /Developer/lazarus
      Compiler path: /usr/local/bin/ppc386 (PowerPC Macs: /usr/local/bin/ppcppc)
      FPC Source: /usr/local/share/fpcsrc
    

Project | Options 
    
    
      All paths blank
      LCL Widget Type: default (Carbon beta)
    

Project | Inspector 
    
    
      Required Packages
         LCL
    
    

Note - different versions of Lazarus depend on particular versions of the FreePascal compiler and will not work with other versions. 

Another common problem is that the versions of fpc and fpcsrc are different. 

This is the easiest way to install Lazarus on Mac OS X. 

## Install from Source / Development version

You need the latest stable released FPC installed in order to compile the development version. 

### Download and install a compiler

Download and install the FPC package: <https://sourceforge.net/project/showfiles.php?group_id=89339>

There are two development versions of the compiler: 2.6.x is stable version without new features - only bug fixes. The unstable version 2.7.x comes with lots of new features but sometimes also with bugs. Best is to download and install fpc 2.6.x. Some daily snapshots can be found [here](<http://snapshots.lazarus.shikami.org/lazarus/>). Keep in mind that these are daily snapshots and that you can have bad luck and get a buggy version. The probability is about 1:30. So if the version is buggy try another day or use the released packages instead. 

# Uninstalling Lazarus and Free Pascal

## Installed using fink

The complete uninstall of all fpc and lazarus packages is done with: 
    
    
    fink purge --recursive fpc-config

If you want to preserve changes to the preference file /sw/etc/fpc.cfg, do: 
    
    
    fink remove --recursive fpc-config

For the removal of the Lazarus preference folder and files in your home directory, see below ( Lazarus preference folder). 

## Installed from disk images or sources

Normally you uninstall an application on OS X simply by dragging it from the Applications folder to the trash. But because Lazarus and Free Pascal are development tools, they're installed in several folders that you don't normally see in Finder. 

You can copy and save the commands below to file uninstallLaz.sh and run it if you need to uninstall Lazarus and Free Pascal. You can usually install newer versions of Lazarus and Free Pascal over older versions, but as with most software it's not a bad idea to clean out everything before you install a newer version. This list of commands should also give you a sense of where the various pieces of Lazarus and Free Pascal are located. 
    
    
    bin=/usr/local/bin
    share=/usr/local/share
    lib=/usr/local/lib
    receipts=/Library/Receipts
    private=/private/etc
     
    rm -fv $bin/ppcppc
    rm -fv $bin/ppc386
    rm -fv $bin/bin2obj
    rm -fv $bin/data2inc
    rm -fv $bin/delp
    rm -fv $bin/fd2pascal
    rm -fv $bin/fpc
    rm -fv $bin/fpcmake
    rm -fv $bin/fpcmkcfg
    rm -fv $bin/fpcsubst
    rm -fv $bin/fpdoc
    rm -fv $bin/fprcp
    rm -fv $bin/h2pas
    rm -fv $bin/h2paspp
    rm -fv $bin/makeskel
    rm -fv $bin/mkxmlrpc
    rm -fv $bin/plex
    rm -fv $bin/postw32
    rm -fv $bin/ppdep
    rm -fv $bin/ppudump
    rm -fv $bin/ppufiles
    rm -fv $bin/ppumove
    rm -fv $bin/ptop
    rm -fv $bin/pyacc
    rm -fv $bin/rstconv
    rm -fv $bin/unitdiff
    rm -r $private/lazarus
    rm -r $bin/lazarus
    rm -r $share/lazarus
    rm -r $share/fpcsrc
    rm -r $share/doc/fpc-2.2.2
    rm -r $share/examples/fpc-2.2.2
     
    rm -r $lib/fpc
     
    rm -r $receipts/lazarus-*.pkg
    rm -r $receipts/fpcsrc-*.pkg
    rm -r $receipts/fpc-*.pkg
     
    rm -fv /etc/fpc.cfg

Note that this assumes you have version 2.2.2 snapshot of Free Pascal installed. If you have a different version, change the two relevant commands to specify your version. 

To run this script, change to the directory where it's stored and enter: 
    
    
    chmod +x uninstallLaz.sh
    sudo ./uninstallLaz.sh

Then enter your password when prompted. 

### Lazarus preference folder .lazarus in the home directory

_**Note:**_ Lazarus also creates a .lazarus folder in your home directory where it stores environment settings and a list of recently opened projects and files. You can leave this folder alone if you want the new version of Lazarus you're installing to use your old settings. macOS normally doesn't show files or folders whose names start with a dot (.). To see this folder and its contents, enter the following in a terminal window: 
    
    
    cd ~/
    ls -al
    cd .lazarus
    ls

To remove it, execute the following commands: 
    
    
    cd ~/
    rm -rv .lazarus

# Known issues

  * FPC 2.4.4 has a bug. You can not compile the IDE with the range check flag **-Cr**. 
  * On OS X 10.4 you have to manually uninstall any previous version before installing a new dmg. Delete the following files and folders: 
    * /Developer/lazarus 
    * /Library/Receipts/lazarus.pkg 
    * /etc/lazarus 
    * /usr/local/bin/lazbuild 



# See Also

  * [Mac Installation FAQ](<Mac_Installation_FAQ.md> "Mac Installation FAQ")



## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues 
  * [Win32/64 Interface](<Win32/64_Interface.md> "Win32/64 Interface") \- The winapi interface for Windows 95/98/Me/2K/XP/Vista, but not CE 
  * [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones 
  * [Carbon Interface](<Carbon_Interface.md> "Carbon Interface") \- The Carbon interface for Mac OS X 
  * [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa interface for Mac OS X 
  * [Qt Interface](<Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, Mac OS X, Windows, and Linux-based PDAs 
  * [Qt5 Interface](<Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, Mac OS X, Windows, and Linux-based PDAs 
  * [GTK1 Interface](<GTK1_Interface.md> "GTK1 Interface") \- The gtk1 interface for Unixes, Mac OS X (X11), Windows 
  * [GTK2 Interface](<GTK2_Interface.md> "GTK2 Interface") \- The gtk2 interface for Unixes, Mac OS X (X11), Windows 
  * [GTK3 Interface](<GTK3_Interface.md> "GTK3 Interface") \- The gtk3 interface for Unixes, Mac OS X (X11), Windows 
  * [fpGUI Interface](<fpGUI_Interface.md> "fpGUI Interface") \- Based on the fpGUI library, which is a cross-platform toolkit completely written in Object Pascal 
  * [Custom Drawn Interface](<Custom_Drawn_Interface.md> "Custom Drawn Interface") \- A cross-platform LCL backend written completely in Object Pascal inside Lazarus. The Lazarus interface to Android. 



### Platform specific Tips

  * [OS X Programming Tips](<OS_X_Programming_Tips.md> "OS X Programming Tips") \- Lazarus installation, useful tools, Unix commands, and more... 
  * [WinCE Programming Tips](<WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Using the telephone API, sending SMSes, and more... 
  * [Windows Programming Tips](<Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips. 



### Mac How To Articles

  * [Mac How](<http://www.mac-how.net>) \- If you having troubles in solving some major issues. 



### Interfaces Development Articles

  * [Carbon interface internals](<Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface 
  * [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones 
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface 
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL 
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL

---

_Source: [https://wiki.freepascal.org/Lazarus_release_version_for_Mac](https://web.archive.org/web/20190522121612/https://wiki.freepascal.org/Lazarus_release_version_for_Mac)_
