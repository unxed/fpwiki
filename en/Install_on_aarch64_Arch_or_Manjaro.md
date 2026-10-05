# Install on aarch64 Arch or Manjaro

│ **English (en)** │

[![Stock-dialog-warning.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b3/Stock-dialog-warning.svg/50px-Stock-dialog-warning.svg.png)](</File:Stock-dialog-warning.svg>)

This article applies to [ARM](</Category:ARM> "Category:ARM") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![Crystal 128 penguin.png](https://upload.wikimedia.org/wikipedia/commons/1/1c/Crystal_128_penguin.png)](</File:Crystal_128_penguin.png>)

This article applies to [Linux](</Category:Linux> "Category:Linux") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Introduction
  * 2 Install Free Pascal
  * 3 Install Lazarus
  * 4 Add Lazarus to your KDE start menu
  * 5 Acknowledgement



## Introduction

As of August 2020 binary packages for Lazarus are not available for Arch or Manjaro Linux on 64 bit ARM (aarch64). 

A list of ARM packages for Arch Linux is available [here](<https://archlinuxarm.org/packages>). 

Manjaro is the default Linux version pre-installed on the Pinebook Pro for example. 

It is therefore necessary to download the FPC tar installer, FPC source, Lazarus source and compile Lazarus yourself. 

## Install Free Pascal

1\. Install binutils 
    
    
    sudo pacman -Syu binutils
    

2\. Download the FPC binaries 

Download the FPC binaries from [here](<https://www.freepascal.org/down/aarch64/linux-hungary.html>). 

3\. Unpack and install the FPC binaries 
    
    
    tar -xf fpc-3.2.0.aarch64-linux.tar
    cd fpc-3.2.0.aarch64-linux
    sudo sh install.sh
    

4\. Download and unpack the FPC source 

Download fpc-3.2.0.source.tar.gz from [here](<https://www.freepascal.org/down/source/sources.html>), and unpack the it into some convenient directory (eg ~/fpc/fpc-3.2.0). 

5\. Test FPC 

Use an editor to create helloworld.pas: 
    
    
    Program helloworld;
    begin
      writeln ('Hello world');
    end.
    

Compile it: 
    
    
    fpc helloworld.pas 
    Free Pascal Compiler version 3.2.0 [2020/06/05] for aarch64
    Copyright (c) 1993-2020 by Florian Klaempfl and others
    Target OS: Linux for AArch64
    Compiling helloworld.pas
    Assembling helloworld
    Linking helloworld
    4 lines compiled, 0.5 sec
    

Run it: 
    
    
    ./helloworld 
    Hello world
    

## Install Lazarus

1\. Install the necessary Manjaro packages 
    
    
    sudo pacman -Syu make subversion qt5pas
    

2\. Download the Lazarus source 
    
    
    cd
    mkdir fpc
    cd fpc
    svn co https://svn.freepascal.org/svn/lazarus/trunk lazarus
    

3\. Compile Lazarus 
    
    
    cd lazarus
    make LCL_PLATFORM=qt5
    

and run it: 
    
    
    ./startlazarus
    

## Add Lazarus to your KDE start menu

1\. Create this file 
    
    
    touch ~/.local/share/applications/lazarus.desktop
    

2\. Open the file you just created in an editor 
    
    
    kate ~/.local/share/applications/lazarus.desktop
    

2\. Paste in this code 
    
    
    [Desktop Entry]
    Categories=Development;
    Comment=IDE
    Exec=~/fpc/lazarus/startlazarus %f
    GenericName=IDE
    Icon=lazarus-icon
    InitialPreference=9
    MimeType=text/plain;
    Name=Lazarus
    Path=
    StartupNotify=true
    Terminal=false
    TerminalOptions=
    Type=Application
    X-DBUS-ServiceName=
    X-DBUS-StartupType=
    X-KDE-HasTempFileOption=true
    X-KDE-SubstituteUID=false
    X-KDE-Username=
    

3\. Save the file and close the editor 

4\. Create a 48x48 xpm format icon and save it to /usr/share/pixmaps/lazarus-icon.xpm You can download mine from <http://pscan.uk/images/lazarus-icon.xpm>

A new menu entry called "Lazarus" should appear in KDE's menu. 

## Acknowledgement

[Original source of information](<https://forum.manjaro.org/t/how-to-install-fpc-and-lazarus-on-pinebook-pro-arm-64-bit/152872>). 

[Mirror of this article](<https://wiki.pine64.org/wiki/Pinebook_Pro/Freepascal_and_Lazarus_IDE_on_Manjaro>).

---

_Source: [https://wiki.freepascal.org/Install_on_aarch64_Arch_or_Manjaro](https://web.archive.org/web/20250227075931/https://wiki.freepascal.org/Install_on_aarch64_Arch_or_Manjaro)_
