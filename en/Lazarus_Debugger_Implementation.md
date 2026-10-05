# Lazarus Debugger Implementation

│ **English (en)** │    
****

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** There is another page on this topic [TDebuggerIntf](<TDebuggerIntf.md> "TDebuggerIntf")

## Contents

  * 1 Introduction
  * 2 Debugger-Frontend
    * 2.1 Files and Folders
    * 2.2
  * 3 Debugger-Interface
    * 3.1 Files and Folders
    * 3.2
    * 3.3 TDebuggerIntf - main class
      * 3.3.1 State
      * 3.3.2 RequestCommand
      * 3.3.3 Events
    * 3.4 TDebuggerDataMonitor and TDebuggerDataSupplier - stack, watches, ....
  * 4 Debugger-Backend
    * 4.1 GDB / GDBMI based
    * 4.2 LLDB
    * 4.3 FpDebug
  * 5 Testcases (Backend)



# Introduction

This page provides an overview of how the debugger is implemented in the Lazarus IDE. 

The debugger comprises the following parts: 

Debugger-Frontend
    The controls (run, step, stop buttons) and dialogs (watches, stack) in the IDE.
Debugger-Interface
    A set of base-classes defining the communication between the front-end and each back-end
Debugger-Backend
    Any code to control the debugged app, and retrieve/modify data from/in that app. The back-end may itself be split into an **IDE back-end** and an **external back-end** (e.g. gdb).

# Debugger-Frontend

The code for the debugger frontend belongs currently directly to the IDE (to the project ide/lazarus.lpi). It is not a package of it's own. 

Some of this code may one day be moved into a package. And maybe even introduce a package DebuggerFrontendIntf, to allow alternative 3rd party front-ends. 

### Files and Folders

  * package: IdeDebugger in /ide/packages/idedebugger (Lazarus 2.3 up)
  * folder: /debugger
  * folder: /ide, files: debugmanager.pas and basedebugmanager.pas
  * folders: /ide/frames and /debugger/frames (for configuration settings)



Most of the Frontend-Debugger code should eventually move to the package IdeDebugger. Some abstract type definitions may go into the IdeIntf package. 

### 

BaseDebugManager is currently based on components\debuggerintf\dbgintfdebuggerbase.pp 

# Debugger-Interface

### Files and Folders

  * package: DebuggerIntf in /components/debuggerintf/ (old code)
  * package: LazDebuggerIntf in /components/debuggerintf/lazdebuggerintf (new version of the Interface / work in progress / Lazarus 2.3 up)



Code in "DebuggerIntf" will over time move into other locations. (Frontend, LazDebuggerIntf or individual Backends) 

### 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** The debugger interface is still subject to change. It was created by just extracting code from the GdbMiDebuggers (gdb/server/ssh) - which were at the time the only debuggers. The IDE now having several other debuggers may in future warrant a redesign of this interface 

## TDebuggerIntf - main class

TDebuggerIntf provides the communication between front ad back-end. 

### State

Note: Currently State contains some value internal to the backend. (dsIdle/dsInit). Those may vary from back-end to back-end 

There are 2 pause states. 

  * The proper pause (debugged app is paused) = dsPause.
  * And dsInternalPause used for example for breakpoints that will automatically continue to run the debuggee.



See documentation in source code. 

### RequestCommand

The front-end can send asynchronous commands to the back-end. A result - if any - will be provided via an event. 

Commands are (some entries of the list are outdated) 
    
    
     TDBGCommand = (
       dcRun, dcPause,  dcStop,
       dcStepOver,  dcStepInto,  dcStepOut,  dcStepTo,
       dcRunTo,  dcJumpto, dcAttach,  dcDetach,
       dcBreak,  dcWatch,  dcLocal,  dcEvaluate,  dcModify,
       dcEnvironment, dcSetStackFrame,  dcDisassemble,
       dcStepOverInstr,  dcStepIntoInstr,
       dcSendConsoleInput  //, dcSendSignal
       );
    

The back-end does not need to implement all commands. (see procedure SupportedCommands) The availability of commands also depends on the **State**

There are helpers like 
    
    
     procedure Run;
    

They will take additional care of calling **Init** if needed. 

### Events

## TDebuggerDataMonitor and TDebuggerDataSupplier - stack, watches, ....

The debugger can supply data like Threads, Stack, Locals, Watches... Any of this data is provided by a ___DataSupplier. On the IDE site to each Supplier, one ___DataMonitor is attached. The Supplier/Monitor concept was introduced to allow both sides to have their own, yet synchronized list of items for the list-entries. 

  
For an example: Watches. 

