# Compile and Develop on Maemo device

[FreePascal compiler](<http://freepascal.org>) is a free compiler for the various Pascal dialects. There is also a project [Lazarus](<http://lazarus.freepascal.org>) which provides a Delphi-like IDE for the FreePascal compiler. This article describes how to compile and install FreePascal compiler right on the device (currently [Nokia N900|N900](<http://wiki.maemo.org/Nokia_N900>) only, still have troubles to successfully compile it on my [N810](</index.php?title=Nokia_N810&action=edit&redlink=1> "Nokia N810 \(page does not exist\)")). You may need a lot of space, so it is very useful to [format an external SD card as ext2 or ext3](<http://wiki.maemo.org/Ext2_on_microSD_card>). 

## Contents

  * 1 Bootstrapping
  * 2 Creating config
  * 3 Compile Lazarus
  * 4 Compile on device
  * 5 See also



## Bootstrapping

First we need to get a working copy of fpc for ARM EABI. Currently, it is possible to get only ARB OABI version at the FreePascal web site. (version 2.2.2) 

That's why we need to compile an ARM version of the compiler on the GNU/Linux PC first, then transfer it to the Nokia device, and compile the full compiler distribution there. 

  1. First you need to have fpc installed on your GNU/Linux box. 
  2. Then, download fpc source with svn. 
         
         svn checkout http://svn.freepascal.org/svn/fpc/trunk fpc
         

  3. You also need cross binutils for ARM. Let's compile it like this: 
         
         mkdir binutils
         cd binutils
         BINUTILS=binutils-2.20.1
         wget http://ftp.gnu.org/gnu/binutils/$BINUTILS.tar.bz2
         tar jxvf $BINUTILS.tar.bz2
         cd $BINUTILS
         ./configure --prefix=/opt/binutils-arm-eabi --target=arm-none-eabi --with-gnu-as
          --with-gnu-ld
         make
         make install
         ln -s /opt/binutils-arm-eabi/bin/arm-none-eabi-as /opt/binutils-arm-eabi/bin/arm
         -linux-as
         ln -s /opt/binutils-arm-eabi/bin/arm-none-eabi-ld /opt/binutils-arm-eabi/bin/arm
         -linux-ld
         

  4. Then, export PATH so it will contain your binutils path first 
         
         export PATH=/opt/binutils-arm-eabi/bin:$PATH
         

  5. Now we can compile fpc for ARM EABI. Go to fpc svn directory and run: 
         
         make all PREFIX=/opt/fpc-arm OS_TARGET=linux CPU_TARGET=arm OPT="-dFPC_ARMEL"
         

  6. After successful compilation you will get "ppcarm" binary in the "compiler" folder inside fpc source tree. 
         
         noch@hactar:~/freepascal/svn/fpc/compiler$ file ppcarm
         ppcarm: ELF 32-bit LSB executable, ARM, version 1 (SYSV), statically linked, stripped
         

  7. Now it's time to scp ppcarm to the Nokia Maemo device, and compile with it sources there. From this point we work only on Maemo. I assume that you have `openssh-server` installed. You will also need the following packages: 
     * `subversion` \- to get fpc source tree
     * `binutils` \- to get GNU assembler and linker
     * `diffutils-gnu` \- fpc needs "diff" to compile itself
     * `coreutils-gnu` \- contains, "install" command, needed to run "make install"

For Diablo, I found "diffutils" package in the [Dayless](<http://www.gronmayer.com/it/dl.php?id=224>) repository at the [gronmayer](<http://www.gronmayer.com/it>) repository list. 
    
    # dpkg -L diffutils-gnu | grep bin
    /usr/bin
    /usr/bin/gnu
    /usr/bin/gnu/cmp
    /usr/bin/gnu/diff
    /usr/bin/gnu/diff3
    /usr/bin/gnu/sdiff
    /usr/bin/gcmp
    /usr/bin/gsdiff
    /usr/bin/gdiff3
    /usr/bin/gdiff
    

  8. Because fpc needs "diff" executable, then there's two options. Whether to add `/usr/bin/gnu` to the `PATH`: 
         
         export PATH=/usr/bin/gnu:$PATH

or create a symlink to `gdiff` in `/usr/bin`
         
         ln -s /usr/bin/gdiff /usr/bin/diff

  9. There is a linker problem like 
         
         ./link.res: file format not recognized; treating as linker script
         ./link.res:281: syntax error
         pp.pas(209,27) Error: Error while linking
         pp.pas(209,27) Fatal: There were 1 errors compiling module, stopping
         Fatal: Compilation aborted 
         

In order to avoid this, go to `compiler/systems` and change `t_linux.pas` so that it contain path to the `/lib/ld-linux.so.3` instead of `/lib/ld-linux.so.2`, because we do not have `/lib/ld-linux.so.2` on Maemo 4 and Maemo 5. This is a [[quote](<http://free-pascal-general.1045716.n5.nabble.com/FreePascal-on-ARM-Linux-SoftFloat-and-EABI-issues-td2819338.html>)] from the freepascal mailing list: 
         
         > also although it creates an EABI 4 executable correctly there is a
         > problem with fpc-svn/compiler/systems/t_linux.pas that sticks the wrong
         > dynamic linker name in the file. I fixed it with this...
         >
         >
         > {$ifdef arm}
         >     {$ifdef FPC_ARMEL}
         >          defdynlinker:='/lib/ld-linux.so.3';
         >     {$else} 
         

  10. So, change to the fpc source directory and run: 
         
         make clean all PP=/path/to/newlycopied/ppcarm PREFIX="/opt/fpc-svn" OPT="-dFPC_ARMEL -O- -dFPC_ABI_EABI"
         

  11. If you have compilation error like this 
         
         __missing_command_DATA2INC fpc
         

it means compiler cannot find "data2inc" file and we need to provide path for that, so the command is 
         
         make clean all PP=/path/to/newlycopied/ppcarm DATA2INC="/path/to/fpc_source/utils/data2inc" PREFIX="/opt/fpc-svn" OPT="-dFPC_ARMEL -O- -dFPC_ABI_EABI"
         

Then, make install with the same options 
         
         make install PP=/path/to/newlycopied/ppcarm PREFIX="/opt/fpc-svn" OPT="-dFPC_ARMEL -O- -dFPC_ABI_EABI"
         




## Creating config

Now, we have a working compiler in `/opt/fpc-svn/bin`. But there are no config file, so it won't work. 

  1. In order to create a config file, do: 
         
         /opt/fpc-svn/bin/fpcmkcfg -d basepath=/opt/fpc-svn/lib/fpc/2.5.1/ -o /etc/fpc.cfg
         

  2. It is also necessary to have correct path: 
         
         export PATH=/opt/fpc-svn/lib/fpc/2.5.1:/opt/fpc-svn/bin:$PATH
         

Second directory contains ppcarm binary, which is necessary for compilation. 




This is it, now you have a working FreePascal compiler on Maemo. Next step is to install Lazarus, so you will be able to compile Lazarus created sources right on the device. 

## Compile Lazarus

Get the source: 
    
    
    svn co http://svn.freepascal.org/svn/lazarus/trunk lazarus
    

Lazarus compilation will fail with the binutils of Maemo 5. Thus we need to compile newer binutils and we need gcc for it. 

  1. Add SDK repository to the apt config 
         
         vi /etc/apt/sources.list.d/dev.list
         
         deb http://repository.maemo.org fremantle/sdk free non-free

DO NOT UPGRADE YOUR SYSTEM WITH THIS CONFIG LINE ENABLED! 

  2. apt-get update
         apt-get install gcc autoconf automake1.9 libtool flex bison gdb automaken libltdl3-dev
         

  3. At this point I also have gettext installed. 
         
         apt-get install gettext
         

  4. Dependencies for lazarus: 
         
         apt-get install libgtk2.0-dev libgpmg1-dev libncurses5-dev

It will compile, but won't link without them. 

  5. Now install binutils. For that you also need `tar-gnu` and `bzip2` packages: 
         
         apt-get install tar-gnu bzip2
         

  6. I use the following script: 
         
         #!/bin/bash
         BINUTILS=binutils-2.20.1
         rm -rf $BINUTILS
         wget -c http://ftp.gnu.org/gnu/binutils/$BINUTILS.tar.bz2
         /usr/bin/gnu/tar -jxvf binutils-2.20.1.tar.bz2  
         cd $BINUTILS                                                                    
         ./configure --prefix=/opt/binutils-arm-eabi --target=arm-none-eabi --with-gnu-as
         make        
         make install                                                                    
         ln -s /opt/binutils-arm-eabi/bin/arm-none-eabi-as /opt/binutils-arm-eabi/bin/arm
         ln -s /opt/binutils-arm-eabi/bin/arm-none-eabi-ld /opt/binutils-arm-eabi/bin/arm
         

  7. Update the `PATH` environment variable so it contain newly installed binutils at first place: 
         
         export PATH=/opt/binutils-arm-eabi/bin:$PATH
         

or if fpc is not currently in the path in this terminal, then 
         
         export PATH=/opt/binutils-arm-eabi/bin:/opt/fpc-svn/lib/fpc/2.5.1:/opt/fpc-svn/bin:$PATH

  8. cd into lazarus directory 
         
         make clean all
         

if it fails with 
         
         Need "lazarus_0.9.31.orig.tar.gz" to build for DEBBUILD = ""
         

then you will be able to compile it with 
         
         make DEBUSESVN=1 clean all
         

if it still fails with 
         
         make -C ide ide
         make[1]: Entering directory `/media/mmc1/noch/src/freepascal/lazarus-svn-2011-02-10/ide'
         ../tools/svn2revisioninc .. revision.inc
         make[1]: *** [revisioninc] Segmentation fault
         make[1]: Leaving directory `/media/mmc1/noch/src/freepascal/lazarus-svn-2011-02-10/ide'
         make: *** [ide] Error 2
         

then according to [this link](<http://bugs.freepascal.org/bug_view_page.php?bug_id=18271>) it could be solved by USESVN2REVISIONINC=0 
         
         make LCL_PLATFORM=gtk USESVN2REVISIONINC=0 DEBUSESVN=1 clean all
         




## Compile on device

This script will help you to compile your application on device. 
    
    
    ARCH="arm-linux"
    LAZARUS="/media/mmc1/noch/lazarus"
    fpc project1.lpr -MObjFPC -Scgi -O1 -gl -WG -vewnhi -l -Fu$LAZARUS/lcl/units/$ARCH/ -Fu$LAZARUS/lcl/units/$ARCH/gtk2/ -Fu$LAZARUS/packager/units/$ARCH/ -Fu. -oproject1 -dLCL -dLCLgtk2
    

Now it is possible to run it :) 

Actually, you may need a longer compile line, to find out that, click on "Show options" button in Project -> Compiler Options, and change the line correspondingly, by replacing ARCH and LAZARUS paths. 

## See also

  * [Smartphone Development](<Smartphone_Development.md> "Smartphone Development")
  * [Setup Cross Compile For ARM](<Setup_Cross_Compile_For_ARM.md> "Setup Cross Compile For ARM")
  * [Qt4 Maemo Binding](<Qt4_Maemo_Binding.md> "Qt4 Maemo Binding")

---

_Source: [https://wiki.freepascal.org/Compile_and_Develop_on_Maemo_device](https://web.archive.org/web/20240311162847/https://wiki.freepascal.org/Compile_and_Develop_on_Maemo_device)_
