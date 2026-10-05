# Basic Pascal Tutorial/Chapter 3/CASE

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_3/CASE/bg> "Basic Pascal Tutorial/Chapter 3/CASE/bg")** │  **English (en)** │  **[español (es)](</Basic_Pascal_Tutorial/Chapter_3/CASE/es> "Basic Pascal Tutorial/Chapter 3/CASE/es")** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_3/CASE/fr> "Basic Pascal Tutorial/Chapter 3/CASE/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_3/CASE/ja> "Basic Pascal Tutorial/Chapter 3/CASE/ja")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_3/CASE/zh_CN> "Basic Pascal Tutorial/Chapter 3/CASE/zh CN")** │    
****

[ ◄ ](<IF.md> "Basic Pascal Tutorial/Chapter 3/IF") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..DO")  
---|---|---  
  
3Cb - CASE (author: Tao Yue, state: changed) 

Case opens a case statement. The case statement compares the value of an ordinal expression to each selector, which can be a [constant](<../../Const.md> "Const"), a subrange, or a list of selectors separated by [commas](<../../Comma.md> "Comma"). Selector field separated to action field by [Colon](<../../Colon.md> "Colon"). 

Suppose you wanted to branch one way if `b` is `1, 7, 2037,` or `5`; and another way if otherwise. You could do it by: 
    
    
    if (b = 1) or (b = 7) or (b = 2037) or (b = 5) then
      Statement1
    else
      Statement2;
    

But in this case, it would be simpler to list the numbers for which you want Statement1 to execute. You would do this with a `case` statement: 
    
    
    case b of
      1,7,2037,5: Statement1;
      otherwise   Statement2
    end;
    

The general form of the `case` statement is: 
    
    
    case selector of
      List1:    Statement1;
      List2:    Statement2;
      ...
      Listn:    Statementn;
      otherwise Statement
    end;
    

The `otherwise` part is optional. When available, it differs from compiler to compiler. Many compilers use the word `else` instead of `otherwise`. 

selector is any variable of an ordinal data type. You may not use Reals! 

Note that the lists must consist of literal values. That is, you must use constants or hard-coded values -- you cannot use variables. 

[ ◄ ](<IF.md> "Basic Pascal Tutorial/Chapter 3/IF") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..DO")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_3/CASE](https://web.archive.org/web/20250401001359/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_3/CASE)_
