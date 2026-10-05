# macOS App Nap

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │    
****

## Contents

  * 1 Overview
    * 1.1 Timer coalescing
  * 2 Managing App Nap
    * 2.1 Automatic termination
  * 3 NSActivityOptions
    * 3.1 Recommended usage
  * 4 Example code
  * 5 App Nap status
    * 5.1 Method 1: Activity Monitor
    * 5.2 Method 2: pmset
  * 6 Enhancing App Nap
    * 6.1 Implement active application transition delegate methods
      * 6.1.1 Example code
    * 6.2 Register for application-level occlusion notifications
      * 6.2.1 Example code
  * 7 See also
  * 8 External Links



## Overview

macOS 10.9 (Mavericks) introduced substantial power-saving features under the App Nap umbrella, especially for laptops. App Nap is based on several key principles: 

  * The features work without the developer needing to modify existing applications.
  * The features keep the hardware as idle as possible given the demand for resources.
  * When a Mac is on battery power, only the work the user requests or that is absolutely essential is done.



App Nap is activated by the operating system when: 

  * The application's windows are not visible; eg minimised to the dock or hidden by another application's window(s).
  * The application has not updated any visible part of an open window for some time.
  * The application is not playing audio.
  * The application is not using OpenGL graphics.
  * The application is in the background.
  * The application has not informed macOS that it is still active via IOKit power management or NSProcessInfo assertions.



The App Nap power-saving measures include: 

  * **Timer throttling** : The frequency with which an application's timers are executed is reduced, increasing CPU idle time when running applications that, for example, check for data.


  * **I/O throttling** : Disk and network activity is assigned the lowest priority for applications that are "napping" thereby reducing the speed at which the application can read/write data from/to a device. This also reduces the likelihood that a napping application will impact an application which is actively being used.


  * **Priority reduction** : The UNIX process priority of an application is reduced so that it receives less available CPU time.



### Timer coalescing

Timer coalescing, although not an App Nap feature, was introduced in Mavericks at the same time. To maximise the amount of time that the CPU spends at idle, timer coalescing shifts the execution of timers by a small amount so that timers of multiple applications are executed at the same time. This is done by applying a time window to every timer based on the importance of the process. A timer can be executed at any time during this window, so may be shifted backward or forward a small amount so that it lines up with other timers that need to be executed at similar times. The timer windows are: 

Process type | Timer window   
---|---  
Application (default) | 1ms   
System daemon | 70-90ms   
Background process | 80-120ms   
Critical/Real time process | 0ms   
  
The NSTimer class in Mavericks introduced a new _tolerance_ parameter which enables developers to tune how timely timer-event driven events need to be. 

## Managing App Nap

App Nap introduced a new API in _NSProcessInfo_ which gives developers the ability to inform the operating system when an application is performing a long-running operation that may need to prevent App Nap or system sleep. 
    
    
    NSProcessInfoActivity = objccategory external (NSProcessInfo)
        function beginActivityWithOptions_reason (options: NSActivityOptions; reason: NSString): NSObject; message 'beginActivityWithOptions:reason:';
        procedure endActivity (activity: NSObject); message 'endActivity:';
        procedure performActivityWithOptions_reason_usingBlock (options: NSActivityOptions; reason: NSString; block: OpaqueCBlock); message 'performActivityWithOptions:reason:usingBlock:';
    

Be aware that failing to execute _endActivity_ after a _beginActivityWithOptions_ for an extended period of time can have significant negative impacts on the performance of your user's computer, so only use the minimum amount of time required. 

### Automatic termination

Automatic termination is a feature introduced in macOS 10.7 (Lion) that you must explicitly code for in your application. If it is enabled, then your application needs to save the current state of its user interface so that it can be restored later . The system can kill the underlying process for an auto-terminable application at any time, so saving this information maintains application continuity. The system usually kills an application’s underlying process some time after the user has closed all of its windows. However, the system may also kill an application with open windows if it is not currently visible on the screen. 

