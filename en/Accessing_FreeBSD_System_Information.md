# Accessing FreeBSD System Information

│ **English (en)** │

[Sysctl](<https://www.freebsd.org/cgi/man.cgi?query=sysctl&sektion=3>) provides an interface that allows you to read and, with appropriate privileges, set many kernel attributes in FreeBSD (the BSDs, macOS and Linux). This provides a wealth of information detailing system hardware and configuration attributes which can be useful when debugging or simply optimising your application (eg to take advantage of threads on multi-core CPU systems). 

The '/usr/local/share/fpcsrc/rtl/freebsd/sysctlh.inc' source file is a partial translation of the FreeBSD '/usr/include/sys/sysctl.h' file. For details of the sysctls available, start a console or X11 xterm and type 'man sysctl'. Typing 'sysctl -a | more' will list all the available sysctls and their values. 

## Contents

  * 1 FPsysctl()
    * 1.1 Example: Number of CPUs
    * 1.2 Example: CPU model
    * 1.3 Example: Clock info
    * 1.4 Example: Real executable path
  * 2 FPsysctlbyname
    * 2.1 Example: Number of CPU cores
    * 2.2 Example: CPU architecture
    * 2.3 Example: CPU clock speed
  * 3 Last words
  * 4 External links



## FPsysctl()

The fpSysCtl() function allows you to retrieve system information. The data returned from the function comprises integers (integer and int64), strings (AnsiStrings) and C structures (records). The function is defined in '/usr/local/share/fpcsrc/rtl/bsd/sysctl.pp' as: 
    
    
    function FPsysctl (Name: pchar; namelen:cuint; oldp:pointer;oldlenp:psize_t; newp:pointer;newlen:size_t):cint; cdecl; external name 'sysctl';
    

_name_ \-- a pointer to a Management Information Base (MIB) array of integers. Each element of this array must contain the correct value to read or write the particular system attribute. 

_namelen_ \-- the length of the array of integers in name. 

Note: if the old attribute value is not required, the next two variables can be set to Nil and 0. 

_oldp_ \-- a pointer to the buffer into which the value is copied. 

_oldlenp_ \-- the size of the oldp buffer. 

Note: if the new attribute value is not being set, the next two variables should be set to Nil and 0. 

_newp_ \-- a pointer to the buffer containing the new value to be set. 

_newlen_ \-- the size of the newp buffer. 

  
The size of the MIB array depends on the data to be read or written. The first element indicates the level of the information and the subsequent elements indicate the value to read. The top level names are: 
    
    
    	-------------------------------------------------------------------------------
    	Name                     Value    Next level names    Description
    	                                  are found in
    	-------------------------------------------------------------------------------
    	#define CTL_KERN         1        sys/sysctl.h        /* High kernel limits */
    	#define CTL_VM           2        vm/vm_param.h       /* Virtual memory */
    	#define CTL_VFS          3        sys/mount.h         /* File system */
    	#define CTL_NET          4        sys/socket.h        /* Networking */
    	#define CTL_DEBUG        5        sys/sysctl.h        /* Debugging */
    	#define CTL_HW           6        sys/sysctl.h        /* Generic CPU, I/O */
    	#define CTL_MACHDEP      7        sys/sysctl.h        /* Machine dependent */
    	#define CTL_USER         8        sys/sysctl.h        /* User-level */
    	#define CTL_P1003_1B     9        sys/sysctl.h        /* POSIX 1003.1B */
    	-------------------------------------------------------------------------------
    

Let's look at CTL_HW level of information. The subsequent elements are found in the system sysctl.h file and are: 
    
    
    	-------------------------------------------------------------------------------          
    	Name                     Value          Description
    	-------------------------------------------------------------------------------          
    	#define HW_MACHINE       1              /* string: machine class */
    	#define HW_MODEL         2              /* string: specific machine model */
    	#define HW_NCPU          3              /* int: number of cpus */
    	#define HW_BYTEORDER     4              /* int: byte order (4321 or 1234) */
    	#define HW_PHYSMEM       5              /* int: total memory in bytes */
    	#define HW_USERMEM       6              /* int: memory in bytes not wired */
    	#define HW_PAGESIZE      7              /* int: software page size */
    	#define HW_DISKNAMES     8              /* strings: disk drive names */
    	#define HW_DISKSTATS     9              /* struct: diskstats[] */
    	#define HW_FLOATINGPT   10              /* int: has HW floating point? */
    	#define HW_MACHINE_ARCH 11              /* string: machine architecture */
    	#define HW_REALMEM      12              /* int: 'real' memory */
    	-------------------------------------------------------------------------------
    

### Example: Number of CPUs

Armed with this information, we can now find out the number of CPUs: 
    
    
    {$mode objfpc}
    ...
    
    Uses
      unix,
      sysctl,     // fpSysCtl
      sysutils;   // RaiseLastOSError
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
      status := fpSysCtl(PChar(@mib), Length(mib), @Result, @len, Nil, 0);
      if status <> 0 then RaiseLastOSError;
    end;
    

The above code returns the number of CPUs in an integer; you need to check the C header file to determine what is being returned (an integer, int64, string, struct/record). 

There is a possible issue with the above because if you have, for example, a multi-core CPU. One guess. Yes, it will return the total number of cores and, if your microprocessor supports hyperthreading, the number of cores plus the number of hardware threads -- not the number of discrete CPUs. This may not matter depending on your use case, but you should be aware of it. 

### Example: CPU model

Before we move on to look at a second function 'FPsysctlbyname', let's look at retrieving a string with FPsysctl() which is a little different than retrieving an integer. 
    
    
    {$mode objfpc}
    ...
    
    Uses
      unix,
      sysctl,     // fpSysCtl
      sysutils;   // RaiseLastOSError
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
    
     status := fpSysCtl(PChar(@mib), Length(mib), Nil, @len, Nil, 0);
     if status <> 0 then RaiseLastOSError;
    
     GetMem(p, len);
    
     try
       status := fpSysCtl(PChar(@mib), Length(mib), p, @len, Nil, 0);
       if status <> 0 then RaiseLastOSError;
       Result := p;
     finally
       FreeMem(p);
     end;
    end;
    

Notice that this time we needed two calls to FPsysctl() - one to find out the size of the buffer required to hold the string value to be returned, and the second to actually retrieve the string value into that buffer. On my FreeBSD Apple Mac mini computer this returns the string "Intel(R) Core(TM) i7-8700B CPU @ 3.20GHz". 

### Example: Clock info

No, we're not quite ready to move on yet... we still need to see how to retrieve information in a C structure (Pascal record). 
    
    
    {$mode objfpc}
    ...
    
    Uses
      unix,
      sysctl,     // fpSysCtl
      sysutils;   // RaiseLastOSError
    ...
    
    function GetClockInfo: String;
    
    type
     clockinfo = record
        hz      : Integer;  // clock frequency
        tick    : Integer;  // ms per Hz tick
        spare   : Integer;  // unused
        stathz  : Integer;  // statistics clock frequency
        profhz  : Integer;  // profiling clock frequency
      end;
    
    var
     mib : array[0..1] of Integer;
     status : Integer;
     len : size_t;
     clock : clockinfo;
    begin
     mib[0] := CTL_KERN;
     mib[1] := KERN_CLOCKRATE;
     clock := Default(clockinfo);
    
     FillChar(clock, sizeof(clock), 0);
     len := sizeof(clock);
    
     status := fpSysCtl(@mib, Length(mib), @clock, @len, Nil, 0);
     if status <> 0 then RaiseLastOSError;
    
     Result := 'Clock freq: ' + IntToStr(clock.hz) + 'Hz' + LineEnding
               + 'Ms per Hz tick: ' + IntToStr(clock.tick) + LineEnding
               + 'Profiling clock freq: ' + IntToStr(clock.profhz) + ' Hz'
               + LineEnding;
    end;
    

On my computer this returned: 
    
    
    Clock freq: 100Hz
    Ms per Hz tick: 10000
    Profiling clock freq: 8128 Hz
    

The clockinfo record was pretty easily translated into a Pascal record from the C structure found in the system time.h file: 
    
    
    struct clockinfo {
            int     hz;             /* clock frequency */
            int     tick;           /* micro-seconds per hz tick */
            int     spare;
            int     stathz;         /* statistics clock frequency */
            int     profhz;         /* profiling clock frequency */
    };
    

### Example: Real executable path

First some background. Many Pascal programmers are in love with the `ExtractFilePath(ParamStr(0))` function to find the path of an executable. **Just don't do it!** Paramstr(0) must only be used on DOS and Windows (and maybe OS/2). On all other platforms the results are unpredictable and it will not work as you expect (in the best case it will never work; in the worst case it will only fail under certain circumstances, resulting in hard-to-find bugs). For the gory details, see [this thread's posts by Jonas](<https://lists.freepascal.org/pipermail/fpc-pascal/2009-February/020211.html>). 

Instead, sysctl to the rescue. 
    
    
    {$mode objfpc}
    
    Uses
      unix,
      sysctl,    // fpSysCtl
      sysutils;  // RaiseLastOSError
    
    function realPath: string;
    
    var
      mib: array[0..3] of Integer;
      status : Integer;
      len : size_t;
    
    begin
      mib[0] := CTL_KERN;
      mib[1] := KERN_PROC;
      mib[2] := KERN_PROC_PATHNAME;
      mib[3] :=  -1;
    
      len := SizeOf(Result);
      status := fpSysCtl(PChar(@mib), Length(mib), @Result, @len, Nil, 0);
      if status <> 0 then RaiseLastOSError;
    end;
    
    begin
      WriteLn(realPath);
    end.
    

Finally, yes, it is time to move on to the FPsysctlbyname() function. 

## FPsysctlbyname

The FPsysctlbyname function is defined in '/usr/local/share/fpcsrc/rtl/bsd/sysctl.pp' as: 
    
    
    function FPsysctlbyname (Name: pchar; oldp:pointer;oldlenp:psize_t; newp:pointer;newlen:size_t):cint; cdecl; external name 'sysctlbyname';
    

There. Not really very different from the FPsysctl() function. Is that a sigh of relief? 

### Example: Number of CPU cores

So let's look at finally retrieving the number of CPU cores. 
    
    
    {$mode objfpc}
    ...
    
    Uses
      unix,
      sysctl,   // fpSysCtlByName
      sysutils; // RaiseLastOSError
    ...
    
    function NumberOfCPU: Integer;
    
    var
      status : Integer;
      len : size_t;
    
    begin
      len := SizeOf(Result);
    
      status := fpSysCtlByName('hw.ncpu', @Result, @len, Nil, 0);
      if status <> 0 then RaiseLastOSError;
    end;
    

On my FreeBSD computer this returns the number of cores: 6. Where do you find the special incantation 'hw.ncpu' ? In the manual pages for the sysctl command (_man 3 sysctl_ [Library functions section] and _man 8 sysctl_ [System manager's section]). Here's a brief excerpt of the hardware selectors: 
    
    
        Parameters that are byte counts are 64 bit numbers. All other byte parameters are 32 bit numbers.
    
    
    
        --------------------------------------------------------------------------------------------
        Descriptor                  Type         Changeable    Description
        --------------------------------------------------------------------------------------------
        hw.machine                  string        no           Machine class
        hw.model                    string        no           Machine model
        hw.ncpu                     integer       no           Number of cpus
        hw.byteorder                integer       no           Byteorder (4321 or 1234)
        hw.physmem                  integer       no           Bytes of physical memory
        hw.usermem                  integer       no           Bytes of non-kernel memory
        hw.pagesize                 integer       no           Software page size
        hw.floatingpoint            integer       no           Non-zero if hardware floating point
        hw.machine_arch             string        no           Machine dependent architecture
        hw.realmem                  integer       no           Bytes of real memory
        --------------------------------------------------------------------------------------------
    

If you prefer, you can also use the sysctl command line utility in a console or X11 xterm to list all the hw selectors with "sysctl hw.". 

### Example: CPU architecture

Retrieving a string value, rather than an integer, is again reminiscent of FPsysctl(). Two calls to FPsysctlbyname() are needed: (1) to find out the size of the buffer to hold the string to be returned and (2) to return the string value into that buffer. Don't forget to allocate memory for the buffer before the second call! 
    
    
    {$mode objfpc}
    ...
    
    Uses
      unix,
      sysctl,   // fpSysCtlByName
      sysutils; // RaiseLastOSError
    ...
    
    function GetCPUarchitecture: AnsiString;
    
    var
      status : Integer;
      len : size_t;
      p : PChar;
    begin
      status := fpSysCtlByName('hw.machine_arch', Nil, @len, Nil, 0);
      if status <> 0 then RaiseLastOSError;
    
      GetMem(p, len);
    
      try
        status := fpSysCtlByName('hw.machine_arch', p, @len, Nil, 0);
        if status <> 0 then RaiseLastOSError;
        Result := p;
      finally
        FreeMem(p);
      end;
    end;
    

On my FreeBSD computer, this returns the string: amd64. 

### Example: CPU clock speed
    
    
    {$mode objfpc}
    ...
    
    Uses
      unix,
      sysctl,   // fpSysCtlByName
      sysutils; // RaiseLastOSError
    ...
    
    function GetCPUClockRate: integer;
    
    var
      status : Integer;
      len : size_t;
    
    begin
      len := SizeOf(Result);
      status := fpSysCtlByName('hw.clockrate', @Result, @len, Nil, 0);
      if status <> 0 then RaiseLastOSError;
    end;
    

For an Intel(R) Core(TM) i7-2620M CPU @ 2.70GHz, this returns 2.693 (GHz). 

## Last words

For the optimised, performance fiends out there, the man page for these functions notes that the FPsysctl() function runs in about a third the time as the same request made via the FPsysctlbyname() function. 

It should be noted that many of the sysctl attributes are read-only, but there are also quite a few that can be changed. I've left changing a sysctl attribute as an exercise for you dear reader. It should be simple if you study the definitions of the function parameters, paying particular attention to newp and newlen and the manual page for sysctl (man 3 sysctl). 

## External links

  * [sysctlbyname improved](<https://gitlab.com/alfix/sysctlbyname-improved>) \- A new sysctl internal object to convert a sysctl name to the corresponding OID and to improve sysctlbyname(3).

---

_Source: [https://wiki.freepascal.org/Accessing_FreeBSD_System_Information](https://web.archive.org/web/20250401022158/https://wiki.freepascal.org/Accessing_FreeBSD_System_Information)_
