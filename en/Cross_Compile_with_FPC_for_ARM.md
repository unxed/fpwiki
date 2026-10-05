# Cross Compile with FPC for ARM

│ **English (en)** │

## Overview

These instructions cover cross compiling to Linux on ARM. See [arm-wince](<arm-wince.md> "arm-wince") for instructions on cross-compiling to Windows CE on ARM. 

## Cross compile with FPC for ARM (e.g. Zaurus)

  * First, setup a cross compilation environment; you can get more information about this here [[1]](<http://www.cartel-securite.fr/pbiondi/zaurus/crosscompile.html>), here [[2]](<http://www.nautilus6.org/operation/doc/wide-memo-nautilus6-zaurus-ccenv-setup-00.txt>) and here [[3]](<http://projects.buici.com/arm/cross/>).
  * Install the lastest native FPC [from sourceforge](<http://sourceforge.net/project/showfiles.php?group_id=89339>)
  * get an ARM FPC snapshot from the downloads page of [FPC Qt Binding for Zaurus](<http://users.pandora.be/Jan.Van.hijfte/qtforfpc/qtedemo.html>)
  * Download it to somedir. The name of the tar file may be different of course:


    
    
    cd somedir
    tar zxvf arm-linux-fpc.i386-linux-20050217.tar.gz
    

  * you will find a 'lib/fpc/1.9.7/units/arm-linux/...' dir. The version may be '1.9.8' or higher.
  * create FPC library directory structure for FPC in a lib directory on your system
  * most systems use '/usr/local/lib', some use '/usr'


    
    
    mkdir -p /usr/local/lib/fpc/1.9.7/units
    cp -fr lib/fpc/1.9.7/units/arm-linux /usr/local/lib/fpc/1.9.7/units/
    

  * copy the FPC cross compiler to its default place


    
    
    cp lib/fpc/1.9.7/ppcarm /usr/local/lib/fpc/1.9.7
    

  * set a soft link to it in a bin dir in your path
  * most distributions use '/usr/local/bin', some '/usr/bin'


    
    
    ln -sf /usr/local/lib/fpc/1.9.7/ppcarm /usr/local/bin/ppcarm
    

  * verify you can call ppcarm; it should return something like '1.9.7' and not 'command not found'


    
    
    ppcarm -iV
    

  * download [FPC 1.9.x ARM binutils](<ftp://ftp.freepascal.org/pub/fpc/snapshot/v19/arm-linux/binutils.tar.gz>) to somedir


    
    
    cd somedir
    tar zxvf binutils.tar.gz
    

  * copy the 2 programs in the tar to a bin dir in your path


    
    
    cp arm-linux-* /usr/local/bin
    

  * create a 'Hello World' program and compile it


    
    
    echo "program hello; begin writeln('Hello World'); end." > hello.pas
    ppcarm -XParm-linux- hello.pas
    

  * output should be something like this:


    
    
    # ppcarm -XParm-linux- hello.pas
    Free Pascal Compiler version 1.9.7 [2005/02/17] for arm
    Copyright (c) 1993-2005 by Florian Klaempfl
    Target OS: Linux for ARM
    Compiling hello.pas
    Assembling hello
    Linking hello
    1 Lines compiled, 0.2 sec
    

  * Copy it to the home directory of the user 'zaurus' on your zaurus device.
  * With an SSH setup this could be as simple as:


    
    
    scp hello zaurus:/home/zaurus
    

  * Install terminal program on your Zaurus. For a Zaurus SL6000: see manual on CD.
  * Run the terminal program. On a SL6000 the console will default to user 'zaurus' in its home dir /home/zaurus
  * Run the program


    
    
    ./hello
    

  * Output should be something like:


    
    
    Hello World

---

_Source: [https://wiki.freepascal.org/Cross_Compile_with_FPC_for_ARM](https://web.archive.org/web/20230205113656/https://wiki.freepascal.org/Cross_Compile_with_FPC_for_ARM)_
