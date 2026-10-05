# Cocoa Internals/Application

Cocoa's Application API pretty much matches what LCL provides. It could be possible to simply call NSApp apis, that correspond to LCL APIs. However, there's a major difference, that makes a simple solution not possible. LCL Application provides a methods **HandleMessages** and **ProcessMessagess** allowing a manual control over the event-loop. 

NSApp doesn't provide any corresponding counter parts, thus it's necessary to process events loops manually. 

## Contents

  * 1 Event Loop
    * 1.1 HiJacking (COCOALOOPHIJACK)
    * 1.2 Native Loop (COCOALOOPNATIVE)
    * 1.3 Manual Event Loop (COCOALOOPOVERRIDE)
    * 1.4 Sub-classing NSApplication
  * 2 Termination
  * 3 Sync (MainThreadWake), PostMessage and SendMessage
    * 3.1 Clogging with Dialogs
  * 4 Post Event Processing
    * 4.1 Clipboard
    * 4.2 ToCollect
    * 4.3 Focus Change
  * 5 System Wide Events
    * 5.1 End of User Session
    * 5.2 Application Active Status
    * 5.3 Application maximization
    * 5.4 Files Drop
    * 5.5 System Settings Changes
  * 6 See Also



## Event Loop

The technical decisions over TCocoaApplication implementation are described below. 

### HiJacking (COCOALOOPHIJACK)

macOS 10.15 beta is throwing a KVO inconsistency error. The issue <https://bugs.freepascal.org/view.php?id=35702>
    
    
    *** Terminating app due to uncaught exception 'NSInternalInconsistencyException', reason: 
    'Cannot update for observer <NSFrontmostDocumentWindowObserver 0x7fff8ec52208> for the key path "mainWindow.representedURL" 
    from <TCocoaApplication 0x100d089a0>, most likely because the value for the key "mainWindow" has changed without an appropriate 
    KVO notification being sent. Check the KVO-compliance of the TCocoaApplication class.'
    

Crash can occur on on startup (for our app) or also reported to happen when a two form app when opening second form as modal dialog. 

Error is caused because Cocoa currently doesn't run through base level NSApplication.run and thus appropriate ._isrunning flag is not set (notice that this also required the override of NSApplication.isRunning done for Mantis 32177) 

NSFrontmostDocumentWindowObserver registers for all changes to mainWindow.representedURL. During initialization of NSApplication (down inside Cocoa code) calls to willChangeValueForKey() and didChangevalueForKey() are made to '_mainWindow' or 'mainWindow' depending on state of _isRunning flag (not from calls to .isRunning but from the memory locaiton where it is stored) and NSapplication.isActive. Because we don't go through NSApplication.run the bit for .isRunning is never set and calls to willChangevalueForKey(), didChangevalueForKey() do not happen appropriately for 'mainWindow'. 

The crash occurs when _handleApplicationActivated calls removeObserverForKeyPath() on an observer object that is supposed to be set up for 'mainWindow' but it was never actually set up i.e. addObserverForKeyPath() was never called. Documentation for removeObserverForKeyPath() indicates that removing an object that hasn't been set up with addObserverForKeyPath() is an error condition. That is the condition we are hitting. 

Suggested fix (patch attached) is to run through NSApplication.run first and then jump into our aloop(). 

Accomplished by removing TCocoaApplication.isRunning and TCocoaApplication.run methods thus allowing NSApplication.run to be called and set up NSRunloop and make first call to nextEventMatching:unitDate, etc. On first pass through nextEventMatching. (as caught by isRun = false) we grab control and call aloop. 

### Native Loop (COCOALOOPNATIVE)

The approach doesn't interfere with Cocoa loop processing. The approach also DOES NOT use LCL event processing loop (as LCL event loop blocks Cocoa loop from running). Instead approach is catching FPC exceptions and handles them through LCL routines. Also end of event processing should cause Idle event to be performed. That lets Cocoa loop to run without interruptions. 

**Downside**

  * Since **Application.RunLoop** is not used, every time it's updated, CocoaWS side needs to be updated also. It's possible however, to Modify LCL to make the task easier. (which might look like a ultimate solution).



