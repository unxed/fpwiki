# macOS daemons and agents

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 The launchd startup process
  * 3 launchctl
  * 4 Types of background processes
  * 5 Creating a launchd property list file
  * 6 Writing daemon code
    * 6.1 Mandatory
    * 6.2 Recommended
    * 6.3 Not recommended
  * 7 Examples
    * 7.1 Launch Daemon to run a script as a user on a schedule
  * 8 Launch daemon to run a TCP server
  * 9 See also
  * 10 External links



## Overview

UNIX-based operating systems have **daemons** or programs that run as background processes in the system context (eg named - the Internet Domain Name Server which is a daemon responsible for resolving domain name queries). Daemons are therefore better described as system background processes and should not have a graphical user interface. They are system-wide background processes of which there is only one instance for all clients. 

**Agents** are very similar to daemons in that they are both programs that run as background processes, but in the case of agents they are not running in the system context but in the context of an interactive user's session. Agents are therefore better described as user background processes and may have a graphical user interface or communicate with the user through the macOS system menu bar (eg temperature monitors generally display the status in the system menu bar). They are per-user background processes. 

Daemons and agents are each useful in their own right, but each has its own specific use cases. jobs that rely on system-level access should be run as daemons in the system context and jobs that do not require system-level access should be run as agents in the user's context. 

_Note: in the rest of this article the generic term daemon will be used to refer to both daemons and agents unless required otherwise by the context._

**launchd** is the macOS system-wide and per-user daemon manager which is started by the kernel during the system boot process. It manages processes, both for the system as a whole and for individual users. launchd provides the following benefits to daemon developers: 

  * Simplifies the process of making a daemon by handling many of the standard housekeeping chores normally associated with launching a daemon.
  * Provides system administrators with a central place to manage daemons on the system.
  * Supports inetd-style daemons.
  * Eliminates the primary reason for running daemons as root. Because launchd runs as root, it can create low-numbered TCP/IP listen sockets and hand them off to the daemon.
  * Simplifies error handling and dependency management for inter-daemon communication. As daemons can be launched on demand, communication requests do not fail if the daemon is not launched. They are simply delayed until the daemon can launch and process them.



## The launchd startup process

After the system is booted and the kernel is running, launchd is run to finish the system initialization. As part of that initialization, it goes through the following steps: 

  * Loads the parameters for each launch-on-demand system-level daemon from the property list files found in `/System/Library/LaunchDaemons/` and `/Library/LaunchDaemons/`.
  * Registers the sockets and file descriptors requested by those daemons.
  * Launches any daemons that requested to be running all the time.
  * As requests for a particular service arrive, it launches the corresponding daemon and passes the request to it.
  * When the system shuts down, it sends a SIGTERM signal to all of the daemons that it started.



The process for per-user agents is similar. When a user logs in, a per-user launchd is started. It does the following: 

  * Loads the parameters for each launch-on-demand user agent from the property list files found in `/System/Library/LaunchAgents/`, `/Library/LaunchAgents/`, and the user’s individual `~/Library/LaunchAgents/` directory.
  * Registers the sockets and file descriptors requested by those user agents.
  * Launches any user agents that requested to be running all the time.
  * As requests for a particular service arrive, it launches the corresponding user agent and passes the request to it.
  * When the user logs out, it sends a SIGTERM signal to all of the user agents that it started.



As launchd registers the sockets and file descriptors used by all daemons before it launches any of them, daemons can be launched in any order. If a request comes in for a daemon that is not yet running, the requesting process is suspended until the target daemon finishes launching and responds. 

If a daemon does not receive any requests over a specific period of time, it can choose to shut itself down and release the resources it holds. When this happens, launchd monitors the shutdown and makes a note to launch the daemon again when future requests arrive. 

## launchctl

**launchctl** is the primary interface to launchd which, among other things, allows the administrator or user to load or unload daemons **or** agents. Where possible, it is preferred that background jobs are launched on demand, based on criteria specified in their respective [property list files](<macOS_property_list_files.md> "macOS property list files"). 

