# Stdcall

│ **English (en)** │

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The modifier **stdcall** : 

  * belongs to the calling conventions for internal and external subroutines;
  * pushes the parameters from right to left on the stack;
  * aligns all the parameters to a default alignment.



Example: 
    
    
     function subTest : string; [stdcall];
     begin
       subTest := 'abc';
     end;
    

Example 2: 
    
    
     ...
     function funcTest(strTestData : Pchar) : LongWord; stdcall; external 'testLibrary.dylib';
     ...

---

_Source: [https://wiki.freepascal.org/Stdcall](https://web.archive.org/web/20250215134239/https://wiki.freepascal.org/Stdcall)_
