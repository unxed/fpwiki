# Install fpc on Raspberry with Gentoo

[![Raspberry Pi Logo.png](https://wiki.freepascal.org/images/8/85/Raspberry_Pi_Logo.png)](</File:Raspberry_Pi_Logo.png>)

This article applies to [Raspberry Pi](</Category:Raspberry_Pi> "Category:Raspberry Pi") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Introduction
  * 2 What kind of install?
  * 3 Install the latest version of fpc from repository
  * 4 Manually install the latest version of fpc
  * 5 Installing subversion
  * 6 Downloading fpc trunk sources
  * 7 Before building
  * 8 Build the compiler



# Introduction

This brief guide describes the steps I took to install the fpc free pascal compiler on my raspberry with gentoo. I could not find guides to install free pascal (not Lazarus) on a raspberry with gentoo, so, I want to share my experience. I followed several guides found on the net, especially this: <http://www.michellcomputing.co.uk/blog/2012/11/lazarus-on-the-raspberry-pi> I also searched (and found) help in both free pascal and gentoo forums and, in both cases, I received very valuable aid. 

If you are interested to the result, here you can find and download the SD image for your raspberry. <https://sourceforge.net/projects/pastoo/?source=navbar>

# What kind of install?

Below I describe the method I adopted to get a linux distro with these features: 

  * Minimum linux installation
  * No X server, no desktop manager
  * Software installed: ssh, subversion and git
  * The latest stable version of the official fpc compiler (2.6.4)
  * additional and isolated (from the official 2.6.4) trunk fpc compiler (2.7.1) installed from source downloaded with subversion and compiled on raspberry.



I chose the gentoo distro and, of course, the installation does not is the subject of this guide also because the official Gentoo documentation is very good. 

# Install the latest version of fpc from repository

Whatever linux distro, it rarely happens the packages fpc/lazarus are aligned with the latest stable release. In my case, the last official fpc release is 2.6.4 (the only officially needed to compile the trunk), while in gentoo repositories I found fpc 2.6.2. 

I strongly recommend the use of official packages from the distro. Always check the repository for the release that you need, in the case of gentoo: 

1) update the list of packages: 
    
    
       # emerge --sync 
    

2) search for fpc compiler: 
    
    
       # emerge --search fpc 
    

In case you find the correct version to compile the trunk (in this case 2.6.4), install it with no doubts: 
    
    
       # emerge dev-lang/fpc 
    

and then jump to the section "Installation of subversion." 

In case there is not ... proceed manually according to the following section. 

  


# Manually install the latest version of fpc

We must search for the arm fpc release. We have different places to search for, starting from following links: 

  * <ftp://ftp.freepascal.org/pub/fpc/dist/2.6.4/arm-linux/>
  * <http://sourceforge.net/projects/freepascal/files/Linux/2.6.4/>



Always verify both of them, they are not updated at the same time. 

Download the tarball and we can prepare to install (as root): 
    
    
       # su
       # mkdir /usr/local/fpc
       # cd /usr/local/fpc
       # wget -O fpc-2.6.4.arm-linux.tar <http://sourceforge.net/projects/freepascal/files/Linux/2.6.4/fpc-2.6.4.arm-linux.tar/download>
       # tar xvf fpc-2.6.4.arm-linux.tar
       # cd fpc-2.6.4.arm-linux
       # ./install.sh
    

You will be asked for some questions which usually we can answer pressing the ENTER key, howewer I have done the installation as follows: 

  * Installation folder : /usr/local
  * documentation not installed
  * examples not installed



If you have installed in folder /usr/local, you can verify the presence of fpc : 
    
    
       # ls /usr/local/lib/fpc
    

and you will see its subfolders : 

  * 2.6.4
  * lexyacc



Also try to run the compiler : 
    
    
       # fpc
       Free Pascal Compiler version 2.6.4 [2014/03/21] for arm
       Copyright (c) 1993-2014 by Florian Klaempfl and others
       /usr/local/lib/fpc/2.6.4/ppcarm...
    

# Installing subversion

