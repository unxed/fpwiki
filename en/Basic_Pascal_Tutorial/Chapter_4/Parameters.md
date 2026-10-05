# Basic Pascal Tutorial/Chapter 4/Parameters

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_4/Parameters/bg> "Basic Pascal Tutorial/Chapter 4/Parameters/bg")** │  **English (en)** │  **[español (es)](</Basic_Pascal_Tutorial/Chapter_4/Parameters/es> "Basic Pascal Tutorial/Chapter 4/Parameters/es")** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_4/Parameters/fr> "Basic Pascal Tutorial/Chapter 4/Parameters/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_4/Parameters/ja> "Basic Pascal Tutorial/Chapter 4/Parameters/ja")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_4/Parameters/zh_CN> "Basic Pascal Tutorial/Chapter 4/Parameters/zh CN")** │    
****

[ ◄ ](<Procedures.md> "Basic Pascal Tutorial/Chapter 4/Procedures") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Functions.md> "Basic Pascal Tutorial/Chapter 4/Functions")  
---|---|---  
  
4B - Parameters (author: Tao Yue, state: unchanged) 

A parameter list can be included as part of the procedure heading. The parameter list allows variable values to be transferred from the main program to the procedure. The new procedure heading is: 
    
    
    procedure Name (formal_parameter_list);
    

The parameter list consists of several parameter groups, separated by semicolons: 
    
    
    param_group_1; param_group_2; ... ; param_group_n
    

Each parameter group has the form: 
    
    
    identifier_1, identifier_2, ... , identifier_n : data_type
    

The procedure is called by passing arguments (called the actual parameter list) of the same number and type as the formal parameter list. 
    
    
    procedure Name (a, b : integer; c, d : real);
    begin
      a := 10;
      b := 2;
      writeln (a, b, c, d)
    end;
    

Suppose you called the above procedure from the main program as follows: 
    
    
    alpha := 30;
    Name (alpha, 3, 4, 5);
    

When you return to the main program, what is the value of `alpha`? 30. Yet, `alpha` was passed to `a`, which was assigned a value of 10. What actually happened was that `a` and `alpha` are totally distinct. The value in the main program was not affected by what happened in the procedure. 

This is called **call-by-value**. This passes the **value** of a variable to a procedure. 

Another way of passing parameters is **call-by-reference**. This creates a **link** between the formal parameter and the actual parameter. When the formal parameter is modified in the procedure, the actual parameter is likewise modified. _Call-by-reference_ is activated by preceding the parameter group with a `VAR`: 
    
    
    VAR identifier_1, identifier_2, ..., identifier_n : datatype;
    

In this case, constants and literals are not allowed to be used as actual parameters because they might be changed in the procedure. 

Here's an example which mixes _call-by-value_ and _call-by-reference_ : 
    
    
    procedure Name (a, b : integer; VAR c, d : integer);
    begin
      c := 3;
      a := 5
    end;
    
    begin
      alpha := 1;
      gamma := 50;
      delta := 30;
      Name (alpha, 2, gamma, delta);
    end.
    

Immediately after the procedure has been run, `gamma` has the value 3 because `c` was a reference parameter, but `alpha` still is 1 because `a` was a value parameter. 

This is a bit confusing. Think of call-by-value as copying a variable, then giving the copy to the procedure. The procedure works on the copy and discards it when it is done. The original variable is unchanged. 

Call-by-reference is giving the actual variable to the procedure. The procedure works directly on the variable and returns it to the main program. 

In other words, call-by-value is a one-way data transfer: main program to procedure. Call-by-reference goes both ways. 

[ ◄ ](<Procedures.md> "Basic Pascal Tutorial/Chapter 4/Procedures") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Functions.md> "Basic Pascal Tutorial/Chapter 4/Functions")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_4/Parameters](https://web.archive.org/web/20250404194117/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_4/Parameters)_
