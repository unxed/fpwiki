# GTK2 Interface

[![GTK+ logo.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/GTK_logo.svg/50px-GTK_logo.svg.png)](</File:GTK_logo.svg>)

This article applies to [GTK+ Widgetsets](</Category:GTK_Widgetsets> "Category:GTK Widgetsets") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │    
****

## Contents

  * 1 Introduction
  * 2 Other Interfaces
    * 2.1 Platform specific Tips
    * 2.2 Interface Development Articles
  * 3 Road map for the gtk2 interface
  * 4 Quick start guide for Linux, FreeBSD, etc
    * 4.1 Ubuntu Unity liboverlay
    * 4.2 Ubuntu Unity Menuproxy
    * 4.3 Raspberry Pi OS
  * 5 Using the GTK2 interface under macOS
    * 5.1 Installing X11
    * 5.2 Installing GTK2-Lazarus with fink
    * 5.3 Installing gtk using MacPorts
    * 5.4 Using the GTK2 libraries included with Gimp
  * 6 Using the Gtk2 interface under Microsoft Windows
    * 6.1 Getting the needed Gtk2 libraries
    * 6.2 Standalone Application under Windows
    * 6.3 Themes
    * 6.4 A sample project
  * 7 Current issues
    * 7.1 Editor Options allows to select incompatible fonts
    * 7.2 Moving with keys in the OI sometimes does not move the edit field
    * 7.3 Destroying handle during OnKeyDown gives AV
    * 7.4 Z-Order of controls
    * 7.5 Open Dialog Options
    * 7.6 TScrollBox and Scrolling problems
    * 7.7 Shortcut keys
    * 7.8 Up/Down key navigation between TEdits
  * 8 Done issues
    * 8.1 Done: ~~Synedit is very slow~~
    * 8.2 Done: ~~Small TButton does not center text~~
    * 8.3 Done: ~~TOpenPictureDialog does not update the preview~~
    * 8.4 Done: ~~Only the main form appears in the window list~~
    * 8.5 Done: ~~Slow and visible resizing when creating/resizing a dialog~~
    * 8.6 Done: ~~Selecting a TPage in the designer gives the wrong bounds rectangle~~
    * 8.7 Done: ~~Sometimes the hint window of the component palette is not resized~~
    * 8.8 Done: ~~gtk criticals in IDE codetools options dialog~~
    * 8.9 Done: System Colors
  * 9 Gtk1/Gtk2 separation roadmap
  * 10 Profiling



## Introduction

The gtk2 widgetset is stable and all components work. And it needs optimizations. 

