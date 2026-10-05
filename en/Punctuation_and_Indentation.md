# Punctuation and Indentation

│ [**български (bg)**](</Punctuation_and_Indentation/bg> "Punctuation and Indentation/bg") │  [**Deutsch (de)**](</Punctuation_and_Indentation/de> "Punctuation and Indentation/de") │  **English (en)** │  [**français (fr)**](</Punctuation_and_Indentation/fr> "Punctuation and Indentation/fr") │  [**日本語 (ja)**](</Punctuation_and_Indentation/ja> "Punctuation and Indentation/ja") │  [**한국어 (ko)**](</Punctuation_and_Indentation/ko> "Punctuation and Indentation/ko") │  [**русский (ru)**](<../ru/Punctuation_and_Indentation.md> "Punctuation and Indentation/ru") │  [**中文（中国大陆）‎ (zh_CN)**](</Punctuation_and_Indentation/zh_CN> "Punctuation and Indentation/zh CN") │    
****

[ ◄ ](<Standard_Functions.md> "Standard Functions") | [ ▲ ](<Contents.md> "Contents") | [ ► ](<Programming_Assignment.md> "Programming Assignment")  
---|---|---  
  
1G - Punctuation and Indentation (author: Tao Yue, state: unchanged) 

Since Pascal ignores end-of-lines and spaces, punctuation is needed to tell the compiler when a statement ends. 

You _must_ have a semicolon following: 

  * the program heading
  * each constant definition
  * each variable declaration
  * each type definition (to be discussed later)
  * almost all statements



The last statement in a `BEGIN-END` block, the one immediately preceding the END, does not require a semicolon. However, it's harmless to add one, and it saves you from having to add a semicolon if suddenly you had to move the statement higher up. 

Indenting is not required. However, it is of great use for the programmer, since it helps to make the program clearer. If you wanted to, you could have a program look like this: 
    
    
    program Stupid; const a=5; b=385.3; var alpha,beta:real; begin 
    alpha := a + b; beta:= b / a end.
    

But it's much better for it to look like this: 
    
    
    program NotAsStupid;
    
    const
      a = 5;
      b = 385.3;
    
    var
      alpha,
      beta : real;
    
    begin (* main *)
      alpha := a + b;
      beta := b / a;
    end. (* main *)
    

In general, indent each block. Skip a line between blocks (such as between the const and var blocks). Modern programming environments (IDE, or Integrated Development Environment) understand Pascal syntax and will often indent for you as you type. You can customize the indentation to your liking (display a tab as three spaces or four?). 

Proper indentation makes it much easier to determine how code works, but is vastly aided by judicious commenting. 

[ ◄ ](<Standard_Functions.md> "Standard Functions") | [ ▲ ](<Contents.md> "Contents") | [ ► ](<Programming_Assignment.md> "Programming Assignment")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Punctuation_and_Indentation](https://web.archive.org/web/20230922154106/https://wiki.freepascal.org/Punctuation_and_Indentation)_
