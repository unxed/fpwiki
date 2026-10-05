# Set

│ **[Deutsch (de)](</Set/de> "Set/de")** │  **English (en)** │  **[suomi (fi)](</Set/fi> "Set/fi")** │  **[français (fr)](</Set/fr> "Set/fr")** │  **[русский (ru)](<../ru/Set.md> "Set/ru")** │    
****

  
Back to [data types](<Data_type.md> "Data type"). 

Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  


## Introduction

A Set encodes many values from an enumeration into an Ordinal type. 

For example let's consider this enumeration: 
    
    
      TSpeed = (spVerySlow,spSlow,spAverage,spFast,spVeryFast);
    

And this set: 
    
    
      TPossibleSpeeds = set of TSpeed
    

[Constant](<Constant.md> "Constant") instances of TPossibleSpeeds can be defined using [brackets](<square_brackets.md> "square brackets") to hold set elements: 
    
    
      const
        RatherSlow = [spVerySlow,spSlow];
        RatherFast = [spFast,spVeryFast];
    

RatherSlow and RatherFast are some Set of TSpeed. 

## Manipulating sets

You can assign a set's contents directly: 
    
    
      var
        SomeSpeeds: TPossibleSpeeds = [spAverage];
      begin
        SomeSpeeds := [spSlow,spVerySlow];
      end;
    

Two [functions](<Function.md> "Function") defined in the [RTL](<RTL.md> "RTL") [System unit](<System_unit.md> "System unit") are used to manipulate set elements individually: [Include](</index.php?title=Include&action=edit&redlink=1> "Include \(page does not exist\)")(ASet,AValue) and [Exclude](</index.php?title=Exclude&action=edit&redlink=1> "Exclude \(page does not exist\)")(ASet,AValue). 
    
    
      var
        SomeSpeeds: TPossibleSpeeds;
      begin
        SomeSpeeds := [];
        Include(SomeSpeeds,spVerySlow);
        Include(SomeSpeeds,spVeryFast);
      end;
    

Sets cannot be directly manipulated if they are published. You usually have to make a local copy, change the local copy and then to call the setter. 
    
    
      procedure TSomething.DoSomething(Sender: TFarObject);
      var
        LocalCopy: TPossibleSpeeds;
      begin
        LocalCopy := Sender.PossibleSpeeds; // getter to local
        Include(LocalCopy,spVerySlow);
        Sender.PossibleSpeeds := LocalCopy; // local to setter.
      end;
    

The [reserved word](<Reserved_word.md> "Reserved word") [`in`](<In.md> "In") is also used to test if a value is in a set. It's usually used in this fashion: 
    
    
      var
        CanBeSlow: Boolean;
      const
        SomeSpeeds = [Low(TSpeed)..High(TSpeed)];
      begin
        CanBeSlow := (spVerySlow in SomeSpeeds) or (spSlow in SomeSpeeds);
      end;
    

Empty square brackets represent an empty set. You can test if a set currently contains no values at all by comparing against an empty set: 
    
    
      var
        SomeSpeeds: TPossibleSpeeds;
      begin
        if SomeSpeeds = [] then SomeSpeeds := [spAverage];
      end;
    

## Bitmasks

Sets can be used to create bitmasks as shown in the example. 
    
    
    (*
      The set [FLAG_A, FLAG_C] will be stored like this:
      
      TFlags :   0000'0101
                       │││
      FLAG_C ──────────┘││
      FLAG_B ───────────┘│
      FLAG_A ────────────┘
    *)
    
    type
      TFlag = (FLAG_A, FLAG_B, FLAG_C);
      TFlags = set of TFlag;
    
    var
      Flags: TFlags;
    
    [..]
      Flags:= [FLAG_A, FLAG_C];
      if FLAG_A in Flags then ..  // check FLAG_A is set in flags variable
    

  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") `set` [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/set](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/set)_
