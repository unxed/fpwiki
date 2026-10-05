# Logging exceptions

│ **English (en)** │  **[русский (ru)](<../ru/Logging_exceptions.md>)** │

## Contents

  * 1 Introduction
    * 1.1 SysUtils unit
    * 1.2 System unit
    * 1.3 Line information
    * 1.4 Stabs
    * 1.5 DWARF
    * 1.6 Unit LCLProc
  * 2 Dump current call stack
  * 3 Dump exception call stack
  * 4 Handling exceptions
    * 4.1 Manual exception handling
    * 4.2 System ExceptProc
    * 4.3 TApplication.OnException
    * 4.4 Handling thread exceptions
    * 4.5 Using map file
    * 4.6 Exceptions in DLL
  * 5 See also
  * 6 External links



## Introduction

A **stacktrace** is sometimes called **backtrace** or **call stack**. This is a list of stack frames placed on the stack containing return address and local variables. A stacktrace is therefore useful to trace back the path of execution of your program (following the calls to procedures). 

In order to obtain a stacktrace, the compiler first has to be directed to generate debug information using switches: 

  * -g - generate debug information (in the default output format for the platform concerned, which is dwarf2 on a lot of platforms)
  * -gl - generate line numbers for debug information
  * -gs - generate older stabs debug information; don't use both -gs and -gw
  * -gw - generate dwarf2 debug information; don't use both -gs and -gw
  * -Xg - use external debug symbol file



  


#### SysUtils unit

