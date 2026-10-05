# Debugger Setup

│ **English (en)** │  **[español (es)](</Debugger_Setup/es> "Debugger Setup/es")** │  **[français (fr)](</Debugger_Setup/fr> "Debugger Setup/fr")** │  **[русский (ru)](<../ru/Debugger_Setup.md> "Debugger Setup/ru")** │    
****

## Contents

  * 1 Configure the IDE
    * 1.1 Updating from an older IDE
    * 1.2 Choosing the correct debugger backend
    * 1.3 Setting up the debugger backend for the IDE
  * 2 Project Options
  * 3 GDB / LLDB Version
  * 4 See also



  


## Configure the IDE

### Updating from an older IDE

Older versions of Lazarus were only providing the GdbMI backend. It is highly recommended to update to Fp-Debugger or LLDB+FpDebug as indicated for your OS below. 

Please note that many other wiki pages may be outdated and still refer only to GDB. 

### Choosing the correct debugger backend

The Lazarus IDE provides several debuggers (backends). The page [Status of the debugger backends](<Debugger_Status.md> "Debugger Status") provides an overview. 

The list below indicates which backend is recommended for your environment. 

  


Operating System  | Hardware  | Recommended  
debugger backend  | Recommended  
debug info  | Notes   
  
---|---|---|---|---  
Windows 32/64bit  | Intel/AMD   
i386 or x86_64  | Fp-Debugger  | DWARF-3  | Includes cross debugging of a 32bit app on a 64bit system   
Windows 32bit   
Cross 64bit | Intel/AMD   
x86_64  | GdbMi  | DWARF-2 with sets  | _**Only**_ **cross debugging** of a 64bit app on a 32bit system   
Windows  | ARM / AArch64 / other  | GdbMi  | DWARF-2 with sets  | You may also try "GdbMI+FpDebug"   
Linux  | Intel/AMD   
i386 or x86_64  | Fp-Debugger  | DWARF-3  | If RTL or packages have debug info, ensure it is the exact same type as for the project   
Linux  | ARM / AArch64 / other  | GdbMi  | DWARF-2 with sets  | You may also try "GdbMI+FpDebug"   
Mac  | Intel/AMD x86_64   
AArch64 (e.g. M1)  | LLDB+FpDebug  | DWARF-3  |   
Embedded AVR  |  | FpDebug-avr package  | DWARF-2 or 3  | Requires gdb server   
Remote  | Any supported by GDB  | Gdb-Server  | DWARF-2 with sets  | Debugging on a different PC or VM   
Other  | Any supported by GDB  | GdbMi  | DWARF-2 with sets  |   
Any OS  
Debug with **STDIN/STDOUT redirect**  
(via "Run Parameters" command line: >file < file) | Any supported by GDB  | GdbMi  | DWARF-2 with sets  |   
  
### Setting up the debugger backend for the IDE

The following settings should be made once, after you installed Lazarus. They are often already set by default. 

The settings can be found under
    
