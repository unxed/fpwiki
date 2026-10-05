# Cross Compile to RasPi from Linux

## Contents

  * 1 Introduction
  * 2 Getting the necessary libraries
  * 3 Edit fpc.cfg
  * 4 sudo
  * 5 Install the Pi Libraries
  * 6 Build the Cross Compiler
  * 7 Test
  * 8 Further Information
  * 9 See Also



### Introduction

This page is about setting up a Cross Compiler from a Linux Box to a [ Raspberry Pi](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi"). You should be able to select arm as the CPU in Lazarus or if just using FPC, choose to use the ppcrossarm compiler and produce v6 arm binaries. Raspberry Pi is just a specific example of arm, only small changes would seem necessary on other similar platforms. 

  * Here we talk about making armhf software, not armel, there is a very brief discussion about that lower down.
  * Tested on an Ubuntu 21.04 and Debian Bullseye System. Its should be similar on most mainstream Linux platforms.
  * This recipe assumes **you already have a current FPC** and, possibly Lazarus installed. You MUST use packages downloaded from the FPC/Lazarus SourceForge site, its most likely that FPC and Lazarus packages from your Distribution will not work as expected.
  * A large part of this install is done as root. Working like that gives you the potential to make a real mess of your system, please be very careful.



**This page is under development and the recipe presented is still being tested.**

### Getting the necessary libraries

Firstly, we are going to need some reasonably current libraries from your Raspberry Pi, as near as I can determine this is the best way to get the necessary libraries. Make sure the libraries you will need (to cross compile) are already installed on your pi. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** It possibly makes good sense to choose to make Qt5 apps for the Raspberry Pi now that the basic install of the Raspberry Pi Operating System does not include GTK2 libraries anymore. Qt5 as a dependency is a lot smaller than GTK2. So, install the necessary Qt5 libraries on the Pi before the next step, libqt5pas1 and libqt5pas-dev, they will bring along the necessary dependencies hopefully. If you are using Lazarus Main (aka trunk) then look at 

<https://github.com/davidbannon/libqt5pas>

On the pi, we will tar up what we need, this does not need to be done as root. 
    
    
    tar czf usr_lib_arm-linux-gnueabihf.tgz /usr/lib/arm-linux-gnueabihf
    tar czf lib_arm-linux-gnueabihf.tgz /lib/arm-linux-gnueabihf
    

Next, we need the crt files, first, find where they are, yours may be in a directory other than the '10' shown here (thats bullseye). 
    
    
    raspberrypi:~ $ find / -name "crtbegin.o" 2>null
    /usr/lib/gcc/arm-linux-gnueabihf/10/crtbegin.o
    raspberrypi:~ $ (cd /usr/lib/gcc/arm-linux-gnueabihf/10 && tar czf $HOME/crt.tgz crt*.o)
    

Note: is probably a good idea to take these libraries from a "minimal" install rather than one with all the bells and whistles. But do make sure the libraries (and the dev version) that you will need are installed. Messy otherwise. 

Take these three files, 248M (or 330M with Qt5), 7M and 3K respectively to the Intel box and put them in your home directory, then, we will deal with them as root further down. They will use up some 640M of disk and probably includes libraries you will never use but its easier than trying to work out exactly what you do need. 

### Edit fpc.cfg

**Evidence is this step is unnecessary, just ignore it for now, further research is indicated** Only one change is necessary, must tell the compiler which particular arm cpu we are targeting. 
    
    
    #ifdef cpuarm
    -CpARMV6
    -CfVFPV2
    -OoFASTMATH
    #endif
    

If your fpc.cfg is in /etc and it probably is, you will need to do that edit as root, thats OK, we are about to do a whole load of stuff as root. So, from now on, you will be running as root, that means great care must be used. Note these instructions are meant to be copied and pasted, one by one. They are not a script ! 

### sudo

OK, here we must become root, that implies very careful ! Most linux systems allow sudo, if not, try su - 

If you do not have the root password (or don't want to use it), then reconside this whole operation, you can install the base compiler into user space from a tarball and build Lazarus from source, again in user space. Pretty good way to work, you will need to adjust the various paths accordingly. 
    
    
    sudo -i
    

### Install the Pi Libraries

Next, we need to install the needed libraries from a Pi from those tar balls we moved over before. The new libraries all end up in /usr/lib/arm-linux-gnueabihf - so easy clean up if necessary. 
    
    
    cd / 
    tar  xzf  /home/"$SUDO_USER"/usr_lib_arm-linux-gnueabihf.tgz 
    tar  xzf  /home/"$SUDO_USER"/lib_arm-linux-gnueabihf.tgz
    tar  xzf  /home/"$SUDO_USER"/crt.tgz  -C  /usr/lib/arm-linux-gnueabihf 
    

Next, install some needed things and symlink them up to what Lazarus expects to find. 
    
    
    apt install binutils-arm-linux-gnueabihf
    ln -s /usr/bin/arm-linux-gnueabihf-ld /usr/bin/arm-linux-ld
    ln -s /usr/bin/arm-linux-gnueabihf-as /usr/bin/arm-linux-as
    ln -s /usr/bin/arm-linux-gnueabihf-objcopy /usr/bin/arm-linux-objcopy
    ln -s /usr/bin/arm-linux-gnueabihf-strip /usr/bin/arm-linux-strip
    

### Build the Cross Compiler

OK, now we build the cross compiler and a set of pre compiled arm units. 
    
    
     export FPCVER="3.2.2"
     cd /usr/share/fpcsrc/"$FPCVER"/
    
     make clean all FPMAKEOPT="-T 4" CPU_TARGET=arm OPT="-dFPC_ARMHF" CROSSOPT="-CpARMV6 -CaEABIHF" CROSSBINDIR=/usr/arm-linux-gnueabihf/bin
    
     make crossinstall OS_TARGET=linux CPU_TARGET=arm OPT="-dFPC_ARMHF" CROSSBINDIR=/usr/arm-linux-gnueabihf/bin   CROSSOPT="-CpARMV6 -CaEABIHF" INSTALL_PREFIX=/usr
    
     ln -sf /usr/lib/fpc/"$FPCVER"/ppcrossarm /usr/bin/ppcrossarm
    

OK, done, Control-D to get out of that somewhat dangerous root access and test ! 

### Test

Testing fpc functionality is easy, put the hello world source in a file, hello.pas and try this - 
    
    
    db@U2104G-VirtualBox:~/Pascal/console$ ppcrossarm hello.pas
    Free Pascal Compiler version 3.2.2 [2021/07/19] for arm
    Copyright (c) 1993-2021 by Florian Klaempfl and others
    Target OS: Linux for ARMEL
    Compiling hello.pas
    Linking hello
    4 lines compiled, 0.1 sec
    
    
    
    db@U2104G-VirtualBox:~/Pascal/console$ file hello
    hello: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), statically linked, for GNU/Linux 2.0.0, stripped
    

You can move that binary over to a Pi and test it really works. Note this test uses no additional libraries so is not a complete test. 

Testing the Lazarus functionality is more involved, fire up Lazarus, open a new project, maybe set a control or two on it, open Project->ProjectOptions->ConfigAndTarget and set ProjectCPUFamily to 'arm'. You probably will find a few missing dev libraries, remember that are usually just symlinks from the actual library to the base name, easier to make that link than go back to your Pi. 

Compile and see if the binary looks like this - 
    
    
    db@U2104G-VirtualBox:~/Pascal/ArmTest$ file project1
    project1: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.3, for GNU/Linux 2.0.0, not stripped
    

### Further Information

This page is about making **armhf** software, that suits arm chips using hardware floating point capabilities. The current Raspberry Pi is an example. The alternative is the **armel** or soft float arm chip, it too appears in many systems. It is possible that omitting the OPT="-dFPC_ARMHF" bit from the above command lines may make it work for soft float. That has not been tested ! 

Binaries can be ArmV6 or ArmV7 (or even ArmV8 but different rules apply) in either armhf or armel instruction set. Generally soft float arm software may run on (eg) a Raspberry Pi but it may also confuse your package manager. Again, theoretical advice. 

I found the **readelf** command useful for looking at a resulting binary after compilation, try the following - 
    
    
    readelf -a mybinary | grep Tag [enter]
    ...
    readelf -h mybinary [enter]
    

Another useful tip when playing in this space is to use Lazarus's Tools->RefreshFPCSource menu entry after making changes to the FPC config. 

## See Also

  * [Cross Compiling](<Cross_compiling.md> "Cross compiling")
  * [Installing Lazarus](<Installing_Lazarus.md> "Installing Lazarus")
  * [Installing the Free Pascal Compiler](<Installing_the_Free_Pascal_Compiler.md> "Installing the Free Pascal Compiler")

---

_Source: [https://wiki.freepascal.org/Cross_Compile_to_RasPi_from_Linux](https://web.archive.org/web/20250616134233/https://wiki.freepascal.org/Cross_Compile_to_RasPi_from_Linux)_
