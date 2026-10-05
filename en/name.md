# Name

│ **English (en)** │

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The reserved word **name** : 

  * belongs to the call of external subroutines;
  * is followed by the alias name of the subroutine.



Example: 
    
    
      // Declaration of an external subroutine
       function GlobalMemoryStatusEx(var lpBuffer : TMemoryStatusEx) : BOOL;  stdcall; 
         external 'kernel32.dll' name 'GlobalMemoryStatusEx';

---

_Source: [https://wiki.freepascal.org/name](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/name)_