Some of the useful commands available include: 

Command | Description   
---|---  
`launchctl list` | List loaded jobs   
`launchctl list <LABEL>` | List information about this loaded job from its .plist file.   
`launchctl load /Library/LaunchDaemons/<LABEL>.plist` | Load and start a job (system daemon) that is not disabled.   
`launchctl load -w /Library/LaunchDaemons/<LABEL>.plist` | Load, start and mark a job as not disabled (system daemon). Job restarts on next reboot.   
`launchctl unload /Library/LaunchDaemons/<LABEL>.plist` | Unload and stop a job (system daemon). Job restarts on next boot.   
`launchctl unload -w /Library/LaunchDaemons/<LABEL>.plist` | Unload, stop and disable a job (system daemon). Job does not restart on next reboot.   
`launchctl start <LABEL>` | Start a loaded job. Useful when debugging.   
`launchctl stop <LABEL>` | Stop a loaded job. Job may restart immediately if configured to keep running.   
`launchctl restart <LABEL>` | Restart a loaded job.   
  
**Note:** The commands listed above, while documented as "legacy", are simpler to use than the more complicated replacement commands. For the full details, open an Application > Utilities > Terminal and type `man launchctl` at the prompt to access the UNIX manual page. 

## Types of background processes

There are four types of background processes in macOS. To select the appropriate type of background process, consider the following: 

  * Whether it does something for the currently logged in user or for all users.
  * Whether it will be used by single application or by multiple applications.
  * Whether it ever needs to display a user interface or launch a GUI application.

Type | Managed by launchd? | Runs in | Can present UI?   
---|---|---|---  
Login item | No* | Per user context | Yes   
XPC service | Yes | Per user context | No  
(Except in a very limited way using IOSurface)   
Launch Daemon | Yes | System-wide context | No   
Launch Agent | Yes | Per user context | Best practice: Not recommended   
  
* Login items are started by the per-user instance of launchd, but it does not take any actions to manage them. 

## Creating a launchd property list file

To run your daemon under launchd, you must provide a configuration property list file for it. This file contains information about your daemon, including the list of sockets or file descriptors it uses to process requests. Specifying this information in a property list file lets launchd register the corresponding file descriptors and launch your daemon only after a request arrives for your daemon’s services. The required and recommended keys for all daemons are: 

Key | Description   
---|---  
Label | Contains a unique string that identifies your daemon to launchd. (required)   
ProgramArguments | Contains the arguments used to launch your daemon. The first argument is the absolute path to your program executable. (required)   
inetdCompatibility | Indicates that your daemon requires a separate instance per incoming connection. This causes launchd to behave like inetd, passing each daemon a single socket that is already connected to the incoming client. (required if your daemon was designed to be launched by inetd; otherwise, must not be included)   
RunAtLoad <boolean> | This optional key is used to control whether your job is launched once at the time the job is loaded. The default is false.   
StartInterval <integer> | Optional key causes the job to be started every N seconds. If the computer is asleep, the job will be started the next time it wakes up. If multiple intervals occur before the computer wakes, those events will be coalesced into one event.   
KeepAlive | This key specifies whether your daemon launches on-demand or must always be running. It is recommended that you design your daemon to be launched on-demand and for this reason this key defaults to false.   
  
The full list of keys for daemon property lists may be found in the online UNIX manual page for `launchd.plist` which you can access by opening an Application > Utilities > Terminal window and typing `man launchd.plist` at the prompt. 

The [property list file](<macOS_property_list_files.md> "macOS property list files") is structured the same for both daemons and agents. You indicate whether it describes a daemon or agent by the directory you place it in. Property list files describing daemons are installed in `/Library/LaunchDaemons/`, and those describing agents are installed in `/Library/LaunchAgents/` or in the individual user's `~/Library/LaunchAgents/` subdirectory. The following table lists these directories and describes their correct usage: 

