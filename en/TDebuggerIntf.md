# TDebuggerIntf

**TDebuggerIntf** is the base class of implementing a debugger as Lazarus-IDE plugin package. The package must have DebuggerIntf package as it's requirements. Any debugger plug-in must implement the class and register via **RegisterDebugger** (called at **Register** of registering unit). 

Implementation notes: 

  * the class is designed to act as asynchronous object. The interaction is implemented in form of commands sent to the class.
  * the command should be put (or not put) into an execution queue. The information about acceptance or denial of the command should be returned as soon as possible.



    Command request is sent from the Main Thread. Thus any actual debugging job should be implemented in a separate thread NOT to impact the user experience.

  * once a command has been handled from the queue, the corresponding event should be called and/or debugger state changed



## Contents

  * 1 Debugger Workflow
    * 1.1 Initialization
    * 1.2 Breakpoints
      * 1.2.1 Validation
      * 1.2.2 Hit
    * 1.3 Execution Address
    * 1.4 Threads Information
  * 2 Minimal Implementation
  * 3 Reference
    * 3.1 RequestCommand
    * 3.2 property Environment
  * 4 Commands
  * 5 Development Hints
  * 6 See Also



## Debugger Workflow

### Initialization

IDE does following steps via TDebuggerIntf methods: 

  * allocate (external debugger executable name is provided) (the state is expected **dsNone**)
  * allocate debug objects (Breakpoints, watches) within TDebuggerIntf
  * assign debugger properties
  * .Init() (as a result of Init method, the debugger should be in **dsIdle** state)
  * assigning environment
  * assigning debugged process information (WorkingDir, FileName, Arguments, etc...)



### Breakpoints

In order to set a break-point there should be a new sub-class of **TGDBBreakPoint** provided. 

The list of break-points is stored and managed at TDBGBreakPoints class. The class constructor requires to specify the class of **TGDBBreakPoint** to be created (which should be the new sub-class) TDBGBreakPoints is created via protected **CreateBreakPoints** method. The method needs to be overridden and the code to allocate TDBGBreakPoints with the proper sub-class used. 

#### Validation

IDE allows to create a breakpoint on any address and or source line. However, the source line where the breakpoint is created might not contain any code or debugging information. In order to indicate if the breakpoint could be stopped at **TGDBBreakPoint** class provides **Valid** property of enumeration **TValidState** type. The property is a public read-only property, however, it could be changed via the protected method **SetValid()**. 

  * vsUnknown - this is the initial value of Valid property. Once the debugger verifies, if the requested break point is a valid point, it should change the value to either Valid or Invalid.
  * vsValid - the breakpoint is a valid breakpoint, and the execution could stop on it.
  * vsInvalid - the breakpoint requested is at the invalid location or debugging information doesn't exist.



#### Hit

Whenever breakpoint is hit, **TGDBBreakPoint.Hit()** method should be called. 

### Execution Address

The execution address or execution line should be shown whenever the debugger is stopped or paused. This could occur whenever the debugger stops on a breakpoint, paused or an error occurs in the debugged process. 

The debugger should call (protected) method **DoCurrent()** , to specify where location of the instructions be executed. The method accepts one parameter, where address and source line of code is specified. At least address field should be populated. It's understood that source line information might not be available, if so Line should be set to 0. 

### Threads Information

The information about threads and each thread call stack is an additional information about debugged process. **TDebuggerIntf** should either populate it whenever the information is available or whenever the information is explicitly requested. 

In order to populate the information the debugger should update **Threads** property (type **TThreadsSupplier**). The property is a list of threads. The list could be cleaned every time before population. 

The list should be populated using **Add** method. **TThreadEntity** class could be either allocated explicitly, or **CreateEntity** method of **Threads** could be used. It is important to remember that **Add** methods allocates a copy of **TThreadEntity** , thus right after the call to **Add** the added object could be freed (and must be freed eventually). 

Once the list of threads is added to **Threads** , it's necessary to validate the data by calling **SetValidity** method, passing **ddsValid** as a parameter. 

Finally, whenever a debugged process is stopped, it's desired to identify the "current" event thread by assinging ID of the thread to **CurrentThreadId** property of **Threads** object. 

**TThreadsSupplier** should be sub-classed in order to get the notification when thread information is needed. (The sub-class should be created in **CreateThreads** of TDebuggerIntf sub-class) The sub-class must override and implement **RequestMasterData** method. The method is called whenever IDE needs the up-to-date information about threads. Similar to **RequestCommand** the method is expected to behave asynchronously and doesn't have to update threads information immediately. 

## Minimal Implementation

