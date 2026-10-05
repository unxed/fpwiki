# Dispose

│ **English (en)** │

Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

The reserved word **dispose** belongs to object-oriented programming. 

The reserved word **dispose** releases the memory by reducing the reference counter to the memory object by 1. If the reference counter of an object is 0, the memory object is released. 

Example: 
    
    
      var
       intI: ^ Integer;
    
     begin
       ...
       if assigned (intI) then ... // Checks whether an address has been assigned to intI
       ...
       new (intI);  // Assigns a valid address to intI
       ...
       dispose (intI);  // Releases the address
       ...

---

_Source: [https://wiki.freepascal.org/Dispose](https://web.archive.org/web/20250321113728/https://wiki.freepascal.org/Dispose)_