Directory | Description   
---|---  
`~/Library/LaunchAgents` | Per-user agents provided by the user.   
`/Library/LaunchAgents` | Per-user agents provided by the administrator.   
`/Library/LaunchDaemons` | System-wide daemons provided by the administrator.   
`/System/Library/LaunchAgents` | Per-user agents provided by macOS.   
`/System/Library/LaunchDaemons` | System-wide daemons provided by macOS.   
  
The appropriate location for the executable program that you launch as your background process is `/usr/local/libexec/`. 

## Writing daemon code

Processes that are managed by launchd must follow certain requirements so that they interact properly with it. This includes launch daemons and launch agents. 

### Mandatory

  * You must provide a property list file with some basic launch-on-demand criteria for your daemon. See Creating a launchd Property List File.


  * You **must not** daemonize your process. No calling any daemon function, fork followed by exec, or fork followed by exit. If you do, launchd will consider your process has died. Depending on your property list key settings, launchd will either keep trying to relaunch your process until it gives up with a "respawning too fast" error or will be unable to restart it if it really does die.


  * Daemons and agents that are installed globally must be owned by the root user. Agents installed for the current user must be owned by that user. All daemons and agents must not be group or world writable.



### Recommended

  * Wait until your daemon is fully initialized before attempting to process requests. Your daemon should always provide a reasonable response (rather than an error) when processing requests.


  * Register the sockets and file descriptors used by your daemon in your launchd configuration property list file. If your daemon advertises a socket, check-in with launchd as part of your daemon initialization using the launch_activate_socket() function call.


  * Provide a handler to catch the SIGTERM signal sent by launchd to shut down your daemon.



### Not recommended

  * Do not set the user or group ID for your daemon. Include the UserName, UID, GroupName, or GID keys in your daemon’s configuration property list file instead.


  * Do not set the working directory. Include the WorkingDirectory key in your daemon’s configuration property list file instead.


  * Do not call chroot to change the root directory. Include the RootDirectory key in your daemon’s configuration property list file instead.


  * Do not call setsid to create a new session.


  * Do not close any stray file descriptors.


  * Do not change stdio to point to /dev/null. Include the StandardOutPath or StandardErrorPath keys in your daemon’s configuration property list file instead.


  * Do not set up resource limits with setrusage. Use the SoftResourceLimits and HardResourceLimit keys in your daemon’s configuration property list file instead.


  * Do not set the daemon priority with setpriority. Use the nice, ProcessType, LowPriorityIO, LowPriorityBackgroundIO, nice keys in your daemon’s configuration property list file instead.



## Examples

### Launch Daemon to run a script as a user on a schedule

Here is an example of a [property list file](<macOS_property_list_files.md> "macOS property list files") for a system-wide daemon which runs a script as a specific user at 11 o'clock every morning. It will run even when there is no user logged in to the computer provided that the computer is operating. 
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
            <key>Label</key>
            <string>my_daemon.job</string>
            <key>UserName</key>
            <string>username</string>
            <key>WorkingDirectory</key>
            <string>/Users/username/work_dir</string>
            <key>EnvironmentVariables</key>
            <dict>
                    <key>PATH</key>
                    <string>/Users/username/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin</string>
                    <key>SHELL</key>
                    <string>/bin/sh</string>
            </dict>
            <key>LowPriorityBackgroundIO</key>
            <true/>
            <key>Nice</key>
            <integer>10</integer>
            <key>Program</key>
            <string>/Users/username/bin/my_daemon.sh</string>
            <key>StandardErrorPath</key>
            <string>/Users/username/tmp/my_daemon.stderr</string>
            <key>StandardOutPath</key>
            <string>/Users/username/tmp/my_daemon.stdout</string>
            <key>StartCalendarInterval</key>
            <dict>
                    <key>Hour</key>
                    <integer>11</integer>
                    <key>Minute</key>
                    <integer>0</integer>
            </dict>
    </dict>
    </plist>
    

