# Char

│ **English (en)** │  **[русский (ru)](<../ru/Char.md>)** │

A **char** stores a single character and is currently one byte, and [AnsiChar](<AnsiChar.md> "AnsiChar") is an alias for it. However, in the future, **char** may become the same as a [WideChar](<WideChar.md> "WideChar"). For now, [byte](<Byte.md> "Byte") and char are almost identical - one byte (8-bits) in size. However, a char can only be used as a character, or as part of a [string](<String.md> "String") type, and cannot be used in an arithmetic expression, while a byte can only be referred to as a numeric type. 

For example: 
    
    
     var ch: char;
          c: byte; 
    
     begin
       ch := 'A';  c := 65;  { are the same action, and are legal }
       ch := 65;   c := 'A'; { while they are internally the same values, 
                               direct assignment between Chars and Bytes is illegal }
     end.
    

The use of char or byte as a data type provides better documentation as to the purpose of the use of the particular variable. The char type can be [coerced](</index.php?title=coersion&action=edit&redlink=1> "coersion \(page does not exist\)") to byte by using the [ord](<Ord.md> "Ord") function. Byte type values can be coerced to char by using the [chr](<Chr.md> "Chr") function. 

type chars functions follows the [ASCII](<ASCII.md> "ASCII"). 

The above program corrected to legal use: 
    
    
     var ch: char;
          c: byte; 
    
     begin
        ch := 'A';      c := 65;        { are the same action, and are legal }
        ch := chr(65);  c := ord('A');  { now legal }
        ch := Char(65); c := Byte('A'); { also legal and guaranteed to happen at compile time }
     end.
    

A [FOR](<For.md> "For") loop control variable can be any enumerable (capable of being put into one-to-one correspondence with the positive integers) which allows a CHAR variable to be used in that regard: 
    
    
    var
       Loop: Char;
    
    begin
      for Loop := 'a' to 'c' do WriteLn(Loop);
    end.
    

## See also

  * [Character and string types](<Character_and_string_types.md> "Character and string types"), a detailed reference covering internal memory layout and access options.



  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") `char` [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/Char](https://web.archive.org/web/20250121224140/https://wiki.freepascal.org/Char)_
