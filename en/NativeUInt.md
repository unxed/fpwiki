# NativeUInt

│ **English (en)** │

  
Back to the [data types](<Data_type.md> "Data type"). 

  
Range of values: processor-dependent. Examples of Intel and Intel compatible processors: 

32 bit processors: 0 .. 4294967295 64 bit processors: 0 .. 18446744073709551615 

Memory requirements: Examples for Intel and Intel compatible processors: 

32 bit processors: 4 bytes or 32 bits 64 bit processors: 8 bytes or 64 bits 

A data field of the data type **NativeUInt** can only accept positive [integer values](<Integer.md> "Integer"). Assigning other values ​​leads to compiler error messages when the program is compiled and the compilation process is aborted. That is, the executable program is not created. 

Definition of a data field of the type **NativeUInt** : 
    
    
      var 
        nuint : NativeUInt;
    

Examples of assigning valid values: 
    
    
      nuint := 0;
      nuint := 4294967295;
    

Examples of assigning invalid values: 
    
    
      nuint := '0';
      nuint := '4294967295';
    

The difference between the two examples is that the upper example is the assignment of literals of the type [integer](<Integer.md> "Integer"), while the assignment of the lower example is literals of the type [String](<String.md> "String").

---

_Source: [https://wiki.freepascal.org/NativeUInt](https://web.archive.org/web/20250517160629/https://wiki.freepascal.org/NativeUInt)_
