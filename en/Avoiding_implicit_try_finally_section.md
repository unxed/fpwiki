# Avoiding implicit try finally section

│ **English (en)** │  **[русский (ru)](<../ru/Avoiding_implicit_try_finally_section.md>)** │

## Contents

  * 1 Overview
  * 2 Possible solutions
  * 3 Risks and when to apply
  * 4 Demo program



## Overview

When optimizing code it helps to know that the [compiler](<Compiler.md> "Compiler") will wrap certain code constructs in an implicit [`try`](<Try.md> "Try") … [`finally`](<Finally.md> "Finally") statement. This is needed whenever you use [variables](<Variable.md> "Variable") such as [AnsiString](<Ansistring.md> "Ansistring"), [Variant](<Variant.md> "Variant") or [dynamic arrays](<Dynamic_array.md> "Dynamic array") which require [initialization](<Initialization.md> "Initialization") and [finalization](<Finalization.md> "Finalization") (i.e. where the standard [procedures](<Procedure.md> "Procedure") `initialize` and `finalize` are needed for correct allocation and release of acquired memory). 

For example, a procedure like 
    
    
    procedure doSomething;
    var
      msg: ansiString;
    begin
      // do something with msg
    end;
    

is actually expanded by the compiler to look like this (difference highlighted): 
    
    
    procedure doSomething;
    var
      msg: ansiString;
    begin
      initialize(msg);
      try
        // do something with msg
      finally
        finalize(msg);
      end;
    end;
    

The compiler thereby ensures that the reference count of `msg` will be properly decremented when `procedure doSomething` exits with [exception](<Exceptions.md> "Exceptions"). However, often this may have significant adverse effects on the generated code's speed. 

This is issue was a subject on the `fpc-devel` list in the [`TList` slowness classes](<http://www.mail-archive.com/fpc-devel@lists.freepascal.org/msg01367.html>) thread. 

Note, that temporary `ansiString` variables can be created _implicitly_. The only way to be completely certain about what actually is being done is to read the [assembler](<Assembler.md> "Assembler") output. 

## Possible solutions

  * use [`{$implicitexceptions off}`](</index.php?title=$implicitExceptions&action=edit&redlink=1> "$implicitExceptions \(page does not exist\)"): Ensure this applies to release versions only. Debugging can become cumbersome with that switch especially locating memory leaks and corruption.
  * split off rarely used code that causes an implicit `try…finally` into separate procedures. (You can use nested procedures)
  * use [`const` parameters](<Const.md> "Const") rather than value parameters. This avoids the need to change `refcount` but temporary variables could still be an issue.
  * use [global variables](<Global_variables.md> "Global variables"): You have to be careful with reentrancy issues here though and temporary variables could still be an issue.
  * use non-reference-counted types like [`shortstring`](<ShortString.md> "ShortString").



## Risks and when to apply

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** These exception frames are generated for a reason. If you leave them out any exception in that code will leave a memory leak

In 2007 [`{$implicitExceptions}`](</index.php?title=sImplicitExceptions&action=edit&redlink=1> "sImplicitExceptions \(page does not exist\)") was added to the [`strutils`](<https://www.freepascal.org/docs-html/rtl/strutils/index.html>) [unit](<Unit.md> "Unit"). For this, the following approach was followed: 

  * A [routine](<Routine.md> "Routine") that calls a routine that [raises](<Raise.md> "Raise") exceptions is unsafe – e.g. [`strToInt`](<https://www.freepascal.org/docs-html/rtl/sysutils/strtoint.html>), but not [`strToIntDef`](<https://www.freepascal.org/docs-html/rtl/sysutils/strtointdef.html>).
  * A routine that raises exceptions itself is unsafe.
  * Very large routines are not worth the trouble, because of the risk and low gains – e.g. [date formatting](<https://www.freepascal.org/docs-html/rtl/sysutils/datetimeroutines.html>) routines.
  * Floating point usage can raise exceptions that are converted into catchable exceptions by [`sysUtils`](<sysutils.md> "sysutils"). I'm not sure if this really is sufficient reason, but I skipped floating point using routines initially for this reason.



If you detect problems with these changes please contact [Marco](</User:Marcov> "User:Marcov"). 

## Demo program

Below is a small demo [program](<Program.md> "Program") that 

  * When run, clearly shows that avoiding an implicit `try … finally`-block can make code a lot faster. When I run this program on my system, I get


    
    
    time of fooNormal: 141
    time of fooFaster: 17
    

  * Shows a trick how to avoid implicit `try … finally`-block (without changing the meaning or safety of the code) in some cases (when you don't need to actually use that [`AnsiString`](<Ansistring.md> "Ansistring")/[`Variant`](<Variant.md> "Variant")/[something](<Data_type.md> "Data type") every time procedure is called but e.g. only if some parameter has some particular value).


    
    
    program implicitExceptionDemo;
    
    // for exceptions
    {$mode objfpc}
    // data type 'string' refers to 'ansistring'
    {$longstrings on}
    
    uses
      {$IFDEF UNIX}
      BaseUnix, Unix, // needed only to implement clock()
      {$ENDIF}
      sysUtils;
    
    function clock(): int64;
    {$IFDEF UNIX}
    var
      dummy: tms;
    begin
      clock := fpTimes(dummy);
    end;
    {$ELSE}
    var
      TS: TTimeStamp;
    begin
      TS := DateTimeToTimeStamp(Now);
      result := TS.Time;
    end;
    {$ENDIF}
    
    // When fooNormal() and fooFaster() are called
    // i is always >= 0, so no exception is ever actually raised,
    // and string constants are not really used.
    
    procedure fooNormal(i: integer);
    var
      s: string;
    begin
      if i = -1 then
      begin
        s := 'Some operation with AnsiString';
        raise Exception.Create(s);
      end;
    end;
    
    procedure fooFaster(i: integer);
      //
      procedure RaiseError;
      var
        s: string;
      begin
        s := 'Some operation with AnsiString';
        raise Exception.Create(s);
      end;
      //
    begin
      if i = -1 then
      begin
        RaiseError;
      end;
    end;
    
    // M A I N =================================================
    const
      testCount = 10000000;
    var
      i: integer;
      start: int64;
    begin
      // normal version
      start := clock();
      for i := 0 to testCount do
      begin
        fooNormal(i);
      end;
      writeLn('time of fooNormal: ', clock() - start);
    
      // faster version
      start := clock();
      for i := 0 to testCount do
      begin
        fooFaster(i);
      end;
      writeLn('time of fooFaster: ', clock() - start);
    end.
    

By putting `raiseError` into a nested [scope](<Scope.md> "Scope") of `fooFaster`, exception handling does not become part of the main thread of execution.

---

_Source: [https://wiki.freepascal.org/Avoiding_implicit_try_finally_section](https://web.archive.org/web/20250125104242/https://wiki.freepascal.org/Avoiding_implicit_try_finally_section)_
