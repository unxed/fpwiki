# Qt4 Maemo Binding

[![Qt logo.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Qt_logo_2013.svg/50px-Qt_logo_2013.svg.png)](</File:Qt_logo_2013.svg>)

This article applies to [Qt widgetset](</Category:Qt> "Category:Qt") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

This page describes the Free Pascal Qt4 Binding for Maemo. 

Currently Maemo5 on the Nokia N900 version PR1.2 is supported. The binding is an interface to Qt 4.6.2 of the PR1.2 release. 

## Contents

  * 1 Introduction
  * 2 Links
  * 3 Compilation of the binding
  * 4 Installation on the N900 device
  * 5 Cross compile FPC/Qt4 for N900
    * 5.1 Setup Cross Compilation
    * 5.2 Compile and Run the Binding Maemo Demo
  * 6 Cross compile a Lazarus LCL/Qt4 project for the N900
    * 6.1 Cross Compile the LCL
    * 6.2 Create a Lazarus LCL/Qt4 N900 program



# Introduction

To develop with C++ Qt for the N900, Nokia provides a scratchbox based SDK: [Maemo5 SDK Installation](<http://wiki.maemo.org/Documentation/Maemo_5_Final_SDK_Installation>)

The Free Pascal binding is technically just a Qt C++ library, so to compile this binding library, the scratchbox SDK is needed. 

To develop with Free Pascal Qt for the N900, you do not need to install the complex scratchbox environment, you can use the provided binding binary. 

# Links

  * [Pascal Qt4 binding home page](<http://users.telenet.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>)
  * [Setup Cross Compile For ARM](<Setup_Cross_Compile_For_ARM.md> "Setup Cross Compile For ARM") This wiki page has some overlapping information with this one.



  


# Compilation of the binding

These instructions explain how to compile the Free Pascal Qt4 Binding for Maemo sources. If you use the provided binary of the binding, this is not necessary. 

  * Install the scratchbox based Maemo5 SDK: [Maemo5 SDK Installation](<http://wiki.maemo.org/Documentation/Maemo_5_Final_SDK_Installation>)
  * After installation, start as root scratchbox: /scratchbox/sbin/sbox_ctl start
  * As normal user login: /scratchbox/login
  * select using sb-menu the arm environment
  * install the Qt4 devel package 
    * fakeroot apt-get update
    * fakeroot apt-get install libqt4-dev
  * download (e.g. using wget) and unpack the sources
  * cd into the sources directory
  * qmake (creates Makefile)
  * make (compiles)
  * strip LibraryFileName (reduce filesize)
  * fakeroot make install (installs in scratchbox)



# Installation on the N900 device

These instructions explain how to install the Free Pascal Qt4 Binding for Maemo on the Nokia N900 device. 

How to setup a wifi or usb access to the device is out of the scope (see Nokia instructions) The instructions assume a working ssh access to the N900 using hostname n900. 

First obtain the binding library (libQt4Pas.so.6.2.2) either from the [provided binary](<http://users.telenet.be/Jan.Van.hijfte/qtforfpc/maemo5_bin-qt4pas-V2.2_Qt4.6.2.tar.gz>) or from compilation within scratchbox. 

Note that it is possible to setup ssh from within scratchbox and ssh from scratchbox to the N900. You can also access the compiled library that resides in scratchbox outside the scratchbox environment. If you installed scratchbox to /scratchbox, then the scratchbox user home directory is located at /scratchbox/users/username/home/username/. 

  


  * Copy the library to the device opt directory ([optify](<http://wiki.maemo.org/Opt_Problem#The_.2Fopt_Problem>))


    
    
    scp libQt4Pas.so.6.2.2 root@n900:/opt/lib/
    

  * login to the device as root


    
    
    ssh root@n900
    

  * create symlinks


    
    
    ln -s /opt/lib/libQt4Pas.so.6.2.2 /usr/lib/libQt4Pas.so.6
    ln -s /opt/lib/libQt4Pas.so.6.2.2 /usr/lib/libQt4Pas.so
    

# Cross compile FPC/Qt4 for N900

These instructions explain how to cross compile a Free Pascal Qt4 Maemo program from linux to N900. 

## Setup Cross Compilation

It is strongly advised to read the Free Pascal Docmumentation about cross compilation to understand what you do : [Build Faq](<http://www.stack.nl/~marcov/buildfaq.pdf>)

The scratchbox SDK is not needed, only N900 binutils and some N900 device libraries are needed and will be copied to a directory named /arm 

  

    
    
    mkdir /arm ; cd /arm
    wget http://users.telenet.be/Jan.Van.hijfte/qtforfpc/n900-binutils.tar.gz
    tar zxvf n900-binutils.tar.gz
    rm -f n900-binutils.tar.gz
    mkdir lib 
    scp root@n900:/lib/* lib/             
    mkdir -p usr/lib
    scp root@n900:/usr/lib/* usr/lib/
    

There will be some errors about the sub directories not being copied, these are not necessary as there is little chance you will use the libs residing there. 

When compiling and linking, you may have errors like ...skipping incompatible.. and ...cannot find -lX11..., to solve this, you just create some extra links (instead of installing the devel packages of X11) 
    
    
    cd /arm/usr/lib/
    ln -s libX11.so.6.2.0 libX11.so
    cd /arm/lib/
    ln -s libc.so.6 libc.so
    ln -s libpthread.so.0 libpthread.so
    

  
Make sure you already have a recent x86 fpc installed and a fpc subversion trunk available in e.g. /svn/fpc 
    
    
    cd /svn/fpc
    make distclean
    make crossinstall CPU_TARGET=arm OPT="-dFPC_ARMEL" BINUTILSPREFIX=arm-linux- CROSSBINDIR=/arm/binutils/ INSTALL_PREFIX=/usr
    ln -sf /usr/lib/fpc/2.5.1/ppcrossarm /usr/bin/ppcarm
    

Add an arm section before the Parsing Switches section into fpc.cfg 
    
    
    #IFDEF CPUARM
     -g
     -gl
     -O-
     -dFPC_ARMEL 
     -dMAEMO5
     -XParm-linux-
     -FD/arm/binutils/
     -Xd
     -Fl/arm/lib
     -Fl/arm/usr/lib
     -XR/arm/
     -Xr/arm/lib/
     -Xr/arm/usr/lib/
     -k-rpath /arm/lib
     -k-rpath /arm/usr/lib
     -k-L/arm/lib
     -k-L/arm/usr/lib
    #ENDIF

## Compile and Run the Binding Maemo Demo

Download Demo source and compile 
    
    
    wget http://users.telenet.be/Jan.Van.hijfte/qtforfpc/maemo5_demos-V2.2_Qt4.6.2.tar.gz
    tar zxvf maemo5_demos-V2.2_Qt4.6.2.tar.gz
    cd demos-V2.2_Qt4.6.2
    cd maemowidgets
    ppcarm maemowidgets
    

Transfer executable on device and run 
    
    
    scp maemowidgets root@n900:/opt/
    ssh root@n900
    cd /opt
    ./maemowidgets
    

The demo shows some [Maemo specific Qt classes](<http://doc.qt.nokia.com/qt-maemo-4.6/qtmaemo5.html>). 

# Cross compile a Lazarus LCL/Qt4 project for the N900

These instructions explain how to cross compile a Lazarus LCL/Qt4 project for the N900. 

## Cross Compile the LCL

First ensure the Free Pascal cross arm setup is ready and tested as described above. 

Then compile on i386 a recent svn trunk Lazarus LCL/Qt4. 

The Free Pascal Maemo binding, is actually a binding to Qt 4.6.2 of Maemo PR 1.2. This Qt for the embedded target N900 is configured and compiled to reduce the footprint. Some Qt classes and methods that are not necessary on the N900 have been removed. The Maemo Qt on the N900 does not have support for e.g. Printer, a Trayicon and some function calls marked as deprecated. To be able to compile Lazarus LCL/Qt, a patch is therefore necessary. [Coordination](<http://lists.lazarus.freepascal.org/pipermail/qt/2010-June/001547.html>) is ongoing to integrate the patch into svn. The patch was created on Lazarus svn 25956. 

Apply the patch to a lazarus trunk svn that e.g. resides in the directory /svn/lazarus: 
    
    
    cd /svn
    wget http://users.telenet.be/Jan.Van.hijfte/qtforfpc/maemo.diff.txt.tar.gz
    tar zxvf maemo.diff.txt.tar.gz
    cd lazarus
    patch -p0 < ../maemo.diff.txt 
    make PREFIX=/usr clean all LCL_PLATFORM=qt OPT="-gl"
    

The last command compiles lazarus. 

Then start lazarus and go to 'Tools' / 'Configure "Build Lazarus"'. Check Build for 'LCL' and 'Package Integration', set Target Cpu to arm and click build. 

The above fpc.cfg will ensure that the necessary settings for the N900 will be applied. The libraries will be searched in /arm/... , the cpu option armel will be defined and using the define MAEMO5, the patch will ensure LCL/Qt is able to compile. 

## Create a Lazarus LCL/Qt4 N900 program

Start lazarus, choose Project, New Project and save the project somewhere. Then use Project, Project Options, Code Generation and set Target CPU to arm. Just Compile the project and the resulting executable (e.g. project1) will be able to run on the N900. 

The Maemo Qt on the N900 is styled very differently than the Desktop Qt, this is to achieve touchscreen finger friendliness. The change the default style and preserve designer proportions of the widgets, start the program like this: 
    
    
    ./project1 -style plastik

---

_Source: [https://wiki.freepascal.org/Qt4_Maemo_Binding](https://web.archive.org/web/20240804124232/https://wiki.freepascal.org/Qt4_Maemo_Binding)_