The following methods must be implemented for TDebbugerIntf 

  * **GetSupportedCommands** (or **GetCommands**) - returns the set of formats that a debugger can perform
  * **RequestCommand** \- the method actually performing commands. Only commands returned by **GetSupportedCommands** would be passed to this method
  * **Caption** \- the method return user-friendly name of the debugger


    
    
    type
      TMiniDebugger = class(TDebuggerIntf)
      protected
        function RequestCommand(const ACommand: TDBGCommand; const AParams: array of const): Boolean; override;
        function GetSupportedCommands: TDBGCommands; override;
      public
        class function Caption: String; override;
      end;
    
    procedure Register;
    
    implementation
    
    procedure Register;
    begin
      RegisterDebugger(TMiniDebugger);
    end;
    
    class function TMiniDebugger.Caption: String;
    begin
      Result:='Initial Debugger Plug-in';
    end;
    
    function TMiniDebugger.GetSupportedCommands: TDBGCommands;
    begin
      Result := [dcRun, dcStop];
    end;
          
    function TMiniDebugger.RequestCommand(const ACommand: TDBGCommand;
      const AParams: array of const): Boolean;
    begin
      case ACommand of
        dcRun: begin       // the debugger was requested to run
          Result:=true;    // typically it should set the state into dsIdle (to get more data from IDE)
          SetState(dsRun); // the debugger replies as if it started immediately 
        end;
        dcStop: begin      // the debugger was requested to stop
          Result:=true;
          SetState(dsStop); // the debugger replied as if it stopped immediately
        end;
      else
        Result:=false;
      end;
    end;
    

## Reference

### RequestCommand

The method should return **false** , if the command cannot be executed. 

### property Environment
    
    
      property Environment: TStrings read write
    

Environment variables that should be passed (or updated) in a debugged project. Assigning values to Environment triggers dcEnvironment command. 

## Commands

The following commands are called via **RequestCommand** method. 

Command  | Explanation  | Parameters   
---|---|---  
dcRun  | Starts (or Restarts) or Resumes the execution of a debugged process. The debugged process executable and arguments to be used are available through **FileName** and **Arguments** properties of TDebuggerIntf object.  | no parameters   
dcPause  |  | no parameters   
dcStop  |  | no parameters   
dcStepOver  |  | no parameters   
dcStepInto  |  | no parameters   
dcStepOut  |  | no parameters   
dcRunTo  |  | 0 - AnsiString - file name of the source code to run to 1 - Integer - Line number to run to   
dcJumpto  |  | 0 - AnsiString - file name of the source code to jump to 1 - Integer - Line number to run to   
dcAttach  |  | 0 - AnsiString - process ID to attach-to.   
dcDetach  |  | no parameters   
dcBreak  |  |   
dcWatch  |  |   
dcLocal  |  |   
dcEvaluate  |  | 0 - AnsiString - expression to be evaluated 1 - pointer to AnsiString - the string where the result should be saved into 2 - pointer to TDBGType - the reference where the information about the result type should be saved to 3 - Integer - TDBGEvaluateFlags casted into Integer   
dcModify  | Modifies the given expression / Assigns a value to a variable/field/property  | 0 - AnsiString - identified expression 1 - AnsiString - value to be assigned   
dcEnvironment  | Prepares/updates an environmental variable for a debugged process.  | 0 - AnsiString - environment variable in %name%=%value% format 1 - Boolean - the flag, if "true" then the variable needs to be set, "false" if needs to be removed   
dcSetStackFrame  |  |   
dcDisassemble  |  |   
dcStepOverInstr  |  |   
dcStepIntoInstr  |  |   
dcSendConsoleInput  |  |   
  
## Development Hints

  * it might be helpful to recompile DebuggerIntf package, adding the following Custom Options to the compiler options of the package



    

    -dDBG_STATE
    -dDBG_EVENTS
    -dDBG_VERBOSE
    -dDBG_WARNINGS
    -dDBG_DATA_MONITORS
    -dDBG_DISASSEMBLER
    these defines provide more logging output of TDebuggerIntf

  * it might be helpful to rebuild IDE adding the following defines into build Options:



    

    -dVerboseDebugger
    **VerboseDebugger** logs messages debugging calls from IDE itself

## See Also

  * [FpGdbmiDebugger](<FpGdbmiDebugger.md> "FpGdbmiDebugger") \- debugger plug-in, that works over the external GDB debugger, but has a better understanding of Pascal expressions and types

---

_Source: [https://wiki.freepascal.org/TDebuggerIntf](https://web.archive.org/web/20250513171013/https://wiki.freepascal.org/TDebuggerIntf)_
