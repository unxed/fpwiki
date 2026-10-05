# Qt Interface

[![Qt logo.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Qt_logo_2013.svg/50px-Qt_logo_2013.svg.png)](</File:Qt_logo_2013.svg>)

This article applies to [Qt widgetset](</Category:Qt> "Category:Qt") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[español (es)](</Qt_Interface/es> "Qt Interface/es")** │  **[日本語 (ja)](</Qt_Interface/ja> "Qt Interface/ja")** │    
****

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** This page deals with Qt4 and very old versions of Lazarus. It is most likely you would find the [Qt5 Interface](<Qt5_Interface.md> "Qt5 Interface") page more helpful.

## Contents

  * 1 Introduction
  * 2 Quick start guide for Linux
    * 2.1 For Lazarus 0.9.30 and below
    * 2.2 For Lazarus 0.9.31
  * 3 Quick start guide for macOS
  * 4 Quick start guide for PDAs and Smartphones
  * 5 Quick start guide for Windows
  * 6 Qt 4 Bindings
    * 6.1 Compiling the bindings
  * 7 Road map for the Qt 4 interface
  * 8 Conditional defines accepted by the Qt Interface
  * 9 Screenshots
  * 10 Contributing
    * 10.1 How to add a new control
  * 11 Mailling List
  * 12 Other Interfaces
    * 12.1 Platform specific Tips
    * 12.2 Interface Development Articles



## Introduction

