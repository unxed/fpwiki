# Accessing macOS System Information

│ **English (en)** │ 

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Overview
  * 2 Sysctl
    * 2.1 FPsysctl()
    * 2.2 FPsysctlbyname
    * 2.3 Last words
  * 3 NSProcessinfo
    * 3.1 Properties
    * 3.2 Methods
    * 3.3 Example code
  * 4 Gestalt Manager
    * 4.1 Example: Retrieving OS version from Mac OS X 10.0+
  * 5 IOKit
    * 5.1 Example: Retrieving the platform serial number
    * 5.2 Example: Retrieving the platform UUID
  * 6 External links



## Overview

macOS provides four main ways to access system information: 

  * Sysctl interface - An interface used by Unix and Unix-like operating systems to access system hardware and kernel configuration attributes.
  * NSProcessInfo class - A class in the Foundation framework which gives access to a collection of information about the current process.
  * Gestalt Manager - Allows applications to query the running system configuration. **Deprecated** after 10.7 (Lion) in favour of the NSProcessInfo class or the SysCtl interface.
  * IOKit framework - A framework that implements non-kernel access to I/O Kit objects (drivers and nubs) through the device-interface mechanism.



These are described below with code examples. 

## Sysctl

Sysctl provides an interface that allows you to read and, with appropriate privileges, set many kernel attributes in macOS (and Linux and the BSDs). This provides a wealth of information detailing system hardware and configuration attributes which can be useful when debugging or simply optimising your application (eg to take advantage of threads on multi-core CPU systems). 

The '/usr/local/share/fpcsrc/rtl/darwin/sysctlh.inc' source file is a partial translation of the macOS '/usr/include/sys/sysctl.h' file. For details of the sysctls available, start a Terminal and type 'man sysctl'. Typing 'sysctl-a | more' in a Terminal will list all the sysctls. 

### FPsysctl()

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The function definition below for FPsysctl() is for FPC 3.2.0 and later - For FPC 3.0.4 and earlier the first argument is _pchar_ and not _pcint_. Adjust the code examples accordingly.

The fpSysCtl() function allows you to retrieve system information. The data returned from the function comprises integers (integer and int64), strings (AnsiStrings) and C structures (records). The function is defined in `/usr/local/share/fpcsrc/rtl/bsd/sysctl.pp` as: 
    
    
    function FPsysctl (Name: pcint; namelen:cuint; oldp:pointer;oldlenp:psize_t; newp:pointer;newlen:size_t):cint; cdecl; external name 'sysctl';
    

_name_ \-- a pointer to a Management Information Base (MIB) array of integers. Each element of this array must contain the correct value to read or write the particular system attribute. 

_namelen_ \-- the length of the array of integers in name. 

Note: if the old attribute value is not required, the next two variables can be set to Nil and 0. 

_oldp_ \-- a pointer to the buffer into which the value is copied. 

_oldlenp_ \-- the size of the oldp buffer. 

Note: if the new attribute value is not being set, the next two variables should be set to Nil and 0. 

_newp_ \-- a pointer to the buffer containing the new value to be set. 

_newlen_ \-- the size of the newp buffer. 

  
The size of the MIB array depends on the data to be read or written. The first element indicates the level of the information and the subsequent elements indicate the value to read. The top level names are: 
    
    
              --------------------------------------------------------------
              Name              Next level names          Description
                                are found in
              --------------------------------------------------------------
              CTL_DEBUG         sys/sysctl.h              Debugging
              CTL_VFS           sys/mount.h               File system
              CTL_HW            sys/sysctl.h              Generic CPU, I/O
              CTL_KERN          sys/sysctl.h              High kernel limits
              CTL_MACHDEP       sys/sysctl.h              Machine dependent
              CTL_NET           sys/socket.h              Networking
              CTL_USER          sys/sysctl.h              User-level
              CTL_VM            sys/resources.h           Virtual memory
              --------------------------------------------------------------
    

