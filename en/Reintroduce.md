# Reintroduce

│ **English (en)** │

The [modifier](<modifier.md> "modifier") `reintroduce` belongs to [object-oriented programming](<object-oriented_programming.md> "object-oriented programming"). The modifier `reintroduce` allows a method of the parent [class](<Class.md> "Class") to be concealed by a new [method](<Method.md> "Method") with the same name. That is, a new method exists in the class derived from the parent class and in all other classes derived from it. The method in the parent class is preserved and can still be used by it. 

The method of the parent class does not exist anymore in the new class, it has been replaced by the new method with the same name. The method continues to exist in its original form in the parent class and can be used through the parent class. This is as opposed to [modifier](<modifier.md> "modifier") override which only works for virtual methods. The [modifier](<modifier.md> "modifier") reintroduce merely suppresses a warning that a similar method already exists and that the programmer is aware of that. 

Example: 
    
    
    interface
    
    type
      TParentClass = class
        procedure SetTest(intNum: Integer); // Some method
      end;
    
      TDerivedClass = class(TParentClass)
        procedure SetTest(strName: String); reintroduce; // This replaces the method of the parent class in the derived class. And supresses warnings that a method with an identical signature already exists.
      end;
    
    implementation
    
    procedure TDerivedClass.SetTest(strName: String);
    begin
      inherited SetTest(1); // Call method with same name from parent class, if needed
    end;

---

_Source: [https://wiki.freepascal.org/Reintroduce](https://web.archive.org/web/20240701000000/https://wiki.freepascal.org/Reintroduce)_
