# UInt16

│ **English (en)** │

  
Back to [data types](<Data_type.md> "Data type"). 

  
Range of values: 0 .. 65535 

Memory requirement: 2 bytes or 16 bits 

A [data field](<Data_field.md> "Data field") of the **UInt16** data type can only contain positive integer values. Assigning other values ​​leads to compiler error messages when the program is compiled and the compilation process is aborted. That is, the executable program is not created. 

Definition of a data field of type UInt16: 
    
    
      var 
        ui16 : uint16;
    

Examples of assigning valid values: 
    
    
      ui16 := 0;
      ui16 := 65535;
    

Examples of assigning invalid values: 
    
    
      ui16 := '0';
      ui16 := '32767';
    

The difference between the two examples is that the upper example is the assignment of literals of the type [Integer](<Integer.md> "Integer"), while the assignment of the lower example is literals of the type [String](<String.md> "String").

---

_Source: [https://wiki.freepascal.org/UInt16](https://web.archive.org/web/20250114101516/https://wiki.freepascal.org/UInt16)_
