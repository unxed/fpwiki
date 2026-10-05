# Installing Lazarus on FreeBSD

│ **English (en)** │

## Contents

  * 1 Installing Lazarus on FreeBSD
    * 1.1 via the Ports tree
    * 1.2 via the pkg system
    * 1.3 via the Lazarus repository
    * 1.4 What does the bigide make argument do?
  * 2 Installation troubleshooting
  * 3 Multiple Lazarus installs
  * 4 Lazarus FAQ
  * 5 Installing old versions



## Installing Lazarus on FreeBSD

The following applies to FreeBSD 13.x (i386, amd64), 14.x (i386, amd64) and 15-CURRENT (aarch64, amd64) only. Earlier FreeBSD versions are end-of-life and not supported. 

### via the Ports tree

Currently, two versions of Lazarus are available in the FreeBSD ports tree v3.6.0 (release version) and v4.99 (development version). The latest version of the Free Pascal Compiler in the ports tree is v3.2.3 and v3.3.1 (development version). You can install the GTK2 version, QT5 version _or_ QT6 version of Lazarus as follows: 
    
    
    # cd /usr/ports/editors/lazarus && make install clean
    

or 
    
    
    # cd /usr/ports/editors/lazarus-qt5 && make install clean
    

or 
    
    
    # cd /usr/ports/editors/lazarus-qt6 && make install clean
    

If you want use development (aka trunk) version of Lazarus do the following: 
    
    
    # cd /usr/ports/editors/lazarus-devel && make install clean
    

or 
    
    
    # cd /usr/ports/editors/lazarus-qt5-devel && make install clean
    

or 
    
    
    # cd /usr/ports/editors/lazarus-qt6-devel && make install clean
    

Note: Installing Lazarus will also install the Free Pascal Compiler and its source files. 

### via the pkg system
    
    
    # pkg install editors/lazarus
    

or 
    
    
    # pkg install editors/lazarus-qt5
    

or 
    
    
    # pkg install editors/lazarus-qt6
    

or 
    
    
    # pkg install editors/lazarus-devel
    

or 
    
    
    # pkg install editors/lazarus-qt5-devel
    

or 
    
    
    # pkg install editors/lazarus-qt6-devel
    

Lazarus should install the Free Pascal Compiler source files. If you don't have them, you can add them: 
    
    
    # pkg install lang/fpc-source
    

If you start Lazarus IDE at this point by typing **lazarus** you will get a dialog which needs you to input the directory in which the Free Pascal source files are located. Set it to **/usr/local/share/fpc-source-3.2.3**

### via the Lazarus repository

This option will often be used if you want to follow Lazarus trunk, a fixes branch, or some other release (eg compiling from a source tarball). 

  * Use the SubVersion or Git repositories to checkout a copy of the source code you want, or unpack a downloaded source archive into a suitable location. Recent versions of FreeBSD include the **svn** command as **svnlite** , so you do not need to install full Subversion package to checkout a copy of the source code.


  * The readme.txt file in Lazarus directory mentions **make clean all**. This only works if you are using Linux. Under FreeBSD you need to replace **make** with **gmake** and ensure that you have also installed the GNU gmake from ports.


    
    
      cd /path/to/lazarus_source
      gmake distclean all bigide
    

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

## Installation troubleshooting

Troubleshooting details that should (hopefully) be applicable across platforms may be found in the article [Installation Troubleshooting](<Installation_Troubleshooting.md> "Installation Troubleshooting"). 

Some additional notes for FreeBSD installations can be found in the articles [FreeBSD](<FreeBSD.md> "FreeBSD") and [Installing the Free Pascal Compiler](<Installing_the_Free_Pascal_Compiler.md> "Installing the Free Pascal Compiler"). 

## Multiple Lazarus installs

Please see [Multiple Lazarus](<Multiple_Lazarus.md> "Multiple Lazarus") for details on having more than one Lazarus version installed on one system. We cover issues that can arise due to multiple Lazarus installs here, because they can also happen when installing over a previous version. 

## Lazarus FAQ

The Lazarus FAQ - Frequently Asked Questions - page is available [here](<Lazarus_Faq.md> "Lazarus Faq"). 

## Installing old versions

See [Installation hints for old versions](<Installation_hints_for_old_versions.md> "Installation hints for old versions")

---

_Source: [https://wiki.freepascal.org/Installing_Lazarus_on_FreeBSD](https://web.archive.org/web/20250417153228/https://wiki.freepascal.org/Installing_Lazarus_on_FreeBSD)_
