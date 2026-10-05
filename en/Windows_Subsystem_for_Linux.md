# Windows Subsystem for Linux

Microsoft introduced a certain compatibility functionality in order to run ELF binaries under Windows 10. 

## Contents

  * 1 WSL1
    * 1.1 Installing WSL and FPC
    * 1.2 Running the test app from Windows
    * 1.3 Running (linux) FPC from Windows
    * 1.4 Running (linux) Gdb from Windows
  * 2 WSL2
    * 2.1 Installing
      * 2.1.1 Unable to switch to version 2
    * 2.2 Using FPC
    * 2.3 Using GDB
  * 3 Running ELF binaries from Windows app
    * 3.1 Using WSL.exe
  * 4 Lazarus
    * 4.1 Using Lazarus from WSL
    * 4.2 Using WSL FPC and GDB from Windows
    * 4.3 Quick and Dirty - Using WSL FPC from Windows
  * 5 See Also



## WSL1

WSL1 is implemented as Microsoft developed Linux-kernel-compatible execution environment. This is not an actual Linux kernel, but only its substitute, which provides the most common functionality. 

The functionality is ENOUGH to run FPC without modifications. 

### Installing WSL and FPC

Microsoft provides a number of Linux distros through Microsoft Store application. The following steps are using Ubuntu 16.4 LTS distro: 

  * Install WSL 
    * you can either install WSL through Powershell (as suggested in Microsoft documentation OR you can install via "Turn Windows features on or off")



    

    You can run control panel (press Win+R type in "control" and press enter) and search for "windows features" in order to bring the dialog up. The link is also available in "Add and Remove programs" section of control panel).

