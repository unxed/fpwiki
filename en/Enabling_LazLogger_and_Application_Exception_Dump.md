# Enabling LazLogger and Application Exception Dump

Since the release of Lazarus 2.0.0, LCL applications by default do no longer have LazLogger enabled, nor do they do an application exception dump (using debugln) when an application crashes (see: [Lazarus_2.0.0_release_notes#No_default_LazLogger](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") and [Lazarus_2.0.0_release_notes#No_LCL_Application_exception_dump](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") for an explantion why).  


Especially for the debug build mode of your application, it is very usefull to have these two features enabled.  
In [this section of the 2.0 releae notes](<Lazarus_2.0.md>) it is suggested to add `-FaLCLExceptionStacktrace,LazLogger` to "Additions and Overrides" of the Compiler Options of the debug build mode.  
While this works just fine on Windows, on Linux and Amiga (at least) this will cause your application to crash, since these units need a threadmanger to be active and using the "-Fa" option for the compiler wil load these units before any uses clause is parsed, so no threadmanager will activated yet.  
  
A different solution is to create a dedicated unit for the sole purpose of initializing application exception dumping and logging using debugln.  
In this unit you will then make sure a threadmanager is used (as the first unit in the uses clause).  
  
Example: 
    
    
    unit LCLDebug;
    {$mode objfpc}
    {$h+}
    
    {
      Activates logging and backtrace capabilities for Lazarus Applications
    
      The recommended way to use this is by using the -Fa paramter for the compiler:
      -FaLCLDebug
    
      This can be set in Project Options -> Cmpiler Options -> Custom Options in Lazarus IDE.
      The idea is that you uses this in the "debug" build mode.
    
      You should use the LazLoggerBase unit in any unit from your program that uses the
      Debugln() procedure: it won't output anything by default.
      (Using LazLogger unit overrides this behaviour then).
    
      Copyright (C) 2025 by FlyingSheep Inc. and Bart Broersma
    }
    
    interface
    
    uses
      {$IFDEF UNIX}
      cthreads,
      {$ENDIF}
      {$IFDEF HASAMIGA}
      athreads,
      {$ENDIF}
      LazLogger,              //enables debugln() etc
      LCLExceptionStacktrace; //enables backtracing on exceptions
    implementation
    
    initialization
    //empty section to fool the compiler that the unit actually does something, so it doesn't emit the "unit LCLDebug not used in ..." hint.
    
    end.
    

Now, in the compiler options for your debug build mode, you go to "Custom Options" and add "-FaLCLDebug" (without the quotes).  
[![custom options lcldebug.png](https://wiki.freepascal.org/images/5/53/custom_options_lcldebug.png)](</File:custom_options_lcldebug.png>)

---

_Source: [https://wiki.freepascal.org/Enabling_LazLogger_and_Application_Exception_Dump](https://web.archive.org/web/20260105041251/https://wiki.freepascal.org/Enabling_LazLogger_and_Application_Exception_Dump)_
