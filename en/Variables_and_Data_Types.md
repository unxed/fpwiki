# Basic Pascal Tutorial/Chapter 1/Variables and Data Types

│ **English (en)** │  **[русский (ru)](<../ru/Variables_and_Data_Types.md>)** │

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_1/Constants.md> "Basic Pascal Tutorial/Chapter 1/Constants") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_1/Assignment_and_Operations.md> "Basic Pascal Tutorial/Chapter 1/Assignment and Operations")  
---|---|---  
  
1D - Variables and Data Types (author: Tao Yue, state: _changed_) 

Variables are similar to constants, but their values can be changed as the program runs. Variables must first be declared in Pascal before they can be used: 
    
    
    var
      IdentifierList1 : DataType1;
      IdentifierList2 : DataType2;
      IdentifierList3 : DataType3;
      ...
    

`IdentifierList` is a series of identifiers, separated by commas (`,`). All identifiers in the list are declared as being of the same data type. 

The basic [data field](<Data_field.md> "Data field") [data types](<Data_type.md> "Data type") in Pascal include: 

  * [Integer](<Integer.md> "Integer")
  * [Word](<Word.md> "Word")
  * [LongInt](<Longint.md> "Longint")
  * [Real](<Real.md> "Real")
  * [Char](<Char.md> "Char")
  * [Boolean](<Boolean.md> "Boolean")



Standard Pascal does not make provision for the string data type, but most modern compilers do. Experienced Pascal programmers also use pointers for dynamic memory allocation, objects for object-oriented programming, and many others, but this gets you started. 

More information on Pascal data types: 

  * The [Integer](<Integer.md> "Integer") data type can contain whole numbers. the size of an integer depends on the compiler and the processor. On PCs before the 80386, "integer" meant 16-bit whole numbers in the range from `-32768` to `32767`. This is the signed range that can be stored in a 16-bit word, and is a legacy of the era when 16-bit CPUs were common. For backward compatibility purposes, a 32-bit signed integer is a longint and can hold a much greater range of values, `2147483647` to `-2147483648`.
  * The [Word](<Word.md> "Word") data type is a 16-bit unsigned integer, which has a range of `0` to `65535`.
  * The [Real](<Real.md> "Real") data type has a range from `3.4x10-38` to `3.4x1038`, in addition to the same range on the negative side. Real values are stored inside the computer similarly to scientific notation, with a mantissa and exponent, with some complications. In Pascal, you can express real values in your code in either fixed-point notation or in scientific notation, with the character `E` separating the mantissa from the exponent. Thus, `452.13` is the same as `4.5213e2`
  * The [Char](<Char.md> "Char") data type holds characters. Be sure to enclose them in single quotes, like so: `'a' 'B' '+'` Standard Pascal uses 8-bit characters, not 16-bits, so Unicode, which is used to represent all the world's language sets in one UNIfied CODE system, is not supported.
  * The [WideChar](<WideChar.md> "WideChar") is a two-byte character (an element of a DBCS: Double Byte Character Set) and can hold a Unicode character. Note: some Unicode characters require two WideChars. See _UTF-16_.
  * Free Pascal supports the Delphi implementation of the [PChar](<PChar.md> "PChar") data type. PChar is defined as a pointer to a Char type, but allows additional operations. The PChar type can be understood best as the Pascal equivalent of a C-style null-terminated string, i.e. a variable of type PChar is a pointer that points to an array of type Char, which is ended by a null-character (#0). Free Pascal supports initializing of PChar typed constants, or a direct assignment. For example, the following pieces of code are equivalent:


    
    
    program one;  
    var P : PChar;  
    begin  
      P := 'This is a null-terminated string.';  
      WriteLn (P);  
    end.
    
    
    
    program two;  
    const P : PChar = 'This is a null-terminated string.';  
    begin  
      WriteLn (P);  
    end.
    

  * Free Pascal supports the [String](<String.md> "String") type as it is defined in Turbo Pascal: a sequence of characters with an optional size specification. It also supports [AnsiStrings](<Ansistring.md> "Ansistring") (with unlimited length) as in Delphi. And can be declared as:


    
    
    variable_name : string;                    // if no length is given, it defaults to 255
    variable_name : string[length];            // where:  1 < length <= 255
    

  * The predefined type [ShortString](<Shortstring.md> "Shortstring") is defined as a string of size 255.
  * [AnsiStrings](<Ansistring.md> "Ansistring") are strings that have no length limit. They are reference counted and are guaranteed to be null terminated. Internally, an ansistring is treated as a pointer: the actual content of the string is stored on the heap, as much memory as needed to store the string content is allocated.
  * [WideStrings](<Widestrings.md> "Widestrings") (used to represent unicode character strings) are implemented in much the same way as ansistrings: reference counted, null-terminated arrays, only they are implemented as arrays of `WideChars` instead of regular `Chars`.
  * The [Boolean](<Boolean.md> "Boolean") data type can have only two values: **TRUE** and **FALSE**



An example of declaring several variables is: 
    
    
    var
      Age, Year, Grade : integer;
      Circumference : real;
      LetterGrade : char;
      DidYouFail : boolean;
    

**From the FPC manual**

**integer types** Type | Range | Bytes   
---|---|---  
Byte | 0 .. 255 | 1   
Shortint | -128 .. 127 | 1   
Smallint | -32768 .. 32767 | 2   
Word | 0 .. 65535 | 2   
Integer | smallint or longint | 2 or 4   
Cardinal | longword | 4   
Longint | -2147483648 .. 2147483647 | 4   
Longword | 0..4294967295 | 4   
Int64 | -9223372036854775808 .. 9223372036854775807 | 8   
QWord | 0 .. 18446744073709551615 | 8   
  
Free Pascal does automatic type conversion in expressions where different kinds of integer types are used. 

**real types** Type | Range | Significant digits | Bytes   
---|---|---|---  
Real | platform dependent | ??? | 4 or 8   
Single | 1.5E-45 .. 3.4E38 | 7-8 | 4   
Double | 5.0E-324 .. 1.7E308 | 15-16 | 8   
Extended* | 1.9E-4932 .. 1.1E4932 | 19-20 | 10   
Comp | -2E64+1 .. 2E63-1 | 19-20 | 8   
Currency | -922337203685477.5808 | 922337203685477.5807 | 8   
  
  * Note that for Windows 64 bits and non-Intel targets Extended is an alias for Double.



  


**boolean types** Type | Bytes | Ord(True)   
---|---|---  
Boolean | 1 | 1   
ByteBool | 1 | Any nonzero value   
WordBool | 2 | Any nonzero value   
LongBool | 4 | Any nonzero value   
[ ◄ ](<Basic_Pascal_Tutorial/Chapter_1/Constants.md> "Basic Pascal Tutorial/Chapter 1/Constants") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_1/Assignment_and_Operations.md> "Basic Pascal Tutorial/Chapter 1/Assignment and Operations")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Variables_and_Data_Types](https://web.archive.org/web/20240507002835/https://wiki.freepascal.org/Variables_and_Data_Types)_
