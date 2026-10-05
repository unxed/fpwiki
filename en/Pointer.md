# Pointer

│ **English (en)** │  **[русский (ru)](<../ru/Pointer.md>)** │

**Pointer** has two meanings in [Free Pascal](<FPC.md> "FPC"): 

  * term or concept for a class of types and [variables](<Variable.md> "Variable") of those types
  * a simple [data type](<Data_type.md> "Data type") that can hold the address of any data type



  


## Contents

  * 1 Concept
  * 2 Data Type
  * 3 Predeclared Pointer Types
  * 4 How Pointer variables are initialized
  * 5 See also



## Concept

A **pointer** is a variable that contains the memory address of a local or global variable, or the memory address of a value or complex object that has been created and stored in the heap part of a program's memory layout. It can be said to be a [reference](</index.php?title=reference&action=edit&redlink=1> "reference \(page does not exist\)") for the value/variable, or that it "points to" the value/variable. For any data type, a pointer type for that data can be declared using operator **[^](<^.md> "^")** in front of the data type. With the {$TYPEDADDRESS ON} [compiler directive](<Compiler_directive.md> "Compiler directive") set (advisable), pointers can only hold the addresses of variables or values of the same type that they have been declared to point to. 

Pointer variables are assigned the address of a variable with the **[@](<@.md> "@")** operator and they are assigned the address of a value/object on the heap by using the output of the New procedure. To access, for reading or writing, the data "pointed to" by a pointer variable, you _de-reference_ it with the **[^](<^.md> "^")** operator. 
    
    
    program test_pointer_wiki2;
    {$TYPEDADDRESS ON}
    var 
      a         : Integer = 100;
      b         : Real = 10.5;
      ptrToInt  : PInteger;  // PInteger is a pointer type declared in the RTL
                             // which can only point to Integer value/variab
      ptrToInt2 : ^Integer;  // the general format for declaring a pointer type
    begin
      //ptrToInt := @b;      // compile error with {$TYPEDADDRESS ON} 
      ptrToInt := @a;        
      //Writeln(ptrToInt);   // compiler error: Error: Can't read or write variables of this type
      Writeln(ptrToInt^);    // prints 100
      New(ptrToInt2);        // allocate an un-initialized Integer on the heap
      Writeln(ptrToInt2^);   // writes random integer value
      ptrToInt2^ := 222;     // assign a value to the heap allocated integer
      Writeln(ptrToInt2^);   // prints 222 
    end.
    

## Data Type

Back to [data types](<Data_type.md> "Data type"). 

  
The **Pointer** data type is a simple type which can contain the address of a variable or value of **any** data type. This is in contrast to typed pointer variables (e.g. PInteger) that can only point to a variables or values of a specific type with compiler directive {$TYPEDADDRESS ON} . For example: 
    
    
    {$TYPEDADRESS ON} 
    Var 
      a : Integer = 20;
      b : Real = 20.5;
      c : Boolean = true;
      ptr : Pointer;
    begin
      ptr := @a; // valid to hold Integer address 
      ptr := @b; // valid to hold Real address
      ptr := @c; // valid to hold Boolean address
    end.
    

But, before a Pointer variable with a value can be used, it must be 

  1. cast to the type of the value or variable that it points to
  2. de-referenced with the **^** operator



For example: 
    
    
    Var 
      a      : Integer = 20;
      ptr    : Pointer;
    begin
      ptr := @a;          // valid to hold Integer address 
      //Writeln(ptr);     // Invalid - will not compile
                          // Error: Can't read or write variables of this type
      //Writeln( ptr^);   // Error: Can't read or write variables of this type
                          // Just de-referencing a Pointer variable is not sufficient
      Writeln( PInteger(ptr)^); // After cast, then de-reference: prints 20
    end.
    

## Predeclared Pointer Types

The [System unit](<System_unit.md> "System unit") of the standard [Runtime Library](<RTL.md> "RTL") declares a number of pointer types for the built-in variables including: 
    
    
      PSmallInt           = ^Smallint;
      PShortInt           = ^Shortint;
      PInteger            = ^Integer;
      PByte               = ^Byte;
      PWord               = ^word;
      PDWord              = ^DWord;
      PLongWord           = ^LongWord;
      PLongint            = ^Longint;
      PCardinal           = ^Cardinal;
      PQWord              = ^QWord;
      PBoolean            = ^Boolean;
    

## How Pointer variables are initialized

Q: I would like to understand the default initialization behavior more - what is the value Pointer variable is being initialized to? 

A (by FPC developer **PascalDragon**): 

Pointer variables/fields by themselves are not initialized at all. There value will depend upon where they are used: 

  * if it is a global variable or part of a record/object that's declared as a global variable it will be Nil unless an explicit value is assigned to it, because that's what global variables without explicit value initializers are initialized to
  * if it is a field of a class then it will be Nil, because class instances are zeroed before the constructor is run
  * if it is a local variable or part of a record/object that's declared as a local variable and there is no value initializer then the value will be undefined and can be anything
  * if it's a variable that is allocated using GetMem or New or is part of a record/object that is allocated that way then the value will be undefined and can be anything



In the later two cases you need to explicitly set the value yourself. 

A2 (by forum member **Warfley**): 

It's not just for pointer variables the case pointers are basic unmanaged variables, so they behave like other basic unmanaged variables (like float, integer or boolean), if they are not members of a block that explicitly guarantees initialization (like global variables or class members which are guaranteed to be 0), the value is undefined and must be initialized by you. 

## See also

  * Usage of [Pointers](<Pointers.md> "Pointers")
  * <http://www.freepascal.org/docs-html/ref/refse15.html>



  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") `pointer` [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/Pointer](https://web.archive.org/web/20250529211913/https://wiki.freepascal.org/Pointer)_
