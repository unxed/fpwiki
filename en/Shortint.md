# Shortint

│ **English (en)** │  **[русский (ru)](<../ru/Shortint.md>)** │


A shortint is a signed integer in the range of -128 to 127. The shortint is 8 bits long. 

The [Byte](<Byte.md> "Byte") type is 8 bits long, too. But the byte datatype is an unsigned type, meaning that it encodes numbers from 0 to 255. 

  

    
    
    var
      a_shortint: shortint;
      a_byte : byte;
      s1, s2 : string;
    begin
      a_shortint := %11110001;   // binary number
      a_byte     := %11110001;
      s1 := IntToStr(a_shortint); // s1 = '-15'
      s2 := IntToStr(a_byte);     // s2 = '241'

## See also

  * [function OnesComplement2](<Not.md> "Not")
  * [Smallint](<Smallint.md> "Smallint") is integer type supporting values from -32768 to 32767 
  * [ binary numbers](<Binary_numeral_system.md> "Binary numeral system")
  * [IntToStr](</index.php?title=IntToStr&action=edit&redlink=1> "IntToStr \(page does not exist\)") convert an integer into a string 

Data Types   
---  
Simple Data Types  |  [Boolean](<Boolean.md> "Boolean") | [Byte](<Byte.md> "Byte") | [Cardinal](<Cardinal.md> "Cardinal") | [Char](<Char.md> "Char") | [Currency](<Currency.md> "Currency") | [Extended](<Extended.md> "Extended") | [Int64](<Int64.md> "Int64") | [Integer](<Integer.md> "Integer") | [Longint](<Longint.md> "Longint") | [Pointer](<Pointer.md> "Pointer") | [Real](<Real.md> "Real") | **Shortint** | [Smallint](<Smallint.md> "Smallint") | [Word](<Word.md> "Word")  
Complex Data Types  |  [Array](<Array.md> "Array") | [Class](<Class.md> "Class") | [Record](<Record.md> "Record") | [Set](<Set.md> "Set") | [String](<String.md> "String")

---

_Source: [https://wiki.freepascal.org/Shortint](https://web.archive.org/web/20170512012059/https://wiki.freepascal.org/Shortint)_