**todo:**

  * ~~test application termination with Modal loops involved. (**stop:** needs to be called for EVERY modal loop started)~~ passed
  * ~~check exception catching for "sync" methods. (as those are not really events)~~ passed



### Manual Event Loop (COCOALOOPOVERRIDE)

    **WARNING** the method described below is no longer used.

Cocoa Widgetset implements even loops manually. Apple documentation advises that it's possible, though could be complicated. 

The actual event processing code could be found at AppWaitMessage and AppProcessMessage. 
    
    
    procedure TCocoaWidgetSet.AppWaitMessage;
    var
      event : NSEvent;
      pool:NSAutoReleasePool;
    begin
      pool := NSAutoreleasePool.alloc.init;
      event := NSApp.nextEventMatchingMask_untilDate_inMode_dequeue(NSAnyEventMask, NSDate.distantFuture, NSDefaultRunLoopMode, true);
      if event <> nil then
      begin
        if (event.type_ = NSApplicationDefined) and (event.subtype = LCLEventSubTypeMessage) and (event.data1 = LM_NULL) and (event.data2 = AppHandle) then
          CheckSynchronize
        else
          NSApp.sendEvent(event);
        NSApp.updateWindows;
      end;
      pool.release;
    end;
    

### Sub-classing NSApplication

    **WARNING** the method described below is no longer used.

With a manual event loop implementation, there's another problem showed up. 

NSWindows are behaving differently depending if NSApplication has started or not. (running property returning TRUE or FALSE). The Apple documentation doesn't mention that specifically, except for the following note: 

    

    **Important**
    Many AppKit classes rely on the NSApplication class and may not work properly until this class is fully initialized.
    As a result, you should not, for example, attempt to invoke methods of other AppKit classes from an initialization method of an NSApplication subclass.

