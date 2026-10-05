# Lazarus Manager

## Contents

  * 1 Introduction
  * 2 Windows
    * 2.1 First Installation
    * 2.2 Update
    * 2.3 Directory structure
    * 2.4 How to start Lazarus
    * 2.5 Configure first Lazarus launch
  * 3 Download
  * 4 Extra Info
  * 5 See also
  * 6 Credit



## Introduction

This program is made to have latest news of lazarus/fpc development, because install everyday the daily snapshot is a little bit laborious. 

You can edit and improve Lazarus Manager sending me your feedback, maybe I will release the source code in future, but now I prefer to develop it myself. 

Finally, these scripts are for who doesn't want waste time on installations/configurations, but wants to develop on quick time ("end-users", not "developers of the development system"). 

**NOTE** : Lazarus Manager is made by [Kjow](</index.php?title=User:Kjow&action=edit&redlink=1> "User:Kjow \(page does not exist\)") and are **not supported by the FPC/Lazarus team**. This is occasionally updated to _latest revision_ , but there may be problems with _last minute_ SVN changes (so, it might not work with the _last minute_ version). 

**NOTE 2** : Before Lazarus Manager, there were four scripts (2 for windows and 2 for Linux). These are obsolete. They don't work anymore due changes in Lazarus/FPC source and Windows/Linux structure. **Don't ask for these, they don't exist anymore.**

  


## Windows

Lazarus Manager is a sort of "one-click" installation that permit to install Lazarus/FPC svn without anything else. It downloads all the necessary (svn client, utils, binutils, Lazarus branches/trunk svn, FPC branches/trunk svn), makes the directories structure and builds everything for also cross-compile for arm android/linux. 

  
From Windows (with android ndk, if required) it build:   
  


  * Lazarus branch with targets: 
    * i386-win32
    * X86_64-win64



  


  * Lazarus trunk with targets: 
    * i386-win32
    * X86_64-win64
    * arm-android
    * arm-linux



  


### First Installation

Lazarus Manager has some controls to check if something fails (e.g. android ndk path not found), anyway before start you should know something and set some little things. 

Lazarus Manager saves latest settings used in **./config.ini** : 

  * Drive
  * Path
  * Branches svn url
  * Trunk svn url
  * Android ndk paths



The usage of Lazarus Manager is intuitive, so in first tab you will find base file urls. 

If you want to make shortcuts to desktop check the flag in left-bottom corner. 

