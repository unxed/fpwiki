# External

│ **English (en)** │

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The **external** modifier: 

  * belongs to the calling conventions for external subroutines (eg dynamically loaded libraries);
  * allows access to an external subroutine.



  
Examples: 
    
    
      ...
       function funcTest(strTestData : Pchar) : LongWord; cppdecl; external 'Test.dll';
      ...
    
    
    
      ...
       function funcTest(strTestData : Pchar) : LongWord; cppdecl; external 'test.dylib';
      ...

---

_Source: [https://wiki.freepascal.org/External](https://web.archive.org/web/20250516144550/https://wiki.freepascal.org/External)_