Let's look at CTL_HW. The subsequent elements are found in the system sysctl.h file and are: 
    
    
              -------------------------------------------------------------------------------          
              Name                     Value          Description
              -------------------------------------------------------------------------------          
              #define HW_MACHINE       1              /* string: machine class */
              #define HW_MODEL         2              /* string: specific machine model */
              #define HW_NCPU          3              /* int: number of cpus */
              #define HW_BYTEORDER     4              /* int: machine byte order */
              #define HW_PHYSMEM       5              /* int: total memory */
              #define HW_USERMEM       6              /* int: non-kernel memory */
              #define HW_PAGESIZE      7              /* int: software page size */
              #define HW_DISKNAMES     8              /* strings: disk drive names */
              #define HW_DISKSTATS     9              /* struct: diskstats[] */
              #define HW_EPOCH        10              /* int: 0 for Legacy, else NewWorld */
              #define HW_FLOATINGPT   11              /* int: has HW floating point? */
              #define HW_MACHINE_ARCH 12              /* string: machine architecture */
              #define HW_VECTORUNIT   13              /* int: has HW vector unit? */
              #define HW_BUS_FREQ     14              /* int: Bus Frequency */
              #define HW_CPU_FREQ     15              /* int: CPU Frequency */
              #define HW_CACHELINE    16              /* int: Cache Line Size in Bytes */
              #define HW_L1ICACHESIZE 17              /* int: L1 I Cache Size in Bytes */
              #define HW_L1DCACHESIZE 18              /* int: L1 D Cache Size in Bytes */
              #define HW_L2SETTINGS   19              /* int: L2 Cache Settings */
              #define HW_L2CACHESIZE  20              /* int: L2 Cache Size in Bytes */
              #define HW_L3SETTINGS   21              /* int: L3 Cache Settings */
              #define HW_L3CACHESIZE  22              /* int: L3 Cache Size in Bytes */
              #define HW_TB_FREQ      23              /* int: Bus Frequency */
              #define HW_MEMSIZE      24              /* uint64_t: physical ram size */
              #define HW_AVAILCPU     25              /* int: number of available CPUs */
              #define HW_MAXID        26              /* number of valid hw ids */
              -------------------------------------------------------------------------------
    

Armed with this information, we can now find out the number of CPUs: 
    
    
    // The full source code for this example is available from:
    // https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/fpc_sysctls/num_cpu.pas 
    ...
    Uses
      SysUtils, SysCtl, Unix;
    ...
    function NumberOfCPU: Integer;
    
    var
      mib: array[0..1] of Integer;
      status : Integer;
      len : size_t;
    
    begin
      mib[0] := CTL_HW;
      mib[1] := HW_NCPU;
    
      len := SizeOf(Result);
      status := fpSysCtl(PCInt(@mib), Length(mib), @Result, @len, Nil, 0);
      if status <> 0 then RaiseLastOSError;
    end;
    

The above code returns the number of CPUs in an integer; you need to check the C header file to determine what is being returned (an integer, int64, string, struct/record). 

There is a possible issue with the above because if you have, for example, an Intel CPU with hyperthreading. One guess. Yes, it will return the total number of threads and not the number of CPUs or even the number of physical cores. HW_NCPU is pretty much deprecated these days as a useful attribute. Instead, we can retrieve the number of CPU packages, physical CPUs (cores) or logical CPUs (the same as HW_NCPU). To do so requires the use of a second function 'FPsysctlbyname'. 

However, before we move on, let's look at retrieving a string with FPsysctl() which is a little different than retrieving an integer. 
    
    
    // The full source code for this example is available from:
    // https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/fpc_sysctls/hardware_model.pas
    ...
    Uses
      SysUtils, SysCtl, Unix;
    ...
    function HWmodel : AnsiString;
    
    var
      mib : array[0..1] of Integer;
      status : Integer;
      len : size_t;
      p   : PChar;
    
    begin
     mib[0] := CTL_HW;
     mib[1] := HW_MODEL;
    
     status := fpSysCtl(PCInt(@mib), Length(mib), Nil, @len, Nil, 0);
     if status <> 0 then RaiseLastOSError;
    
     GetMem(p, len);
    
     try
       status := fpSysCtl(PCInt(@mib), Length(mib), p, @len, Nil, 0);
       if status <> 0 then RaiseLastOSError;
       Result := p;
     finally
       FreeMem(p);
     end;
    end;
    

Notice that this time we needed two calls to FPsysctl() - one to find out the size of the buffer required to hold the string value, and the second to actually retrieve the string value into that buffer. On my Apple computer this returns the string "Macmini8,1" - the 2018 Mac mini. 

