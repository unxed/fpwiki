# extended class syntax

### Static fields, static properties, regular fields and regular properties
    
    
    type
      TSomeClass = class
      private
        type
          TSomeType = type integer;    // an internal type
        class var
          FSomeClassVar: TSomeType;    // class variable belongs to a class, not an instance
        var
          FSomeIntanceVar: TSomeType;  // instance variable belongs to instance. it is a usual field
        class procedure SetSomeClassVar(const AValue: TSomeType); static;
      public
        class property SomeProperty: TSomeType read FSomeClassVar write SetSomeClassVar; // class property - belongs to a class
        property SomeInstanceProp: TSomeType read FSomeIntanceVar;
      end;
    
    { TSomeClass }
    
    class procedure TSomeClass.SetSomeClassVar(const AValue: TSomeType);
    begin
       FSomeClassVar := AValue;
    end;
    
    var
      SomeClass: TSomeClass;
    begin
      SomeClass.SomeProperty := 1;
      WriteLn(SomeClass.SomeProperty);
    end.
    

### Class constants
    
    
    type
      TSomeClass = class
      strict private
        const
          PrivateConst = 1;
        type
          PrivateType = type Integer;
        var
          FPrivateField: PrivateType;
      public
        procedure DoSomething(Value: PrivateType = PrivateConst);
        function ReturnSomething: PrivateType;
        property SomeProp: PrivateType read FPrivateField write FPrivateField default PrivateConst;
      end;
    
      procedure TSomeClass.DoSomething(Value: PrivateType = PrivateConst);
      begin
      end;
    
      function TSomeClass.ReturnSomething: PrivateType;
      begin
        Result := PrivateConst;
      end;
    
    begin
    end.
    

### Nested classes
    
    
    type
      TSomeClass = class
      strict private
        type
          TPrivateNestedClass = class
          public
            procedure DoSomething;
          end;
      public
        type
          TNestedClass = class
          public
            procedure DoSomething;
          end;
        class procedure Test; virtual;
      end;
    
      TDescendant = class(TSomeClass)
      public
        class procedure Test; override;
      end;
    
    procedure TSomeClass.TPrivateNestedClass.DoSomething;
    begin
      WriteLn('TSomeClass.TPrivateNestedClass.DoSomething: ok');
    end;
    
    procedure TSomeClass.TNestedClass.DoSomething;
    begin
      WriteLn('TSomeClass.TNestedClass.DoSomething: ok');
    end;
    
    class procedure TSomeClass.Test;
    var
      P: TPrivateNestedClass;
      N: TNestedClass;
    begin
      P := TPrivateNestedClass.Create;
      P.DoSomething;
      P.Free;
      N := TNestedClass.Create;
      N.DoSomething;
      N.Free;
    end;
    
    class procedure TDescendant.Test;
    var
      N: TNestedClass;
    begin
      N := TNestedClass.Create;
      N.DoSomething;
      N.Free;
    end;
    
    begin
      TSomeClass.Test;
      TDescendant.Test;
    end.

---

_Source: [https://wiki.freepascal.org/class_extensions_examples](https://web.archive.org/web/20200926015008/https://wiki.freepascal.org/class_extensions_examples)_
