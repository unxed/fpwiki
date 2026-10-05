# LazLogger

│ **English (en)** │  **[русский (ru)](<../ru/LazLogger.md> "LazLogger/ru")** │ 

## Contents

  * 1 Overview
  * 2 Usage
    * 2.1 Adding logging support to your source code
      * 2.1.1 Activate/Deactivate LazLogger by using the correct unit
    * 2.2 Logging output
      * 2.2.1 Stdout/Console
      * 2.2.2 File
      * 2.2.3 Debug server
    * 2.3 Log Groups
  * 3 Documentation
  * 4 Multithreading
  * 5 See also



## Overview

The unit **LazLogger** is supplied with Lazarus and provides logging to file or console. 

## Usage

### Adding logging support to your source code

(For historical reasons some parts are also accessible through LclProc (removed with Lazarus 1.10); this access might be removed in future) 

By just including the unit **lazlogger** you can use the following: 

  * **[DebugLn](<DebugLn.md> "DebugLn"):** which works about the same as WriteLn.
  * **DebugLnEnter/DebugLnExit:** same as debugln, but DebugLnEnter increases (and DebugLnExit decreases) the indentation of the written line.
  * **DbgOut:** which works about the same as Write.



All of the above can be called with either: 

  * A single string, or list of (up to 15) strings: DebugLn('Foo'); DbgOut('a','b');
  * An array of const: DebugLn(['Foo=', 1, ' Bar=', anInteger]);
  * Same arguments as Format(). String + array of const: DbgOut('a %d',[1]);



Also there are many **dbgs** functions. Each converts a common type to a string for display. For example dbgs(aRect) converts a variable of type TRect to a string. You can write your own dbgs() functions for type conversions Lazarus does not provide, such as your custom records, which you can then use in DebugLn etc. 

#### Activate/Deactivate LazLogger by using the correct unit

LazLogger provides 3 units for your "uses" clause 

LazLogger
    Use this unit in your main program only. Using this unit activates the functionality of LazLogger.

LazLoggerBase
    Use this unit whenever you want to use DebugLn. What LazLoggerBase does depends on whether the unit LazLogger is used in any other unit of your application. If the LazLogger unit is **not** used, LazLoggerBase installs a "black hole" logger which discards all log messages. If LazLogger **is** used anywhere, then all units using LazLoggerBase will write the log messages as described under "Logging Output"

LazLoggerDummy
    This unit provides you with dummy DebugLn calls (and all other LazLogger functions). They are just calls to empty methods. This allows you to disable all logging in a unit, without the need to remove any debugln statement.

### Logging output

#### Stdout/Console

By default the output created by `DebugLn` is written to the `stdout` device. The output is silently discarded if `stdout` is not open. On Windows this is the default behavior, if project type "Application" is used to create a Windows GUI application. To get a console alongside your Windows GUI application, you need to **uncheck** the option "Win32 gui application (-WG)" under Project options / Compiler options / Config and Target / Target specific options. Alternatively you can add {$APPTYPE CONSOLE} at the top of your project .lpr file, or configure multiple "Build Modes" to control the -WG switch. 

#### File

Debug output can also be written to file. The initialization code of the logging unit 

  * checks your program's command line parameters for **\--debug-log= <file>** (In Lazarus: Run / Run Parameters / Command line parameters). On finding this parameter, any subsequent debug output is sent to <file>.



    The param name can be changed with **DebugLogger.ParamForLogFileName:='--debug-log';** This property exists only if the unit LazLogger is used. (LazLoggerBase does not define this property)

  * If no '--debug-log' command line parameter has been given, it next checks if an operating system environment variable xxx_debuglog exists, where xxx is the program file name without extension. E.g. for Lazarus this would be lazarus_debuglog. If such an environment variable exists, it uses the file specified in that environment variable as file to receive debug output.



Example: if you do: 
    
    
    set lazarus_debuglog=c:\lazarus\debug.txt
    

and run Lazarus, debug output will be written to c:\lazarus\debug.txt. 

#### Debug server

Not available in LazLogger; see [DebugServer](<DebugServer.md> "DebugServer"). 

### Log Groups

You can add log groups. 
    
    
    var MY_LOG_GROUP: PLazLoggerLogGroup; 
    
    initialization
      MY_LOG_GROUP := DebugLogger.FindOrRegisterLogGroup('fancy_name_for_mylog' {$IFDEF MY_LOG_GROUP_ON_BY_DEFAULT} , True {$ENDIF} );
    

In your code use 
    
    
    debugln(MY_LOG_GROUP, 'log this text');
    debugln(MY_LOG_GROUP, ['a=',a,' b=',b]); // a,b must be basic types: integer, byte, ansistring. See types available for "array of const"
    

and to enable it call your app with 
    
    
    --debug-enable=fancy_name_for_mylog
    

\--debug-enable= takes a comma separated list 

An already enabled group ( DebugLogger.FindOrRegisterLogGroup('grpname', True) ) can be disabled by specifying the group name with a minus. 
    
    
    --debug-enable=-fancy_name_for_mylog
    

The parameter name can be set via **DebugLogger.ParamForEnabledLogGroups='--debug-enable';**

## Documentation

For more features, see the unit itself and the LCL documentation. 

Using LazLogger/LazLoggerBase your code can access the DebugLogger instance. This offers some properties to fine tune logging. 

You can also write your own logger class and use it with LazLogger/Base. 

## Multithreading

  * **DebugLn** is thread safe, since Lazarus 2.0.



    Earlier versions of this text stated "since 1.0", but a further race condition was found and fixed for 2.0.

  * Calls to **DbgOut** , **DebugLnEnter** , **DebugLnExit** can be made from threads, but may not always format/indent the output correctly.


  * The creation/setup/destruction/configuration of the logger is [not](</index.php?title=not&action=edit&redlink=1> "not \(page does not exist\)") thread-safe, and should be done before threads are started.



    Creation is usually done automatically during unit initialization.
    But any properties (eg ParamForLogFileName) should be set before accessing the logger from threads.

  * Callback to events like OnDebugLn are made in each thread. The code in the callback must be thread-safe by itself.



    Changing the property is NOT thread-safe

## See also

  * [TEventLog documentation](<http://lazarus-ccr.sourceforge.net/docs/fcl/eventlog/teventlog.html> "doc:fcl/eventlog/teventlog.html") Built in support for logging in FPC/Lazarus
  * [DebugServer](<DebugServer.md> "DebugServer") Allows sending log entries to a viewer via IPC
  * [MultiLog](<MultiLog.md> "MultiLog")
  * [log4delphi](<log4delphi.md> "log4delphi")
  * The package IdeLazLogger, which adds a dialog to the IDE allowing to enable/disable log groups used in the IDE itself.

---

_Source: [https://wiki.freepascal.org/LazLogger](https://web.archive.org/web/20230131225045/https://wiki.freepascal.org/LazLogger)_
