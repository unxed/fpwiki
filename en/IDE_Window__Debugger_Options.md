# IDE Window: Debugger Options

│ [**Deutsch (de)**](</IDE_Window:_Debugger_Options/de> "IDE Window: Debugger Options/de") │  **English (en)** │  [**español (es)**](</IDE_Window:_Debugger_Options/es> "IDE Window: Debugger Options/es") │  [**français (fr)**](</IDE_Window:_Debugger_Options/fr> "IDE Window: Debugger Options/fr") │  [**português (pt)**](</IDE_Window:_Debugger_Options/pt> "IDE Window: Debugger Options/pt") │  [**русский (ru)**](<../ru/IDE_Window__Debugger_Options.md> "IDE Window: Debugger Options/ru") │    
****  
****

## Contents

  * 1 General
    * 1.1 Debugger type and path
    * 1.2 Additional search path
    * 1.3 Debugger general options
    * 1.4 Debugger specific options
      * 1.4.1 GNU debugger (gdb)
  * 2 Event Log
    * 2.1 General
    * 2.2 Messages
  * 3 Language Exceptions
    * 3.1 Ignore these exceptions
    * 3.2 Break on Lazarus Exceptions
  * 4 OS Exceptions
    * 4.1 Signals
  * 5 See also



## General

This article describes the settings in the Tools/Options menu related to debugging. 

[![Debugger options general.png](https://wiki.freepascal.org/images/8/8b/Debugger_options_general.png)](</File:Debugger_options_general.png>)

### Debugger type and path

Choose the debugger. 

None
    No debugger. On Run, simply execute the program.
GNU debugger (gdb)
    [GDB](<GDB.md> "GDB") is not a part of Lazarus. Unless you are using Windows or Mac OS X with Xcode, you must install it yourself. This is the connector to gdb. You must set the path to gdb (for example /usr/bin/gdb) in the field below.
GNU debugger through SSH
    for remote debugging. You can use an SSH connection to another computer and execute gdb there. You need a SSH connection without prompt for password for this. See the SSH documentation on how to do that. This feature has certain limits. [Read more ...](<Remote_Debugging.md> "Remote Debugging")
GDB remote debugger (gdbserver)
    for remote debugging.

### Additional search path

You can add extra directories, where to search for sources, named in the debugging information of the executable. This is used for all projects. 

### Debugger general options

Show message on stop
    Enable this to show a notification, when programs stops.
Reset debugger after each run
    The IDE keeps GDB running and re-uses it. If you are using a (older) version of GDB that does not support this, then you can start a new GDB instance each time you start a debug session.

### Debugger specific options

Each debugger type has special options. 

#### GNU debugger (gdb)

Debugger_Startup_Options
    Pass extra arguments to GDB. This is not needed for normal usage. This is if you are familiar with GDB and wish to modify it's behaviour. Using this option may interfere with the proper working of the debugger
DisableLoadSymbolsForLibraries
    Prevent loading any symbols from libraries. (Must not be used, if debugging libraries). There are several gdb issues triggered by symbols loaded from libraries. If you get any error mention "solib", try setting this to true. Also see: [GDB_Debugger_Tips#Known_Problems_.2F_Errors_reported_by_the_IDE](<GDB_Debugger_Tips.md> "GDB Debugger Tips")
EncodeCurrentDirPath/EncodeCurrentFilePath
    Experimental. Those option affect the quoting of certain path/filenames when they are given to GDB. Changing the option to the wrong value will stop the debugger from working.
InternalStartBreak
    Changes the way the debugger detects your applications, main entry point. It is advised to leave this at default. Other values may be tried, if the debugger report an error "_The debugger could not set a breakpoint on the application _s entry point_ "._
MaxDisplayLengthForString
    For any string (pchar) gdb reads a maximum as specified by this setting. GDB always terminates at the 1st zero. GDB does not really handle pascal strings well.
UseAsyncCommandMode
    Mainly supported by gdbserver. Setting should be used for any remote debugging (gdbserver/gdb over ssh). See gdb documentation for "set target async". IF supported by gdb, can also be used for local debugging.
WarnOnInternalError
    Default true. If set to False, internal errors by gdb will be ignored by the IDE. Yet that does not change, that gdb did have an internal error, and that debugging may report incorrect data, or dis-behave in anyway. Neither will it prevent follow up error. It simple skips informing the user, yet fixes nothing. Leave on true, unless you get repeatedly the same internal error, and have tested well, that it does not affect you. (Then the warning dialog would be annoying, and you may want to skip it)
TimeOutForEval/WarnOnTimeOut
    Read [GDB_Debugger_Tips#TimeOuts](<GDB_Debugger_Tips.md> "GDB Debugger Tips")

## Event Log

[![Debugger options eventlog.png](https://wiki.freepascal.org/images/c/cf/Debugger_options_eventlog.png)](</File:Debugger_options_eventlog.png>)

### General

Eventually logging info should go the the [event log](<IDE_Window_Event_Log.md> "IDE Window:Event Log"), for now it is show in the [debug output](<IDE_Window__Debug_Output.md> "IDE Window: Debug Output") window. 

  * **Clear log on run** : clear the event log, on each start of the program.
  * **Limit line count to** : keep only the last lines of output.



### Messages

The Messages window is usually positioned below the source editor and shown, when building the project. It shows the compiler output and can show the output of external tools. 

[![IDEMessagesWindowFind1.png](https://wiki.freepascal.org/images/a/ae/IDEMessagesWindowFind1.png)](</File:IDEMessagesWindowFind1.png>)

See [IDE Window: Messages](<IDE_Window__Messages.md> "IDE Window: Messages"). 

## Language Exceptions

[![Debugger options language exceptions.png](https://wiki.freepascal.org/images/4/44/Debugger_options_language_exceptions.png)](</File:Debugger_options_language_exceptions.png>)

Programs can raise exceptions. For example, when a file can not be read. Here you setup, if the debugger should stop on an exception. 

### Ignore these exceptions

Add your exceptions to ignore here. For example: EDivByZero 

### Break on Lazarus Exceptions

Uncheck this option if you don't want to stop on any exception 

## OS Exceptions

[![Debugger options os exceptions.png](https://wiki.freepascal.org/images/1/1f/Debugger_options_os_exceptions.png)](</File:Debugger_options_os_exceptions.png>)

### Signals

(not implemented) 

Defines if signals should be handled by the debugger or by the user program. For instance, an div by zero is first signalled by the OS. Then the FPC RTL translates this to an EDivByZero. When the signal is handled by the debugger, the program is stopped before the RTL translates this message. Currently the debugger always stops on a signal. 

## See also

  * [Debugger Setup](<Debugger_Setup.md> "Debugger Setup")

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Debugger_Options](https://web.archive.org/web/20210920101420/https://wiki.freepascal.org/IDE_Window%3A_Debugger_Options)_
