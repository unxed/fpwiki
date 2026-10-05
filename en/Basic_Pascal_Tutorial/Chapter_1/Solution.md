# Basic Pascal Tutorial/Chapter 1/Solution

│ **English (en)** │  **[русский (ru)](<../../../ru/Basic_Pascal_Tutorial/Chapter_1/Solution.md>)** │

[ ◄ ](<Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 1/Programming Assignment") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<../Chapter_2/Input.md> "Basic Pascal Tutorial/Chapter 2/Input")  
---|---|---  
  
1Ha - Solution (author: Tao Yue, state: changed) 

Here's one way to solve the programming assignment in the previous section. 
    
    
    (* Author:    Tao Yue
       Date:      19 June 1997
       Description:
          Find the sum and average of five predefined numbers
       Version:
          1.0 - original version
    *)
    
    program SumAverage;
    
    const
       NumberOfIntegers = 5;
    
    var
       A, B, C, D, E : integer;
       Sum : integer;
       Average : real;
    
    begin    (* Main *)
       A := 45;
       B := 7;
       C := 68;
       D := 2;
       E := 34;
       Sum := A + B + C + D + E;
       Average := Sum / NumberOfIntegers;
       writeln ('Number of integers = ', NumberOfIntegers);
       writeln ('Number 1 = ', A);
       writeln ('Number 2 = ', B);
       writeln ('Number 3 = ', C);
       writeln ('Number 4 = ', D);
       writeln ('Number 5 = ', E);
       writeln ('Sum = ', Sum);
       writeln ('Average = ', Average)
    end.     (* Main *)
    

[ ◄ ](<Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 1/Programming Assignment") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<../Chapter_2/Input.md> "Basic Pascal Tutorial/Chapter 2/Input")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Solution](https://web.archive.org/web/20250403163317/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Solution)_