This interface is based on Qt 4 (Qt 4.8.4 is tested). For documentation, fixes and download, go to [Qt Project](<http://qt-project.org/>) (Installers at [download 4.8](<http://download.qt.io/archive/qt/4.8/>)). Lazarus with Qt interface (qt-lcl) can be used on Windows, Linux x32/x64/arm, FreeBSD, macOS 32 bit (Carbon)/ 64 bit (Cocoa). 

## Quick start guide for Linux

**As of lazarus 0.9.29 svn rev. 23858 2.1 bindings are needed, also bindings name is changed from libqt4intf to libQt4Pas !**   
**IMPORTANT NOTE: for qt >= 4.7.XX libQt4Pas is compiled with -mstackrealign to avoid crashes, so download proper version from bindings page (binary bindings for 4.7 are marked) if your Qt version is 4.7 or higher. **   
The first thing to do is go to the [official website](<http://users.pandora.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>) of the bindings and download the binary Qt bindings. Copy the file libQt4Pas.so.5.2.1 + its symlinks libQt4Pas.so.5.2 libQt4Pas.so.5 libQt4Pas.so to the directory /usr/lib/ or /usr/local/lib Now run "ldconfig" to update the linker cache. You can verify its success by running: 
    
    
    ldconfig -p | grep libQt4Pas.so.5.2.1
    

It must say something like: 
    
    
           libQt4Pas.so (libc6) => /usr/local/lib/libQt4Pas.so
    

If it didn't work, you have to check the config files in /etc/ld.so.conf.d/ or the config file /etc/ld.so.conf, depending on your distro - it must include the path where you copied the libQt4Pas.so* files. 
    
    
    Hint: If you use a Debian based distribution, you can add the following repositories to your sources.list 
     deb http://ppa.launchpad.net/ximion/ppa/ubuntu karmic main 
     deb-src http://ppa.launchpad.net/ximion/ppa/ubuntu karmic main
     The repositories contain libQt4Pas.
    
    

To let your package management system trust the keys in the PPA mentioned above use something like: 
    
    
    gpg --recv-keys 4BF17E057EC6E8C3 --keyserver http://wwwkeys.eu.pgp.net 
    gpg --armor --export 4BF17E057EC6E8C3 | apt-key add - #may need to use sudo here
    

  


### For Lazarus 0.9.30 and below

Now compile the LCL for Qt. First open your normal gtk2 compiled Lazarus. Then go on the menu "Tools" --> "Configure Build Lazarus". Set LCL to "clean+build" and everything else to "None". Now select "Qt" and click on the "Ok" button. Next go to the menu "Tools" --> "Build Lazarus". Now the LCL is compiled for Qt. 

To compile a project for Qt just select it as the target widgetset on the Compiler Options dialog. 

### For Lazarus 0.9.31

To compile the IDE for qt, first create a backup of the _lazarus_ executable for the case something goes wrong. Then open _Tools / Configure Build Lazarus_ and set **LCL Widget Type** to **qt**. Then click _Build_. After a restart you will get the qt IDE. All projects are now compiled by default with qt. 

If you use a gtk2 IDE and want to compile a project for qt, open the Project / Project Options / Build Modes. Go to _Set Macro Values_. Choose macro name **LCLWidgetType** and choose macro value to **qt**. Close the dialog with Ok and compile. All packages needed by your project will be automatically updated. 

  
**Installing Qt 4**

Most distributions now have packages for Qt 4. If your distribution is RPM-based you can search for a qt4 package here: <http://rpm.pbone.net/index.php3/stat/2/simple/2>   
**The supported Qt version is 4.5.0 or superior**

**Known problems on linux**

  * glibc < 2.4 (older distros eg. FC3) users must compile qt-x11 with -no-sse or you have immediate segfaults.
  * ~~**Qt-4.4.1** if x11 < 7.0 & glibc < 2.4 (older distros) QPalette doesn't return good results for some palettes eg. QToolTip_palette().~~



## Quick start guide for macOS

Instructions on the [Qt Interface Mac](<Qt_Interface_Mac.md> "Qt Interface Mac") wiki page. 

## Quick start guide for PDAs and Smartphones

please write me 

## Quick start guide for Windows

There's nothing special to say for Windows, it works like on Linux and seems less buggy than the Win32 interface with some controls (TListView). Not needed to rebuild IDE, just set Qt in current project: 

  * open "Project / Project Options / Build Modes"
  * go to "Set Macro Values"
  * choose macro name LCLWidgetType and set macro value to "qt"



**Version**

It seems Qt 4.8.6 doesn't work with current Qt4Pas5.dll. Better use 4.8.4 or maybe 4.8.5. 

**Installation**

  * Download Qt4 opensource edition for MinGW, from [official website](<http://qt-project.org/downloads>), from [this archive folder](<http://download.qt.io/archive/qt/>)
  * Download Qt4Pas5.dll from [FPC Qt4 Binding](<http://users.telenet.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>)
  * Download MinGW installer, from installer add MinGW C++ compiler and DLLs
  * Add to system PATH: a) Qt4 "bin" folder, b) folder of Qt4Pas5.dll, c) folder of MinGW DLLs
  * Alternative to prev step: copy all DLLs into app folder



## Qt 4 Bindings

This interface utilizes Qt 4 bindings created by Den Jean. The bindings are a c++ library which exports the methods of the Qt objects as procedures. The library (around 800kb in Linux) consists of a single .so file that needs to be distributed with your LCL program. 

You can find more information about those bindings on the official [website](<http://users.pandora.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>) and on [FreePascal Qt4 binding](<Qt4_binding.md> "Qt4 binding"). 

It is being reported that it may be possible to link to Qt 4 directly, using some tricks. Many think it is not. This is yet to be tested. It is expected that any such binding will be compatible with the currently utilized one, so the interface code won´t have to be changed. 

### Compiling the bindings

  * Qt-4.5 and higher are now available under the LGPL license. This means that you can distribute qt Pascal bindings with your program, GPL or non-GPL.
  * Lower Qt versions: 
    * If you plan to release GPL-licensed software: it is not necessary to compile the bindings yourself. GPL Binaries are available on Den Jean's website.
    * If you want to release non-GPL code, then you must compile the bindings yourself using the Commercial Edition of Qt.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If you plan to compile the bindings make sure to compile Qt with OpenSSL support. In debian for example, installing the libssl-dev package should do it

**Step 1** \- To start with download all the files needed to compile the bindings. 

  * Download the source code of the bindings. Go to the official website of the bindings. Link above.


  * Also Download Qt 4.5.2(3) or Qt 4.6.2(3) source code for the desired platform. This is the download page: [[1]](<http://www.trolltech.com/download/opensource.html>)



**Step 2** \- Unpack all the files you downloaded. Enter the directory where you downloaded Qt 4.5(6).2(3) source code and use this command: 
    
    
    run qmake -query to inspect your Qt installation
    qmake (creates Makefiles)
    make
    make install
    use make clean to clean sources directory when switching qt versions
    

**Step 3** \- ~~Go to the directory where you downloaded and extracted qt4pas sources and edit the file compile_lib.bash. Change the path for the Qt 4.5.2(3) source code.~~ needed for old bindings. 

**Step 4** \- ~~Run the script called compile_lib.bash. Now you should have a file called libqt4intf.so.5.XXXX where XXXX is version of qt bindings, and its symbolic links libqt4intf.so.5 and libqt4intf.so~~ needed for old bindings.With new bindings you'll get libQt4Pas.so.5.2.1 and symlinks libQt4Pas.so.5.2 libQt4Pas.so.5 libQt4Pas.so which must be in your library path (eg. copy it into /usr/lib). 

## Road map for the Qt 4 interface

Moved here: [Roadmap#Widgetset_dependent_components](<Roadmap.md> "Roadmap")

## Conditional defines accepted by the Qt Interface

Moved here: [LCL_Defines#Qt_defines](<LCL_Defines.md> "LCL Defines")

## Screenshots

[![Much qt progress.png](https://wiki.freepascal.org/images/8/87/Much_qt_progress.png)](</File:Much_qt_progress.png>)  


**More screenshots**  
[Qt Lazarus IDE running under linux (kernel 2.6.12, XOrg-6.8.2, KDE-3.4.0)](<http://wiki.lazarus.freepascal.org/Image:Linuxqtide.png>)  
[Qt Lazarus IDE running under Tiger 10.4.10](<http://wiki.lazarus.freepascal.org/Image:macosxqt.png>)

## Contributing

### How to add a new control

For example TButton. 

TButton is defined in lcl/buttons.pp. This is the platform independent part of the LCL, which is used by the normal LCL programmer. 

Its widgetset class is in lcl/widgetset/wsbuttons.pp. This is the platform independent base for all widgetsets (qt, carbon, gtk, win32, ...). 

Its qt interface class is in lcl/interfaces/qt/qtwsbuttons.pp: 
    
    
     TQtWSButton = class(TWSButton)
     private
     protected
     public
       class function  CreateHandle(const AWinControl: TWinControl; const AParams: TCreateParams): TLCLIntfHandle; override;
     end;
    

Every WS class, that actually implements something must be registered. See the initialization section at the end of the qtwsXXX.pp unit: 
    
    
     RegisterWSComponent(TQtButton, TQtWSButton);
    

TQtWSButton overrides CreateHandle to create a qt QPushButton. The code is short and should be easily adaptable for other controls like TCheckBox. Remember that all controls on the Qt widgetset have a helper class on qtprivate.pp, and it is also necessary to add a class for the new control. This isn´t difficult. 

Also notice that DestroyHandle should be implemented to clean up memory utilized by the control. 

## Mailling List

There is a Lazarus - Qt mailling list for support and talk about the development of this interface here: <http://lists.lazarus.freepascal.org/mailman/listinfo/qt>

## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<Carbon_Interface.md> "Carbon Interface") \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
  * [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa 64 bit interface for macOS
  * Qt Interface \- The Qt4 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [Qt5 Interface](<Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, macOS, Windows, and Linux-based PDAs
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

_Source: [https://wiki.freepascal.org/Qt_Interface](https://web.archive.org/web/20250317124348/https://wiki.freepascal.org/Qt_Interface)_
