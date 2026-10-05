# Library

│ **English (en)** │

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The reserved word **library** : 

  * belongs to shared library programming;
  * identifies a unit as a shared library (DLL);
  * replaces the reserved word [unit](<Unit.md> "Unit") in shared library units.



Example of the basic structure of a shared library: 
    
    
    library TestLibrary;
    
    {$mode objfpc} {$H+}
    
    uses
      SysUtils;
    
    // library subroutine
    function cvtString(strIn : string) : PChar; cdecl;
      begin
        cvtString := PChar(UpperCase(strIn));
      end;
    
    // exported subroutine(s)
    exports
      cvtString;
    end.

---

_Source: [https://wiki.freepascal.org/Library](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/Library)_