The documentation can be found [here](<http://www.gtk.org/>). 

The minimum Gtk 2 version supported is Gtk 2.8 

## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<Carbon_Interface.md> "Carbon Interface") \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
  * [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa 64 bit interface for macOS
  * [Qt Interface](<Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [Qt5 Interface](<Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [GTK1 Interface](<GTK1_Interface.md> "GTK1 Interface") \- The gtk1 interface for Unixes, macOS (X11), Windows
  * GTK2 Interface \- The gtk2 interface for Unixes, macOS (X11), Windows
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



## Road map for the gtk2 interface

Here: [Roadmap#Widgetset_dependent_components](<Roadmap.md> "Roadmap")

## Quick start guide for Linux, FreeBSD, etc

The first thing to do is to install the gtk2 libraries including the development packages. For example: linux/debian systems call them libgtk2.0-dev. There are complete installers for Windows [here](<http://www.gtk.org/>). 

The gtk2 is the default widgetset, so it should work out of the box. 

You can compile the LCL for gtk2. First open your normal compiled Lazarus. Then go on the menu "Tools" --> "Configure Build Lazarus". Set LCL to "clean+build" and everything else to "None". Now select "gtk2" and click on the "Ok" button. Next go to the menu "Tools" --> "Build Lazarus". Now the LCL is compiled for gtk2 too. 

To compile a project for gtk2 just select it as the target widgetset on the Compiler Options dialog. 

### Ubuntu Unity liboverlay

The Ubuntu's Unity LIBOVERLAY override breaks the gtk_grab_add function, so mouse capturing does not work in synedit, treeview and many more LCL controls that descend from TCustomControl. That means dragging or selecting text with the mouse does not work. 

This is true for many applications, which is why liboverlay has an internal list to disable itself for many famous applications (e.g. gimp). 

That's why the gtk2 interface of the LCL disables liboverlay by default. You can enable it by compiling the LCL with -dEnableLibOverlay. 

If it is enabled: The LCL will write a warning with [DebugLn](<DebugLn.md> "DebugLn") if it spots liboverlay. 

To disable liboverlay for a single application set the environment variable LIBOVERLAY_SCROLLBAR to '0' *before* starting the application. There is no way to disable liboverlay when the application is already running. For example: 
    
    
    LIBOVERLAY_SCROLLBAR=0 ./lazarus
    

You can disable it on a whole machine by uninstalling the package "liboverlay-scrollbar-0.2-0". 

The IDE starter "startlazarus" sets this variable before starting the IDE (since 1.0). startlazarus is called for example when you use the desktop menu item. 

### Ubuntu Unity Menuproxy

In more recent Ubuntu versions, Ubuntu tries to create a "one menubar" look like macOS has, but this fails miserably with multi window programs like Lazarus. The main problem is not being able to access the menu without first selecting the main lazarus window. (the window that would hold the menubar). 

Turning off seems to be impossible without massive changes (probably that is because then everybody would turn such features off immediately). 

Turning off manually is possible through setting environment variable UBUNTU_MENUPROXY equal to 0 before starting. 

UBUNTU_MENUPROXY=0 ./lazarus 

or put it in a script like: 
    
    
    #!/bin/sh
    export UBUNTU_MENUPROXY=0
    ./startlazarus
    

### Raspberry Pi OS

Note: the following advice is proably not necessary if you have installed libgtk2.0-dev as mentioned further up this page 

Lazarus GTK2 cannot be compiled by default, because of missing symlinks for libraries. These symlinks are missed: 

  * libgdk-x11-2.0.so
  * libgtk-x11-2.0.so
  * libX11.so
  * libgdk_pixbuf-2.0.so
  * libglib-2.0
  * libgthread-2.0.so
  * libgmodule-2.0.so
  * libpango-1.0.so
  * libcairo.so
  * libatk-1.0.so



The existing .so files are located in the "/usr/lib/aarch64-linux-gnu". For all mentioned files, you need to make symlinks like this: 
    
    
    cd /usr/lib/aarch64-linux-gnu
    sudo ln -s libatk-1.0.so.0 libatk-1.0.so
    

## Using the GTK2 interface under macOS

### Installing X11

If you want to use the gtk widgetset instead of the default Carbon widgetset, then make sure **X11** is installed. X11 is no longer included with Mac, but X11 server and client libraries are available from the [XQuartz project](<https://www.xquartz.org/>). The last XQuartz release was version 2.7.11 on 2016-10-29 for Snow Leopard 10.6.3 or later. 

How do you know whether X11 is installed? Look in the /Applications/Utilities folder for an application named XQuartz.app. All further details are in /opt/X11/ (soft linked from /usr/X11). 

**Tip** \- Drag and drop X11 onto the dock so you'll have a one-click way of opening an X11 window. 

### Installing GTK2-Lazarus with fink

Install [Fink](<http://www.finkproject.org/>), the package manager for macOS. Install the GTK2 version of Lazarus with the following command: 
    
    
     fink install lazarus-gtk2
    

You will be asked to install a number of other packages. Simply answer yes. The installation takes quite a while. After it has finished, the GTK2 application can be started with a double click on /Applications/Fink/lazarus. 

Note: Projects using GTK2 can equally be created using the Aqua version of Lazarus, which is installed with "fink install lazarus-aqua". 

### Installing gtk using MacPorts

Follow these steps to install the libraries using MacPorts. Install MacPorts from [**www.macports.org**](<http://www.macports.org/>). When MacPorts is set up, you need to install two library sets using the following commands: 
    
    
    sudo /opt/local/bin/port install gtk1
    sudo /opt/local/bin/port install gdk-pixbuf
    

Then make sure that Lazarus can find the library by using the command below before running Lazarus. Alternatively you can add this command to your bash startup script (`/etc/profile` or `~/.bash_profile`) before you start the shell to run Lazarus: 
    
    
    export DYLD_FALLBACK_LIBRARY_PATH="/opt/local/lib:$DYLD_FALLBACK_LIBRARY_PATH"
    

If this doesn't work, simply do 
    
    
    sudo ln -s /opt/local /sw
    

### Using the GTK2 libraries included with Gimp

Now that the Cocoa widgetset is the default for Lazarus on macOS, there probably isn't much need for developing or distributing GTK2-based apps on macOS. However, you might want to test your apps with the GTK2 widgetset to see how well the graphical portion will work under Linux, where GTK2 is likely to be the default. You can also compile Lazarus itself against the GTK2 widgetset if you're curious about how Lazarus looks with GTK2. 

An easy way to obtain recent GTK2 libraries is to download and install Gimp from <http://www.gimp.org/macintosh/>. Gimp is not only a useful image editor in its own right, but it also installs GTK2 version **2.16.6** libraries that you can link your apps against. 

Copy and paste the files given below into a text editor and save them as **gimplib.cfg** and **glaunch.sh**. File gimplib.cfg tells FPC how to link against the GTK2 libraries; file glaunch.sh initializes GTK2 and starts a GTK2-compiled executable specified on the command line. 

To compile an app against the GTK2 widgetset and link it with the Gimp GTK2 libraries, do the following: 

  * In the Lazarus Compiler Options dialog, on the Other tab, check Use Additional Compiler Config File and enter the path to gimplib.cfg in the text box (for example, ~/tools/scripts/gimplib.cfg).
  * In the Lazarus Compiler Options dialog, on the Paths tab, select GTK2 as the LCL Widget Type.
  * Choose Run | Build.



To run a GTK2-compiled executable, open an X11 window, change to your app's folder, and enter the following (which assumes glaunch.sh is in ~/tools/scripts): 

  * ~/tools/scripts/glaunch.sh myexec



To compile Lazarus itself, do the following: 

  * Open an X11 window and change to your Lazarus source folder.
  * Make a copy of your Carbon-based Lazarus, in case something goes wrong (cp -p lazarus lazarus-carb).
  * make clean all LCL_PLATFORM=gtk2 FPC=fpc OPT="@~/tools/scripts/gimplib.cfg -dHasX"



This will rebuild lazarus, lazbuild and startlazarus. You'll probably want to rename your lazarus executables: 

  * mv lazarus lazarus-gtk2
  * mv lazarus-carb lazarus



To run the GTK2-compiled Lazarus: 

  * ~/tools/scripts/glaunch.sh lazarus-gtk2



Here is the gimplib.cfg config file. Note that this assumes you installed Gimp in your Applications folder. 
    
    
    -k'-headerpad_max_install_names'
    -k'-L/usr/lib'
    -k'-L/usr/X11R6/lib'
    -k'-L/Applications/Gimp.app/Contents/Resources/lib'
    -k'-lgthread-2.0'
    
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libpangoft2-1.0.0.dylib:/Applications/Gimp.app/Contents/Resources/lib/libpangoft2-1.0.0.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libz.1.dylib:/Applications/Gimp.app/Contents/Resources/lib/libz.1.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libpangocairo-1.0.0.dylib:/Applications/Gimp.app/Contents/Resources/lib/libpangocairo-1.0.0.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libgmodule-2.0.0.dylib:/Applications/Gimp.app/Contents/Resources/lib/libgmodule-2.0.0.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libintl.8.dylib:/Applications/Gimp.app/Contents/Resources/lib/libintl.8.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libiconv.2.dylib:/Applications/Gimp.app/Contents/Resources/lib/libiconv.2.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libcairo.2.dylib:/Applications/Gimp.app/Contents/Resources/lib/libcairo.2.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libfreetype.6.dylib:/Applications/Gimp.app/Contents/Resources/lib/libfreetype.6.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libfontconfig.1.dylib:/Applications/Gimp.app/Contents/Resources/lib/libfontconfig.1.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libexpat.1.dylib:/Applications/Gimp.app/Contents/Resources/lib/libexpat.1.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libXrender.1.dylib:/Applications/Gimp.app/Contents/Resources/lib/libXrender.1.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libtiff.3.dylib:/Applications/Gimp.app/Contents/Resources/lib/libtiff.3.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libjpeg.62.dylib:/Applications/Gimp.app/Contents/Resources/lib/libjpeg.62.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libpng12.0.dylib:/Applications/Gimp.app/Contents/Resources/lib/libpng12.0.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libXinerama.1.dylib:/Applications/Gimp.app/Contents/Resources/lib/libXinerama.1.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libpixman-1.0.dylib:/Applications/Gimp.app/Contents/Resources/lib/libpixman-1.0.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libgio-2.0.0.dylib:/Applications/Gimp.app/Contents/Resources/lib/libgio-2.0.0.dylib'
    -k'-dylib_file'
    -k'/tmp/skl/Gimp.app/Contents/Resources/lib/libjasper.1.dylib:/Applications/Gimp.app/Contents/Resources/lib/libjasper.1.dylib'
    

Here is the glaunch.sh script file: 
    
    
    #!/bin/sh
    
    TMP="/tmp/$UID/TemporaryItems"
    GRES="Gimp.app/Contents/Resources"
    GIMP="/Applications/$GRES"
    if ! [ -e "$GIMP" ]
    then
      GIMP="~/Desktop/$GRES"
      if ! [ -e "$GIMP" ]
      then
        GIMP="~/$GRES"
        if ! [ -e "$GIMP" ]
        then
          GIMP="~/Applications/$GRES"
          if ! [ -e "$GIMP" ]
          then
            GIMP=""
          fi
        fi
      fi
    fi
    
    if [ "$GIMP" = "" ]
    then
      echo "Can't locate Gimp"
    else
      export "DYLD_FORCE_FLAT_NAMESPACE=1"
      export "DYLD_INSERT_LIBRARIES=/usr/lib/libiconv.dylib"
      export "DYLD_LIBRARY_PATH=/System/Library/Frameworks/ApplicationServices.framework/Versions/A/Frameworks/ImageIO.framework/Versions/A/Resources:$GIMP/lib"
      export "PANGO_RC_FILE=$TMP/pangorc"
      export "FONTCONFIG_PATH=$GIMP/etc/fonts"
      export "GTK_IM_MODULE_FILE=$TMP/gtk.immodules"
      export "GDK_PIXBUF_MODULE_FILE=$TMP/gdk-pixbuf.loaders"
      export "GTK_DATA_PREFIX=$GIMP"
      export "GTK_EXE_PREFIX=$GIMP"
    
      mkdir -p "$TMP"
      sed 's|/tmp/skl/Gimp.app/Contents/Resources/etc/pango|'"$TMP|g" "$GIMP/etc/pango/pangorc" > "$TMP/pangorc"
      sed 's|/tmp/skl/Gimp.app/Contents/Resources|'"$GIMP|g" "$GIMP/etc/pango/pango.modules" > "$TMP/pango.modules"
      cp -f "$GIMP/etc/pango/pangox.aliases" "$TMP"
      sed 's|/tmp/skl/Gimp.app/Contents/Resources|'"$GIMP|g" "$GIMP/etc/gtk-2.0/gtk.immodules" > "$TMP/gtk.immodules"
      sed 's|/tmp/skl/Gimp.app/Contents/Resources|'"$GIMP|g" "$GIMP/etc/gtk-2.0/gdk-pixbuf.loaders" > "$TMP/gdk-pixbuf.loaders"
    
      ./$1
    fi
    

## Using the Gtk2 interface under Microsoft Windows

### Getting the needed Gtk2 libraries

Using the Gtk2 libraries from gtk.org: 

  * <http://www.gtk.org/download>



or using the GTK-Runtime project: 

  * <https://gtk-win.sourceforge.io>



or using the Gtk2 libraries provided by Gimp: 

  * <http://gimp-win.sourceforge.net/stable.html>



### Standalone Application under Windows

You may prefer a standalone application to avoid versions conflicts. All needed libraries are in the packages mentioned above. They should be placed in the same folder of your executable. You will need all these libraries: 

  * libcairo-2.dll
  * libglib-2.0-0.dll
  * libpangocairo-1.0-0.dll
  * libpng12.dll
  * libgdk-win32-2.0-0.dll
  * libgmodule-2.0-0.dll
  * libgtk-win32-2.0-0.dll
  * libatk-1.0-0.dll
  * libgdk_pixbuf-2.0-0.dll
  * libgobject-2.0-0.dll
  * libpango-1.0-0.dll
  * libpangowin32-1.0-0.dll
  * libgio-2.0-0.dll
  * zlib1.dll



### Themes

The default GTK2 interface is quite ugly and not very integrated with the Windows Desktop. Your GTK2 application may use the Windows Theme if you use the [gtk-wimp project](<http://gtk-wimp.sourceforge.net/>). Into your application's folder you should add the following files ([Available here](<http://sourceforge.net/project/showfiles.php?group_id=76416>)): 

  * .\lib\gtk-2.0\engines\libwimp.dll
  * .\share\themes\Raleigh\gtk-2.0\gtkrc



### A sample project

A complete and working "Hello world" project (with sources) is available [here](<http://www.lazarusforum.de/lazwiki/winGTK2_Demo.zip>). 

[![WinGTK2.png](https://wiki.freepascal.org/images/b/b1/WinGTK2.png)](</File:WinGTK2.png>)

## Current issues

Here are some notes about some of the current issues. It is not a complete list, but mainly a collection of the information about some of the most annoying problems, that can not be solved simply. 

  


### Editor Options allows to select incompatible fonts

TFontDialog must be extended to show only monospace fonts by default and only if the users insists show all fonts. 

  


### Moving with keys in the OI sometimes does not move the edit field

ToDo 

### Destroying handle during OnKeyDown gives AV

This is a more general problem, not only KeyDown. There are two solutions: 

  * Increase reference counters of all used widgets during events. Difficult to maintain and we will never be sure to ref count every used widget.
  * Instead of destroying widgets immediately, hide them, untie them and put them into a queue. The widgets will then be destroyed in the message loop and on gtk intf shut down.



### Z-Order of controls

In Gtk2 many controls don't have a window, so they are just painted on their parent. This makes they be painted before controls with a window, so we get a wrong Z-Order. 

Here is a list of gtk widgets that don't have windows: <https://developer.gnome.org/gtk-tutorial/stable/x481.html>

The solution is adding a GtkEventBox under each of these widgets: <https://developer.gnome.org/gtk-tutorial/stable/c1226.html#SEC-EVENTBOX>

**Bug reports**

  * <http://bugs.freepascal.org/view.php?id=964>
  * <http://bugs.freepascal.org/view.php?id=8346>



### Open Dialog Options

TOpenDialog has several options to control the dialog. When implementing these options in Gtk2, however, one needs to watch out because of the vast amount of bugs in Gtk 2, which may prevent the options from working. For example, ofForceShowHidden doesn't work because of Gtk 2 bugs, althougth it is implemented correctly. 

**Bug reports**

  * ofForceShowHidden - <http://bugs.freepascal.org/view.php?id=8473>



### TScrollBox and Scrolling problems

TScrollBox and Scrolling seems deeply broken in Gtk 2. 

**Bug reports**

  * <http://bugs.freepascal.org/view.php?id=10779>
  * <http://bugs.freepascal.org/view.php?id=10952>
  * <http://bugs.freepascal.org/view.php?id=9049>
  * <http://bugs.freepascal.org/view.php?id=11010>



### Shortcut keys

**Bug reports**

  * <http://bugs.freepascal.org/view.php?id=1946>
  * <http://bugs.freepascal.org/view.php?id=7249>
  * <http://bugs.freepascal.org/view.php?id=10282>



### Up/Down key navigation between TEdits

~~A standard Gtk 2 feature allows the use of up/down key to navigate between GtkEntry widgets (like TEdits), but this is undesirable if the user implements a dropdrow list to be attached to the TEdit. As we cannot know what code the user has written, the best option is to disable this feature. Possible ways to do that are:~~

~~~~

~~
  * Listen to key press events of GtkEntry and for Up/Down events shift the focus to implemented widget.
~~
  * ~~Don't propagate Up/Down key press events to GtkEntry.~~



**As of lazarus 0.9.31 r31562 GtkEntry behaviour is same as under other widgetsets - NO FOCUS CHANGING BY VK_UP or VK_DOWN.**

**Bug reports**

  * <http://bugs.freepascal.org/view.php?id=11115>



## Done issues

### Done: ~~Synedit is very slow~~

Actually this is not a bug of the gtk2 intf, but a problem of gtk2 and some video drivers. On some systems SynEdit is as fast as any other text editor. Some bugs were fixed with gtk 2.12. SynEdit already paints every token with the ExtTextOut function. It does not use a TCanvas text function, which would be even slower. 

Bug Tracker item: <http://www.freepascal.org/mantis/view.php?id=7717>

For people using the Nvidia drivers, setting GlyphCache=1 may speed up SynEdit. You will need the beta-drivers: [http://www.nvnews.net/vbulletin/showthread.php?s=399832e3761a522d34db6faaf8423ad9&t=118088](<http://www.nvnews.net/vbulletin/showthread.php?s=399832e3761a522d34db6faaf8423ad9&t=118088>)

  


### Done: ~~Small TButton does not center text~~

Actually the text is centered, but some themes (like the default under ubuntu) define a big minimum space above and below the label. The gtk2 default is not that big. So, it is a user setting. 

  * a) It might be possible to override this setting and set the space to 0. 
    * Pro: Most small autosize buttons would look better.
    * Cons: the AutoSize would be incorrect. And the bug is still there for themes with big text.
  * b) set the interface constraint to the gtk_button normal height. 
    * Pro: there are no small buttons.
    * Con: Enlarging a button requires autosizing all surrounding controls too. And you need a nice auto shrink feature, which does not exist yet.
  * c) Set the space to 0 and when calculating the preferred size, restore temporarily the space size. 
    * Pro: Most small buttons will look better, autosize will still work.
    * Con: Most difficult solution to implement. Maybe not possible without a hack.



**Bug Reports**

  * <http://bugs.freepascal.org/view.php?id=11120>
  * <http://bugs.freepascal.org/view.php?id=8765>



### Done: ~~TOpenPictureDialog does not update the preview~~

### Done: ~~Only the main form appears in the window list~~

Probably a bug in the window hints parameters. 

### Done: ~~Slow and visible resizing when creating/resizing a dialog~~

This has several reasons, which fall into 3 categories: 

1\. Too many resizes. Status: Acceptable. 

2\. Slow painting. Status: It seems other gtk2 apps are as slow, so maybe it is a gtk2 issue. 

3\. Some resizes happen after painting. Status: Acceptable. 

  * The application does not resize the controls in one step, but in many small steps. For example: It resizes a control in several small steps SetBounds, AnchorParallel, ... . Or first the the Form is resized, then the a TPageControl, then a TButton, ... . Solution: The LCL sets BeginAlign/EndAlign during loading/creation.
  * If there are anchored or autosized controls, every resize triggers the resize system of the the LCL. The LCL tries to combine many resizes during csLoading and Begin/EndAlign. During creation the LCL still sends several times the bounds to the interface. ToDo Solution: Send bounds not during BeginAlign (including parents), but send them all in EndAlign.
  * The hen-egg problem. In order to autosize, the LCL needs the current gtk theme sizes. The current gtk theme sizes can only be calculated by creating a handle. When you create a handle you need the size. Solution: Create some hidden widgets to test the current gtk sizes. This already works for TCustomGroupBox and descendants. ToDo: the other controls.
  * The gtk does not resize immediately, but puts resizes on a queue and resizes later. These values are stored in private, hidden variables. Especially if you resize a TGroupBox, the inner borders are updated later by the gtk. That means the GetClientRect function is not reliable during resizing. That's why the LCL first works with the wrong ClientRect. Solution: The LCL anticipates the new ClientRect based on the old. And there is now a GetDefaultClientRect function to ask the interface for an estimation of the ClientRect.
  * The gtk1 intf caches LCL resize requests. This accelerates many typical overhead, but it only works with delayed painting and creates flicker during creation and resizing a form never works smooth. The delayed painting breaks the gtk2 double buffering and special paint contexts (opengl), so this trick is disabled under gtk2 and therefore the delayed resize does not work under gtk2.
  * Top level controls, like TForm can not move/resize freely. The window manager decides, what is possible. Often they take only the first position/size request and ignores the later changes. And even worse: first the window is created, realized, then resized, then mapped and 'shown' and finally moved to the final position. So, if the gtk intf asks the gtk where a form is during creation, it will most of the time get 0,0, even though it told the gtk to move the window long ago. Because this is a more general problem (not gtk specific, but X), the LCL tries avoids asking the interface for coords and only asks on rare occasions like LM_SIZE, LM_MOVE messages coming from the interface or if told to do so.
  * The LCL expects a LM_SIZE message on resize, including window state changes like iconify/maximize. But the gtk first notifies about the change and then resizes. ToDo: 1. Maybe: Omit the message, if the state does not change (is this Delphi compatible?). 2. Send the current window state with the normal resizes (Should work for maximize and restore from maximize). 3. Find a solution for iconify and restore from iconify.



Things done to handle the above issues: 

  * The gtk2 intf now calls gtk_widget_size_allocate when resizing widgets. This initializes the Widget^.allocation, so that further reads by the LCL no longer give old values.
  * Resizes of child widgets (not gtkwindow or not top level) are now given to the gtk immediately. This does not resize immediately, but together with other changes this resizes earlier, reducing the total number of resizes.
  * gtk resize events are now given almost always directly to the LCL. The only exception is gtkwindow and the client areas (gtk_fixed). The gtk resize events come bottom up, so the client areas are always triggered before the widget itself is resized.
  * The gtk intf now checks the mapped attribute for gtkwindow, which seems finally to work reliable under gtk2. This means the gtk intf no longer send window positions to the LCL, before the window is really moved.
  * In order to let the LCL resize during component/handle creation, the new function GetDefaultClientRect allows every widget class to provide a nice clientrect even before the handle is created. This is done by using the hidden style widgets of the gtk intf.



### Done: ~~Selecting a TPage in the designer gives the wrong bounds rectangle~~

GetClientOrigin needed extra code for TNotebook, which does not have a normal client area. 

### Done: ~~Sometimes the hint window of the component palette is not resized~~

Sometimes the gtk does not resize the gdkwindow. This is now forced and it seems to fix this problem. 

### Done: ~~gtk criticals in IDE codetools options dialog~~

Empty comboboxes do not have all private structures, so you can not set callbacks. 

### Done: System Colors

The system colors depend on the theme, but many themes don't give adequate colors and many controls use system colors, so we need them to have a correct value. To solve this, a list of ignored system colors was constructed, and in case the user sets any of these colors, it is ignored and the default theme color is used instead. 

List of colors which were tested and need to be ignored for the correct functioning of programs: 

  * clBtnFace - Otherwise Notebooks and Buttons look bad



**Bug reports**

  * <http://bugs.freepascal.org/view.php?id=8684>



## Gtk1/Gtk2 separation roadmap

List of already separated elements: 

  * gtk2wscomctrls --- gtkwscomctrls
  * gtk2wsdialogs --- gtkwsdialogs
  * gtk2wsspin --- gtkwsspin
  * gtk2wscalendar --- gtkwscalendar



Next tasks: 

  * Rework of the events system
  * Rework of private classes



## Profiling

There are various compiler switches: 

  * VerboseSizeMsg - This will show you all changes and messages, with coords and reasons. Even for simple forms this gives a lot of output.
  * DisableLoadedClientSize - This will ignore the ClientWidth/Height stored in the .lfm/.lrs files. This way you can somehow simulate, what happens, if you start an application under a different theme.

---

_Source: [https://wiki.freepascal.org/GTK2_Interface](https://web.archive.org/web/20250415142341/https://wiki.freepascal.org/GTK2_Interface)_
