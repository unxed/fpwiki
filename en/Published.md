# Published

│ **[Deutsch (de)](</Published/de> "Published/de")** │  **English (en)** │    
****

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The **published** modifier: 

  * is part of object-oriented programming;
  * designates an area within a class that can be accessed from anywhere;
  * allows this section to get streaming properties just like when using the local directive **{M+}**.



Example: 
    
    
     Type
       TEltern class = class                                // The parent class is derived from the base class
       published
         constructor Create(intWidth, intHeight : Integer); // The constructor is public and has streaming properties
         function surface double;                           // The function is public and has streaming properties
       end;

---

_Source: [https://wiki.freepascal.org/Published](https://web.archive.org/web/20250219111752/https://wiki.freepascal.org/Published)_
