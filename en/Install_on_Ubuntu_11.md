# Install on Ubuntu 11.10

[![Logo-ubuntu cof-orange-hex.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/a/ab/Logo-ubuntu_cof-orange-hex.svg/50px-Logo-ubuntu_cof-orange-hex.svg.png)](</File:Logo-ubuntu_cof-orange-hex.svg>)

This article applies to [Ubuntu](</Category:Ubuntu> "Category:Ubuntu") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 In General
    * 1.1 Overview
    * 1.2 Versions
  * 2 Ubuntu 11.10 (Oneiric Ocelot)
    * 2.1 What you want
    * 2.2 Find and install the packages
    * 2.3 Problem 1
    * 2.4 Solution (10 mins): Remove the Ubuntu taskbar menu and overlay scrollbars
    * 2.5 Problem 2
    * 2.6 Solution (10 mins): Reconfigure Lazarus as sudo
    * 2.7 Lazarus Forum
    * 2.8 Video
    * 2.9 Problem 2 image
  * 3 Ubuntu 10.04 LTS (Lucid Lynx)
    * 3.1 Install from .deb files
  * 4 See also



## In General

### Overview

To install Lazarus you install three things: 

  * fpc - the Free Pascal compiler
  * fpc-src - the source code for Free Pascal
  * Lazarus -the IDE for Free Pascal



### Versions

You must install the correct version of Free Pascal for the version of Lazarus you choose. The versions below are correct. 

## Ubuntu 11.10 (Oneiric Ocelot)

### What you want

Matching versions of Lazarus and Free Pascal are in the repository (20/02/12). They are 

  * Lazarus 0.9.30-2.
  * Free Pascal 2.4.4



In Synaptic they are called Lazarus 0.9.30-2Build1, fpc, and fp. 

### Find and install the packages

In the Software Centre search for and install: 

  * IDE for FreePascal - SDK metapackage - search under lazarus
  * Free Pascal - SDK metapackage - search under fpc
  * FreePascal - SDK source code metapackage- search under fpc



With Synaptic, install everything beginning with Laz, fpc or fp. 

### Problem 1

Once you have installed Lazarus, it will not work. Even if you add no code at all and compile and run everyone's favourite 'Blank form' project (having first saved it of course) you will get this error message: 
    
    
    Project project1 raised Exception class EInterface critical with message: ....
    

### Solution (10 mins): Remove the Ubuntu taskbar menu and overlay scrollbars

  * Remove the taskbar



Open a terminal and try 
    
    
    sudo apt-get remove appmenu-gtk3 appmenu-gtk appmenu-qt
    

Give your password These can be re-installed if needed with the same command but put 'install' in place of 'remove'. 

  * Remove the Ubuntu overlay scrollbars



Open synaptic and search for 'liboverlay-scrollbar'. 

Uninstall the 2 packages highlighted. They can be re-installed if needed with Synaptic 
    
    
    _You now need to restart Ubuntu and hope._
    

**The 'blank form' project should now run with no errors.**

### Problem 2

Add a button to your 'blank form' project. In the _Events_ tab of the _Object Inspector_ , try to open the 'Click' event handler. You will get the error message 
    
    
    unit not found: Classes
    

or 
    
    
    unit not found: Sysutils
    

### Solution (10 mins): Reconfigure Lazarus as sudo

This can be solved as follows: 

  * Open Lazarus as sudo



Open a terminal and start Lazarus as sudo with 
    
    
    gksudo StartLazarus
    

Give your sudo password 

  * Rescan the Pascal source directory



**In Lazarus** choose 
    
    
    _Tools - > Rescan FPC source directory_
    

  * Rebuild Lazarus



**In Lazarus** choose 
    
    
    _Tools - > BuildLazarus with profile:Build all_
    

  * Close Lazarus.



_You should now be able to open Lazarus normally and create a project with a button that is clickable!'_

'Hello World' here we come. 

_Oh Joy!_

### Lazarus Forum

There are several threads that are useful 

[Forgetting to install Free Pascal source code](<http://www.lazarus.freepascal.org/index.php/topic,16062.0.html>)

[Solving problem 1](<http://www.lazarus.freepascal.org/index.php/topic,14982.0.html>)

[Solving problem 2](<http://www.lazarus.freepascal.org/index.php/topic,14672.0.html>)

[More about problem 2](<http://www.lazarus.freepascal.org/index.php/topic,15005.0.html>)

[Installing using debs](<http://www.lazarus.freepascal.org/index.php/topic,14672.0.html>)

### Video

It's in German. It is very clear. In this case, after the installation, Lazarus ran without problem - mine didn't. 

[You tube showing install](<http://www.youtube.com/watch?v=fgnzdegIB40>)

### Problem 2 image

[![LazarusErrorSmall.jpg](https://wiki.freepascal.org/images/a/a2/LazarusErrorSmall.jpg)](</File:LazarusErrorSmall.jpg>)

## Ubuntu 10.04 LTS (Lucid Lynx)

The version in this repository is Lazarus 0.9.28.2 which is quite old. It installs via synaptic and it then works. 

### Install from .deb files

If you want to install a newer version using .deb files then see the wiki page 

[ How to install from .deb files](<Install_on_Ubuntu_from_.md> "Install on Ubuntu from .deb files")

## See also

  * [Install on Ubuntu from .deb files](<Install_on_Ubuntu_from_.md> "Install on Ubuntu from .deb files")

---

_Source: [https://wiki.freepascal.org/Install_on_Ubuntu_11.10](https://web.archive.org/web/20190918183414/https://wiki.freepascal.org/Install_on_Ubuntu_11.10)_
