# Profiling

│ **English (en)** │

**Profiling** is a performance analysis technique that can be used to find bottlenecks in your application. In particular it encompasses measuring the frequency and duration of function calls. 

## Contents

  * 1 Profiling and optimization
  * 2 Free Pascal profiler support
  * 3 Using Valgrind/Callgrind
  * 4 Using KCacheGrind
  * 5 Memory Profiling
    * 5.1 Heaptrc, LineInfo
    * 5.2 Valgrind Memcheck
  * 6 On macOS
    * 6.1 Instruments
    * 6.2 Shark
    * 6.3 spindump
    * 6.4 Valgrind
    * 6.5 FPProfiler
  * 7 Simple profiling using ifdefs and timing routines



## Profiling and optimization

Profiling can help to identify “critical” parts of your code, whose optimization could seriously improve the performance of your program. It is done best at times when you are preparing for a release and having a feature-freeze for your code. It is a waste of time and resources to optimize something that will be thrown away the next day during some code restructuring. 

Be careful not to over-optimize your program. It is seldom worth it to spent days on getting only a 2% performance gain. It is mostly cutting down run-time with 30% in just half an hour of coding a few optimizations that will pay off. 

Of course optimizing code that is reused very often is worth a little more effort for performance gain. 

## Free Pascal profiler support

Free Pascal has built-in support for generating executables suitable for (at least) two profilers: 

  * **gprof** , the Gnu Profiler: enable with `-pg`. 
    * **Availability** : Most FPC target platforms
    * **Notes** : 
      * Support for using gprof under Linux is broken in FPC 2.2.0-2.4.0; it is fixed in FPC 2.6+
      * Under Windows, additional libraries are required to get gprof support working;
  * **Valgrind** , using the plug-in Callgrind: enable with `-gv`. 
    * **Availablility** : Linux, Mac OS X 10.5 (10.6 in Valgrind development versions), and some other UNIXes.
    * **Notes**
      * Should work fine on all platforms supported by Valgrind, as it does not require any compiler support (other than that the profiled program should be dynamically linked, and that the compiler should not generate certain kinds of debug information -- both issues are taken care of by the `-gv` option)
      * The `-gv` option implicitly includes the [CMem](<CMem.md> "CMem") unit; thus you need to remove it from your uses clause



There's also an FPC native profiler project called [FPProfiler](<FPProfiler.md> "FPProfiler"). Because the profiler is native to FPC it should work on all platforms that FPC supports. 

A Lazarus IDE addon which enables you to profile your project with one click is also available: [LazProfiler](<LazProfiler.md> "LazProfiler")

## Using Valgrind/Callgrind

Valgrind was originally only a memory debugger, but now it also supports e.g. profiling using the Callgrind plug-in (which has replaced the Calltree plugin). To install Valgrind with Callgrind on Debian, run e.g. 
    
    
     aptitude install valgrind valgrind-callgrind
    

To profile your program, you need to do three steps: 

  * Compile your program with the `-gv` switch.
  * Run your program using `valgrind`
  * Analyze the output with `valgrind_annotate` or the GUI tool `KCacheGrind`



Compile your program with 
    
    
     fpc -gv myprogram.pas
    

Run Valgrind using e.g. 
    
    
     valgrind --tool=callgrind ./myprogram --options --to --my --program
    

You can also profile only specific parts of your program. For example you can start the program with 
    
    
     valgrind --tool=callgrind --instr-atstart=no ./myprogram --options --to --my --program
    

At the moment you want actual profiling to start, run 
    
    
     callgrind_control -i on
    

To stop the profiling, run the same command with `off` instead of `on`. To get only profiling information when you are inside a particular function, you can pass the `--toggle-collect=funcNamePrefix` option to valgrind. Refer to the Valgrind documentation for further details. 

Callgrind has created a trace file with a typical name like `callgrind.out.12345`. To get a human-readable statistical report, run e.g. 
    
    
     callgrind_annotate callgrind.out.12345
    

