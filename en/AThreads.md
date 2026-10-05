# AThreads

This page is about the threading implementation for Amiga-like systems in Free Pascal, which supports [Amiga](<Amiga.md> "Amiga"), [MorphOS](<MorphOS.md> "MorphOS") and [AROS](<AROS.md> "AROS"). AThreads unit is available since Free Pascal 3.0. 

[![Amiga-checkmark.png](https://wiki.freepascal.org/images/f/f2/Amiga-checkmark.png)](</File:Amiga-checkmark.png>)

This article applies to [Amiga](</Category:AmigaOS> "Category:AmigaOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![AROS-mascotte.png](https://wiki.freepascal.org/images/c/c3/AROS-mascotte.png)](</File:AROS-mascotte.png>)

This article applies to [AROS](</Category:AmigaOS> "Category:AmigaOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![MorphOS-butterfly.png](https://wiki.freepascal.org/images/f/f0/MorphOS-butterfly.png)](</File:MorphOS-butterfly.png>)

This article applies to [MorphOS](</Category:AmigaOS> "Category:AmigaOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 General overview
  * 2 Usage
  * 3 Stack
  * 4 Terminating threads
  * 5 Library usage in subthreads
  * 6 bsdsocket.library issue
  * 7 task^.tc_UserData issue
  * 8 threadvars
  * 9 RTL Events



## General overview

Amiga-like systems don't support threads, so AThreads emulates threading with separate processes. This is not an issue in general, because Amiga-like systems are not memory protected and use a shared address space. Therefore Amiga processes are barely more than threads on other systems. However, they still have some unique properties, which have to be kept in mind when one uses threading on Amiga-like systems. For the most important ones, read below. 

## Usage

Much like Unix systems, Amiga-like systems doesn't have a default ThreadManager in the System unit. Therefore, AThreads unit, which provides a native ThreadManager **must** be included as the **first** unit in the main program's uses clause. Similarly to Unix systems' CThreads unit. 
    
    
    program myapp;
     
    uses
      {$IFDEF HASAMIGA}
      AThreads,
      {$ENDIF}
      // additional units...
    

## Stack

Unlike other systems, where a 4MB default stack is enforced for subthreads, with AThreads they inherit the stack size of the main thread instead. AThreads follows the Delphi convention and provides the **const STACK_SIZE_PARAM_IS_A_RESERVATION** to specify as a flag for _BeginThread_ , to change the default allocation. You can use the **{$MEMORY}** directive to change the stack size for the main thread. For TThread-based subthreads, there's no way to change the inherited stack size directly, the stacksize argument of the constructor does *NOT* work currently. This is to be fixed in the future. 

## Terminating threads

Amiga-like systems won't clean up the subthreads when the main process exits, since the threads are individual processes there. Therefore it's unsafe to: 

  * trying to directly exit the app from a subthread
  * not exiting running threads properly, before the main thread attempts to exit



AThreads has some protection to wait the running subthreads to exit on the main process' termination. In these cases, this protection will cause a deadlock, and the app will wait forever to exit. Otherwise the system would free the main process' code segment on exit, which is shared with the other still running processes, and this condition would most likely cause a system crash immediately. 

Additionally, please note that the timeout argument of **WaitForThreadTerminate** function is *NOT* supported. This is similar to CThreads on Unix systems, where it is also not supported. 

## Library usage in subthreads

Some shared libraries (like _bsdsocket.library_ , see below) in the Amiga environment requires to be opened per process. If you interface to these libraries, the RTL provides the following functions to allow you to hook the Open/Close code for these libraries into threading: 
    
    
    Procedure AddThreadInitProc(Proc: TProcedure);
    Procedure AddThreadExitProc(Proc: TProcedure);
    

For a working example, see the Amiga-specific implementation of the _sockets_ unit. 

## bsdsocket.library issue

_bsdsocket.library_ which provides a BSD-style network API on Amiga-like systems, needs to be opened per process (therefore per subthread with AThreads). Additionally, socket handles cannot be shared among threads directly. The library provides several functions and constants to pass socket-handles among threads: 
    
    
    { Definition for Release(CopyOf)Socket unique id }
    const
      UNIQUE_ID = -1;
    
    { Amiga-specific functions for passing socket descriptors between threads (processes) }
    function ObtainSocket(id: LongInt; domain: LongInt; _type: LongInt; protocol: LongInt): LongInt;
    function ReleaseSocket(s: LongInt; id: LongInt): LongInt;
    function ReleaseCopyOfSocket(s: LongInt; id: LongInt): LongInt;
    

These are declared in the _sockets_ unit. There's no way to hide this functionality and make it transparent without a major performance hit, so networking code which uses threads simply has to be adapted to this Amiga behavior. For the documentation of these functions, see the SDK of your system. 

## task^.tc_UserData issue

Amiga-like systems provide a pointer-sized field in their task structure for the task itself to use, which is called _tc_UserData_. AThreads currently overwrites this field, so it's no longer available to the user program. This behavior is likely to be changed in the future, but mixing AThreads and Amiga-API threading solutions is unsafe anyway. (Note that FPC programs can still use _tc_UserData_ , but not together with AThreads unit.) 

## threadvars

Threadvars are fully supported, however they have a quite large performance penalty compared to normal global variables. It's advised to cache threadvars in local variables when used extensively in a performance-critical function. 

## RTL Events

RTL events are currently unsupported. They will be implemented in the future.

---

_Source: [https://wiki.freepascal.org/AThreads](https://web.archive.org/web/20250118081439/https://wiki.freepascal.org/AThreads)_
