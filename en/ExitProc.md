# ExitProc

│ **English (en)** │

## Contents

  * 1 ExitProc
    * 1.1 Example
  * 2 ExitCode
    * 2.1 Terminated due to runtime error
    * 2.2 Forced termination
  * 3 See also



# ExitProc

With ExitProc, you can assign a procedure that is called at the end of the program; even when terminated by a runtime error. 

It is recommended to save the old ExitProc and to call it from the new ExitProc. 

## Example

In this example, a file that does not exist is deliberately opened. 
    
    
    program ExitProc_Example;
    
     const
       MyExitCode = 123;
    
     var
       OldExitProc : Pointer;
    
       procedure end;
       begin
         WriteLn('This is the end');
    
         case ExitCode of
           2: 
             begin
               WriteLn('file not found');
             end;
    
           MyExitCode:
             begin
               WriteLn('It was deliberately terminated with ', MyExitCode);
             end;
    
           else 
             begin
               WriteLn('Error: ', ExitCode);
             end;
         end;
    
         ExitProc := OldExitProc;
       end;
    
     var
       f : file;
    
     begin
       // Save old ExitProc
       OldExitProc := ExitProc;
    
       // Assign new ExitProc.
       ExitProc := @end ;
    
       // Open a file that doesn't exist
       Assign(f, 'abc.txt');
    
       // This triggers runtime error 2
       Reset(f);
    
       // Terminate with your own exit code
       Halt(MyExitCode);
     end.
    

# ExitCode

ExitCode specifies the error code with which the program terminates. Usually, if ExitCode is 0, the program ended normally. 

## Terminated due to runtime error

In this example, if the memory field is exceeded, the exit code is 201 (range check error). 
    
    
      var
       a : array[0 .. 3] of byte;
    
     begin
       OldExitProc := ExitProc;
       ExitProc := @end;
    
       // memory field deliberately exceeded
       a[4] := 12;
    

## Forced termination

If you force a program termination with Halt(), ExitProc is still called. 

If you Halt(num), then num is interpreted as an exit code. In this example, the exit code is 123. 
    
    
      Halt(123);
    

# See also

  * [AddExitProc](<AddExitProc.md> "AddExitProc") \- Add ExitProc procedure at program end.

---

_Source: [https://wiki.freepascal.org/ExitProc](https://web.archive.org/web/20240304022438/https://wiki.freepascal.org/ExitProc)_
