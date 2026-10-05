# Debugger Status

This is based on Lazarus 2.2. 

## Contents

  * 1 Available Debuggers
  * 2 Availability (OS/Platform)
  * 3 Start/Stop
  * 4 Execution Control (Run, Step, ...)
  * 5 Breakpoints / Exceptions
  * 6 Data (Watches, Locals, ...)
  * 7 Disassembler
  * 8 Other
  * 9 Debug Info Support (DWARF, Stabs)



#### Available Debuggers

GNU Debugger (gdb)
    The first debugger that was supplied with Lazarus. This debugger uses GDB as backend. The IDE performs translations between gdb's none Pascal-ish interface and the interface provided by the IDE. Package: LazDebuggerGdbmi
GNU Debugger through SSH (gdb)
    Allows to launch GDB over an ssh connection for remote debugging. This has however some serious limitations. Package: LazDebuggerGdbmi
GNU remote debugger (gdbserver)
    Remote debugger using GdbServer (part of the gdb suite). Package: LazDebuggerGdbmi
LLDB Debugger (Alpha)
    A basic LLDB based backend. This is a prove of concept implementation. The IDE displays **raw** LLDB output (no translation, no clean-up...) and there are no plans to add translation of the LLDB output. For using LLDB on Pascal applications the "LLDB Debugger (with fpdebug)" should be used. Package: LazDebuggerLldb
LLDB Debugger (with fpdebug)
    This debugger uses LLDB as a backend. In order to display Pascal-style results, it uses the "fpdebug" engine to show locals/watches. LLDB is used to control stepping, breakpoint and execution. For reading data LLDB is used to read the raw memory only. Package: LazDebuggerFPLldb
[GNU Debugger (with fpdebug)](<FpGdbmiDebugger.md> "FpGdbmiDebugger")
    Using GDB for stepping/breakpoints/... and FpDebug for watches. Package: LazDebuggerFPGdbmi
[FpDebug](<FpDebug.md> "FpDebug") / [LazDebuggerFp - integrated DWARF debugger](<LazDebuggerFp.md> "LazDebuggerFp") / ([Known issues](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues?scope=all&state=all&label_name%5B%5D=Debug%3AFpDebug>))
    A new debugger completely implemented in Pascal. This debugger implements the [DWARF](<DWARF.md> "DWARF") standard (currently 2 and 3 are partly implemented). It has special implementations added to interpret info provided by FPC. It can currently be used on Windows and Linux (very basic support for MacOS). Package: LazDebuggerFP
[FpDebug](<FpDebug.md> "FpDebug") / [LazDebuggerFpRspRemote](<LazDebuggerFpRspRemote.md> "LazDebuggerFpRspRemote")
    A new remote debugger to communicate with a gdbserver stub using gdb's remote serial protocol over tcp/ip (in future this could be expanded to include serial/UART). This debugger inherits from LazDebuggerFP, hence in principle supports the same functionality. Currently it only supports the AVR target. Not stable yet. Package: LazDebuggerFpRspRemote
