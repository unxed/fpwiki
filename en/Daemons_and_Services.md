# Daemons and Services

│ **English (en)** │  **[español (es)](</Daemons_and_Services/es> "Daemons and Services/es")** │  **[français (fr)](</Daemons_and_Services/fr> "Daemons and Services/fr")** │  **[polski (pl)](</Daemons_and_Services/pl> "Daemons and Services/pl")** │  **[português (pt)](</Daemons_and_Services/pt> "Daemons and Services/pt")** │  **[русский (ru)](<../ru/Daemons_and_Services.md> "Daemons and Services/ru")** │    
****

## Contents

  * 1 What are daemons, services and agents?
  * 2 Creating a Service/Daemon (using Lazarus and visual Components)
    * 2.1 Prerequisite: Install the LazDaemon package
    * 2.2 Creating and using scaffolded Daemon Project Files
    * 2.3 Populating the DaemonMapper Class
    * 2.4 Writing the Daemon Methods
    * 2.5 Daemon Start/Stop Signal Handler
    * 2.6 Daemon Shutdown Handler
    * 2.7 Install and Uninstall for Linux (systemd)
    * 2.8 Daemon Worker Thread
    * 2.9 Initializing TDaemon Properties in Code
  * 3 Creating a Service/Daemon (in Code only)
  * 4 Daemon/Service Installation
    * 4.1 Windows
    * 4.2 Linux
  * 5 Reading the Log File
  * 6 Debugging a Daemon/Service
    * 6.1 Windows
    * 6.2 Linux
  * 7 Known problems
    * 7.1 Permission related Problems
    * 7.2 Exception EStreamError: "no streaming method available"
    * 7.3 Several "Stream Read" and "Unkown Properties" errors ...
    * 7.4 Lazarus IDE suggests to clear invalid properties from the .lpm file
    * 7.5 The Daemon won't start on Unix because of missing threading support
    * 7.6 Debugging Issues observed on Windows
    * 7.7 Debugging Issues observed on Linux
    * 7.8 Daemons have no Access to Network- and Cloud Drives (Onedrive, Dropbox)
    * 7.9 Specifying a Service User Account does not work on Linux
    * 7.10 System Codepage / UTF-8
  * 8 [Some historical contents - needs to be verified and integrated or removed]
    * 8.1 Linux (only for older Debian)
  * 9 See also



## What are daemons, services and agents?

Unix _daemons_ and Windows _services_ are system-wide programs running without user interaction; macOS _agents_ are per user programs (cf system-wide daemons) that may or may not run without user interaction. Although the nomenclature differs, their function is similar: for example **www** or **ftp** servers are called daemons under Linux and services under Windows. Because they do not interact with the user directly, they close their stdin, stdout, stderr descriptors at start. 

With Free Pascal and Lazarus it is possible to write these daemons/services in a platform-independent manner using the Lazarus lazdaemon package. To avoid name conflicts with the Delphi components these classes are called 'daemons'. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This page focuses on Windows and Linux. To read about Lazarus and macOS "Agents", please refer to [macOS daemons and agents](<macOS_daemons_and_agents.md> "macOS daemons and agents").

## Creating a Service/Daemon (using Lazarus and visual Components)

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The complete sample source code for this wiki page is available via the link at the end of this wiki page.

### Prerequisite: Install the LazDaemon package

Before you can start creating a daemon using the Lazarus IDE, you must install the LazDaemon package either via "Components" - "Configure installed packages" or by installing the lpk file directly from .../lazarus/components/daemon/lazdaemon.lpk. The package installs some new components and menu items in the IDE. 

