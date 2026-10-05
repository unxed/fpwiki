# Executing External Programs

│ **[Deutsch (de)](</Executing_External_Programs/de> "Executing External Programs/de")** │  **English (en)** │  **[español (es)](</Executing_External_Programs/es> "Executing External Programs/es")** │  **[français (fr)](</Executing_External_Programs/fr> "Executing External Programs/fr")** │  **[italiano (it)](</Executing_External_Programs/it> "Executing External Programs/it")** │  **[日本語 (ja)](</Executing_External_Programs/ja> "Executing External Programs/ja")** │  **[Nederlands (nl)](</Executing_External_Programs/nl> "Executing External Programs/nl")** │  **[polski (pl)](</Executing_External_Programs/pl> "Executing External Programs/pl")** │  **[português (pt)](</Executing_External_Programs/pt> "Executing External Programs/pt")** │  **[русский (ru)](<../ru/Executing_External_Programs.md> "Executing External Programs/ru")** │  **[slovenčina (sk)](</Executing_External_Programs/sk> "Executing External Programs/sk")** │  **[中文（中国大陆） (zh_CN)](</Executing_External_Programs/zh_CN> "Executing External Programs/zh CN")** │    
****

## Contents

  * 1 Overview: Comparison
  * 2 (Process.)RunCommand
    * 2.1 RunCommand extensions
  * 3 SysUtils.ExecuteProcess
  * 4 MS Windows: CreateProcess, ShellExecute and WinExec
    * 4.1 Using ShellExecuteEx for elevation/administrator permissions
  * 5 Unix fpsystem, fpexecve and shell
  * 6 TProcess
    * 6.1 The Simplest Example
    * 6.2 A Simple Example
    * 6.3 An improved example (but not correct yet)
    * 6.4 Reading large output
    * 6.5 Using input and output of a TProcess
    * 6.6 Hints on the use of TProcess
    * 6.7 macOS show application bundle in foreground
    * 6.8 Run detached program
    * 6.9 Example of "talking" with aspell process
    * 6.10 Replacing shell operators like "| < >"
      * 6.10.1 Why using special operators to redirect output doesn't work
    * 6.11 How to redirect output with TProcess
      * 6.11.1 Notes
    * 6.12 Redirecting input and output and running under root
    * 6.13 Using fdisk with sudo on Linux
    * 6.14 Parameters which contain spaces (Replacing Shell Quotes)
  * 7 LCLIntf Alternatives
    * 7.1 Open document in default application
    * 7.2 Open web page in default web browser
  * 8 WinExec
  * 9 See also



## Overview: Comparison

Here are different ways available in RTL, FCL and LCL libraries on how to execute an external command/process/program. 

Method  | Library  | Platforms  | Single Line  | Features   
---|---|---|---|---  
[ExecuteProcess](<Executing_External_Programs.md> "Executing External Programs") | RTL  | Cross-Platform  | Yes  | Very limited, synchronous.   
[ShellExecute](<Executing_External_Programs.md> "Executing External Programs") | WinAPI  | MS Windows only  | Yes  | Many. Can start programs with elevation/admin permissions.   
[fpsystem, fpexecve](<Executing_External_Programs.md> "Executing External Programs") | Unix  | Unix only  |  |   
[TProcess](<Executing_External_Programs.md> "Executing External Programs") | FCL  | Cross-Platform  | No  | Full.   
[RunCommand](<Executing_External_Programs.md> "Executing External Programs") | FCL  | Cross-Platform **Requires FPC 2.6.2+** | Yes  | Covers common TProcess usage.   
[OpenDocument](<Executing_External_Programs.md> "Executing External Programs") | LCL  | Cross-Platform  | Yes  | Only open document. The document would open with the application associated with that type of the document.   
[WinExec](<Executing_External_Programs.md> "Executing External Programs") | WinAPI  | MS Windows only  | Yes Legacy option found in old Delphi code. Replace with one of the above.   
  
