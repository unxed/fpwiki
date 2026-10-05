# compile-time error

│ **English (en)** │  **[suomi (fi)](</compile-time_error/fi> "compile-time error/fi")** │    
****

Compile-time error, which means the [program](<Program.md> "Program") did not compile. The [compiler](<Compiler.md> "Compiler") found and reported an error during [compile time](<Compile_time.md> "Compile time"). 

## Fairly common compile time errors

  * Missed [semicolons](</;> ";") \- A fairly common coding error is the omission of the required semicolons. Usually every [Pascal](<Pascal.md> "Pascal") [statement](<statement.md> "statement") ends with a semicolon



## Own compile time error

The [compiler directive](<Compiler_directive.md> "Compiler directive") `{$Fatal Error Message text}` can create your own compile time error: 
    
    
    begin
      // some code ..
    
      // To keep the compilation going
      // you need to change the code at this point
    
      {$Fatal  Error Message text}
    
      // .. here is the code that is not compiled
      // because the compilation stopped in error
    end.
    

## See also

  * [run-time error](<runtime_error.md> "runtime error")

---

_Source: [https://wiki.freepascal.org/compile-time_error](https://web.archive.org/web/20250214022821/https://wiki.freepascal.org/compile-time_error)_
