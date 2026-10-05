# Setup Cross Compile For ARM

│ **English (en)** │  **[русский (ru)](<../ru/Setup_Cross_Compile_For_ARM.md>)** │

## Contents

  * 1 How To setup Lazarus/FPC for arm-linux cross compiling.
  * 2 Automated Script
  * 3 Generic things to do
    * 3.1 Downloads
    * 3.2 Unpack/copy all files in the respective directories
    * 3.3 Prepare Target Libs
    * 3.4 Install dependencies
  * 4 Install & Configure Binutils
    * 4.1 Compile binutils for arm-linux
  * 5 FPC
    * 5.1 Install precompiled fpc 2.2.4 branch
    * 5.2 Compile FPC from sources
  * 6 FPC for ARM
    * 6.1 Make FPC able to cross compile for arm-linux
    * 6.2 Create custom fpc.cfg
  * 7 Lazarus
    * 7.1 Compile Lazarus
    * 7.2 Start Lazarus
    * 7.3 Configure Lazarus for cross Compile
  * 8 Make your first arm-linux project in Lazarus
    * 8.1 New Project
    * 8.2 Set Targets
    * 8.3 Add target libs to your project
    * 8.4 Build Project
    * 8.5 Time to run
  * 9 APPENDIX A: Testing console programs
    * 9.1 Create a TestARM.pas
    * 9.2 Time to run
  * 10 Appendix B: Target libs
  * 11 See also



## How To setup Lazarus/FPC for arm-linux cross compiling.