An explanation of the above file: 

**Line 1:** The XML prolog 

**Line 2:** The XML DTD for property list files 

**Line 6:** A unique name identifying this job 

**Line 8:** The username under which to run the job 

**Line 10:** Directory to change to before running the job 

**Line 14:** The path environment variable to use to find commands used in the script 

**Line 16:** The shell to use 

**Line 19:** Tells the kernel to consider this daemon to be low priority when doing filesystem I/O when throttled with a background classification 

**Line 21:** Sets the scheduling priority of this job to 10 (-20 = highest; 20 = lowest) 

**Line 23:** The script to run 

**Line 25:** The path to the file to which to map the the job's _stdout_ (Useful for debugging; the file is created if it does not exist. If omitted, output is directed to _/dev/null_.) 

**Line 27:** The path to the file to which to map the the job's _stderr_ (Useful for debugging; the file is created if it does not exist. If omitted, output is directed to _/dev/null_.) 

**Line 31:** The hour at which to start the job every day 

**Line 33:** The minute at which to start the job every day (if omitted, the job would run _every_ minute!) 

Here is the example `my_daemon.sh` script (referenced in the property list file above) to be executed: 
    
    
    #!/bin/sh
    date
    svn up 
    date
    

which just prints the date and time, updates the source code in the Subversion repository in `work_dir` and prints the date and time again. The standard output is captured in the `my_daemon.stdout` file and any error output is captured in the `my_daemon.stderr` file. On subsequent runs, these files will be _appended to_ and not overwritten. 

The property list file should be copied to the `/Library/LaunchDaemons/` directory and the file should be owned by root and not group or world writable. 

To start the job: 
    
    
    $ sudo launchctl load /Library/LaunchDaemons/my_daemon.plist
    

To stop the job from executing any more: 
    
    
    $ sudo launchctl unload /Library/LaunchDaemons/my_daemon.plist
    

## Launch daemon to run a TCP server

Here is an example of a property list file for a system-wide daemon which runs a TCP server. It will run even when there is no user logged in to the computer provided that the computer is operating. 
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
    	<key>Label</key>
    	<string>my_daemon</string>
    	<key>ProgramArguments</key>
    	<array>
    		<string>/usr/local/bin/my_daemon.sh</string>
    	</array>
    	<key>RunAtLoad</key>
            <true/>
    	<key>StandardErrorPath</key>
    	<string>/Users/username/tmp/my_daemon.stderr</string>
    	<key>StandardOutPath</key>
    	<string>/Users/username/tmp/my_daemon.stdout</string>
    </dict>
    </plist>
    

The property list file should be copied to the `/Library/LaunchDaemons/` directory and the file should be owned by root and not group or world writable. 

This is a simple TCP server daemon which simply prints "Hello Pascal World!" when a client connects to the server's port 12345; after printing that message the server then disconnects the client. For example, with telnet on FreeBSD: 
    
    
     $ telnet macmini8 12345
     Trying 192.168.1.21...
     Connected to macmini8.sentry.org.
     Escape character is '^]'.
     Hello Pascal World!
     Connection closed by foreign host.
    

