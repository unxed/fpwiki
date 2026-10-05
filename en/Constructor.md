# Constructor

│ **[Deutsch (de)](</Constructor/de> "Constructor/de")** │  **English (en)** │  **[español (es)](</Constructor/es> "Constructor/es")** │  **[suomi (fi)](</Constructor/fi> "Constructor/fi")** │  **[русский (ru)](<../ru/Constructor.md> "Constructor/ru")** │    
****

The [reserved word](<Reserved_word.md> "Reserved word") `constructor` belongs to [object-oriented programming](<object-oriented_programming.md> "object-oriented programming"). `Constructor` is a [class](<Class.md> "Class") builder [method](<Method.md> "Method") that creates the object of that class. 

Example: 
    
    
    // class definition
    type
      TKlasse = class
      end;
    
    var
      // declare variable of type of class
      clsKlasse: TKlasse;
    
    begin
      ...
      // create class
      clsKlasse := TKlasse.Create; 
      ...
    end;
    

## See also

  * [Destructor](<Destructor.md> "Destructor")

---

_Source: [https://wiki.freepascal.org/Constructor](https://web.archive.org/web/20250421222737/https://wiki.freepascal.org/Constructor)_
