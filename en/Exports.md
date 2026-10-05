# Exports

│ **[Deutsch (de)](</Exports/de> "Exports/de")** │  **English (en)** │    
****

  
Back to the [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The reserved word **exports** is used to export names when creating a shared library or an executable program. It means that the symbol(s) will be publicly available, and can be imported from other programs. 

  
Example: 
    
    
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

_Source: [https://wiki.freepascal.org/Exports](https://web.archive.org/web/20250516144453/https://wiki.freepascal.org/Exports)_
