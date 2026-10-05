# Cardinal

│ **[Deutsch (de)](</Cardinal/de> "Cardinal/de")** │  **English (en)** │  **[français (fr)](</Cardinal/fr> "Cardinal/fr")** │  **[русский (ru)](<../ru/Cardinal.md> "Cardinal/ru")** │    
****

**Cardinal** is an integer type defined as an alias for [DWord](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") under a 32-bit platform. Like the DWord (double word) type it's 32 bits and interpreted as an unsigned integer. Its minimal value is 0x0000000 and its maximal value 0xFFFFFFFF (4,294,967,295). 

On x86 systems Cardinal type is often used to hold a memory address, like a pointer: 
    
    
      var
        anAddress: Cardinal;
        anObject: TObject;
      begin
        anAddress := Cardinal(Self);
        with TObject(anAddress) do
        begin
          // anAddress is casted as a TObject.
        end;
      end;
    

However, because of 32 and 64 bit little endian systems, using the Cardinal type is _**not recommended**_ anymore for memory/pointer operations/arithmetic. It's recommended to use **NativeInt** or **NativeUInt** types instead. These types will match the width of the CPU registers which can be used to encode a memory address and so will always be the right size. For example under a 64b-bit OS, a NativeUInt will be like a UInt64 or a QuadWord and under a 32-bit OS, a NativeUInt will be like a DWord or a Cardinal. 

  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") `cardinal` [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/Cardinal](https://web.archive.org/web/20240920204107/https://wiki.freepascal.org/Cardinal)_
