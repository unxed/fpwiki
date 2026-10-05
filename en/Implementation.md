# Implementation

│ **English (en)** │

**implementation** is a [reserved word](<Reserved_words.md> "Reserved words") that is used to structure (subdivide) a [unit](<Unit.md> "Unit"). 

In the implementation section everything that has been declared there can only be used in this unit. It is the _private part_ in other languages. 

  
Example of a unit structure: 
    
    
     unit ...;      // Name of the unit
    
     interface      // Everything declared here may be used by this and other units (public)
    
     uses ...;
    
       ...
    
     implementation // The implementation of the requirements for this unit only (private)
    
     uses ...;
    
       ...
    
     initialization // Optional section: variables, data etc initialised here
    
       ...
    
     finalization   // Optional section: code executed when the program ends
    
       ...
     end.

---

_Source: [https://wiki.freepascal.org/Implementation](https://web.archive.org/web/20240920204117/https://wiki.freepascal.org/Implementation)_
