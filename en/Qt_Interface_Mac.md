# Qt Interface Mac

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![Qt logo 2013.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Qt_logo_2013.svg/50px-Qt_logo_2013.svg.png)](</File:Qt_logo_2013.svg>)

This article applies to [Qt widgetset](</Category:Qt> "Category:Qt") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

## Contents

  * 1 Introduction
  * 2 Quick Install and Use Guide
    * 2.1 Qt4 Framework Library
    * 2.2 Qt4 Interface Binary Bindings
    * 2.3 Build Lazarus IDE on Qt
    * 2.4 Build Lazarus with qt-cocoa (qt binary distribution from nokia >= 4.7.4)
    * 2.5 Build your first Qt Application
  * 3 Outdated Build Guide
    * 3.1 Install the Qt Library Frameworks
    * 3.2 Creating the Qt interface library
    * 3.3 Creating the Qt interface framework
    * 3.4 Compiling the non-Lazarus demo programs with Free Pascal
    * 3.5 Creating a demo program application bundle
    * 3.6 Creating a Qt based Lazarus
      * 3.6.1 Starting purely from source
      * 3.6.2 Starting from a Gtk based Lazarus
    * 3.7 Creating an LCL app that uses the Qt widgetset
  * 4 Distributing an app that uses the Qt Interface Framework
  * 5 Uninstalling the Qt4 Interface and Qt4 Framework



## Introduction

There are two main path you can follow to start deploying your first Qt Application for Mac: 

  * Install compiled Interface Binary Bindings and Qt Framework Library
  * Build yourself Interface Binary Bindings and Qt Framework Library (hard and may take many hours)



## Quick Install and Use Guide

Fastest way to get you ready to develop applications with Qt Framework (+ LCL Widgetset) 

### Qt4 Framework Library

