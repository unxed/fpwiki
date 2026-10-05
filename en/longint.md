# Longint

│ **[Deutsch (de)](</Longint/de> "Longint/de")** │  **English (en)** │  **[suomi (fi)](</Longint/fi> "Longint/fi")** │  **[français (fr)](</Longint/fr> "Longint/fr")** │  **[русский (ru)](<../ru/Longint.md> "Longint/ru")** │    
****

  
Back to [data types](<Data_type.md> "Data type"). 

  
Range of values: -2,147,483,648 .. 2,147,483,647 (-231 .. 231-1) 

Memory requirement: 4 bytes or 32 bits (The [Cardinal](<Cardinal.md> "Cardinal") type is also 32 bits long, but is an unsigned type.) 

A data field of the **LongInt** data type can only take integer values ​​with and without sign. Assigning other values ​​leads to compiler error messages when the program is compiled and the compilation process is aborted. That is, the executable program is not created. 

Definition of a Longint data field: 
    
    
      var 
        left : Longint;
    

Examples of assigning valid values: 
    
    
      left := -2147483648;
      left := 0;
      left := 2147483647;
    

Examples of assigning invalid values: 
    
    
      left := '-2147483648';
      left := '0';
      left := '2147483647';
    

The difference between the two examples is that the upper example is the assignment of literals of the type integer, while the assignment of the lower example is literals of the type String. 

  
  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") `longint` [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/longint](https://web.archive.org/web/20220819013509/https://wiki.freepascal.org/longint)_