[![Pre-Installation.png](https://wiki.freepascal.org/images/f/fb/Pre-Installation.png)](</File:Pre-Installation.png>)
    
    
    **NOTE** : If you choose to select "One click installation" (recommended), buttons **"Generate Directory Structure"** and **"Start Necessaries Download"** are _[disabled](</index.php?title=disabled&action=edit&redlink=1> "disabled \(page does not exist\)")_ and the installation process will do all the work for you.
    

  
In "Stable Tree" tab there are the svn urls (you can change with newer revision when they will came out) and targets: 

  


[![Stable-Tree.png](https://wiki.freepascal.org/images/e/e0/Stable-Tree.png)](</File:Stable-Tree.png>)

  
If you need to cross-compile Lazarus app for Android, you need to download and extract the NDK in your hard disk, then you had to save two paths: 

  * arm-linux-androideabi
  * binutils



  
E.g. if you use Windows X64 (I tested on Windows 8.1 X64), download and initialize Android NDK for Windows X64. So, if you have version r9 of ndk the requested paths will be: 

  * C:\Android\android-ndk-r9\toolchains\arm-linux-androideabi-4.8\prebuilt\windows-x86_64\arm-linux-androideabi\bin
  * C:\Android\android-ndk-r9\toolchains\arm-linux-androideabi-4.8\prebuilt\windows-x86_64\bin



  


[![Trunk-Tree.png](https://wiki.freepascal.org/images/5/5a/Trunk-Tree.png)](</File:Trunk-Tree.png>)

  
In "Install/Update" tab you can do the first installation or just update actual installation: 

  


[![Install-Update.png](https://wiki.freepascal.org/images/c/cb/Install-Update.png)](</File:Install-Update.png>)

### Update

In the end, if you want automatize update process you can make a link (or batch file) to LazarusManager.exe to put in Windows auto-start directory, or just launch it when necessary. An example of this link is supplied in zip file.   
  
LazarusManager accepts three parameters: 

  * First parameter 
    * **autoupdate** (it indicates that Lazarus should be updated automatically to latest svn, without interactions)



  


  * Second parameter 
    * **both** (it updates **both** branch and trunk trees)
    * **branch** (it updates **branch** tree)
    * **trunk** (it updates **trunk** tree)



  


  * Third parameter (**NOTE: i386-win32 target is always included**) 
    * **all** (it updates **all** targets)
    * **x64b** (it updates **x86_64-win64 branch** target)
    * **x64t** (it updates **x86_64-win64 trunk** target)
    * **andarm** (it updates **arm-android trunk** target)
    * **linarm** (it updates **arm-linux trunk** target)



  
Example 1: 
    
    
    LazarusManager.exe autoupdate both all
    

Example 2: 
    
    
    LazarusManager.exe autoupdate branch x64b
    

Wrong combinations are ignored, e.g. 
    
    
    LazarusManager.exe autoupdate branch andarm
    

  


### Directory structure

[![Directories.png](https://wiki.freepascal.org/images/c/c6/Directories.png)](</File:Directories.png>)

### How to start Lazarus

Lazarus Manager will create in lazarus branch/trunk roots a shortcuts that make start Lazarus with config dir inside the same root instead of Windows user directory, so you can have how many installations you want. 

To start Lazarus go to Lazarus root (branch or trunk) and double click on **Lazarus_B_Develop** for branch or **Lazarus_T_Develop** for trunk (**NOTE** : "Develop" is the general installation root, if you change it on install phase, it will be name as you choose). 

### Configure first Lazarus launch

Here it is an example of generic "first run", you need to use correct paths to ensure the right functionality. 

[![Configuration \(click to zoom\)](https://wiki.freepascal.org/images/b/b1/Config.png)](</File:Config.png> "Configuration \(click to zoom\)")

  
**NOTE** : remember to edit Tools -> IDE Options -> Environment to set paths concording the installation: 

  
[![IDE Options Environment.png](https://wiki.freepascal.org/images/2/26/IDE_Options_Environment.png)](</File:IDE_Options_Environment.png>)

## Download

You can download Lazarus Manager from [here](<http://www.kjow.net/lazarus-utilities/>). 

## Extra Info

  * Lazarus Manager will save in the root you choose to install Lazarus a log.txt file with the entry **last** process made (e.g. First Installation, Update, etc).


  * In log form or in log.txt, at the end of installation there il a reminder of which paths you need to add at the Windows **System PATH** , e.g.:


    
    
    ...
    ...
    ...
    ********************
    To complete the installation, please add these paths to System PATH:
    C:\Develop\fpc\Bootstrap;C:\Develop\fpc\Binutils;C:\Develop\fpc\Utils\bin\i386-win32;C:\Program Files\SlikSvn\bin\;C:\Develop\fpc\fpc_2.6.x\bin\i386-win32;C:\Develop\fpc\fpctrunk\bin\i386-win32
    ********************
    

## See also

  * [Installing Lazarus](<Installing_Lazarus.md> "Installing Lazarus") Official way to install Lazarus
  * [fpcup](<fpcup.md> "fpcup") Another tool that installs/updates FPC+Lazarus for you.



## Credit

[Kjow](</index.php?title=User:Kjow&action=edit&redlink=1> "User:Kjow \(page does not exist\)") 01:00, 10 January 2014 (CET)  
Latest Revision: [Kjow](</index.php?title=User:Kjow&action=edit&redlink=1> "User:Kjow \(page does not exist\)") 15:49, 10 January 2014 (CET)

---

_Source: [https://wiki.freepascal.org/Lazarus_Manager](https://web.archive.org/web/20190819123928/https://wiki.freepascal.org/Lazarus_Manager)_
