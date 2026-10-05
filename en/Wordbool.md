# WordBool

│ **[Deutsch (de)](</WordBool/de> "WordBool/de")** │  **English (en)** │    
****

  
Back to [data types](<Data_type.md> "Data type"). 

  
Range of values: True .. False 

Memory requirement: 2 bytes or 16 bits 

A [data field](<Data_field.md> "Data field") of the **WordBool** [data type](<Data_type.md> "Data type") can only contain truth values. 

Assigning other values ​​leads to compiler error messages when the program is compiled and the compilation process is aborted. This means that the executable program is not created. 

The WordBool data type is used like the Boolean data type and the WordBool data type behaves like the Boolean data type. 

Definition of a data field of the type WordBool: 
    
    
      var 
        w : WordBool;
    

Examples of assigning valid values: 
    
    
      w := True;
      w := false;
      w := 10 <> 20;  // The result of the comparison is true
    

Examples of assigning invalid values: 
    
    
      w := 'True';
      w := 'False';
      w := '10 <> 20';
      w := 24;
    

The difference between the two examples is that the upper example is the assignment of literals of the type [truth value](</index.php?title=Truth_value&action=edit&redlink=1> "Truth value \(page does not exist\)"), while the assignment of the lower example is literals of the type [String](<String.md> "String") and the type [ShortInt](<Shortint.md> "Shortint").

---

_Source: [https://wiki.freepascal.org/Wordbool](https://web.archive.org/web/20250517164019/https://wiki.freepascal.org/Wordbool)_
