# Safecall

│ **English (en)** │

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The **safecall** modifier: 

  * belongs to the calling conventions of internal and external subroutines;
  * works like the [stdcall](<Stdcall.md> "Stdcall") modifier, with the difference that the register contents are saved and restored.



Example: 
    
    
    function subTest : string; [safecall];
    begin
      subTest := 'abc';
    end;
    

Example 2: 
    
    
    ...
    function funcTest(strTestData : Pchar) : LongWord; safecall; external 'testLibrary.dylib';
    ...

---

_Source: [https://wiki.freepascal.org/Safecall](https://web.archive.org/web/20250324233825/https://wiki.freepascal.org/Safecall)_
