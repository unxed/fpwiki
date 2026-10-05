# Yet another Running Timer

## Contents

  * 1 About
  * 2 Author
  * 3 Description
  * 4 Implementation
  * 5 Platforms
  * 6 Operations
  * 7 Downloads
  * 8 Test Program
  * 9 See also
  * 10 Discussion



### About

The first goal for this timer is to fire an event when a certain amount of time has elapsed. For most of the cases the already existing TTimer component will be enough. But it requires the LCL and when threads are involved, things may change. 

A second reason to implement a new timer is the way the TTimer processes properties when they change and specially when the Enabled property is set. Whenever any property changes, the current timer object is destroyed and re-created, which could be a time and resources consuming process. 

Another reason is to get rid of the multi thread limitation with the [TFPTimer](<http://lazarus-ccr.sourceforge.net/docs/fcl/fptimer/tfptimer.html> "doc:fcl/fptimer/tfptimer.html") used as a timer replacement where the LCL is not available (like in a service). The TFPTimer uses a synchronize method to fire the timer event, which limits its usage to the main thread only, as the TFPTimer uses an internal thread. 

For these reasons I decided to make a new timer control, based somewhat on the TFPTimer but without the threads limitation. 

As is, the number of active timers depends on the number of threads supported by the runtime platform. 

### Author

[Antonio Fortuny](</index.php?title=User:A.fortuny&action=edit&redlink=1> "User:A.fortuny \(page does not exist\)")

### Description

The timer itself is a TComponent descendant but inside there is a thread which actually does the job. The timer does not rely on any other component except the basic TComponent. 

Once created, the timer will continuously run a thread and count the elapsed time from the start by steps of 10 ms. As a matter of fact, the accuracy of the timer seems to be around a couple of ms within a range of a few seconds. This is why I do not recommend it for time critical operations unless the accuracy is acceptable. 

Another lack is that the timer relies on the GetTickCount system function. As far as I now, this function returns the number of elapsed ms from the last system boot. As the return value is a DWORD, the maximum value is a 32 bit integer. So the value will return to 0 after almost 48 days. This could lead to potential timing errors. 

One immediate improvement could be to break this 48 days barrier without adding new time critical penalties. 

### Implementation

The timer is implemented using a single unit. However, because IDE users like to use and drop non visual components as well, a package has been made to wrap the main unit and have a TSimpleTimer descendant which can be installed in the Lazarus IDE. 

The timer thread is driven by the means of an Enabled property and an event. When enabled, the event is set and the timer runs continuously. On each loop it waits for 10 ms. When entering the disabled state it waits 500 ms for the event to be set. This means that in disabled state, the timer is almost "invisible". 

This component was designed for cross-platform applications and was written specifically for the Lazarus IDE and Free Pascal Compiler and includes a demo application. 

### Platforms

The timer runs without problems on Win32, Win64, Linux x86_64 (OpenSuse 12, Ubuntu). It could maybe run on other platforms, testers are welcome. 

### Operations

The operations interface has deliberately been made as simple as possible: 

  * One time interval to be checked
  * One Active property to check whether the timer is active or not
  * An Enabled property to start (True) and stop (False) the timer
  * One event to be fired when the interval is exhausted
  * Of course a constructor and a destructor.



That's all. 

The point where the user should take care is the event which will be fired. Because the timer does not care about the context in which the event code has been written, there is a potential error situation from the application point of view: all the code inside the event will be executed in the context of the timer thread. This means that the user should be aware of multi-threading programming rules. 

About multi-threading you could read the Lazarus wiki page about the subject. "[http://wiki.freepascal.org/Multithreaded_Application_Tutorial](<Multithreaded_Application_Tutorial.md>)" 

It is not recommended to use the same timer event code for multiple timers for obvious reasons. Read the article above on how protect code against multi threading. 

### Downloads

Have a look at "<https://sourceforge.net/projects/controltimer/files/>" 

### Test Program

[![testprogram.jpg](https://wiki.freepascal.org/images/d/d0/testprogram.jpg)](</File:testprogram.jpg>)

This test program shows how to use the timers from the application main thread and from an independent thread launched by the GUI application. With this program you can test, in a GUI application: 

  * \- Start and stop a dropped TSimpleTimer
  * \- Start and stop up to 4 dynamically created timers, linked either to the main thread or to an independent thread
  * \- Use a fake window to receive and process messages using the application queue (Button3 on the form)



As you canread in the program code, you can mix front-end and threaded timers provided you check or not the "Do the same within a thread" check box before starting them. Before trying to stop timers check or uncheck the previous check box accordingly. The interval in the edit box on top will be taken to initialize the event firing interval. You can change it from time to time. The PostMessage OS function has been used to send messages to the main tread to avoid trouble with all treads being mixed. 

A second test program is available which demonstrates the timer usage in a console program. The number of timers has been fixed to 4, two of them run in the main program loop, the other two run inside an independent thread launched by the main process. 

Both test programs run into Win32 and Linux. 

### See also

  * [FPTimer included with FPC/Lazarus](<http://lazarus-ccr.sourceforge.net/docs/fcl/fptimer/index.html> "doc:fcl/fptimer/index.html")
  * [EpikTimer](<EpikTimer.md> "EpikTimer")



### Discussion

Hi Antonio your timer is cool. Saved me a lot of Time. I made a small change for better compatibility. I changed the eventtype in Ctimer.pas from TOnTimerEvent to TNotifyEvent cause its the same but buildin. --[Neptuntriton](</User:Neptuntriton> "User:Neptuntriton") 15:30, 13 August 2015 (CEST) 
    
    
      { TTimerThread }
    
      TTimerThread = class(TTHread)
      private
        FInterval     : Cardinal;
        FEnabled      : Boolean;
        FActive       : Boolean;
        FEnableEvent  : TEvent;
        FID           : Integer;
        //FOnTimer      : TOnTimerEvent;
        FOnTimer      : TNotifyEvent;
        {$IFDEF DEBUG}
        FTraceHandle  : THandle;
        {$ENDIF}

---

_Source: [https://wiki.freepascal.org/Yet_another_Running_Timer](https://web.archive.org/web/20250317104420/https://wiki.freepascal.org/Yet_another_Running_Timer)_
