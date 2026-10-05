# Target NativeNT

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

This target allows to compile applications for the basic Windows NT kernel interface. This includes usermode applications und kernelmode device drivers. 

## Contents

  * 1 Reasons for this port
  * 2 Supported systems
  * 3 Building the compiler
  * 4 Building the RTL
  * 5 Usermode Hello World
    * 5.1 Building
    * 5.2 Running
  * 6 Kernelmode Hello World
    * 6.1 Building
    * 6.2 Running
  * 7 Driver signing



## Reasons for this port

Windows NT operating systems (Windows NT 3.5 up to Windows 7) are based on a design that allows running different environments (called "environment subsystem") that user applications interact with. 

Examples are 

  * the infamous Win32 subsystem (which provides the WinAPI)
  * the POSIX / Interix subsystem (included as SUA in Windows Vista/7 Ultimate and Enterprise)
  * OS/2 subsystem (dropped in Windows 2000)



Those subsystems don't use the usual WinAPI, but the API of the NT kernel itself which is exposed through ntdll.dll to user space. Also the device drivers that run on a Windows NT system don't use the WinAPI, but the API provided by **ntoskrnl.exe** and **hal.dll**. 

As the Win32/64 targets link to WinAPI I saw no better way than to start a new target which supports the kernel API, but thanks to the very common structure of Win32/64 and NativeNT (same binary format, many kernel32 functions are similar to ntdll ones) the implementation of this port is rather (!) easy. 

## Supported systems

