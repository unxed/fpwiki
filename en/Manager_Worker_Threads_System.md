# Manager Worker Threads System

│ **English (en)** │  **[français (fr)](</Manager_Worker_Threads_System/fr> "Manager Worker Threads System/fr")** │    
****

Lazarus offers access to FPC's multi-threaded environment libraries under Linux and Windows. If you are looking to develop lightning fast native engines specific to Linux or even Windows 64 this system is a great start to helping you understand how best to leverage multiple cores that modern scientific applications require to process vast amounts of data in real-time. 

This example is meant to get you thinking on how best to engineer a multi-threaded system that is re-entrant in nearly every aspect, as well as to demonstrate how to use Critical Sections to protect memory objects by synchronizing access to such objects. 

## Contents

  * 1 One Manager Thread
  * 2 Collection of Worker Threads
  * 3 Critical Sections
  * 4 Putting Threads To Sleep
  * 5 Adding Data
  * 6 Push Data In
  * 7 Propagate Data to the Queue
  * 8 Data inside the Queue
  * 9 Processing Data
  * 10 Processed Data
  * 11 Unit Files
  * 12 See also



## One Manager Thread

Define as many of the lists of objects, data structures, and Critical Sections here as well as the list of Threads needed to bring your system up to scale. In the example provided, creating an instance of a Manager Thread will require the scale factor desired. Since this is just a simple example, that scale factor is static but with propper locking mechanisms this system could be upgraded to feature dynamic scaling. 

## Collection of Worker Threads

The Manager thread maintains a list of Thread objects it "manages" and typically does not do much to interfere with operations of each worker thread. The best way to design highly efficient multi-threaded systems is to code worker threads as tight as you can with as little critical sections as possible. And if you need to lock remember that all other threads at that instant in time **could** be placed in a wait state until that lock is released. 

## Critical Sections

When protecting data from multiple threads from writing to data at the same place a Critical Section is needed. Critical Sections are supported and if you have any Windows Experience it is similar with a few caveats which are discussed in this article. 

[InitCriticalSection](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/initcriticalsection.html> "doc:rtl/system/initcriticalsection.html")(Lock : TRTLCriticalSection) - The name of this procedure is different from Windows API where the old name was InitializeCriticalSection. You must call this procedure because it is required for locks to perform. 

[DoneCriticalSection](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/donecriticalsection.html> "doc:rtl/system/donecriticalsection.html")(Lock : TRTLCriticalSection) - The name of this procedure is also different from Windows API where the old name was DeleteCriticalSection. You must call this procedure because it enables the Operating System to free memory it allocated to perform locking on your threads. 

[EnterCriticalSection](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/entercriticalsection.html> "doc:rtl/system/entercriticalsection.html")(Lock: TRTLCriticalSection) - The name of this procedure is identical to Windows API. You must think carefully where you place these and you always follow this up with an exception handling block. 

[LeaveCriticalSection](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/leavecriticalsection.html> "doc:rtl/system/leavecriticalsection.html")(Lock: TRTLCriticalSection) - The name of this procedure is identical to Windows API. You must ensure that this procedure is called finally in your Locking/Unlocking block. There is one exception to this rule but you really need to keep track of locks. In the event that a method can take long periods of time it is conceivable that you would unlock the thread, if you know that an operation is going to take a significant amount of time, and re-lock it again. In that event, just make sure you handle all exceptions so that the Lock is eventually unlocked. 

Example Block that Performs Locking and Unlocking 
    
    
    EnterCriticalSection(Lock);
    try
      // Perform Code here
    finally
      LeaveCritialSection(Lock);
    end;
    

## Putting Threads To Sleep

Under Windows, making a thread go to sleep is best accomplished **not** by the **Sleep** method, rather the **WaitForSingleObject** method. Waiting for an event places threads in a stasis where there is little to no CPU cycles being used by the system until such time as an event was signaled or the specified timeout has been reached. 

Since FPC did not offer a WaitForSingleObject method, the best way to achieve highly efficient systems is to use Event driven Waits. Even though you know that no event will trigger an interrupt to that wait, it is best to use an Event. Therefore, in the example included, instead of using a Sleep(WAIT_MILLISECONDS) I included a technique which creates one Event Handle per Manager. Keep in mind most Lazarus applications, most likely will include only one instance of a Manager, but these exemplary units can be used in applications that contain multiple instances of the Manager. 

