# simple type

A **simple type** is a single value which can be stored in an [Identifier](<Identifier.md> "Identifier"). Simple types are predefined by the compiler, but are not [reserved words](<Reserved_word.md> "Reserved word"). While it is not recommended, they can be redefined. 

Simple types predefined by the compiler are: 

name | bytes | comment   
---|---|---  
boolean | 1 |   
cardinal | 4 |   
char | 1 (unless you use {$mode DelphiUnicode} or {$ModeSwitch UnicodeStrings}, then it will be 2) | most likely 1   
int8 | 1 | by definition   
int16 | 2 | by definition   
int32 | 4 | by definition   
int64 | 8 | by definition   
integer | either 2 or 4, depends on compilation mode |   
shortint | 1 | <= integer   
smallint | 2 | <= integer   
longint | 4 | >= integer   
byte | 1 | by definition   
word | 2 | by definition   
dword | 4 | by definition   
qword | 8 | by definition   
pointer | depends on target cpu bitness: on 32-bit it is 4, on 64-bit it is 8 |   
single | 4 | single precision float   
double | 8 (if the system supports this type, otherwise it's an alias for single) | double precision float   
extended | 10 (if the system supports this type, otherwise it's an alias for double) | extended precision float   
[real](</index.php?title=real&action=edit&redlink=1> "real \(page does not exist\)") | 4 or 8 (platform dependant) |   
currency | 8 |   
  
  


navigation bar: data types  simple data types |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/simple_type](https://web.archive.org/web/20241201000000/https://wiki.freepascal.org/simple_type)_
