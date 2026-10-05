# GDB on OS X Mavericks or newer and Xcode 5 or newer

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

## Contents

  * 1 Overview
  * 2 Homebrew
    * 2.1 Install homebrew
    * 2.2 Install gdb
  * 3 Fink
    * 3.1 Install Fink
    * 3.2 Install a version of gdb
  * 4 Codesigning gdb
  * 5 Known issues & Work-Arounds



# Overview

This page explains how to build and install gdb on macOS using #Homebrew or #Fink. 

Since Mavericks 10.9, Xcode 5 no longer installs _gdb_ by default and not globally. 

For general notes about installing FPC and Lazarus see [Installing Lazarus on macOS](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS"). 

About using _gdb_ see [GDB Debugger Tips](<GDB_Debugger_Tips.md> "GDB Debugger Tips"). 

# Homebrew

## Install homebrew

Open a **Terminal** and install homebrew: 
    
    
    ruby -e "$(curl -fsSL <https://raw.githubusercontent.com/Homebrew/install/master/install>)"
    

Here is an example output: 
    
    
    mattias@mac:~$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ==> This script will install:
    /usr/local/bin/brew
    /usr/local/Library/...
    /usr/local/share/man/man1/brew.1
    ==> The following directories will be made group writable:
    /usr/local/.
    /usr/local/bin
    /usr/local/lib
    /usr/local/share
    /usr/local/share/doc
    ==> The following directories will have their group set to admin:
    /usr/local/.
    /usr/local/bin
    /usr/local/lib
    /usr/local/share
    /usr/local/share/doc
    
    Press ENTER to continue or any other key to abort
    ==> /usr/bin/sudo /bin/chmod g+rwx /usr/local/. /usr/local/bin /usr/local/lib /usr/local/share /usr/local/share/doc
    
    WARNING: Improper use of the sudo command could lead to data loss
    or the deletion of important system files. Please double-check your
    typing when using sudo. Type "man sudo" for more information.
    
    To proceed, enter your password, or type Ctrl-C to abort.
    
    Password:
    ==> /usr/bin/sudo /usr/bin/chgrp admin /usr/local/. /usr/local/bin /usr/local/lib /usr/local/share /usr/local/share/doc
    ==> Installing the Command Line Tools (expect a GUI popup):
    ==> /usr/bin/sudo /usr/bin/xcode-select --install
    xcode-select: note: install requested for command line developer tools
    Press any key when the installation has completed.
    ==> Downloading and installing Homebrew...
    remote: Finding bitmap roots...
    remote: Counting objects: 136798, done.
    remote: Compressing objects: 100% (45940/45940), done.
    remote: Total 136798 (delta 94648), reused 131779 (delta 89816)
    Receiving objects: 100% (136798/136798), 25.15 MiB | 738.00 KiB/s, done.
    Resolving deltas: 100% (94648/94648), done.
    From https://github.com/mxcl/homebrew
     * [new branch]      master     -> origin/master
    HEAD is now at 55d2970 play/sox conflict
    ==> Installation successful!
    You should run `brew doctor' *before* you install anything.
    Now type: brew help
    

## Install gdb
    
    
    brew install <https://raw.github.com/Homebrew/homebrew-dupes/master/gdb.rb>
    

or 
    
    
    brew install gdb
    

Here is an example output: 
    
    
    mattias@mac:~$ brew install https://raw.github.com/Homebrew/homebrew-dupes/master/gdb.rb
    ######################################################################## 100,0%
    ==> Installing gdb dependency: readline
    ==> Downloading https://downloads.sf.net/project/machomebrew/Bottles/readline-6.2.4.maveri
    ######################################################################## 100,0%
    ==> Pouring readline-6.2.4.mavericks.bottle.1.tar.gz
    ==> Caveats
    This formula is keg-only: so it was not symlinked into /usr/local.
    
    macOS provides the BSD libedit library, which shadows libreadline.
    In order to prevent conflicts when programs look for libreadline we are
    defaulting this GNU Readline installation to keg-only.
    
    Generally there are no consequences of this for you. If you build your
    own software and it requires this formula, you'll need to add to your
    build variables:
    
        LDFLAGS:  -L/usr/local/opt/readline/lib
        CPPFLAGS: -I/usr/local/opt/readline/include
    
    ==> Summary
    /usr/local/Cellar/readline/6.2.4: 31 files, 1,6M
    ==> Installing gdb
    ==> Downloading http://ftpmirror.gnu.org/gdb/gdb-7.6.1.tar.bz2
    ######################################################################## 100,0%
    ==> ./configure --prefix=/usr/local/Cellar/gdb/7.6.1 --with-system-readline --with-python=
    ==> make
    ==> make install
    ==> Caveats
    gdb requires special privileges to access Mach ports.
    You will need to codesign the binary. For instructions, see:
    
      http://sourceware.org/gdb/wiki/BuildingOnDarwin
    ==> Summary
    🍺  /usr/local/Cellar/gdb/7.6.1: 27 files, 5,0M, built in 105 seconds
    

The new gdb does not run yet. You will get the error: 
    
    
    The GDB command:
    „-exec-run“ returned the error:
    „,msg=„Unable to find Mach task port for process-id 62593:
    (os/kern) failure (0x5). (please check gdb is codesigned - see taskgated(8))““
    

Follow the instructions in the #Codesigning gdb section to solve this. 

# Fink

## Install Fink

Install fink according to the description here: <http://www.finkproject.org/download/index.php>

## Install a version of gdb

There is the choice between gdb with modifications of apple and more recent version of gdb without apple mods. 
    
    
    :~$ fink install apple-gdb
    
    :~$ fink install gdb
    

The first gives you /sw/bin/gdb, the second /sw/bin/fsf-gdb. You can use either of the two, but only after you have code signed them (See below). 

# Codesigning gdb

Follow the instructions on the [gdb wiki](<https://sourceware.org/gdb/wiki/BuildingOnDarwin#Giving_gdb_permission_to_control_other_processes%7Cofficial>). That page also contains troubleshooting hints. 

If you installed apple-gdb via fink, there are two issues: 

  1. The file `/sw/bin/gdb` is a script that invokes the actual gdb binary. Codesigning the script is possible, but does not achieve anything. The actual binary that you have to codesign is located at `/sw/lib/apple-gdb/gdb`
  2. The author of the `/sw/bin/gdb` script does not like the codesigning method of giving gdb permissions to debug programs (because you still have to provide an administrator password in that case), and therefore that script will still produce an error even after codesigning the gdb binary. The easiest way to work around this is to start the debugger directly using `/sw/lib/apple-gdb/gdb` instead.



# Known issues & Work-Arounds

  1. **Breakpoints might not work** : If that happens, go to your **Project Options** and change the **Type of debug info** from _Automatic_ to **Dwarf2 (-gw2)**. Do a project Build, and try running your application again via the IDE.

---

_Source: [https://wiki.freepascal.org/GDB_on_OS_X_Mavericks_and_Xcode_5](https://web.archive.org/web/20240526170702/https://wiki.freepascal.org/GDB_on_OS_X_Mavericks_and_Xcode_5)_