## Adding Data

When adding data to the system, keep in mind that memory is required for each data object for the duration of its cycle. In the example included, there is a data structure defined and represents a file that exists on a network or on the local machine. This system takes input as data objects, queues them, and processes them. 

For other data objects, this project can be extended to use a thread-pooled SQL connector and issue SQL statements to retrieve data to process and even callbacks to store processed data objects after worker threads processed them. 

## Push Data In

First, data is converted to a structure and added to a collection of items to be added to the queue. Organized this way, the locking only occurs if you have multiple threads trying to push data into the system. 

This is highly efficient in many ways. Take adding data from the main application thread for one example: Locking never occurs. This is accomplished because only the main application thread will ever get a chance to add items. Take adding data from another multi-threaded instance example: locking only occurs to the instance thread that is trying to add data to the queue. So under both scenarios, ALL Worker threads are not put into a sleep mode while data is being added to the system. 

## Propagate Data to the Queue

Before data even reaches any one individual worker thread it must be queued. The manager is maintaining which items have been added, queued, and finally processed. The worker threads are only requesting more data to process. Data propagates to the queue in controlled fashion by way of a Process method inside the Manager thread's execution. 

## Data inside the Queue

Data resides in the queue until a worker thread requests a Data Object. Having established a lock, no other worker threads can be assigned data until their lock has been established. The first Data object is pulled from the Queue list, removed from the Queue list, added to the Processing list, where it will wait until the next call to get more data will occur. This is highly efficient because it serves two purposes in one call by Placing the data objects in their appropriate list, as well as feeding new data to a particular worker thread. Items added to the system are processed in a First In, First Out (FIFO) method because items added first will be the first to be processed. 

**Note:** FILO would have been better if I were using a TThreadDataPointers rather than a TList object. The reason is because TList.First and SetLength(). I would be pivoting off the Last Element in the TThreadDataPointers and using SetLength(Length-1) to reclaim memory space. These are things you will need to consider when designing your system. 

## Processing Data

The concept of processing data from a Worker Thread standpoint will be the most critical piece of code you will write. Every declaration is going to cost cycles. If you can, declare variables local to the thread object and let them be, Declaring variables inside the process method is going to be at too high a cost however, processing does need to happen here so just think twice. 

  * Introduce NO waits here.
  * Use Exception Handlers here or it will disrupt the processing engine.
  * Be right to the point with your code. Try to optimize everything. The execution of code can be slow and costly and will hamper the number of simultaneous threads your system can allocate if you are sloppy.



Declare variables local to the thread object as private and deallocate them at the Destruction event of the Worker Thread. It would be costly to keep allocating and deallocating memory during the Process method or any methods you add to process a single data object. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** A single data object can be much more than a string. You could make these an NxM matrix of Int64s or Floats or what ever you need.

## Processed Data

This example pushes Processed data during the GetNextItem call to the Manager. This was done because there was an efficiency gain to lock the list at that point and move data where it belongs all at the same time. Your needs may differ however, think about how you want to organize your data before actually committing to the way in which to implement. 

This example does not include a callback to notify the main application that a data object has just been completed. It is something that may or may not be required since merely adding a file to this system would pretty much guarantee it was processed. 

Which leads us into logging, while entirely another matter, is useful. I have experience with Windows and TFileStream being thread safe but that is *Windows*. Under unix, I would probably stick to adding a custom log file using TFileStream rather than posting data to any system log. The reason is because these simultaneous systems can create tens of thousands of log entries per second if done properly, and most system administrators won't appreciate cleaning out log files on your behalf. 

## Unit Files

The complete example can be downloaded from [Lazarus CCR](<https://sourceforge.net/project/showfiles.php?group_id=92177&package_id=148359&release_id=648747>) on SourceForge. 

## See also

  * [Multithreaded Application Tutorial](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial")
  * [Parallel procedures](<Parallel_procedures.md> "Parallel procedures")

---

_Source: [https://wiki.freepascal.org/Manager_Worker_Threads_System](https://web.archive.org/web/20241212105446/https://wiki.freepascal.org/Manager_Worker_Threads_System)_
