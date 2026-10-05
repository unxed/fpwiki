# Cardinal

│ [**Deutsch (de)**](</Cardinal/de> "Cardinal/de") │  **English (en)** │  [**français (fr)**](</Cardinal/fr> "Cardinal/fr") │    


Cardinal is an integer type defined as an alias for DWord under a 32-bit platform. Like the DWord (double word) type it's 32 bits and interpreted as an unsigned integer. Its minimal value is 0x0000000 and its maximal value 0xFFFFFFFF (4,294,967,295). 

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

However, because of 32 and 64 bit little endian systems, using the Cardinal type is not recommended anymore for memory/pointer operations/arithmetic. It's recommended to use **NativeInt** or **NativeUInt** types instead. These types will match the width of the CPU registers which can be used to encode a memory address and so will always be the right size. For example under a 64b-bit OS, a NativeUInt will be like a UInt64 or a QuadWord and under a 32-bit OS, a NativeUInt will be like a DWord or a Cardinal. 

Data Types   
---  
Simple Data Types  |  [Boolean](<Boolean.md> "Boolean") | [Byte](<Byte.md> "Byte") | **Cardinal** | [Char](<Char.md> "Char") | [Extended](<Extended.md> "Extended") | [Int64](<Int64.md> "Int64") | [Integer](<Integer.md> "Integer") | [Longint](<Longint.md> "Longint") | [Pointer](<Pointer.md> "Pointer") | [Real](<Real.md> "Real") | [Shortint](<Shortint.md> "Shortint") | [Smallint](<Smallint.md> "Smallint") | [Word](<Word.md> "Word")  
Complex Data Types  |  [Array](<Array.md> "Array") | [Class](<Class.md> "Class") | [Record](<Record.md> "Record") | [Set](<Set.md> "Set") | [String](<String.md> "String")

---

_Source: [https://wiki.freepascal.org/cardinal](https://web.archive.org/web/20160903002727/https://wiki.freepascal.org/cardinal)_