No, we're not quite ready to move on yet... we still need to see how to retrieve information in a C structure (Pascal record). 
    
    
    // The full source code for this example is available from:
    // https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/fpc_sysctls/swap_usage.pas
    ...
    Uses
      SysUtils, SysCtl, Unix;
    ...
    function SwapUsage: String;
    
    type
     swapinfo = record
        xsu_total    : Int64;
        xsu_avail    : Int64;
        xsu_used     : Int64;
        xsu_pagesize : Integer;
        xsu_encrypted: Boolean;
    end;
    
    const
      VM_SWAPUSAGE = 5;
    
    var
      mib : array[0..1] of Integer;
      status : Integer;
      len : size_t;
      swap : swapinfo;
      SwapEncrypted: String;
    
    begin
      mib[0] := CTL_VM;
      mib[1] := VM_SWAPUSAGE;
      swap := Default(swapinfo);
    
      FillChar(swap, sizeof(swap), 0);
      len := sizeof(swap);
    
      status := fpSysCtl(PCInt(@mib), Length(mib), @swap, @len, Nil, 0);
      if status <> 0 then RaiseLastOSError;
    
      if(swap.xsu_encrypted = true) then
        SwapEncrypted := 'Yes' else SwapEncrypted := 'No';
    
      Result := 'Swap total: ' + FloatToStr(Round(swap.xsu_total /1024 /1024)) + ' MB'
                + LineEnding + 'Swap used: ' + FloatToStr(Round(swap.xsu_used /1024 /1024)) + ' MB'
                + LineEnding + 'Swap free: ' + FloatToStr(Round(swap.xsu_avail /1024 /1024)) + ' MB'
                + LineEnding + 'Swap page size: ' + IntToStr(swap.xsu_pagesize) + ' bytes'
                + LineEnding + 'Swap encrypted: ' + SwapEncrypted + LineEnding;
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If your computer has not used any swap, then the results for Swap total, used and free will all be zero.

On my computer this returned: 
    
    
     Swap total: 1024 MB
     Swap used: 191 MB
     Swap free: 833 MB
     Swap page size: 4096 bytes
     Swap encrypted: Yes
    

The swapinfo record was pretty easily translated from the C structure found in the system `sysctl.h` file: 
    
    
    struct xsw_usage {
    	u_int64_t       xsu_total;
    	u_int64_t       xsu_avail;
    	u_int64_t       xsu_used;
    	u_int32_t       xsu_pagesize;
    	boolean_t       xsu_encrypted;
    };
    

We also defined the MIB constant VM_SWAPUSAGE by looking at the system `sysctl.h` file. Why? Because Free Pascal 3.2.0 and earlier are missing this identifier (fixed in FPC 3.2.2). So, we looked it up and found: 
    
    
    #define VM_SWAPUSAGE    5               /* total swap usage */
    

Finally, yes, it is time to move on to the FPsysctlbyname() function. 

### FPsysctlbyname

The FPsysctlbyname function is defined in '/usr/local/share/fpcsrc/rtl/bsd/sysctl.pp' as: 
    
    
    function FPsysctlbyname (Name: pchar; oldp:pointer;oldlenp:psize_t; newp:pointer;newlen:size_t):cint; cdecl; external name 'sysctlbyname';
    

There. Not really very different from the FPsysctl() function. Is that a sigh of relief? So let's look at finally retrieving the number of physical CPU cores. 
    
    
    // Full source code for this example is available from:
    // https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/fpc_sysctls/num_cores.pas 
    ...
    {$mode objfpc}
    ...
    
    Uses
      SysCtl, SysUtils, Unix;
    
    function NumberOfCores: Integer;
    
    var
      status : Integer;
      len : size_t;
    
    begin
      len := SizeOf(Result);
    
      status := fpSysCtlByName('hw.physicalcpu', @Result, @len, Nil, 0);
      if status <> 0 then RaiseLastOSError;
    end;
    

