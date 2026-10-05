# Basic Pascal Tutorial/Chapter 4/Scope

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_4/Scope/bg> "Basic Pascal Tutorial/Chapter 4/Scope/bg")** │  **English (en)** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_4/Scope/fr> "Basic Pascal Tutorial/Chapter 4/Scope/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_4/Scope/ja> "Basic Pascal Tutorial/Chapter 4/Scope/ja")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_4/Scope/zh_CN> "Basic Pascal Tutorial/Chapter 4/Scope/zh CN")** │    
****

[ ◄ ](<Functions.md> "Basic Pascal Tutorial/Chapter 4/Functions") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Recursion.md> "Basic Pascal Tutorial/Chapter 4/Recursion")  
---|---|---  
  
4D - Scope (author: Tao Yue, state: unchanged) 

_Scope_ refers to where certain variables are visible. You have procedures inside procedures, variables inside procedures, and your job is to try to figure out when each variable can be seen by a procedure. 

A global variable is a variable defined in the main program. Any subprogram can see it, use it, and modify it. All subprograms can call themselves, and can call all other subprograms defined before it. 

The main point here is: within any block of code (procedure, function, whatever), the only identifiers that are visible are those defined before that block and either in or outside of that block. 
    
    
    program ScopeDemo;
    var A : integer;
    
      procedure ScopeInner;
      var A : integer;
      begin
        A := 10;
        writeln (A)
      end;
    
    begin (* Main *)
      A := 20;
      writeln (A);
      ScopeInner;
      writeln (A);
    end. (* Main *)
    

The output of the above program is: 
    
    
    20
    10
    20
    

The reason is: if two variables with the same identifiers are declared in a subprogram and the main program, the main program sees its own, and the subprogram sees its own (not the main's). The most local definition is used when one identifier is defined twice in different places. 

Here's a scope chart which basically amounts to an indented copy of a program with just the variables minus the logic. By stripping away the application logic and enclosing blocks in boxes, it is sometimes easier to see where certain variables are available. 

[![Scope.gif](https://wiki.freepascal.org/images/4/47/Scope.gif)](</File:Scope.gif>)

  * Everybody can see global variables `A, B`, and `C`.
  * However, in procedure `Alpha` the global definition of `A` is replaced by the local definition.
  * `Beta1` and `Beta2` can see variables `VCR, Betamax`, and `cassette`.
  * `Beta1` cannot see variable `FailureToo`, and `Beta2` cannot see `Failure`.
  * No subprogram except `Alpha` can access `F` and `G`.
  * Procedure `Beta` can call `Alpha` and `Beta`.
  * Function `Beta2` can call any subprogram, including itself (the main program is not a subprogram).

[ ◄ ](<Functions.md> "Basic Pascal Tutorial/Chapter 4/Functions") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Recursion.md> "Basic Pascal Tutorial/Chapter 4/Recursion")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_4/Scope](https://web.archive.org/web/20250401033619/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_4/Scope)_
