# IDE Window: DebuggerClassOptionsFrame

│ **English (en)** │    
****  
****

## Contents

  * 1 About
  * 2 Options
    * 2.1 Main Toolbar
      * 2.1.1 Backend drop-down
      * 2.1.2 Add
      * 2.1.3 Copy
      * 2.1.4 Delete
    * 2.2 Settings
      * 2.2.1 Name
      * 2.2.2 Debugger Path and type
      * 2.2.3 Properties
  * 3 See also



## About

This article describes the "Debugger backend" settings in the Lazarus options dialog. Reachable via the Tools/Options menu. 

Before Lazarus 2.2 these options used to be part of: [IDE_Window:_Debugger_Options](<IDE_Window__Debugger_Options.md> "IDE Window: Debugger Options")

Lazarus comes with different backends to its Debugger. Each of them has its own set of options. This page does not describe the individual backends, but how to create a configuration entry for any of them. 

Which backends you can choose from depends on the package you have installed. 

  
Available backends are: 

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

LLDB based backends were created with MacOS in mind. They may work on other platform if you can get a well working LLDB for the platform. The backend does not usually receive any maintenance for none MacOS. 

## Options

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** To switch between back-ends use the "Backend drop-down" in the Toolbar.   
  
Do **not** use the "Debugger type and path". Those two dropdowns should only be used to setup new entries ("Add" button)   
Using "Debugger type and path" for existing entries changes the name suffix, and change project specific debugger settings if they refer the selected entry 

[![dbg class options.png](https://wiki.freepascal.org/images/0/02/dbg_class_options.png)](</File:dbg_class_options.png>)

### Main Toolbar

#### Backend drop-down

Choose the backend that the IDE should use by default. Projects can within their "Project Options" choose an alternate backend (from the list of backends configured here in the global options) 

The chosen backend is also the backend that you are editing on this page. If you want to edit a backend that is not your default, then you must temporarily change to it. 

The drop-down shows the user specified "Name" (instance name) of the backend as configured in the settings below. The name can be left empty, in which case the drop-down will show the class-name of the backend. 

For each class there can only be one un-named backend. The un-named backends are for compatibility with older versions of Lazarus (before 2.2). Older versions of Lazarus will only see the un-named entries. This is because older Lazarus only provided storage for one set of settings per backend-class. 

#### Add

Create a new backend 

#### Copy

Create a new backend, by copying the current 

#### Delete

Delete the current backend 

  


### Settings

#### Name

A name to identify the backend. 

Also see compatibility remarks above. 

#### Debugger Path and type

The class of the backend. 

If the backend requires an external exe, then this must be set in the path field. E.g., GDB based backends need the path to gdb, and LLDB based backends need the path to lldb. 

Changing the backend can loose properties configured for the current backend. Properties present in both backends are usually preserved (e.g. between gdb and gdbserver many properties are present in both) 

#### Properties

Settings specific to the selected backend. 

## See also

  * [Debugger Setup](<Debugger_Setup.md> "Debugger Setup")

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_DebuggerClassOptionsFrame](https://web.archive.org/web/20240304143508/https://wiki.freepascal.org/IDE_Window%3A_DebuggerClassOptionsFrame)_
