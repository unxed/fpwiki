# Threads

│ **English (en)** │  **[polski (pl)](</Threads/pl> "Threads/pl")** │  **[русский (ru)](<../ru/Threads.md> "Threads/ru")** │    
****

Free Pascal supports thread programming, with a procedural and object-oriented interface that is mostly platform-neutral. 

The official documentation is here: [Programmer's guide chapter 10](<https://www.freepascal.org/docs-html/prog/progch10.html>). It describes the procedural threading interface thoroughly. 

The object-oriented interface `TThread` is described on this more comprehensive page: [Multithreaded Application Tutorial](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial")

  


## Contents

  * 1 Notes on procedural thread management
  * 2 Thread synchronisation
    * 2.1 Critical sections
    * 2.2 RTLevent
    * 2.3 Semaphores
    * 2.4 WaitForThreadTerminate



## Notes on procedural thread management

`BeginThread` returns a thread handle directly, but also returns a thread ID as an output parameter. On Posix systems, there are no thread handles, so a copy of the thread ID is returned as both the ID and handle. On Windows, the thread handle is used for all thread management, while the ID is a separate unique value used as a thread enumerator. Therefore, regardless of platform, the thread handle returned by `BeginThread` is what all other RTL threading functions expect. (`CloseThread`, `SuspendThread`, `WaitForThreadTerminate`, etc) 

`GetCurrentThreadID` returns specifically the thread ID, not the handle. So, while you could use this on Posix systems to call RTL threading functions, this would fail on Windows. To get the handle, you could use `GetCurrentHandle` or `OpenThread` in the Windows API, or retain the handle you got from `BeginThread`. 

There are three steps in a thread's lifespan: 

  * Another thread creates it by calling `BeginThread`.
  * The thread completes its work, then exits its top level function; or ends itself explicitly by calling `EndThread`; or is killed by another thread by calling `KillThread`.
  * Another thread detects that the thread has terminated, and calls `CloseThread` to clean up.



`CloseThread` does nothing on Posix systems, but on Windows it releases the thread handle. If a program keeps creating new threads but neglects to close them, this is a resource leak and could lead to system instability in extreme cases. All handles are released when the program terminates, but it's still good practice to explicitly call `CloseThread` as soon as a thread exits. The return value of `CloseThread` is always 0 on Posix, but non-zero on Windows if successful. 

`DoneThread` and `InitThread` in the system unit are primarily for internal use, ignore them. 

`KillThread` should be avoided if at all possible. If a thread happens to be blocking a manual synchronisation mechanism when it is terminated, any other thread waiting for that mechanism may be left waiting forever. 

`SuspendThread` and `ResumeThread` are likewise dangerous, and not even supported on Posix systems, due to similar deadlocking concerns. Co-operative synchronisation is the safe alternative (threads themselves check periodically if they are being asked to shut down). 

`ThreadGetPriority` and `ThreadSetPriority` allow setting a thread's priority between -15 (idle) and +15 (critical), on Windows. On Posix these functions are not implemented yet through the procedural interface. 

There is a [GetCPUCount](<https://www.freepascal.org/docs-html/rtl/system/getcpucount.html>) function in the system unit. Use this to estimate at runtime how many parallel threads to create. 

  


## Thread synchronisation

Synchronisation is used to ensure different threads or processes access shared data in a safe manner. The most efficient synchronisation mechanisms are those provided by the operating system; FPC's synchronisation mechanisms act as a minimal platform-neutral wrapper around the operating system mechanisms. 

The native thread synchronisation mechanisms in the system unit are: critical sections, RTL events, semaphores (deprecated), `WaitForThreadTerminate`. The code for these can be found in **rtl/ <arch>/cthreads.pp** or **rtl/ <arch>/systhrd.inc**. 

Fully cross-process communication requires a more robust solution, as thread synchronisation mechanisms are insufficient. [SimpleIPC](<https://www.freepascal.org/docs-html/fcl/simpleipc/index.html>) may be appropriate for this. 

  


### Critical sections

The functions used are `InitCriticalSection`, `EnterCriticalSection`, `LeaveCriticalSection`, and `DoneCriticalSection`. 

Critical sections are a co-operative code mutex, allowing only a single thread at a time into the protected code section, provided each thread enters and exits the section cleanly. This is a safe cross-platform way of protecting relatively small blocks of code. 

Threads are only blocked from the section when they call `EnterCriticalSection`. If a thread is somehow able to get into the section without calling `EnterCriticalSection`, it is not blocked and the section remains unsafe. Threads are released into the critical section in FIFO order. 

The critical section mutex has a lock counter. If a thread holding the critical section mutex calls `EnterCriticalSection` repeatedly, it must call `LeaveCriticalSection` an equal number of times to release the mutex. If a thread exits the section without calling `LeaveCriticalSection`, eg. due to an unhandled exception, the mutex remains locked and other threads waiting for it will be deadlocked. 

Calling `LeaveCriticalSection` when the mutex is not locked causes an error on Posix systems, but on Windows it decrements the lock counter to below 0. Calling `DoneCriticalSection` while the mutex is still in use causes an error on Posix systems, but on Windows it just deadlocks any threads waiting on the mutex, though any thread already in the critical section is able to leave normally. 

  


### RTLevent

The functions used are `RTLEventCreate`, `RTLEventSetEvent`, `RTLEventResetEvent`, `RTLEventWaitFor`, and `RTLEventDestroy`. 

RTL events are the preferred cross-platform synchronisation method. RTL events start out as unset. They block threads waiting for them; when the event is set, a single waiting thread is released (in FIFO order) and the event is immediately reset. 

Due to platform differences, there is no way to directly query an event's current state. There is also no way to directly detect an event wait timeout, because any wait for the event causes an automatic state reset and waits do not have a return value. 

If a thread starts to wait for an RTL event that has already been set, the wait completes immediately and the event is automatically reset. The event does not count how many times it has been set, so multiple sets are still cleared by a single reset. Be careful: if you have 8 threads starting to wait for a single RTL event, which will be set 8 times, threads may become deadlocked if anything clears multiple sets in one reset. 

Destroying an RTL event while a thread is waiting for it does not release the thread. It is the programmer's responsibility to ensure no thread is waiting for the event when the event is destroyed. 

  


### Semaphores

A semaphore blocks threads waiting for it, and each `SemaphorePost` releases one waiting thread. 

The semaphore implementation in FPC's RTL was Posix-only, and has been deprecated, and removed as of FPC 3.2.0. If semaphores are to be reimplemented in the RTL, it must be compatible with Delphi's [SyncObjs.TSemaphore](<http://docwiki.embarcadero.com/Libraries/Tokyo/en/System.SyncObjs.TSemaphore>). Outside of the RTL, other custom implementations do exist, for example [here](<https://forum.lazarus.freepascal.org/index.php?topic=48032.0>). 

  


### WaitForThreadTerminate

`WaitForThreadTerminate` blocks the current thread until the target thread has exited. The timeout parameter is ignored on Posix platforms, and the wait will never timeout. 

`WaitForSingleObject` and `WaitForMultipleObjects` famously do not exist under Linux, so they're not good for cross-platform use. They can be of some use under Windows.

---

_Source: [https://wiki.freepascal.org/Threads](https://web.archive.org/web/20250427010754/https://wiki.freepascal.org/Threads)_
