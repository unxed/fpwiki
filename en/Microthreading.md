# Microthreading

## Contents

  * 1 Introduction
    * 1.1 CPU context switching methods
    * 1.2 Objectives
  * 2 Implementation
  * 3 External links



## Introduction

There are some situations where applications need to execute lots of asynchronous concurrent operations. OS-level threads could be used for this purpose, but an application can create only a limited count of threads. As for CPU utilization there is no significant performance gain if more threads are created than physical cores available on a CPU. In fact threads could be used for parallel processing on a multicore CPU and for writing more readable code. OS-level threads should be used mainly used for parallelism and better CPU utilization. If a programmer needs to write code which will be called asynchronously, then OS-level threads will be rather expensive in perspective of system resources. 

### CPU context switching methods

  * Cooperative (use of explicit [Yield](</index.php?title=Yield&action=edit&redlink=1> "Yield \(page does not exist\)") call) 
  * Preemptive (periodic timer based) 
  * Combined 



### Objectives

  * Unlimited number of instances (limited by available memory) 
  * Fast switching, creation, destruction 
  * Automatic thread pool management by physical CPU core count 
  * Ability to run in main loop only (without [TThread](</index.php?title=TThread&action=edit&redlink=1> "TThread \(page does not exist\)") instances) 
  * Provide own synchronization tools (Yield, Sleep, CriticalSection, Semaphore, Mutex, WaitForMultipleObjects, Queues, Synchronize, ...) 
  * Priority control 
  * Support for view list of all microthreads 



## Implementation

  * [MicroThreading](<http://svn.zdechov.net/PascalClassLibrary/MicroThreading/>) \- Lazarus package, functional yet not finished, not multi-platform, need patching FPC 



## External links

  * [C# Yield implementation in Delphi](<http://santonov.blogspot.com/2007/10/yield-you.html>)
  * [Cool little Coroutines function (Much better version)](<http://www.festra.com/wwwboard/messages/12899.html>)
  * [Fibers(Windows)](<http://msdn.microsoft.com/en-us/library/ms682661%28v=vs.85%29.aspx>)
  * [MeSDK Delphi coroutine implementation](<https://code.google.com/p/meaop/source/browse/trunk/MeObjects/src/uMeCoroutine.pas>)

---

_Source: [https://wiki.freepascal.org/Microthreading](https://web.archive.org/web/20230205114424/https://wiki.freepascal.org/Microthreading)_
