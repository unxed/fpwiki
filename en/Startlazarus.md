# Startlazarus

│ **English (en)** │  **[magyar (hu)](</Startlazarus/hu> "Startlazarus/hu")** │  **[polski (pl)](</Startlazarus/pl> "Startlazarus/pl")** │    
****

## Contents

  * 1 Overview
  * 2 Under Windows
  * 3 Searching the right executable
  * 4 Restarting the IDE
  * 5 Parameters
  * 6 See also



## Overview

Startlazarus is an application that works together with the IDE. Its primary task is to start the right IDE executable. Its secondary task is to restart the IDE. 

At the moment FPC cannot compile dynamic libraries with shared classes. That's why designtime packages are compiled statically into the IDE, which means every time you install/uninstall a package a new IDE executable must be compiled. 

Linux and macOS users compiling Lazarus from svn don't need startlazarus, but it does not hurt to use it. 

## Under Windows

Windows locks exe files while running the application. The IDE compiles a lazarus.new.exe. When the IDE restarts it starts startlazarus and quits itself. startlazarus then renames the old lazarus.exe to lazarus.old.exe and renames the new lazarus.new.exe to lazarus.exe. Then it starts lazarus.exe and quits itself. 

## Searching the right executable

When the Lazarus directory is write protected, as in Unix systems, the IDE builds its new executable into a sub folder of the primary config path, for example ~/.lazarus/bin/. You can see the primary config path in View / IDE internals / About IDE. 

Once you run startlazarus, it searches and starts the newest lazarus executable. Initially it starts the original from the installation directory. After Lazarus has rebuilt itself, startlazarus starts the newly built version. 

## Restarting the IDE

When the IDE restarts, it starts _startlazarus_ and quits. startlazarus waits for the IDE to quit, searches for the new executable and starts it. Under Windows it renames the exe. 

## Parameters

\--debug
    if given, startlazarus calls the IDE with "--debug-log=<primaryconfigpath>/debug.log".

\--lazarus-pid=
    passed by the IDE on restart. startlazarus waits for this process ID before starting a new IDE.

\--no-splash-screen or --nsc
    do not show splash screen

\--primary-config-path= or --pcp=
    [primary config path](<pcp.md> "pcp"), passed to the IDE, since 0.9.31 used by startlazarus to find the new executable

\--secondary-config-path= or --scp=
    secondary config path, passed to the IDE, not used by startlazarus

## See also

  * [Lazarus directory permissions on Linux and Unix](<Lazarus_directory_permissions_on_Linux_and_Unix.md> "Lazarus directory permissions on Linux and Unix")

---

_Source: [https://wiki.freepascal.org/Startlazarus](https://web.archive.org/web/20240615011532/https://wiki.freepascal.org/Startlazarus)_