Of course you can also experiment with the other tool plug-ins that ship with Valgrind. Cachegrind, for example, profiles the cache use. Other plug-ins can be used to profile or debug memory allocation or detect data race conditions. 

## Using KCacheGrind

Though the output of `callgrind_annotate` can definitely shed some light on the performance characteristics of your program, the GUI tool KCacheGrind can give you even more interesting information, presented in different forms: caller/callee lists, caller/callee maps ans call graphs. To install it on Debian, run 
    
    
     aptitude install kcachegrind graphviz
    

(The `graphviz` package is required if you want KCacheGrind to generate call graphs.) 

[![](https://wiki.freepascal.org/images/0/0a/KCacheGrind_Lazarus_Screenshot.png)](</File:KCacheGrind_Lazarus_Screenshot.png>)

[](</File:KCacheGrind_Lazarus_Screenshot.png> "Enlarge")

Example of KCacheGrind showing profiling information for Lazarus (Click to enlarge)

Run KCachegrind with a simple call to `kcachegrind`. You can now open a trace file using the GUI and all should be self-explanatory. 

On the left, you will see a 'flat profile', which list for each function (in the top 100) 

  * Incl: amount of time spent in the function, _including_ callees (functions called by the function).
  * Self: amount of time spent within the function itself, _excluding_ callees.
  * Called: the number of the times the function has been called.



_Note:_ if you click on one of the column headers to sort the list, you might need to do a reload (F5) to ensure that the top 100 _with respect to that column_ will be shown. Another hint: in the configuration options, you can make the list longer than only the top 100. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** "Time" in valgrind/callgrind is based on instructions/cycles executed. It is not exact. A CPU executing a function in real life (outside the profiler) may sometimes spent less time for more cycles.

On the right, you'll see a number of tabs, allowing you to see lists and maps of callers and callees, as well as a caller graph, which is very useful to see in a glance who calls whom the most. 

## Memory Profiling

### Heaptrc, LineInfo

To spot memory leaks, simply compile with -gh. This will compile as if you added the **heaptrc** unit as first unit to the uses section of your program. Heaptrc links into the memory manager of fpc and checks various things. Together with the -gl option (adding the lineinfo unit) it will give you useful information and backtraces, where a memory block was allocated. It supports some options that can be controlled via environment option _HEAPTRC_ containing keywords: 

  * keepreleased: Any freed memory, is kept and checked if overwritten.
  * disabled: disable tracing. This is useful for program compiled with heaptrc and should work normally.
  * nohalt: normally heaptrc stops on the first error it finds. Add this keyword if the program should continue (as it would normally do without heaptrc).
  * haltonnotreleased: Normally heaptrc prints for every not freed block a backtrace, which can take a long time. Add this keyword to stop after first reported mem leak.
  * log=<filename>: Instead of writing to stdout, you can give a log filename.



Note: **heaptrc** will conflict with **cmem** if you have to debug external libraries. Valgrind should be used in this case. 

### Valgrind Memcheck

There are mem leaks which heaptrc does not find. The valgrind program has a tool called memcheck, which does a lot more bookkeeping than heaptrc and finds some more bugs. The drawback is that valgrind is snail slow, so do this only if heaptrc could not help you. Here is an example: 
    
    
     valgrind --tool=memcheck --leak-check=summary --log-file=log.txt ./project1
    

The log.txt will almost always find something, because it tries to check the used external libraries too and some common libraries are badly coded. 

To get even more information run with 
    
    
     valgrind --tool=memcheck --leak-check=full --log-file=log.txt --show-reachable=yes --num-callers=50 ./project1
    

How to run massif: 
    
    
    valgrind --tool=massif --run-libc-freeres=no  ./project1
    

## On macOS

Most of what is said above for Linux also applies on macOS. gprof is however not supported by FPC on Darwin/i386 and on Darwin/ARM at this time. 

### Instruments

Instruments is a powerful performance analysis and testing tool that's part of Xcode toolset. Instruments can help you profile your apps on all platforms - iOS, macOS, watchOS, tvOS - in order to better understand and optimize their behavior and performance. 

From Xcode's product menu, choose Profile. Although Instruments is embedded within and is often used with Xcode, Instruments is a separate app which may be used independently as needed. For example: 
    
    
    open -a /Applications/Xcode.app/Contents/Applications/Instruments.app
    

Apple documentation links: 

  * [Instruments Help](<https://help.apple.com/instruments/mac/current/>).
  * [Getting Started with Instruments](<https://developer.apple.com/videos/play/wwdc2019/411/>) \- WWDC 2019 Video
  * [Creating Custom Instruments](<https://developer.apple.com/videos/play/wwdc2018/410/>) \- WWDC 2018 Video



### Shark

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Since macOS Lion 10.7, Shark is not on the Apple site any more and was replaced by Instruments above.

Shark is a sampling-based profiler. It supports time profiles, profiles based on performance counters (such as L2 cache misses), application-specific profiles and whole-system profiles. By default, it's installed at /Developer/Applications/Performance Tools/Shark.app 

If you compile the profiled code with debug information (either Stabs or DWARF), Shark will display the (Pascal) source code in addition to the assembler code. 

Shark only works for i386 and PowerPC. For 64 bit platforms, you have to use Instruments.app instead, which offers similar functionality. 

### spindump

The macOS `spindump` utility can be used with the `-timeline` option to sample and profile your application in order to determine which QoS class applies as a specific portion of code executes at a given time. For more details, see the manual page by opening an Applications > Utilities > Terminal and typing `man spindump`. 

### Valgrind

Valgrind has been ported to AMD64/macOS 10.12 (Sierra). There is also preliminary support for X86/macOS 10.13, AMD64/macOS 10.13 (High Sierra). For current details, check valgrind's [homepage](<http://valgrind.org/>). 

If you experience the following issue: 
    
    
     valgrind: ./test: cannot execute binary file
    

Chances are that your valgrind only supports 64 bit while your FPC program is compiled for 32 bit. To make your application compatible simply add the switch **-Px86_64**. 

### FPProfiler

FPProfiler has been ported to macOS as a fink package. For details see here: [FPProfiler](<FPProfiler.md> "FPProfiler")

## Simple profiling using ifdefs and timing routines

Another, simpler, way of profiling is simply adding ifdefs and time counting. For counting the time the simples way is just using Now() from SysUtils. Other possibilities are NowUTC() from lazutf8sysutils or [EpikTimer](<EpikTimer.md> "EpikTimer"). 

For example, to profile the following procedure: 
    
    
    procedure TMyClass.DoSomething;
    begin
      ReallyDoSomething;
    end;
    

One would add the following extra code around it: 
    
    
    function DateTimeToMilliseconds(aDateTime: TDateTime): Int64;
    var
      TimeStamp: TTimeStamp;
    begin
      {Call DateTimeToTimeStamp to convert DateTime to TimeStamp:}
      TimeStamp:= DateTimeToTimeStamp (aDateTime);
      {Multiply and add to complete the conversion:}
      Result:= TimeStamp.Time;
    end;
    
    procedure TMyClass.DoSomething;
    var
      {$IFDEF VerboseProfiler}
      lTimeStart: TDateTime;
      {$ENDIF}
      ...
    begin
      {$IFDEF VerboseProfiler}
      lTimeStart := Now();
      {$ENDIF}
    
      ReallyDoSomething;
    
      {$IFDEF VerboseProfiler}
      DebugLn(Format('[TMyClass.DoSomething] Duration: %d ms', [DateTimeToMilliseconds(Now() - lTimeStart)]));
      {$ENDIF}
    end;
    

Activate the define to get the debug info in the [DebugLn](<DebugLn.md> "DebugLn") target.

---

_Source: [https://wiki.freepascal.org/Profiling](https://web.archive.org/web/20250323133026/https://wiki.freepascal.org/Profiling)_