## (Process.)RunCommand

  * [RunCommand](<http://www.freepascal.org/docs-html/fcl/process/runcommand.html>) reference
  * [RunCommandInDir](<http://www.freepascal.org/docs-html/fcl/process/runcommandindir.html>) reference



In FPC 2.6.2, some helper functions for TProcess were added to unit process based on wrappers used in the [fpcup](<Projects_using_Lazarus.md> "Projects using Lazarus") project. These functions are meant for basic and intermediate use and can capture output to a single string and fully support the _large output_ case. 

A simple example is: 
    
    
    program project1;
    
    {$mode objfpc}{$H+}
    
    uses 
      Process;
    
    var 
      s : ansistring;
    
    begin
    
    if RunCommand('/bin/bash',['-c','echo $PATH'],s) then
       writeln(s); 
    
    end.
    

But note that not all shell "built-in" commands (eg alias) work because aliases by default are not expanded in non-interactive shells and .bashrc is not read by non-interactive shells unless you set the BASH_ENV environment variable. So, this does not produce any output: 
    
    
    program project2;
    
    {$mode objfpc}{$H+}
    
    uses 
      Process;
    
    var 
      s : ansistring;
    
    begin
    
    if RunCommand('/bin/bash',['-c','alias'],s) then
      writeln(s); 
    
    end.
    

An overloaded variant of RunCommand returns the exitcode of the program. The RunCommandInDir runs the command in a different directory (sets p.CurrentDirectory): 
    
    
    function RunCommandIndir(const curdir:string;const exename:string;const commands:array of string;var outputstring:string; var exitstatus:integer): integer;
    function RunCommandIndir(const curdir:string;const exename:string;const commands:array of string;var outputstring:string): boolean;
    function RunCommand(const exename:string;const commands:array of string;var outputstring:string): boolean;
    

In **FPC 3.2.0+** the Runcommand got additional variants that allow to override TProcessOptions and WindowOptions. 

  


### RunCommand extensions

In **FPC 3.2.0+** the RunCommand implementation was generalized and integrated back into TProcess to allow for quicker construction of own variants. As an example a RunCommand variant with a timeout: 
    
    
      
    program TestTProcessTimeout;
    
    uses classes, sysutils, process, dateutils;
    
    type
     { TProcessTimeout }
     TProcessTimeout = class(TProcess)
                       protected
                         timeoutperiod: TTime;
                         timedout : boolean;
                         started : TDateTime;
                         procedure LocalnIdleSleep(Sender,Context : TObject;status:TRunCommandEventCode;const message:string);
                       public
                         class function RunCommandwithTimeout(const exename:TProcessString;const commands:array of TProcessString;const dir:string;out outputstring:string;out errorstring:string;out exitstate:integer; ProcessOptions : TProcessOptions = [];SWOptions:TShowWindowOptions=swoNone;timeout:integer=15):boolean;static;
                       end;
    
    
    
    procedure TProcessTimeout.LocalnIdleSleep(Sender,Context : TObject;status:TRunCommandEventCode;const message:string);
    begin
      if status=RunCommandIdle then
      begin
         writeln(Executable+'('+ Parameters.CommaText+')' +' time: '+TimeToStr(now-started));
         if (now-started)>timeoutperiod then
         begin
           writeln('process timed out ');
           timedout:=true;
           Terminate(255);
           exit;
         end;
         sleep(RunCommandSleepTime);
      end;
    end;
    
    class function TProcessTimeout.RunCommandwithTimeout(
                                 const exename     : TProcessString;
                                 const commands    : array of TProcessString;
                                 const dir         : string;
                                 out outputstring  : string;
                                 out errorstring   : string;
                                 out exitstate     : integer;
                                 ProcessOptions    : TProcessOptions = [];
                                 SWOptions         : TShowWindowOptions=swoNone;
                                 timeout           : integer=15
                                ):boolean;
    Var
        p : TProcessTimeout;
        i : integer;
    begin
      p:=TProcessTimeout.create(nil);
      p.OnRunCommandEvent:=@p.LocalnIdleSleep;
      p.CurrentDirectory:=dir;
    
    
      //timeout in Minutes, Check every  Seconds
      p.timeoutperiod:=timeout/MinsPerDay;
    
      (*
      //if you want to do the Timeout in Seconds 
      p.timeoutperiod:=timeout/SecsPerDay;
      *)
    
      // check every Second if finished
      p.runcommandsleeptime := 1000;
    
      if ProcessOptions<>[] then
        P.Options:=ProcessOptions - [poRunSuspended,poWaitOnExit];
      p.options:=p.options+[poRunIdle]; // needed to run the RUNIDLE event. See User Changes 3.2.0
    
      P.ShowWindow:=SwOptions;
      p.Executable:=exename;
      if high(commands)>=0 then
       for i:=low(commands) to high(commands) do
         p.Parameters.add(commands[i]);
      p.timedout:=false;
      p.started:=now;
      try
        // the core loop of runcommand() variants, originally based on the "large output" scenario in the wiki, but continously expanded over 5 years.
        RunCommandwithTimeout:=p.RunCommandLoop(outputstring,errorstring,exitstate)=0;
        if p.timedout then
          RunCommandwithTimeout:=false;
      finally
        p.free;
      end;
      if exitstate<>0 then RunCommandwithTimeout:=false;
    end;
    
    // example use:
    
    var
      output,errors : string;
      exitcode  :integer;
    begin
      //Windows and Unix have other programs that can be used
      {$if defined(WINDOWS)}
      if TProcessTimeout.RunCommandwithTimeout('cmd',['/c','dir','C:\*.exe','/s'],'c:\',output,errors,exitcode,[],swoNone,1) then // dir c:\*.exe /s
      {$else$}
      if TProcessTimeout.RunCommandwithTimeout('find',['/','-name','"*.sh"'],'/',output,errors,exitcode,[],swoNone,1) then // find / -name "*.sh"
      {$endif}
      begin
        writeln('program finished');
        writeln('--------------------output-------------------');
        writeln(output);
        writeln('--------------------errors-------------------');
        writeln(errors);
        if not(exitcode=0) then
        begin
          writeln('-program finished with errors (exitcode: '+exitcode.tostring+')-');
        end;
        readln();
      end
      else
      begin
        writeln('program not finished');
        writeln('--------------------output-------------------');
        writeln(output);
        writeln('--------------------errors-------------------');
        writeln(errors);
        writeln('-------------------EXITCODE------------------');
        writeln(exitcode);
        readln();
      end;
    end.
    

## SysUtils.ExecuteProcess

(Cross-platform)  
Despite a number of limitations, the simplest way to launch a program (modal, no pipes or any form of control) is to simply use : 
    
    
    SysUtils.ExecuteProcess(UTF8ToSys('/full/path/to/binary'), '', []);
    

The calling process runs synchronously: it 'hangs' until the external program has finished - but this may be useful if you require the user to do something before continuing in your application. For a more versatile approach, see the section about the preferred cross-platform **RunCommand** or other **TProcess** functionality, or if you only wish to target Windows you may use **ShellExecute**. 

  * [ExecuteProcess reference](<http://www.freepascal.org/docs-html/rtl/sysutils/executeprocess.html>)



## MS Windows: CreateProcess, ShellExecute and WinExec

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** While FPC/Lazarus has support for **CreateProcess** , **ShellExecute** and/or **WinExec** , this support is only in Win32/64. If your program is cross-platform, consider using **RunCommand** or **TProcess**.

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** WinExec is a 16-bit call that has been deprecated for years in the Windows API. In recent versions of FPC it generates a warning.

**ShellExecute** is a standard MS Windows function (ShellApi.h) with good [documentation on MSDN](<http://msdn.microsoft.com/en-us/library/windows/desktop/bb762153\(v=vs.85\).aspx>) (note their remarks about initialising COM if you find the function unreliable). 
    
    
    uses ..., ShellApi;
    
    // Simple one-liner (ignoring error returns) :
    if ShellExecute(0,nil, PChar('"C:\my dir\prog.exe"'),PChar('"C:\somepath\some_doc.ext"'),nil,1) =0 then;
    
    // Execute a Batch File :
    if ShellExecute(0,nil, PChar('cmd'),PChar('/c mybatch.bat'),nil,1) =0 then;
    
    // Open a command window in a given folder :
    if ShellExecute(0,nil, PChar('cmd'),PChar('/k cd \path'),nil,1) =0 then;
    
    // Open a webpage URL in the default browser using 'start' command (via a brief hidden cmd window) :
    if ShellExecute(0,nil, PChar('cmd'),PChar('/c start www.lazarus.freepascal.org/'),nil,0) =0 then;
    
    // or a useful procedure:
    procedure RunShellExecute(const prog,params:string);
    begin
      //  ( Handle, nil/'open'/'edit'/'find'/'explore'/'print',   // 'open' isn't always needed 
      //      path+prog, params, working folder,
      //        0=hide / 1=SW_SHOWNORMAL / 3=max / 7=min)   // for SW_ constants : uses ... Windows ...
      if ShellExecute(0,'open',PChar(prog),PChar(params),PChar(extractfilepath(prog)),1) >32 then; //success
      // return values 0..32 are errors
    end;
    

There is also ShellExecuteExW as a WideChar version, and ShellExecuteExA is AnsiChar. 

The fMask option can also use SEE_MASK_DOENVSUBST or SEE_MASK_FLAG_NO_UI or SEE_MASK_NOCLOSEPROCESS, etc. 

If in Delphi you used ShellExecute for **documents** like Word documents or URLs, have a look at the open* (OpenURL etc) functions in lclintf (see the Alternatives section lower down this page). 

### Using ShellExecuteEx for elevation/administrator permissions

If you need to execute external program with administrator/elevated privileges, you can use the **runas** method with the alternative ShellExecuteEx function: 
    
    
    uses ShellApi, ...;
    
    function RunAsAdmin(const Handle: Hwnd; const Path, Params: string): Boolean;
    var
      sei: TShellExecuteInfoA;
    begin
      FillChar(sei, SizeOf(sei), 0);
      sei.cbSize := SizeOf(sei);
      sei.Wnd := Handle;
      sei.fMask := SEE_MASK_FLAG_DDEWAIT or SEE_MASK_FLAG_NO_UI;
      sei.lpVerb := 'runas';
      sei.lpFile := PAnsiChar(Path);
      sei.lpParameters := PAnsiChar(Params);
      sei.nShow := SW_SHOWNORMAL;
      Result := ShellExecuteExA(@sei);
    end;
    
    procedure TFormMain.RunAddOrRemoveApplication;
    begin
      // Example that uses elevated rundll to open the Control Panel to Programs and features
      RunAsAdmin(FormMain.Handle, 'rundll32.exe shell32.dll,Control_RunDLL appwiz.cpl', '');
    end;
    

## Unix fpsystem, fpexecve and shell

These functions are platform dependent. 

  * [fpsystem reference](<http://www.freepascal.org/docs-html/rtl/unix/fpsystem.html>)
  * [fpexecve reference](<http://www.freepascal.org/docs-html/rtl/baseunix/fpexecve.html>)



Linux.Shell/Unix.Shell was an 1.0.x equivalent to fpsystem with less narrowly defined error handling, and after a decade of deprecation it was finally removed. In nearly all cases it can be replaced by fpsystem that features more POSIX like errorhandling. 

## TProcess

You can use TProcess to launch external programs. Some of the benefits of using TProcess are that it is: 

  * Platform Independent
  * Capable of reading from stdout and writing to stdin.
  * Possible to wait for a command to finish or let it run while your program moves on.



Important notes: 

  * TProcess is not a terminal/shell! You cannot directly execute scripts or redirect output using operators like "|", ">", "<", "&" etc. It is possible to obtain the same results with TProcess using pascal, some examples are below..
  * Presumably on Linux/Unix: you **must** specify the full path to the executable. For example '/bin/cp' instead of 'cp'. If the program is in the standard PATH then you can use the function [FindDefaultExecutablePath](<http://lazarus-ccr.sourceforge.net/docs/lcl/fileutil/finddefaultexecutablepath.html> "doc:lcl/fileutil/finddefaultexecutablepath.html") from the [FileUtil](<http://lazarus-ccr.sourceforge.net/docs/lcl/fileutil/index.html> "doc:lcl/fileutil/index.html") unit of the LCL.
  * On Windows, if the command is in the path, you don't need to specify the full path.
  * [TProcess reference](<http://lazarus-ccr.sourceforge.net/docs/fcl/process/tprocess.html> "doc:fcl/process/tprocess.html")



### The Simplest Example

A lot of typical cases have been prepared in the [Runcommand](<Executing_External_Programs.md> "Executing External Programs") functions. Before you start copy and paste the examples below, check them out first. 

### A Simple Example

This example (**that shouldn't be used in production, see Large Output or, better,[Runcommand](<Executing_External_Programs.md> "Executing External Programs")**) just shows you how to run an external program, nothing more: 
    
    
    // This is a demo program that shows
    // how to launch an external program.
    program launchprogram;
     
    // Here we include files that have useful functions
    // and procedures we will need.
    uses 
      Classes, SysUtils, Process;
     
    // This defines the var "AProcess" as a variable 
    // of the type "TProcess"
    var 
      AProcess: TProcess;
     
    // This is where our program starts to run
    begin
      // Now we will create the TProcess object, and
      // assign it to the var AProcess.
      AProcess := TProcess.Create(nil);
     
      // Tell the new AProcess what the command to execute is.
      // Let's use the Free Pascal compiler (i386 version that is)
      AProcess.Executable:= 'ppc386';
    
      // Pass -h together with ppc386 so actually 'ppc386 -h' is executed:
      AProcess.Parameters.Add('-h');
     
      // We will define an option for when the program
      // is run. This option will make sure that our program
      // does not continue until the program we will launch
      // has stopped running.                vvvvvvvvvvvvvv
      AProcess.Options := AProcess.Options + [poWaitOnExit];
     
      // Now let AProcess run the program
      AProcess.Execute;
     
      // This is not reached until ppc386 stops running.
      AProcess.Free;   
    end.
    

That's it! You have just learned to run an external program from inside your own program. 

### An improved example (but not correct yet)

That's nice, but how do I read the Output of a program that I have run? 

Well, let's expand our example a little and do just that: **This example is kept simple so you can learn from it. Please don't use this example in production code, but use the code in#Reading large output.**
    
    
    // This is a 
    // FLAWED
    // demo program that shows
    // how to launch an external program
    // and read from its output.
    program launchprogram;
     
    // Here we include files that have useful functions
    // and procedures we will need.
    uses 
      Classes, SysUtils, Process;
     
    // This is defining the var "AProcess" as a variable 
    // of the type "TProcess"
    // Also now we are adding a TStringList to store the 
    // data read from the programs output.
    var 
      AProcess: TProcess;
      AStringList: TStringList;
    
    // This is where our program starts to run
    begin
      // Now we will create the TProcess object, and
      // assign it to the var AProcess.
      AProcess := TProcess.Create(nil);
     
      // Tell the new AProcess what the command to execute is.
      AProcess.Executable := '/usr/bin/ppc386'; 
      AProcess.Parameters.Add('-h'); 
    
      // We will define an option for when the program
      // is run. This option will make sure that our program
      // does not continue until the program we will launch
      // has stopped running. Also now we will tell it that
      // we want to read the output of the file.
      AProcess.Options := AProcess.Options + [poWaitOnExit, poUsePipes];
     
      // Now that AProcess knows what the commandline is it can be run.
      AProcess.Execute;
      
      // After AProcess has finished, the rest of the program will be executed.
     
      // Now read the output of the program we just ran into a TStringList.
      AStringList := TStringList.Create;
      AStringList.LoadFromStream(AProcess.Output);
       
      // Save the output to a file and clean up the TStringList.
      AStringList.SaveToFile('output.txt');
      AStringList.Free;
     
      // Now that the output from the process is processed, it can be freed.
      AProcess.Free;   
    end.
    

### Reading large output

In the previous example we waited until the program exited. Then we read what the program has written to its output. 

Suppose the program writes a lot of data to the output. Then the output pipe becomes full and the called progam waits until the pipe has been read from. 

But the calling program doesn't read from it until the called program has ended. A deadlock occurs. 

The following example therefore doesn't use poWaitOnExit, but reads from the output while the program is still running. The output is stored in a memory stream, that can be used later to read the output into a TStringList. 

If you want to read output from an external process, and can't use RunCommand, this is the code you should as a base for production use. If you are using **FPC 3.2.0+** a parametrisable form of this loop is available as the method **RunCommandLoop** in TProcess. An event **OnRunCommandEvent** can be hooked to further modify the behaviour. 
    
    
    program LargeOutputDemo;
    
    {$mode objfpc}{$H+}
    
    uses
      Classes, SysUtils, Process; // Process is the unit that holds TProcess
    
    const
      BUF_SIZE = 2048; // Buffer size for reading the output in chunks
    
    var
      AProcess     : TProcess;
      OutputStream : TStream;
      BytesRead    : longint;
      Buffer       : array[1..BUF_SIZE] of byte;
    
    begin
      // Set up the process; as an example a recursive directory search is used
      // because that will usually result in a lot of data.
      AProcess := TProcess.Create(nil);
    
      // The commands for Windows and *nix are different hence the $IFDEFs
      {$IFDEF Windows}
        // In Windows the dir command cannot be used directly because it's a build-in
        // shell command. Therefore cmd.exe and the extra parameters are needed.
        AProcess.Executable := 'c:\windows\system32\cmd.exe';
        AProcess.Parameters.Add('/c');
        AProcess.Parameters.Add('dir /s c:\windows');
      {$ENDIF Windows}
    
      {$IFDEF Unix}
        AProcess.Executable := '/bin/ls';
    
        {$IFDEF Darwin}
          AProcess.Parameters.Add('-recursive');
          AProcess.Parameters.Add('-all');
        {$ENDIF Darwin}
    
        {$IFDEF Linux}
          AProcess.Parameters.Add('--recursive');
          AProcess.Parameters.Add('--all');
        {$ENDIF Linux}
    
        {$IFDEF FreeBSD}
          AProcess.Parameters.Add('-R');
          AProcess.Parameters.Add('-a');
        {$ENDIF FreeBSD}
     
        AProcess.Parameters.Add('-l');
      {$ENDIF Unix}
    
      // Process option poUsePipes has to be used so the output can be captured.
      // Process option poWaitOnExit can not be used because that would block
      // this program, preventing it from reading the output data of the process.
      AProcess.Options := [poUsePipes];
    
      // Start the process (run the dir/ls command)
      AProcess.Execute;
    
      // Create a stream object to store the generated output in. This could
      // also be a file stream to directly save the output to disk.
      OutputStream := TMemoryStream.Create;
    
      // All generated output from AProcess is read in a loop until no more data is available
      repeat
        // Get the new data from the process to a maximum of the buffer size that was allocated.
        // Note that all read(...) calls will block except for the last one, which returns 0 (zero).
        BytesRead := AProcess.Output.Read(Buffer, BUF_SIZE);
    
        // Add the bytes that were read to the stream for later usage
        OutputStream.Write(Buffer, BytesRead)
    
      until BytesRead = 0;  // Stop if no more data is available
    
      // The process has finished so it can be cleaned up
      AProcess.Free;
    
      // Now that all data has been read it can be used; for example to save it to a file on disk
      with TFileStream.Create('output.txt', fmCreate) do
      begin
        OutputStream.Position := 0; // Required to make sure all data is copied from the start
        CopyFrom(OutputStream, OutputStream.Size);
        Free
      end;
    
      // Or the data can be shown on screen
      with TStringList.Create do
      begin
        OutputStream.Position := 0; // Required to make sure all data is copied from the start
        LoadFromStream(OutputStream);
        writeln(Text);
        writeln('--- Number of lines = ', Count, '----');
        Free
      end;
    
      // Clean up
      OutputStream.Free;
    end.
    

Note that the above could also be accomplished by using RunCommand: 
    
    
    var s: string;
    ...
    RunCommand('c:\windows\system32\cmd.exe', ['/c', 'dir /s c:\windows'], s);
    

### Using input and output of a TProcess

See processdemo example in the [Lazarus-CCR SVN](<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/examples/process>). 

### Hints on the use of TProcess

When creating a cross-platform program, the OS-specific executable name can be set using directives "{$IFDEF}" and "{$ENDIF}". 

Example: 
    
    
    {...}
    AProcess := TProcess.Create(nil)
    
    {$IFDEF WIN32}
      AProcess.Executable := 'calc.exe'; 
    {$ENDIF}
    
    {$IFDEF LINUX}
      AProcess.Executable := FindDefaultExecutablePath('kcalc');
    {$ENDIF}
    
    AProcess.Execute;
    {...}
    

### macOS show application bundle in foreground

You can start an **application bundle** via TProcess by starting the executable within the bundle. For example: 
    
    
     AProcess.Executable:='/Applications/iCal.app/Contents/MacOS/iCal';
    

This will start the _Calendar_ , but the window will be behind the current application. To get the application in the foreground you can use the **open** utility with the **-n** parameter: 
    
    
     AProcess.Executable:='/usr/bin/open';
     AProcess.Parameters.Add('-n');
     AProcess.Parameters.Add('-a'); // optional: specifies the application to use; only searches the Application directories
     AProcess.Parameters.Add('-W'); // optional: open waits until the applications it opens (or were already open) have exited 
     AProcess.Parameters.Add('Pages.app'); // including .app is optional
    

If your application needs parameters, you can pass **open** the **\--args** parameter, after which all parameters are passed to the application: 
    
    
     AProcess.Parameters.Add('--args');
     AProcess.Parameters.Add('argument1');
     AProcess.Parameters.Add('argument2');
    

See also: [macOS open command](<macOS_Open_Sesame.md> "macOS Open Sesame"). 

### Run detached program

Normally a program started by your application is a child process and is killed, when your application is killed. When you want to run a standalone program that keeps running, you can use the following: 
    
    
    var
      Process: TProcess;
      I: Integer;
    begin
      Process := TProcess.Create(nil);
      try
        Process.InheritHandles := False;
        Process.Options := [];
        Process.ShowWindow := swoShow;
    
        // Copy default environment variables including DISPLAY variable for GUI application to work
        for I := 1 to GetEnvironmentVariableCount do
          Process.Environment.Add(GetEnvironmentString(I));
    
        Process.Executable := '/usr/bin/gedit';  
        Process.Execute;
      finally
        Process.Free;
      end;
    end;
    

### Example of "talking" with aspell process

Inside [pasdoc](<https://github.com/pasdoc/pasdoc/wiki>) source code you can find two units that perform spell-checking by "talking" with running aspell process through pipes: 

  * [PasDoc_ProcessLineTalk.pas unit](<https://github.com/pasdoc/pasdoc/blob/master/source/component/PasDoc_ProcessLineTalk.pas>) implements TProcessLineTalk class, descendant of TProcess, that can be easily used to talk with any process on a line-by-line basis.


  * [PasDoc_Aspell.pas units](<https://github.com/pasdoc/pasdoc/blob/master/source/component/PasDoc_Aspell.pas>) implements TAspellProcess class, that performs spell-checking by using underlying TProcessLineTalk instance to execute aspell and communicate with running aspell process.



Both units are rather independent from the rest of pasdoc sources, so they may serve as real-world examples of using TProcess to run and communicate through pipes with other program. 

### Replacing shell operators like "| < >"

Sometimes you want to run a more complicated command that pipes its data to another command or to a file. Something like 
    
    
    ShellExecute('firstcommand.exe | secondcommand.exe');
    

or 
    
    
    ShellExecute('dir > output.txt');
    

Executing this with TProcess will not work. i.e: 
    
    
    // this won't work
    Process.CommandLine := 'firstcommand.exe | secondcommand.exe'; 
    Process.Execute;
    

#### Why using special operators to redirect output doesn't work

TProcess is just that, it's not a shell environment, only a process. It's not two processes, it's only one. It is possible to redirect output however just the way you wanted. See the [next section](<Executing_External_Programs.md> "Executing External Programs"). 

### How to redirect output with TProcess

You can redirect the output of a command to another command by using a TProcess instance for **each** command. 

Here's an example that explains how to redirect the output of one process to another. To redirect the output of a process to a file/stream see the example [ Reading Large Output ](<Executing_External_Programs.md> "Executing External Programs")

Not only can you redirect the "normal" output (also known as stdout), but you can also redirect the error output (stderr), if you specify the poStderrToOutPut option, as seen in the options for the second process. 
    
    
    program Project1;
      
    uses
      Classes, sysutils, process;
      
    var
      FirstProcess,
      SecondProcess: TProcess;
      Buffer: array[0..127] of char;
      ReadCount: Integer;
      ReadSize: Integer;
    begin
      FirstProcess  := TProcess.Create(nil);
      SecondProcess := TProcess.Create(nil);
     
      FirstProcess.Options     := [poUsePipes]; 
      FirstProcess.Executable  := 'pwd'; 
      
      SecondProcess.Options    := [poUsePipes,poStderrToOutPut];
      SecondProcess.Executable := 'grep'; 
      SecondProcess.Parameters.Add(DirectorySeparator+ ' -'); 
      // this would be the same as "pwd | grep / -"
      
      FirstProcess.Execute;
      SecondProcess.Execute;
      
      while FirstProcess.Running or (FirstProcess.Output.NumBytesAvailable > 0) do
      begin
        if FirstProcess.Output.NumBytesAvailable > 0 then
        begin
          // make sure that we don't read more data than we have allocated
          // in the buffer
          ReadSize := FirstProcess.Output.NumBytesAvailable;
          if ReadSize > SizeOf(Buffer) then
            ReadSize := SizeOf(Buffer);
          // now read the output into the buffer
          ReadCount := FirstProcess.Output.Read(Buffer[0], ReadSize);
          // and write the buffer to the second process
          SecondProcess.Input.Write(Buffer[0], ReadCount);
      
          // if SecondProcess writes much data to it's Output then 
          // we should read that data here to prevent a deadlock
          // see the previous example "Reading Large Output"
        end;
      end;
      // Close the input on the SecondProcess
      // so it finishes processing it's data
      SecondProcess.CloseInput;
     
      // and wait for it to complete
      // be carefull what command you run because it may not exit when
      // it's input is closed and the following line may loop forever
      while SecondProcess.Running do
        Sleep(1);
      // that's it! the rest of the program is just so the example
      // is a little 'useful'
    
      // we will reuse Buffer to output the SecondProcess's
      // output to *this* programs stdout
      WriteLn('Grep output Start:');
      ReadSize := SecondProcess.Output.NumBytesAvailable;
      if ReadSize > SizeOf(Buffer) then
        ReadSize := SizeOf(Buffer);
      if ReadSize > 0 then
      begin
        ReadCount := SecondProcess.Output.Read(Buffer, ReadSize);
        WriteLn(Copy(Buffer,0, ReadCount));
      end
      else
        WriteLn('grep did not find what we searched for. ', SecondProcess.ExitStatus);
      WriteLn('Grep output Finish:');
      
      // free our process objects
      FirstProcess.Free;
      SecondProcess.Free;
    end.
    

That's it. Now you can redirect output from one program to another. 

#### Notes

This example may seem overdone since it's possible to run "complicated" commands using a shell with TProcess like: 
    
    
    Process.Commandline := 'sh -c "pwd | grep / -"';
    

But our example is more crossplatform since it needs no modification to run on Windows or Linux etc. "sh" may or may not exist on your platform and is generally only available on *nix platforms. Also we have more flexibility in our example since you can read and write from/to the input, output and stderr of each process individually, which could be very advantageous for your project. 

### Redirecting input and output and running under root

A common problem on Unixes (FreeBSD, macOS) and Linux is that you want to execute some program under the root account (or, more generally, another user account). An example would be running the _ping_ command. 

If you can use sudo for this, you could adapt the following example adapted from one posted by andyman on the forum ([[1]](<http://lazarus.freepascal.org/index.php/topic,14479.0.html>)). This sample runs `ls` on the `/root` directory, but can of course be adapted. 

A **better way** to do this is to use the policykit package, which should be available on all recent Linuxes. [See the forum thread for details.](<http://lazarus.freepascal.org/index.php/topic,14479.0.html>)

Large parts of this code are similar to the earlier example, but it also shows how to redirect stdout and stderr of the process being called separately to stdout and stderr of our own code. 
    
    
    program rootls;
    
    { Demonstrates using TProcess, redirecting stdout/stderr to our stdout/stderr,
    calling sudo on FreeBSD/Linux/macOS, and supplying input on stdin}
    {$mode objfpc}{$H+}
    
    uses
      Classes,
      Math, {for min}
      Process;
    
      procedure RunsLsRoot;
      var
        Proc: TProcess;
        CharBuffer: array [0..511] of char;
        ReadCount: integer;
        ExitCode: integer;
        SudoPassword: string;
      begin
        WriteLn('Please enter the sudo password:');
        Readln(SudoPassword);
        ExitCode := -1; //Start out with failure, let's see later if it works
        Proc := TProcess.Create(nil); //Create a new process
        try
          Proc.Options := [poUsePipes, poStderrToOutPut]; //Use pipes to redirect program stdin,stdout,stderr
          Proc.CommandLine := 'sudo -S ls /root'; //Run ls /root as root using sudo
          // -S causes sudo to read the password from stdin.
          Proc.Execute; //start it. sudo will now probably ask for a password
    
          // write the password to stdin of the sudo program:
          SudoPassword := SudoPassword + LineEnding;
          Proc.Input.Write(SudoPassword[1], Length(SudoPassword));
          SudoPassword := '%*'; //short string, hope this will scramble memory a bit; note: using PChars is more fool-proof
          SudoPassword := ''; // and make the program a bit safer from snooping?!?
    
          // main loop to read output from stdout and stderr of sudo
          while Proc.Running or (Proc.Output.NumBytesAvailable > 0) or
            (Proc.Stderr.NumBytesAvailable > 0) do
          begin
            // read stdout and write to our stdout
            while Proc.Output.NumBytesAvailable > 0 do
            begin
              ReadCount := Min(512, Proc.Output.NumBytesAvailable); //Read up to buffer, not more
              Proc.Output.Read(CharBuffer, ReadCount);
              Write(StdOut, Copy(CharBuffer, 0, ReadCount));
            end;
            // read stderr and write to our stderr
            while Proc.Stderr.NumBytesAvailable > 0 do
            begin
              ReadCount := Min(512, Proc.Stderr.NumBytesAvailable); //Read up to buffer, not more
              Proc.Stderr.Read(CharBuffer, ReadCount);
              Write(StdErr, Copy(CharBuffer, 0, ReadCount));
            end;
          end;
          ExitCode := Proc.ExitStatus;
        finally
          Proc.Free;
          Halt(ExitCode);
        end;
      end;
    
    begin
      RunsLsRoot;
    end.
    

Other thoughts: It would no doubt be advisable to see if sudo actually prompts for a password. This can be checked consistently by setting the environment variable SUDO_PROMPT to something we watch for while reading the stdout of TProcess avoiding the problem of the prompt being different for different locales. Setting an environment variable causes the default values to be cleared(inherited from our process) so we have to copy the environment from our program if needed. 

### Using fdisk with sudo on Linux

The following example shows how to run fdisk on a Linux machine using the sudo command to get root permissions. **Note: this is an example only, and does not cater for large output.**
    
    
    program getpartitioninfo;
    {Originally contributed by Lazarus forums wjackon153. Please contact him for questions, remarks etc.
    Modified from Lazarus snippet to FPC program for ease of understanding/conciseness by BigChimp}
    
    Uses
      Classes, SysUtils, FileUtil, Process;
    
    var
      hprocess: TProcess;
      sPass: String;
      OutputLines: TStringList;
    
    begin  
      sPass := 'yoursudopasswordhere'; // You need to change this to your own sudo password
      OutputLines:=TStringList.Create; //... a try...finally block would be nice to make sure 
      // OutputLines is freed... Same for hProcess.
         
      // The following example will open fdisk in the background and give us partition information
      // Since fdisk requires elevated priviledges we need to 
      // pass our password as a parameter to sudo using the -S
      // option, so it will will wait till our program sends our password to the sudo application
      hProcess := TProcess.Create(nil);
      // On Linux/Unix/FreeBSD/macOS, we need specify full path to our executable:
      hProcess.Executable := '/bin/sh';
      // Now we add all the parameters on the command line:
      hprocess.Parameters.Add('-c');
      // Here we pipe the password to the sudo command which then executes fdisk -l: 
      hprocess.Parameters.add('echo ' + sPass  + ' | sudo -S fdisk -l');
      // Run asynchronously (wait for process to exit) and use pipes so we can read the output pipe
      hProcess.Options := hProcess.Options + [poWaitOnExit, poUsePipes];
      // Now run:
      hProcess.Execute;
    
      // hProcess should have now run the external executable (because we use poWaitOnExit).
      // Now you can process the process output (standard output and standard error), eg:
      OutputLines.Add('stdout:');
      OutputLines.LoadFromStream(hprocess.Output);
      OutputLines.Add('stderr:');
      OutputLines.LoadFromStream(hProcess.Stderr);
      // Show output on screen:
      writeln(OutputLines.Text);
    
      // Clean up to avoid memory leaks:
      hProcess.Free;
      OutputLines.Free;
      
      //Below are some examples as you see we can pass illegal characters just as if done from terminal 
      //Even though you have read elsewhere that you can not I assure with this method you can :)
    
      //hprocess.Parameters.Add('ping -c 1 www.google.com');
      //hprocess.Parameters.Add('ifconfig wlan0 | grep ' +  QuotedStr('inet addr:') + ' | cut -d: -f2');
    
      //Using QuotedStr() is not a requirement though it makes for cleaner code;
      //you can use double quote and have the same effect.
    
      //hprocess.Parameters.Add('glxinfo | grep direct');   
    
      // This method can also be used for installing applications from your repository:
    
      //hprocess.Parameters.add('echo ' + sPass  + ' | sudo -S apt-get install -y pkg-name'); 
    
     end.
    

### Parameters which contain spaces (Replacing Shell Quotes)

In the Linux shell it is possible to write quoted arguments like this: 
    
    
    gdb --batch --eval-command="info symbol 0x0000DDDD" myprogram
    

And GDB will receive 3 arguments (in addition to the first argument which is the full path to the executable): 

  1. \--batch
  2. \--eval-command=info symbol 0x0000DDDD
  3. the full path to myprogram



The best solution to avoid complicated quoting is to to switch to TProcess.Parameters.Add instead of setting the Commandline for non trivial cases e.g. 
    
    
     AProcess.Executable:='/usr/bin/gdb';
     AProcess.Parameters.Add('--batch');
     AProcess.Parameters.Add('--eval-command=info symbol 0x0000DDDD'); // note the absence of quoting here
     AProcess.Parameters.Add('/home/me/myprogram');
    

And also remember to only pass full paths. 

TProcess.Commandline however does support some basic quoting for parameters with quoting. Quote the _whole_ parameter containing spaces with double quotes. Like this: 
    
    
    AProcess.CommandLine := '/usr/bin/gdb --batch "--eval-command=info symbol 0x0000DDDD" /home/me/myprogram';
    

  
The .CommandLine property is [deprecated](<http://bugs.freepascal.org/view.php?id=14446>) and bugreports to support more complicated quoting cases won't accepted. 

## LCLIntf Alternatives

Sometimes, you don't need to explicitly call an external program to get the functionality you need. Instead of opening an application and specifying the document to go with it, just ask the OS to open the document and let it use the default application associated with that file type. Below are some examples. 

### Open document in default application

In some situations you need to open some document/file using default associated application rather than execute a particular program. This depends on running operating system. Lazarus provides a platform independent procedure **OpenDocument** which will handle it for you. Your application will continue running without waiting for the document process to close. 
    
    
    uses LCLIntf;
    ...
    OpenDocument('manual.pdf');  
    ...
    

  * [OpenDocument reference](<opendocument.md> "opendocument")



### Open web page in default web browser

Just pass the URL required, the leading http:// appears to be optional under certain circumstances. Also, passing a filename appears to give the same results as OpenDocument() 
    
    
    uses LCLIntf;
    ...
    OpenURL('www.lazarus.freepascal.org/');
    

See also: 

  * [OpenURL reference](<OpenURL.md> "OpenURL")



Or, you could use **TProcess** like this: 
    
    
    uses Process;
    
    procedure OpenWebPage(URL: string);
    // Apparently you need to pass your URL inside ", like "www.lazarus.freepascal.org"
    var
      Browser, Params: string;
    begin
      FindDefaultBrowser(Browser, Params);
      with TProcess.Create(nil) do
      try
        Executable := Browser;
        Params:=Format(Params, [URL]);
        Params:=copy(Params,2,length(Params)-2); // remove "", the new version of TProcess.Parameters does that itself
        Parameters.Add(Params);
        Options := [poNoConsole];
        Execute;
      finally
        Free;
      end;
    end;
    

## WinExec

The Windows 3.x precursor of CreateProcess, sometimes still encountered in ancient Delphi code. Replace with one of the above. It is still available in unit Windows, but marked deprecated 

## See also

  * [TProcess documentation](<http://lazarus-ccr.sourceforge.net/docs/fcl/process/tprocess.html>)
  * [OpenDocument](<opendocument.md> "opendocument")
  * [OpenURL](<OpenURL.md> "OpenURL")
  * [TProcessUTF8](<TProcessUTF8.md> "TProcessUTF8")
  * [TXMLPropStorage](<TXMLPropStorage.md> "TXMLPropStorage")
  * [Webbrowser](<Webbrowser.md> "Webbrowser")

---

_Source: [https://wiki.freepascal.org/Executing_External_Programs](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/Executing_External_Programs)_