On my 2018 Mac mini this returns the correct number of cores: 6. Where do you find the special incantation **hw.physicalcpu** ? Yes, in the system `sysctl.h` file. Here's a brief excerpt of some of the hardware selectors: 
    
    
     These are the support HW selectors for sysctlbyname.  Parameters that are byte counts or frequencies are 64 bit numbers.
     All other parameters are 32 bit numbers.
     
     hw.memsize                - The number of bytes of physical memory in the system.
     hw.ncpu                   - The maximum number of processors that could be available this boot.
                                 Use this value for sizing of static per processor arrays; i.e. processor load statistics.
     hw.activecpu              - The number of processors currently available for executing threads.
                                 Use this number to determine the number threads to create in SMP aware applications.
                                 This number can change when power management modes are changed.
     hw.physicalcpu            - The number of physical processors available in the current power management mode.
     hw.physicalcpu_max        - The maximum number of physical processors that could be available this boot
     hw.logicalcpu             - The number of logical processors available in the current power management mode.
     hw.logicalcpu_max         - The maximum number of logical processors that could be available this boot
     hw.tbfrequency            - This gives the time base frequency used by the OS and is the basis of all timing services.
                                 In general is is better to use mach's or higher level timing services, but this value
                                 is needed to convert the PPC Time Base registers to real time.
     hw.cpufrequency           - These values provide the current, min and max cpu frequency.  The min and max are for
     hw.cpufrequency_max       - all power management modes.  The current frequency is the max frequency in the current mode.
     hw.cpufrequency_min       - All frequencies are in Hz.
     hw.busfrequency           - These values provide the current, min and max bus frequency.  The min and max are for
     hw.busfrequency_max       - all power management modes.  The current frequency is the max frequency in the current mode.
     hw.busfrequency_min       - All frequencies are in Hz.
     hw.cputype                - These values provide the mach-o cpu type and subtype.  A complete list is in <mach/machine.h>
     hw.cpusubtype             - These values should be used to determine what processor family the running cpu is from so that
                                 the best binary can be chosen, or the best dynamic code generated.  They should not be used
                                 to determine if a given processor feature is available.
     hw.cputhreadtype          - This value will be present if the processor supports threads.  Like hw.cpusubtype this selector
                                 should not be used to infer features, and only used to name the processors thread architecture.
                                 The values are defined in <mach/machine.h>
     hw.byteorder              - Gives the byte order of the processor.  4321 for big endian, 1234 for little.
     hw.pagesize               - Gives the size in bytes of the pages used by the processor and VM system.
     hw.cachelinesize          - Gives the size in bytes of the processor's cache lines.
                                 This value should be use to control the strides of loops that use cache control instructions
                                 like dcbz, dcbt or dcbst.
     hw.l1dcachesize           - These values provide the size in bytes of the L1, L2 and L3 caches.  If a cache is not present
     hw.l1icachesize           - then the selector will return an error.
     hw.l2cachesize            -
     hw.l3cachesize            -
     hw.packages               - Gives the number of processor packages.
    

If you prefer, you can instead use the `sysctl` command line utility in a Terminal to list all the hw selectors with the `sysctl hw` command. 

Retrieving a string value, rather than an integer, is again reminiscent of FPsysctl(). Two calls to FPsysctlbyname() are needed: (1) to find out the size of the buffer to hold the string to be returned and (2) to return the string value into that buffer. Don't forget to allocate memory for the buffer before the second call! 
    
    
    // Full source code for this example is available from:
    // https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/fpc_sysctls/brand_cpu.pas 
    ...
    {$mode objfpc}
    ...
    
    Uses
      SysCtl, SysUtils, Unix;
    
    function BrandOfCPU: AnsiString;
    
    var
      status : Integer;
      len : size_t;
      p : PChar;
    
    begin
      status := fpSysCtlByName('machdep.cpu.brand_string', Nil, @len, Nil, 0);
      if status <> 0 then RaiseLastOSError;
    
      GetMem(p, len);
    
      try
        status := fpSysCtlByName('machdep.cpu.brand_string', p, @len, Nil, 0);
        if status <> 0 then RaiseLastOSError;
        Result := p;
      finally
        FreeMem(p);
      end;
    end;
    

On my 2018 Mac mini computer, this returns the very informative string: _Intel(R) Core(TM) i7-8700B CPU @ 3.20GHz_. On my 2020 Mac mini computer, this returns the somewhat less informative string: _Apple M1_. 

Oh, you noticed, I changed the top level identifier from **hw** to **machdep** (machine dependent). Again, consult `sysctl.h` or use the `sysctl` command line utility in a Terminal. 
    
    
    // Full source code for this example is available from:
    // https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/fpc_sysctls/os_version.pas
    
    // Note: The kern.osproductversion attribute was added to the Darwin kernel in December 2018
    //       https://github.com/apple/darwin-xnu/commit/5bbb823c13f3ab1ab58878f96b35433a29882676
    
    Program os_version;
    
    {$mode objfpc}
    
    Uses
      SysCtl, SysUtils, Unix;
    
    function OS_version : AnsiString;
    
    var
       status : Integer;
       len	  : size_t;
       p	  : PChar;
    
    begin
       status := fpSysCtlByName('kern.osproductversion', Nil, @len, Nil, 0);
       if status <> 0 then RaiseLastOSError;
    
       GetMem(p, len);
    
       try
         status := fpSysCtlByName('kern.osproductversion', p, @len, Nil, 0);
         if status <> 0 then RaiseLastOSError;
         Result := p;
       finally
         FreeMem(p);
       end;
    end;
    
    begin
      WriteLn(OS_version);
    end.
    

The above FPC example program will return the operating system version. On my M1 processor Mac mini this was "11.2.3" (Big Sur). 

