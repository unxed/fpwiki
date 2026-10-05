# Override

│ **English (en)** │

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The **override** modifier: 

  * is part of object-oriented programming;
  * allows a virtual and abstract method from a parent class to be overridden (replaced).



Example: 
    
    
     Type
       TParentClass = class                           // The parent class is derived from the base class
       public
         function volume : double; virtual; abstract; // This method must be one from this class
                                                      // - derived class can be hidden or overridden
         function surface double; virtual; abstract;  // This method must be one from this class
                                                      // - derived class can be hidden or overridden
       end;
    
     Type
       TChildClass = class(ParentClass)               // The child class is derived from the parent class
       public
         function volume : double; override;          // the virtual method is overridden (replaced)
         function surface double; override;           // the virtual method is overridden (replaced)
       end;

---

_Source: [https://wiki.freepascal.org/Override](https://web.archive.org/web/20250422181318/https://wiki.freepascal.org/Override)_
