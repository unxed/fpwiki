# Basic Pascal Tutorial/Chapter 4/Recursion

│ **English (en)** │

[ ◄ ](<Scope.md> "Basic Pascal Tutorial/Chapter 4/Scope") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Forward_Referencing.md> "Basic Pascal Tutorial/Chapter 4/Forward Referencing")  
---|---|---  
  
4E - Recursion (author: Tao Yue, state: unchanged) 

**Recursion** means allowing a function or procedure to call itself until some limit is reached. 

The summation function, designated by an uppercase letter _sigma_ (Σ) in mathematics, can be written recursively: 
    
    
    function Summation (num : integer) : integer;
    begin
      if num = 1 
      then Summation := 1
      else Summation := Summation(num-1) + num
    end;
    

Suppose you call `Summation` for `3`. 
    
    
    a := Summation(3);
    

  * `Summation(3)` becomes `Summation(2) + 3`.
  * `Summation(2)` becomes `Summation(1) + 2`.
  * At `1`, the recursion stops and becomes `1`.
  * `Summation(2)` becomes `1 + 2 = 3`.
  * `Summation(3)` becomes `3 + 3 = 6`.
  * `a` becomes `6`.



Recursion works backward until a given point is reached at which an answer is defined, and then works forward with that definition, solving the other definitions which rely upon that one. 

All recursive procedures/functions should have a test to stop the recursion, the base condition. Under all other conditions, the recursion should go deeper. If there is no base condition, the recursion will either not take place at all, or become infinite. 

In the example above, the base condition was `if num = 1`. 

[ ◄ ](<Scope.md> "Basic Pascal Tutorial/Chapter 4/Scope") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Forward_Referencing.md> "Basic Pascal Tutorial/Chapter 4/Forward Referencing")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_4/Recursion](https://web.archive.org/web/20250331225716/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_4/Recursion)_
