# Constref

│ **English (en)** │  **[français (fr)](</Constref/fr> "Constref/fr")** │    
****

Version 2.6 of Free Pascal added the **constref** parameter qualifier. 
    
    
    program call_method;
    
    var x : longint ;
    
    procedure test_cr(constref cr: longint);
    begin
      writeln('Parameter 5 has been passed by reference: ', cr );
    end;
    
    begin
      test_cr(5);
      x:=6;
      test_cr(x);
    end.
    

It is like the [ const](<Const.md> "Const") parameter qualifier. This qualifier informs the compiler that within the entire program there is no code that will change the value of the parameter while the procedure/function is executing. 

This means that not only the _parameter_ , but also the _variable_ (in above example: x) passed by the caller (e.g. a global var) must not be changed until the call with the constref parameter has returned. 

In addition to being like [ const](<Const.md> "Const") parameter qualifier, the **constref** qualifier enforces that the parameter is passed by reference. 

This differs from the [ const](<Const.md> "Const") parameters, which may be passed as reference or value depending on what the compiler thinks is best. 

The [ new feature notes](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") for version 2.6 suggest that this can be used for interfacing with external routines in other languages, where this type of parameter passing is required. Other uses of constref may hinder the compiler from optimizing code. 

## See also

  * [const](<Const.md> "Const")
  * [var](<Var.md> "Var")

---

_Source: [https://wiki.freepascal.org/Constref](https://web.archive.org/web/20250327031832/https://wiki.freepascal.org/Constref)_
