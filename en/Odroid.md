# Odroid

## Contents

  * 1 Overview
    * 1.1 U2, U3
      * 1.1.1 Installing FPC/Lazarus using fpcup
      * 1.1.2 Notes
  * 2 XU
  * 3 See also



## Overview

Odroid are ARM devices manufactured by HardKernel. 

### U2, U3

These devices contain a Samsung ARM chip 

  * instruction set: ARMv7A
  * floating point coprocessor (FPU): VFPv3



Operating systems: Xubuntu (ARMHF; hardfloat), Android. 

#### Installing FPC/Lazarus using fpcup

This section focuses on using Linux on the Odroids. 

  * FPC 2.6.x



As the provided Linux is ARMHF, standard FPC 2.6.x is not suitable (insufficient ARMHF support0. The included Xubuntu Linux probably supplies modified FPC 2.6.x+Lazarus (much like the Raspbian distribution). 

  * FPC trunk



The easiest way to install FPC trunk and Lazarus is to run [fpcup](<fpcup.md> "fpcup") which will use a 2.6.x bootstrap compiler to build a trunk ARMHF intermediate compiler. Now the regular FPC and Lazarus environments are built by fpcup. 

Note that fpcup is a thin wrapper around the regular svn/make FPC/Lazarus install process, so if you're uncomfortable with this process, it may not be for you. 

Instructions - to be run as a regular user: 

  * Install prerequisites for Lazarus and subversion, e.g. something like:


    
    
    sudo apt-get install build-essential libgtk2.0-dev libgdk-pixbuf2.0-dev libpango1.0-dev subversion
    

  * Download fpcup_linux_armhf (or perhaps fpcup_linux_arm) and fpcupodroid.ini into a directory (say ~)


    
    
    cd ~
    wget https://bitbucket.org/reiniero/fpcup/downloads/fpcup_linux_armhf
    wget https://bitbucket.org/reiniero/fpcup/downloads/fpcupodroid.ini
    

Note: you may need to edit fpcupodroid.ini. Please see warning below about what FPC/Lazarus revisions work. 

  * Make fpcup executable:


    
    
    chmod ug+rx fpcup_linux_armhf #replace filename with proper name if different
    

  * Run fpcup, telling it to use the [General] profile in fpcupodroid.ini:


    
    
    ./fpcup_linux_armhf --inifile=fpcupodroid.ini #replace filename with proper name if different
    

For a full make distclean/install etc session: 
    
    
    ./fpcup_linux_armhf --inifile=fpcupodroid.ini --inisection=full #replace filename with proper name if different
    

fpcup will create desktop shortcuts etc. For more details, please see the [fpcup](<fpcup.md> "fpcup") wiki page. 

You can also download and run fpcupgui next to fpcup in order to run fpcup via a GUI. 

#### Notes

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Serious problems compiling with current (March 2014) FPC trunk. 

  * Please see e.g. <http://www.mail-archive.com/fpc-pascal@lists.freepascal.org/msg36074.html>
  * FPC revision r26550 together with Lazarus revision 43823 seems to work. Other versions may also work. Please use fpcrevision and lazrevision options; both when compiling fpcup and when using fpcup to compile FPC/Lazarus




* currently fpcup will do a make distclean before installing/updating, which may stress your memory card. Experimental fix in [update] section (see above)... however, the creation of an intermediate ARMHF compiler in fpcup will probably still clean out e.g. the units directory (make cycleclean or something is called in the make script for the cross compiler).
* (general fpcup problem) you may have to rebuild Lazarus yourself (Tools/Build Lazarus) to get all packages/controls fpcup installed for you.

## XU

The Odroid XU can run e.g. Ubuntu. 

Michel Catudal provides Ubuntu images that include Lazarus/FPC; see <http://home.comcast.net/~mcatudal/>

_more details welcome_

## See also

  * [fpcup](<fpcup.md> "fpcup") Main fpcup page. Details on settings, troubleshooting, bug reporting, cross compilers etc.
  * [Lazarus on Raspberry Pi](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi") Raspbian also is ARMHF but has an older ARM processor architecture.

---

_Source: [https://wiki.freepascal.org/Odroid](https://web.archive.org/web/20250124211858/https://wiki.freepascal.org/Odroid)_
