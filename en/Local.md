# Local

│ **[Deutsch (de)](</Local/de> "Local/de")** │  **English (en)** │    
****

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The **local** modifier is part of Linux programming and has no meaning for Windows. It was introduced for compatibility with Kylix. 

The local modifier allows the compiler to optimize functions/procedures. A local function/procedure cannot be in the interface section of a unit. A local function/procedure can only be in the implementation section of the unit from which it follows that it cannot be exported from a library. 
    
    
     procedure subHello(); local;
     begin
       ...
     end;

---

_Source: [https://wiki.freepascal.org/Local](https://web.archive.org/web/20250324224020/https://wiki.freepascal.org/Local)_