_Tools > Options > Debugger > Debugger Backend_ [![Lazarus menu options.png](https://wiki.freepascal.org/images/c/ce/Lazarus_menu_options.png)](</File:Lazarus_menu_options.png>)

The image shows where to find the option dialog. On Mac the dialog is under "Preferences" 

  


[![lazarus dbg backend fpdebug.png](https://wiki.freepascal.org/images/3/3d/lazarus_dbg_backend_fpdebug.png)](</File:lazarus_dbg_backend_fpdebug.png>)

  * Select the correct backend on top of the page
  * The Fp-Debugger does not require a path.
  * If you have an LLDB or Gdb(MI) based debugger you must set the path to the gdb or lldb exe under "Debugger type and path" 
    * On Linux/Unix/Mac systems it may be something like "/usr/bin/gdb" or "/usr/bin/lldb"
    * On Windows it should be in a folder called "mingw\bin\" under the directory in which Lazarus is installed.


  * On [Status of the debugger backends](<Debugger_Status.md> "Debugger Status") you find a list of all backends with details on further settings. 
    * For the GdbMI debugger under Lazarus 2.0 and up, enable the option "FixIncorrectStepOver" in the property grid.



  


The backend can also be set on a per project config. For details see [IDE_Window:_Project_Options_-_Debugger_Backend](<IDE_Window__Project_Options_-_Debugger_Backend.md> "IDE Window: Project Options - Debugger Backend")

## Project Options

[![Dbg setup project1.png](https://wiki.freepascal.org/images/9/92/Dbg_setup_project1.png)](</File:Dbg_setup_project1.png>)

In order to debug your project, you need to tell the IDE to compile it in a special way, which provides additional information required by the debugger. 

The required settings are made in the "Project Options" dialog: 

  


  * Please note: This will considerably increase the size of your executable ([See FAQ](<Lazarus_Faq.md> "Lazarus Faq")). If you want to build a release version of your software you should switch those settings off (see also [Build Modes](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options"))



  


  


[![lazarus projectopts dbg.png](https://wiki.freepascal.org/images/0/05/lazarus_projectopts_dbg.png)](</File:lazarus_projectopts_dbg.png>)

  * You must enable "Generate Debug Info for GDB"
  * You must enable "Run uses the debugger (disable for release mode)"
  * You should set "Type of debug info" as recommended for your PC. 
    * If you set this to a non supported type, the IDE will prompt you for a correct setting when you try to start the debugger.
    * **Automatic** : Lets fpc choose. Can not be used with all backends.
    * **Stabs** : Old and outdated. Supported by GDB only (maybe LLDB **without** FpDebug). Should not be used
    * **Dwarf 2** : Minimal setting. Can not display watches for "set of". Issues with detecting PChar vs AnsiString. Should not be used
    * **Dwarf 2 with sets** : GDB friendly. Issues with detecting PChar vs AnsiString
    * **Dwarf 3** : Best setting for FpDebug.
    * None of them can currently display "property" with getter function.



* * *

  * You should **not** enable "Use external debug symbols"  
(It is a recommendation not to use this)
  * You **must** **not** enable "Strip symbols from executable"



* * *

You can use the "Build modes". The "..." button brings of a dialog to create "release and debug mode". In this case you should still check the "Type of debug info". 

  


  


[![Dbg setup project4.png](https://wiki.freepascal.org/images/3/36/Dbg_setup_project4.png)](</File:Dbg_setup_project4.png>)

  *     * Use optimization "Level 0" ("Level 1" may be used, but in some cases may cause issues)   

    * Do **not** use "Link Smart"



  


## GDB / LLDB Version

GDB 7.5 requires Lazarus 1.4 or higher. 

GDB 7.7.1 seems to work well with Lazarus 1.2.4. 

On macOS: **lldb** is part of the Apple developer tools 

## See also

  * IDE Options / Configuration 

Lazarus 2.0.x (and before)
    [IDE Window: Debugger Options](<IDE_Window__Debugger_Options.md> "IDE Window: Debugger Options")
Lazarus 2.2 (and up)
    [Debugger General Options](<IDE_Window__DebuggerGeneralOptionsFrame.md> "IDE Window: DebuggerGeneralOptionsFrame") and [Debugger Backend Options](<IDE_Window__DebuggerClassOptionsFrame.md> "IDE Window: DebuggerClassOptionsFrame")
  * Project Option / Command line args, Environment, and launcher app: [Run -> Run Parameters](<IDE_Window__Run_parameters.md> "IDE Window: Run parameters")
  * The FAQ for the default "GNU Debugger" in Lazarus: [GDB Debugger Tips](<GDB_Debugger_Tips.md> "GDB Debugger Tips")
  * Alternative debuggers in Lazarus: [Debugger Status](<Debugger_Status.md> "Debugger Status") Includes a feature map and default-key-strokes

---

_Source: [https://wiki.freepascal.org/Debugger_Setup](https://web.archive.org/web/20250317191244/https://wiki.freepascal.org/Debugger_Setup)_
