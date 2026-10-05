# FreeAndNil

**FreeAndNil** is a procedure defined in the [SysUtils unit](</index.php?title=SysUtils_unit&action=edit&redlink=1> "SysUtils unit \(page does not exist\)") of the Free Pascal [Runtime Library](<RTL.md> "RTL"). It calls an object's destructor (via TObject.Free) and also sets the object variable (which is a reference to heap memory created by a constructor call) to nil. If you just call an object's destructor, [Assigned](<Assigned.md> "Assigned") will still return [True](<True.md> "True") for the object variable. But after calling FreeAndNil, [Assigned](<Assigned.md> "Assigned") will return [False](<False.md> "False") for the object variable. While the procedure has an _untyped parameter_ , it is only for use with object references - that is variables that are instances of a [Class](<Class.md> "Class"). 
    
    
    procedure FreeAndNil( var obj );
    

Example: 
    
    
    {$mode ObjFPC}
    uses SysUtils;
    
    type
       SomeClass = class(TObject)
          destructor Destroy; override;
       end;
    
    destructor SomeClass.Destroy;
    begin
       WriteLn('SomeClass Destructor called');
       inherited;
    end;
    
    var
       myClass : SomeClass;
       myClass2: SomeClass;
    begin
       myClass  := SomeClass.Create;
       WriteLn('myClass is assigned? ', Assigned(myClass));
       myClass2 := SomeClass.Create;
       WriteLn('myClass2 is assigned? ', Assigned(myClass2));
       myClass.Destroy;
       WriteLn('myClass is assigned? ', Assigned(myClass));
       FreeAndNil(myClass2);
       WriteLn('myClass2 is assigned? ', Assigned(myClass2));
       // assigning Nil after destructor is called is the same as
       // FreeAndNil
       myClass := Nil;
       WriteLn('myClass is assigned? ', Assigned(myClass));
    end.
    

**Output:**
    
    
     myClass is assigned? TRUE  
     myClass2 is assigned? TRUE
     SomeClass Destructor called
     myClass is assigned? TRUE
     SomeClass Destructor called
     myClass2 is assigned? FALSE
     myClass is assigned? FALSE

---

_Source: [https://wiki.freepascal.org/FreeAndNil](https://web.archive.org/web/20250324074638/https://wiki.freepascal.org/FreeAndNil)_
