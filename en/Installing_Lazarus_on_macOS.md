# Installing Lazarus on macOS

│ **English (en)** │  **[русский (ru)](<../ru/Installing_Lazarus_on_macOS.md>)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

Installing Lazarus on a Mac is not particularly difficult but it is critical that you do the install in the correct order. Skipping steps will almost certainly end in tears. In brief, here is what you need to do: 

  1. Download and install Xcode (optional depending on your needs - see below for details).
  2. Install the global command line tools for Xcode. Definitely required.
  3. Download and install the Free Pascal Compiler (FPC) binaries package and the FPC source package from the [Lazarus IDE file area](<https://sourceforge.net/projects/lazarus/files/>) (**Important** do this _before_ you install the Lazarus IDE)
  4. Download and install the Lazarus IDE from the [Lazarus IDE file area](<https://sourceforge.net/projects/lazarus/files/>) or, perhaps more useful, download and compile Lazarus source code, eg [Lazarus Fixes 3.0](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/tree/fixes_3_0?ref_type=heads>)
  5. Configure LLDB - the Apple supplied (and signed) debugger from within the Lazarus IDE.



These steps are explained in more detail below. 

## Contents

  * 1 Installation
    * 1.1 Step 1: Download Xcode (optional)
    * 1.2 Step 2: Xcode Command Line Tools
    * 1.3 Step 3: FPC binaries
    * 1.4 Step 4: Binary Install of the Lazarus IDE
    * 1.5 Step 5: Configure the Debugger
      * 1.5.1 Extra Information on using lldb
      * 1.5.2 Installing LazDebuggerFpLLdb
  * 2 Installing Lazarus from Source
    * 2.1 Downloading Lazarus source
    * 2.2 Building source
    * 2.3 Setup a Config Directory Structure
    * 2.4 Starting Lazarus
    * 2.5 What does the bigide make argument do?
    * 2.6 Managing your Config Directory
  * 3 Installing non-release versions of FPC
    * 3.1 Installing from source
      * 3.1.1 FPC development version
      * 3.1.2 FPC Fixes 3.2
  * 4 Cocoa 64 bit vs Carbon 32 bit
  * 5 FPC + Lazarus Compatibility Matrix
  * 6 Known issues and solutions
    * 6.1 Lazarus IDE v2.2.0 - rebuilding issue
    * 6.2 Rebuilding errors do not happen on Intel computers
    * 6.3 Lazarus IDE - Unable to "run without debugging"
    * 6.4 Upgrading from Mojave (10.14) to Catalina (10.15)
    * 6.5 Building the FPC compiler from Mojave (10.14) onwards
    * 6.6 Mac Installation FAQ
  * 7 Uninstalling Lazarus and Free Pascal
  * 8 Legacy Information
  * 9 See also
  * 10 External links



## Installation

The detailed instructions assume a recent version of macOS on your Mac, a recent version of Xcode and/or Xcode command line tools from Apple and a recent version of Lazarus. On the Legacy Information page, you will see older information that may be relevant if you are using older components. You can assist by replacing out of date information, either deleting it, or, if it may help someone working with a legacy project, moving it to the [legacy information](<Legacy_Information__Installing_Lazarus_on_Mac.md> "Legacy Information: Installing Lazarus on Mac") page. 

In general, this is about using both the **Carbon** and **Cocoa** Widget Sets. While Carbon was once seen as a little more stable, with the release Lazarus 2.0.8 the 64 bit Cocoa Widget Set has now surpassed Carbon and should be considered seriously for any new projects. Carbon was intentionally limited to 32 bits by Apple and you should be aware that it and the ability to run 32 bit executables have been removed completely from macOS 10.15 Catalina and later versions which now only support 64 bit executables using Cocoa. 

### Step 1: Download Xcode (optional)

Xcode is a 12GB download which will take up 16GB of disk space. You **only** need to download and install the full Xcode development environment **if** you need: 

  * the SDKs for **iOS** , **iPadOS** , watchOS and tvOS; or
  * to validate and upload apps to the Mac App Store; or
  * to [notarise](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+") apps for distribution outside of the Mac App Store.



Xcode 11.3.1 for use on macOS 10.14 Mojave must now be installed by downloading it from [Apple Developer Connection](<http://developer.apple.com/>) (ADC), which requires free registration. Xcode 12.4.x for use on macOS 10.15 Catalina and later versions can be installed from the [Mac App store](<https://apps.apple.com/us/genre/mac-developer-tools/id12002?mt=12>). Note that you must first move any old Xcode versions from the Applications folder into the trash or rename the Xcode app (eg Xcode.app to Xcode_1014.app). You can then select which version of Xcode to use with the command line utility `xcode-select`. Open an Applications > Utilities > Terminal and type `man xcode-select` for this utility's manual page. 

**Older systems:**

The Xcode developer tools can be installed from the original macOS installation disks or a newer copy downloaded from the [Apple Developer Connection](<http://developer.apple.com/>) (ADC), which requires free registration. Download the Xcode file, it will end up in your Downloads directory as a zip file. Click it. It is unarchived into your Downloads directory. You may be happy with it there but maybe not. Other users will see the path to it but be unable to use it. And it is untidy there. So I moved mine and then told xcode-select where it was moved to (in a terminal) - 
    
    
    mv Downloads/Xcode.app /Developer/.
    sudo xcode-select -s /Developer/Xcode.app/Contents/Developer
    

### Step 2: Xcode Command Line Tools

This is shown here as a separate step because it really is a separate step. Don't confuse these standalone command line tools with the internal Xcode command line tools that the Xcode GUI will tell you are already installed if you installed the full Xcode package in Step 1. FPC cannot use those Xcode internal command line tools without configuration changes (see [Xcode](<Xcode.md> "Xcode") for details). 

Do the following, it is quick and easy for all macOS versions up to and including Catalina 10.15: 
    
    
    sudo xcode-select --install
    sudo xcodebuild -license accept
    

For Big Sur 11.x and later, you only need to enter the first of the two commands above unless you have also installed the full Xcode package. If you have only installed the command line tools, you should omit entering the _xcodebuild_ command. 

If you have problems installing the command line tools using this command line method (eg the installer freezes while "finding Software"), you can also download and install the command line tools package by logging in to the [Apple Developer Site](<https://developer.apple.com/download/more/?=for%20Xcode>) and downloading and installing the _Command Line Tools for Xcode_ disk image. 

### Step 3: FPC binaries

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** For Apple Silicon/AArch64/M1 installations, if you are installing FPC 3.2.2 (with Lazarus 2.2.0 or later) you don't need to compile a native Apple Silicon version of FPC because FPC 3.2.2 for macOS is a universal binary containing both Intel and aarch64 executables. If you are installing a version of FPC earlier than 3.2.2, then please refer to [these instructions](<macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers") for building a native Apple Silicon Free Pascal Compiler after installing the 64 bit Intel binary and source packages for FPC.

Download and install the official FPC binaries package from the [Lazarus IDE file area](<https://sourceforge.net/projects/lazarus/files/>). 

When you arrive at the Lazarus IDE file area: 

  1. Choose the correct version of your operating system. Intel Macs need the files from the **Lazarus macOS x86-64** directory, while Apple M1+ Macs need the ones from **Lazarus macOS aarch64**. Every Mac computer since late 2006 has been 64 bit capable and Apple has completely dropped all 32 bit support from macOS 10.15 Catalina and beyond.
  2. Download and install the _fpc-3.2.2.intelarm64-macosx.dmg_.



These installation packages are built by the FPC/Lazarus developers and track formal releases. As these installation packages are not approved by Apple, you need to hold down the _Control key_ , click the package and choose _Open_ and confirm you want to install from an _Unknown Developer_. 

You might like to try a simple and quick test of FPC at this stage - [Testing FPC installation](<Installing_the_Free_Pascal_Compiler.md> "Installing the Free Pascal Compiler"). 

### Step 4: Binary Install of the Lazarus IDE

It is critical that a compatible Free Pascal Compiler (FPC) be installed _before you install_ the Lazarus IDE. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** there are two versions of this step. The first is to install a binary package that someone else has already compiled for you. As Lazarus evolves, especially so on the Mac, that may be a confining limitation. The second approach is to compile your own, you already have FPC installed and obviously intend to compile your own project, why not do the same for Lazarus ? This will get you access to the newer bug fixes, LCL code in your own disk space (avoiding many "read only" errors) and give you control of whats happening.

**Binary Install** Download and unzip the Lazarus IDE from the [Lazarus IDE file area](<https://sourceforge.net/projects/lazarus/files/>). When you arrive at that file area, choose the correct version of your operating system. Intel Macs need the files from the **Lazarus macOS x86-64** directory, while Apple M1+ Macs need the ones from **Lazarus macOS aarch64**. Since this is a downloaded zip it has been marked as unsafe. You must unmark the zip with a terminal command: 
    
    
    xattr -cr lazarus-darwin-aarch64-4.0.zip
    

If you forgot the above xattr step, you will get the message _“lazarus.app” is damaged and can’t be opened. You should move it to the Bin."_

[![MacErrorStartLazarusForgotChattr.png](https://wiki.freepascal.org/images/f/fe/MacErrorStartLazarusForgotChattr.png)](</File:MacErrorStartLazarusForgotChattr.png>)

**Source Install** For installing from source, please refer to the Installing Lazarus from Source instructions below to build a native Lazarus IDE. This applies to both x86_64 and Apple Silicon/aarch64 systems. You can use this approach to install a release version, a Fixes version or the development version, "main". 

### Step 5: Configure the Debugger

In versions of Lazarus 1.8.4 and earlier, you needed to use `gdb` as a debugger, slow to install and hard to sign. Since Lazarus 2.0.0 you can (and should) use `lldb`, a debugger provided by Apple, no signing required. 

Assuming you have installed what is necessary and started Lazarus, all that remains is configuring the debugger. If you don't do this now, Lazarus will try to use `gdb` and fail. 

  * Lazarus version 2.2.0: First, click Lazarus > Preferences > Debugger - Debugger Backend;


  * Lazarus before version 2.2.0: First, click Tools > Options > Debugger.



Top right of the window now open has a label, "Debugger type and path", you must set both the type and the path. Select the type "LLDB debugger (with fpdebug) (Beta)" and select or type the path `/usr/bin/lldb`. 

[![Set Debugger2.png](https://wiki.freepascal.org/images/d/d5/Set_Debugger2.png)](</File:Set_Debugger2.png>)

If "LLDB debugger (with fpdebug) (Beta)" is not shown in the selection list, see Installing LazDebuggerFpLLdb below. The XCode command line tools install `lldb` to the `/usr/bin` directory. Save those settings and you can now try to compile the almost nothing program that Lazarus has kindly provided for you (click the small green triangle near top left). 

Next you see a puzzling question, see image below. Choose a "Debug Format" from one of those offered - 

[![Set Dwarf.png](https://wiki.freepascal.org/images/0/03/Set_Dwarf.png)](</File:Set_Dwarf.png>)

Martin_fr, the person who has given us this interface between the Lazarus IDE and `lldb`, suggests you use **"dwarf3"**. Then, you need to enter your password, a macOS cuteness because one application appears to be interfering with another. In this case, that's fine! 

When running the debugger from time to time you will be asked for your password to allow debugging. This is for your own safety. 

#### Extra Information on using lldb

A great deal of information about using lldb appears in [this forum thread](<https://forum.lazarus.freepascal.org/index.php/topic,42869.0.html>). Here are a few gems, again, from Martin_fr: 

In the unexpected case of problems, it may be worth trying "dwarf with sets" instead of just "dwarf3". 

The "debug info" setting only affects the units directly in your project. However, units in packages may have debug info too. This can be: 

  * set per package
  * for many, but not all packages in the menu Tools > Configure "Build Lazarus"
  * Project > Project Options > [Additions and Overrides](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options")



If you change settings for a package, you might want to check which package you expect to step into when debugging. Packages you do not step into, do not need debug info. 

If you use a type from a package (such as TForm from LCL) it is enough that your unit (in which you declare the variable / must declare and use a variable to include the type) has debug info. Reducing the number of packages with debug info (including those that default to have debug info), can shorten the debuggers start-up time. 

Also it may be worth comparing (it has not been tested) the debuggers start up time for the same settings, only changing the checkbox "use external debug info". 

This needs to be only set in your project. If set in your project it will affect all packages. (If set in a package it should have no affect.) 

#### Installing LazDebuggerFpLLdb

If you installed from source and used the `bigide` parameter to `make`, then the correct debugger will be installed, as a package, and ready to go. If, however, you installed in another way, it may, or may not be there. 

From the main IDE screen, click Packages->Install/UninstallPackages. Shown is two lists of packages, the list on the left is installed, the list on the right is available to install. Look for LazDebuggerFpLldb (exactly that, there are some similar named but less suitable packages). If it is on the right hand side, click it, press "Install Selection" and then "Save and rebuild IDE". This will take a little time, the IDE will shutdown and restart and all should be well. Now jump back up the page and continue configuring the debugger. 

## Installing Lazarus from Source

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** When building Lazarus from source, Apple very kindly add a quarantine attribute to your *.app files that ensures they will not run. **xattr -drv com.apple.quarantine *** will solve this for you. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This process can be seen as an alternative to step 4 above, you still need to install Xcode Command Line Tools and Free Pascal Compiler and its Source.

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Mac users are advised not to build Lazarus from the 4.0RC2 release candidate source. There is a Mac specific bug, fixed in Trunk and almost certainly will be fixed in an RC3 or full release, which ever comes first. <https://forum.lazarus.freepascal.org/index.php/topic,70214.msg548936.html#msg548936> . Someone, please remove this note sometime after the next Lazarus Release.

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** In the first quarter of 2025, it was noted that Macs running Sequoia and aarch64 processors failed to link (when building an app or Lazarus itself). A temporary solution is to force FPC to build a binary suitable for earlier MacOS (not a bad thing anyway). Use -WM10.15 as an option to FPC. See <https://forum.lazarus.freepascal.org/index.php/topic,70698.msg553691.html#msg553691>

This chapter focuses of Fixes but the same process applies to Lazarus Main (ie the development version) and other branches or tags in gitlab. At present (November 2023) Fixes_3.0 makes sense and examples below assume that. Later, newer version numbers may be needed. 

There are a number of reasons why you may be better off building from Source (particularly, in this case, Fixes_3.0). 

  * It gives you access to the 'newest and best' or, perhaps, the 'bleeding edge'. Fixes and main are not usually available as binary installs.
  * The MacOS side of Lazarus changes rapidly, this is particularly so with respect to the debugger and Apple Silicon / aarch64. At the time of writing, for example, Fixes_3.0 has improvements to the debugger that are not present in the 3.0 Release Candidates.
  * Fixes of any version is usually a safer and more stable option than the development branch, but still gets the new features much faster than the release version.
  * [Fixes included in fixes 3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch")



  


  * When working with an Apple Silicon M1 processor Mac ensure you use the FPC 3.2.2 or later as it provides a universal binary with both a native aarch64 and Intel compiler. If you must use an earlier FPC than 3.2.2 see [Apple Silicon Support](<macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers") for how to do this. When building the Lazarus IDE, change the **CPU_TARGET** in the instructions below from **x86_64** (Intel 64 bit) to **aarch64** (ARM 64 bit - Apple Silicon M1).



#### Downloading Lazarus source

As always, you have several choices when it comes to downloading Lazarus Source. 

  * Browse to the [Lazarus source repository](<https://gitlab.com/freepascal.org/lazarus/lazarus>) and select the branch you are interested in. Download a zip or tar file. This is quick and (relatively) small and suits a 'one off' install.
  * Use git, its bigger and slower but a good solution if you are already a git user or expect to be sending in patches to contribute to Lazarus. In this case, you need main, Lazarus does not accept patches for other branches. Advantage of git (or svn) is that its easily and quickly updated and if you plan to track the frequent updates that happen (to non-release) versions its worthwhile. This wiki has good instructions on using [git](<FPC_git.md> "FPC git"). Typically `git clone <https://gitlab.com/freepascal.org/lazarus/lazarus.git>` to get Lazarus Main.
  * If you have wget installed and know the url you want, its all too easy. Some examples, firstly the current (Nov 2023) Fixes_3.0 and then main.


    
    
    $> wget  https://gitlab.com/freepascal.org/lazarus/lazarus/-/archive/fixes_3_0/lazarus-fixes_3_0.zip
    $>
    $> wget https://gitlab.com/freepascal.org/lazarus/lazarus/-/archive/main/lazarus-main.zip
    

The zip or tarballs are about 60Meg, git (or svn) bigger because they hold all the history as well. In all cases, ensure your source ends up in a directory by it self, remember you may end up with several installs so make a reasonable directory structure. This (very un Mac like) author keeps such things in (eg) $HOME/bin/Lazarus/lazarus-fixes_3.0. 

#### Building source

Assuming some things here, you may make different choices, adjust accordingly - 

  * you have Lazarus Fixes source (from either tar, zip or git)
  * you have the directory structure mentioned above
  * you are using an x86_64 box (use CPU_TARGET=aarch64 otherwise)
  * you want to keep your Lazarus configs in their own specified place (essential if you will have multiple installs, a very good idea other wise).


    
    
    $> cd bin/Lazarus/lazarus-fixes_3.0
    $> make clean all LCL_PLATFORM=cocoa CPU_TARGET=x86_64 bigide
    //-wait some time...
    $> xattr -drv com.apple.quarantine *
    

If this is a rebuild and all you have downloaded is an update to a not too old version, you can restore your installed packages and configuration by using "make useride" after a successful "make clean ..." step, possibly even instead off if its recent enough. Does no harm if you fail. 

  * The xattr command removes a quarantine attribute from your app directories kindly added by Apple to prevent them from working.


  * On older macOS versions supporting 32 bit applications, replace the above make line with "_make LCL_PLATFORM=carbon CPU_TARGET=i386 bigide_ " and set up your project as mentioned in the Carbon and Cocoa section below.



#### Setup a Config Directory Structure

This step is not necessary if you will be happy with just this one Lazarus install. Lazarus will make a config directory if it needs to but will not make any directories about that. So, here we make a single directory to hold all our Lazarus configs. Before we do that, while we are still in the lazarus-fixes_3.0 directory, we create a lazarus.cfg file that will tell this Lazarus where to store its config. You do this only once for each Lazarus install. 
    
    
    $> cat > lazarus.cfg <enter>
    --pcp='~/bin/Lazarus/LazConfigs/lazarus-fixes_3.0 <enter>
    Press Ctrl D
    
    $> mkdir ../LazConfigs
    

#### Starting Lazarus

You must start Lazarus using the "app bundle", that is, typically startlazarus.app or lazarus.app in this case. Don't use the lazarus or startlazarus binary, described in the Finder as 'unix binaries'. If you get that wrong, directly starting a Lazarus binary will result in no menu or mouse interaction. Its an Apple thing. 

  * **From the command line** that means for example 'open lazarus.app'. You will probably cd to the directory containing the Lazarus install and just type `open ./lazarus.app` or, if you want pass the name of a Config Dir and start from anywhere, something like `open startlazarus.app --args "--pcp=~/bin/Lazarus/LazConfigs/lazarus-fixes_3.0"`
  * **Using the Finder** is, perhaps, a bit more "mac like". Navigate to the directory where your Lazarus install is, double click on the lazarus.app (or startlazarus.app) entry and all is good. However, by default, the Finder does not show the ".app" part of the name, its hard to tell the difference between "lazarus" and "lazarus.app". You want the one with a caricature of a leopard's face, Finder describes it as an Application.


  * Thirdly, depending on how you like to work, maybe **make a bash script to start Lazarus** ? Note I pass a parameter to define the config directory, that is unnecessary if you have put the suggested lazarus.cfg file in place.


    
    
    #!/bin/bash
    LAZDIR="lazarus-fixes_3.0"
    cd ~/bin/Lazarus/"$LAZDIR"
    open ~/bin/lazarus/"$LAZDIR"/lazarus.app --args "--pcp=~/bin/Lazarus/LazConfigs/$LAZDIR"
    

### What does the bigide make argument do?

The _bigide_ `make` argument adds a bunch of packages to Lazarus that many find useful and cannot do without. The packages that are added are: 

  * cairocanvas
  * chmhelp
  * datetimectrls
  * externhelp
  * fpcunit
  * fpdebug
  * instantfpc
  * jcf2
  * lazcontrols
  * lazdebuggers

| 

  * lclextensions
  * leakview
  * macroscript
  * memds
  * onlinepackagemanager
  * pas2js
  * PascalScript
  * printers
  * projecttemplates
  * rtticontrols

| 

  * sdf
  * sqldb
  * synedit
  * tachart
  * tdbf
  * todolist
  * turbopower_ipro
  * virtualtreeview

  
---|---|---  
  
The above list is sourced from the _`[Lazarus source directory]/IDE/Makefile.fpc`_ and may be subject to change. 

Note that if you have not compiled your own Lazarus IDE with the _bigide_ argument, you can install any of these packages yourself using the Lazarus IDE [_Package > Install/Uninstall Packages..._](<IDE_Window__Installed_Packages.md> "IDE Window: Installed Packages") dialog. 

  


### Managing your Config Directory

Further up, in this section, we mentioned the Lazarus Config Directory. Maybe a few words about it here will help, especially if you are having some problems. 

  * The config dir can have any name you like but, as is frequently the case with low level stuff, best not to have spaces in the directory name nor in the path to it. In practise, its a good idea to give the config dir a name that hints at the install of Lazarus that uses it.
  * The config dir can be anywhere you like as long as you have write access to it.
  * The config dir must be used by only one install of Lazarus. If you install a new version of Lazarus, not too far from your old one, Lazarus will offer to upgrade it but there is no going back to the old one.
  * The config dir holds a whole lot of information about what you are doing with Lazarus. For example, things you set when Lazarus first started up, Options such as tab spacing, where the help files are and, importantly, any package you may have installed. For users who have Lazarus (initially) installed in read only disk space, Lazarus keeps anything of itself that has been altered in the config dir too. (That's just one reason its better to build from source.)
  * Given the above point, if you remove the Lazarus Config Directory, its a bit like doing a factory reset on your smart phone. All your settings and additions will be discarded and Lazarus will startup fresh, will want to know where things like fps are, what debugger you are using etc, fortunately, new versions of lazarus are pretty good at finding sensible defaults, older version might need a bit of help.



Ok, so, the Lazarus Config directory is clearly important and probably best not fiddled with by the inexperienced but it is a quite reasonable thing to just delete it to go (right) back to a known good install. 

  * If you have **selected a config directory of your own** (with a lazarus.cfg file or a command line parameter) then you know where it is and you can delete it using either the Finder or Command Line.
  * If you have **allowed Lazarus to choose** where to put the config dir then Lazarus will make and use a directory called .lazarus in your home directory. The dot makes it a hidden directory, both the Finder and ls command will ignore it unless instructed to display hidden files. But you don't need to see it, you know it is in your home directory, delete it with -



`rm -Rf ~/.lazarus <enter>`

I am sure I don't need to be warn about the danger of the rm command but I will, be careful ! Never use `rm -Rf` without an explicit directory name to delete ! 

## Installing non-release versions of FPC

### Installing from source

There are two current non-release branches of the Free Pascal Compiler: the development (trunk) branch and the Fixes 3.2 branch which includes additional fixes to the released 3.2.2 version. Developers, and those who like living on the bleeding edge and testing new features and fixes, will choose the development version; more normal users, who wish to use a stable branch with some additional fixes since the latest release version, will choose the Fixes branch. The instructions below cover both these branches. 

The source code is kept in a version control system called _git_ : 

  * macOS 10.5 and higher already contain a command line git client if you have installed the Xcode command line utilities.


  * You also need the latest released Free Pascal Compiler version (3.2.2 as of March 2022) installed in order to be able to successfully compile the development (trunk) version.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** **When building a native aarch64 Free Pascal Compiler (ppca64) for an Apple Silicon M1 processor Mac, change the CPU_TARGET in the instructions below from x86_64 (Intel 64 bit) to aarch64 (ARM 64 bit - Apple Silicon M1) and change any reference to ppcx64 to ppca64**

#### FPC development version

  * [FPC New Features in development](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")
  * [FPC User Changes in development](<User_Changes_Trunk.md> "User Changes Trunk") \- may break existing code.



Note that since the development version of FPC (was known as "trunk" in SVN; now known as "main" in GIT) is by definition still under development, some of the features may still change before they end up in a release version. 

Create a directory where you would like to put the source (eg fpc_main in your home directory). You don't need to be root to do this. Any normal user can do this. Open an Applications > Utilities > Terminal and do the following: 
    
    
     []$ git clone -b main https://gitlab.com/freepascal.org/fpc/source.git fpc_main
    

This will create a directory called `fpc_main` and download the FPC main source to it. 

To subsequently update your local source repository with the latest source changes you can simply do: 
    
    
     []$ cd
     []$ cd fpc_main
     []$ git clean -f -d
     []$ git pull
    

To build and install FPC (the highlighted text should be all on one line): 
    
    
     []$ cd
     []$ cd fpc_main
     []$ make distclean all FPC=/usr/local/lib/fpc/3.2.2/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64
    

You will also need to update the links for the compiler in `/usr/local/bin` which will be pointing to the previous FPC version. For example: 
    
    
     []$ cd /usr/local/bin
     []$ sudo rm ppc386
     []$ sudo rm ppcx64
     []$ sudo ln -s /usr/local/lib/fpc/3.3.1/ppc386
     []$ sudo ln -s /usr/local/lib/fpc/3.3.1/ppcx64
    

Note that you will need to build a new `ppc386` compiler if you want to continue to compile 32 bit applications by replacing these lines (this is not be possible after Xcode 11.3.1 and macOS 10.14.6 Mojave because of Apple's removal of the 32 bit frameworks): 
    
    
     []$ make clean all FPC=/usr/local/lib/fpc/3.2.2/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64
    

with these two lines: 
    
    
     []$ make clean all FPC=/usr/local/lib/fpc/3.2.2/ppc386 OS_TARGET=darwin CPU_TARGET=i386 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppc386 OS_TARGET=darwin CPU_TARGET=i386
    

#### FPC Fixes 3.2

Create a directory where you would like to put the source (eg `fpc_fixes32` in your home directory). You don't need to be root to do this. Any normal user can do this. Open an Applications > Utilities > Terminal and do the following: 
    
    
     []$ git clone -b fixes_3_2 https://gitlab.com/freepascal.org/fpc/source.git fpc_fixes32
    

This will create a directory called `fpc_fixes32` and download the FPC source to it. 

To subsequently update your local copy of the repository source with the latest source changes you can simply do: 
    
    
     []$ cd fpc_fixes32
     []$ git clean -f -d
     []$ git pull
    

To build and install FPC (the highlighted text should be all on one line): 
    
    
     []$ cd
     []$ cd fpc_fixes32
     []$ make distclean all FPC=/usr/local/lib/fpc/3.2.2/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64
    

You will also need to update the links for the compiler in `/usr/local/bin` which will be pointing to the previous FPC version. For example: 
    
    
     []$ cd /usr/local/bin
     []$ sudo rm ppc386
     []$ sudo rm ppcx64
     []$ sudo ln -s /usr/local/lib/fpc/fixes-3.2/ppc386
     []$ sudo ln -s /usr/local/lib/fpc/fixes-3.2/ppcx64
    

Note that you will need to build a new `ppc386` compiler if you want to continue to compile 32 bit applications by replacing these lines (this may not be possible after Xcode 11.3.1 and macOS 10.14.6 Mojave because of Apple's removal of the 32 bit frameworks): 
    
    
     []$ make clean all FPC=/usr/local/lib/fpc/3.2.2/ppc386 OS_TARGET=darwin CPU_TARGET=i386 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppc386 OS_TARGET=darwin CPU_TARGET=i386
    

## Cocoa 64 bit vs Carbon 32 bit

Using the Apple 64 bit Cocoa framework is now undeniably the future on the Mac. The Apple 32 bit Carbon framework, while no longer being developed, works pretty much as expected, but you are advised to try Cocoa first because Apple dropped all support for 32 bit applications and the 32 bit Carbon framework in macOS 10.15 Catalina (October 2019). 

It is possible to build a Carbon version of the Lazarus IDE (unless you are running macOS 10.15 Catalina or later) and use that to generate Cocoa 64 bit binaries. 

To produce Cocoa applications in the Carbon or Cocoa IDE, you need to set the Target to the 64 bit processor and select the Cocoa widgetset: 

  * Open your project with Lazarus and from the menu select Project > Project Options
  * In the "Config and Target" panel set the "Target CPU family" to be "x86_64" (Intel) or "aarch64" (Apple M1)
  * In the "Config and Target" panel, if the "Current LCL widgetset" is not set to Cocoa, click "Select another LCL widgetset" which will take you to the "Additions and Overrides" panel where you can click on "Set LCLWidgetType" pulldown and set the value to "Cocoa"
  * Make sure under Tools > Options (Lazarus > Preferences in v2.2.0 and later) that "Compiler Executable" is set to "/usr/local/bin/fpc" to get 64 bit applications.
  * Now compile your project - and please feed back any problems you experience.



  


## FPC + Lazarus Compatibility Matrix

Not every combination of Lazarus and Free Pascal is compatible with every installation of macOS. Please refer to the following table in order to find the correct version for your development environment: 

Lazarus Compatibility Matrix | Lazarus 1.8.x  | Lazarus 2.0.y  | Lazarus 2.0.8 | Lazarus 2.0.10  | Lazarus 2.0.12 | Lazarus 2.2.y   
---|---|---|---|---|---|---  
| FPC 3.0.4  | FPC 3.2.0  | FPC 3.2.2   
PPC processors   
Mac OS X 10.4 (Tiger)‡ | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible   
Mac OS X 10.5 (Leopard)‡ | Not tested | Not tested | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible   
Intel processors   
Mac OS X 10.4 (Tiger)‡ | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible   
Mac OS X 10.5 (Leopard)  | Not tested | Compatible^ | Not tested | Compatible^**† | Not tested | Not tested | Not tested   
Mac OS X 10.6 (Snow Leopard)  | Compatible | Compatible^^ | Not tested | Not tested | Not tested | Not tested | Not tested   
Mac OS X 10.7 (Lion)  | Compatible | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
OS X 10.8 (Mountain Lion)  | Compatible^^ | Compatible | Compatible**# | Compatible**# | Compatible**# | Compatible# | Compatible   
OS X 10.9 (Mavericks)  | Compatible^^ | Compatible | Compatible**† | Compatible**† | Not tested | Not tested | Not tested   
OS X 10.10 (Yosemite)  | Compatible^^ | Compatible | Compatible**† | Compatible**† | Not tested | Not tested | Not tested   
OS X 10.11 (El Capitan)  | Compatible^^ | Compatible | Compatible***† | Compatible† | Compatible† | Compatible† | Comptaible   
macOS 10.12 (Sierra)  | Compatible^^ | Compatible | Compatible***† | Compatible† | Compatible† | Compatible† | Compatible   
macOS 10.13 (High Sierra)  | Not tested | Compatible | Compatible***† | Compatible† | Compatible† | Compatible† | Comptaible   
macOS 10.14 (Mojave)  | Not tested | Compatible | Compatible***† | Compatible† | Compatible† | Compatible† | Compatible   
macOS 10.15 (Catalina)  | Not tested | Compatible | Compatible***† | Compatible† | Compatible† | Compatible† | Compatible   
macOS 11 (Big Sur)  | Not tested | Compatible | Compatible***† | Compatible† | Compatible† | Compatible† | Compatible   
macOS 12 (Monterey)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Compatible   
macOS 13 (Ventura)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Compatible   
macOS 14 (Sonoma)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Compatible   
Apple Silicon M series processors   
macOS 11 (Big Sur)  | Not tested | Not tested | Not tested | Compatible†† | Compatible†† | Compatible††† | Compatible*   
macOS 12 (Monterey)  | Not tested | Not tested | Not tested | Compatible†† | Compatible†† | Compatible††† | Compatible*   
macOS 13 (Ventura)  | Not tested | Not tested | Not tested | Compatible†† | Compatible†† | Compatible††† | Compatible*   
macOS 14 (Sonoma)  | Not tested | Not tested | Not tested | Compatible†† | Compatible†† | Compatible††† | Compatible*   
  
**x** = 0, 2 or 4; **y** = 0, 2, 4 or 6 

^ Carbon interface compiles - Cocoa does not. 

^^ [Restrictions](<GDB_on_OS_X_Mavericks_and_Xcode_5.md> "GDB on OS X Mavericks and Xcode 5") apply to debugging with [gdb](<GDB.md> "GDB"). 

* Lazarus 2.2.0 installs universal binaries for FPC 3.2.2, but an Intel Lazarus IDE binary which you can use or recompile the IDE from within itself for a native aarch64 version. 

** See [Installing Lazarus 2.0.8, 2.0.10 with FPC 3.2.0 for macOS 10.10 and earlier](<Legacy_Information__Installing_Lazarus_on_Mac.md> "Legacy Information: Installing Lazarus on Mac") for instructions. 

*** See [Installing Lazarus 2.0.8 with FPC 3.2.0 for macOS 10.11+](<Legacy_Information__Installing_Lazarus_on_Mac.md> "Legacy Information: Installing Lazarus on Mac") for instructions. 

# Cannot run without debugging in the IDE. Can run compiled application outside of the IDE. See [Issue #37324](<https://bugs.freepascal.org/view.php?id=37324>). Choose the gdb debugger, change timeout option to false or click through five "timeout" dialogs to run with debugging in the IDE. 

† Cannot "run without debugging" in the IDE. Can run compiled application outside of the IDE. See [Lazarus IDE - Unable to "run without debugging"](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS") for workaround. See [Issue #36780](<https://bugs.freepascal.org/view.php?id=36780>). 

†† You need to compile a native aarch64 version of FPC 3.3.1 (trunk) and Lazarus 2.0.12 from source to support an Apple Silicon M series processor. Refer to [these instructions](<macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers") for FPC and [these instructions](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS") for the Lazarus IDE. 

††† After installing FPC 3.2.2, you need to compile a native aarch64 version of Lazarus from source to support an Apple Silicon M series processor. Refer to [these instructions](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS") for compiling the Lazarus IDE. 

‡ See the [legacy version of this compatibility matrix](</Template:Compatibility_matrix_of_Lazarus_for_Mac_Legacy> "Template:Compatibility matrix of Lazarus for Mac Legacy") for recommended installs on very old versions of macOS. 

  


## Known issues and solutions

### Lazarus IDE v2.2.0 - rebuilding issue

Any attempt to rebuild the Lazarus IDE from within itself fails with permission errors because the files and directories are owned by a user with the id of 503 whereas most users will find that their id is 501. The solution is to open an Applications > Utilities> Terminal and: 
    
    
    cd /Applications
    sudo chown -R your_username Lazarus
    

### Rebuilding errors do not happen on Intel computers

The next error that occurs is that the resource compiler "fpcres" is not found. The solution is to create a `.fpc.cfg` (note the initial dot) file in your home directory and add these lines to it: 
    
    
    #include /etc/fpc.cfg
    -FD/usr/local/bin
    

so that "fpcres" can be found where it was installed. 

The next error complains of an unsupported target architecture `-Paarch64`, with the advice to invoke the "fpc" compiler driver instead - except that is what we are doing already. The solution is to go to Lazarus > Preferences > Compiler executable which is set to `/usr/local/bin/fpc` and change that to `/usr/local/lib/fpc/3.2.2/ppca64` instead. 

The IDE will now be successfully rebuilt but will not restart automatically. Trying to then start Lazarus from either the `startlazarus.app` icon or the `lazarus.app` icon in `/Applications/Lazarus` still results in the original Intel lazarus binary being launched. The correct aarch64 binary can be found in `~/.lazarus/bin/aarch64-darwin/lazarus`. So the final solution is to open an Application > Utilities > Terminal and: 
    
    
    mv /Applications/Lazarus/lazarus /Applications/Lazarus/lazarus.old
    mv  ~/.lazarus/bin/aarch64-darwin/lazarus /Applications/Lazarus/
    

You can then start the IDE with either the `startLazarus.app` or `lazarus.app` icons. When you do restart lazarus, it is worth returning to the Lazarus > Preferences > Compiler executable which is set to `/usr/local/lib/fpc/3.2.2/ppca64` and changing it back to `/usr/local/bin/fpc` so that you can compile both aarch64 and Intel binaries to, for example, make a [Universal Binary](<macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers") which will run on both Intel and aarch64 machines. 

### Lazarus IDE - Unable to "run without debugging"

If you are using the Lazarus IDE 2.0.10 and the _Run > Run without debugging_ menu option fails with a dialog similar to: 

[![run dialog fail.png](https://wiki.freepascal.org/images/1/19/run_dialog_fail.png)](</File:run_dialog_fail.png>)

then you need to patch the Lazarus 2.0.10 source code ([Issue #37324](<https://bugs.freepascal.org/view.php?id=37324>) and [Issue #36780](<https://bugs.freepascal.org/view.php?id=36780>)). Specifically, patch `../ide/main.pp` as below (unpatched lines shown first, patched lines shown second): 
    
    
     7243,7245c7243,7245
     <     if RunAppBundle
     <         and FileExistsUTF8(Process.Executable)
     <     and FileExistsUTF8('/usr/bin/open') then
     ---
     >     if RunAppBundle then
     >     //    and FileExistsUTF8(Process.Executable)
     >     //and FileExistsUTF8('/usr/bin/open') then
    

and recompile the Lazarus IDE. 

Alternatively, you can omit patching the source code and simply recompile Lazarus 2.0.10 with FPC 3.0.4. 

A similar patching exercise and recompiling, or simply recompiling with FPC 3.0.4, needs to be done for Lazarus 2.0.8 if it has been compiled with FPC 3.2.0. 

### Upgrading from Mojave (10.14) to Catalina (10.15)

  * Run `sudo xcode-select --install`



  * To enable Lazarus to locate the `crt1.10.5.o` file change in `/etc/fpc.cfg` the -Fl behind "#ifdef cpux86_64" from


    
    
    -Fl/Applications/Xcode.app/Contents/Developer/Toolchain...
    

to 
    
    
    -Fl/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/lib/
    

### Building the FPC compiler from Mojave (10.14) onwards

  * To find the crt1.10.5.o file when building a later Free Pascal Compiler version from source, you need to specify:


    
    
     OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
    

on the make command line because FPC ignores the `/etc/fpc.cfg` configuration file during builds of itself. 

### Mac Installation FAQ

  * See the [Mac Installation FAQ](<Mac_Installation_FAQ.md> "Mac Installation FAQ") for solutions to other common problems that may arise during (and after) installation of Lazarus and Free Pascal on macOS.



## Uninstalling Lazarus and Free Pascal

Refer to [Uninstalling Lazarus on macOS](<Uninstalling_Lazarus_on_macOS.md> "Uninstalling Lazarus on macOS") for uninstallation options. 

## Legacy Information

See [Legacy Information](<Legacy_Information__Installing_Lazarus_on_Mac.md> "Legacy Information: Installing Lazarus on Mac") for details of: 

  * Installing Lazarus on Mac OS X 10.4 (Tiger), Mac OS X 10.5 (Leopard), OS X 10.8 (Mountain Lion)
  * Installing Lazarus 2.0.8, 2.0.10 with FPC 3.2.0 for macOS 10.10 and earlier
  * Installing Lazarus 2.0.8 with FPC 3.2.0 for macOS 10.11+
  * Installing Lazarus on PowerPC-based Macs
  * Old Xcode versions
  * Installing the gdb debugger
  * Compatibility matrix for Lazarus 1.0.0 through 1.6.4 and FPC 2.6.0 through 3.0.2.



## See also

  * [Xcode](<Xcode.md> "Xcode").
  * [Other macOS installation options](<Other_macOS_installation_options.md> "Other macOS installation options").
  * [Installing multiple Lazarus versions](<Multiple_Lazarus.md> "Multiple Lazarus").
  * [Mac Portal](<Portal_Mac.md> "Portal:Mac") for an overview of development for macOS with Lazarus and Free Pascal.
  * [Mac Installation FAQ](<Mac_Installation_FAQ.md> "Mac Installation FAQ") for solutions to the most frequent problems that may arise during (and after) installation of Lazarus and Free Pascal on macOS.



## External links

  * [Lazarus daily development version download](<https://sourceforge.net/projects/macos-lazarus-snapshots/files/intel/>) (Intel x86_64)
  * [Lazarus + FPC daily development version downloads](<https://sourceforge.net/projects/macos-lazarus-snapshots/files/arm64/>) (M1, ARM64, aarch64)
  * [GIT Succinctly](<https://www.syncfusion.com/succinctly-free-ebooks/git>) \- Free Ebook (PDF, MOBI, EPUB) from SyncFusion.

---

_Source: [https://wiki.freepascal.org/Installing_Lazarus_on_macOS](https://web.archive.org/web/20250503214601/https://wiki.freepascal.org/Installing_Lazarus_on_macOS)_
