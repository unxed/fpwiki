# IDE Window: DebuggerGeneralOptionsFrame

│ **English (en)** │    
****  
****

## Contents

  * 1 About
  * 2 Options
    * 2.1 Additional search path
    * 2.2 Show message on stop
    * 2.3 Reset Debugger after each run
    * 2.4 Automatically close assembler window, after source not found
  * 3 See also



## About

This article describes the "General" Debugger settings in the Lazarus options dialog. Reachable via the Tools/Options menu. 

Before Lazarus 2.2 these options used to be part of: [IDE_Window:_Debugger_Options](<IDE_Window__Debugger_Options.md> "IDE Window: Debugger Options")

[![dbg general options.png](https://wiki.freepascal.org/images/1/1b/dbg_general_options.png)](</File:dbg_general_options.png>)

## Options

### Additional search path

If the IDE needs to find the sources for any unit included and described by the debugged executable, then it will use these directories. 

The IDE will first search in directories belonging to the project (so they do not need to be added here) and directories of any packages used. But if it can not find a given unit's source in the project/packages then it will search the "Additional search path" 

Before using this option you should ensure you understand why the debugger did not find your sources. 

### Show message on stop

The debugger can show a dialog when the debugged app terminated. 

### Reset Debugger after each run

In order to speed up starting a debug session, the debugger performs certain initializations only once. The GDB based debuggers for example, keep the gdb process running, and load the "application to be debugged" into this running process, saving the time to load and init gdb each time. In rare cases this has caused subsequent debug sessions to expose erroneous behaviour due to some internal setting changed in the first debug session. This option causes the debugger to be completely restarted between any two runs. 

There also is a menu entry in the "Run" menu, that allows to reset the debugger just once. 

### Automatically close assembler window, after source not found

If the application in the debugger is paused outside the user sources, the debugger sometimes can not show a line in any source code to indicate where it stopped. In such cases the debugger opens the assembler window. If the option is set, continuing execution will close the assembler window in such cases (I.e., if it was opened due to the described condition) 

## See also

  * [Debugger Setup](<Debugger_Setup.md> "Debugger Setup")

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_DebuggerGeneralOptionsFrame](https://web.archive.org/web/20240422232635/https://wiki.freepascal.org/IDE_Window%3A_DebuggerGeneralOptionsFrame)_
