# Remote Debugging

****

## Contents

  * 1 Introduction
  * 2 Using SSH (Secure Shell)
    * 2.1 Requirements for Lazarus
      * 2.1.1 Configuring SSH
  * 3 Using gdbserver
  * 4 Using gdbserver on cross-platform remote
  * 5 Using avr-gdb



## Introduction

Remote debugging means you work on your local computer and you want to start and debug a program on another computer, the remote machine. In the following examples the name of the local computer is 'localcomp' and the name of the remote computer is 'remotecomp'. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Remote debugging will only work if the gdb supports async mode (see gdb doc for "set target async"). GDBServer usually does. GDB does for some targets only. 

In some cases gdb/gdbserver does support it, but not if "mi" mode is used (used by the IDE. A workaround for this must yet be implemented in the IDE.  


**IF async mode is not supported, or not for "mi" then:**

  * It is currently not possible to use the "Pause" function to interrupt the running application.
  * It is not possible to modify break-/watch-points, while the application in the debugger is running.
  * One should no switch editor tabs, or open/close units, while the application in the debugger is running.

The 2nd and 3rd can be done, if the app in the debugger is paused. The only way for the app to become paused is by reaching a breakpoint that was set before the app was run. 

## Using SSH (Secure Shell)

SSH (SSH client) is a program for logging into a remote machine and for executing commands on a remote machine. It provides secure encrypted communications between two untrusted hosts over an insecure network. X11 connections and arbitrary TCP/IP ports can also be forwarded over the secure channel. 

See the SSH man page for details. This text only covers SSH protocol 2. For differences for protocol 1 see the SSH man page. 

### Requirements for Lazarus

You must be able to log in via ssh to the remote machine (the computer where the program will run). This means the remote machine has an installed and running SSH server and you have an account allowed to login from your local machine (the computer where the Lazarus IDE is running). 

You can test this by doing: 
    
    
      
    ssh username@remotecomp ls -la
    

This will create an SSH connection to 'remotecomp' with the username 'username'. After authentication it will print out a directory listing and return. 

#### Configuring SSH

The IDE needs an SSH connection that does not prompt for a password. There are a lot of possibilities to achieve this. This text only describes a few. For security reasons it is strongly recommended that you read the SSH manpage. 

Solution 1: User based authentication. 

This will allow one specific user on the local computer to establish an SSH connection to the remote computer as a specific user without prompting for passwords. 

    ToDo: describe the server settings. On redhat this works without any change.

Step 1
    create the public and private keys on the local machine

This will create two files on the local machine: 
    
    
     ~/.ssh/id_rsa and ~/.ssh/id_rsa.pub
    

If you already have these files, skip this step. 
    
    
     
    ssh-keygen -t rsa
    

Keep the default and leave the passphrase empty. 

Step 2
    copy the public key of the local machine to the remote machine
    
    
      
    scp ~/.ssh/id_rsa.pub user@remotecomp:remote.pub
    

Step 3
    create the file ~/.ssh/authorized_keys2 on the remote machine
    
    
      
    ssh user@remotecomp
    touch ~/.ssh/authorized_keys2
    chmod 600 ~/.ssh/authorized_keys2
    

The chmod will set the permissions to only allow yourself to read the file. SSH requires this. 
    
    
      
    cat remote.pub >> ~/.ssh/authorized_keys2
    rm remote.pub
    exit
    

Step 4
    test

You should now be able to login without password. 
    
    
      
    ssh user@remotecomp
    

Step 5
    Setup the ssh debugger in the IDE

_ToDo_

* * *

Step 6
    Set "Run parameters"

For remote debug we need to set the correct value for "Host application", "Command line parameters" and "Working directory". Remember: **It is remote!**

[![remote debug run parameters.png](https://wiki.freepascal.org/images/f/f7/remote_debug_run_parameters.png)](</File:remote_debug_run_parameters.png>)

Step 7
    Set "Debugger parameters"

Select "Debugger type and path" to "GNU Debugger trough SSH(ssh)" 

[![remote debug debuger parameters.png](https://wiki.freepascal.org/images/a/a6/remote_debug_debuger_parameters.png)](</File:remote_debug_debuger_parameters.png>) _ToDo_

## Using gdbserver

For now this is the best approach I could find: Steps for remote debugging from Windows to Linux machine, other situations should not be so much different: 

1- Manually include gdbmidebugger the public "uses" clause of debugmanager.pas file located in lazarus/ide and recompile ide. 

2- Install gdb and gdbserver on Linux. 

3- Make a cross compile Lazarus. 

4- Set IDE options/Debugger to gdbserver and in the debugger options on the bottom set Debugger_Remote_Hostname to remote Linux machine IP or name and set the port to 2159. 

5- Add a Linux build mode to project option. 

6- Build project on Linux build mode. 

7- Use whatever tool like WinSCP to move binary file to Linux machine. for WinSCP use it like this : WinSCP.com /script=script.txt and content of script.txt will be like this (you can use WinSCP 5.8 and later session/ script menu to make and modify it) : 
    
    
    open sftp://username:remotepassword@remotenameorip/ -hostkey="XXXXXX"
    
    cd ./projectdir
    put -permissions=744 "C:\Project\project1"
    
    exit
    

8- Run gdbserver on remote machine like this : gdbserver host:2159 ./project1 

9- Set breakpoints where you want. 

10- Run project in your IDE and test it. 

For more reading see : 

  * [[Remote debugging from Windows to Linux](<http://forum.lazarus.freepascal.org/index.php/topic,32393.0.html>)]
  * [[Initial GdbServer post on forum](<http://forum.lazarus.freepascal.org/index.php/topic,19014.msg107930.html#msg107930>)]
  * [[target async support (ssh and gdbserver)](<http://forum.lazarus.freepascal.org/index.php/topic,20101.msg115408.html#msg115408>)]
  * [[Some of the gdb-server backend settings explained](<https://forum.lazarus.freepascal.org/index.php/topic,66960.msg514304.html#msg514304>)]



## Using gdbserver on cross-platform remote

When, say, you want cross-build an application for Raspberry Pi on Windows and then debug it remotely on RasPi, the default **gdb.exe** wouldn't work. You need to obtain **gdb-multiarch** (supports different remote architectures). For Windows, the easiest way is to install [MSYS2 distribution](<https://www.msys2.org>) and then install the package gdb-multiarch ([w64 gdb-multiarch](<https://packages.msys2.org/package/mingw-w64-x86_64-gdb-multiarch>)). 

Then, in Lazarus, in **Debugger backend** options, select "GNU remote debugger (gdbserver)", set "Debugger path" to **gdb-multiarch.exe** , and specify "Architecture" **arm**. 

## Using avr-gdb

I've managed to debug AVR programs using two different methods: 

Executing code through [simavr](<https://github.com/buserror/simavr>) (an AVR simulator)

simavr includes a gdbserver interface. To simulate and debug code, launch simavr with the -g option: 
    
    
    ./runavr -g -m atmega328p -f 16000000 <firmware>
    

By default a remote gdb connection is available on port 1234. 

Running on hardware and debugging over DEBUGwire using [dwire-debug](<https://github.com/dcwbrown/dwire-debug>)

Launch dwire as follows: 
    
    
    ./dwdebug device <serial port>, gdbserver, qr
    

By default a remote gdb connection is available on port 4444. The _qr_ option will result in dwire exiting with the target running when the debug connection is killed. Note that currently dwire returns _S00_ when hitting a breakpoint. This results in Lazarus displaying a popup dialog with an error message. This is to indicate that a non-standard signal was received and is merely an annoyance. See [this](<https://github.com/dcwbrown/dwire-debug/issues/33>) for a fix. 

To configure Lazarus for debugging over avr-gdb
    

  * Select _GNU remote debugger_ under Debugger options
  * Enter path to avr-gdb
  * Enter the appropriate port number
  * At the moment a Lazarus [patch](<https://bugs.freepascal.org/view.php?id=32084>) is required because no process ID is available on the embedded platform.



I've tested different versions of gdb compiled for AVR target: 

  * gdb 6.6 - Doesn't work because async mode isn't supported.
  * gdb 7.0.1 - ~~Works~~ Sometimes work, see note below.
  * gdb 8.0-9.1 - Doesn't work as is, but work with this [patch](<https://sourceware.org/bugzilla/show_bug.cgi?id=13519#c13>).
  * gdb 10.1 - This version finally includes the patch to fix the disassemble bug.



Notes 

  * After I upgraded Linux Mint and retested gdb 7.0.1 I noticed the following warning while testing a debug session:



`warning: (Internal error: pc 0xa2 in read in psymtab, but not in symtab.)` GDB would sometimes not work properly and debugging from Lazarus using this version of gdb could hang the IDE. 

  * Using gdb 10.1 with Lazarus gdbmi debugger gives an error on startup if the AVR RTL has been compiled with debug information. This error does not appear to affect the stability or functionality of gdb with Lazarus (based on preliminary testing).

---

_Source: [https://wiki.freepascal.org/Remote_Debugging](https://web.archive.org/web/20241108190359/https://wiki.freepascal.org/Remote_Debugging)_