The IDE creates a Watch as "TIdeWatch". The Ide can stick any data to a watch that the IDE may require. For example watch-properties (display, enabled, ...) or streaming. 

The back-end may need different information, such as internal details retrieved from the external-backend (gdb). So the back-end uses "TGDBMIWatches = class(TWatchesSupplier)". And other back-ends have their own sub-classes. 

The supplier/monitor concept keeps the two lists synchronized. For Watches, the IDE side (the monitor) creates and deletes the entries. For Stack and locals, it is the back-end that creates/deletes entries. 

When the IDE needs a value then the request is forwarded to the data-supplier. A value in this context can be anything read from any of the involved objects. The count of thread/stackframes is a value, the frame itself is a value (which is an object with multiple data fields). And the evaluated result of a watch is a value too. 

  
All data is retrieved event driven. Asking for Stackcount, or watch-result will not return the value immediately. It will trigger its evaluation, and the data-supplier will sent an update event when the value (any value) becomes available. 

For this to work, each value has a validity "TDebuggerDataState". 

  * AWatch.Values[ThreadId, Stackframe].Value and AWatch.Values[ThreadId, Stackframe].Validity
  * In other cases validity is not exposed. Count may return 0, if it is not yet valid. (Maybe a TODO, to expose the validity? Just maybe.)



There is no rule how many update-events may be triggered before some value becomes valid. Any code listening to events from the monitor simply has to start over when it gets an event. Events may occur nested, code must protect itself again re-entrance in such a case. 

  
The validity is internally also used to avoid duplicate triggers. The IDE can ask for a value as often as it wants. When it asks for the first time the validity changes to "requested", preventing further triggers to the back-end. Validity is usually reset, when the debugger leaves the dsPause state. (dsPause is a state related to the debuggers execution of the debuggee). 

# Debugger-Backend

"Command Queue"
    

This is an item most/all backends have in common. Commands are received via **RequestCommand** and may take any amount of time to execute. Other work for the backend comes from the DataMonitor/Suppliers. And Backends may also schedule work of there own (for example retrieving the current location when an app enters pause). All this work is usually serialized in the backend (though backends are free to process multiple items paralel). For this backends have (at least) one CommandQueue. Usually this queue supports some sort of priority, so items in response to a user action (e.g. evaluating an expression for source editor hint) can be executed first. This means the user does not need to wait unnecessary long for this. 

  


## GDB / GDBMI based
    
    
     Location: components/lazdebuggergdbmi
    

## LLDB
    
    
     Location: components/lazdebuggers/LazDebuggerLldb
               components/lazdebuggers/LazDebuggerFpLldb
    

## FpDebug
    
    
     Location: components\lazdebuggers\LazDebuggerFp
    

  


# Testcases (Backend)

Most/all backends come with a testcase, located in the subdirectory **test**

The testcase are designed to test the backend against a set of "test apps". The results of the test may depend on: 

  * the version of fpc used to compile the "test apps"
  * the version of the external debugger, if any (gdb, lldb)



Therefore there are some config files which should be set up before running the testcase. Those are fpclist.txt, and in case of gdb/lld based debuggers gdblist.txt or lldblist.txt. There are sample files included. Note that the specification of the version in those files may affect which tests are run. (i.e. not all version may support all features). If they do not exist, the testcase will try to find the lazarus config, but it may not run correctly. 

Note, on Linux the "test/TestApps" dir is expected as "test/testapps". A symbolic link needs to be created. 

Optional a "test/logs" folder should be created. If detected logs will be filed there instead of directly under "tests/". 

When running tests, they will create files/folders in test/TestApps. Some of them are not correctly cleaned up, and need to occasionally be removed by the user. Also if a testcase is not properly exited, it leaves behind files in the test/TestApps/lib folder, which could cause the next test run to fail, so they also need to be removed in that case. 

  
When the test are run, they will show test-entries for each combination of fpc (each version configured), and each external debugger (if applicable). There is a control window, which allows to deselect tests by various criteria. 

  
Each entry in the list of tests (test tree), actually runs many individual tests. Therefore at least on-error a logfile should be written to see what exactly failed. The log also tells how many subtests did fail, for quick access this info is also embedded in the filename.

---

_Source: [https://wiki.freepascal.org/Lazarus_Debugger_Implementation](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Lazarus_Debugger_Implementation)_
