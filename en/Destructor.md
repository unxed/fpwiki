# Destructor

│ **[Deutsch (de)](</Destructor/de> "Destructor/de")** │  **English (en)** │  **[suomi (fi)](</Destructor/fi> "Destructor/fi")** │    
****

The [reserved word](<Reserved_word.md> "Reserved word") `destructor` belongs to [object-oriented programming](<object-oriented_programming.md> "object-oriented programming"). The destroyer is used to release resources like memory. The destroyer must always be made so that when the memory is released, the memory used by the entire [class](<Class.md> "Class") (object) is released and therefore no memory leak occurs. 

Release of the object takes place by calling a class `free` [method](<Method.md> "Method"). Calling `free` causes a `Destroy` invitation. It also checks that the [`self`](<Self.md> "Self") [variable](<Variable.md> "Variable") is not [`nil`](<Nil.md> "Nil"). 

  
for example: 
    
    
    program Project1;
    {$ mode objfpc} {$ H +}
    
    type
    
       // class definition
       {TClass}
    
       TClass = Class
         constructor Create;
         destructor Destroy; override; // allows the use of a parent class destroyer
       end;
    
    // class builder
    constructor TClass.Create;
    begin
       Writeln ('Build object');
    end;
    
    // class eraser
    destructor TClass.Destroy;
    begin
       Writeln ('Demolished Object');
       inherited; // Also called parent class destroyer
    end;
    
    var
    
    // Defines the class variable
       myclass: TClass;
    
    begin
       myclass: = TClass.Create; // Initialize the object by calling the class builder
       Writeln ('Something Code ...');
       myclass.Free; // Free invites your own class Destroy discharger
       Writeln ('Press <Enter>');
       readln;
    end.

---

_Source: [https://wiki.freepascal.org/Destructor](https://web.archive.org/web/20250421224129/https://wiki.freepascal.org/Destructor)_
