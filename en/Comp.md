# Comp

│ **English (en)** │

Value range: -2E64 + 1 .. 2E63-1. 

Accuracy: 19 positions. 

Memory occupancy: 8 bytes or 64 bits. 

Property: A data field of data type Comp accepts floating point values ​​or integers, signed or unsigned. Any other value leads to a compilation error, so the program will not be created. 

Important: The Comp type is only supported by 80x86 processors (this type is therefore not portable). 

Declaration of a data field of type Comp: 
    
    
      Var 
        c : Comp;
    

Example of correct assignments: 
    
    
        c := -123.45678;
        c := 0;
        c := 123.45678;
    

Example of incorrect assignments: 
    
    
        c := '-123.45678';
        c := '0';
        c := '123.45678';
    

These last examples try to affect non-integer literal values ​​(string) which require an explicit conversion.

---

_Source: [https://wiki.freepascal.org/Comp](https://web.archive.org/web/20250120201317/https://wiki.freepascal.org/Comp)_
