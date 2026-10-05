# Qt5 Interface

│ **English (en)** │  **[русский (ru)](<../ru/Qt5_Interface.md> "Qt5 Interface/ru")** │    
****

[![Qt logo.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Qt_logo_2013.svg/50px-Qt_logo_2013.svg.png)](</File:Qt_logo_2013.svg>)

This article applies to [Qt5 widgetset](</Category:Qt> "Category:Qt") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Introduction
  * 2 Windows
    * 2.1 Running and Deployment
  * 3 Linux
    * 3.1 Lazarus
    * 3.2 libqt5pas
    * 3.3 Replace your libQt5Pas ?
    * 3.4 Systems using Wayland
    * 3.5 Qt5 Colours
    * 3.6 Why Qt5 instead of GTK ?
  * 4 macOS (64-bit)
  * 5 Example Project
  * 6 Other Interfaces
    * 6.1 Platform specific Tips
    * 6.2 Interface Development Articles



## Introduction

This interface is based on Qt 5 (Qt 5.6.2 is tested). For documentation, fixes and download, go to [Qt Project](<http://qt-project.org/>) (Installers at [download 5.6.2](<https://download.qt.io/new_archive/qt/5.6/5.6.2/>)). Lazarus with Qt5 interface (qt5-lcl) can be used on Windows 32/64, Linux x32/x64/arm, macOS x64 (Cocoa). Qt5 widgetsets have been available in Lazarus from 1.8 stable release. 

Most current Linux distros have a suitable Qt5 in their standard repositories however, long term support releases have issues due to their age. Further, many distros, at any given point in time, may have an older version of libqt5pas than necessary. 

  


## Windows

A nice [howto](<https://github.com/zoltanleo/fpc_lazarus_notes/blob/main/README_ENG.md#installing_qt_lazarus>)by forum user zoltanleo (suitable for qt6 also). 

There is a _Q5Pas1.dll_ binary available. 

  * x86: <https://gitlab.com/freepascal.org/lazarus/binaries/-/raw/main/i386-win32/qt5/Qt5Pas1.dll?inline=false>



no 64-bit version yet. 

The build is based on MinGW, thus you want to use the MinGW Qt library (i.e. qt-opensource-windows-x86-mingw492-5.6.2.exe). 

If you need to build **cbindings** project, you need **MinGW**. You can use **MinGW** from the Qt package (qt-opensource-windows-x86-mingw492-5.6.2.exe) - it's an optional component of Qt package installation. 

### Running and Deployment

The project needs to be deployed with _Q5Pas1.dll_. 

For Qt5 .dlls that should be deployed, see Qt5 guides: <https://doc.qt.io/qt-5/windows-deployment.html>. 

For MinGW based package the following set of DLLs would be required to run a single FORM project. 

Those DLLs come with Qt5 package (can be found at C:\Qt\Qt5.6.2\5.6\mingw49_32\bin). 

    Qt5Core.dll
    Qt5PrintSupport.dll
    Qt5Widgets.dll
    Qt5Gui.dll
    Qt5Network.dll
    libstdc++-6.dll
    libwinpthread-1.dll
    libgcc_s_dw2-1.dll

Builds based on MSVC rather than MinGW might have different set of (non Qt5) dlls required. Note that [Qt5Pas1.dll](<https://gitlab.com/freepascal.org/lazarus/binaries/-/raw/main/i386-win32/qt5/Qt5Pas1.dll?inline=false>) is built against Qt5-5.6.2, but Qt5Pas1.dll can be used with any Qt5 > 5.6.2. 

## Linux

### Lazarus

[![](https://wiki.freepascal.org/images/4/46/SelectQt5.png)](</File:SelectQt5.png>)

[](</File:SelectQt5.png> "Enlarge")

Image of Qt5 selected

Lazarus "out of the box" on Linux uses and makes apps for GTK2. It is not marked as having a dependency on Qt5 so if you wish to make Qt5 apps or build a Qt5 version of Lazarus you probably will need to install _libQt5Pas-dev_ , Using your distros repository (see below) : 

  * Fedora, Mageia - sudo dnf install qt5pas-devel <enter>
  * Ubuntu, Debian - sudo apt install libqt5pas-dev <enter>
  * Arch type systems - sudo pacman -S --needed qt5pas <enter>



it will bring along the necessary Qt5 libraries as dependencies, typically about 50Meg on a system that has no existing Qt5 needs. If you wish to use a Qt5 version of Lazarus, build from source with a command like "make bigide LCL_PLATFORM=qt5 <enter>" but note the default GTK2 Lazarus is quite happy to build Qt5 apps for you. 

From within Lazarus, to choose to build a Qt5 app, Projects->ProjectOptions->AdditionsandOverrides and click Set "LCLWidgetType", and select Qt5 for the current Build Mode. Better yet, add a special build mode for Qt5, maybe a release and debug. 

Note : that if you install Lazarus using you distribution's version of Lazarus, you may need to specifically ask for the Qt5 LCL components. If installing from source, you will get the lot right from the start. 

Don't forget, when releasing your app, it should be marked as being dependent on libqt5pas1. Further, as detailed below, your app may need a more current libqt5pas than the one in your end user's system. 

### libqt5pas

**Note | At the time of writing, March 2025, the current libQt5Pas is V1.2.16. A project built with any recent Lazarus will need at least V1.2.15 and on some distros (due to the version of Qt5 they use) that is V1.2.16.**

_libqt5pas_ is a library that bridges between Qt5 (or in the case of libqt6pas, Qt6) and your Lazarus application. Mark you application as dependent on libqt5pas1 and all will be OK. You, as a developer using Qt5 will also need libqt5pas-dev as mentioned above. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** if you are using a current version of Lazarus, you (and your end users) will need a current libQt5Pas. Most distros have a libqt5Pas in its repo that was released just before the distro version was released. So you need to build your own current libqt5Pas or use the unofficial <https://github.com/davidbannon/libqt5pas/releases/latest> packages as mentioned below. Remember, your end users need a current library too ! Newer distros (U24.04, Debian Trixie, most Arch based) have working versions available in their repositories.

Using your distros repository - 

  * Fedora, Mageia - sudo dnf install qt5pas<enter>
  * Ubuntu, Debian - sudo apt install libqt5pas1 <enter>
  * Pacman system - sudo pacman -S --needed qt5pas <enter>



You can check the libQt5Pas package version in human-friendly form by issuing this command: `ls -al libQt5Pas.so*` in the following folder: 

On Debian / Ubuntu variants: `/usr/lib/x86_64-linux-gnu`

On Raspberry OS: `/usr/lib/arm-linux-gnueabihf`

### Replace your libQt5Pas ?

**Build your own.** The Lazarus source has the code to build your own, you will need gcc and a few other things, documented in the README.txt. Then copy the new library over top of your existing libQt5Pas1 (as root of course). 

**Download an unofficial package.** You find deb, rpm and tarball packages at <https://github.com/davidbannon/libqt5pas/releases/latest> for Intel and Arm 32bit and 64bit. You might need to ensure your Package Manager does not object or, even, replace your bright shiny new one with its preferred distro one. Raise a (github) issue or shout out on the Forum if you need something other than the ones there now. Install with the usual commands for your distro (remembering to always put a ./ ahead of the package name, else the installer tries to get it from its repo). As a developer, you will also need the -dev (or -devel) package, remove an existing one if present. 

### Systems using Wayland

Some distributions use Wayland by default (e.g. the default Fedora with a GNOME desktop) and Qt5 does not work with Wayland. You will receive a message like: 
    
    
    QSocketNotifier: Can only be used with threads started with QThread
    [FORMS.PP] ExceptionOccurred
    ....
    

Logout, from the login screen click the small gear symbol, select "GNOME on XOrg". Log back in again. 

There is, apparently a Wayland plugin for Qt5, for the brave - <https://wayland.freedesktop.org/qt5.html>

### Qt5 Colours

On Linux, sometimes Qt5 apps will inherit the colours it uses from the Desktop/OS. However this does not always work and appears to be Distribution / Desktop dependent. With the current fad of Dark Themes, a more reliable approach is to use qt5ct. qt5ct appears to be in most distribution's repository and is a quick and easy install. When you run it, it opens on the "Appearance" tab, for a Dark Theme, choose Palette = Custom, Color Scheme = Darker. Click "Apply" and "OK" to close the app. 

[![qt5ct dark.png](https://wiki.freepascal.org/images/f/f1/qt5ct_dark.png)](</File:qt5ct_dark.png>)

Did you note the message across the top, "The application is not configured correctly" ? What it is worried about is that you have not added the environment variable setting that will tell your app to use the colours you just set. You can set it for just one application by setting it in the application's command line : 
    
    
    QT_QPA_PLATFORMTHEME=qt5ct myapplication

That may be added to the app's Desktop file or maybe you edit the menu entry. The alternative is to add it /etc/environment (obviously without the application's name) so it is automatically set for all apps, you need to logout and back in again. Adding an entry into your own .bashrc is not quite appropriate in that GUI apps started from the menu for example will not be told about it. Note: On Debian installing qt5ct automatically add QT_QPA_PLATFORMTHEME to environment variables. 

### Why Qt5 instead of GTK ?

Good question ! Most Linux Lazarus applications are built using the default GTK2. The various Linux distributions are delivering fewer and fewer GTK2 apps, generally 'upgrading' to GTK3. However, the Lazarus GTK3 interface still needs some work and its not ready for production systems yet. Until late 2019, this has not mattered much, GTK2 works fine. But Ubuntu19.10 does not have GTK2 libraries installed by default and Xfce has announced its next release will be GTK2 free, apparently many distributions are looking to drop GTK2 from their default kits. While you can still install GTK2, its pretty big.... 

Qt5 is an obvious solution, its already installed on distros like Fedora 30 plus, its a relatively lightweight addition to Ubuntu, only 48Meg compared with GTK2's 445Meg. If you mark your application dependent on libqt5pas then it will be installed at the same time, bringing along the other qt5 files if necessary. Manually installing libqt5pas with your package manager (as long as it resolves dependencies!) will also work. 

Clearly, Qt based distributions, ones using Plasma or KDE as a desktop will require only _libqt5pas_. 

## macOS (64-bit)

The following steps have been tested with macOS 10.13.6 (High Sierra), Xcode command line tools 10.0.0 (no Xcode required), Lazarus 1.8.4 and minimum Qt 5.6.2 (Qt 5.12.xx works without problems too). 

After installing Qt 5.6.2 (see above), open up a terminal window and run these commands (paths may vary): 
    
    
    export LazarusDir=/Developer/Lazarus
    export QtDir=~/Qt5.6.2
    cd $LazarusDir/lcl/interfaces/qt5/cbindings
    export PATH=$QtDir/5.6/clang_64/bin:$PATH
    qmake
    

**If`qmake` fails** with the following error: 
    
    
    xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory '/Library/Developer/CommandLineTools' is a command line tools instance
    

**it does not** mean that you have to install Xcode. See [Qt without Xcode](<https://gist.github.com/shoogle/750a330c851bd1a924dfe1346b0b4a08>) for a workaround and try running `qmake` again. (The Xcode dependency seems to be fixed in Qt 5.9.4 or maybe earlier.) 

Proceed the build process with the following commands: 
    
    
    make
    make install
    

It may be helpful to add symlinks to the Qt5Pas.framework and other *.framework files (so no modifications to the path are required): 
    
    
    cd /Library/Frameworks/
    sudo ln -s $QtDir/5.6/clang_64/lib/*.framework .
    

Remember that Qt5 is 64 bit, so in Lazarus under Tools - Options - Environment, you need to change Compiler Executable from `/usr/local/bin/ppc386` to `/usr/local/bin/ppcx64` . With ppc386, you would later get the following errors when compiling your Lazarus projects: 
    
    
    Error: linker: Undefined symbols for architecture i386:
    Error: linker: "_QAbstractButton_click", referenced from:
    [...]
    ld: symbol(s) not found for architecture i386
    

Finally, create a new Lazarus project (or open an existing one), open Project - Project options - Compiler options - Additions and overrides, set `LCLWidgetType:=qt5` . You should now be able to compile your project with the Qt5 widget set. 

## Example Project

See [First Steps with Lazarus Qt5 interface](<https://tondrej.blogspot.com/2018/04/first-steps-with-lazarus-qt5-interface.html>) for an example project for Win32, Win64, macOS (64 bit) and Linux (64 bit). 

## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<Carbon_Interface.md> "Carbon Interface") \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
  * [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa 64 bit interface for macOS
  * [Qt Interface](<Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * Qt5 Interface \- The Qt5 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [GTK1 Interface](<GTK1_Interface.md> "GTK1 Interface") \- The gtk1 interface for Unixes, macOS (X11), Windows
  * [GTK2 Interface](<GTK2_Interface.md> "GTK2 Interface") \- The gtk2 interface for Unixes, macOS (X11), Windows
  * [GTK3 Interface](<GTK3_Interface.md> "GTK3 Interface") \- The gtk3 interface for Unixes, macOS (X11), Windows
  * [fpGUI Interface](<fpGUI_Interface.md> "fpGUI Interface") \- Based on the fpGUI library, which is a cross-platform toolkit completely written in Object Pascal
  * [Custom Drawn Interface](<Custom_Drawn_Interface.md> "Custom Drawn Interface") \- A cross-platform LCL backend written completely in Object Pascal inside Lazarus. The Lazarus interface to Android.



### Platform specific Tips

  * [Android Programming](<Android_Programming.md> "Android Programming") \- For Android smartphones and tablets
  * [iPhone/iPod development](<iPhone/iPod_development.md> "iPhone/iPod development") \- About using Objective Pascal to develop iOS applications
  * [FreeBSD Programming Tips](<FreeBSD_Programming_Tips.md> "FreeBSD Programming Tips") \- FreeBSD programming tips
  * [Linux Programming Tips](<Linux_Programming_Tips.md> "Linux Programming Tips") \- How to execute particular programming tasks in Linux
  * [macOS Programming Tips](<macOS_Programming_Tips.md> "macOS Programming Tips") \- Lazarus tips, useful tools, Unix commands, and more...
  * [WinCE Programming Tips](<WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Using the telephone API, sending SMSes, and more...
  * [Windows Programming Tips](<Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips



### Interface Development Articles

  * [Carbon interface internals](<Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset

---

_Source: [https://wiki.freepascal.org/Qt5_Interface](https://web.archive.org/web/20250429184016/https://wiki.freepascal.org/Qt5_Interface)_
