# $A

│ **[Deutsch (de)](</$A/de> "$A/de")** │  **English (en)** │    
****

  
Back to [local compiler directives](<local_compiler_directives.md> "local compiler directives"). 

  
The local compiler directives {$ALIGN 1} or {$A1}, {$ALIGN 2} or {$A2}, {$ALIGN 4} or {$A4}, {$ALIGN 8} or {$A8}, {$ALIGN 16} or {$A16} and {$ALIGN 32} or {$A32} determine the alignment of records. The number signifies the alignment boundary in bytes. 

Packing records reduces the amount of memory required at the expense of slowing down processing speed. 

These directives are related to the directive **$PACKRECORDS**. 

Example: 
    
    
      {$A8}      // the result is the same as {$PACKRECORDS 8}
    
      {$ALIGN 8} // the result is the same as {$PACKRECORDS 8}

---

_Source: [https://wiki.freepascal.org/$A](https://web.archive.org/web/20221205214123/https://wiki.freepascal.org/$A)_