[![2022-02-21 14 34 28-LINDER-LAZW7 - VMware Workstation.png](https://wiki.freepascal.org/images/3/31/2022-02-21_14_34_28-LINDER-LAZW7_-_VMware_Workstation.png)](</File:2022-02-21_14_34_28-LINDER-LAZW7_-_VMware_Workstation.png>)

Due to the way packages work in Lazarus, you need to "Save and rebuild the IDE" to install the LazDaemon package. 

### Creating and using scaffolded Daemon Project Files

After having installed the LazDaemon package, from the "Project" - "New Project" Menu, pick "Daemon (service) application)". 

[![2022-02-17 16 31 12-Create a new project.png](https://wiki.freepascal.org/images/e/ec/2022-02-17_16_31_12-Create_a_new_project.png)](</File:2022-02-17_16_31_12-Create_a_new_project.png>)

This will automatically create two units, one for a TDaemon descendant ("DaemonUnit"), and one for a TDaemonMapper descendant ("DaemonMapperUnit"), and a main project file ("TestDaemon.lpr"). This file needs a little change to work: 
    
    
    Program TestDaemon;
    
    Uses
    
    // This is the scaffolded code, needs modification like shown below,
    // since UseCThreads is usually not definied, and is not needed here
    // {$IFDEF UNIX}{$IFDEF UseCThreads}
    //  CThreads,
    // {$ENDIF}{$ENDIF}
    
    {$IFDEF UNIX}
      CThreads,
    {$ENDIF}
      DaemonApp, lazdaemonapp, daemonmapperunit, DaemonUnit
      { add your units here };
    
    {$R *.res}
    
    begin
      Application.Initialize;
      Application.Run;
    end.
    

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The scaffolded .lpr file contains an additional {$IFDEF UseCThreads} ... {$ENDIF} clause (commented out in the sample above), which has been observed not to work, unless additional measures are taken to define UseCThreads. You can remove the conditional entirely or comment it out, like shown in the above .lpr sample, or add a -dUseCThreads option when you compile your daemon on Linux. Windows services are not affected.

Despite that daemons don't have a GUI, both the DaemonApp and the DaemonMapper unit have support for the Lazarus Form Editor. This way you get a familiar GUI to populate the various properties of the daemon and the mapper class. Of course, if you prefer, you can initialize all properties in code as well like shown below. 

Both the scaffolded DaemonApp and the DaemonMapper unit contain a superfluous "var" definition (var DaemonMapper1: TDaemonMapper1; var Daemon1: TDaemon1;). Those are actually never used by the running daemon, they remain uninitialized, trying to access them results in an access violation fatal error. If you have a closer look at how daemon applications work internally you'll see that the code relies solely on the types (TDaemonMapper1, TDaemon), the vars can be ignored or removed entirely. 

The code provided by the TDaemon class does mainly respond to the various service control messages sent by the OS. The TDaemonMapper contains data structures describing the service. Both classes need to be registered with the daemon application framework, they will be used internally to configure the daemon when it starts. In this code sample registration is done in the "Initialization" section of each unit. 

The main "Application" object for a service-style application is introduced by putting a reference to DaemonApp into the uses section of each unit. In a daemon application, "Application" provides the complete service framework including advanced features like install and uninstall support, and logging. For the "worker" part of the daemon you are supposed to create a thread of its own, see sample code below, don't try to use the "Execute" method provided by the TDaemon class. 

### Populating the DaemonMapper Class

For the moment let's just fill some basic properties to get things going. Note that the DaemonClassName must exactly match with what you have defined in the DaemonClass unit. 

[![2022-01-31 23 32 52-Settings.png](https://wiki.freepascal.org/images/1/19/2022-01-31_23_32_52-Settings.png)](</File:2022-01-31_23_32_52-Settings.png>)

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** For cross-platform programmers: In Unix everything is case-sensitive, using lowercase is common use, so you may cause some confusion if you define a mixed-case service name like shown in the above sample.

Maybe have a short look into the WinBindings property, which lets you configure various service properties for use by the Windows Service Manager, like service start type and service user account. While not used for this basic demo daemon, setting those options properly is important for a real life daemon application. WinBindings doesn't have any effect on Linux, see the section about install and uninstall support for Linux on how to achieve the same for Linux by setting options in the systemd "unit" (.service) file. 

### Writing the Daemon Methods

TDaemons support the following methods: 

Method/Event | Description   
---|---  
`OnStart` | Called when daemon should start. This method must return immediately with _OK:=True_.   
`OnStop` | Called when daemon should stop. This method must return immediately with _OK:=True_.   
`OnShutDown` | Called when daemon should be killed, because the system shuts down. This method must stop the daemon immediately and return with _OK:=True_. This is not triggered under Linux. Linux simply kills the daemon process.   
`OnPause` | Called when daemon should pause. This method must return immediately with _OK:=True_. Under Linux this is not triggered, instead the kernel stops the whole daemon on STOP.   
`OnContinue` | Called when daemon should continue after a pause. This method must return immediately with _OK:=True_. Under Linux this is not triggered.   
`OnExecute` | Do not use this property to implement the working code, use a worker task like shown in the code sample.   
`BeforeInstall` | Called before service installation.   
`AfterInstall` | Called after (successful) service installation.   
`BeforeUninstall` | Called before service de-installation.   
`AfterUninstall` | Called after successful service de-installation.   
  
The following code snippets show all the major event handlers necessary to successfully implement a simple daemon. They have been tested on Windows 7/10/11 and several Linux distros (Debian 10.8 and Ubuntu 20.04.3). 

Some handy helper functions have been offloaded into separate units, so they do not obfuscate the daemon core code: 

  * FileLoggerUnit: a thread safe log to file helper. It is required because the control signal receiver included in the TDaemon application and the service's working code need to be implemented as separate threads, otherwise the daemon will not respond to control signals while the service worker code runs. So in any daemon there are at least two tasks involved, which could create a clash when accessing the log file simultaneously. See the Lazarus wiki [Multithreaded Application Tutorial](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial") for details about how to serialize accesses to a single resource by using a TRTLCriticalSection to make the code thread-safe. The FileLoggerUnit code does always write the log into the program directory, please make sure the daemon has write access permissions. The code writes one file per day, the filename contains the creation date. If you need a more sophisticated logger, consider using the [LazLogger](<LazLogger.md> "LazLogger") unit.
  * [DaemonWorkerThread](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/tree/main/examples/TDaemon>): a TThread descendant class to hold the daemon "worker" code. See the Lazarus wiki [Multithreaded Application Tutorial](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial") for details about the TThread.Execute method containing the inevitable "while not terminated" loop. There is nothing special with a worker thread initiated by a TDaemon, it's just a simple thread like any other.
  * [DaemonSystemdInstallerUnit](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/tree/main/examples/TDaemon>): a unit which tries to provide support for the -install and -uninstall command line parameters for Linux. Adds systemd/systemctl support by writing an appropriate control file to /lib/systemd/system.



### Daemon Start/Stop Signal Handler
    
    
    // ---------------------
    // Start and Stop signal
    // ---------------------  
    
    procedure TDaemon1.DataModuleStart(Sender: TCustomDaemon; var OK: Boolean);
    begin
      LogToFile(Format('Daemon received start signal, PID:%d', [GetProcessID]));
      // Create a suspended worker thread - see DaemonWorkerThread unit
      FDaemonWorkerThread := TDaemonWorkerThread.Create;
      // Parametrize it
      FDaemonWorkerThread.FreeOnTerminate := False;
      // Start the worker
      FDaemonWorkerThread.Start;
      OK := True;
    end;
    
    procedure TDaemon1.DataModuleStop(Sender: TCustomDaemon; var OK: Boolean);
    begin
      LogToFile('Daemon received stop signal');
      // stop and terminate the worker
      if assigned(FDaemonWorkerThread) then
      begin
        FDaemonWorkerThread.Terminate;
        // Wait for the thread to terminate.
        FDaemonWorkerThread.WaitFor;
        FreeAndNil(FDaemonWorkerThread);
      end;
      LogToFile('Daemon stopped');
      OK := True;
    end;
    

These handlers deal with the start and stop signals issued by the operating system. If the daemon starts, DataModuleStart fires and we spawn the "worker" thread found in the DaemonWorkerThread unit, see the TDeamonWorkerThread.Execute method to find the actual working code of our daemon. To understand the thread code, refer to the [Multithreaded Application Tutorial](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial") wiki page and to the documentation of TThread. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** To implement the daemon worker code, one might consider to implement a handler for the "execute" method found in TDaemon. This does not work well, especially not on WIndows. Execute does not run as a separate thread, while it executes the daemon stops handling control messages. The daemon will start and run fine, but if you try to stop/pause/resume it using the Windows Service Manager or the sc command, the daemon will not respond and appear to hang.

### Daemon Shutdown Handler

This Windows-only handler is called when the daemon is to be stopped because of an operating system shutdown. If we do not handle the event, Windows will simply forcefully kill the daemon process, potentially corrupting open files. For this simple demo service one can simply call the DataModuleStop event handler, executing the same code like a normal service stop. On Linux this handler is not supported, Linux will instead directly call the stop handler. 
    
    
    procedure TDaemon1.DataModuleShutDown(Sender: TCustomDaemon);
    // Supported on Windows systems only
    begin
      self.Stop;   // On shutdown, we trigger the stop handler. This will do nicely for this demo
      LogToFile('Daemon received shutdown signal');
    end;
    

### Install and Uninstall for Linux (systemd)

The built-in install and uninstall support (-install and -uninstall command line parameters) is implemented in LazDaemon applications for Windows only. Here is a simple code sample showing how to make custom handlers for install and uninstall events to add a similiar functionality for Linux. 
    
    
    // --------------------------------
    // Installation and De-Installation
    // --------------------------------
    
    procedure TDaemon1.DataModuleAfterInstall(Sender: TCustomDaemon);
    
      var
      isInstalled: boolean = True;
      FilePath: string;
    
    begin
      LogToFile('Daemon installing');
      {$IFDEF UNIX}
      FilePath := GetSystemdControlFilePath(Self.Definition.Name);
      isInstalled := CreateSystemdControlFile(self, FilePath);
      if not isInstalled then
        LogToFile('Error creating systemd control file: ' + FilePath);
      {$ENDIF}
      if isInstalled then
        LogToFile('Daemon installed');
    end;
    
    procedure TDaemon1.DataModuleBeforeUnInstall(Sender: TCustomDaemon);
      var
        isUnInstalled: boolean = True;
        FilePath: string;
    
      begin
        LogToFile('Daemon uninstalling');
        {$IFDEF UNIX}
        FilePath := GetSystemdControlFilePath(Self.Definition.Name);
        isUnInstalled := RemoveSystemdControlFile(FilePath);
        if not isUninstalled then
          LogToFile('Error removing systemd control file: ' + FilePath);
        {$ENDIF}
        if isUninstalled then
          LogToFile('Daemon uninstalled');
      end;
    

These Unix/Linux-only handlers utilize the routines in DaemonSystemdInstallerUnit to write a systemd control file (.service file) for our service to /lib/systemd/system, so the daemon can be controlled using the systemctl command. On uninstall, the .service file ist deleted. 

If you don't specify a user account in the .service file, systemd will run the daemon as root. Running a daemon as "root" (Linux) or "LocalSystem" (Windows) is, for security reasons, considered bad practice, and can, amongst many other obstacles, hamper debugging the running daemon. Consider to add a line 
    
    
        [...]
        f.WriteString('Service', 'Type', 'simple');
        f.WriteString('Service', 'User', '...');   // insert a service account here, otherwise the daemon will run as root
        f.WriteString('Service', 'ExecStart', Application.ExeName + ' -r');
        [...]
    

in the DaemonSystemdInstallerUnit.CreateSystemdControlFile code. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** At runtime you'll find all the TDaemonMapper and especially the TDaemonMapper.WinBindings settings in the "self.Definition" and "self.Definition.WinBindings" structures. If you prefer, you may consider using some of them to populate settings in the .service file.

For a comprehensive guide on all settings available in a systemd .service file (sometimes called a systemd "unit file") please see the documentation of your Linux distribution. 

### Daemon Worker Thread

At the core of every daemon there is a piece of code which does the actual work, I named it the "worker" thread. It needs to be a separate thread, because in the background the daemon is supposed to continue to listen for control messages and process them. Creation and destruction of the thread can be seen in the above samples for TDaemon.DataModuleStart and TDaemon.DataModuleStop handlers. 
    
    
    procedure TDaemonWorkerThread.Execute;
    
    var
      i: integer;
    
    begin
      LogToFile('Daemon worker thread executing');
      while not Terminated do
      begin
        // placeholder, put your actual service code here
        // ...
        LogToFile('Daemon worker thread running');
        // Thread- and CPUload friendly 5s delay loop
        for i := 1 to 50 do
        begin
          if Terminated then break;
          Sleep(100);
        end;
        // ...
        // ----------------------------------------
      end;
      LogToFile('Daemon worker thread terminated');
    end;
    

This "worker" doesn't do much, it loops until terminated and writes a message into the log every 5 seconds. 

For your own daemon functionality, replace the code between "..." and "..." with your own. 

The inner loop of a TThread.Execute method must be layed out in a way so it checks the "Terminated" flag frequently. The flag is set by the TThread.Terminate method which is called to gracefully end a thread execution, like it can be seen in the above TDaemon.DataModuleStop sample. There is more info about TThreads available in the [Multithreaded Application Tutorial](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial") wiki. 

To keep the sample demo code simple, it does not handle runtime errors in any way. Errors in the logging unit will be dropped silently, all other runtime errors in the daemon will crash the daemon. Please refer to the wiki page about [Exceptions](<Exceptions.md> "Exceptions") to read more about catching and handling exceptions in general, and see the documentation on TThread.HandleException to read more about how to properly catch and forward an exception within the execute method of a worker thread. 

### Initializing TDaemon Properties in Code

If you prefer not to use the Lazarus IDE and its Forms Editor to populate your TDaemon and TDeamonMapper properties, you can do it all in code as well. If you want to go this route, note that your project must not be based on the LCL/Forms like other applications, but on DaemonApp. Failing this will make the code fail with numerous stream loading errors. 

Working with TDaemon (The event driven approach to program a daemon) has already been described in the above sections, the only real difference is that you will assign the event handlers in the application code like so: 
    
    
    TDaemon1 = class(TDaemon)
        {...}
      procedure DataModuleStart(Sender: TCustomDaemon; var OK: Boolean);   // sample event handler event
        {...}
    public
      constructor Create(AOwner: TComponent); override;  // overwrite the constructor to initialize the event handlers
    end;
    
    {...}
    
    constructor TDaemon1.Create(AOwner: TComponent);
    begin
      inherited Create(AOwner);
      {...}
      onStart := @DataModuleStart;   // assign the sample event handler code
      {...}
    end;
    

Follow the same strategy with TDaemonMapper to populate the Mapper. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If you prefer an OOP based approach using inheritance to customize the daemon- and the mapper class you can do this as well, inherit your own daemon from TCustomDaemon and your own mapper from TCustomDaemonMapper, and overwrite the various virtual functions in the "Protected" section as needed

Here is an excerpt from the DaemonApp unit showing the available properties: 
    
    
      TCustomDaemon = Class(TDataModule)
      private
    
        [...]
    
      Protected
        Function Start : Boolean; virtual;
        Function Stop : Boolean; virtual;
        Function Pause : Boolean; virtual;
        Function Continue : Boolean; virtual;
        Function Execute : Boolean; virtual;
        Function ShutDown : Boolean; virtual;
        Function Install : Boolean; virtual;
        Function UnInstall: boolean; virtual;
    
        [...]
    

## Creating a Service/Daemon (in Code only)

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The complete sample source code for this wiki page is available via the link at the end of this wiki page.

TestDaemonCodeOnly is a demo project utilizing the TCustomDaemon and TCustomDaemonMapper classes, and it works without the Lazarus IDE. It does therefore not need any Lazarus specific helper files like .lpr or .lfm, and can be maintained using any plain text editor. The programming paradigm used is not event-driven like in GUI centric code, but works based on OOP and inheritance. The helper units providing file logging, a daemon worker thread and support for systemd -install and -uninstall for Linux are the same as in the above GUI sample. 

Support for -install and -uninstall for Linux is provided by the same DaemonSystemdInstallerUnit as used above. Windows service installation and uninstallation is already built-in in the DaemonApp unit. The TDaemonMapper class provides the WinBindings property to configure the service for Windows, on Linux this property is not used and has no effect. The code provided in the DeamonSystemdInstallerUnit provides similar functionality for Linux/systemd/systemctl. 

The -run parameter is not supported on Windows, Windows has its own facilities (the Windows Service Control Manager or the "sc" command line tool) to control services. 

The daemon code needs to be implemented in multiple threads, one for handling the daemon control messages and a separate thread for the daemon "worker" code. Just overwriting the TCustomDaemon.Execute method won't work well, since while the daemon loops in the "TDaemon.Execute" method it will not handle control messages. It will therefore appear to hang in the Service Control Manager and refuse to respond to sc commands. The "worker" thread code is identical with what's required in the above GUI sample, and implemented in the DaemonWorkerThread unit. 

The absence of a GUI and console requires the use of a logging facility. The TCustomDaemon object provides a logging facility (through the EventLog property), but that is not thread-safe. The FileLoggerUnit provides a simple thread-safe log to file facility. For more sophisticated logging, consider to use the [LazLogger](<LazLogger.md> "LazLogger") unit. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The logfile created by the FileLoggerUnit is written to the program directory, please ensure that the dameon has write access to it.

For more detailed informations about multithreading and how to make code thread-safe through the use of CriticalSections see the [Multithreaded Application Tutorial](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial"). 
    
    
    program TestDaemonCodeOnly;
    
    {$mode objfpc}{$H+}
    
    uses
     {$IFDEF UNIX}
      cthreads,
       {$ENDIF}
      Classes,
      SysUtils,
      { you can add units after this }
      DaemonApp,
      FileLoggerUnit,             // Thread safe file logger
      DaemonWorkerThread,         // a TThread descendant to do the daemon's work
      DaemonSystemdInstallerUnit; // -install and -uninstall support for Linux/systemd
    
    
    // ------------------------------------------------------------------
    // TDaemonMapper: This class type defines the settings for the daemon
    // ------------------------------------------------------------------
    
    type
      TDaemonMapper1 = class(TCustomDaemonMapper)
      public
        constructor Create(AOwner: TComponent); override;
      end;
    
      constructor TDaemonMapper1.Create(AOwner: TComponent);
    
      begin
        inherited Create(AOwner);
        with TDaemonDef(self.DaemonDefs.Add) do
        begin
          DaemonClassName := 'TDaemon1';           // This must exactly match the daemon class
          Name := 'TestDaemonCodeOnly';            // Service name
          DisplayName := 'Test Daemon (CodeOnly)'; // Service display name
          Description := 'Lazarus Daemons and Services Wiki Demo Service (Created in Code only)';
          Options := [doAllowStop, doAllowPause];
          WinBindings.StartType := stManual;  // stBoot, stSystem, stAuto, stManual, stDisabled
          WinBindings.ServiceType := stWin32;
        end;
      end;
    
    // -------------------------------------------------------------------
    // TDaemon: This class type definies the daemon task and handles the
    //          events triggered by the Windows/Linux Service Manager
    // -------------------------------------------------------------------
    
    type
    
      { TDaemon1 }
    
      TDaemon1 = class(TCustomDaemon)
      private
        FDaemonWorkerThread: TDaemonWorkerThread;
      public
        function Start: boolean; override;     // start the daemon worker thread
        function Stop: boolean; override;      // stop the daemon worker thread
        function Pause: boolean; override;     // pause the daemon worker thread (Windows only)
        function Continue: boolean; override;  // resume the daemon worker thread (Windows only)
        function ShutDown: boolean; override;  // stop the daemon worker thread because of OS shutdown
        function Install: boolean; override;   // added -install suppport for Linux
        function UnInstall: boolean; override; // added -uninstall suppport for Linux
      end;
    
      { TDaemon1 }
    
      // ------------------------------------------------
      // Daemon start and stop signal
      // ------------------------------------------------
    
      function TDaemon1.Start: boolean;
      begin
        // Create a suspended worker thread - see DaemonWorkerThread unit
        FDaemonWorkerThread := TDaemonWorkerThread.Create;
        // Parametrize it
        FDaemonWorkerThread.FreeOnTerminate := False;
        // Start the worker
        FDaemonWorkerThread.Start;
        LogToFile(Format('TDaemon1: service %s started, PID=%d', [self.Definition.Name, GetProcessID]));
        Result := True;
      end;
    
      function TDaemon1.Stop: boolean;
      begin
        // stop and terminate the worker
        if assigned(FDaemonWorkerThread) then
        begin
          FDaemonWorkerThread.Terminate;
          // Wait for the thread to terminate.
          FDaemonWorkerThread.WaitFor;
          FreeAndNil(FDaemonWorkerThread);
        end;
        Result := True;
        LogToFile(Format('TDaemon1: service %s stopped', [self.Definition.Name]));
      end;
    
    // ------------------------------------------------
    // Daemon pause and continue signal (Windows only)
    // ------------------------------------------------
    
      function TDaemon1.Pause: boolean;
      begin
        FDaemonWorkerThread.Suspend;    // deprecated, yet still working
        LogToFile(Format('TDaemon1: service %s paused', [self.Definition.Name]));
        Result := True;
      end;
    
      function TDaemon1.Continue: boolean;
    
      begin
        LogToFile(Format('TDaemon1: service %s continuing', [self.Definition.Name]));
        FDaemonWorkerThread.Resume;    // deprecated, yet still working
        Result := True;
      end;
    
    // --------------------------------------------------------------
    // Daemon stop on operating system shutdown signal (Windows only)
    // --------------------------------------------------------------
    
      function TDaemon1.ShutDown: boolean;
      begin
        Result := self.Stop;   // On shutdown, we trigger the stop handler. This will do nicely for this demo
        LogToFile(Format('TDaemon1: service %s shutdown', [self.Definition.Name]));
      end;
    
    // -----------------------------------------------------------------------------------------
    // Daemon install and uninstall helpers for Linux, Windows is already built in TCustomDaemon
    // -----------------------------------------------------------------------------------------
    
      function TDaemon1.Install: boolean;
    
      var
        FilePath: string;
    
      begin
        Result := False;
        {$IFDEF WINDOWS}
        Result := inherited Install;
        {$ELSE}
          {$IFDEF UNIX}
          FilePath := GetSystemdControlFilePath(Self.Definition.Name);
          LogToFile(Format('TDaemon1: installing control file: %s',[FilePath]));
          Result := CreateSystemdControlFile(self, FilePath);
          if not Result then
            LogToFile('TDaemon1: Error creating systemd control file: ' + FilePath);
          {$ENDIF}
        {$ENDIF}
        LogToFile(Format('TDaemon1: service %s installed: %s', [self.Definition.Name, BoolToStr(Result, 'ok', 'failure')]));
      end;
    
      function TDaemon1.UnInstall: boolean;
    
      var
        FilePath: string;
    
      begin
        Result := False;
        {$IFDEF WINDOWS}
        Result := inherited UnInstall;
        {$ELSE}
          {$IFDEF UNIX}
          FilePath := GetSystemdControlFilePath(Self.Definition.Name);
          Result := RemoveSystemdControlFile(FilePath);
          if not Result then
            LogToFile('TDaemon1: Error removing systemd control file: ' + FilePath);
          {$ENDIF}
        {$ENDIF}
        LogToFile(Format('TDaemon1: service %s uninstalled: %s', [self.Definition.Name, BoolToStr(Result, 'ok', 'failure')]));
      end;
    
    // ---------------------
    // Daemon main init code
    // ---------------------
    
    begin
      RegisterDaemonClass(TDaemon1);
      RegisterDaemonMapper(TDaemonMapper1);
      Application.Run;
    end.
    

## Daemon/Service Installation

### Windows

You can install the service from any elevated command prompt by starting the executable with the -install parameter. 

Open a terminal with elevated privileges (run as administrator), navigate to the directory where you compiled your test application into, and try the following commands: 

Command | Description   
---|---  
`TestDaemon -install` | install the daemon   
`sc query TestDaemon` | check the service status   
`sc config TestDaemon start=auto` | configure the service to be started  
when the machine boots   
`sc config TestDaemon start=manual` | configure the service to be started manually   
`sc config TestDaemon start=disabled` | disable the service (cannot be started manually)   
`TestDaemon -uninstall` | remove the daemon   
  
[![Screenshot taken 2/2022 on a Windows 11 machine](https://wiki.freepascal.org/images/a/a4/2022-01-31_23_50_28-Lazarus_IDE_v2.2.0RC2_-_Daemon_application.png)](</File:2022-01-31_23_50_28-Lazarus_IDE_v2.2.0RC2_-_Daemon_application.png> "Screenshot taken 2/2022 on a Windows 11 machine")

After successful installation you can control the service either via the sc command line, or by GUI using the "Services" management console. Recent Windows Versions do also have a basic service control facility on the "Services" tab of the Task Manager. 

The most important sc commands to control a service after installation are: 

Command | Description   
---|---  
`sc start TestDaemon` | start the service   
`sc query TestDaemon` | query the service state   
`sc stop TestDaemon` | stop the service   
  
You may also use the Windows service control manager console. 

[![ServiceControlManager.png](https://wiki.freepascal.org/images/0/07/ServiceControlManager.png)](</File:ServiceControlManager.png>)

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Most Windows service properties, like autostart on boot or usage of a specific service account, can be set via the TDaemonMapper1.WinBindings property in the DaemonMapperUnit1 file.

### Linux

Unlike for Windows, the TDaemonApp application does not have any support for an automated installation, since under Linux/Unix there is a wide variety of service control subsystems. You can implement your own installation scripts in the install and uninstall handlers of the TCustomDaemon object, like shown for systemd support in the above code sample. If you follow that route, installation and de-installation is essentially the same like on Windows. 

You can install and uninstall the service from a terminal by starting the executable with sudo and the -install or -uninstall parameter: 

Command | Description   
---|---  
`sudo ./TestDaemon -install` | install the service   
`sudo ./TestDaemon -uninstall` | uninstall the service   
`systemctl enable TestDaemon` | configure the service to be started  
when the machine boots   
`systemctl disable TestDaemon` | configure the service to not be started  
when the machine boots   
  
After successful installation you can control the service using the systemctl command 

Command | Description   
---|---  
`systemctl start TestDaemon` | start the service   
`systemctl status TestDaemon` | query the service state   
`systemctl stop TestDaemon` | stop the service   
  
[![OnLinuxWithSystemctl.png](https://wiki.freepascal.org/images/4/4f/OnLinuxWithSystemctl.png)](</File:OnLinuxWithSystemctl.png>)

The code in DaemonSystemdInstallerUnit.CreateSystemdControlFile triggered by the -install command line parameter reads the most basic properties of the TDaemonMapper class and writes a simple systemd .service file into the /lib/systemd/system directory to put your daemon under control of systemd. If you prefer to do it manually, you may use any text editor to create your own file following this template: 
    
    
    [Unit]
    Description=Long description of your application
    After=network.target
    
    [Service]
    Type=simple
    User=name_of_a_service_user_account
    ExecStart=complete_path_and_file_name -r
    RemainAfterExit=yes
    TimeoutSec=25
    
    [Install]
    WantedBy=multi-user.target
    

  * Edit the following values 
    * Description - Long Description of your service application
    * ExecStart - complete-path_and_file_name is the name of your compiled service application with its complete path
    * User - specify an service user account here, if omitted the daemon will run as root, running a daemon as root is strongly discouraged


  * Save the file 
    * Navigate to /lib/systemd/system/
    * Name the file the name_of_your_service.service



Many more entries in the .service file created by DaemonSystemdInstallerUnit.CreateSystemdControlFile are available to customize how systemd controls your daemon, please see the the systemd "unit" file reference of your Linux distribution. 

## Reading the Log File

This is a sample log file showing the daemon's internals while running, it was created on Windows, but will look exactly the same on Linux. 

[![2022-02-01 12 17 27-TestDaemon.png](https://wiki.freepascal.org/images/d/d1/2022-02-01_12_17_27-TestDaemon.png)](</File:2022-02-01_12_17_27-TestDaemon.png>)

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The FileLoggerUnit does always log into the program directory, please make sure the daemon has write access permissions to it.

## Debugging a Daemon/Service

Debugging a daemon is not as straightforward as debugging a normal application, since a running daemon does usually have neither a GUI nor a console, and its run/stop state is usually managed by the operating system. Windows and Linux require different approaches to debugging, especially to the way you initiate a debugging session. Once you have attached the debugger to the daemon and hit your first breakpoint, things work just with any other application, and you may control the debugger directly from your Lazarus GUI and the daemon source code. 

As a prerequisite, you need to compile your service/daemon using a "debug" configuration, so debug code gets inserted and a matching debug symbol file (.dbg) file is created. 

### Windows

The debugging strategy on Windows depends on what portion of the code you need to examine: 

\- If you need to debug -install or -uninstall, set "Run" - "Run Parameters" in the Lazarus GUI to -install or -uninstall, set your breakpoints, and start the code [F9]. 

\- If you need to debug the service worker code, start the service via the operating system (sc command or Windows Service Manager), determine the Process ID (PID), and then choose "Run" - "Attach to Program". Once attached, the debugger will automatically halt at a predefined temporary breakpoint in ntdll.dll!DbgUserBreakPoint, and the service will be looping somewhere within the worker code. After setting your breakpoints and watches in the worker code, resume your debugging by running the code using [F9]. 

[![2022-02-21 21 25 22-Lazarus IDE v2.2.0 - Daemon application \(debugging ...\).png](https://wiki.freepascal.org/images/3/38/2022-02-21_21_25_22-Lazarus_IDE_v2.2.0_-_Daemon_application_%28debugging_...%29.png)](</File:2022-02-21_21_25_22-Lazarus_IDE_v2.2.0_-_Daemon_application_\(debugging_...\).png>)

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The "Pause" button in the Lazarus symbol bar does not seem to work when debugging a running process remotely, it will hang the debugger if pressed. Use "Run" [F9] to resume debugging after a breakpoint.

\- If you need to debug the create or start handlers, or any other piece in code which might have already been executed before you had a chance to "Attach to program" you can make your code wait until a debugger is attached by inserting the following line in your code: 
    
    
    // This code will loop until a debugger is attached
    While not IsDebuggerPresent do sleep(100);   // will loop until a debugger is attached
    // ... put a breakpoint somewhere after the loop to catch the code as soon as it leaves the loop
    

[![2022-02-21 21 36 38-Lazarus IDE v2.2.0 - Daemon application \(debugging ...\).png](https://wiki.freepascal.org/images/c/c7/2022-02-21_21_36_38-Lazarus_IDE_v2.2.0_-_Daemon_application_%28debugging_...%29.png)](</File:2022-02-21_21_36_38-Lazarus_IDE_v2.2.0_-_Daemon_application_\(debugging_...\).png>)

After attaching the debugger and pressing [F9] the daemon will stop right at the breakpoint. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The -run parameter supported by Linux is not available on Windows. You must use the Windows Service Control Manager to start and stop the service. To attach the debugger to the then running process you need to run the Lazarus IDE with Administrative Prvilege (elevated), otherwise you will get Error 5 (access denied) when trying to attach to the service. If you are running Lazarus elevated, and nevertheless have difficulties attaching to your service ("Access denied" or "Run" - "Attach to program" is grayed out) make sure you have set the debugger in "Project" - "Project Settings" - "Debugger" properly, most likely you will need "Gdb (Gnu Debugger)".

### Linux

Linux is propably somewhat easier to debug, since you can execute your daemon like every other program from within the Lazarus IDE, if you enter -run into "Command line parameters" in the "Run" - "Run Parameters" menu. Keep in mind though, that many problems originate from security restrictions enforced by the operating system. Many of those restrictions are not applied when the daemon is run in user context. 

[![2022-02-22 12 18 20-Debian 10.8 Buster - VMware Workstation.png](https://wiki.freepascal.org/images/f/fd/2022-02-22_12_18_20-Debian_10.8_Buster_-_VMware_Workstation.png)](</File:2022-02-22_12_18_20-Debian_10.8_Buster_-_VMware_Workstation.png>)

Now you can start the daemon using [F9] like any normal application and debug it. 

[![2022-02-22 12 19 41-Debian 10.8 Buster - VMware Workstation.png](https://wiki.freepascal.org/images/e/e9/2022-02-22_12_19_41-Debian_10.8_Buster_-_VMware_Workstation.png)](</File:2022-02-22_12_19_41-Debian_10.8_Buster_-_VMware_Workstation.png>)

If you do, for instance because you suspect a problem with service specific restrictions, need to attach the debugger to the already running daemon, you can do so using "Run" - "Attach to Program", if you have configured the daemon to run under a specific user account instead of root. See the chapter above about Linux install and uninstall support on how to add an appropriate option to the .service file. 

Unfortunately there isn't such a handy "IsDebuggerPresent" call available like under Windows, but the following trick works nicely. 
    
    
    procedure WaitForDebugger;
    
    var
      SetMeValue:boolean=false;
    
    begin
      // Linux trick: attach the debugger, and change SetMeValue to true
      while not SetMeValue do
         sleep(100); // put a breakpoint here
    end;
    

Put a breakpoint into the line with the "sleep" command. Once the daemon is started using systemctl, execution it will be halted at your breakpoint in the while loop. Now you can attach the debugger to the PID you got from systemctl status, put a watch ([Ctrl-F5]) on the "SetMeValue" variable and then use the Evaluate/Modify context menu to change the value of the "SetMeValue" variable from "false" to "true", and resume program execution ([F9]). The execution will immedately leave the while loop and run to the next breakpoint. 

## Known problems

### Permission related Problems

While you can do most of the development of the core daemon code under a user account and from within an IDE (Lazarus), the live daemon will most likely run "in the background" under a different context. Nowadays operating systems do, for security reasons, run daemons in more or less restricted environments. Because a daemon does have neither a terminal nor a GUI to show any messages, debugging such issues can be hard. Unfortunately there is no common recipe which resolves all service related problems in all operating systems supported by FPC/Lazarus, as the strategy behind restricting service code is vastly different between Windows, Linux and macOS, and it is even changing between different versions of the same operating system, with the more recent versions trending to be more restrictive than their predecessors. 

If you make the transition from running the daemon code under your developer account to "go live" and test the daemon running under a service user account oder under "Localsystem" or "root" (which, btw, is considered bad security practice since many years), take the time to study the security related documentations of your operating system and find out which restrictions and other specialities apply if code runs in the background. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** See the section above about debugging daemons running in the background by attaching the debugger to the running code. Make your daemon, once the daemon is started by the operating system, wait until the debugger is attached. From there you can then observe the execution and detect restrictions related exceptions and other security/background operation related nastinesses.

### Exception EStreamError: "no streaming method available"
    
    
    Exception at 0041A30E: EStreamError:
    Failed to initialize component class "TDaemon1": No streaming method available.
    

You have included visual components (which require initalization by a built-in resource stream created by the .lfm file in GUI applications) in a code-only daemon project. If you decide to create the daemon in code, you may neither include the LazDaemon package, nor add the "LazDaemonApp" to the uses section. If you do, your project will compile fine, but if you start the exe you get the above error. 

### Several "Stream Read" and "Unkown Properties" errors ...

... when loading a daemon project through the Lazarus IDE. 

Most likely you try to open a TDaemon (GUI component) based project, but you haven't yet installed the LazDaemon package. 

Abort all loading errors, and install the LazDaemon package using the Lazarus Package Manager like described above. After Lazarus has re-compiled, the daemon project should re-load and compile without problems. 

[![Image 9.png](https://wiki.freepascal.org/images/f/fb/Image_9.png)](</File:Image_9.png>)

[![Image 10.png](https://wiki.freepascal.org/images/b/b3/Image_10.png)](</File:Image_10.png>)

### Lazarus IDE suggests to clear invalid properties from the .lpm file

This is another issue which was observed when you try to open a TDaemon (GUI) based project, but you haven't yet installed the LazDaemon package. 

[![Image 11.png](https://wiki.freepascal.org/images/1/13/Image_11.png)](</File:Image_11.png>)

Abort all loading errors, and install the LazDaemon package using the Lazarus Package Manager like described above. After Lazarus has re-compiled itself, the daemon project should re-load and compile without problems. 

### The Daemon won't start on Unix because of missing threading support

The code compiles fine, but if you try to run the daemon using a console and the -run parameter, you get a message saying something like "no thread support compiled in". If you try to start the dameon through systemctl, you won't see any message, it simply won't seem to run. You can use "systemctl status" to see the last error caused by the daemon, this will reveal the same crash infos like starting it using -run. 

` This binary has no thread support compiled in. Recompile the application with a thread-driver in the program uses clause before other units using thread. Runtime error 232 at ...   
...  
No heap dump by heaptrc unit Exitcode = 232 `

Most likely you compiled your project without support for the CThreads library (not required on Windows). See the above project lpr file sample code and the notes about "UseCThreads". The short of it is to go to the .lpr file and remove the {$IFDEF UseCThreads} ... {$ENDIF} around the "uses CThreads" statement, so the CThreads library gets linked into your code on Linux. On Windows this library is not required. 

### Debugging Issues observed on Windows

The "Pause" button in the Lazarus symbol bar activates, if a breakpoint is hit, but it does not seem to work when debugging a running process remotely, debugging will hang if the button is pressed. Use "Run" [F9] instead to resume debugging after a breakpoint. 

To attach the debugger to the running daemon process you need to run the Lazarus IDE with Administrative Prvilege (elevated), otherwise you will get "Error 5 (access denied)" when trying to attach the debugger. 

If all debugging selections in the "Run" menu are unavailable (grayed), navigate to "Project" - "Project Options" - "Debugger" and set the "Debugger Backend" to "Gdb [GNU debugger (gdb)]". It has been observed that this setting may get messed up if you copy the project source files from one computer / Lazarus installation over to a different one. 

### Debugging Issues observed on Linux

Using "Attach to program" to debug a running daemon via process ID (PID) does not seem to work, you get an error "Debugger Error" ... "Attach failed". One reason may be that you haven't specified a user in the service control file, and thus the service runs as "root", which hampers debugging. Besides of running the service using a dedicated service user account, as a workaround, consider to start debugging by loading the daemon from within Lazarus using "Run" - "Run Parameters" and entering the -run command line parameter. This way debugging does work, but note that the code runs in a different context (user instead of system- or service user account), which may change it's behaviour because of permission related issues. 

### Daemons have no Access to Network- and Cloud Drives (Onedrive, Dropbox)

When choosing the directory for storing the daemon executable (which is by default also used for logging), keep in mind that once the daemon runs in the background it does usually run under the context of a system- or service account. User-specific virtual drives created in one user's account are usually available neither to other user accounts nor to the system account. If you run the daemon from a networked/cloud drive, it is likely that the daemon won't start, since the OS will not be able to find the executable, and that write attempts to the log will fail, since by default the log file is written where the executable is. This may be confusing, since -install and -uninstall will likely work and log fine, because they are usually executed by hand and thus run in your user's context. 

### Specifying a Service User Account does not work on Linux

If you start the daemon and check which account it runs under, it appears to run as root, despite you have specified a different account in your systemd .service file. 

There are (unconfirmed) reports around that (on some versions of Linux?) in the .service file controlling the systemd the "User" entry must appear before the "ExecStart" entry to work properly, like shown in the code samples in the sections about Linux install and uninstall support. 

### System Codepage / UTF-8

[Old content, needs verfification 3/2022, may be obsolete] A LazDeamon project is working with default, not UTF-8, codepage. The _-dDisableUTF8RTL_ mode has to be activated with _Project Options ..._ -> _Compiler Options_ -> _Additions and Overrides_ -> _Use system encoding_. 

## [Some historical contents - needs to be verified and integrated or removed]

[Old content, needs verfification 3/2022, may be obsolete] 

### Linux (only for older Debian)

Download, configure, and "Save As" - the sample script located at Web Archive: [[1]](<https://web.archive.org/web/20130209102726/http://aurawin.com/lazarus/debian-service.sh>) (The original link is dead for a long time). 

  * SVC_ALIAS is the long description of your application
  * SVC_FILENAME is the actual file name of your compiled service application
  * SVC_DIR is the place your you copied the service application
  * SVC_SERVICE_SCRIPT is the final name of the service.sh when you "Save As" the customized debian-service.sh script.



Place your script in the /etc/init.d/ folder 

start the service by running "sudo service Name_Of_Your_Script start" 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** sudo has some variations, e.g.: 
    
    
      
    sudo -s #
      
    sudo -H #
      
    sudo -i  #
      
    sudo su  #
    

  
sudo sh # 

In order to auto-run the service at startup you can try update-rc.d or else will need a third party tool that will do this. 

Option 1 
    
    
    sudo update-rc.d Name_Of_Your_Script defaults
    

Option 2 
    
    
    sudo apt-get install chkconfig
    sudo chkconfig --add Name_Of_Your_Script
    sudo chkconfig --level 2345 Name_Of_Your_Script on
    

## See also

  * [GitHub Repository](<https://github.com/arminlinder/Lazarus-FPC-Wiki-Code-Samples/zipball/master/>) Complete demo project sample code (.zip)
  * [macOS daemons and agents](<macOS_daemons_and_agents.md> "macOS daemons and agents") \- macOS native agents using launchd
  * [ServiceManager](<ServiceManager.md> "ServiceManager") Example for the Free Pascal unit for managing Windows services
  * [Taming the daemon: PDF by Michaël Van Canneyt](<https://www.freepascal.org/~michael/articles/daemons/daemons.pdf>)
  * [Docker Containerization](<Docker_Containerization.md> "Docker Containerization") \- Containerization as a means to create daemons / services
  * [UniqueInstance](<UniqueInstance.md> "UniqueInstance")

---

_Source: [https://wiki.freepascal.org/Daemons_and_Services](https://web.archive.org/web/20240906204857/https://wiki.freepascal.org/Daemons_and_Services)_
