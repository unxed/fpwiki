# arm-wince

[![WinCE Logo.png](https://wiki.freepascal.org/images/8/86/WinCE_Logo.png)](</File:WinCE_Logo.png>)

This article applies to [Windows CE](</Category:WinCE> "Category:WinCE") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

This page describes setup of a crosscompiling environment (host is Win32),. 

## Contents

  * 1 The easy way: download the cross compiler
  * 2 The hard way: building WinCE cross compiler from sources
    * 2.1 Step 1: Cross binutils
    * 2.2 Step 2: Building the cross compiler
    * 2.3 Step 3: Configuration file
  * 3 Building FPC libraries
  * 4 Compiling a Test Project
    * 4.1 Step 1: Installing and Configuring the Emulator
    * 4.2 Step 2: Installing the command line
    * 4.3 Step 3: Compiling



## The easy way: download the cross compiler

The easiest way to start development for WinCE is to download the latest release version of FPC for Win32 and arm-wince cross compiler for Win32. 

Download them here: <http://www.freepascal.org/download.var>

First install FPC for Win32, then install arm-wince cross compiler as add-on package. 

## The hard way: building WinCE cross compiler from sources

This tutorial demonstrates how to build the **arm-wince** crosscompiler on Windows which allows you to compile Windows CE programs from your Windows desktop. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** During the tutorial some paths will be used to demonstrate the build process. Just substitute those paths with the ones on your system.

We will suppose that: 

  * your FPC compiler is located here: `c:\fpc\bin\i386-win32\fpc.exe`
  * your Free Pascal source code is located here: `c:\fpc`



### Step 1: Cross binutils

Binutils are the basic tools necessary to create executables, such as: linker (ld), assembler (as), archiver (ar), strip and some others. 

You need cross binutils for arm-wince (i.e. they run on your desktop but produce output suitable for arm-wince). Get them from <ftp://ftp.freepascal.org/pub/fpc/contrib/cross/arm-wince-binutils.zip>. 

Extract them to some dir in the path on your machine. We will suppose that you extracted the cross binutils to: `C:\cross\arm`

  


### Step 2: Building the cross compiler

Now you need to build the FPC cross compiler for the ARM processor. To do that, you need: 

  * FPC sources
  * a working installation of FPC
  * the cross binutils mentioned above



You can get the latest FPC source repository from Subversion: <http://www.freepascal.org/develop.html#svn>

In order to build the cross compiler it is necessary to have a correct `PATH` environment variable. On Windows it is very easy to get a `PATH` crowded with information put by installers. To ensure that your path is correct, create a batch file called `c:\fpc\compiler\create_cross_arm_wince.cmd` with the following code: 
    
    
    PATH=c:\fpc\bin\i386-win32;C:\cross\arm
    make cycle CPU_TARGET=arm OS_TARGET=wince
    

Now open the Windows command line, and run the batch file: 
    
    
    cd /d c:\fpc\compiler
    create_cross_arm_wince.cmd
    

The compile should run without any errors. 

You should now have a `ppccrossarm.exe` in `c:\fpc\compiler` and some `.o` and `.ppu` files in `c:\fpc\rtl\units\arm-wince`

Now copy those files to your Free Pascal installation. 

  * Copy the cross compiler `ppccrossarm.exe` to `c:\fpc\bin\i386-win32\`
  * Copy the units in `c:\fpc\rtl\units\arm-wince` to `c:\fpc\units\arm-wince`



  


### Step 3: Configuration file

Now you need to edit your FPC configuration file in `c:\fpc\bin\i386-win32\fpc.cfg` in order to use `ppccrossarm.exe` easily. 
    
    
    #IFDEF CPUARM
    #IFDEF WINCE
    # Settings specific to cross compilation to wince on ARM.
    -FuC:\fpc\units\arm-wince
    -XParm-wince-
    -FDC:\cross\arm
    #ENDIF
    #ENDIF
    

Now when you want to compile for WinCE you just have to specify the OS and Arch targets to FPC or Lazarus: 
    
    
    fpc -Twince -Parm yourfile.pas
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** When using Lazarus, make sure the compiler path/file in Lazarus options is set to "fpc.exe", not "ppc386.exe"

## Building FPC libraries

If you want to build libraries available with FPC just go to the library folder and execute: 
    
    
    PATH=c:\fpc\bin\i386-win32;c:\fpc\compiler;C:\cross\arm
    make OS_TARGET=wince CPU_TARGET=arm PP=ppcrossarm.exe
    

  


## Compiling a Test Project

You compiled the compiler! Now, what can I do with it? This is a tutorial to create a hello world like software with your new compiler. 

### Step 1: Installing and Configuring the Emulator

You will need a Windows CE Emulator configured for ActiveSync. The ActiveSync is necessary to install PocketCMD, a command line tool to run our hello world software. 

Download the free Pocket PC device emulator from Microsoft. It emulates ARM CPU. <http://msdn.microsoft.com/mobility/downloads/Emulator/default.aspx>

There is a tutorial on how to set up the Emulator for ActiveSync [here](<http://www.pocketpcfaq.com/faqs/5.0/emulator.htm>). 

### Step 2: Installing the command line

Download and install **PocketCMD** by SymbolicTools. Get it [here](<http://www.symbolictools.de/public/pocketconsole/applications/PocketCMD>) (this site seems to be gone) but try here [[1]](<http://www.mobile2day.de/platform_wince/device/CASIO/Cassiopeia+E-115/download.html?file=c112516.exe&prod=14571>)

To enable the console on newer devices (I needed it in my 6.1CE pro), you may to set the following registry value [link](<http://forum.xda-developers.com/showthread.php?t=275019&highlight=console>)
    
    
    HKEY_LOCAL_MACHINE\Drivers\Console\OutputTo -> 0
    

Check [WinCE_port_Hints](<WinCE_port_Hints.md> "WinCE port Hints") for more details on programming under Windows CE platform. 

### Step 3: Compiling

An example file to compile: 
    
    
    program test;
    
    {$apptype console}
    
    var
      Str: string;
    begin
      WriteLn('Software Developed with:');
      WriteLn('The Free Pascal Compiler');
      WriteLn('');
      WriteLn('Please, enter your name:');
      ReadLn(Str);
      WriteLn('Your name is: ' + Str);
    end.
    

Command line to compile `test.pas`: 
    
    
    fpc -Twince -Parm test.pas
    

You will get a `test.exe` executable file. Copy it to your Windows CE device and run. 

[Here](<http://wiki.lazarus.freepascal.org/index.php/Windows_CE_Interface#Screenshots>) is a screenshot of a software created this way.

---

_Source: [https://wiki.freepascal.org/arm-wince](https://web.archive.org/web/20240920213339/https://wiki.freepascal.org/arm-wince)_