First you need to download the Cocoa Mac binary package of **Qt4 Framework Library** from [Nokia Qt download section](<http://qt.nokia.com/downloads/qt-for-open-source-cpp-development-on-mac-os-x>) or from a [Trolltech mirror](<ftp://ftp.trolltech.com/qt/source/>). The file you need is named something like _qt-mac-opensource-4.7.4.dmg_ or with greater version. 

  * Now Mount the disk image and install the package that you find inside.



### Qt4 Interface Binary Bindings

Next you need to download the Interface Binary Bindings from [Jan.Van.hijfte site](<http://users.telenet.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>), search for **Mac: Libray Binary Intel** , the file you download is named something like _mac_bin-qt4pas-V2.4_Qt4.5.3.zip_ (don't worry about version in file name, it's indicating the lowest version required by Binaries). 

  * After you click on the downloaded file it will be automatically unpacked and you will see another zip inside, unzip it too and install the package you find inside.
  * (Optional) Rename qt4.pas in qt45.pas and copy it inside _/Developer/lazarus/lcl/interfaces/qt_



### Build Lazarus IDE on Qt

Lazarus 1.2/1.3 You must rebuild libQt4Pas library according to your needs (32/64bit). Build with make (from terminal): 

  * 32bit (Carbon): make bigide LCL_PLATFORM=qt
  * 64bit (Cocoa): make bigide OS_TARGET=darwin CPU_PLATFORM=x86_64 LCL_PLATFORM=qt



Build from Lazarus IDE: 

  * 32bit just set qt widgetset in ide build config dialog and rebuild.
  * 64bit set qt widgetset and add -Px86_64 -Tdarwin in ide build config dialog.IMPORTANT NOTE: you must build IDE with make first (from terminal), and then you can rebuild from IDE because of bug (dunno where it is) which causes linking of lazarus against 32bit objects.



### Build Lazarus with qt-cocoa (qt binary distribution from nokia >= 4.7.4)

**Qt binary distribution ( >=4.7.4) is 64bit (cocoa only), so we need 64bit Qt4Pas and 64bit 2.6.0 compiler**

  * Download and install fpc-2.6.0 (with 2.4.4 64bit you can compile lazarus etc but it will crash imediatelly).I've used "svn co <http://svn.freepascal.org/svn/fpc/tags/release_2_6_0> fpc260" , then "fpcmake -Tall" , "make all", "sudo make install", "sudo make CPU_TARGET=x86_64 make install" (of course you need fpc-2.4.4 to build 2.6.0, you can download dmgs from lazarus daily snapshots.
  * Download qt-4.7.4 from nokia site (DO NOT DOWNLOAD 4.8.0 SINCE IT'S WRONGLY PACKAGED atm - won't work (missing symbols in QWebKit) <https://bugreports.qt.nokia.com/browse/QTBUG-23157> )
  * Install qt
  * Download qt bindings source (Qt4Pas).
  * Open Qt4Pas.pro and comment CXXFLAGS += -mstackrealign (since it will be error with 64bit build on mac).
  * Build and install bindings as in readme.
  * dload lazarus trunk.



**Build from terminal (short way)**

  * make bigide CPU_TARGET=x86_64 LCL_PLATFORM=qt OPT="-k'-framework' -k'ApplicationServices'" (don't forget to change default compiler in Tools->Options->Compiler path)



**Build carbon from terminal, then ide from ide**

  * build it for carbon (32bit), startlazarus, go to Tools->Options, change cpu from /usr/local/bin/fpc to /usr/local/bin/ppcx64 in Compiler path (also fix source path to 2.6.0 if it isn't already fixed).
  * Tools->Configure Build Lazarus-> choose LCL widget type qt, in options write -k-framework -kApplicationServices (yes, without any quotes)
  * Build lazarus



**Spotted problems**

  * Qt-cocoa have problems with mouseGrabber (possible it's fixed in 4.8.0)
  * Cannot build pure cocoa widget set ATSUXXXX symbols are missing (probably need to pass more frameworks to linker)



**Build environment**

  * 10.6.8 (Snow Leopard)
  * XCode 3.(Don't know - but it's free one)



### Build your first Qt Application

  * Note: Latest Lazarus IDE creates [Application Bundles](<Application_Bundle.md> "Application Bundle") by himself when you create a new project, so you don't need to worry about this (while you leave the _Create Application Bundle_ check box inside _Project - > Project Options -> Application tab_ to it's default _enabled_ state.


  1. Create a new project or use the default one that Lazarus IDE creates on first launch.
  2. Add some buttons and edit boxes to your form.
  3. Inside _Project - > Project Options -> Compile Options -> Build modes_ click on empty _macro line_ and choose **LCLWidgetType** , in relative _macro value_ choose **qt** and click _Ok_.
  4. Now click green _Run_ arrow and congratulate with yourself for building your first Qt Application.



* * *

## Outdated Build Guide

[Shebuka](</index.php?title=User:Shebuka&action=edit&redlink=1> "User:Shebuka \(page does not exist\)") 17:03, 8 September 2011 (CEST) At this time, this part of guide can't be followed, because all shell script mentioned here to build Interface and Libraries are not present in [Mac library sources](<http://users.pandora.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>). 

Sections 2, 3, and 4 only need to be done if you are creating the Interface library, and Interface framework yourself. If you have installed the interface framework using [Mac Library Binary (intel)](<http://users.pandora.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>), you can start with section 5 and begin compiling the demo programs. 

**Notes on terminology:**

These instructions generally use the term "Qt libraries" to describe the QtCore and QtGui libraries that are created in section 2. The term "interface library" is used to describe the Qt4Intf library created in section 3 that interfaces between Free Pascal and the Qt libraries. The "interface framework" is an macOS-specific folder created in section 4 that contains both the Qt libraries and interface library and can be linked against when compiling with Free Pascal. 

If you're not familiar with macOS terms such as "framework" and "app bundle", refer to Apple's extensive [online documentation](<https://developer.apple.com/documentation/>). 

_**Important!**_ To run any of the scripts mentioned in sections 3-6, you may need to change its permission so that macOS will allow it to be executed: 
    
    
     chmod +x script_name.sh

### Install the Qt Library Frameworks

  * Download and install Qt4 from Trolltech/Nokia: [http:/<ftp://ftp.trolltech.com/qt/source/>]. The file will be named something like qt-mac-opensource-4.5.X.dmg.
  * Several Qt frameworks will be installed in /Library/Frameworks. QtCore.framework and QtGui.framework are used.



### Creating the Qt interface library

A script file for doing this is included with [Mac library source](<http://users.pandora.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>). The downloaded file will be named like mac_qt4pas-V1.XX_Qt4.4.X.zip Download and double-click this file to create folder qt4pas-V1.XX_Qt4.4.X.zip. Move this folder to your home folder and change to it (cd). Run the script as follows: 
    
    
     ./compile_lib_mac.sh

This script will use the GCC g++ (installation of macOS DevSDK (Xcode) is required) compiler to compile qtpas.cpp and create Qt4Intf, the Qt interface library. If the script can't find the Qt4 library frameworks, it will output an error message. The script does not check the exact Qt version, use the appropriate Qt (Qt4.3.X cannot be mixed with Qt 4.4.X) 

### Creating the Qt interface framework

A script file for doing this is included with mac_qt4pas-1.xx.zip. Run the script as follows: 
    
    
     sudo ./create_framework_mac.sh

This creates the Qt4Intf.framework folder under /Library/Frameworks. Note that sudo is required because of the need of write access in the /Library/Frameworks folder. 

If the script can't find the Qt4Intf library it needs, it will output an error message. you need to compile it first (see step above) 

### Compiling the non-Lazarus demo programs with Free Pascal

The Pascal source for several demo programs is included in file [mac_demos-1.xx.zip](<http://users.pandora.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>). Download and double-click this file to create folder demos-1.xx. Move this folder to your home folder (/Users/login) and change to it (cd). 

To compile one of the demo programs: 
    
    
     cd somedemo
     ../compile_exe_fr_mac.sh somedemo
    

This will compile the demo program with Free Pascal, creating an executable file with the same name. 

_**Tip:**_ The compile script uses the compiler option -Fu..(find units in parent directory) to only need a single copy for all demoes of the rather large Qt4.pas file. 

Note that Lazarus is not required to compile these demo programs. 

### Creating a demo program application bundle

To create a double-clickable application on macOS, you need to create an app bundle. An app bundle is a folder with an .app extension in its name that looks like a file to the user. A script file is included for doing this: 
    
    
     ../create_app_mac.sh somedemo

This creates the somedemo.app folder, as well as several folders and files inside it. 

Note that this script does not copy the executable file into the app bundle the way you normally would when you're ready to distribute an app bundle. Instead, it creates a symbolic link inside the folder that points to the executable in the somedemo folder. This approach allows you to edit and compile a program as many times as necessary without needing to copy the executable file into the app bundle folder after each compile in order to run it. 

In Finder, double-click the somedemo application (not the executable file) to launch it. You can also drag and drop the somedemo application onto the dock and launch it from there. 

### Creating a Qt based Lazarus

#### Starting purely from source

Note: The X11 package does not have to be installed at all. 

  * Checkout svn source



in a terminal do 
    
    
    svn co <http://svn.freepascal.org/svn/lazarus/trunk> lazarus
    

  * Compile lazarus using installed Qt4Intf framework
  * adapt USE_QT_45 to the used Qt version
  * since svn >= 21913, Qt 4.5 is default, no need to define any USE_XX_YY when using Qt 4.5


    
    
    cd lazarus
    make clean all LCL_PLATFORM=qt OPT="-dUSE_QT_45 -k'-framework' -k'Qt4Intf' -k'-lobjc' -k'-framework' -k'Foundation'"
    

  * run lazarus :-)


    
    
    ./startlazarus
    

#### Starting from a Gtk based Lazarus

Currently Lazarus is only available for the X11-based GTK widgetset. The compiled LCL units included with Lazarus are only available for the GTK widgetset as well. The LCL units are also installed read-only, so the easiest way of compiling them for the Qt widgetset is simply to copy the LCL source to your home folder and rebuild it there. Here are the steps: 

  * Copy the "lazarus" folder from /usr/local/share to your home folder (/Users/login).



_**Tip:**_ How do you get to the Lazarus folder? In Finder, choose Go | Go to Folder and enter /usr, then navigate to /usr/local/share. 

_**Tip:**_ You can also use Subversion (SVN) to download the Lazarus source to a folder in your home folder (or someplace where you have write access to it). 

_**Tip:**_ For the purposes of these examples, you really only need to copy the Lazarus "lcl" folder, but you'll need to ignore the warnings that Lazarus gives you later on. 

  * Make sure the Qt widgetset is using an up-to-date version of qt4.pas. Compare the version of qt4.pas file used in section 5 above with /Users/login/Lazarus/lcl/interfaces/qt/qt4.pas. The version should be indicated in a comment at the top of qt4.pas. If necessary, install a more recent version of Lazarus.


  * Start Lazarus by opening an X11 window and entering:


    
    
     cd /usr/local/share/lazarus
     ./lazarus
    

  * In Lazarus, choose Tools -> Options -> Environment. On the Files tab, change the Lazarus directory from /usr/local/share/lazarus to


    
    
     /Users/login/Lazarus

If you only copied the "lcl" folder, you'll get a warning when you click OK. Click Ignore to ignore the warning. 

  * In Lazarus, choose Tools | Configure "Build Lazarus". Set Build LCL to "Build"; set all others to "None". Also click "qt" under LCL interface.


  * Click the Build button.


  * When Free Pascal is done compiling the LCL, check that you have newly created .ppu and .o files in /Users/login/Lazarus/lcl/units/powerpc-darwin/qt.


  * Choose Tools | Configure, restore the LCL interface setting to "gtk", then click "Save settings".



### Creating an LCL app that uses the Qt widgetset

  * Create a new project in Lazarus. When you started Lazarus, it created a new project -- you can use this.


  * In Lazarus, choose Project | Compiler Options. On the Paths tab, select "qt" under LCL Widget Type.


  * On the Linking tab, select Pass Options To The Linker and enter


    
    
     -framework Qt4Intf -lobjc -framework Foundation

and click OK. 

  * Add one or more controls to your project's main form. Since the Qt widgetset is a work in progress, consult [Roadmap#Widgetset_dependent_components](<Roadmap.md> "Roadmap") to see which controls are working.



In the meantime qt widgetset becomes more mature, so you can see [Lazarus Qt IDE under macOS](<http://wiki.lazarus.freepascal.org/Image:macosxqt.png>)

  * Save the project to a folder in your home folder (/Users/login).


  * In Lazarus, choose Run | Build to compile the project.



_**Tip:**_ You can also compile your project from a script: 
    
    
    qtunits=~/lazarus/lcl/units/powerpc-darwin
    /usr/local/bin/fpc -dLCL -WG -Sd -gl -O1 -Cirot -k'-framework' -k'Qt4Intf' -k'-lobjc' -Fu$qtunits -Fu$qtunits/qt myproj.lpr
    

You can change most of the switches as needed, but be sure to leave the -k switches alone. If necessary, change the first line to the path to the LCL units you created in the previous section. 

  * Before running the app, make sure you've created an app bundle for it. You can copy the create_app_mac.sh script from section 6 to your project's folder and run it to create an app bundle for your compiled project.


  * To run the app, double-click the application (not the executable file) in Finder or drag it to the dock and launch it from there.


  * Before writing mac apps read [macOS Human Interface guidelines](<https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/>).



* * *

## Distributing an app that uses the Qt Interface Framework

  * The Lazarus IDE creates a symbolic link inside the application bundle to the actual executable file. To copy the executable file into the application bundle in order to distribute it along with the application bundle, do this:


    
    
     cd /path/to/your/application/bundle
     rm MyApp.app/Contents/MacOS/MyApp
     cp -p MyApp MyApp.app/Contents/MacOS/MyApp
    

You should substitute the name of your application for "MyApp" above. 

  * Consult [Deploying Your Application#An Application Bundle](<Deploying_Your_Application.md> "Deploying Your Application") for help with creating a distributable disk image file (.dmg extension) from your application bundle folder. Ignore the discussion of X11 and GTK since your app does not require them!


  * Your application will depend on the Qt4 Framework Library and on the Qt4 Interface Binary Bindings. You can put these files inside your application bundle like you do with .dylib following [macOS Programming Tips#Using a library in a Mac application](<macOS_Programming_Tips.md> "macOS Programming Tips").


  * Read [Deploying an Application on macOS](<http://qt-project.org/doc/qt-4.8/deployment-mac.html>) about adding the Qt framework to your application bundle. Very handy for that purpose is the tool _macdeployqt_.



## Uninstalling the Qt4 Interface and Qt4 Framework

  * If you need to remove the Qt4 Interface from your computer, simply drag the Qt4Pas.framework folder from /Library/Frameworks to the trash. Finder will prompt you for your password before allowing this.


  * If you need to remove the Qt4 Framework from your computer, simply drag the Qt3Support.framework and all QtXxxx.framework folders (like QtCore.framework, QtGui.framework, QtWebKit.framework ecc.) from /Library/Frameworks to the trash. Finder will prompt you for your password before allowing this.


  * (Optional) You can also drag Qt4 Free Pascal Interface.pkg (if present) from /Library/Receipts to the trash.

---

_Source: [https://wiki.freepascal.org/Qt_Interface_Mac](https://web.archive.org/web/20240225152135/https://wiki.freepascal.org/Qt_Interface_Mac)_
