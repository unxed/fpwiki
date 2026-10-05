# Inherited

│ **English (en)** │

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
In an overridden virtual [method](<Method.md> "Method"), it is often necessary to call the parent [`class`](<Class.md> "Class")’ implementation of the virtual method. This can be done with the `inherited` [reserved word](<Reserved_word.md> "Reserved word"). Likewise, the `inherited` [keyword](<Keyword.md> "Keyword") can be used to call any method of the parent `class`. 

This case is the simplest: 
    
    
    Type  
      TMyClass = Class(TComponent)  
        Constructor Create(AOwner : TComponent); override;  
      end; 
    
    Constructor TMyClass.Create(AOwner : TComponent);  
    begin  
      Inherited;  
      // Do more things  
    end;
    

## Constructors and destructors case

[`Constructor`](<Constructor.md> "Constructor"), example 1 : 
    
    
      ...
      TTest.Create;
      begin
        Inherited; // Always at the beginning of the constructors and start the constructor (code only) of the parent class
        ...
      end;
    

`Constructor`, example 2 : 
    
    
      ...
      TTest.Create(...);
      begin
        Inherited Create(...); // Always at the beginning of the constructors and start the constructor (code only) of the parent class
        ...
      end;
      ...
    

[`Destructor`](<Destructor.md> "Destructor"), example 3 : 
    
    
      TTest.Destroy;
      begin
        ...
        Inherited;  // Always at the end of the destructors and start the destructor (code only) of the parent class
      end;
      ...
    

## Virtual methods override
    
    
    type  
      TMyClass = class(TStrings)  
        function GetObject(Index: Integer): TObject; override;  
      end; 
    
    function TMyClass.GetObject(Index: Integer): TObject;
    begin
      // Get result from parent class method 
      Result := inherited GetObject(Index);  
      // Do something  
    end;

---

_Source: [https://wiki.freepascal.org/inherited](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/inherited)_
