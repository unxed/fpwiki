# Class

│ **[Deutsch (de)](</Class/de> "Class/de")** │  **English (en)** │  **[français (fr)](</Class/fr> "Class/fr")** │  **[русский (ru)](<../ru/Class.md> "Class/ru")** │    
****

A **class** is a highly structured data [type](<Type.md> "Type") in Object [Pascal](<Pascal.md> "Pascal") dialects such as [Delphi](<Mode_Delphi.md> "Mode Delphi") or the [ObjFPC](<Mode_ObjFPC.md> "Mode ObjFPC") dialect. Classes are able to contain [variables](<Variable.md> "Variable"), [constructors](<Constructor.md> "Constructor"), [destructors](<Destructor.md> "Destructor"), [functions](<Function.md> "Function"), [procedures](<Procedure.md> "Procedure"), and [properties](</Property> "Property") using access scopes. 

Classes are able to inherit and to be inherited by other classes. For [run-time](<runtime.md> "runtime") purposes, any class not specifying a parent class automatically inherits from [TObject](<TObject.md> "TObject"), as it has required components for all classes. Because of TObject's dependency, any [subclass](</index.php?title=subclass&action=edit&redlink=1> "subclass \(page does not exist\)")'s destructor must have the [override](</index.php?title=override&action=edit&redlink=1> "override \(page does not exist\)") directive. Additionally, any of your class's constructors must specify [inherited](<Inherited.md> "Inherited") in their body. A class can have several constructors, but only one destructor. 

Object Pascal does not support multiple inheritance: apart from the implicit inheritance by TObject, classes can have only one ancestor class. Polymorphism is implemented with method directives. Here is a simple class declaration; let's explain it. 
    
    
    type
      TMyClass = class
      private
        FSomeVar: integer;
      public
        constructor Create; overload;
        constructor Create(Args: array of integer); overload;
        destructor Destroy; override;
        function GetSomeVar: integer;
        procedure SetSomeVar(newvalue: integer);
      published
        property SomeVar: integer read GetSomeVar write SetSomeVar default 0;
      end;
    

Between the keywords 'class' and 'end' we find member declarations, both variables and methods. Some methods (functions and/or procedures) are preceded by scope modifiers (**private** , **public** , **published**) and followed by directives (**overload** , **override**) and a strange **property** thing. Let's explain them all. 

## Contents

  * 1 Notes on inheritance
  * 2 Scope modifiers
  * 3 Properties
  * 4 self
  * 5 Method directives
    * 5.1 virtual, dynamic, override
    * 5.2 static
    * 5.3 abstract
    * 5.4 overload
  * 6 Classes compared to other structured types
    * 6.1 See also



## Notes on inheritance

In Object Pascal, derived classes inherit all members of the base class, even those not overloaded with the same name. Example: 
    
    
    type
      //base class
      MyClass = class
        procedure Proc1;   
      end;
    
      //derived class
      YourClass = class(MyClass)  
        procedure Proc1; //same name of MyClass procedure
      end;
    
    var
      a: MyClass;
      b: YourClass;
    begin
      a := MyClass.Create;
      b := YourClass.Create;
      a.Proc1;          // uses procedure in MyClass 
      b.Proc1;          // uses procedure in YourClass
      MyClass(b).Proc1; // uses procedure in MyClass
    

## Scope modifiers

Scope modifiers tell the compiler who can call a method: 

  * private: the member can be called/accessed only by other methods in the same class;
  * public: the member can be called/accessed by any other part of the program;
  * protected: the member can be called/accessed from other classes in the same unit and from derived classes, but not from further derivated classes.
  * published: the variable is public, and it will appear in the IDE's Object Inspector.



Scope modifiers cannot be changed in derived classes: members will maintain their visibility (or lack thereof) forever, everywhere. 

## Properties

A property is a variable that is accessed through methods. The variable SomeVar in the example above will be read and written in the code like a simple variable, but under the hood the [compiler](<Compiler.md> "Compiler") will call the methods specified in the property declaration. This allows to calculate it on the fly, or run boundary checks, input validation, formatting or just a placeholder for future extensions. Some rules apply: 

  * Either read or write methods, at least one of them, must be present;
  * The read method (if present) cannot have parameters;
  * The write method (if present) must have exactly one parameter;
  * Both read and write methods cannot be **dynamic** : if they are **virtual** they cannot be **overload**.



**default** sets a starting value for the property: it's optional and can be omitted. Another option is **stored false** : 
    
    
    property SomeVar: Integer read GetSomeVar write SetSomeVar stored false;
    

This way the write method will be called, but the value will NOT be saved in the variable. 

## self

The **[self](<Self.md> "Self")** argument is passed by default to every (non static) method: it's an alias to the specific class instance which the method belongs to. This way every class can identify itself and have access to its members without ambiguity. 

## Method directives

Class methods can be changed, overloaded, made static and more. A method can have more than one directive. Directives rule the whole polymorphism system in Object Pascal; they can change the call model, too. 

### virtual, dynamic, override

these three directives are mutually exclusive. **virtual** means that the method can be overwritten by the derived class. **dynamic** means the same thing, but the implementation differs: virtual members addresses are stored in a table, while dynamic members do not use tables and do not occupy RAM, but their resolution mechanism is slower. 

A derived class can implement its own version of a virtual method, but the base method is still available; if the new method is marked **override** it hides the base virtual method, which cannot be called anymore. This holds true in case of dynamic methods, too. However you cannot override a method if it's not virtual or dynamic. 

### static

A **static** method or variable is common to every instance of the class; one class instance can write a static variable member and every other instance will retain (and read, if accessed) the new value. For this reason, the compiler does NOT pass the 'self' parameter to static methods: it would be nonsense. Moreover, static methods cannot be virtual (but see class methods below). 

### abstract

A method declared **abstract** is declared, but not implemented in the base class. Derived classes will be forced to provide their own implementation. 

### overload
    
    
    function MultiSum(a, b: integer): integer; overload;
    function MultiSum(a, b: AnsiString): AnsiString; overload;
    

With the **overload** directive you can declare many methods - functions or procedures - with the same name but different type and parameters. 

The class needs the constructor to be implemented in the implementation section of the unit: 
    
    
    implementation
    
    constructor TMyClass.Create;
    begin
      inherited;
      SomeVar := 6;
    end;
    
    end.
    

The destructor and all procedures and functions of the class must also be implemented in the implementation section. 

Instances of the class can then be declared as variables: 
    
    
    var
      classInstance: TMyClass;
    

Before using an instance, it must be created with: 
    
    
    begin
      classInstance := TMyClass.Create;
      ...
    end;
    

## Classes compared to other structured types

Feature  | [Record](<Record.md> "Record") | [Adv Record](<Record.md> "Record") | [Object](<Object.md> "Object") | Class  
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

### See also

[extended class syntax](<extended_class_syntax.md> "extended class syntax") Examples 

  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") `class` [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/Class](https://web.archive.org/web/20250424035321/https://wiki.freepascal.org/Class)_