### Last words

For the optimised, performance fiends out there, the man page for these functions notes that the FPsysctl() function runs in about a third the time as the same request made via the FPsysctlbyname() function. 

It should be noted that many of the sysctl attributes are read-only, but there are also quite a few that can be changed. I've left changing a sysctl attribute as an exercise for you dear reader. It should be simple if you study the definitions of the function parameters, paying particular attention to newp and newlen. 

## NSProcessinfo

Each macOS process has a single, shared NSProcessInfo object, known as a process information agent. This process information agent can return information such as arguments, environment variables, host name, and process name. The processInfo class method returns the shared agent for the current process—that is, the process whose object sent the message. 

### Properties

Property | Description   
---|---  
processInfo | Returns the process information agent for the process. An NSProcessInfo object is created the first time this method is invoked, and that same object is returned on each subsequent invocation.   
arguments | Array of strings with the command-line arguments for the process.   
environment | The variable names (keys) and their values in the environment from which the process was launched.   
globallyUniqueString | Global unique identifier for the process. The global ID for the process includes the host name, process ID, and a time stamp, which ensures that the ID is unique for the network. This property generates a new string each time its getter is invoked, and it uses a counter to guarantee that strings created from the same process are unique.   
processIdentifier | The identifier of the process (often called process ID or PID).   
processName | The name of the process.   
userName | Returns the account name of the current user. macOS 10.12+   
fullUserName | Returns the full name of the current user. macOS 10.12+   
hostName | The name of the host computer on which the process is executing.   
operatingSystemVersionString | A string containing the version of the operating system on which the process is executing. macOS 10.2+   
operatingSystemVersion | The version of the operating system on which the process is executing. macOS 10.10+   
processorCount | The number of processing cores available on the computer. Equivalent to: `sysctl -n hw.ncpu`. macOS 10.5+   
activeProcessorCount | The number of active processing cores available on the computer. Equivalent to: `sysctl -n hw.logicalcpu`. macOS 10.5+   
physicalMemory | The amount of physical memory on the computer in bytes. macOS 10.5+   
systemUptime | The amount of time in seconds that the system has been awake since the last time it was restarted. macOS 10.6+   
thermalState | The current thermal state of the system: Nominal, Fair, Serious or Critical. macOS 10.10.3+   
automaticTerminationSupportEnabled | A Boolean value indicating whether the app supports automatic termination. Currently, setting this property to False has no effect. macOS 10.7+   
  
### Methods

Method | Description   
---|---  
isOperatingSystemAtLeastVersion | Returns a Boolean value indicating whether the version of the operating system on which the process is executing is the same or later than the given version. macOS 10.10+   
  
