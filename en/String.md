# String

│ **English (en)** │  **[русский (ru)](<../ru/String.md>)** │

**String** is a [type](<Type.md> "Type") which may contain [characters](<Character_and_string_types.md> "Character and string types"). 

## Contents

  * 1 Usage
  * 2 Alias
  * 3 String types
  * 4 String type in Lazarus
  * 5 See also



## Usage
    
    
    var
      s, str1, str2, str3, str4: string;
      c: char;
      n: integer;
    
    str1 := 'abc';     // assignment
    str2 := '123';     // string containing chars 1, 2 and 3
    str3 := #13#10;    // cr lf
    str4 := 'this is a ''quoted'' string';  // use of quotes within a string
    s := str1 + str2;  // concatenation
    c := s[1];         // use as index in array
    n := Length( s );  // length of string s
    

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
  * RawByteString is an alias for AnsiString.
  * [WideString](<Character_and_string_types.md> "Character and string types") has a variable length like AnsiString but contains [WideChar](<WideChar.md> "WideChar") instead of [Char](<Char.md> "Char"). It is a BWSTR compatible string type and has no reference count.
  * [UnicodeString](<Character_and_string_types.md> "Character and string types") is similar to [WideString](<Character_and_string_types.md> "Character and string types") but UnicodeString is a managed type and has a reference count whereas widestring is a BWSTR compatible stringtype that is COM compatible and is not reference counted.



Note that BWSTR types rely on COM marshaling or - when used alone - copy semantics instead of reference counting. In a COM context they are governed by the COM marshaling subsystem if available. (i.e. Windows) 

[Note from compiler developer **PascalDragon**](<https://forum.lazarus.freepascal.org/index.php/topic,61081.msg458774.html#msg458774>): The WideString type on Windows is considered as managed. Also the WideString type on non-Windows is internally a UnicodeString. You can see that by looking at the used RTL helper function: e.g. `u := 'Foobar'` will use `fpc_unicodestr_assign` on non-Windows instead of `fpc_widestr_assign`. 

## String type in Lazarus

The Lazarus IDE stores everything in UTF-8 encoding. The type **String** in Lazarus is by default also UTF-8. 

So, the string contains more bytes than "characters", since the "lowercase i with accent" is made up of 2 bytes. 

In UTF-8 all plain ASCII (so up to #127) are stored as a single byte. All other "characters" are stored as 2, 3 or 4 byte sequences. This makes iteration through a UTF-8 encoded string more complex than old style single byte encoding (ALIAS codepages). 

The LazUTF8 unit from Lazarus has various functions to handle UTF8 encoded strings. E.g. UTF8Length(): it returns the length in "UTF-8 characters", instead of the length in bytes (as Length() does): Utf8Length('Ä') is 1, whilst Length('Ä') is 2. 

Displaying UTF-8 encoded strings will be displayed as expected in any visual component of Lazarus. 

When you write to the console, you have to understand that the console has a different codepage altogether. It can only display 255 different characters, and it treats strings as being single byte encoded. So "i with accent", which consist of 2 bytes is treated as 2 separate chars, and how they look on the console is dependant on your codepage. It will look different e.g. in Dutch locale, than on e.g. a Windows with Russian locale settings. 

You can fight the Lazarus system and declare your stings as being of a certain codepage and use **RawByteString** to prevent the compiler form doing unwanted codepage conversion. But in the long run, you better go with the flow. 

Note: I write "character" where I mean the the visual glyph on the screen we normally interpret as being a character (and this probably only holds for Western language, not for e.g. Farsi). The term character is a bit fuzzy when it comes to Unicode. 

## See also

  * [Character and string types](<Character_and_string_types.md> "Character and string types"), a detailed reference covering internal memory layout and access options.



  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") `string` [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/String](https://web.archive.org/web/20250318111613/https://wiki.freepascal.org/String)_