Subversion is one of those programs that you will definitely find in the repository, whatever linux distro you have chosen. In the case of gentoo: 
    
    
       # emerge dev-vcs/subversion
    

On a raspberry with gentoo, emerge subversion is challenging process. It will take a while. 

  


# Downloading fpc trunk sources

Now we need to download the sources of fpc trunk. 
    
    
       # mkdir /usr/local/fpc/2.7.1
       # cd /usr/local/fpc/2.7.1
       # svn co <http://svn.freepascal.org/svn/fpc/trunk> trunk
       # svn export --force trunk trunktmp
       # cd trunktmp
    

  


# Before building

There are a few things to check: 

1) ld-linux.so from the description in the man pages (man ld-linux.so): "The programs ld.so and ld-linux.so * find and load the shared libraries needed by a program, prepare the program to run, and then run it." You have to find this .so, whatever it's called and wherever it is located 
    
    
       # find /lib/ld-*
       /lib/ld-linux-armhf.so.3
    

Remember this fullpath as we must pass it as a parameter. 

2) crtbegin.o and crtend.o These files are also needed to proceed with the construction of the compiler, so you have to find them; just look for one of the two: 
    
    
       # find /usr/lib -name crtbegin.o         
       /usr/lib/gcc/armv6j-hardfloat-linux-gnueabi/4.7.3/crtbegin.o
    

Another fullpath to remember 

  


# Build the compiler

Run the following commands in sequence: 
    
    
       # cd /usr/local/fpc/trunktmp
       # make all OPT='-gl -O3p3' -j 8 FPMAKEOPT="-o -Fo/usr/lib/gcc/armv6j-hardfloat-linux-gnueabi/4.7.3" FPC=/usr/local/lib/fpc/2.6.4/ppcarm OPT=-FL/lib/ld-2.17.so OPT=-Fo/usr/lib/gcc/armv6j-hardfloat-linux-gnueabi/4.7.3
    

here you have to wait a bit 
    
    
       # make install PP=compiler/ppcarm PREFIX=/usr/local -j 8 FPMAKEOPT="-o -Fo/usr/lib/gcc/armv6j-hardfloat-linux-gnueabi/4.7.3"
    

Now, instead of creating a symbolic link in / usr / bin to be able to use the new compiler, I want to keep in use two compilers (such as mentioned at the beginning) and I want them isolated. This is why I do not create the link, but later, I will create a script. 

We can instal sources 
    
    
       # make install sourceinstall PREFIX=/usr/local
    

Now we need to create a configuration file so that it does not conflict with the one already present in /etc (related to the compiler 2.6.4). I want to place the fpc.cfg file in the “etc” subfolder inside the compiler folder. First of all, we can backup the files in /etc. Now we can create the etc subfolder: 
    
    
       # mkdir /usr/local/lib/fpc/2.7.1/etc
       # /usr/local/lib/fpc/2.7.1/samplecfg /usr/local/lib/fpc/2.7.1 /usr/local/lib/fpc/2.7.1/etc
    

At this point I create a script to launch the new compiler passing the configuration file as parameter: 
    
    
       # nano /usr/local/bin/fpc271.sh 
    

and there I copy the following content: 
    
    
       #!/bin/sh
       # This script execute the fpc trunk compiler 
       
       if [ $# -eq 0 ]; then
          /usr/local/lib/fpc/2.7.1/ppcarm
       else
          /usr/local/lib/fpc/2.7.1/ppcarm -n @/usr/local/lib/fpc/2.7.1/etc/fpc.cfg ""$@""
       fi
    

At this point, to compile with the 2.6.4 release, simply invoke compiler with the classic name fpc: 
    
    
       # fpc [options]
    

To invoke the 2.7.1 compiler: 
    
    
       # fpc271.sh [options]

---

_Source: [https://wiki.freepascal.org/Install_fpc_on_Raspberry_with_Gentoo](https://web.archive.org/web/20250117071621/https://wiki.freepascal.org/Install_fpc_on_Raspberry_with_Gentoo)_