### Example code
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Dialogs, StdCtrls,
      ctypes,      // needed for cint
      typinfo,     // needed for GetEnumName
      CocoaAll;    // needed for NS...
    
    type
      // From HERE...
      NSProcessInfoThermalState = (
        Nominal,       // within the normal range; no corrective action is required.
        Fair,          // thermal state is slightly elevated and the fan is audible.
        Serious,       // fan is at maximum speed; performance may be affected; 
                       // applications should reduce use of CPU, GPU, and any I/O.
        Critical);     // thermal state severely affects performance; need immediate cooling;
                       // applications should reduce use of CPU, GPU, and any I/O to minimum
                       // required for user interaction; stop using peripherals such as cameras.
    
      NSOperatingSystemVersion = record
        majorVersion: NSInteger;
        minorVersion: NSInteger;
        patchVersion: NSInteger;
      end;
    
      NSProcessInfo = objcclass external (NSObject)
      private
        environment_: NSDictionary;
        arguments_: NSArray;
        hostName_: NSString;
        name: NSString;
        automaticTerminationOptOutCounter: NSInteger;
      public
        class function processInfo: NSProcessInfo; message 'processInfo';
        function environment: NSDictionary; message 'environment';
        function arguments: NSArray; message 'arguments';
        function hostName: NSString; message 'hostName';
        procedure setProcessName(newValue: NSString); message 'setProcessName:';
        function processName: NSString; message 'processName';
        function processIdentifier: cint; message 'processIdentifier';
        function globallyUniqueString: NSString; message 'globallyUniqueString';
        function operatingSystemVersionString: NSString; message 'operatingSystemVersionString';
        function userName: NSString; message 'userName';
        function fullUserName: NSString; message 'fullUserName';
        function operatingSystemVersion: NSOperatingSystemVersion; message 'operatingSystemVersion';
        function processorCount: NSUInteger; message 'processorCount';
        function activeProcessorCount: NSUInteger; message 'activeProcessorCount';
        function physicalMemory: culonglong; message 'physicalMemory';
        function isOperatingSystemAtLeastVersion (os_version: NSOperatingSystemVersion): Boolean; message 'isOperatingSystemAtLeastVersion:';
        function systemUptime: NSTimeInterval; message 'systemUptime';
        function thermalState: NSInteger; message 'thermalState';
        {procedure disableSuddenTermination; message 'disableSuddenTermination';
        procedure enableSuddenTermination; message 'enableSuddenTermination';
        procedure disableAutomaticTermination (reason: NSString); message 'disableAutomaticTermination:';
        procedure enableAutomaticTermination (reason: NSString); message 'enableAutomaticTermination:';}
        procedure setAutomaticTerminationSupportEnabled(newValue: Boolean); message 'setAutomaticTerminationSupportEnabled:';
        function automaticTerminationSupportEnabled: Boolean; message 'automaticTerminationSupportEnabled';
      end;
      // ... To HERE is ONLY NEEDED if you are using FPC 3.0.4 or earlier. FPC 3.2.0 does not need this section.
    
      { TForm1 }
    
      TForm1 = Class(TForm)
        Button1: TButton;
        Memo1: TMemo;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    
    { TForm1 }
    
    procedure ProcessInfo;
    var
       thermalState : String;
       myDictionary : NSDictionary;
       i            : Byte = 0;
       minOsVer     : NSOperatingSystemVersion;
    begin
        Form1.Memo1.Lines.Clear;
    
        Form1.Memo1.Lines.Add('Processor count: '
          + IntToStr(NSProcessInfo.ProcessInfo.processorCount)
          + LineEnding + 'Active processors: '
          + IntToStr(NSProcessInfo.ProcessInfo.activeProcessorCount)
          + LineEnding + 'Physical memory: '
          + FloatToStr(NSProcessInfo.ProcessInfo.physicalMemory / 1024 / 1024 / 1024) + 'GB'
          + LinEending + 'Process name: '
          + NSProcessInfo.ProcessInfo.processName.UTF8String
          + LinEending + 'PID: '
          + IntToStr(NSProcessInfo.ProcessInfo.processIdentifier)
          + LineEnding + 'Uptime: '
          + FormatFloat('0.00', NSProcessInfo.ProcessInfo.systemUptime / 60 / 60 / 24) + ' days'
          + LineEnding + 'Operating system: '
          + NSProcessInfo.ProcessInfo.operatingSystemVersionString.UTF8String
          + LineEnding
          + 'Major: ' + IntToStr(NSProcessInfo.ProcessInfo.OperatingSystemVersion.majorVersion)
          + ' Minor: ' + IntToStr(NSProcessInfo.ProcessInfo.OperatingSystemVersion.minorVersion)
          + ' Patch: ' + IntToStr(NSProcessInfo.ProcessInfo.OperatingSystemVersion.patchVersion)
          + LineEnding + 'Hostname: '
          + NSProcessInfo.ProcessInfo.hostName.UTF8String
          + LineEnding + 'Globally unique string: '
          + NSProcessInfo.ProcessInfo.globallyUniqueString.UTF8String
          + LineEnding + 'Username: '
          + NSProcessInfo.ProcessInfo.userName.UTF8String
          + LineEnding + 'Full Username: '
          + NSProcessInfo.ProcessInfo.fullUserName.UTF8String
          + LineEnding
         );
    
         myDictionary := NSProcessInfo.ProcessInfo.environment;
         for i := 0 to (myDictionary.count - 1) do
           begin
             Form1.Memo1.Lines.Add('Env Var: '
               + myDictionary.allKeys.objectAtIndex(i).UTF8String
               + ' = '
               + myDictionary.allValues.objectAtIndex(i).UTF8String
             );
           end;
    
         ThermalState := GetEnumName(TypeInfo(NSProcessInfoThermalState), Ord(NSProcessInfo.ProcessInfo.thermalState));
         Form1.Memo1.Lines.Add(LineEnding + 'Thermal state: ' + ThermalState
         );
    
         // Setup minimum OS version
         minOsVer.majorVersion:= 10;
         minOsVer.minorVersion:= 14;
         minOsVer.patchVersion:= 6;
    
         if(NSProcessInfo.ProcessInfo.isOperatingSystemAtLeastVersion(minOSVer)) then
           Form1.Memo1.Lines.Add(LineEnding + 'OS version is at least 10.14.6' + LineEnding)
         else
           Form1.Memo1.Lines.Add(LineEnding + 'OS version is NOT at least 10.14.6' + LineEnding)
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      processInfo;
    end;
    
    
    end.
    

