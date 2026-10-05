# Raise

│ **[Deutsch (de)](</Raise/de> "Raise/de")** │  **English (en)** │  **[suomi (fi)](</Raise/fi> "Raise/fi")** │    
****

` Raise` is an elementary [statement](<statement.md> "statement"). It explicitly throws an [exception](<Exceptions.md> "Exceptions") and transfers control to an exception handler. The `raise` statement is available and becomes a [reserved word](<Reserved_word.md> "Reserved word") if `{$modeSwitch exceptions+}`. 

## Contents

  * 1 syntax
  * 2 behavior
  * 3 application
  * 4 caveats
  * 5 see also



## syntax

`Raise` requires one [`tObject`](<TObject.md> "TObject") assignment-compatible [pointer](<Pointer.md> "Pointer") expression as an argument. Usually this pointer is a [`class`](<Class.md> "Class") instantiated “just for the occasion”: 
    
    
    program raiseDemo(input, output, stdErr);
    {$modeSwitch class+}
    {$modeSwitch exceptions+}
    type
    	exception = class end;
    begin
    	raise exception.create;
    end.
    

After that an optional `at`-clause may override up to two pointer values: 
    
    
    	raise exception.create at codePointer(123), pointer(456);
    

This is frequently used to make an exception appear to originate from outside some error-handling code, e. g. like this: 
    
    
    raise exception.create at get_caller_addr(get_frame), get_caller_frame(get_frame);
    

Furthermore, _inside_ an [`except` … `on`](<Except.md> "Except") frame the caught exception can be “re-raised” by simply writing 
    
    
    raise;
    

## behavior

Once an exception is “thrown” (in Java and other languages the statement reads `throw`) or “raised” (Delphi/FPC), the surrounding [`try`](<Try.md> "Try") frame (if any) is left _immediately_ and the pointer (usually some kind of exception object) is passed up the hierarchy until there is a matching [`except` … `on`](<On.md> "On") clause: 
    
    
    program catchDemo(input, output, stdErr);
    {$modeSwitch class+}
    {$modeSwitch exceptions+}
    type
    	exception = class end;
    begin
    	try
    		raise exception.create;
    		writeLn('Never written.');
    	except on e: exception do
    		writeLn(stdErr, 'Caught exception');
    		{ Caught exceptions are automatically destroyed }
    		{ before program flow resumes. }
    	end;
    end.
    

Note, the text `Never written.` is never written. The program state, e. g. the values variables possessed before the exception was raised, is not saved, thus it is not possible to simply return to the place where the exception was originally raised. 

If an exception is _not_ caught, or if there is not any surrounding `try … except end` frame to begin with, the exception along with the values in the `at`-clause are passed to an [exception handler](<https://www.freepascal.org/docs-html/rtl/system/exceptproc.html>). If none is installed, the [run-time error](<runtime_error.md> "runtime error") 217 “Unhandled exception occurred” is generated. 

## application

Primarily, exceptions are raised to communicate some error condition that just occurred. It is recommended and common practice to use the [`sysUtils` `unit`](<sysutils.md> "sysutils"). The `sysUtils` `unit` [installs a universal exception handler](<https://www.freepascal.org/docs-html/rtl/system/index.html>) turning all [RTEs](<runtime_error.md> "runtime error") into exceptions. This exception handler expects, and therefore it is recommended that all custom exception classes descend from the [`exception` `class`](<https://www.freepascal.org/docs-html/rtl/sysutils/exception.html>) (or possibly an appropriate descendant). 

While exceptions are usually used to _communicate_ a condition, yet sometimes `raise` is simply (ab‑)used as a [OOP](<object-oriented_programming.md> "object-oriented programming") substitute for [`goto`](<Goto.md> "Goto"). 

## caveats

  * Raising an exception in an `initialization` section of a `unit` may not have the desired behavior, for instance, if `sysUtils`’ exception handler has not been installed yet.
  * [Mulit-threaded](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial") applications need special care.



## see also

  * [Exceptions (Delphi)](<https://docwiki.embarcadero.com/RADStudio/Sydney/en/Exceptions_\(Delphi\)>)
  * [run-time error](<runtime_error.md> "runtime error")

---

_Source: [https://wiki.freepascal.org/Raise](https://web.archive.org/web/20250514123135/https://wiki.freepascal.org/Raise)_
