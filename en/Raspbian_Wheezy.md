# Raspbian

│ **English (en)** │

[![Raspberry Pi Logo.png](https://wiki.freepascal.org/images/8/85/Raspberry_Pi_Logo.png)](</File:Raspberry_Pi_Logo.png>)

This article applies to [Raspberry Pi](</Category:Raspberry_Pi> "Category:Raspberry Pi") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Content here is quite out of date. As the Raspberry Pi OS is just another Debian based distribution, the reader is referred to [Installing_Lazarus_on_Linux](<Installing_Lazarus_on_Linux.md> "Installing Lazarus on Linux") and [Lazarus_on_Raspberry_Pi](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi"), those page are kept up to date and contains everything you need. (Please don't update this page unless you are committed to maintaining it in the future) 

**Raspbian** , now called [Raspberry Pi OS](<https://www.raspberrypi.com/software/>), is a free operating system based on the [Debian](<https://debian.org>) [Linux](<Linux.md> "Linux") distribution optimized for the [Raspberry Pi](<Raspberry_Pi.md> "Raspberry Pi") hardware. Most historic and current versions of Raspbian support both [Free Pascal](<Free_Pascal.md> "Free Pascal") and [Lazarus](<Lazarus.md> "Lazarus"). 

## Contents

  * 1 Installing Lazarus and Free Pascal
    * 1.1 Graphical Installer on modern versions of Raspbian
  * 2 Installing via the command shell on older versions of Raspbian
    * 2.1 Installing Free Pascal
    * 2.2 Installing Lazarus
  * 3 Compiling from sources
  * 4 Screenshots
  * 5 Hardware access
  * 6 External Links



## Installing Lazarus and Free Pascal

### Graphical Installer on modern versions of Raspbian

On modern versions of Raspbian installation is very easy. It can be performed with the PiPackage manager. You have to select simply "Add / Remove Software" in the global Preferences menu. 

  * [![Step 1: Install Free Pascal with PiPackage.](https://wiki.freepascal.org/images/1/18/Installing_FPC_on_Raspbian_Stretch.jpeg)](</File:Installing_FPC_on_Raspbian_Stretch.jpeg> "Step 1: Install Free Pascal with PiPackage.")

Step 1: Install Free Pascal with PiPackage. 

  * [![Step 2: Install Lazarus](https://wiki.freepascal.org/images/e/e6/Installing_Lazarus_on_Raspbian_Stretch.jpeg)](</File:Installing_Lazarus_on_Raspbian_Stretch.jpeg> "Step 2: Install Lazarus")

Step 2: Install Lazarus 

  * [![Lazarus is now available in the global "Programming" menu](https://wiki.freepascal.org/images/8/8e/Running_Lazarus_from_the_%22Programming%22_menu_on_Raspbian_Stretch.jpeg)](</File:Running_Lazarus_from_the_%22Programming%22_menu_on_Raspbian_Stretch.jpeg> "Lazarus is now available in the global "Programming" menu")

Lazarus is now available in the global "Programming" menu 

  * [![A simple session with Lazarus on Raspbian Stretch.](https://wiki.freepascal.org/images/9/9a/Lazarus_1_6_on_Raspbian_Stretch.jpg)](</File:Lazarus_1_6_on_Raspbian_Stretch.jpg> "A simple session with Lazarus on Raspbian Stretch.")

A simple session with Lazarus on Raspbian Stretch. 




## Installing via the command shell on older versions of Raspbian

### Installing Free Pascal

Free Pascal is easily installed with the following shell commands: 
    
    
      sudo apt-get update
      sudo apt-get upgrade
      sudo apt-get install fpc
    

There are three modes to use Free Pascal on Raspbian: 

  * via the shell command `fpc`. This requires to enter a number of options along with the `fpc` command.
  * via the shell command `fp`. This command starts a text-based IDE.
  * via Lazarus, see below



### Installing Lazarus

The steps to install Lazarus are very similar to those required for installing Free Pascal: 
    
    
      sudo apt-get update
      sudo apt-get upgrade
      sudo apt-get install fpc
      sudo apt-get install lazarus
    

This installs a ready-to-use precompiled version of Lazarus, however not necessarily the newest one. 

## Compiling from sources

The newest versions of Lazarus are distributed as source code. 

To compile current FPC and Lazarus sources for Raspbian Buster (based on Debian 10 Buster), see [Build current FPC and Lazarus for Raspbian](<Build_current_FPC_and_Lazarus_for_Raspbian.md> "Build current FPC and Lazarus for Raspbian"). 

  
In order to compile Lazarus from subversion sources see [Michell Computing: Lazarus on the Raspberry Pi](<http://www.michellcomputing.co.uk/blog/2012/11/lazarus-on-the-raspberry-pi/>) for details. The information there is somewhat outdated but still usable. 

  
The easiest way to compile from source is to use [fpcup](<https://github.com/LongDirtyAnimAlf/Reiniero-fpcup>) . See also [fpcup](<fpcup.md> "fpcup") in this wiki. 

## Screenshots

  * [![Lazarus on Raspbian Wheezy](https://wiki.freepascal.org/images/e/ef/Lazarus_on_Raspberry_Pi_Raspian_Wheezy_version_2012-10-28.png)](</File:Lazarus_on_Raspberry_Pi_Raspian_Wheezy_version_2012-10-28.png> "Lazarus on Raspbian Wheezy")

Lazarus on Raspbian Wheezy 

  * [![Free Pascal text mode IDE on Raspbian Wheezy](https://wiki.freepascal.org/images/4/43/fp_raspian.png)](</File:fp_raspian.png> "Free Pascal text mode IDE on Raspbian Wheezy")

Free Pascal text mode IDE on Raspbian Wheezy 




## Hardware access

See [Lazarus on Raspberry Pi](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi") for details. 

## External Links

  * [Raspberry Pi Foundation](<http://www.raspberrypi.org>)
  * [Official Raspbian site](<http://www.raspbian.org>)
  * [Additional information on Lazarus and Raspberry Pi at eLinux.org](<http://www.elinux.org/Lazarus_on_RPi>)

---

_Source: [https://wiki.freepascal.org/Raspbian_Wheezy](https://web.archive.org/web/20250215141701/https://wiki.freepascal.org/Raspbian_Wheezy)_
