# LazDebuggerFpRspRemote

│ **English (en)** │

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** For more information about the Free Pascal debugger see [Debugger Status](<Debugger_Status.md> "Debugger Status") and [FpDebug](<FpDebug.md> "FpDebug")

**LazDebuggerFpRspRemote** is a [debugger](<Debugger.md> "Debugger") integrated into the IDE based on **[FpDebug](<FpDebug.md> "FpDebug")**. It inherits from LazDebuggerFp and adds the functionality to debug a remote target using gdb's remote serial protocol over tcp/ip. It is intended that simple targets with gdbserver stub implementations will be supported. Currently only the AVR target is supported. This package is not stable yet. 

## Installation in the IDE

Install the package LazDebuggerFpRspRemote 

  * menu [Package|Install/Uninstall packages]
  * click **LazDebuggerFpRspRemote** in right pane, and push button [Install selection]
  * Button [Save and rebuild IDE]



## Debugger specific options
    
    
    Tools | [Options](<IDE_Window__IDE_Options_Dialog.md> "IDE Window: IDE Options Dialog") | [Debugger](<IDE_Window__Debugger_Options.md> "IDE Window: Debugger Options") | Debugger backend
    

Select RSP [FpDebug Dwarf remote debugger - RSP] 

## Compiling
    
    
    Project | Options | [Compiler Options](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options") | [Debugging](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options")
    

Many of the options are identical to [LazDebuggerFp](<LazDebuggerFp.md> "LazDebuggerFp"), only the options added by LazDebuggerFpRspRemote will be listed here: 

AfterConnectMonitorCommands
    Send remote monitor commands to configure the remote gdb stub.

AfterUploadBreakZero
    Option to add a breakpoint at address zero. This is intended for debugging startup code of embedded targets.

AfterUploadMonitorCmds
    Some gdbserver stubs needs to reset the target after the firmware is uploaded. The monitor reset command can be given here.

Host
    Specify the host IP address or network name where the gdbserver stub is running. Use **localhost** if it is running on the same computer as the debugger instance.

Port
    The tcp/ip port to connect to.

SkipUploadOfSectionList
    Specify a list of Elf sections marked as loadable that should not be uploaded to the target. This could be used to skip altering fuse or EEPROM values.

UploadBinary
    If set to true, upload loadable section contents to the target before continuing with debugging. Some gdbserver stubs to not support writing to memory when in debug mode, these gdbservers must be launched with the appropriate options to load the latest executable version of the project being debugged.

---

_Source: [https://wiki.freepascal.org/LazDebuggerFpRspRemote](https://web.archive.org/web/20221001140306/https://wiki.freepascal.org/LazDebuggerFpRspRemote)_
