# GTK1 Interface

[![GTK+ logo.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/GTK_logo.svg/50px-GTK_logo.svg.png)](</File:GTK_logo.svg>)

This article applies to [GTK+ Widgetsets](</Category:GTK_Widgetsets> "Category:GTK Widgetsets") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Gtk 1 development has been discontinued. The [GTK2 Interface](<GTK2_Interface.md> "GTK2 Interface") is now the default interface for *nix. Patches however are still accepted to fix compilation when Gtk1 breaks. There are no core Lazarus developers that still use the gtk1 interface. The information below may be outdated.

## Contents

  * 1 Introduction
  * 2 Other Interfaces
    * 2.1 Platform specific Tips
    * 2.2 Interface Development Articles
  * 3 Using the Gtk1 interface under macOS
    * 3.1 Required software
      * 3.1.1 If you decide not to use Fink or if your Fink installation is customized
    * 3.2 How to get going
    * 3.3 Lazarus as Icon



## Introduction

The gtk1 widgetset is quite stable. There are still some bugs and missing properties. 

The documentation can be found [here](<http://www.gtk.org/>). 

The minimum Gtk 1 version supported is Gtk 1.2.18 

## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<Carbon_Interface.md> "Carbon Interface") \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
  * [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa 64 bit interface for macOS
  * [Qt Interface](<Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [Qt5 Interface](<Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * GTK1 Interface \- The gtk1 interface for Unixes, macOS (X11), Windows
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



## Using the Gtk1 interface under macOS

If you want to build gtk1 applications on macOS you need X11 and GTK 1.2 too: 

Apple X11: On a new system: choose "Customize" in the install dialog, check "X11". 

On an already installed system: 

MacOSX 10.3: download at: 

    <http://www.apple.com/downloads/macosx/apple/x11formacosx.html>

MacOSX 10.4: install X11 from the DVD. 

GTK 1.2: Downloading the GTK packages and installing them properly can be a time consuming task. Therefore there are various package managers. One of them is fink: 

    Download fink from <http://fink.sourceforge.net/download/index.php?phpLang=en>; follow the install instructions.

In the Terminal, run 
    
    
           sudo /sw/bin/apt-get install gdk-pixbuf gtk+ gtk+-data gtk+-shlibs gtk-doc gtk-engines
    

The X11 application is in /Applications/Utilities/X11. You can drag and drop it on the dock so you can start it with one click. X11 on Leopard (10.5) is different and starting X11.app is not recommended. 

Tip: Most X11 programs, including Lazarus, expect to receive focus when the user clicks on a window. Under macOS this is deactivated. You can activate it for all X11 applications by typing in the terminal: 
    
    
           defaults write com.apple.x11 wm_click_through -bool true
    

When compiling GTK-related software, you need to add two lines to the fpc.cfg file, to enable use of libraries: 
    
    
    -Fl/sw/lib
    -Fl/usr/X11R6/lib
    

### Required software

  * Install Apple´s X11, available on your Installation Disk.



For **Jaguar 10.2.x** you can download X11 on source forge at the [X11 on Mac](<http://sourceforge.net/projects/xonx>) Project. 

For **Panther 10.3.x** (but not for any other version!) also at [Apple](<http://www.apple.com/downloads/macosx/apple/x11formacosx.html>). 

For **Tiger 10.4** you must install X11 from your tiger DVD. This is to be able to display X11 programs. Tip: Drag the X11-Icon from /Applications/Utilities/X11 into the Dock for convenient starting. 

  * Install Apple´s XCode, available on your Mac Installation Disk or at [Apple Developer Connection](<http://connect.apple.com/>) (requires creation of account, but the free online program is enough). This installs the XCode IDE and all the developer stuff.
  * Install fink, available at [Sourceforge](<http://fink.sourceforge.net/>). This is a Mac 'apt-get'esque package system.
  * Check if installation was successful, open a Terminal and type:


    
    
     echo $PATH
    

This should give you: 
    
    
     /sw/sbin:/bin:/sbin:/usr/bin:/usr/sbin
    
    

  * After successfull installation of the above, open a Terminal and type:


    
    
    sudo apt-get update
    sudo apt-get install glib glib-shlibs gtk+-shlibs gdk-pixbuf gdk-pixbuf-shlibs
    

  
This installs GTK. 

  * Install [fpc and Lazarus](<http://sourceforge.net/project/showfiles.php?group_id=89339>) as .dmg for Mac. Now your´re ready to run.



  


#### If you decide not to use Fink or if your Fink installation is customized

If you install `gdk-pixbuf`, `gtk`, and `glib` libraries not using Fink, or if your Fink installation is customized to install under other than `/sw` directory, then make sure that the environment variable `DYLD_LIBRARY_PATH` point to where the libraries are. For instance, it you use [MacPorts](<http://www.macports.org/>), the command below should do the trick: 
    
    
    export DYLD_FALLBACK_LIBRARY_PATH="/opt/local/lib:$DYLD_FALLBACK_LIBRARY_PATH"
    

The environment variable should be setup every time before running lazarus, or should be run from shell startup script. 

### How to get going

  * Start X11.
  * You get your XTerm. Focus it and type


    
    
    cd /usr/local/share/lazarus/
    ./lazarus
    

### Lazarus as Icon

You can drag and drop the X11 app onto your dock to have a one-click way of starting X11. 

To simplify starting Lazarus, in X11, choose Applications | Customize Menu, then click Add. Under Menu Name enter Lazarus, under Command enter the following: 
    
    
    /usr/local/share/lazarus/lazarus
    

Click Done to close. Now you can start Lazarus from X11 simply by choosing Applications | Lazarus. 

BEWARE: if you use the below svn, then use 
    
    
     open -a /Users/yourname/pascal/lazarus/lazarus

---

_Source: [https://wiki.freepascal.org/GTK1_Interface](https://web.archive.org/web/20240920204108/https://wiki.freepascal.org/GTK1_Interface)_
