# Byte

│ **[Deutsch (de)](</Byte/de> "Byte/de")** │  **English (en)** │  **[español (es)](</Byte/es> "Byte/es")** │  **[suomi (fi)](</Byte/fi> "Byte/fi")** │  **[français (fr)](</Byte/fr> "Byte/fr")** │  **[italiano (it)](</Byte/it> "Byte/it")** │  **[русский (ru)](<../ru/Byte.md> "Byte/ru")** │  **[中文（中国大陆）‎ (zh_CN)](</Byte/zh_CN> "Byte/zh CN")** │    
****

A `byte` is an unsigned [`integer`](<Integer.md> "Integer") in the range of `0..255`. A `byte` is 8 bits long. A `byte` and a [`char`](<Char.md> "Char") are virtually the same thing as of version 3 of [FPC](<FPC.md> "FPC"). 

## Contents

  * 1 Valid values
  * 2 Standard functions
    * 2.1 Conversion to and from character
    * 2.2 String representation



## Valid values

The key difference is, a `byte` can only be referred to as a numeric [`type`](<Type.md> "Type"), while a `char` can be used as a character, or as part of a string type, and cannot be used in an arithmetic expression. A `byte` will always be the same size as an [`ansiChar`](<AnsiChar.md> "AnsiChar"), but in the future `char` may be considered a synonym for [`wideChar`](<WideChar.md> "WideChar"), not `ansiChar`. 

For example: 
    
    
    program byteDemo(input, output, stderr);
    
    var 
    	foo: byte;
    	bar: char;
    
    begin
    	// those two assignments are physically the same
    	foo := 65;
    	bar := 'A';
    	
    	// although they are the same action,
    	// the following would be illegal
    	//foo := 'A';
    	//bar := 65;
    end.
    

The use of `byte` or `byte` as a data type provides better documentation as to the purpose of the use of the particular variable. 

## Standard functions

### Conversion to and from character

The `byte` type can be [coerced](</index.php?title=coersion&action=edit&redlink=1> "coersion \(page does not exist\)") to `char` by using the [`chr` function](<Chr.md> "Chr"). Char type values can be coerced to byte by using the [`ord` function](<Ord.md> "Ord"). 

The above program corrected to legal use: 
    
    
    program ordChrDemo(input, output, stderr);
    
    var
    	foo: byte;
    	bar: char;
    
    begin
    	foo := 65;
    	bar := 'A';
    	
    	foo := ord('A');
    	// chr(65) is equivalent to #65
    	bar := chr(65);
    	bar := #65;
    	
    	// alternatively: typecasts
    	// typecasts of constant expressions
    	// are guaranteed to happen at compile-time
    	foo := byte('A');
    	bar := char(65);
    end.
    

### String representation

The [`binStr` function](<https://www.freepascal.org/docs-html/rtl/system/binstr.html>) from the [`system` unit](<System_unit.md> "System unit") can be used to get a [`string`](<String.md> "String") showing the [binary representation](<Binary_numeral_system.md> "Binary numeral system") of a `byte`: 
    
    
    program binStrDemo(input, output, stderr);
    
    var
    	foo: byte;
    
    begin
    	foo := 10;
    	writeLn(binStr(foo, 8));
    end.
    

The output is: 
    
    
    00001010
    

A more versatile function is [`intToBin` provided by the `strUtils` unit](<https://www.freepascal.org/docs-html/rtl/strutils/inttobin.html>). 

  
  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") `byte` [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/Byte](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/Byte)_
