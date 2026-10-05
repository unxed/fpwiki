# Basic Pascal Tutorial/Chapter 4/Forward Referencing

│ **English (en)** │

[ ◄ ](<Recursion.md> "Basic Pascal Tutorial/Chapter 4/Recursion") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 4/Programming Assignment")  
---|---|---  
  
4F - Forward Referencing (author: Tao Yue, state: unchanged) 

After all these confusing topics, here's something easy. 

Remember that procedures/functions can only see variables and other subprograms that have already been defined. Well, there is an exception. 

If you have two subprograms, each of which calls the other, you have a dilemma that no matter which you put first, the other still can't be called from the first. 

To resolve this chicken-and-the-egg problem, use _forward referencing_. 
    
    
    procedure Later (parameter list); forward;
    
    procedure Sooner (parameter list);
    begin
      ...
      Later (parameter list);
    end;
    ...
    procedure Later;
    begin
      ...
      Sooner (parameter list);
    end;
    

The same goes for functions. Just stick a forward; at the end of the heading. 

[ ◄ ](<Recursion.md> "Basic Pascal Tutorial/Chapter 4/Recursion") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 4/Programming Assignment")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_4/Forward_Referencing](https://web.archive.org/web/20250401040551/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_4/Forward_Referencing)_
