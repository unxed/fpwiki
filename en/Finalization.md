# Finalization

│ **[Deutsch (de)](</Finalization/de> "Finalization/de")** │  **English (en)** │  **[suomi (fi)](</Finalization/fi> "Finalization/fi")** │  **[русский (ru)](<../ru/Finalization.md> "Finalization/ru")** │    
****

` Finalization` is a [reserved word](<Reserved_words.md> "Reserved words") within [Object Pascal](<Object_Pascal.md> "Object Pascal"). It starts the optional finalization part of a [unit](<Unit.md> "Unit"). 

## Structure of a unit
    
    
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
    

## See also

  * Finalization
  * [Implementation](<Implementation.md> "Implementation")
  * [Initialization](<Initialization.md> "Initialization")
  * [Interface](<Interface.md> "Interface")
  * [Uses](<Uses.md> "Uses")

---

_Source: [https://wiki.freepascal.org/Finalization](https://web.archive.org/web/20250516132657/https://wiki.freepascal.org/Finalization)_
