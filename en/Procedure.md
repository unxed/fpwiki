# Procedure

│ **[Deutsch (de)](</Procedure/de> "Procedure/de")** │  **English (en)** │  **[suomi (fi)](</Procedure/fi> "Procedure/fi")** │  **[français (fr)](</Procedure/fr> "Procedure/fr")** │  **[italiano (it)](</Procedure/it> "Procedure/it")** │  **[русский (ru)](<../ru/Procedure.md> "Procedure/ru")** │    
****

A **procedure** is a [routine](<Routine.md> "Routine") that does not return a value. `procedure` is a [reserved word](<Reserved_word.md> "Reserved word"). 

## Prematurely leaving a procedure

In a procedure the routine [`exit`](<https://www.freepascal.org/docs-html/rtl/system/exit.html>) can be called in order to (prematurely) leave the procedure. `exit` may not be supplied with any parameters, since procedures do not return any value, but [functions](<Function.md> "Function") do. Supplying a parameter to `exit` inside a procedure definition will yield the compile-time error “Error: Procedures cannot return a value”. 

## Invocation

Procedure calls are statements. They may not appear in expressions, since they do not produce a value of any kind. The following example highlights all lines with procedure calls. 
    
    
    program procedureDemo(input, output, stderr);
    
    var
    	x: longint;
    
    procedure foo;
    begin
    	exit;
    	inc(x);
    end;
    
    begin
    	x := 42;
    	foo;
    	writeLn(x);
    end.
    

Note, that `foo` contains unreachable code (`inc(x)` is never executed because of the [unconditional] `exit`). 

## See also

  * [Tutorial: procedures](<Basic_Pascal_Tutorial/Chapter_4/Procedures.md> "Basic Pascal Tutorial/Chapter 4/Procedures")
  * [§ “Procedure statements” in the Reference Guide](<https://www.freepascal.org/docs-html/ref/refsu53.html>)
  * [§ “Procedural types” in the Reference Guide](<https://www.freepascal.org/docs-html/ref/refse17.html>)

---

_Source: [https://wiki.freepascal.org/Procedure](https://web.archive.org/web/20240920204125/https://wiki.freepascal.org/Procedure)_