With this tutorial you can make a binary file that runs on arm-linux devices. This is tested with a Nokia N900 with "[Maemo 5 OS](<http://maemo.org/>)" (the gnu/linux Debian based OS, developed by [Nokia](<http://maemo.nokia.com/videos/introducing-maemo-5/>). It is an arm-linux device that runs GTK2 and QT4.6). 

If you need a simple "command prompt"/"terminal" program, after #FPC for ARM section skip to #APPENDIX A: Testing console programs. 

**NOTE 1** : All procedures are tested on a Virtual Machine with **Ubuntu 9.10 - x86** with Windows 7 as host and VirtualBox 3.0.12 as virtual machine. It is a fresh install, with all updates up to date. 

**NOTE 2** : Replace every _/home/user/_ with your real user path. All other paths are simply for explanation. If you use "[Maemo SDK Virtual Image](<http://maemovmware.garage.maemo.org/2nd_edition/>)", replace every _/home/user/_ with _/home/maemo/_

## Automated Script

The previously provided scripts are obsolete. Please see [HERE](<http://wiki.lazarus.freepascal.org/Lazarus_Manager>) (see discussion page for alternative. 

## Generic things to do

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** These instructions are quite old - they refer to 2.2.4 while the current (December 2013) stable FPC is 2.6.2. Please adjust as you follow these and update the page. Thanks.

### Downloads

  * any starting compiler from the 2.2.4 branch (e.g. fpc-2.2.4-20091214.i386.rpm) from [HERE](<http://www.hu.freepascal.org/lazarus>) or [HERE](<http://snapshots.lazarus.shikami.org/>)). It is needed to compile >= 2.2.4 fpc sources
  * latest binutils.tar.gz (e.g. binutils-2.20.tar.gz) from [HERE](<http://ftp.gnu.org/gnu/binutils/>)
  * latest [fpc.zip](<http://ftp//ftp.freepascal.org/pub/fpc/snapshot/trunk/source/fpc.zip>) from [HERE](<http://www.freepascal.org/develop.var>)
  * latest Lazarus source zip (e.g. lazarus-0.9.29-23129-20091214-src.zip) from [HERE](<http://www.hu.freepascal.org/lazarus>) or [HERE](<http://snapshots.lazarus.shikami.org/>) (faster for me)
  * target libs (You can grab from SDK or from arm-device. _See "#APPENDIX A: Testing console programs"_)



### Unpack/copy all files in the respective directories

FPC 2.2.4 installer: 
    
    
    ~/fpc_tools/fpc_2.2.4/fpc-2.2.4-20091214.i386.rpm
    

Binutils sources: 
    
    
    ~/fpc_tools/binutils-2.20/
    

Lazarus sources: 
    
    
    ~/lazarus/
    

FPC 2.5.1 sources: 
    
    
    ~/lazarus/fpc/2.5.1/
    

### Prepare Target Libs

  * See "#APPENDIX A: Testing console programs" to know how/where to find target libs
  * Copy all target libs to ~/lazarus/fpc/libcross



### Install dependencies
    
    
    sudo apt-get install -y alien libncurses5-dev libXp-dev libgtk2.0-dev
    

## Install & Configure Binutils

### Compile binutils for arm-linux

  * If not already done, extract binutils-2.20.tar.gz in ~/fpc_tools/binutils-2.20
  * Then type:


    
    
    cd ~/fpc_tools/binutils-2.20
    ./configure --target=arm-linux --disable-werror
    make
    sudo make install
    mkdir ~/lazarus/fpc/binutils
    ln -s /usr/local/bin/arm-linux-ar ~/lazarus/fpc/binutils/ar
    ln -s /usr/local/bin/arm-linux-ld ~/lazarus/fpc/binutils/ld
    sudo mv /usr/local/bin/arm-linux-as /usr/local/bin/arm-linux-as_org
    
    sudo gedit /usr/local/bin/arm-linux-as
    

  * Paste:


    
    
    #!/bin/sh
    /usr/local/bin/arm-linux-as_org -meabi=5 $@
    

**NOTE** : option _-meabi=5_ force assembler to make binary _eabi v5_ compatible. E.g. if you need eabi v4 you will change it in _-meabi=4_. 

  * Save and close gedit
  * Type:


    
    
    sudo chmod +x /usr/local/bin/arm-linux-as
    ln -s /usr/local/bin/arm-linux-as ~/lazarus/fpc/binutils/as
    

## FPC

### Install precompiled fpc 2.2.4 branch

**REMEMBER TO CHANGE EVERY** /home/user/**WITH YOUR OWN USER PATH!**

  * Type:


    
    
    sudo alien -i -c /home/user/fpc_tools/fpc_2.2.4/fpc-2.2.4-20091214.i386.rpm
    

  * Be sure that ppc386 is right installed, type:


    
    
    which ppc386
    

if ppc386 is in the path it returns the path (e.g. /usr/bin/ppc386) 

### Compile FPC from sources

  * Type:


    
    
    cd ~/lazarus/fpc/2.5.1
    make all OPT='-gl -O3p3' PP=/usr/bin/ppc386 (add NOGDB=1 at the end if asked)
    sudo make install PP=/usr/bin/ppc386 PREFIX=/usr/ (add NOGDB=1 at the end if asked)
    sudo rm /usr/bin/ppc386
    sudo ln -s /usr/lib/fpc/2.5.1/ppc386 /usr/bin/ppc386
    sudo mkdir /usr/share/fpcsrc/
    sudo ln -sf ~/lazarus/fpc/2.5.1/ /usr/share/fpcsrc/
    sudo /usr/lib/fpc/2.5.1/samplecfg /usr/lib/fpc/2.5.1/ /etc
    

## FPC for ARM

### Make FPC able to cross compile for arm-linux

  * Type:


    
    
    cd ~/lazarus/fpc/2.5.1/
    sudo make crossinstall CPU_TARGET=arm OS_TARGET=linux CROSSBINDIR=/home/user/lazarus/fpc/binutils/ OPT=-dFPC_ARMEL INSTALL_PREFIX=/usr
    

### Create custom fpc.cfg

**REMEMBER TO CHANGE EVERY** /home/user/**WITH YOUR OWN USER PATH!**

  * Type:


    
    
    gedit ~/.fpc.cfg
    

  * Paste in ~/.fpc.cfg:


    
    
    #INCLUDE /etc/fpc.cfg
    #IFDEF CPUARM
    -Fu/usr/lib/fpc/2.5.1/units/$fpctarget/*
    -Fl/usr/lib/fpc/2.5.1/units/$fpctarget/rtl/
    -XP/home/user/lazarus/fpc/binutils/
    -Xr/usr/lib/fpc/2.5.1/units/arm-linux/rtl/
    -Xr/home/user/lazarus/fpc/libcross
    -XR/home/user/lazarus/fpc/
    -darm
    -Tlinux
    #ENDIF
    

  * save and close gedit
  * Type:


    
    
    sudo ln -sf /usr/lib/fpc/2.5.1/ppcrossarm /usr/local/bin/ppcarm
    

## Lazarus

If you don't need graphical programs, you can skip this section and go directly to #Appendix B: Target libs

### Compile Lazarus

  * Type:


    
    
    cd ~/lazarus
    make clean all
    

### Start Lazarus

  * Start Lazarus to see that everything is ok and then to compile LCL and Package Registration
  * Type:


    
    
    ./lazarus
    

### Configure Lazarus for cross Compile

  * Now you must cross compile the LCL and Package Registration, this part comes from [HERE](<http://wiki.lazarus.freepascal.org/Cross_compiling#Cross_compiling_the_LCL>).
  * From the IDE: 
    1. In Tools -> Options -> Environment -> Files, set the Compiler path to the path to fpc. Normally this is already done.
    2. Then open Tools / Configure Build Lazarus / Advanced Build Options.
    3. Set Target OS as _linux_ and Target CPU as _arm_.
    4. Set LCL and Package registration to Build (the middle radio button) and all other to None (left radio buttons).
    5. Click the Build button.



## Make your first arm-linux project in Lazarus

### New Project

  * Start new project as application
  * Put some component in main form to test (like button, memo and few other things)



### Set Targets

  * From the IDE: 
    1. Set in Project / Compiler Options / Code generation / Target OS (-T): _Linux_
    2. Set in Project / Compiler Options / Code generation / Target CPU family (-P): _arm_
    3. Set in Project / Compiler Options / Other / Custom Option: enter -XParm-linux- into textbox



### Add target libs to your project

**REMEMBER TO CHANGE EVERY** /home/user/**WITH YOUR OWN USER PATH!**

  * From the IDE: 
    1. Set in Project / Compiler Options / Linking / Options (-k): _-L/home/user/lazarus/fpc/libcross_
    2. Set in Project / Compiler Options / Compilation / Execute after / Command: _arm-linux-strip -s /path/to/your/project/binary_ (where _"binary"_ is the executable builded with Lazarus)



### Build Project

  * Build Project as normal: 
    1. Hit keys Ctrl+F9
    2. Run / Build



### Time to run

  * Transfer the binary of your project on device
  * Generally you must give executable permission to the binary directly on the device. If you work in terminal type:


    
    
    chmod +x /path/where/is/YourProject
    ./path/where/is/YourProject
    

[![Nokia N900](https://wiki.freepascal.org/images/0/08/Kjow_hello-world.png)](</File:Kjow_hello-world.png>)

[](</File:Kjow_hello-world.png> "Enlarge")

e.g. Maemo 5 running Lazarus graphical demo.

## APPENDIX A: Testing console programs

### Create a TestARM.pas

  * Type:


    
    
    mkdir ~/dev/TestARM/
    gedit ~/dev/TestARM/TestARM.pas
    

  * Paste this code:


    
    
    program test;
    begin
      writeln('DATE ',{$i %DATE%});
      writeln('FPCTARGET ',{$i %FPCTARGET%});
      writeln('FPCTARGETCPU ',{$i %FPCTARGETCPU%});
      writeln('FPCTARGETOS ',{$i %FPCTARGETOS%});
      writeln('FPCVERSION ',{$i %FPCVERSION%});
    end.
    

  * Type:


    
    
    cd ~/dev/TestARM/
    fpc -Tlinux -Parm -XParm-linux- TestARM.pas
    

### Time to run

  * Transfer TestARM on device
  * Generally you must give executable permission to _TestARM_ directly on the device:


    
    
    chmod +x /path/where/is/TestAMR
    ./path/where/is/TestAMR
    

## Appendix B: Target libs

You need target libs to assemble and link binary file. You can find them into the SDK of your device or into the device itself. 

E.g. for Nokia N900 you need to copy /lib/* and /usr/lib/* (no subdirs needed) that are present in the Nokia N900 root and you must merge all files to ~/lazarus/fpc/libcross/  
Finally, you need to rename each library _not found_ by linker (ld) to the file name searched by linker itself.  


E.g. if you got this error 
    
    
    libX11.so not found (or incompatible)
    

and you have a file called _libX11.so.6.2.0_ , then you need to rename it into _libX11.so_

## See also

  * [Android](<Android.md> "Android") Article on setting up cross-compiling on Windows for Android on ARM
  * [Cross compiling](<Cross_compiling.md> "Cross compiling") for cross compiling from Linux, macOS and Windows along with other useful cross-compilation information.

---

_Source: [https://wiki.freepascal.org/Setup_Cross_Compile_For_ARM](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/Setup_Cross_Compile_For_ARM)_
