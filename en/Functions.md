# Basic Pascal Tutorial/Chapter 4/Functions

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_4/Functions/bg> "Basic Pascal Tutorial/Chapter 4/Functions/bg")** │  **English (en)** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_4/Functions/fr> "Basic Pascal Tutorial/Chapter 4/Functions/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_4/Functions/ja> "Basic Pascal Tutorial/Chapter 4/Functions/ja")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_4/Functions/zh_CN> "Basic Pascal Tutorial/Chapter 4/Functions/zh CN")** │    
****

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_4/Parameters.md> "Basic Pascal Tutorial/Chapter 4/Parameters") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_4/Scope.md> "Basic Pascal Tutorial/Chapter 4/Scope")  
---|---|---  
  
4C - Functions (author: Tao Yue, state: unchanged) 

Functions work the same way as procedures, but they always _return a single value_ to the main program through its _own name_ : 
    
    
    function Name (parameter_list) : return_type;
    

Functions are called in the main program by using them in expressions: 
    
    
    a := Name (5) + 3;
    

If your function has no argument, be careful not to use the name of the function on the right side of any equation inside the function. That is: 
    
    
    function Name : integer;
    begin
      Name := 2;
      Name := Name + 1
    end.
    

is a no-no. Instead of returning the value 3, as might be expected, this sets up an infinite recursive loop in certain language modes (e.g. {$MODE DELPHI} or {$MODE TP}; other modes require brackets for function call even if the brackets are empty due to no parameters being required for the particular function). Name will call Name, which will call Name, which will call Name, etc. 

The return value is set by assigning a value to the function identifier. 
    
    
    Name := 5;
    

It is generally bad programming form to make use of VAR parameters in functions -- functions should return only one value. You certainly don't want the sin function to change your pi radians to 0 radians because they're equivalent -- you just want the answer 0. 

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_4/Parameters.md> "Basic Pascal Tutorial/Chapter 4/Parameters") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_4/Scope.md> "Basic Pascal Tutorial/Chapter 4/Scope")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Functions](https://web.archive.org/web/20250123133407/https://wiki.freepascal.org/Functions)_