In the absence of telnet on macOS, you can use nc (netcat): 
    
    
     $ nc localhost 12345
     Hello Pascal World!
     $
    
    
    
    //
    // Heavily based on forum member leledumbo's TCP server at http://pascalgeek.blogspot.com/2012/06/
    //
    
    Program my_daemon;
    
    {$mode objfpc}{H+}
    
    Uses
      cThreads, // needed to handle each client in its own thread
      Classes,
      SysUtils,
      Sockets,
      fpSock,   // see note
      fpAsync,
      BaseUnix;
    
    Type
    
      { Client handler thread }
    
      TClientHandlerThread = class(TThread)
      private
        FClientStream: TSocketStream;
      public
        constructor Create(AClientStream: TSocketStream);
        procedure Execute; override;
      end;
    
      { TCP server - override constructor to hook when a client connects }
    
      TTestServer = class(TTCPServer)
      private
        procedure TestOnConnect(Sender: TConnectionBasedSocket; AStream: TSocketStream);
      public
        constructor Create(AOwner: TComponent); override;
      end;
    
    { Helper to obtain and save IP and port as a string }
    
    function AddrToString(Addr: TSockAddr): String;
    begin
      Result := NetAddrToStr(Addr.sin_addr) + ':' + IntToStr(Addr.sin_port);
    end;
    
    { TClientHandlerThread }
    
    constructor TClientHandlerThread.Create(AClientStream: TSocketStream);
    begin
      inherited Create(false);
      FreeOnTerminate := true;
      FClientStream := AClientStream;
    end;
    
    procedure TClientHandlerThread.Execute;
    begin
      try
        FClientStream.WriteAnsiString('Hello Pascal World!' + LineEnding);
        WriteLn(AddrToString(FClientStream.PeerAddress) + ' connected');
      except
        on e: EStreamError do begin
          WriteLn('EStreamError');;
        end;
      end;
    
      WriteLn(AddrToString(FClientStream.PeerAddress) + ' disconnected');
      FClientStream.Free;
    end;
    
    { TTestServer }
    
    procedure TTestServer.TestOnConnect(Sender: TConnectionBasedSocket; AStream: TSocketStream);
    begin
      WriteLn('Incoming connection from ' + AddrToString(AStream.PeerAddress));
      TClientHandlerThread.Create(AStream);
    end;
    
    constructor TTestServer.Create(AOwner: TComponent);
    begin
      inherited;
      OnConnect := @TestOnConnect;
    end;
    
    { main }
    
    var
      ServerEventLoop: TEventLoop;
    
    begin
      ServerEventLoop := TEventLoop.Create;
      with TTestServer.Create(nil) do 
        begin
          EventLoop := ServerEventLoop;
          Port := 12345;
          WriteLn('Serving...');
          Active := true;
          EventLoop.Run;
        end;
    end.
    

**Note:** The unit `fpSock` is currently not being compiled for macOS. You will need to copy it from `/usr/local/share/fpcsrc/3.x.x/packages/fcl-net/src/fpsock.pp` to your project directory and then substitute SO_NOSIGPIPE for MSG_NOSIGNAL in the two places where it occurs in that file. Now when you compile your project it should be found, compiled and linked. 

The `my_daemon` executable file should be copied to `/usr/local/bin/` and should be owned by root and not group or world writable. 

The script `/usr/local/bin/my_daemon.sh` (referenced in the property list file above) for Launchd to start the Pascal TCP server daemon executable is: 
    
    
    #!/bin/sh
    /usr/local/bin/my_daemon
    

To start the TCP server, execute: 
    
    
    $ sudo launchctl load /Library/LaunchDaemons/my_daemon.plist
    

To stop the TCP server, execute: 
    
    
    $ sudo launchctl unload /Library/LaunchDaemons/my_daemon.plist
    

**A confession:** This is not the recommended method of launching a TCP server. In theory, the socket name (port) should be listed in the property list file and the macOS launch_activate_socket() function call should take care of setting up the socket. Unfortunately, while I have managed to get launch_activate_socket() working in C, I have so far failed to translate it successfully into Pascal. In the meantime, this somewhat inelegant workaround (using a script to launch the TCP server) works. 

## See also

  * [Cross-platform daemons and services](<Daemons_and_Services.md> "Daemons and Services").



## External links

  * [Apple: Technical Note: Daemons and Agents](<https://developer.apple.com/library/archive/technotes/tn2083/_index.html>).
  * [Apple: Daemons and Services Programming Guide](<https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLaunchdJobs.html>).

---

_Source: [https://wiki.freepascal.org/macOS_daemons_and_agents](https://web.archive.org/web/20241207104811/https://wiki.freepascal.org/macOS_daemons_and_agents)_
