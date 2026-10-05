# LongBool

│ **English (en)** │

  
Back to [data types](<Data_type.md> "Data type"). 

  
Range of values: True .. False 

Memory requirement: 4 bytes or 32 bits 

A data field of the **LongBool** data type can only contain truth values. 

Assigning other values ​​leads to compiler error messages when the program is compiled and the compilation process is aborted. This means that the executable program is not created. 

The **LongBool** data type is used like the Boolean data type and behaves like the [Boolean](<Boolean.md> "Boolean") data type. 

Declaration of a data field of type LongBool: 
    
    
     var 
       l : LongBool;
    

Examples of assigning valid values: 
    
    
      l := true;
      l := false;
      l := 10 <> 20 ;  // The result of the comparison is true
    

Examples of assigning invalid values: 
    
    
      l := 'True';
      l := 'False';
      l := '10 <> 20';
      l := 24;
    

The difference between the two examples is that the upper example is the assignment of literals of the type [Truth value](</index.php?title=Truth_value&action=edit&redlink=1> "Truth value \(page does not exist\)"), while the assignment of the lower example is literals of the type String and the type [ShortInt](<Shortint.md> "Shortint").

---

_Source: [https://wiki.freepascal.org/Longbool](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Longbool)_
