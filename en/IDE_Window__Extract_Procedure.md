# IDE Window: Extract Procedure

│ [**Deutsch (de)**](</IDE_Window:_Extract_Procedure/de> "IDE Window: Extract Procedure/de") │  **English (en)** │  [**español (es)**](</IDE_Window:_Extract_Procedure/es> "IDE Window: Extract Procedure/es") │  [**suomi (fi)**](</IDE_Window:_Extract_Procedure/fi> "IDE Window: Extract Procedure/fi") │  [**français (fr)**](</IDE_Window:_Extract_Procedure/fr> "IDE Window: Extract Procedure/fr") │  [**русский (ru)**](<../ru/IDE_Window__Extract_Procedure.md> "IDE Window: Extract Procedure/ru") │  [**slovenčina (sk)**](</IDE_Window:_Extract_Procedure/sk> "IDE Window: Extract Procedure/sk") │  [**中文（中国大陆）‎ (zh_CN)**](</IDE_Window:_Extract_Procedure/zh_CN> "IDE Window: Extract Procedure/zh CN") │    
****

Abstract: "Extract Procedure" takes some selected pascal statements and creates a new procedure/method from this code. This tool is useful to split big procedures or to easily create a new procedure from some code. 

Basic example: 
    
    
     procedure DoSomething;
     begin
       CallSomething;
     end;

Select the line "CallSomething;" and do Extract Proc. A dialog pop ups and you can select the type and name of the procedure to create. For example: procedure, "NewProc". Result: 
    
    
     procedure NewProc;
     begin
       CallSomething;
     end;
     
     procedure DoSomething;
     begin
       NewProc;
     end;

You can see, that the new procedure "NewProc" was created with the selection as body and the old code was replaced by a call. 

Local Variables and Parameters:  
"Extract Proc" scans for used variables and automatically creates the parameter list and local variables. Example: 
    
    
     procedure TForm1.DoSomething(var Erni, Bert: integer);
     var
       i: Integer; // Comment
     begin
       Erni:=Erni+Bert;
       for i:=Erni to 5 do begin
       |
       end;
     end;

Select the for loop and create a new Procedure "NewProc". The local variable i is only used in the selection, so it will be moved to the new procedure. Erni is also used in the remaining code, so it will become a parameter. 

Result: 
    
    
     procedure NewProc(const Erni: integer);
     var
       i: Integer; // Comment
     begin
       for i:=Erni to 5 do begin
       |
       end;
     end;
     
     procedure TForm1.DoSomething(var Erni, Bert: integer);
     begin
       Erni:=Erni+Bert;
       NewProc(Erni);
     end;

You can see "i" was moved to the new procedure (Note: including its comment) and Erni. 

Limitations:  
Pascal is a very powerful language, so don't expect it will work with every code. Current limits/ToDos: 

  * check if selection bounds on statement bounds
  * "with" statements

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Extract_Procedure](https://web.archive.org/web/20190922224457/https://wiki.freepascal.org/IDE_Window%3A_Extract_Procedure)_