To support automatic termination, you need to: 

  * Declare support for automatic termination either programmatically or setting the _NSSupportsAutomaticTermination_ key value to YES in the application's `Info.plist` file.
  * Support saving and restoring the application's window configurations.
  * Save the user’s data at appropriate times. 
    * Single-window, library-style applications should implement strategies for saving data at appropriate checkpoints.
    * Multiwindow, document-based applications can use the autosaving and saveless documents capabilities in _NSDocument_.



You can also use the App Nap API to control automatic termination. 
    
    
    function beginActivityWithOptions_reason (options: NSActivityOptions; reason: NSString): NSObjectProtocol; message 'beginActivityWithOptions:reason:';
    // Perform some task
    procedure endActivity (activity: NSObjectProtocol); message 'endActivity:';
    

is equivalent to: 
    
    
    procedure disbleAutomaticTermination (reason: NSString);
    // Perform some task
    procedure enableAutomaticTermination (reason: NSString);
    

## NSActivityOptions

Option | Effect   
---|---  
NSActivityIdleDisplaySleepDisabled | require the screen to stay powered on   
NSActivityIdleSystemSleepDisabled | prevent idle sleep   
NSActivitySuddenTerminationDisabled | prevent sudden termination   
NSActivityAutomaticTerminationDisabled | prevent automatic termination   
NSActivityUserInitiated | indicate the application is performing a user-requested action   
NSActivityUserInitiatedAllowingIdleSystemSleep | indicate the application is performing a user-requested action, but that the system can sleep on idle   
NSActivityBackground | indicate the application has initiated some kind of work, but not as the direct result of user request   
NSActivityLatencyCritical | indicate the activity requires the highest amount of timer and I/O precision available   
  
### Recommended usage

**NSActivityUserInitiated** : Used for finite length activities that the user has explicitly started. Examples include exporting or downloading a user specified file. 

**NSActivityBackground** : Used for finite length activities that are part of the normal operation of your application but are not explicitly started by the user. Examples include autosaving, indexing, and automatic downloading of files. 

**NSActivityLatencyCritical** : If your application requires high priority I/O, you can include the flag (using a bitwise OR). You should only use this flag for activities like audio or video recording that really do require high priority. 

## Example code
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Dialogs, StdCtrls, ExtCtrls, CocoaAll;
    
    { NSProcessInfo }
    
    type
      NSProcessInfoActivity = objccategory external (NSProcessInfo)
        function beginActivityWithOptions_reason (options: NSActivityOptions; reason: NSString): NSObjectProtocol; message 'beginActivityWithOptions:reason:'; { available in 10_9, 7_0 }
        procedure endActivity (activity: NSObjectProtocol); message 'endActivity:'; { available in 10_9, 7_0 }
        procedure performActivityWithOptions_reason_usingBlock (options: NSActivityOptions; reason: NSString; block: OpaqueCBlock); message 'performActivityWithOptions:reason:usingBlock:'; { available in 10_9, 7_0 }
      end;
    
      { TForm1 }
    
      TForm1 = Class(TForm)
        Button1: TButton;
        Button2: TButton;
        Button3: TButton;
        Memo1: TMemo;
        procedure Button1Click(Sender: TObject);
        procedure Button2Click(Sender: TObject);
        procedure Button3Click(Sender: TObject);
      private
      public
      end;
    
    var
      Form1: TForm1;
      myActivityToken: NSObjectProtocol;
    
    implementation
    
    {$R *.lfm}
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      Form1.Memo1.Append('beginActivity');
    
      myActivityToken := NSProcessInfo.processInfo.beginActivityWithOptions_reason((NSActivityIdleSystemSleepDisabled or NSActivityUserInitiated or NSActivityAutomaticTerminationDisabled), NSSTR('No napping!'));
    
      if(myActivityToken = Nil) then
        begin
          Form1.Memo1.Append('No Token!');
        end;
    
      myActivityToken.retain;
    end;
    
    procedure TForm1.Button2Click(Sender: TObject);
    begin
      Form1.Memo1.Append('endActivity!');
    
      if (myActivityToken <> Nil) then
        begin
          NSProcessinfo.processinfo.endActivity(myActivityToken);
          myActivityToken.release;
          myActivityToken := Nil;
        end;
    end;
    
    procedure TForm1.Button3Click(Sender: TObject);
    begin
      Close;
    end;
    
    end.
    

