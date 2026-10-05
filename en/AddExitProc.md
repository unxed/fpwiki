# AddExitProc

│ **English (en)** │

## AddExitProc

With AddExitProc, you can assign a procedure that is called when the program ends; even when terminated by a runtime error. 

Compared to the simple [ExitProc](<ExitProc.md> "ExitProc"), this has the advantage that you don't have to worry about saving and calling up the old ExitProc, this happens automatically in the background with AddExitProc. 

In this example, a file that does not exist is deliberately opened: 
    
    
    program AddExitProc_Example;
    
     const
       MyExitCode = 123;
    
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
       end;
    
     var
       f : file;
    
     begin
       // Assign new ExitProc
       AddExitProc(@end);
    
       // Open a file that doesn't exist
       Assign(f, 'abc.txt');
    
       // This triggers runtime error 2
       Reset(f);
    
       // Cancel with your own code.
       Halt(MyExitCode);
     end.
    

## See also

  * [ExitProc](<ExitProc.md> "ExitProc") and [ExitCode](<ExitProc.md> "ExitProc") \- intercept the end of the program and evaluate ExitCode.

---

_Source: [https://wiki.freepascal.org/AddExitProc](https://web.archive.org/web/20240304013648/https://wiki.freepascal.org/AddExitProc)_
