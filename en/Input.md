# Basic Pascal Tutorial/Chapter 2/Input

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_2/Input/bg> "Basic Pascal Tutorial/Chapter 2/Input/bg")** │  **[Deutsch (de)](</Basic_Pascal_Tutorial/Chapter_2/Input/de> "Basic Pascal Tutorial/Chapter 2/Input/de")** │  **English (en)** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_2/Input/fr> "Basic Pascal Tutorial/Chapter 2/Input/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_2/Input/ja> "Basic Pascal Tutorial/Chapter 2/Input/ja")** │  **[한국어 (ko)](</Basic_Pascal_Tutorial/Chapter_2/Input/ko> "Basic Pascal Tutorial/Chapter 2/Input/ko")** │  **[русский (ru)](<../ru/Basic_Pascal_Tutorial/Chapter_2/Input.md> "Basic Pascal Tutorial/Chapter 2/Input/ru")** │  **[中文（中国大陆）‎ (zh_CN)](</Basic_Pascal_Tutorial/Chapter_2/Input/zh_CN> "Basic Pascal Tutorial/Chapter 2/Input/zh CN")** │    
****

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_1/Solution.md> "Basic Pascal Tutorial/Chapter 1/Solution") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_2/Output.md> "Basic Pascal Tutorial/Chapter 2/Output")  
---|---|---  
  
2A - Input (author: Tao Yue, state: changed) 

Input is what comes into the program. It can be from the keyboard, the mouse, a file on disk, a scanner, a joystick, etc. 

We will not get into mouse input in detail, because that syntax differs from machine to machine. In addition, today's event-driven windowing operating systems usually handle mouse input for you. 

The basic format for reading in data is: 
    
    
    read (Variable_List);
    

`Variable_List` is a series of variable identifiers separated by commas. 

`read` treats input as a stream of characters, with lines separated by a special end-of-line character. `readln`, on the other hand, will skip to the next line after reading a value, by automatically moving past the next end-of-line character: 
    
    
    readln (Variable_List);
    

Suppose you had this input from the user, and `a, b, c,` and `d` were all integers. 
    
    
    45 64 97
    1 2 3
    

Here are some sample `read` and `readln` statements, along with the values read into the appropriate variables. 

Statement(s) | a | b | c | d   
---|---|---|---|---  
read (a); read (b); | 45 | 64 |  |   
readln (a); read (b); | 45 | 1 |  |   
read (a, b, c, d); | 45 | 64 | 97 | 1   
readln (a, b); readln (c, d); | 45 | 64 | 1 | 2   
  
When reading in integers, all spaces are skipped until a numeral is found. Then all subsequent numberals are read, until a non-numeric character is reached (including, but not limited to, a space). 
    
    
    8352.38
    

When an integer is read from the above input, its value becomes `8352`. If, immediately afterwards, you read in a character, the value would be '`.`' since the read head stopped at the first alphanumeric character. 

Suppose you tried to read in two integers. That would not work, because when the computer looks for data to fill the second variable, it sees the '`.`' and stops since it couldn't find any data to read. 

With real values, the computer also skips spaces and then reads as much as can be read. However, many Pascal compilers place one additional restriction: a real that has no whole part must begin with `0.` So `.678` is invalid, and the computer can't read in a real, but `0.678` is fine. 

Make sure that all identifiers in the argument list refer to variables! Constants cannot be assigned a value, and neither can literal values. 

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_1/Solution.md> "Basic Pascal Tutorial/Chapter 1/Solution") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_2/Output.md> "Basic Pascal Tutorial/Chapter 2/Output")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Input](https://web.archive.org/web/20230324092538/https://wiki.freepascal.org/Input)_