## App Nap status

There are two ways of checking the App Nap status of your application. 

### Method 1: Activity Monitor

Open Applications > Utilities > Activity Monitor which shows the status of App Nap for all running applications as well as other useful information like the energy impact of each application on the system: 

[![Activity Monitor app Nap.jpg](https://wiki.freepascal.org/images/f/f9/Activity_Monitor_app_Nap.jpg)](</File:Activity_Monitor_app_Nap.jpg>)

### Method 2: pmset

Open an Applications > Utilities > Terminal and type the command: 
    
    
    pmset -g assertionslog
    

which will produce the following output when you run the example code above and click the _BeginActivity_ and _EndActivity_ buttons. 
    
    
    Time             Action      Type                          PID(Causing PID)    ID                  Name                                              
    ====             ======      ====                          ================    ==                  ====                                              
    02/13 16:35:19   Created     PreventUserIdleSystemSleep    1188                0x40e40001880f      No napping!                                       
    02/13 16:35:19   System wide status: PreventUserIdleSystemSleep: 1  
    02/13 16:35:29   Released    PreventUserIdleSystemSleep    1188                0x40e40001880f      No napping!                                       
    02/13 16:35:29   System wide status: PreventUserIdleSystemSleep: 0
    

## Enhancing App Nap

While App Nap operates automatically, with very little effort you can enhance its capabilities by actively reducing your application's energy footprint and further lengthening the time users can use their battery-powered devices without needing to plug them into the power. 

Your application knows best about the importance of its activity and should not rely on App Nap to put it into an idle state. The most effective way to enhance App Nap is for your application to listen for notifications that it’s no longer in active use and to reduce or suspend energy-intensive work as quickly as possible which can happen well before the system triggers App Nap automatically. 

### Implement active application transition delegate methods

Implement NSApplicationDelegate methods in your application delegate to receive calls when the active state of your application — whether it’s in the foreground or not — changes. 

NSApplicationDelegate methods  Method | When sent by the default notification center | Significance   
---|---|---  
applicationWillResignActive | Immediately before the application is deactivated and no longer the foreground (frontmost) application. | Start winding down activity that can be stopped entirely once the change in state has completed.   
applicationDidResignActive | Immediately after the application is deactivated and gives up its position as the foreground application. | Stop any power-intensive operations, animations, and UI updates as much as possible.   
applicationWillBecomeActive | Immediately before the application becomes active and comes to the front. | Start resuming operations.   
applicationDidBecomeActive | Immediately after the application becomes active. | Fully resume operations that were reduced or halted.   
  
#### Example code

In the example code below the relevant delegate methods simply log their occurrence. In a real application you would need to replace them with the desired behaviours to reduce or suspend relevant activities and restart activities as required. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Forms, StdCtrls, CocoaAll;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
        procedure FormClose(Sender: TObject; var CloseAction: TCloseAction);
        procedure FormCreate(Sender: TObject);
      private
    
      public
    
      end;
    
      {TMyAppDelegate}
    
      TMyAppDelegate = objcclass(NSObject)
        private
    
        public
          procedure applicationWillBecomeActive (aNotification: NSNotification);
            message 'applicationWillBecomeActive:';
          procedure applicationDidBecomeActive (aNotification: NSNotification);
            message 'applicationDidBecomeActive:';
          procedure applicationWillResignActive (aNotification: NSNotification);
            message 'applicationWillResignActive:';
          procedure applicationDidResignActive (aNotification: NSNotification);
            message 'applicationDidResignActive:';
        end;
    
    var
      Form1: TForm1;
      myAppDelegate: TMyAppDelegate;
    
    implementation
    
    {$R *.lfm}
    
    {NSApplication Delegates}
    
    procedure TMyAppDelegate.applicationWillBecomeActive(aNotification: NSNotification);
    begin
      NSLog(NSStr('will become active'));
    end;
    
    procedure TMyAppDelegate.applicationDidBecomeActive(aNotification: NSNotification);
    begin
      NSLog(NSStr('did become active'));
    end;
    
    procedure TMyAppDelegate.applicationWillResignActive (aNotification: NSNotification);
    begin
      NSLog(NSStr('will resign active'));
    end;
    procedure TMyAppDelegate.applicationDidResignActive (aNotification: NSNotification);
    begin
      NSLog(NSStr('did resign active'));
    end;
    
    {Button Events}
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      NSLog(NSStr('Closing'));
      Close;
    end;
    
    {Form Events}
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      // NSApp
      // - The global variable for the shared application instance.
      // NSApplication.sharedApplication
      // - Returns the application instance, creating it if it doesn’t exist yet.
      NSApp := NSApplication.sharedApplication;
      NSApp.setDelegate(myAppDelegate);
    end;
    
    procedure TForm1.FormClose(Sender: TObject; var CloseAction: TCloseAction);
    begin
      if(NSApp.isActive) then
        begin
           NSLog(NSStr('Exiting'));
           exit;
        end
      else
        // needed if any part of form is occluded or does not quit
        // until dock icon clicked when quitting via dock menu
        begin
          NSLog(NSStr('Closing2'));
          Application.BringToFront;
          CloseAction := TCloseAction.caFree;
        end;
    end;
    
    Initialization
      NSLog(NSStr('initialization'));
      myAppDelegate := TMyAppDelegate.alloc.init;
    
    Finalization
      NSLog(NSStr('finalization'));
      myAppDelegate.Release; 
    end.
    

Typical log output when the executable is started from an Applications > Utilities > Terminal: 
    
    
    trev@macmini7 [/Users/trev/Programming/LAZARUS/laz_appnap_ex1] $ ./project1.app/Contents/MacOS/project1
    2021-03-01 17:57:40.293 project1[1695:130876] initialization
    2021-03-01 17:57:40.378 project1[1695:130876] will become active
    2021-03-01 17:57:40.379 project1[1695:130876] did become active
    2021-03-01 17:57:43.206 project1[1695:130876] will resign active
    2021-03-01 17:57:43.207 project1[1695:130876] did resign active
    2021-03-01 17:57:47.184 project1[1695:130876] Closing2
    2021-03-01 17:57:47.188 project1[1695:130876] will become active
    2021-03-01 17:57:47.189 project1[1695:130876] did become active
    2021-03-01 17:57:47.190 project1[1695:130876] finalization
    

Alternatively, you can view the output using the Applications > Utilities > Console. 

### Register for application-level occlusion notifications

As soon as your application or any of its windows becomes hidden, your application should immediately halt any work that is no longer useful. When it becomes visible again, it can immediately refresh its content and resume any power-intensive operations. 

Your application is considered hidden from the user when: 

  * Windows of other applications occlude the windows of your application.
  * The screen saver is on and occluding all applications’ windows.
  * Your application is in a Mission Control space where the user is not working.



_applicationDidChangeOcclusionState_ tells the delegate that the application’s occlusion state changed. 

On receiving this notification, you can query the application's occlusion state. Note that this only notifies about changes in the state of the occlusion, not when the occlusion region changes. You can use this notification to increase responsiveness and save power by halting any expensive calculations that the user cannot see. 

If the _NSApplicationOcclusionStateVisible_ global constant variable is set, it means that at least part of a window owned by your application is visible, so your application should continue working as normal. If the variable is not set, it means that no part of any window owned by your application is visible and your application should reduce or halt its operations and calculations because the user will not be able to see the results. 

#### Example code

In the example code below the relevant delegate method simply logs its occurrence. In a real application you would need to replace it with the desired behaviour to reduce or suspend relevant activities and restart activities as required. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Forms, StdCtrls, CocoaAll;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
        procedure FormClose(Sender: TObject; var CloseAction: TCloseAction);
        procedure FormCreate(Sender: TObject);
      private
    
      public
    
      end;
    
      {TMyAppDelegate}
    
      TMyAppDelegate = objcclass(NSObject)
        private
    
        public
          procedure applicationDidChangeOcclusionState (aNotification: NSNotification);
            message 'applicationDidChangeOcclusionState:';
        end;
    
    var
      Form1: TForm1;
      myAppDelegate: TMyAppDelegate;
    
    implementation
    
    {$R *.lfm}
    
    {NSApplication Delegates}
    
    procedure TMyAppDelegate.applicationDidChangeOcclusionState(aNotification: NSNotification);
    begin
      NSLog(NSStr('did change occlusion state'));
    
      // if(NSApp.occlusionState AND NSApplicationOcclusionStateVisible) = 1 << 1 then // OR
      if (NSApp.occlusionState AND NSApplicationOcclusionStateVisible) = %00000010 then
          NSLog(NSStr('state: visible'))
      else
          NSLog(NSStr('state: invisible'))
    end;
    
    {Button Events}
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      NSLog(NSStr('Closing'));
      Close;
    end;
    
    {Form Events}
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      // NSApp
      // - The global variable for the shared application instance.
      // NSApplication.sharedApplication
      // - Returns the application instance, creating it if it doesn’t exist yet.
      NSApp := NSApplication.sharedApplication;
      NSApp.setDelegate(myAppDelegate);
    end;
    
    procedure TForm1.FormClose(Sender: TObject; var CloseAction: TCloseAction);
    begin
      if(NSApp.isActive) then
        begin
           NSLog(NSStr('Exiting'));
           exit;
        end
      else
        // needed if any part of form is occluded or does not quit
        // until dock icon clicked when quitting via dock menu
        begin
          NSLog(NSStr('Closing2'));
          Application.BringToFront;
          CloseAction := TCloseAction.caFree;
        end;
    end;
    
    Initialization
      NSLog(NSStr('initialization'));
      myAppDelegate := TMyAppDelegate.alloc.init;
    
    Finalization
      NSLog(NSStr('finalization'));
      myAppDelegate.Release;
    end.
    

Typical log output when the executable is started from an Applications > Utilities > Terminal: 
    
    
    trev@macmini7 [/Users/trev/Programming/LAZARUS/laz_appnap_ex2] $ ./project1.app/Contents/MacOS/project1
    2021-03-01 18:25:51.348 project1[1894:139611] initialization
    2021-03-01 18:25:51.427 project1[1894:139611] did change occlusion state
    2021-03-01 18:25:51.427 project1[1894:139611] state: visible
    2021-03-01 18:25:55.842 project1[1894:139611] did change occlusion state
    2021-03-01 18:25:55.842 project1[1894:139611] state: invisible
    2021-03-01 18:26:00.552 project1[1894:139611] Closing2
    2021-03-01 18:26:00.558 project1[1894:139611] finalization
    

Alternatively, you can view the output using the Applications > Utilities > Console. 

## See also

  * [macOS Energy Efficiency Guide](<macOS_Energy_Efficiency_Guide.md> "macOS Energy Efficiency Guide")
  * [NSProcessInfo](<Accessing_macOS_System_Information.md> "Accessing macOS System Information") \- accessing system information.



## External Links

  * [Apple: Energy Efficiency Guide for Mac Apps](<https://developer.apple.com/library/archive/documentation/Performance/Conceptual/power_efficiency_guidelines_osx/index.html#//apple_ref/doc/uid/TP40013929-CH15-SW1>).
  * [Apple: NSActivityOptions](<https://developer.apple.com/documentation/foundation/nsactivityoptions>).
  * [Apple: NSProcessInfo](<https://developer.apple.com/documentation/foundation/nsprocessinfo#/apple_ref/occ/cl/NSProcessInfo>).
  * [Apple: NSApplicationDelegate](<https://developer.apple.com/documentation/appkit/nsapplicationdelegate>).
  * [Apple: NSApplicationDidChangeOcclusionStateNotification](<https://developer.apple.com/documentation/appkit/nsapplicationocclusionstate/nsapplicationocclusionstatevisible>).
  * [Apple: NSTimer](<https://developer.apple.com/documentation/foundation/nstimer>).

---

_Source: [https://wiki.freepascal.org/macOS_App_Nap](https://web.archive.org/web/20240304085431/https://wiki.freepascal.org/macOS_App_Nap)_
