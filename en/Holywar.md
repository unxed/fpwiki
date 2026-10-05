# Holywar

│ **English (en)** │  **[русский (ru)](<../ru/Holywar.md> "Holywar/ru")** │    
****

The page is dedicated to list all possible pros (mostly) and cons of Pascal language (all kinds of pascal included). 

Pascal has something of a stigma as an "educational, and therefore weak" language, but it has nothing in common with a real life. 

If you're Pascal developer being involved into a next flame about Pascal language, you're encouraged to use the page. 

## Contents

  * 1 Why do we use Pascal
  * 2 Q&A
  * 3 Advantages over C/C++
  * 4 see also



## Why do we use Pascal

(if you don't use (Object) Pascal, please don't edit the section) 

Often programmers are free to choose their tools, especially then coding as a hobby (rather than for their living). Lots of Pascal programmers choose the language because of: 

  * modern Object Pascal is simply one of the most productive languages and it can be used for almost everything. 
    * [Projects using Lazarus](<Projects_using_Lazarus.md> "Projects using Lazarus")
    * [Applications Build With Delphi](<https://delphi.wikia.com/wiki/Good_Quality_Applications_Built_With_Delphi>)
  * well structured modular language
  * safe and clean syntax, great for team work
  * fast compile-time
  * Object Pascal have really nice oop/interfaces implementation



## Q&A

Q: Why there're so many limitations on what expressions i can use in "for" and "while" operators? 

A: to force you to create maintainable and readable code 

  
Q: Pascal doesn't allow to specify complex types without intermediate types. I.e. "an array of pointers to a function that accepts 2 ints and returns a pointer to function that returns a float" <c> float (*(*a[10])(int, int))() </c>

A: Has it any advantage to define no intermediate type? 
    
    
    type
      TFloatRetFunc = function : single;
      TSomeArray = array [0..9] of function (a, b: Integer): TFloatRetFunc;
    

## Advantages over C/C++

  * no preprocessor (better code reading and understanding);
  * no hardly readable and poorly debugged STL;
  * compiler built-in string-type (string ShortString, AnsiString, WideString). As well as still available to use PChar;



  


## see also

  * [Why use Pascal](<Why_use_Pascal.md> "Why use Pascal")

---

_Source: [https://wiki.freepascal.org/Holywar](https://web.archive.org/web/20250116161715/https://wiki.freepascal.org/Holywar)_
