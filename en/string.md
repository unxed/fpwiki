# String

│ **English (en)** │


**String** is a [type](<Type.md> "Type") which may contain [characters](<Character_and_string_types.md> "Character and string types"). 

## Contents

  * 1 Usage
  * 2 Alias
  * 3 String types
  * 4 See also



## Usage
    
    
    var
      s, str1, str2, str3, str4: string;
      c: char;
      n: integer;
     
    str1 := 'abc';     // assignment
    str2 := '123';     // string containing chars 1, 2 and 3
    str3 := #10#13;    // cr lf
    str4 := 'this is a ''quoted'' string';  // use of quotes within a string
    s := str1 + str2;  // concatenation
    c := s[1];         // use as index in array
    n := length( s );  // length of string s

## Alias

String is an alias for [ShortString](<Character_and_string_types.md> "Character and string types"), [AnsiString](<Character_and_string_types.md> "Character and string types") or [Unicodestring (UTF16)](<Character_and_string_types.md> "Character and string types") depending on a compiler setting. 

If [compiler](<Compiler.md> "Compiler") directive {$H} or [compiler directive](<http://freepascal.org/docs-html/current/prog/progch1.html#x5-40001>) {$LongStrings} has been used with an "on" parameter ( {$H+} or {$LongStrings ON} ), then a String type is the same as an AnsiString type, if not ( {$H-} or {$LongStrings OFF} ), it is a ShortString type. What String is an alias for can also be set by the -Sh [command line option](<http://www.freepascal.org/docs-html/user/userap1.html>). FPC also supports {$mode delphiunicode} for Delphi compatible UTF16 support. 

  
NOTE: The {$mode} compiler directive will also set the String alias. After the compiler mode is set to FPC (the default), ObjFPC, MacPAS or TP, String will be an alias for ShortString. After the compiler mode is set to Delphi, String will be an alias for AnsiString. So the String alias setting should be made following the compiler mode setting to prevent it from being overridden: 
    
    
    {$H+}            // String is an alias for AnsiString
    {$mode ObjFPC}   // also affects String alias - String is now an alias for ShortString
    {$H+}            // String is now an alias for AnsiString

A String variable declared with a length specifier will always be a ShortString regardless of the compiler setting for String alias. 
    
    
    {$H+}            // String is an alias for AnsiString
    var
       name : String[25]; // name is a ShortString variable since a length specification overrides the alias setting

Note that all types of longstring are managed types, whereas ShortStrings are not managed types: they have no reference count. 

## String types

The different string types - ShortString, AnsiString, WideString and UnicodeString - differ with respect to _length_ and _content_ : 

  * [ShortString](<Character_and_string_types.md> "Character and string types") has a _fixed maximum length_ that is decided by the programmer (e.g. _name : String[25];_) but is limited to 255 characters. If a ShortString length is not explicitly given, then the length is implicitly set to 255. It is not reference counted. 
  * [AnsiString](<Character_and_string_types.md> "Character and string types") has a _variable length_ that is limited only by the value of High(SizeInt) (which is platfom dependant) and available memory. It is a reference counted type. 
  * [WideString](<Character_and_string_types.md> "Character and string types") has a variable length like AnsiString but contains [WideChar](<WideChar.md> "WideChar") instead of [Char](<Char.md> "Char"). It is a BWSTR compatible string type and has no reference count. 
  * [UnicodeString](<Character_and_string_types.md> "Character and string types") is similar to [WideString](<Character_and_string_types.md> "Character and string types") but UnicodeString is a managed type and has a reference count whereas widestring is a BWSTR compatible stringtype that is COM compatible and is not reference counted.  
  




Note that BWSTR types rely on COM marshaling or - when used alone - copy semantics instead of reference counting. In a COM context they are governed by the COM marshaling subsystem if available. (i.e. Windows) 

## See also

  * [Character and string types](<Character_and_string_types.md> "Character and string types"), a detailed reference covering internal memory layout and access options. 

Data Types   
---  
Simple Data Types  |  [Boolean](<Boolean.md> "Boolean") | [Byte](<Byte.md> "Byte") | [Cardinal](<Cardinal.md> "Cardinal") | [Char](<Char.md> "Char") | [Currency](<Currency.md> "Currency") | [Extended](<Extended.md> "Extended") | [Int64](<Int64.md> "Int64") | [Integer](<Integer.md> "Integer") | [Longint](<Longint.md> "Longint") | [Pointer](<Pointer.md> "Pointer") | [Real](<Real.md> "Real") | [Shortint](<Shortint.md> "Shortint") | [Smallint](<Smallint.md> "Smallint") | [Word](<Word.md> "Word")  
Complex Data Types  |  [Array](<Array.md> "Array") | [Class](<Class.md> "Class") | [Record](<Record.md> "Record") | [Set](<Set.md> "Set") | **String** | [ShortString](</index.php?title=Shortstring&action=edit&redlink=1> "Shortstring \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/string](https://web.archive.org/web/20171016044350/https://wiki.freepascal.org/string)_
