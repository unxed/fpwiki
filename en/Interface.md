# Interface

│ **English (en)** │

Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

The reserved word **interface** is used to structure (subdivide) a [unit](<Unit.md> "Unit"). This is distinct from the [Interfaces](<Interfaces.md> "Interfaces") facility which relates to class management. 

Everything that has been declared in the interface section can be used by this and other units. It is the _public part_ in other languages. 

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

_Source: [https://wiki.freepascal.org/Interface](https://web.archive.org/web/20241225081349/https://wiki.freepascal.org/Interface)_
