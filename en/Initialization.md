# Initialization

│ **[Deutsch (de)](</Initialization/de> "Initialization/de")** │  **English (en)** │    
****

## Contents

  * 1 Initialization of variables
  * 2 Initialization - reserved word in a unit
    * 2.1 Structure of a unit
    * 2.2 See also



# Initialization of variables

**Initialization** is the process by which an application sets the value of various internal variables, structures, tables, and other constructs which are used by the [run-time library](<RTL.md> "RTL") ("RTL"), or the [operating system](<operating_system.md> "operating system"). 

Initialization and [startup](</index.php?title=startup&action=edit&redlink=1> "startup \(page does not exist\)") are two very similar practices, with the difference being that initialization is presumed to be the setting of values and data necessary for any program of any kind to start, while startup is the actions perfomed once a particular program is initialized. 

For example, a run-time library may need to obtain memory for internal [buffers](</index.php?title=buffer&action=edit&redlink=1> "buffer \(page does not exist\)"), [file handles](</index.php?title=file_handle&action=edit&redlink=1> "file handle \(page does not exist\)") (as used by the [Input](<Input.md> "Input") and [Output](<Output.md> "Output") [standard files](</index.php?title=standard_files&action=edit&redlink=1> "standard files \(page does not exist\)")), [window](</index.php?title=window&action=edit&redlink=1> "window \(page does not exist\)") handles in a [Graphical User Interface](<Graphical_User_Interface.md> "Graphical User Interface"), and other system resources. It may also, depending upon the operating system, load the user program or connect to [shared libraries](<shared_library.md> "shared library"). It would, at that time, startup and transfer control to the user program, which would, itself, perform its own initialization. 

The difference between initialization and startup is that, in general, initialization is where a program sets up internal constants, tables and resources that are necessary for the program to operate, and without which, the program simply cannot continue. 

Startup would be the point at which an application is fully initialized, all the things that it needs to do only once have been done, and it essentially is ready to perform some work related to the problem definition that the application is intended to solve. Startup may also include non-initialization matters that need to be performed at the beginning of execution of an application. 

The main difference being that if a part of the startup of any program fails, the particular program should still continue to work, albeit possibly with degraded performance, while if initialization fails, the program cannot continue. 

# Initialization - reserved word in a unit

**initialization** is also a [reserved word](<Reserved_words.md> "Reserved words") within [Object Pascal](<Object_Pascal.md> "Object Pascal"). It starts an optional initialization part of a [unit](<Unit.md> "Unit") and is terminated with [end](<End.md> "End") or an optional [finalization](<Finalization.md> "Finalization") part of the [unit](<Unit.md> "Unit"). 

Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

## Structure of a unit
    
    
     unit ...;      // Name of the unit
    
     interface      // Everything declared here may be used by this and other units (public)
    
     uses ...;
    
       ...
    
     implementation // The implementation of the requirements for this unit only (private)
    
     uses ...;
    
       ...
    
     initialization // Optional section: variables, data etc initialised here
    
       ...
    
     finalization   // Optional section: code executed when the program ends
    
       ...
     end.
    

## See also

  * [Finalization](<Finalization.md> "Finalization")
  * [Implementation](<Implementation.md> "Implementation")
  * [Interface](<Interface.md> "Interface")
  * [Uses](<Uses.md> "Uses")

---

_Source: [https://wiki.freepascal.org/Initialization](https://web.archive.org/web/20241202160410/https://wiki.freepascal.org/Initialization)_