[DAB](<https://microsoft.github.io/debug-adapter-protocol/>) / [LazDABDebugger](<https://gitlab.com/fpcprojects/LazDABDebugger>)
    The Debug Adapter Protocol (DAB) is a protocol to abstract the communication between a Debugger-frontend like Lazarus, and an underlying debugger like FpdServer. LazDABDebugger is a debugger that connects to (any?) [DAB-adapter](<https://microsoft.github.io/debug-adapter-protocol/implementors/adapters/>). This makes is possible to debug other languages or different platforms within Lazarus.
[DAB - FpdServer](<https://gitlab.com/fpcprojects/fpdserver>) / [LazFPDServerDebugger](<https://gitlab.com/fpcprojects/LazDABDebugger>)
    A derivative of LazDABDebugger specifically shaped to work with FpdServer, a debugger based on FpDebug that supports the DAB-protocol.

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:**

  * For the [GDB](<GDB.md> "GDB") and [LLDB](</index.php?title=LLDB&action=edit&redlink=1> "LLDB \(page does not exist\)") based debugger, support is indicated based on the assumption that your version of GDB supports this. Support indication is generally based on current GDB/LLDB for mainstream OS.
  * [FpDebugger](<FpDebug.md> "FpDebug") currently only works for Windows and Linux (AVR support is in progress)
  * The "LLDB Debugger (Alpha)" is not included below. It is a by-product of the "LLDB+FpDebug". It is not actively developed/maintained, other than what is needed for "LLDB+FpDebug".



Features   
  
---  
  


#### Availability (OS/Platform)

  
  
OS/Platform  | GdbMi | GdbMi+Ssh | GdbServer | GdbMi+FpDebug  | Lldb+FpDebug  | Fp-Debugger  | Comments   
  
Linux  | Y | Y | Y | Y  | Depends on availability  
of a stable LLDB  | Y   
(Intel/AMD)  |   
Mac (Intel)  | Depends on availability  
of a stable  
and codesigned GDB | ? | ? | ?  | Y  | N  |   
Mac (M1)  | Depends on availability  
of a stable  
and codesigned GDB | ? | ? | ?  | Beta   
Lazarus 2.2 and higher  | N  | The "LLDB Debugger (Alpha)" (without FpDebug) can (be attempted to) be used. Features (such as watches) will be limited to "c style"   
Windows  | Y | Y | Y | Y  | Depends on availability  
of a stable LLDB  | Y   
(Intel/AMD)  | For Windows 32 bit: FpDebug does not support SEH exceptions (used by default since FPC 3.2). Stepping over/after such exceptions may not work properly.   
AVR  | N | N | Y | N  | N  | Beta   
(via a remote connection to a gdbserver stub)  
Package "LazDebuggerFpRspRemote"  | The features for AVR remote debugging (using FpDebug+gdbserver) may differ from the Fp-Debugger features listed below.   
  
  


#### Start/Stop

  
  
Feature | Key  | GdbMi | GdbMi+Ssh | GdbServer | GdbMi+FpDebug  | Lldb+FpDebug  | Fp-Debugger  | Comments   
Run | `F9` | Y | Y | Y | Y  | Y  | Y  |   
Run to first line (Step) | `F7` / `F8` | Y | Y | Y | Y  | ?  | Lazarus 3.0 and higher  | For GUI apps the first line is in the code generated by the IDE   
Run to line at cursor |  | Y | Y | Y | Y  | N  | Y  | Lazarus 2.2 and higher   
Attach |  | Y | Y | Y | Y  | ?  | Lazarus 2.2 and higher  |   
Detach |  | Y | Y | Y | Y  | ?  | Lazarus 2.2 and higher  |   
Stop | `Ctrl`+`F2` | Y | Y | Y | Y  | Y  | Y  |   
Reset Debugger |  | Y | Y | Y | Y  | Y  | Y  | Kill/Stop  
Restart the external debugger (gdb/lldb)   
Debugger stopped dialog when app exits |  | Y | Y | Y | Y  | Y  | Y  | Configurable   
Run without debugging | `Ctrl`+`⇧ Shift`+`F9` | Y | Not debugger specific   
  
  


#### Execution Control (Run, Step, ...)

  
  
Feature | Key  | GdbMi | GdbMi+Ssh | GdbServer | GdbMi+FpDebug  | Lldb+FpDebug  | Fp-Debugger  | Comments   
Continue (Run) | `F9` | Y | Y | Y | Y  | Y  | Y  |   
Step over line | `F8` | Y | Y | Y | Y  | Y  | Y  |   
Step into line | `F7` | Y | Y | Y | Y  | Y  | Y  |   
Step (over) to line at cursor | `F4` | Y | Y | Y | Y  | N  | Y  | "STEP OVER" - This works only within the current procedure. It will go to the selected line, or stop if the current procedure is left.   
It will *step* *over* any recursive calls of the procedure, therefore not stopping at the line during recursion.   
Run to line at cursor |  | Y | Y | Y | Y  | N  | Y  | Only Lazarus 2.2 and higher   
Step out (of current function) | `⇧ Shift`+`F8` | Y | Y | Y | Y  | Y  | Y  |   
Step over asm | `Alt`+`F8` | Y | Y | Y | Y  | Y  | Y  |   
Step into asm | `Alt`+`F7` | Y | Y | Y | Y  | Y  | Y  |   
Smart source or asm stepping (over/into) |  | Y | Y | Y | Y  | Y  | Y  | Assign one key (one for "over", one for "into") that acts as source-line or asm stepping, depending on window focus   
Pause |  | Y | depends on gdb, mostly "No" | Y | Y  | Y  | Y  |   
Suspend individual threads |  | N | N | N | N  | N  | Y  | While the app is paused threads can be suspended/resumed for upcoming stepping/running   
Step: steps over "none breaking" breakpoint |  | Y | Y | Y | Y  | ?  | Lazarus 2.0 or 2.2 ?  | Any "step" command will continue to its step-end after a "none breaking" Breakpoints. This is a Breakpoint that has its break property turned off. It may record a snapshot, log a message, or enable/disable other breakpoints.   
[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>) **Warning:** The "auto-continue" (time based) function may not return to stepping, but instead "run/continue" execution.  
Step: steps over ignored exceptions |  | Y | Y | Y | Y  | ?  | Lazarus 2.2  | Only for standard fpc exception handling. And (Lazarus 2.2) SEH on Win64.   
Win32 and other OS structured exception handling are not supported (Ignored exceptions will turn Stepping into Running).   
Source-Step: steps from except to finally or except |  | Y | Y | Y | Y  | ?  | Lazarus 2.2  | Only for standard fpc exception handling. And (Lazarus 2.2) SEH on Win64.   
Win32 and other OS structured exception handling are not supported (Ignored exceptions will turn Stepping into Running).   
Executable/Code line indicators |  | Y | Y | Y | Y  | Y  | Y  | Blue dots in the gutter indicate lines with debug info.   
  
Breakpoints / None-breaking breakpoints / Auto-continue breakpoints / Disabled breakpoints
    A breakpoint usually pauses your app, in addition it can perform tasks like logging a message (debugger event log), dumping data to the log, taking a snapshot (debugger history).   
A none-breaking breakpoint will perform those extra tasks, but not pause your app.   
An auto-continue breakpoint will pause your app (and perform any other action) for a given amount of milliseconds (during which you can use the pause button, to pause permanently), after that it will continue your app. It usually continues using "Run", therefore loosing any current single-step action  
A disabled breakpoint will do nothing. It is as if it does not exist.
Stepping and breakpoints / exceptions
    If any debugger pauses the target-app at a breakpoint or exception while single stepping, then the single-step is lost. The breakpoint/exception may be in a deeply nested called function, and you cannot continue the single-step. I.e. there is no build in way to get back to the function where you started your step, and finish this step. For this you need to go through the callstack yourself and find the start location and use breakpoints
Stepping and none-breaking-breakpoints / Ignored-exceptions
    Most/All? debugger backends can in this case continue to the position where the single-step would have ended. (This may be limited for asm stepping). In case of an exception the single step will end at the next finally or except. "Next" from the point of view of the step-command. I.e., any finally inside a "stepped over function" will be stepped over, but any finally on the same level as the step began will be used as end point. If there is none on the same level, then the next outer will be used.  
This feature relies heavily on FPC implementation details. Thus it may not work with newer FPC versions.  
  


#### Breakpoints / Exceptions

  
  
Feature | Key  | GdbMi | GdbMi+Ssh | GdbServer | GdbMi+FpDebug  | Lldb+FpDebug  | Fp-Debugger  | Comments   
Breakpoint on Source Line | `F5` | Y | Y | Y | Y  | Y  | Y  |   
Breakpoint on Assembler Line |  | Y | Y | Y | Y  | Y  | Y  |   
Watchpoint / Breakpoint Data |  | Y | Y | Y | Y  | ?  | Partial  | Amount/Size is limited by architecture Intel based systems usually have 4 Watchpoints, each with a max size of 32 bit (64 bit on 64bit CPU)   
Conditional Break/Watch-point |  | (Y)  | (Y)  | (Y)  | (Y)  | Condition must be C style for LLDB  
E.g.: a==1 / No strings, only simple types  
Not all data types supported.  | Y  | **GDB based debuggers are limited** to GDB's understanding of the conditional expression.  
**No strings** , except with $_streq() / Not all data types supported.   
Enable/Disable Break/Watch-point according to program flow |  | Y | Y | Y | Y  | Y  | Y  | Enable/Disable a breakpoint (or a group of) when execution passes through a pre-defined point in your code. So you can setup that the debugger will only break/skip-breaking, when the code is called in a specific order, or from a specific outer subroutine.   
Ignore exception by class |  | Y | Y | Y | Y  | Y  | Y  | Configurable for each project   
Break/Watch-point overview Window | `Ctrl`+`Alt`+`B` | Y | Y | Y | Y  | Y  | Y  |   
TODO: .... Document Breakpoint properties |  | . | . | . | .  | .  | .  |   
TODO: .... Document Breakpoint actions |  | . | . | . | .  | .  | .  |   
  
GdbMi+Ssh
    Some functionality depends on the ability to use "Pause". If "Pause" does not work, break/watch-points can not be changed/added/removed while the target app is running. Only when the target app pauses at a breakpoint (or for other reasons), then can changes be made.  
  


#### Data (Watches, Locals, ...)

  
  
Feature | Key  | GdbMi | GdbMi+Ssh | GdbServer | GdbMi+FpDebug  | Lldb+FpDebug  | Fp-Debugger  | Comments   
Watches | `Ctrl`+`Alt`+`W` | Y | Y | Y | Y  | Y  | Y  |   
Locals | `Ctrl`+`Alt`+`L` | Y | Y | Y | Y  | Y  | Y  |   
Stack | `Ctrl`+`Alt`+`S` | Y | Y | Y | Y  | Y  | Y  |   
Threads | `Ctrl`+`Alt`+`T` | Y | Y | Y | Y  | Y  | Y  | Running State indicator may not be accurate   
Inspect | `Alt`+`F5` | Y | Y | Y | Y  | Y  | Y  | View members in property Grid. Double-click to inspect members   
Evaluate / Modify | `Ctrl`+`F7` | Y | Y | Y | Y  | ?  | Y   
Modify only in Lazarus 2.2 and higher  | See notes below   
Memory Viewer |  | Y | Y | Y | Y  | Y  | Y  | Since Lazarus 4.0   
Watch/Inspect Class Properties |  | N | N | N | N  | N  | N   
Work in progress   
Will at least require a new major FPC release: 3.4.0 or up  | Using "DWARF" properties that refer directly to a field (no getter method) are shown.   
Watch/Inspect Function calls |  | N | N | N | N  | N  | Partial   
BETA: Lazarus 2.2 and higher can call certain functions in watches   
Please see [FpDebug-Watches-FunctionEval](<FpDebug-Watches-FunctionEval.md> "FpDebug-Watches-FunctionEval") |   
Watch/Inspect Intrinsic helper functions |  | N | N | N | N  | N  | Lazarus 2.3 and higher   
Please see [FpDebug-Watches-Intrinsic-Functions](<FpDebug-Watches-Intrinsic-Functions.md> "FpDebug-Watches-Intrinsic-Functions") |   
Use Instance type/class |  | Y | Y | Y | Y  | Y  | Y  | By default the debugger shows variables according to their declared type/class. "Sender: TObject" will only show as TObject.   
The debugger can detect the real class of the instance (e.g. a TButton) and show all details   
Tooltip / Hint |  | Y | Y | Y | Y  | Y  | Y  |   
Tooltip / Hint => Auto-deref pointer |  | Y | Y | Y | Y  | Y  | Y  |   
Tooltip / Hint => Use Instance Class |  | Y | Y | Y | Y  | Y  | Y  |   
Paste value into Watches window |  | Y | Y | Y | Y  | Y  | Y  | Paste directly into watches window, no need to press "Add new watch"   
Drag/Draw value from source-editor to Watches window |  | Y | Y | Y | Y  | Y  | Y  |   
TODO: .... Document Watches properties |  | . | . | . | .  | .  | .  |   
Value-Formatters (3.99)   
[IDE_Window:_IDE_Optiorns_-_Debugger_Value-Formatter](<IDE_Window__IDE_Optiorns_-_Debugger_Value-Formatter.md> "IDE Window: IDE Optiorns - Debugger Value-Formatter") |  | Y | Y | Y | Y  | Y  | Y  | Display colors by name, TDate in human-readable form, named constants, ...   
  
Modify
    All debuggers have restriction towards what type of data they can modify (modify target variables during a debug session).  
Managed types (ansistring, dyn array) can not be modified by any debugger. Structured types can only be modified field by field.  
For other types support depends on the debugger backend.  
\- For Gdb/Lldb this is currently not documented.  
\- For FpDebug the following do not NOT work: floating, currency, all strings incl shortstring, packed/bitpacked types.  
  


#### Disassembler

  
  
Feature | Key  | GdbMi | GdbMi+Ssh | GdbServer | GdbMi+FpDebug  | Lldb+FpDebug  | Fp-Debugger  | Comments   
Disassembler | `Ctrl`+`Alt`+`D` | Y | Y | Y | Y  | ?  | Y  |   
History Navigation |  | Y | Y | Y | Y | Y | Y  | Navigate back and forward between previously disassembled addresses and current address  
Lazarus 3.0 and up   
Call/Jump target annotations |  | (Y)  
Depends on GDB version  | ?  | Y  
Lazarus 3.0  | Information on the target of a call/jmp statement is displayed   
  
Ctrl-Click / Link to Call/Jump target |  | N | N | N | N  | N  | Y  
Lazarus 3.0  | Ctrl click on a call/jump statement and view the target address  
Ctrl hover over a call/jump statement and highlight the target (if in view)   
  
  


#### Other

  
  
Feature | Key  | GdbMi | GdbMi+Ssh | GdbServer | GdbMi+FpDebug  | Lldb+FpDebug  | Fp-Debugger  | Comments   
History | `Ctrl`+`Alt`+`H` | Y | Y | Y | Y  | Y  | Y  | Allow to review Watches/Locals/Stack from when your app was previously paused. This does not restore the state of the application. It only allows to view the value that the debugger evaluated previously.   
History - Import / Export |  | Y | Y | Y | Y  | Y  | Y  | 

  * Save your debug results for later review.
  * Or send them to a co-worker for feedback
  * Combined with "snapshot" taking breakpoints, you can send a prepared project (watches/breakpoints set) to a co-worker, have them run the project in the debugger, and return the result to you.

  
Event log | `Ctrl`+`Alt`+`V` | Y | Y | Y | Y  | Y  | Y  |   
View internal debug output |  | Y | Y | Y | Y  
partly  | ?  | Not applicable  | View communication with the external debugger. For troubleshooting   
Debugging libraries   
Stepping and Watches |  | Y | Y | Y | Y  | ?  | Lazarus 3.0 and up  | This pertains to debugging libraries compiled by FPC with debug info   
Using FpDebug the executable-line-markers (blue dots) are missing.   
Debugging libraries   
Breakpoint |  | Depend on various factors | Depend on various factors | Depend on various factors | Depend on various factors  | ?  | Lazarus 3.0 and up   
  
Debugging libraries   
Exceptions |  | Depend on various factors | Depend on various factors | Depend on various factors | Depend on various factors  | ?  | Lazarus 3.0 and up   
  
Cross debugging 32<>64bit |  | Y | Y | Y | Y   
Win-64bit IDE can debug 32 bit target  | ?  | Lazarus 2.2   
Win-64bit IDE can debug 32 bit target  | 

  * Gdb-based: Requires Gdb for target-bitness to be installed, and the "LazGDeBugControl.exe" (included in Lazarus)
  * Fp-Debugger: Not tested on Linux / For Windows, requires Lazarus 2.2

  
Remote debugging |  | N | Y | Y | N  | N  | N*  | *Remote debugging of simple targets (only AVR currently) via gdb's remote serial protocol using package LazDebuggerFpRspRemote   
Edit source code while debugging |  | Y | Y | Y | Y  | Y  | Y  | 

  * Fully working source editor (with code completion), while debugging
  * Editor keeps track of inserted/deleted lines, so breakpoints can be set to correct location

  
StdIn/StdOut redirect |  | Y | Y | Y | Y  | ?  | Y   
Lazarus 4.0  | Redirect the apps StdIn/StdOut by adding ">file" or "<file" to command line in "Run Parameters"   
  
  


#### Debug Info Support (DWARF, Stabs)

  
  
\-  | GdbMi | GdbMi+Ssh | GdbServer | GdbMi+FpDebug  | Lldb+FpDebug  | Fp-Debugger   
Recommended  | DWARF-2 (with sets)  
-gw -godwarfsets | DWARF-3   
-gw3  | DWARF-3   
-gw3  | DWARF-3   
-gw3   
Supported  | All DWARF versions and "Stabs"   
Stability of DWARF 3 and higher depends on FPC and GDB version   
(older FPC produced incorrect DWARF-3 which caused troubles in GDB, but GDB has some issues too) | Only DWARF   
All DWARF versions  | Only DWARF   
All DWARF versions  | Only DWARF   
All DWARF versions   
  
    See also: [Debugger_Setup](<Debugger_Setup.md> "Debugger Setup") and [DWARF](<DWARF.md> "DWARF")

---

_Source: [https://wiki.freepascal.org/Debugger_Status](https://web.archive.org/web/20241213024907/https://wiki.freepascal.org/Debugger_Status)_
