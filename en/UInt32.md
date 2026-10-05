# UInt32

│ **English (en)** │

  
Back to [data types](<Data_type.md> "Data type"). 

  
Range of values: 0 .. 4294967295 

Memory requirement: 4 bytes or 32 bits 

A [data field](<Data_field.md> "Data field") of data type **UInt32** can only take positive integer values. Assigning other values ​​leads to compiler error messages when the program is compiled and the compilation process is aborted. That is, the executable program is not created. 

Definition of a data field of type UInt32: 
    
    
      var 
        ui32 : uint32;
    

Examples of assigning valid values: 
    
    
      ui32 := 0;
      ui32 := 4294967295;
    

Examples of assigning invalid values: 
    
    
      ui32 := '0';
      ui32 := '4294967295';
    

The difference between the two examples is that the upper example is the assignment of literals of the type [Integer](<Integer.md> "Integer"), while the assignment of the lower example is literals of the type [String](<String.md> "String").

---

_Source: [https://wiki.freepascal.org/UInt32](https://web.archive.org/web/20240917024629/https://wiki.freepascal.org/UInt32)_