[![wsl enable marked.png](https://wiki.freepascal.org/images/9/93/wsl_enable_marked.png)](</File:wsl_enable_marked.png>)

Note: WSL1 only requires "Windows subsystem for Linux" to be installed. 

"Virtual Machine Platform" is only requires for WSL2. 

  * Install and Run Ubuntu distro
  * Update the system



    Run the following commands on Linux command-line
    
    
    sudo apt-get update
    sudo apt-get upgrade
    

### Running the test app from Windows

The "wsl" utility can be used to run Linux (x86_64) targets on Windows. If you used the example above to compile the application, then you should be able to run as following, from Windows command-line 
    
    
    cd c:\testrun
    wsl ./testrun
    

    output
    
    
    hello world
    

### Running (linux) FPC from Windows

Similar to the sample application, FPC itself can also be ran from Windows. 

Here's an example create a folder in Windows, and create a file there test.pas. Edit the file (i.e. using notepad) and put the following code: 
    
    
    procedure RunLoop;
    var
      i : integer;
    begin
      for i:=1 to 10 do
        write(i,' ');
      writeln;
    end;
    
    begin 
      {$ifdef mswindows}
      writeln('windows');
      {$endif}
      {$ifdef linux}
      writeln('linux');
      {$endif}
      writeln('hello world');
      writeln(sizeof(Pointer));
      RunLoop;
    end.
    

Run the following commands from command line. Note, that the use of "WSL" is critical (otherwise you'll get a windows binary). (Running the command-line requires both WSL installed and FPC for the distro installed) 
    
    
    wsl fpc ./test
    wsl ./test
    

You should see in command-line the following output: 
    
    
    linux
    hello world
    8
    1 2 3 4 5 6 7 8 9 10
    

Indicating the binary was built for linux 64-bit system 

### Running (linux) Gdb from Windows

WSL1 provides enough functionality in order for GDB to run. 

However, GDB itself is not installed by default (on Ubuntu 16.4 distro) and needs to be installed prior to use 
    
    
    wsl sudo apt-get install gdb
    

Recompile the existing sample program with Dwarf2 enabled. 
    
    
    wsl fpc -gw2 test.pas
    

(you should see the size of the executable increasing) 

Run GDB for the compiledexecutable. 
    
    
    wsl gdb test
    

you should see: 
    
    
    C:\testbuild>wsl gdb test
    GNU gdb (Ubuntu 7.11.1-0ubuntu1~16.5) 7.11.1
    Copyright (C) 2016 Free Software Foundation, Inc.
    License GPLv3+: GNU GPL version 3 or later <<http://gnu.org/licenses/gpl.html>>
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
    and "show warranty" for details.
    This GDB was configured as "x86_64-linux-gnu".
    Type "show configuration" for configuration details.
    For bug reporting instructions, please see:
    <<http://www.gnu.org/software/gdb/bugs/>>.
    Find the GDB manual and other documentation resources online at:
    <<http://www.gnu.org/software/gdb/documentation/>>.
    For help, type "help".
    Type "apropos word" to search for commands related to "word"...
    Reading symbols from test...done.
    

at the initial-load place a breakpoint within the runloop procedure: 
    
    
    (gdb) b test.pas:6
    

resulting in: 
    
    
    Breakpoint 1 at 0x4001e6: file test.pas, line 6.
    

and proceed with the execution: 
    
    
    (gdb) r
    Starting program: /mnt/c/testbuild/test
    linux
    hello world
    8
    
    Breakpoint 1, RUNLOOP () at test.pas:6
    6           write(i,' ');
    (gdb) 
    

at this point you can do all typical Gdb actions you need to do. I.e. researching stack: 
    
    
    (gdb) bt
    #0  RUNLOOP () at test.pas:6
    #1  0x00000000004002fa in main () at test.pas:19
    

or terminating the exectuion 
    
    
    (gdb) q
    

## WSL2

WSL2 is a different approach, that's implemented using an actual Linux kernel running in a virtual machine. Namely the entire Linux kernel functionality is available (... and the sky is limit!). Microsoft also addressed some performance issues when writing files to NTFS. 

### Installing

  * You need to have both "Windows Subsystem for Linux" and "Virtual Machine Platform" installed
  * (Summer 2019) you need to be subscribed (it's free) for Windows Insider Preview program and use "FAST" (instead of "Slow) option of getting updates. 
    * That will allow you to install the "preview" edition of Windows with WSL2 available.
  * Install and initialize a desired Linux distro from Microsoft Online store
  * Switch the distro to use version 2, by running the following command-line:


    
    
    wsl --set-version Ubuntu-16.04 2
    

    if you running into troubles switching to Version 2, see below.
    The process of setting a version takes about a couple of minutes so be patient.

#### Unable to switch to version 2

If you're getting this error 
    
    
    The requested operation could not be completed due to a virtual disk system limitation. 
    Virtual hard disk files must be uncompressed and unencrypted and must not be sparse.
    

There are 2 ways you should be able to resolve this: 

  * Either manually remove "Compressed" flag from the distro folder, which usually resides at:


    
    
    %localappdata%\Packages\CanonicalGroupLimited.UbuntuonWindows_xxxxxxx
    

[![wsl compression mark.png](https://wiki.freepascal.org/images/7/7b/wsl_compression_mark.png)](</File:wsl_compression_mark.png>)

  * or try to disable compression and encryption for the entire disk. The following commands must be ran in "Administrator" mode (and would require a reboot)


    
    
    fsutil behavior set disableencryption 1
    fsutil behavior set disablecompression 1
    

See more at: <https://github.com/microsoft/WSL/issues/4103>

If you're getting an error of Timeout - try to restart the PC. 

### Using FPC

The process of installation and use of FPC is identical to the described in WSL1. (The installation isidentical to an actual Linux or Linux running in a 3d party Virtual Machine) 

No additional changes are needed for FPC itself in order to run and compile from WSL2. 

### Using GDB

In general the installation of GDB is similar to WSL1. 

It seems like there were changes in pipe handling in Gdb, compared to WSL1. 

Thus running 
    
    
     wsl gdb application_name
    

would still work, but sometimes pipe communication might fail and you would not see a GDB prompt. 

On the other hand, if a user tries to use gdb directly from wsl.exe shell, it works just fine. 

## Running ELF binaries from Windows app

Any windows application can run ELF binary using "wsl.exe". 

Windows itself would not recognize ELF executable as an executable. Thus an attempt to open it would make Windows prompt the user for the host application to be specified. There's not special Windows API yet to run them. So far the only way of running an ELF binary is using "wsl.exe". 

That means that a desired Linux distro(s) needs to be installed. (and preferably one of them selected as default) 

### Using WSL.exe

Running ELF binary with WSL.exe requires an explicit call to "wsl.exe" and passing ELF binary as a parameter, as well as any additional parameters. 

That typically involves creating a process with StdOut/StrErr redirected to pipes (either by using WinAPI directory or [TProcess](<TProcess.md> "TProcess")). 

**Caveat** : there seems to be some special handling of Pipe-redirected output in WSL itself. The launched WSL might not terminate until it receives something on the input buffer. It might also skip some output, if the launching application is "sleeping" for too long. Thus the typical wait for output period should be minimal. 
    
    
      p := TProcess.Create(nil);
      try
        p.Executable := 'wsl.exe'; 
        p.Parameters.Add('ls'); // running ls utility 
        p.Parameters.Add('-l'); // with -l parameter
        p.Options := [poUsePipes];
    
        while (p.Running) or (p.Output.NumBytesAvailable > 0) do
        begin
          if p.Running and (p.Output.NumBytesAvailable = 0) then
          begin
            s:=#0; // writing anything to the input
            p.Input.Write(s, length(s));
            Sleep(1); // the shortest sleep possible
          end;
    
          sz := p.Output.NumBytesAvailable;
          if sz>0 then begin
            SetLength(s, sz);
            sz := p.Output.Read(s[1], sz);
            if sz < length(s) then SetLength(s, sz);
            write(s);
          end;
       end;
    
      finally
        p.Free;
      end;
    

It also doesn't seem that WSL is returning the exit code back. 

  
One can refer to the full code example of "wslwrap" application: <https://github.com/skalogryz/wslwrap>

## Lazarus

### Using Lazarus from WSL

WSL itself comes without graphical support by default. Yet it should be possible to install X-Windows server on Windows 10 machine itself, and configure WSL distro to use Windows 10 as its windows server. 

That also should open an option for developing and debugging Linux GUI applications using WSL. 

### Using WSL FPC and GDB from Windows

As long as Lazarus is capable of "prefixing" every FPC (and Gdb) commands with "wsl" - forcing the Windows Subsystem for Linux to execute, it should be capable of producing native (command-line) executables. 

It's likely Lazarus on Windows would also need to substitute the proper file path when dealing with WSL-ed projects. 

### Quick and Dirty - Using WSL FPC from Windows

**Dislaimer** : the proper solution might require additional work on IDE side. While the compiler doesn't need to be changed at all. It's possible to create a WSL compiler in order to by-pass Lazarus limitations, without a need of building a special cross-compiler. 

  * before begin, make sure you've FPC installed on WSL (linux) side.
  * Build wslwrap application (<https://github.com/skalogryz/wslwrap>)
  * Rename wslwrap to ppcwsl_fpc.exe (a name can also follow FPC cross-compiler name convention, in this case the result might be even better)



    The such name has two purposes:
    1) it starts with "ppc". Lazarus verifies the compiler executable to start with either "ppc" or "fpc", otherwise it would produce (a lot of annoying) warnings
    2) "_fpc" part enforces the use of "fpc" executable on WSL side (this is "wslwrap" feature)

  * Place the executable to your lazarus fpc configuration folder. (next to the current fpc.exe)
  * In Lazarus IDE options change "Compiler executable" to the new ppcwsl_fpc.exe.



    The IDE would complain about "fpc.cfg" missing - ignore it.

[![wsl fakefpc ideoption.png](https://wiki.freepascal.org/images/5/5a/wsl_fakefpc_ideoption.png)](</File:wsl_fakefpc_ideoption.png>)

  * Start a new (non GUI) project, i.e. with the following code


    
    
    begin 
      writeln('hello world');
    end.
    

  * Change project settings to:



    Target OS = Linux
    Target CPI = x86_64

  * Compile the project
  * In the project folder you should see the ELF executable showing up. You should be able to execute it using "wsl.exe"



[![wsl widows compiled.png](https://wiki.freepascal.org/images/c/c2/wsl_widows_compiled.png)](</File:wsl_widows_compiled.png>)

In order to rollback, simply restore the original FPC configuration in the IDE option. 

## See Also

  * <https://docs.microsoft.com/en-us/windows/wsl/about> \- official Microsoft WS documentation
  * <https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux> wikipedia
  * <https://virtualizationreview.com/articles/2017/02/08/graphical-programs-on-windows-subsystem-on-linux.aspx-> running WSL programs using X-Windows server installed on native Windows 10.

---

_Source: [https://wiki.freepascal.org/Windows_Subsystem_for_Linux](https://web.archive.org/web/20250116164222/https://wiki.freepascal.org/Windows_Subsystem_for_Linux)_