## Gestalt Manager

![<translate> Warning: </translate>](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/OOjs_UI_icon_notice-destructive.svg/18px-OOjs_UI_icon_notice-destructive.svg.png) **Warning**|  Deprecated by Apple after macOS 10.7 (Lion) in favour of NSProcessInfo, but still useful if you need to support older operating system versions.  
---|---  
  
Despite Apple's deprecation of the Gestalt Manager in favour of the NSProcessInfo class or the SysCtl Interface, there are still occasions when you will need to use it. For example, the NSProcessInfo instance property `OperatingSystemVersion` is only available from macOS 10.10 (Yosemite) onwards according to Apple's documentation, although it seems to also work in macOS 10.9 (Mavericks). 

So what do you do if you want to check the version of the operating system across all versions back to Mac OS X 10.0 (Cheetah)? The example code below holds the solution. 

### Example: Retrieving OS version from Mac OS X 10.0+
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Dialogs, StdCtrls, ExtCtrls,
      MacOSAll,
      CocoaAll;
    
    type
    
      { TForm1 }
    
      TForm1 = Class(TForm)
        Memo1: TMemo;
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
      private
      public
    
      end;
    
    var
      Form1: TForm1;
      counter: Int64;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure ProcessInfo;
    begin
        Form1.Memo1.Lines.Clear;
    
        Form1.Memo1.Lines.Add('macOS version: '
          + IntToStr(NSProcessInfo.ProcessInfo.operatingSystemVersion.majorVersion)
          + '.'
          + IntToStr(NSProcessInfo.ProcessInfo.operatingSystemVersion.minorVersion)
          + '.'
          + IntToStr(NSProcessInfo.ProcessInfo.operatingSystemVersion.patchVersion)
        );
    end;
    
    Procedure GestaltInfo;
    var
        majorVersion: LongInt = 0;
        minorVersion: LongInt = 0;
        patchVersion: LongInt = 0;
    begin
        Form1.Memo1.Lines.Clear;
        Form1.Memo1.Lines.Add('Trying another way...');
    
        Gestalt(gestaltSystemVersionMajor, majorVersion);
        Gestalt(gestaltSystemVersionMinor, minorVersion);
        Gestalt(gestaltSystemVersionBugFix, patchVersion);
    
        Form1.Memo1.Lines.Add('macOS version: ' + IntToStr(majorVersion) + '.' +
          IntToStr(minorVersion) + '.' + IntToStr(patchVersion));
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      aSEL : SEL;
    begin
        // Check whether OS version responds to this selector
        aSEL := objcselector('operatingSystemVersion');
        if NSProcessInfo.processInfo.respondsToSelector(aSEL) then
          ProcessInfo
        // Otherwise fallback to the deprecated Gestalt Manager
        else
          GestaltInfo;
    end;
    
    end.
    

## IOKit

IOKit is Apple’s object-oriented framework for developing device drivers for macOS. While the main use of the IOKit framework is for developers creating device drivers that are to be resident in the kernel, it is also of use to application developers who can using an IOKit device interface to communicate with the Apple hardware. 

While the focus of this section is on accessing macOS system information, it should be noted that the IOKit framework has been deprecated by Apple in macOS 10.15 Catalina in favour of the DriverKit framework which debuted in that version of macOS. DriverKit provides a fully modernized replacement for IOKit to create device drivers. System extensions and drivers built with DriverKit run in user space, where they can't compromise the security or stability of macOS. 

