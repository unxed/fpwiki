# Virtual

│ **[Deutsch (de)](</Virtual/de> "Virtual/de")** │  **English (en)** │    
****

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The modifier **virtual** : 

  * belongs to object-oriented programming;
  * describes a virtual method.



A virtual method must also be implemented (programmed) in the class in which it was declared. 

A virtual method can be overwritten in a derived class with the reserved word override. 

If a virtual method is to be hidden, the class must be covered with the reserved word [Reintroduce](<Reintroduce.md> "Reintroduce"). 

Example: 
    
    
     Type
       TBody = class
       private
         dblHeight : double;
       public
         function volume : double; virtual;     // This method can be in one of this class
                                                // derived class can be hidden or overwritten
         function surfaceArea double; virtual;  // This method must be in one of this class
                                                // derived class can be hidden or overwritten
     end;

---

_Source: [https://wiki.freepascal.org/Virtual](https://web.archive.org/web/20250323141450/https://wiki.freepascal.org/Virtual)_
