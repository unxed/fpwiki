# Basic Pascal Tutorial/Chapter 1/Solution

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_1/Solution/bg> "Basic Pascal Tutorial/Chapter 1/Solution/bg")** │  **[Deutsch (de)](</Basic_Pascal_Tutorial/Chapter_1/Solution/de> "Basic Pascal Tutorial/Chapter 1/Solution/de")** │  **[English (en)](<../../../en/Basic_Pascal_Tutorial/Chapter_1/Solution.md> "Basic Pascal Tutorial/Chapter 1/Solution")** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_1/Solution/fr> "Basic Pascal Tutorial/Chapter 1/Solution/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_1/Solution/ja> "Basic Pascal Tutorial/Chapter 1/Solution/ja")** │  **[한국어 (ko)](</Basic_Pascal_Tutorial/Chapter_1/Solution/ko> "Basic Pascal Tutorial/Chapter 1/Solution/ko")** │  **русский (ru)** │  **[中文（中国大陆）‎ (zh_CN)](</Basic_Pascal_Tutorial/Chapter_1/Solution/zh_CN> "Basic Pascal Tutorial/Chapter 1/Solution/zh CN")** │    
****

[ ◄ ](<Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 1/Programming Assignment/ru") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<../Chapter_2/Input.md> "Basic Pascal Tutorial/Chapter 2/Input/ru")  
---|---|---  
  
Решение

1Ha - Solution (author: Tao Yue, state: unchanged) 

  
Это один из способов решения задачи по программированию в предыдущем разделе. 
    
    
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
       writeln ('Number1 = ', A);
       writeln ('Number2 = ', B);
       writeln ('Number3 = ', C);
       writeln ('Number4 = ', D);
       writeln ('Number5 = ', E);
       writeln ('Sum = ', Sum);
       writeln ('Average = ', Average)
    end.     (* Main *)
    

[ ◄ ](<Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 1/Programming Assignment/ru") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<../Chapter_2/Input.md> "Basic Pascal Tutorial/Chapter 2/Input/ru")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Solution/ru](https://web.archive.org/web/20240304080120/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Solution/ru)_
