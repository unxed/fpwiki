# ThreadEvent

## Contents

  * 1 Enhancing the use of Threads: Thread events vs "parallel"
  * 2 Implementation of the "thread event queue"
  * 3 Thread events
  * 4 Defining a Thread Event
  * 5 Using a Thread Event
  * 6 Implementation of Thread Events
  * 7 See also



#### Enhancing the use of Threads: Thread events vs "parallel"

The **[parallel](<OpenMP_support.md> "OpenMP support")** paradigm imposes a huge change in how a program is to be written, is to be compiled and is to be "read" by a fellow programmer. Thus we would leave the safe ground of Delphi language here quite far. 

The compiler/runtime needs to dynamically create an unlimited count of threads. With **parallel for** this is under control of the user, with naked **parallel** the compiler would need to decide what code sequence is to be run in which thread. All this seems very tricky to me. 

OTOH, Delphi language programmers are used to work with "run to completion" events. (I.e. any code is placed in an "event handler" (a "callback") and is "fired" due to events that happen as a consequence of hardware or OS proceedings or are just directly fired (called) by other Delphi-language events.) 

But due to the make up of the RTL, such "events" only are applicable for the "Main Tread", while all "Worker Treads (created by TThread objects) just run in a "one-shot" or in a cyclic way. Event driven programming is impossible here. 

To make working with threads more usable for the "crowd", the same "event driven" paradigm should (optionally) hold for worker threads, too. To enable this, the RTL needs to implement an "event queue" for all threads in a similar way as it's done for the main thread: The user code in a thread is done in multiple event-handlers, that can be assigned to messages. Incoming messages are queued in a FiFo and the event handlers are "fired" by the RTL one after the other when taking each message from the top of the FiFo. 

#### Implementation of the "thread event queue"

Implementing this should be a lot easier and straight forward than **parallel** , as the threads to be used are managed more explicitly under user control and need not to be created and destroyed automatically by the RTL. 

I think a new class called (e.g.) TEventThread should be provided by the Free Pascal RTL. This might (or might not) be a sibling of TThread. Each object of this type has its own "event queue". This "event queue" is not implemented in the samy way as the "message queue" of the main thread (which only exists for GUI projects (e.g. done in Lazarus) and uses mechanisms of the underlying GUI mechanism (e.g. Windows or QT). It is implemented just by Delphi code creating a FIFO for the "event specification" and some SyncObject to allow the thread to wait for the next event. 

By this a non-GUI project can be created using the "normal" event driven Delphi language paradigm. IMHO this is a major benefit ! 

If the events to be fired are only allowed to be procedures without parameters (similar to TThread.Synchronize), the elements stored in the FiFo simply are of the type **procedure of object**. But IMHO this is a major restriction and we should allow for **in** parameters, **out** parameters and function results. I suppose somewhere in the RTL there already is a functionality to stream such parameters that can be reused to store them in the FiFo. 

Of course a "**procedure ... message** " can't be defined in a thread. This is only possible in the Main (GUI-) thread. 

When doing a GUI project a mechanism needs to be defined to use the main thread as a target for "thread events", as well. Thus a class similar to TEventThread (e.g. called TEventMainThread) should be provided that creates an event queue, uses the appropriate GUI based send-message mechanism to notify the main thread and is hooked in the TApplication message scheduling loop. 

When doing a non-GUI project, TEventMainThread could provide an event scheduling loop based on polling e.g. using CheckSynchronize() (or similar). 

All this can be done without modifying the language or the compiler just by enhancing the RTL. 

#### Thread events

To make this more easily usable, by modifying the compiler, this paradigm can be enhanced by introducing "Thread Events". 

Instead of using additional new language constructs we could enhance the "event" paradigm to optionally using a different thread for executing the code fired by the event. That should be more easily understandable and usable for Delphi programmers, as it does not change much in their picture of the project. 

For defining "events", Delphi language does not use a dedicated keyword, but you just define a property of a procedure or function type. Here I could imagine an "**of Thread** " directive usable similar to the **of Object** clause: 

#### Defining a Thread Event
    
    
    type 
      TThreadEventHandler = procedure(x: Integer) of Thread;
      test = class (TObject)
      private
        FThreadEventHandler : TThreadEventHandler; 
      public
       property ThreadEventHandler : TThreadEventHandler 
         read FThreadEventHandler write FThreadEventHandler;  
      end;
    

Similar to **of Object** , a variable of an **of Thread** type stores the entry address and a Self pointer. The Self pointer here is a sibling of TEventThead. If in runtime the content **is** not such a type, **of Thread** works like **of Object**. 

Thus in the end, we even don't need a new "**of Thread** " keyword, but can reuse the **of Object** keyword, as in runtime the meaning can easily be detected. 

#### Using a Thread Event

The most straight forward way to use a thread event is setting the variable (usually a property in some other object) to a class function of an appropriate TThread (or similar) sibling. With that, the self pointer is stored in the same way as with a normal event due to the **of Object** type. 

Maybe a mechanism (language construct or just a library function) to define a thread event with explicitly stating both the function to call and the thread context to be used, might be desirable. But here the function is not a class function of the TEventThread to be used and thus we need to store as well the Self pointer as the TEventThread pointer, making things a lot more complicated. So IMHO, for the beginning only class functions of a TEventThread sibling can be thread events. 

The Event handler needs to be able to notify the thread it had been fired by (including waking up this thread in case it is waiting for an event or a message). Thus this thread would provide another thread event for that notification. Here it's essential that the main thread (GIU thread in a GUI project or just the main execution code in a non-GUI project) instantiates an object that implements an event queue and (implements or attaches to) an appropriate wait/schedule loop. 

I think the "normal" use of TEventThreads and Thread Events is quite obvious. But Thread Events it can also be used if you want to issue multiple threads do parallel processing with multiple CPUs, which is the target of [another suggestion for making the use of threads easier to use](<OpenMP_support.md> "OpenMP support") in Free pascal. 

Of course with these projects, the thread event implementation needs a lot more user code that just inserting the "**parallel** " keyword as suggested in [OpenMP_support](<OpenMP_support.md> "OpenMP support"). I tried to draft a testing project [here](<thread_event_test_project_1.md> "thread event test project 1"). 

#### Implementation of Thread Events

Of course the implementation of this involves a lot of hazzle. e.g.: 

  * complete implementation of TThreadEvent and "event queues" in the RTL including a way to stream parameters and function results
  * hooking the "event queue" of the main thread to the GUI message queue in case of a GUI project
  * when doing an indirect call to a procedure stored in a variable that has a **procedure ... of object** type, the compiler provides means to check whether the content is a sibling of TEventThread and if this is true, calls the RTL functions to insert the event into the event queue of the appropriate TEventThread.



### See also

  * [OpenMP support](<OpenMP_support.md> "OpenMP support")
  * [Proposal of a Widget Set independent Event Queue implementation](<Proposal_of_a_Widget_Set_independent_Event_Queue_implementation.md> "Proposal of a Widget Set independent Event Queue implementation")


  * Maybe this is already implemented using <http://code.google.com/p/pascal-actor-model/>

---

_Source: [https://wiki.freepascal.org/ThreadEvent](https://web.archive.org/web/20240226045254/https://wiki.freepascal.org/ThreadEvent)_
