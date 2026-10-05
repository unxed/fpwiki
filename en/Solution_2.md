# Solution 2

│ [**български (bg)**](</Solution_2/bg> "Solution 2/bg") │  [**Deutsch (de)**](</Solution_2/de> "Solution 2/de") │  **English (en)** │  [**français (fr)**](</Solution_2/fr> "Solution 2/fr") │  [**日本語 (ja)**](</Solution_2/ja> "Solution 2/ja") │  [**中文（中国大陆）‎ (zh_CN)**](</Solution_2/zh_CN> "Solution 2/zh CN") │    
****

[ ◄ ](<Programming_Assignment_2.md> "Programming Assignment 2") | [ ▲ ](<Contents.md> "Contents") | [ ► ](<Sequential_control.md> "Sequential control")  
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
    

[ ◄ ](<Programming_Assignment_2.md> "Programming Assignment 2") | [ ▲ ](<Contents.md> "Contents") | [ ► ](<Sequential_control.md> "Sequential control")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Solution_2](https://web.archive.org/web/20231004141207/https://wiki.freepascal.org/Solution_2)_
