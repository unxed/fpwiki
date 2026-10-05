# Main Loop Hooks

│ **English (en)** │

## Contents

  * 1 Problem statement
  * 2 Solution details
    * 2.1 Windows
    * 2.2 Gtk/Unix
    * 2.3 Qt4
    * 2.4 Flags
    * 2.5 Pipes and Process Termination
  * 3 See also



## Problem statement

You need to wait for some event (on socket or pipe or ...) to happen, but you want to do this in the main (GUI) thread and do not want to block the GUI and also you do not want multi-threading. Solution to this problem is the ability to add extra "handles" to be watched in the main event loop. 

## Solution details

In unit [LCLIntf](<http://lazarus-ccr.sourceforge.net/docs/lcl/lclintf> "doc:lcl/lclintf") three functions have been added to provide for this functionality, they are: 
    
    
    AddEventHandler(AHandle: THandle; AFlags: dword; 
        AEventHandler: TWaitHandleEvent; AData: PtrInt): PEventHandler;
    RemoveEventHandler(var AHandler: PEventHandler);
    SetEventHandlerFlags(AHandler: PEventHandler; NewFlags: dword);
    

The [ TWaitHandleEvent](<http://lazarus-ccr.sourceforge.net/docs/lcl/interfacebase/twaithandleevent.html> "doc:lcl/interfacebase/twaithandleevent.html") type is declared in unit InterfaceBase as: 
    
    
    TWaitHandleEvent = procedure(AData: PtrInt; AFlags: dword) of object;
    

[ AddEventHandler](<http://lazarus-ccr.sourceforge.net/docs/lcl/lclintf/addeventhandler.html> "doc:lcl/lclintf/addeventhandler.html") adds a handle to be watched to the main event loop. When the handle is "signalled", then the procedure specified in ACallback will be called with as a parameter the AData that was passed when registering the event handler, and any Flags, which are OS specific. The flags passed to AddEventHandler can be modified using SetEventHandlerFlags. The returned handler (a pointer) by AddEventHandler should be passed to RemoveEventHandler to stop watching the associated handle. Main reason for the flags is to provide a way to turn on and off various event types which you want watched (read/write/error). 

[ RemoveEventHandler](<http://lazarus-ccr.sourceforge.net/docs/lcl/lclintf/removeeventhandler.html> "doc:lcl/lclintf/removeeventhandler.html") stops watching the specified handle. RemoveEventHandler will set AHandler to nil before returning. Its argument is a pointer returned by AddEventHandler. 

[ SetEventHandlerFlags](<http://lazarus-ccr.sourceforge.net/docs/lcl/lclintf/seteventhandlerflags.html> "doc:lcl/lclintf/seteventhandlerflags.html") changes the flags on an active EventHandler. Currently this does nothing on windows (see below), but it is vital on other platforms to provide ways on turning event types OFF and ON for being watched. 

### Windows

The AFlags parameter in AddEventHandler is unused, and the AFlags in TWaitHandleEvent will be 0, in the current implementation. 

Win32 supports the following handle types, basically the things supported by MsgWaitForMultipleObjects: 

  * change notifications (for files and directories)
  * console input
  * events: signalled with SetEvent winapi function
  * mutexes: signalled when it is not owned anymore
  * processes: signalled when they terminate
  * semaphore: signalled when it's _count_ is greater than zero
  * threads: signalled when they terminate
  * timers: signalled when expired, see SetWaitableTimer



### Gtk/Unix

The AFlags parameter in AddEventHandler specifies the condition in which the handle should be signalled, with the possible values being the ones documented in the GIOCondition type in the [glib](<http://developer.gimp.org/api/2.0/glib/glib-IO-Channels.html#GIOCondition>) reference. 

In the callback, the AFlags will contain the condition, of the above referenced GIOCondition type, that was satisfied. 

Gtk/unix supports the following handle types: 

  * files
  * sockets
  * pipes



Note that win32 does not support sockets or pipes (at least, their use is "discouraged" by MSDN). Suggestion: use WSAEventSelect to create an event associated with a socket and pass the event handle. 

### Qt4

The Qt4 interface is internally different from Gtk and Win32, but has been emulated within LCL as close as possible to the GTK logic. Even the flags passed have been made compatible (GIO_IN, GIO_OUT and GIO_ERR). The only actual difference for the user is that should 2+ event types happen on the same socket "at once" the callback will not be called with an OR-ed version of the flags, but instead called twice in a row. This is a Qt4 limitation and the possibility of calling EXCEPTION event before a READ or WRITE event is to my knowledge unavoidable. The Qt4 interface was added in Lazarus v0.9.29 trunk (so it should appear in 0.9.30 stable). 

### Flags

Currently the FLAGS parameters are widgetset specific. On windows, this parameter is completely unused (see above), but on Gtk, Qt and others it is used to set what type of events to watch. 

### Pipes and Process Termination

To provide a cross-platform solution for pipes (not well supported on win32) and processes (not well supported on gtk/unix) additional functions have been added: 
    
    
    TChildExitReason = (cerExit, cerSignal);
    TPipeReason = (prDataAvailable, prBroken, prCanWrite);
    TPipeReasons = set of TPipeReason;
     
    TChildExitEvent = procedure(AData: PtrInt; AReason: TChildExitReason; AInfo: dword) of object;
    TPipeEvent = procedure(AData: PtrInt; AReasons: TPipeReasons) of object;
    
    function AddProcessEventHandler(AHandle: THandle; 
      AEventHandler: TChildExitEvent; AData: PtrInt): PProcessEventHandler;
    procedure RemoveProcessEventHandler(var AHandler: PProcessEventHandler);
     
    function AddPipeEventHandler(AHandle: THandle; 
      AEventHandler: TPipeEvent; AData: PtrInt): PPipeEventHandler;
    procedure RemovePipeEventHandler(var AHandler: PPipeEventHandler);
    

When a process terminates the event handler specified will be called. AInfo will contain the exit code if AReason is cerExit, or (on unix only) the termination signal if AReason is cerSignal. For gtk/unix, use the PID to watch as AHandle. Internally, a signal handler is installed to catch the SIGCHLD signal. On win32, AddEventHandler is used to watch process termination. 

To watch a pipe for incoming data, pass the file descriptor (unix) or the pipe read-end handle to AddPipeEventHandler with a custom chosen event handler method as AEventHandler. On gtk/unix, AddEventHandler is called to watch the file descriptor for traffic. On win32, the message loop will timeout every 100 msecs to check all watched pipes for available data; if data is available, the event handler is called. 

## See also

  * [Asynchronous Calls](<Asynchronous_Calls.md> "Asynchronous Calls")
  * [Multithreaded Application Tutorial](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial")

---

_Source: [https://wiki.freepascal.org/Main_Loop_Hooks](https://web.archive.org/web/20241206065430/https://wiki.freepascal.org/Main_Loop_Hooks)_
