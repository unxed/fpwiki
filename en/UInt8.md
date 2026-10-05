# UInt8

│ **[Deutsch (de)](</UInt8/de> "UInt8/de")** │  **English (en)** │  **[français (fr)](</UInt8/fr> "UInt8/fr")** │    
****

  
Back to [data types](<Data_type.md> "Data type"). 

  
Range of values: 0 .. 255 

Memory requirement: 1 byte or 8 bit 

A [data field](<Data_field.md> "Data field") of the **UInt8** data type can only contain positive integer values. 

Assigning other values ​​leads to compiler error messages when the program is compiled and the compilation process is aborted. That is, the executable program is not created. 

Definition of a data field of type UInt8: 
    
    
    var 
      ui8 : uint8;
    

Examples of assigning valid values: 
    
    
      ui8 := 0;
      ui8 := 255;
    

Examples of assigning invalid values: 
    
    
      ui8 := '0';
      ui8 := '255';
    

The difference between the two examples is that the upper example is the assignment of literals of the type [Integer](<Integer.md> "Integer"), while the assignment of the lower example is literals of the type [String](<String.md> "String").

---

_Source: [https://wiki.freepascal.org/UInt8](https://web.archive.org/web/20241207104535/https://wiki.freepascal.org/UInt8)_