[SysUtils](<https://www.freepascal.org/docs-html/rtl/sysutils/exception.html>) contains some routines useful for debugging exceptions. 
    
    
    { Exception handling routines }
    function ExceptObject: TObject;
    function ExceptAddr: Pointer;
    function ExceptFrameCount: Longint;
    function ExceptFrames: PPointer;
    

#### System unit

The system unit contains some stack related routines: 
    
    
    function SysBackTraceStr(Addr:Pointer): ShortString; // Default address to string converter assigned to BackTraceStrFunc
    procedure Dump_Stack(var f : text;bp:pointer); // Dump stack to text file
    procedure DumpExceptionBackTrace(var f:text); // Dump backtrace to text file
    

Procedural variable **BackTraceStrFunc** responsible to translate memory address to string debug information. Default behavior is implemented by **SysBackTraceStr**. 

#### Line information

If the line info debug output is selected (compiler switch -gl), the unit **lineinfo** is automatically included in the program. This unit makes sure that debuggers/exception handlers can find the line numbers of running code. It can be useful if you do not want to deploy a production executable with full debug info, but you do want useful information when errors occur. 

#### Stabs

If the old stabs format (-gs) is used, the **BackTraceStrFunc** function is remapped to **StabBackTraceStr**. 

#### DWARF

If the dwarf debugging format is selected (compiler switch -gw), the unit **lnfodwrf** is automatically included in the program and **BackTraceStrFunc** function is remapped to **DwarfBacktraceStr**. 

### Unit LCLProc

This Lazarus unit has some debug-related functions: 
    
    
    // Debugging
    procedure RaiseGDBException(const Msg: string);
    procedure RaiseAndCatchException;
    procedure DumpExceptionBackTrace;
    procedure DumpStack;
    function GetStackTrace(UseCache: boolean): string;
    procedure GetStackTracePointers(var AStack: TStackTracePointers);
    function StackTraceAsString(const AStack: TStackTracePointers; UseCache: boolean): string;
    function GetLineInfo(Addr: Pointer; UseCache: boolean): string;
    

## Dump current call stack

See also FPC help on dumping stack/exception details: 

  * [DumpExceptionBackTrace](<http://www.freepascal.org/docs-html/rtl/system/dumpexceptionbacktrace.html>)
  * [dump_stack](<http://www.freepascal.org/docs-html/rtl/system/dump_stack.html>)


    
    
    procedure DumpCallStack;
    var
      I: Longint;
      prevbp: Pointer;
      CallerFrame,
      CallerAddress,
      bp: Pointer;
      Report: string;
    const
      MaxDepth = 20;
    begin
      Report := '';
      bp := get_frame;
      // This trick skip SendCallstack item
      // bp:= get_caller_frame(get_frame);
      try
        prevbp := bp - 1;
        I := 0;
        while bp > prevbp do begin
           CallerAddress := get_caller_addr(bp);
           CallerFrame := get_caller_frame(bp);
           if (CallerAddress = nil) then
             Break;
           Report := Report + BackTraceStrFunc(CallerAddress) + LineEnding;
           Inc(I);
           if (I >= MaxDepth) or (CallerFrame = nil) then
             Break;
           prevbp := bp;
           bp := CallerFrame;
         end;
       except
         { prevent endless dump if an exception occured }
       end;
      ShowMessage(Report);
    end;
    

## Dump exception call stack

The call stack of an exception can be obtained through SysUtils functions **ExceptAddr** , **ExceptFrames** and **ExceptFrameCount**. 
    
    
    uses SysUtils;
    
    procedure DumpExceptionCallStack(E: Exception);
    var
      I: Integer;
      Frames: PPointer;
      Report: string;
    begin
      Report := 'Program exception! ' + LineEnding +
        'Stacktrace:' + LineEnding + LineEnding;
      if E <> nil then begin
        Report := Report + 'Exception class: ' + E.ClassName + LineEnding +
        'Message: ' + E.Message + LineEnding;
      end;
      Report := Report + BackTraceStrFunc(ExceptAddr);
      Frames := ExceptFrames;
      for I := 0 to ExceptFrameCount - 1 do
        Report := Report + LineEnding + BackTraceStrFunc(Frames[I]);
      ShowMessage(Report);
      Halt; // End of program execution
    end;
    

## Handling exceptions

### Manual exception handling

Manual executions of exception handler can be inserted in many places in code. 
    
    
    try
      // ... some operation which should raise an exception...
      raise Exception.Create('Test error');
    except
      on E: Exception do 
        DumpExceptionCallStack(E);
    end;
    

### System ExceptProc

If an unhandled exception occurs, the ExceptProc procedure variable is executed. Default behaviour is initialized by procedure **InitExceptions** in unit **System**. If you want to, this procedure can be reassigned to a custom handler. 

An example: 
    
    
    procedure CatchUnhandledException(Obj: TObject; Addr: Pointer; FrameCount: Longint; Frames: PPointer);
    var
      Message: string;
      i: LongInt;
      hstdout: ^Text;
    begin
      hstdout := @stdout;
      Writeln(hstdout^, 'An unhandled exception occurred at $', sysBackTraceStr(addr), ' :');
      if Obj is exception then
       begin
         Message := Exception(Obj).ClassName + ' : ' + Exception(Obj).Message;
         Writeln(hstdout^, Message);
       end
      else
        Writeln(hstdout^, 'Exception object ', Obj.ClassName, ' is not of class Exception.');
      Writeln(hstdout^, BackTraceStrFunc(Addr));
      if (FrameCount > 0) then
        begin
          for i := 0 to FrameCount - 1 do
            Writeln(hstdout^, BackTraceStrFunc(Frames[i]));
        end;
      Writeln(hstdout^,'');
    end;
    

### TApplication.OnException

This event can be used to override default application wide exceptions handling. A custom logging mechanism could provide show custom dialog, log to file, console, sending report to mail, logging to HTTP server, e.g. 
    
    
    procedure TMainForm.CustomExceptionHandler(Sender: TObject; E: Exception);
    begin
      DumpExceptionCallStack;
      Halt; // End of program execution
    end;   
    
    procedure TMainForm.FormCreate(Sender: TObject);
    begin
      Application.OnException := @CustomExceptionHandler;
    end;
    
    procedure TMainForm.ButtonClick(Sender: TObject);
    begin
      raise Exception.Create('Test');
    end;
    

### Handling thread exceptions

Handling exceptions which are raised in threads has to be done manually. The main thread TThread method **Execute** is called from the function **ThreadProc** located in unit **Classes**. 
    
    
    function ThreadProc(ThreadObjPtr: Pointer): PtrInt;
    begin
      ...
      try
        Thread.Execute;
      except
        Thread.FFatalException := TObject(AcquireExceptionObject);
      end; 
      ...
    end;
    

In this function, the Execute method is enclosed in a try-except block and all exceptions are handled by assigning the exception object to the **FatalException** property of TThread object as last occurred exception object. So exceptions are not displayed to the user at all. 

In every thread in the application, a separate try-except block should be inserted to catch all unhandled exceptions by a custom exception handler. 
    
    
    procedure TMyThread.Execute;
    begin
      try
        // some erroneous code
      except
        on E: Exception do 
          CustomExceptionThreadHandler(Self, E);
      end;
    end;
    

Then **CustomExceptionThreadHandler** can get the exception call stack and manage showing error messages or logging log reports to file. As the handler is executed from a thread, showing message dialog has to be done thread safe using the **Synchronize** method. 
    
    
    procedure TMainForm.CustomExceptionHandler(Thread: TThread; E: Exception);
    begin
      Thread.Synchronize(DumpExceptionCallStack);
    end;
    

### Using map file

Use compiler switch -Xm to generate map file. 

### Exceptions in DLL

todo 

## See also

  * [Profiling](<Profiling.md> "Profiling")
  * [Creating a Backtrace with GDB](<Creating_a_Backtrace_with_GDB.md> "Creating a Backtrace with GDB")
  * [MultiLog](<MultiLog.md> "MultiLog") \- A logging package
  * [log4delphi](<log4delphi.md> "log4delphi") \- A logging package



## External links

  * [esprinter](<http://www.ciuly.com/tools/programming/esprinter/index.html>) \- tool to get stacktrace from running FreePascal program specified by process id and thread id (for Win32)
  * [PascalBugReports](<https://github.com/r3code/pascalbugreports>) \- project intended to mimic EurekaLog/MadExcept but has not been worked on for a while
  * [Lazarus Exception Logger](<https://github.com/r3code/lazarus-exception-logger>) \- Extended version of ExceptionLogger

---

_Source: [https://wiki.freepascal.org/Logging_exceptions](https://web.archive.org/web/20250430013736/https://wiki.freepascal.org/Logging_exceptions)_
