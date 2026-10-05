# Exceptions

│ **English (en)** │

[Free Pascal](<FPC.md> "FPC") supports exceptions. Exceptions are useful for error handling and avoiding resource leaks. However, bear in mind that exceptions have a performance impact, i.e. exceptions are expensive (in processor time). 

The official documentation is here: [Reference guide chapter 17](<https://www.freepascal.org/docs-html/ref/refch17.html>). 

By default exceptions are disabled. You can opt in by using the [ObjFPC](<Mode_ObjFPC.md> "Mode ObjFPC") or [DELPHI](<Mode_Delphi.md> "Mode Delphi") [Compiler Mode](<Compiler_Mode.md> "Compiler Mode"), or adding -Sx to the [compiler](<Compiler.md> "Compiler") [commandline](<Command-line_interface.md> "Command-line interface"). This enables the [`try`](<Try.md> "Try"), [`raise`](<Raise.md> "Raise"), [`except`](<Except.md> "Except"), and [`finally`](<Finally.md> "Finally") [reserved words](<Reserved_word.md> "Reserved word") for use in your own code, but it doesn't enable exception generation from the [RTL](<RTL.md> "RTL"). To enable the RTL to raise exceptions instead of generating [runtime errors](<runtime_error.md> "runtime error"), use the SysUtils unit in your [program](<Program.md> "Program"). 

The base `Exception` [class](<Class.md> "Class") is found in the [SysUtils](<https://www.freepascal.org/docs-html/rtl/sysutils/exception.html>) unit. All exceptions should preferably use this class or its descendants. 

SysUtils automatically sets up a basic exception catcher, so any otherwise uncaught exception is displayed in human-readable form, as the program terminates. To generate readable callstacks from caught exceptions in your own code, without necessarily terminating the program, you can use SysUtils functions `ExceptAddr`, `ExceptFrames`, `ExceptFrameCount`, and `BackTraceStrFunc`. 

  
While it is possible to raise an exception with any class descending from TObject, it is recommended to use Exception as the basis of exception class objects: the Exception class introduces properties to associate a message and a help context with the exception being raised. What is more, the SysUtils unit sets the necessary hooks to catch and display unhandled exceptions: in such cases, the message displayed to the end user, will be the message stored in the exception class. 

On Windows with SEH using try … except-blocks (without using the SysUtils unit) will not behave as expected (from FPC 3.2.0 on). This due to a bug that will, in all probability be fixed at some stage. In the mean time, the very simple solution is to "use SysUtils". 

## Contents

  * 1 Examples
    * 1.1 Error handling
    * 1.2 Cleaning up resources
    * 1.3 Exception leaking
    * 1.4 Signalling problems
    * 1.5 Using specialised exception types to signal different problems
    * 1.6 Re-raising exceptions
  * 2 Exception classes
  * 3 Best practice
  * 4 Performance
  * 5 Further reading



## Examples

Note that Pascal uses different [keywords](<Keyword.md> "Keyword") than some other languages: `raise` instead of `throw`, and `except` instead of `catch`. Also, as a compiler design choice, each `try`-block can pair with either an `except` or `finally` block, but not both at the same time. You'll need to nest `try`-blocks if you need `except` and `finally` protecting the same code. 

### Error handling
    
    
    uses sysutils;
    
    begin
      try
        // Do something that might go wrong.
      except
        begin
          // Try to recover or show the user an error message.
        end;
      end;
    end.
    

### Cleaning up resources
    
    
    try
      // Do something that might go wrong.
    finally
      // Clean-up code that is always called even if an exception was raised.
    end;
    

### Exception leaking

Finally-blocks don't destroy exception objects. Any exception that reaches the program's "end." will trigger a memory leak warning. An extra except block can be used to consume such exceptions. This is [Delphi-compatible](<https://bugs.freepascal.org/view.php?id=33650>). 
    
    
    begin
      try
        // Your main program, where an exception object is created.
      finally
        try
          // Clean-up code.
        except
        end;
      end;
    end.
    

### Signalling problems
    
    
    raise Exception.Create('Helpful description of what went wrong');
    

### Using specialised exception types to signal different problems
    
    
    type EMyLittleException = Class(Exception);
    
    begin
      try
        raise EMyLittleException.Create('Foo');
      except
        on E : EMyLittleException do writeln(E.Message);
        on E : Exception do writeln('This is not my exception!');
        else writeln('This is not an Exception-descendant at all!');
      end;
    end;
    

### Re-raising exceptions
    
    
    try
      // Do something that might go wrong.
    except
      // Try to recover or show the user an error message.
      if recoveredSuccessfully = FALSE then
        raise;
    end;
    

  


## Exception classes

SysUtils defines and raises many specific [exception classes](<https://www.freepascal.org/docs-html/rtl/sysutils/index-4.html>). 

With SysUtils included in your program, and exceptions enabled, various [runtime errors](<https://www.freepascal.org/docs-html/user/userap4.html>) are changed into exceptions. Processor-level interrupts like SIGSEGV or SIGFPU, which normally translate to run-time errors, are also changed to exceptions. For example, run-time error 200 (division by zero) becomes EDivByZero or EZeroDivide, while run-time error 216 (a general protection fault) becomes EAccessViolation. 

  


## Best practice

  * Raise exceptions to signal that an operation could not be completed, where it normally should have succeeded.
  * Do not use exceptions as part of expected control flow. Instead, add checks for common error conditions and return error values the old-fashioned way. For example, input parsing problems or file existence fails are usually not truly exceptional.
  * But do raise an exception if it's critical that the error is noticed; programmers may forget to check for returned error values.
  * Naming convention: Prefix exception class names with a capital E.
  * Re-raise exceptions in `except`-blocks using `raise;` if you were unable to recover from the problem; this preserves the original exception's callstack.
  * Be careful about using the catch-all base `Exception` class, since the underlying code or OS/filesystem might produce an unanticipated exception that slips through your exception handler, potentially corrupting the program state.
  * When writing a unit or library, if exceptions are used at all, they should be documented clearly up front. This way the unit user knows what to expect.
  * Keep exception handling away from code that needs to run as fast as possible.



  


## Performance

Compiler optimisation levels don't make much difference. All exception blocks use a small amount of wrapper code that can't be optimised away. There are different ways for a compiler to produce exception code, with varying performance implications. As of FPC 3.0.4, the default exception mechanism uses the standard setjmp/longjmp style. FPC also supports OS-provided Structured Exception Handling; this is the default on Win64, and can be enabled on Win32 (recompile the compiler with `$define TEST_WIN32_SEH`). Other mechanisms may be added to FPC in the future. 

To get a better feel for what's going on, try writing a small test program and compiling it with the `-a` switch. This leaves behind a human-readable assembly file. 

Notes on the setjmp/longjmp method: 

  * `Try`-statements insert some extra address calculations, a stack push and pop, some direct jumps, and a conditional jump. This is the setup cost of an exception frame, even if no exception is raised.
  * `Except`-statements insert the above, plus a push and pop, more direct jumps, and another conditional jump.
  * `Finally`-statements add a pop and a conditional jump.
  * `Raise`-statements create a string and pass control to FPC's exception raiser. The string creation itself can spawn an implicit exception frame for the function/method, due to dynamic string allocations, even if the `raise` is not executed. This is why in e.g. container types one often sees the `raise` moved to a separate local (private) procedure. This localises the exception frame to that procedure, so the performance hit is only taken if the procedure is called.



Furthermore, generating a human-readable callstack from a raised exception involves time-consuming stack traversal and string manipulation. 

With all that said, outside of heavy processing code, the convenience of exceptions usually outweighs their performance impact. Don't feel bad about using them if it makes your code better. 

## Further reading

[Logging exceptions](<Logging_exceptions.md> "Logging exceptions")

[Avoiding implicit try finally section](<Avoiding_implicit_try_finally_section.md> "Avoiding implicit try finally section")

[Exceptions vs Error codes](<http://codenewsfast.com/cnf/article/0/permalink.art-ng53q201750>) \- a situation where exceptions might be a danger in some code (see middle of this page)

---

_Source: [https://wiki.freepascal.org/Exceptions](https://web.archive.org/web/20250508233644/https://wiki.freepascal.org/Exceptions)_
