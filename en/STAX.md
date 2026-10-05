# STAX

**S** ingle **T** hreaded **A** synchronous E**X** ecution framework (STAX for short) enables async/await style co-routines for Free Pascal. 

Author: Frederic Kehrein. 

License: BSD 2-Clause License. 

## Contents

  * 1 Introduction
  * 2 Prerequisites
  * 3 AsyncNet
  * 4 Download



## Introduction

The basic idea behind STAX is to enable asynchronous execution flows without the use of multi-threading. The idea is to split the control flow up into small tasks, which all run on the same thread. Tasks will run until they voluntarily give up their execution time, and another can be scheduled. This introduces two guarantees: 1. there will never be two tasks running simultaneously (i.e. on different CPUs), 2. Tasks will only be interrupted when they are allowing it, i.e. never during any critical section. This completely eliminates the possibility for race conditions, and therefore allows for writing asynchronous code without the requirement for locks or synchronization mechanisms. This is a major advantage over classical threading, as locking and synchronization mechanisms create a lot of maintainance overhead and can easily introduce bugs like deadlocks. 

To guarantee a high degree of concurrency, it must be ensured that tasks yield often to the scheduler. To archive this, the programs need to be designed to consist of multiple small tasks. Rather than one task including a lot of functionality, the functionality must be separated into multiple smaller tasks, which will depend on one another. When one task requires the functionality of another task, it will schedule that task and then yield to the scheduler until that new task finished, also giving other waiting tasks the chance to be scheduled. If all tasks are small and often wait for other tasks, a high degree of concurrency can be archived. 

Another opportunity for tasks to yield to the scheduler is when waiting for events. This includes the simple sleeping for a certain amount of time, but also waiting for the system. A prime example is the waiting for blocking I/O. In networking applications receiving and sending is usually blocking, meaning when a system call to receive data is made, the system will block that thread until data is available. In STAX this waiting time, until data is available, can be used to schedule other tasks. An example for this can be seen in the "examples/tcptest" folder, which implements a TCP echo server which can serve multiple clients on a single thread 

Besides not requiring locks another advantage by having all tasks run on the same thread is, that this can be directly incorporated into LCL GUI applications. A very simple approach on how to use STAX in LCL applications can be seen in "examples/pong", where STAX is used to implement a two player Pong game using TCP, where the TCP connection is handled on the same thread as the GUI, being able to directly access the GUI without any form of synchronization mechanism. 

To give a small example on how such a STAX program would look, here is the tcp server example: 
    
    
    program server;
     
    {$mode objfpc}{$H+}
     
    uses
      stax, stax.asynctcp, stax.functional;
     
    // simple tcp echo server
    procedure HandleConnection(AExecutor: TExecutor; AConnection: TSocket);
    var
      c: Char;
    begin
      while True do
      begin
        // wait until a char was received
        c := specialize Await<Char>(specialize AsyncReceive<Char>(AConnection));
        Write(c);
        // asynchronously send the response
        AExecutor.RunAsync(specialize AsyncSend<Char>(AConnection, c));
      end;
    end;
     
    procedure RunServer(AExecutor: TExecutor; AHost: string; APort: Integer);
    var
      Sock: Tsocket;
      Conn: TSocket;
    begin
      Sock := TCPServerSocket(AHost, APort);
      TCPServerListen(Sock, 10);
      while True do
      begin
        Conn := specialize Await<TSocket>(AsyncAccept(Sock));
        // Asynchronously handle the communication to have this task continue to accept new clients
        AExecutor.RunAsync(specialize AsyncProcedure<Tsocket>(@HandleConnection, Conn));
      end;
    end;
     
    var
      exec: TExecutor;
    begin
      exec := TExecutor.Create;
      exec.RunAsync(specialize AsyncProcedure<String, Integer>(@RunServer, '0.0.0.0', 1337));
      try
        exec.Run;
      except on E: EUnhandledError do
        WriteLn('Unhandled error: ', E.Message);
      end;
      exec.Free;
      ReadLn;
    end.
    

More technical information can be found in the repositories README.md 

## Prerequisites

Author developed and tested STAX under Windows 10 and Linux, both x86_64 systems. On Windows it works right out of the box. On Linux it requires a small change to the RTL i.e. requires a custom FPC build. The required changes are stored as a diff in the fpc.patch of the FPCFiber repository (which is referenced as a submodule in the externals directory of the STAX repository). It can be applied with "git apply" in the local fpc-sources git repository. 

## AsyncNet

Author is also working on an networking lib AsyncNet, which shall provide the functionality of FCL-Net but for asynchronous use. It also provides with the asyncnet.sockets unit a replacement for the stax.asynctcp unit, now supporting also IPv6 as well as UDP. Besides this, it also includes DNS resolution functionality. 

## Download

It is available on GitHub: <https://github.com/Warfley/STAX>

FPCFiber repo: <https://github.com/Warfley/FPCFiber>

AsyncNet repo: <https://github.com/Warfley/AsyncNet>

---

_Source: [https://wiki.freepascal.org/STAX](https://web.archive.org/web/20250417161526/https://wiki.freepascal.org/STAX)_