### Example: Retrieving the platform serial number
    
    
    program project1;
    
    {$mode objfpc}{$H+}
    {$modeswitch cvar}
    {$linkframework IOKit}
    
    uses
      ctypes, Classes, MacOSAll;
    { you can add units after this };
    
    type
      kern_return_t =  cint; // if you use "integer" then you don't need "ctypes" unit
      io_object_t = mach_port_t;
      io_registry_entry_t = io_object_t;
    
      io_string_t = array [0..511] of ansichar;
      IOOptionBits = UInt32;
    
    const
      kIOPlatformSerialNumber = 'IOPlatformSerialNumber';
    
    var
      kIOMasterPortDefault:mach_port_t; cvar; external;
    
    function IORegistryEntryFromPath( masterPort: mach_port_t; const path: io_string_t): io_registry_entry_t; cdecl; external;
    function IORegistryEntryCreateCFProperty(entry: io_registry_entry_t; key: CFStringRef; allocator:CFAllocatorRef; options: IOOptionBits): CFTypeRef; cdecl; external;
    function IOObjectRelease(entry: io_registry_entry_t): kern_return_t; cdecl; external;
    
    function get_platform_serial_num: string;
    var
      ioRegistryRoot :io_registry_entry_t;
      serialnumCf         :CFStringRef;
      l :integer;
    begin
      ioRegistryRoot := IORegistryEntryFromPath(kIOMasterPortDefault, 'IOService:/');
      serialnumCf := CFStringRef (IORegistryEntryCreateCFProperty(ioRegistryRoot, CFSTR(kIOPlatformSerialNumber), kCFAllocatorDefault, 0));
      IOObjectRelease(ioRegistryRoot);
      SetLength(Result, 32);
      CFStringGetCString(serialnumCf, @Result[1], length(Result), kCFStringEncodingMacRoman);
      CFRelease(serialnumCf);
      l:=StrLen(@Result[1]);
      SetLength(Result, l);
    end;
    
    begin
     WriteLn('Platform serial number: ', get_platform_serial_num);
    end.
    

### Example: Retrieving the platform UUID

A UUID (Universally Unique Identifier) is a 16-byte long number. It is normally expressed in a standard format of characters representing hexadecimal digits. UUIDs were formerly known as Globally Unique Identifiers (GUIDs), but are now officially Universally Unique Identifiers, or UUIDs, and are governed by Open Software Foundation, ISO/IEC, and IETF standards. 
    
    
    program project1;
    
    {$mode objfpc}{$H+}
    {$modeswitch cvar}
    {$linkframework IOKit}
    
    uses
      ctypes, Classes, MacOSAll
    { you can add units after this };
    
    type
      kern_return_t =  cint; // if you use "integer" then you don't need "ctypes" unit
      io_object_t = mach_port_t;
      io_registry_entry_t = io_object_t;
    
      io_string_t = array [0..511] of ansichar;
      IOOptionBits = UInt32;
    
    const
      kIOPlatformUUIDKey = 'IOPlatformUUID';
    
    var
      kIOMasterPortDefault:mach_port_t; cvar; external;
    
    function IORegistryEntryFromPath( masterPort: mach_port_t; const path: io_string_t): io_registry_entry_t; cdecl; external;
    function IORegistryEntryCreateCFProperty(entry: io_registry_entry_t; key: CFStringRef; allocator:CFAllocatorRef; options: IOOptionBits): CFTypeRef; cdecl; external;
    function IOObjectRelease(entry: io_registry_entry_t): kern_return_t; cdecl; external;
    
    function get_platform_uuid: string;
    var
      ioRegistryRoot :io_registry_entry_t;
      uuidCf         :CFStringRef;
      l :integer;
    begin
      ioRegistryRoot := IORegistryEntryFromPath(kIOMasterPortDefault, 'IOService:/');
      uuidCf := CFStringRef (IORegistryEntryCreateCFProperty(ioRegistryRoot, CFSTR(kIOPlatformUUIDKey), kCFAllocatorDefault, 0));
      IOObjectRelease(ioRegistryRoot);
      SetLength(Result, 1024);
      CFStringGetCString(uuidCf, @Result[1], length(Result), kCFStringEncodingMacRoman);
      CFRelease(uuidCf);
      l:=StrLen(@Result[1]);
      SetLength(Result, l);
    end;
    
    begin
      WriteLn('Platform UUID: ', get_platform_uuid);
    end.
    

## External links

  * [Apple: SysCtl](<https://developer.apple.com/documentation/installerjs/system/1812308-sysctl>)
  * [Apple: NSProcessinfo](<https://developer.apple.com/documentation/foundation/nsprocessinfo>)
  * [Apple: Gestalt Manager](<https://developer.apple.com/documentation/coreservices/carbon_core/gestalt_manager>)
  * [Apple: Introduction to I/O Kit Fundamentals](<https://developer.apple.com/library/archive/documentation/DeviceDrivers/Conceptual/IOKitFundamentals/Introduction/Introduction.html>)
  * [Apple: IOKit framework](<https://developer.apple.com/documentation/iokit>)
  * [Apple: DriverKit framework](<https://developer.apple.com/documentation/driverkit>)
  * [Apple: Uniquely Identifying a Macintosh Computer](<https://developer.apple.com/library/archive/technotes/tn1103/_index.html#//apple_ref/doc/uid/DTS10002943>)

---

_Source: [https://wiki.freepascal.org/Accessing_macOS_System_Information](https://web.archive.org/web/20240121085952/https://wiki.freepascal.org/Accessing_macOS_System_Information)_
