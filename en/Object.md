# Object

│ **[Deutsch (de)](</Object/de> "Object/de")** │  **English (en)** │  **[français (fr)](</Object/fr> "Object/fr")** │    
****Back to:[data types](<Data_type.md> "Data type") | [reserved words](<Reserved_words.md> "Reserved words"). 

The reserved word **object** is used to construct complex data types that contain both functions, procedures and data. Object allows the user to perform Object-Oriented Programming (OOP). It is similar to [class](<Class.md> "Class") in the types it can create, but by default objects are created on the [stack](</index.php?title=Stack&action=edit&redlink=1> "Stack \(page does not exist\)"), while class data is created on the [heap](</index.php?title=Heap&action=edit&redlink=1> "Heap \(page does not exist\)"). However, object created types can be created on the heap by using the [new](<New.md> "New") procedure. Object was introduced in [Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal"), while [class](<Class.md> "Class") was introduced in [Delphi](<Delphi.md> "Delphi"). Object is maintained for backward compatibility with Turbo Pascal and has largely been superseded by [class](<Class.md> "Class"). 

Example skeleton of the creation of the data type object: 
    
    
    type
      TTest = object
      private
        { private declarations }
      public
        { public declarations }
      end;
    

Example skeleton of the create of a packed version of the data type object: 
    
    
    type
      TTest = packed object
      private
        { private declarations }
      public
        { public declarations }
      end;
    

Example with constructor and destructor: 
    
    
    type
       TTest = object
       private 
         {private declarations}
         total_errors : Integer;
       public
         {public declarations}
         constructor Init;
         destructor Done;
         procedure IncrementErrors;
         function GetTotalErrors : Integer;
       end;
    
    procedure TTest.IncrementErrors;
    begin
      Inc(total_errors);
    end;
        
    function TTest.GetTotalErrors : Integer;
    begin
       GetTotalErrors := total_errors;
    end;
    
    constructor TTest.Init;
    begin
      total_errors := 0;
    end;
    
    destructor TTest.Done;
    begin
       WriteLn('Destructor not needed - nothing allocated on the heap');
    end;
    
    var
       error_counter: TTest;
    
    begin
       error_counter.Init; // unlike C++, constructors must be explicitly called
       error_counter.IncrementErrors;
       error_counter.IncrementErrors;
       WriteLn('current errors:', error_counter.GetTotalErrors);
       error_counter.Done
    end.
    

**Output:**  
current errors:2  
Destructor not needed - nothing allocated on the heap 

## Objects compared to other structured types

Feature  | [Record](<Record.md> "Record") | [Adv Record](<Record.md> "Record") | Object | [Class](<Class.md> "Class")  
---|---|---|---|---  
Encapsulation (combining data and methods + hiding visibility)  | No  | Yes  | Yes  | Yes   
[Inheritance](<Inherited.md> "Inherited") | No  | No  | Yes  | Yes   
Class constructor and destructor  | No  | Yes  | Yes  | Yes   
Polymorphism (virtual methods)  | No  | No  | Yes  | Yes   
Memory allocation  | Stack  | Stack  | Stack  | Heap (Only)   
  
    Setting fields to zero on allocation
| Managed Types only  | Managed Types only  | Managed Types only  | All fields   
  
    Default() function returns a constant with
| all fields zeros  | all fields zeros  | all fields zeros  | returns nil   
Operator overload (global)  | Yes  | Yes  | Yes  | Yes   
Operator overload (in type only)  | No  | Yes  | No  | No   
Type helpers  | No  | Yes  | No  | Yes   
Virtual constructors, class reference  | No  | No  | No  | Yes   
Variant part (case) as c/c++ union  | Yes  | Yes  | No  | No   
Bitpacked (really packing)  | Yes  | Yes  | No  | No   
  
Modified from <https://forum.lazarus.freepascal.org/index.php/topic,30686.30.html> (original author: ASerge). 

## See Also

  * [Programming Using Objects](<Programming_Using_Objects.md> "Programming Using Objects")



  
  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") `object` [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/Object](https://web.archive.org/web/20241225081349/https://wiki.freepascal.org/Object)_
