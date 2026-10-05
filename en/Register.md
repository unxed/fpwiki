# Register

│ **English (en)** │  **[русский (ru)](<../ru/Register.md>)** │

  
Back to [ reserved words](<Reserved_words.md> "Reserved words")

  
The [modifier](<modifier.md> "modifier") **register** : 

  * belongs to the calling conventions of internal and external subroutines;
  * is for compatibility with Delphi;
  * has been supported since FPC 1.9.x;
  * is used to call the first three parameters in the register.



Example: 
    
    
    function subTest: string; [register];
    begin
       subTest: = 'abc';
    end;
    

Example 2: 
    
    
    function funcTest (strTestdaten: Pchar): LongWord; register; external 'Test.dll';

---

_Source: [https://wiki.freepascal.org/Register](https://web.archive.org/web/20240304010641/https://wiki.freepascal.org/Register)_
