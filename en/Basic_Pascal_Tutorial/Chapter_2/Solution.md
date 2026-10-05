# Basic Pascal Tutorial/Chapter 2/Solution

│ **English (en)** │

[ ◄ ](<Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 2/Programming Assignment") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<../Chapter_3/Sequential_control.md> "Basic Pascal Tutorial/Chapter 3/Sequential control")  
---|---|---  
  
2Fa - Solution (author: Tao Yue, state: unchanged) 
    
    
    (* Author:    Tao Yue
       Date:      19 June 1997
       Description:
          Find the sum and average of five predefined numbers
       Version:
          1.0 - original version
          2.0 - read in data from keyboard
    *)
    
    program SumAverage;
    
    const
       NumberOfIntegers = 5;
    
    var
       A, B, C, D, E : integer;
       Sum : integer;
       Average : real;
    
    begin    (* Main *)
       write ('Enter the first number: ');
       readln (A);
       write ('Enter the second number: ');
       readln (B);
       write ('Enter the third number: ');
       readln (C);
       write ('Enter the fourth number: ');
       readln (D);
       write ('Enter the fifth number: ');
       readln (E);
       Sum := A + B + C + D + E;
       Average := Sum / 5;
       writeln ('Number of integers = ', NumberOfIntegers);
       writeln;
       writeln ('Number1:', A:8);
       writeln ('Number2:', B:8);
       writeln ('Number3:', C:8);
       writeln ('Number4:', D:8);
       writeln ('Number5:', E:8);
       writeln ('================');
       writeln ('Sum:', Sum:12);
       writeln ('Average:', Average:10:1);
    end.
    

[ ◄ ](<Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 2/Programming Assignment") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<../Chapter_3/Sequential_control.md> "Basic Pascal Tutorial/Chapter 3/Sequential control")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_2/Solution](https://web.archive.org/web/20250404071331/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_2/Solution)_
