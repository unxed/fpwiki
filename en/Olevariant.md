# Olevariant

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

  
Back to [data types](<Data_type.md> "Data type"). 

  
Memory required for 32-bit compilation: 16 bytes or 128 bits 

Memory required for 64-bit compilation: 24 bytes or 192 bits 

Property: The data type OleVariant is a variant data type that is used for OLE automation (automation of other programs). 

Declaration of a data field of the data type **OleVariant** : 
    
    
    var
       varOle : OleVariant ;
    Create an OleObject:
      begin
       ...
       varOle : = CreateOleObject ( 'Excel.Application' ) ;
       ...
     end ;
    Release of a data field of the data type OleVariant: 
      begin
       ...
       varOle : = Unassigned ;
       ...
     end ;
    

You can find further examples for use on the topic of OleVariant under the topic of software automation.

---

_Source: [https://wiki.freepascal.org/Olevariant](https://web.archive.org/web/20220517150107/https://wiki.freepascal.org/Olevariant)_