If NSApplication is not running, then NSWindows would not pass the focus to another Window with the application. Instead they would keep the focus (if hidden) or make no window a focused window (if closed). (see [#32177](<https://bugs.freepascal.org/view.php?id=32177>)) 

There are two options to make NSApplication [running](<https://developer.apple.com/documentation/appkit/nsapplication/1428759-running?language=objc>) flag to set to true: 

  * use NSApp default [run](<https://developer.apple.com/documentation/appkit/nsapplication/1428631-run?language=objc>) method (which could not be used, due to the need of manual event processing)
  * subclass NSApplication, override "isRunning" (getter of "running" property)



The sub-classing option is used, introducing TCocoaApplication class. It overrides two methods: 

  * **run** \- the code is actually LCL Loop procedure, which would process ObjC events loop manually
  * **isRunning** \- returns true, if **run** method was actually called.



## Termination

Normally, **NSApplication.run:** method never returns after being called. 

NSApplication provides a special **terminate:** method to terminate Cocoa App. The method terminates the running process (allowing **run:** never to return). It should **NEVER** be used for FPC environment, as FreePascal runtime should call unit finalization. 

The way to make **run:** method to return is use **stop:** method. Which **stops** _the current_ event loop, and should be called multiple times with multiple event loops (such as modal) are used. 

## Sync (MainThreadWake), PostMessage and SendMessage

Originally the syncing with the main thread, as well as Post and SendMessage implementations were based on creating a custom **NSApplicationDefined** event with sub type set to **LCLEventSubTypeMessage**

Posting the custom event into an event loop on Cocoa might have a certain undesired consequences. The major problem is with event processing for tracking mouse actions. Unlike WinAPI, where it is expected for to process standalone events, Cocoa developer should actually run a while loop. The loop should catch mouse events and stop as soon as mouse is released. 

Each tracking loop however, is written differently. And some places reacting on "unexpected" events differently. 

Trying to hide "unexpected" events from a loop might work for one implementation, but not the other. 

The only solution that satisfies every loop is NOT to post any custom events at all. 

The implementation was changed from using a custom event to **performSelectorOnMainThread** method. 

For syncing with the main thread there's **lclSyncCheck:** method introduced for **TCocoaApplication**. 

For post and send events, there's a message **TLCLEventMessage** class, which **performSelectorOnMainThread:** method is called. The class provides fields to store WParam, LParam and Result (for SendMessage). 

### Clogging with Dialogs

There's an issue with a clogged sync processing. If there are two messages sent to the main thread, where both are causing a modal window to be shown. (i.e. an error message). Then if the second one arrives, while the first one is still showing the modal window, the application terminates. 

## Post Event Processing

There a number of routines performed after processing each event. 

### Clipboard

In Cocoa, clipboard is named Pasteboard (class NSPasteboard); 

The global (system-wide) pasteboard is available via NSPasteboard.generalPasteboard method. Primary and Secondary selection boards are just unique named NSPasteboards allocated at start. 

The whole implementation of API is in TCocoaWidgetSet class. (todo: could/should be separated) 

The older (10.5 and earlier) APIs are used for operations. (starting with 10.6 Apple suggests using the new API, but the old one has not been deprecated so far) 

There's not NSPasteboard change notification in place (unlike Windows), thus the only way to track, if the content got changed (in LCL/Windows terms - clipboard ownership was lost) is through checking "changeCount" property. The property is checked on **EVERY** event processed (which might impact efficiency). The only way to get rid of that is to rewrite Clipboard LCL object. (instead of relying on a notification - it should check for the change at "Get" data calls) 

### ToCollect

On certain conditions a control can destroy itself while processing its own event. (For example an Edit field of a "authentication" dialog. If enter is pressed, the OnKeyDown event to close the form, causing its all children controls, including the edit field itself). 

If such things occur, the **callback** object (linking NSObject and LCL Target) can be released (in DestoryHandle call). However, the **callback** object must not be freed, as Key processing routine (the one that started OnKeyDown) is not yet over. The same concept applies for other processing routines (i.e. Mouse handling, etc). 

In order to prevent crashed due to access of freed object, the destruction of Callbacks objects is delayed until the end of event processing. Cocoa widgetset interface provides following method **AddToCollect** to add an object to be released after the event processing. 

Keep in mind that events handling might be recursive, thus a destruction of an object only occurs when the processing of the event that added object to the collection is finished. 

### Focus Change

The change if control focus is made post event basis to prevent endless loops of focus-fight. 

Focus-fight occurs, a focus change notification is sent at the time when the focus is actually switched. It's typical for Cocoa controls to temporary switch focus to Window. (in Cocoa world, Window is used a "back-up" focus target. As it always allows to be focused... even with canBecomeKeyWindow false?). If LCL is notified by Windows getting the focus, it would attempt to switch the focus to one of its child control. This would typically be the same control as it's now. Eventually Cocoa would try to switch the focus back to Window, causing the recursive focus-fight with application responsiveness and eventual crash. 

To avoid that, the focus switch is verified after event processing. 

## System Wide Events

There's a number of system-wide events that are reported via Application object. 

### End of User Session

Usually triggered by a user logging out or the entire system shutdown. 

Supported since <https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/34798>

  * **IntfQueryEndSession**(var Cancel: Boolean);
  * **IntfEndSession** ;



### Application Active Status

  * **IntfAppActivate**(const Async: Boolean = False); - the event when application gets "active" by gaining the keyboard focus.



    

    supported by processing of **applicationDidBecomeActive** message

  * **IntfAppDeactivate**(const Async: Boolean = False); - the even when application gets "deactivated" by losing keybaord focus.



    

    supported by processing of **applicationDidResignActive** message

### Application maximization

  * **IntfAppMinimize** ;
  * **IntfAppRestore** ;



### Files Drop

Issued by the system file exchange (drag/drop mechanism). Files can be dropped on the Application icon (shown in the system Dock) 

  * **IntfDropFiles**(const FileNames: Array of String);



    

    supported by processing **application:openFiles** files messages.
    the list of files is also passed to the main form of the application (similar IntfDropFiles event), by TApplication method is called first.

### System Settings Changes

  * **IntfSettingsChange**



    

    not supported

  * **IntfThemeOptionChange**



    

    not supported (need to be supported for Light/Dark theme changes?)

## See Also

  * [Cocoa Internals](<../Cocoa_Internals.md> "Cocoa Internals")
  * [NSApplication](<https://developer.apple.com/documentation/appkit/nsapplication?language=objc>) \- apple reference documentation for NSApplication class.

---

_Source: [https://wiki.freepascal.org/Cocoa_Internals/Clipboard](https://web.archive.org/web/20241212100011/https://wiki.freepascal.org/Cocoa_Internals/Clipboard)_
