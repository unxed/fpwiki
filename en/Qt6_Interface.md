# Qt6 Interface

[Template:Qt6 Interface](</index.php?title=Template:Qt6_Interface&action=edit&redlink=1> "Template:Qt6 Interface \(page does not exist\)")

[![Qt logo.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Qt_logo_2013.svg/60px-Qt_logo_2013.svg.png)](</File:Qt_logo_2013.svg>)

This article applies to [Qt6 widgetset](</Category:Qt> "Category:Qt") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Introduction
    * 1.1 Versions
    * 1.2 Linux
    * 1.3 Windows
    * 1.4 Mac
      * 1.4.1 Requirements
  * 2 Troubleshooting on compiling on Linux
  * 3 Troubleshooting on running the Linux app



## Introduction

This interface is based on Qt 6 (Qt 6.2.x LTS). For documentation, fixes and download, go to [Qt Project](<http://qt-project.org/>) (Installers at [download 6.2.4](<https://download.qt.io/archive/qt/6.2/6.2.4/single/>)). Lazarus with Qt6 interface (qt6-lcl) can be used on Windows 32/64, Linux x32/x64/arm, macOS x64 (Cocoa). Qt6 widgetsets will be available in Lazarus from 3.0 stable release. Currently you can use it from trunk and fixes_3_0. Note that with our Qt6Pas you can use any Qt lib version >= 6.2.x. 

### Versions

As Qt6 is relatively new, look for it in newer distributions. It is believed that all Qt6 releases around at present based on 6.2 and 6.3 have a [known bug](<https://forum.lazarus.freepascal.org/index.php/topic,61295.msg460907.html>) that causes a 20 second delay before a Save Dialog is opened. Hopefully fixes will be released at some stage in the near future. At the time of writing, Nov 22, the ones I tested had - 

  * Debian Testing - 6.3.1 (has Save Dialog Problem). Qt6 6.4.1 is in experimental so may not be too far away.
  * Ubuntu 22.04 - 6.2.4 (has Save Dialog Problem).
  * Ubuntu 22.10 - 6.2.4 (has Save Dialog Problem).
  * Fedora 37 - 6.3.1 (has Save Dialog Problem).



Please, if you have tested other distributions, add them to this list ! 

### Linux

Most current short term Linux distros have a suitable Qt6 in their standard repositories, but LTS ones like Debian Bullseye and Ubuntu 20.04 do not. It is sometimes possible to install from other repos or as kits from Qt themselves but it might be a bit of a task. 

Further, until Lazarus 3.0 is released and makes it into your Distribution of choice, you must build libQt6Pas on your own, or download pre-built (all care, no responsibility) packages for some platforms from 'Releases' of [this GitHub page](<https://github.com/davidbannon/libqt6pas/>). 

### Windows

Qt does not provide 32bit builds of Qt6, so download 64bit Qt >= 6.2.x and build your own Qt6Pas1.dll 64bit. 

### Mac

Download Qt6 library and install, build your own Qt6Pas framework. 

#### Requirements

To just run a Qt6 app, you need, perhaps (TBC) only libqt6core6, libqt6gui6, libqt6printsupport6, libqt6widgets6 and qt6-qpa-plugins. They must be version 6.2.3 or later. Your particular app may need several more libraries of course, this is just the basic requirements. It will typically need a download of around 10Meg and additional disk usage of 33Meg. 

To build Qt6 Lazarus apps, you need qt6-base-dev (deb based) or qt6-qtbase-devel (rpm based). Assuming you already have the run requirements, FPC and Lazarus installed, this will need an additional 4meg download and will use up an additional 28Meg of diskspace. 

Note that the deb packages on the [Unofficial libQt6Pas](<https://github.com/davidbannon/libqt6pas/>) Github site (click "releases", right hand side) will take care of the dependencies mentioned above. 

## Troubleshooting on compiling on Linux

If you don't download libQt6Pas library, FPC will give the error on compiling your Qt6 app, something about "cannot link to library Qt6Pas". This means it cannot find the "libQt6Pas.so" file. By far the best approach is to either build the library yourself and install it in the proper place or download and install the pre-built ones mentioned above. 

Putting libraries in non standard places and trying to point to them is never a good idea on Unix system. However, you could download the tar.gz file from Github, untar and put the library and two symlinks, "libQt6Pas.so" and "libQt6Pas.so.6.2"in the folder of "fpc" binary. 

Then FPC must get past the libQt6Pas step, but it can fail further on if you have not installed the general Qt6 libraries mentioned above. 

## Troubleshooting on running the Linux app

If you compile the app OK, but cannot run the binary, try to run it again from the Terminal. You may see errors about missing .so files. To fix it, unpack libQt6Pas.so* files from [GitHub page](<https://github.com/davidbannon/libqt6pas/>) to e.g. ~/qt6pas. Then open file "~/.bashrc" (I assume OS uses Bash) and add there: 
    
    
    LD_LIBRARY_PATH=~/qt6pas
    export LD_LIBRARY_PATH
    

Create additional symlink(s) there too, if the error tells about concrete filename like "libQt6Pas.so.1.0.2". 

Do log-out and log-in. Now Qt6 binary must find all .so files.

---

_Source: [https://wiki.freepascal.org/Qt6_Interface](https://web.archive.org/web/20250514221642/https://wiki.freepascal.org/Qt6_Interface)_