In theory, user mode applications and device drivers can support all Windows NT versions from Windows NT 4 or even 3.51 on, but the only tested systems are currently: 

  * Windows 2000 (Windows NT 5.0)
  * [ReactOS](<http://www.ReactOS.org/>) (equivalent to Windows 2003 or Windows NT 5.2, see also [ReactOS](<ReactOS.md> "ReactOS"))



The supported versions depend on the used functions. For a rather complete list and the supported kernels see [here](<http://jedi-apilib.sourceforge.net/native/NativeList.html>). 

Also the only supported processor architecture is currently i386. A port to x86_64 (Windows XP and newer) is planned in the near future. Also once the ARM and PPC ports of ReactOS are mature enough (e. g. kernel itself and early user mode is running) I plan to make ports for them. 

Note: Device drivers for WinCE systems are NOT possible with this port, as that system has a different kernel structure. 

## Building the compiler

Currently only a cross compiler is available. To build it you need a version of Free Pascal newer than revision 14568, e.g. FPC 2.6 or later. 

On Linux and similar: 
    
    
    cd $YourFPCDir
    cd compiler
    make
    cp ppc386 $WhereYouWantTheCompiler
    cd ../utils/fpcm
    make
    cp fpcmake $WhereYouWantFPCMake
    

On Windows (assuming you use an SVN checkout/source directory): 

  * go to directory where you checked out FPC
  * open cmd.exe (e. g. Start -> Run -> Write "cmd" -> OK )
  * navigate to your FPC directory


    
    
    cd compiler
    make
    cp ppc386.exe %WhereYouWantTheCompiler%
    cd ..\utils\fpcm
    make
    cp fpcmake.exe %WhereYouWantFPCMake%
    

Notes: 

  * on Windows, you need to have the GNU make utility in your PATH (e.g. the one provided with Lazarus) or use its absolute path
  * on Windows and Linux/*nix you need to have FPC in your PATH or supply it to make with "FPC=path/to/fpc/binary"
  * you do not need to do the copy steps, it's mainly useful if you want to replace your installed (trunk) compiler
  * compiling the fpcmake utility is needed!



## Building the RTL

Now you can compile the RTL 

  * for usermode


    
    
    make FPC=$YourNewPPC386Binary OS_TARGET=nativent
    

  * for kernelmode


    
    
    make FPC=$YourNewPPC386Binary OS_TARGET=nativent OPT="-dKMODE"
    

$YourNewPPC386Binary is "../../compiler/ppc386" when you didn't copy it to another directory. 

The compiled units will be in "../units/i386-nativent" (relative from nativent directory). 

Note: 

  * the define for kernel mode is needed, because the differences between kernel and user mode are rather huge, but I also didn't want to maintain two targets for this (also a normal user doesn't compile the RTL very often)
  * if you want to have both RTLs compile the kernel mode one first and copy the units to an "i386-nativent-kmode" directory (for example)



## Usermode Hello World

### Building

If you're experienced enough you can modify your fpc.cfg to include the NativeNT units (TODO), but for now I only use compiler command line switches. 
    
    
    program helloworld;
     
    uses
      // as WriteLn is not supported, yet, we need to
      // use the functions provided by the kernel directly
      NDK, NDKUtils;
     
    var
      ntstr: TNtUnicodeString;
      interval: TLargeInteger;
    begin
      ShortStrToNtStr('Hello World!'#13#10, ntstr);
      NtDisplayString(@ntstr);
      FreeMem(ntstr.buffer);
     
      // wait 3 seconds
      interval.QuadPart := - 3000 * 10000;
      NtDelayExecution(@interval);
    end.
    

Compile like this 
    
    
    $YourNewPPC386 -n -Tnativent -Fu$PathToUserModeRTL helloworld.pas
    

You should now have a helloworld.exe file in your current directory. 

Note: Your anti virus may complain about this new executable, but it is clean! 

### Running

**Important: Do NOT test this on a production machine (I mean it!). I'm not responsible for any damage to your computer.**

Copy the new executable to your test machine (e.g. a VM) into the system32 directory of your Windows/ReactOS installation (e.g. C:\WINNT\system32 up to Windows 2000, C:\Windows\system32 from Windows XP on, C:\ReactOS\system32 on ReactOS). You might want to backup the registry file system32\config\SYSTEM (e.g. not needed if you use QEMU with an QCow2 base image). Open regedit (Start -> Run -> Write "regedit" -> OK) and navigate to 
    
    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager
    

and edit the BootExecute value (you should see a "autochk" in there). Add this after the "AutoCheck" entry: 
    
    
    HelloWorld helloworld
    

First "HelloWorld" is a custom name and second one is the name of the executable inside the system32 directory. You can optionally pass arguments after "helloworld" like "*" in "AutoCheck" entry, but you need to parse them manually as ParamStr/-Count isn't implemented yet. 

Now restart the machine. You should see a "Hello World!" message in the top left corner of the screen after the boot screen. If you see a Bluescreen: well, I hope you didn't use a production machine. 

## Kernelmode Hello World

### Building

The note about configuration in user mode applies here, too. 
    
    
    library helloworld; // this is important - do not try a "program" here
     
    // tell FPC that we want to compile a kernel mode application
    // (NEEDS a RTL that was compiled with KMODE)
    {$apptype native}
     
    uses
      // for entry point types and debug output
      DDK;
     
    // this method is called once our driver is unloaded
    procedure DriverUnload(aObject: PDriverObject); stdcall;
    begin
      DbgPrint('Unloading driver');
    end;
     
    // during the entry point the variables DriverObject and
    // RegistryPath are valid
    begin
      DbgPrint('Hello World!');
     
      // we need to setup the unload routine or the driver will
      // only be unloaded on shutdown!
      DriverObject^.DriverUnload := @DriverUnload;
    end.
    

Compile like this: 
    
    
    $YourNewPPC386 -n -Tnativent -Fu$PathToKernelModeRTL -ohelloworld.sys helloworld.pas
    

The naming convention for drivers is ".sys", so we change the extention from .dll to .sys. 

### Running

**Note:** As long as you set the driver to "start on demand", instead of "start on boot" or "start automatically" you _might_ do this on a production machine, but I don't recommend it. 

To see the debug messages you need a tool for Windows like [DebugView](<http://technet.microsoft.com/en-us/sysinternals/bb896647.aspx>) (I didn't test this on ReactOS, though) which needs to be started before the driver. 

Now copy the driver to the system32\drivers directory on your test machine. 

To run a driver you need to install it as a service (yes, drivers are regarded as "services" - this might be a heritage of the Mach architecture of Windows NT). I've created a tool which uses the Service Control Manager API to do this, but until I've uploaded it somewhere you need to install it manually: 

  * open regedit
  * navigate to HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services
  * create a new key and give it a unique name (e.g. "helloworld")
  * add the following values in there:



  


Name | Type | Value   
---|---|---  
ErrorControl | DWORD | 0x00000001   
ImagePath | EXPAND_SZ | system32\drivers\helloworld.sys   
Start | DWORD | 0x00000003   
Type | DWORD | 0x00000001   
  
  
Note: You might not be able to create an expandable string value (EXPAND_SZ) in Windows (you can in ReactOS and Vista for certain), so you need to trick by exporting another driver entry and modifying that or you write a Pascal tool which creates those entries. 

Before you can start the driver you need to generate a PE checksum for it (as far as I know Windows checks the PE checksum at least for drivers). You need a tool which can generate a PE checksum or you can write your own one (there are some examples on the net). Also you can wait until I've uploaded my DriverHelper tool which can do this, too. 

After you've created the checksum, you can finally start the driver: 

  * open cmd.exe
  * type "net start helloworld" (the name of the KEY you created)
  * now you should get a "Hello World!" message inside DebugView (or whatever application you use for viewing debug output)
  * type "net stop helloworld" to unload the driver (you should get another message in your debuging application)



Note: If you get a BSOD you can simply restart and everything will be fine until you start the driver again. 

## Driver signing

A 64-bit kernel mode driver needs to be signed. And it may be that on Windows 10+ a 32-bit driver also needs to be signed. You can during development use a local certificate for that as per the DDK documentation on MSDN. 

32-bit drivers below or equal to platform Windows 7 are known to work.

---

_Source: [https://wiki.freepascal.org/Target_NativeNT](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/Target_NativeNT)_
