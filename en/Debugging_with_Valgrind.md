# Debugging with Valgrind

│ **English (en)** │

## Contents

  * 1 Introduction
  * 2 Configuration
    * 2.1 Install valgrind in your system
    * 2.2 Compile your program
    * 2.3 Configure Lazarus debugger.
  * 3 Debugging
    * 3.1 Start Valgrind and VGDB in a separate shell
    * 3.2 Run your program in Lazarus and debug it
  * 4 Other suggestions
  * 5 See also



## Introduction

Valgrind is system for debugging and profiling Linux programs. It comprises of the Valgrind framework and set of tools that can automatically detect many memory management and threading bugs, and profile your programs in detail. You can read about Valgrind here <https://valgrind.org/>

A program running under Valgrind is not executed directly by the CPU. Instead it runs on a synthetic CPU provided by Valgrind. This allows Valgrind to have full control and track program and resources very meticulously. 

Valgrind can be used as standalone issue reporting or performance profiling tool. Also it can be used as a debugger. For this it includes VGDB utility that is GDB-compatible interface to Valgrind. 

The debugging applications stack is following: 

  * GDB
  * VGDB
  * Valgrind executing YourProgram



Though GDB can connect to VGDB directly (via interprocess communication), Lazarus cannot use this mode. You have to use Remote debugging over TCP. In this setup VGDB plays role of GDBServer listening on a TCP port, and Lazarus via GDB connects to it. 

So the debugging applications stack with Lazarus will be: 

  * Lazarus IDE
  * GDB
  * tcp/ip
  * VGDB
  * Valgrind executing YourProgram



  
Usually you will run all parts on same computer. But there is possibility to run them on 2 computers via network. 

In the following example everything will be on same computer. Network interface will be localhost and TCP port will be 33333. 

## Configuration

### Install valgrind in your system

Refer to Valgrind resources to install it. 

  


### Compile your program

(here it is named valgrind_test1.pas) with Valgrind option (-gv). Select menu Project -> Project Options 

[![valgrind project options.png](https://wiki.freepascal.org/images/4/49/valgrind_project_options.png)](</File:valgrind_project_options.png>)

  


### Configure Lazarus debugger.

Select menu Tools -> Options 

[![valgrind tools options.png](https://wiki.freepascal.org/images/d/d5/valgrind_tools_options.png)](</File:valgrind_tools_options.png>)

## Debugging

### Start Valgrind and VGDB in a separate shell
    
    
    valgrind --tool=memcheck --leak-check=yes --log-file=valgrind.trc --vgdb-error=0 ./valgrind_test1 &
    sleep 2; vgdb --port=33333
    

  


### Run your program in Lazarus and debug it

Actually the program is already run by Valgrind, it is just staying in debugger and waiting for continue. 

Please note Valgrind is accurate but it is not a fastest debugger. Execution will be much slower than usual. Be patient. 

Lazarus via VGDB communicates with the program in Valgrind. You can set breakpoints, inspect variable values and so on, like in regular debugger. 

Lasarus will stop on lines where Valgrind throws exceptions. Valgrind tracks memory usage very carefully and detects the issues where FPC, RTL and a Memory Manager may ignore memory misuse. Like in example below, at line 4. 
    
    
      mo:=TMyObj.Create;
      mo.Field:=12345;
      mo.Free;
      if mo.Field = 0 // Mistakenly accessing released memory block 
      then exit;
    

Lazarus will show a message when Valgrind issues an exception. 

[![valgrind error.png](https://wiki.freepascal.org/images/a/ab/valgrind_error.png)](</File:valgrind_error.png>)

After you close it Lazasarus will position you on a source row where the exception has happened. 

Lazarus tells you pretty much nothing about what actually happened. Valgrind will give you lots of details. To get these details you can either take a look into Valgrind trace file itself. But more convenient is to load the trace file into IDE. Select menu View -> Leaks and Traces Open valgrind.trc 

[![valgrind trace.png](https://wiki.freepascal.org/images/6/60/valgrind_trace.png)](</File:valgrind_trace.png>)

Here you can see the errors descriptions and three call stacks for each error: for the place where the exception happened, where the memory was freed and where it was allocated. All this is especially helpful for bug fixing. Select a line, double click and Lazarus will open a file and position cursor at a line in question. 

## Other suggestions

Sometimes it can be still unclear if some memory misuse or a leak happened inside RTL or OS binaries. You can compile RTL with debugging info for Valgrind and without optimization. Please refer to FPC Wiki how to rebuild RTL. 

  
In some cases Valgrind can generate so called false positives. For example, a small variable was allocated in stack. 
    
    
    var A2: array[2] of char;
    

Then the variable was populated from a file and then compared to a constant 
    
    
     if A2 = 'AA' then ...
    

FPC may choose to use MMX instructions for the comparison and allocates 16 bytes block for it, populating just first 2 bytes from the variable. Valgrind may detect it as an error of reading from uninitialized block. Technically it is correct - only two bytes were initialized, and the rest 14 bytes of the alignment block were not initialized but still scanned by MMX instruction. FPC is aware of this, it will mask the result and never use the result of the scan from masked bytes. But Valgrind still treats this as an error. As soon as bytes were allocated and scanned - it is access to uninitialized memory block. 

You can suppress reporting for such logical false positives and for other issues in OS or 3-rd party binaries like GTK, X11, etc, to concentrate more on your code. Please refer to Valgrind documentation regarding suppressions. 

## See also

  * [ General tips/info on debugging](<GDB_Debugger_Tips.md> "GDB Debugger Tips")

---

_Source: [https://wiki.freepascal.org/Debugging_with_Valgrind](https://web.archive.org/web/20221129002220/https://wiki.freepascal.org/Debugging_with_Valgrind)_
