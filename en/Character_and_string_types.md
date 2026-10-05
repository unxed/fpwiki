# Character and string types

│ **English (en)** │  **[русский (ru)](<../ru/Character_and_string_types.md>)** │

Free Pascal supports several **[character](<Char.md> "Char") and [string](<String.md> "String") types**. They range from single ANSI characters to unicode strings and also include pointer types. Differences also apply to encodings and reference counting. 

## Contents

  * 1 Character types
    * 1.1 AnsiChar
      * 1.1.1 Reference
    * 1.2 WideChar
      * 1.2.1 References
  * 2 Character-derived types
    * 2.1 Array of Char
      * 2.1.1 Static Array of Char
      * 2.1.2 Dynamic Array of Char
    * 2.2 PChar
      * 2.2.1 Reference
    * 2.3 PWideChar
      * 2.3.1 Reference
  * 3 String types
    * 3.1 String
      * 3.1.1 Reference
    * 3.2 ShortString
      * 3.2.1 Reference
    * 3.3 AnsiString
      * 3.3.1 Reference
    * 3.4 UnicodeString
      * 3.4.1 Reference
    * 3.5 UTF8String
      * 3.5.1 Reference
    * 3.6 UTF16String
      * 3.6.1 Reference
    * 3.7 WideString
      * 3.7.1 Reference
  * 4 String-derived types
    * 4.1 PShortString
      * 4.1.1 Reference
    * 4.2 PAnsiString
      * 4.2.1 Reference
    * 4.3 PUnicodeString
      * 4.3.1 Reference
    * 4.4 PWideString
      * 4.4.1 Reference
  * 5 String constants
  * 6 See also



## Character types

### AnsiChar

A variable of type **AnsiChar** , also referred to as **[char](<Char.md> "Char")** , is exactly 1 byte in size, and contains one "ANSI" (local code page) character. 

a   
---  
  
#### Reference

  * [FPC AnsiChar documentation](<http://www.freepascal.org/docs-html/ref/refsu6.html>)
  * [Usage Char](<Char.md> "Char")
  * [Wikipedia:Windows code page#ANSI code page](<http://www.wikipedia.org/wiki/Windows_code_page#ANSI_code_page> "wikipedia:Windows code page")



### WideChar

A variable of type **WideChar** , also referred to as **UnicodeChar** , is exactly 2 bytes in size, and usually contains one [Unicode](<LCL_Unicode_Support.md> "LCL Unicode Support") code point (normally a character) in UTF-16 encoding. Note: it is impossible to encode all Unicode code points in 2 bytes. Therefore, 2 WideChars may be needed to encode a single code point. 

| a   
---|---  
  
#### References

  * [FPC WideChar documentation](<http://www.freepascal.org/docs-html/ref/refsu7.html>)
  * [UTF-16 information on Wikipedia](<https://en.wikipedia.org/wiki/UTF-16>)
  * [RTL UnicodeChar documentation](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/unicodechar.html> "doc:rtl/system/unicodechar.html")



## Character-derived types

### Array of Char

Early Pascal implementations that were in use before 1978 did not support a string type (with the exception of string constants). The only possibility to store strings in variables was the use of arrays of char. This approach has many disadvantages and is no longer recommended. It is, however, still supported to ensure backward-compatibility with ancient code. 

#### Static Array of Char
    
    
    type
      TOldString4 = array[0..3] of char;
    var
      aOldString4: TOldString4; 
    begin
      aOldString4[0] := 'a';
      aOldString4[1] := 'b';
      aOldString4[2] := 'c';
      aOldString4[3] := 'd';
    end;
    

The static array of char has now the content: 

a | b | c | d   
---|---|---|---  
  
![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Unassigned chars can have any content, depending on what was just in memory when the memory for the array was made available.

#### Dynamic Array of Char
    
    
    var
      aOldString: Array of Char; 
    begin
      SetLength(aOldString, 5);
      aOldString[0] := 'a';
      aOldString[1] := 'b';
      aOldString[2] := 'c';
      aOldString[3] := 'd';
    end;
    

The dynamic array of char has now the content: 

a | b | c | d | #0   
---|---|---|---|---  
  
![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Unassigned chars in dynamic arrays have a content #0, cause empty positions of all dynamic arrays are initially initialised with 0 (or #0, or nil, or ...)

### PChar

A variable of type **[PChar](<PChar.md> "PChar")** is basically a pointer to a **Char** type, but allows additional operations. PChars can be used to access C-style [null-terminated strings](<http://en.wikipedia.org/wiki/Null-terminated_string>), e.g. in interaction with certain OS libraries or third-party software. 

a | b | c | #0   
---|---|---|---  
^   
  
#### Reference

  * [FPC PChar documentation](<http://www.freepascal.org/docs-html/ref/refsu12.html>)
  * [PChar related functions](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/pcharfunctions.html> "doc:rtl/sysutils/pcharfunctions.html")



### PWideChar

A variable of type **PWideChar** is a pointer to a WideChar variable. 

| a |  | b |  | c | #0 | #0   
---|---|---|---|---|---|---|---  
^   
  
#### Reference

  * [RTL PWideChar documentation](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/pwidechar.html> "doc:rtl/system/pwidechar.html")



## String types

### String

The type **String** may refer to **ShortString** or **AnsiString** , depending from the [{$H} switch](<http://www.freepascal.org/docs-html/prog/progsu25.html#x32-310001.2.25>). If the switch is off ({$H-}) then any string declaration will define a **ShortString**. It size will be 255 chars, if not otherwise specified. If it is on ({$H+}) **string** without length specifier will define an **AnsiString** , otherwise a **ShortString** with specified length. In **mode delphiunicode'__**_String_ is **UnicodeString**. 

#### Reference

  * [Usage String](<String.md> "String").
  * [String functions](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/stringfunctions.html> "doc:rtl/sysutils/stringfunctions.html")
  * [Reference for unit 'strutils': Procedures and functions](<http://lazarus-ccr.sourceforge.net/docs/rtl/strutils/index-5.html> "doc:rtl/strutils/index-5.html")



### ShortString

Short strings have a maximum length of 255 characters with the implicit [codepage](<FPC_Unicode_support.md> "FPC Unicode support") CP_ACP. The length is stored in the character at index 0. A short string of 255 characters uses 256 bytes of memory (one byte for the length specification and 255 bytes for characters). 

#3 | a | b | c   
---|---|---|---  
  
#### Reference

  * [FPC Single-byte String types documentation](<https://www.freepascal.org/docs-html/ref/refsu9.html>)



### AnsiString

Ansistrings are strings that have no length limit. They are [reference counted](<http://en.wikipedia.org/wiki/Reference_counting>) and are guaranteed to be [null terminated](<http://en.wikipedia.org/wiki/Null-terminated_string>). Internally, a variable of type **AnsiString** is treated as a pointer: the actual content of the string is stored on the heap, as much memory as needed to store the string content is allocated. 

|  |  |  |  |  |  |  | a | b | c | #0   
---|---|---|---|---|---|---|---|---|---|---|---  
RefCount | Length   
  
On 64-bit targets, fields RefCount/Length consume 8 bytes each, not 4. 

An AnsiString type may also have a compile-time code page since FPC 2.7.1; a missing value defaults to `DefaultSystemCodePage`. A value of `CP_NONE` results in `**RawBytestring**` and a value of `CP_UTF8` results in `**UTF8String**`. 

#### Reference

  * [FPC Single-byte String types documentation](<https://www.freepascal.org/docs-html/ref/refsu9.html>)



### UnicodeString

Like **AnsiStrings** , **UnicodeStrings** are reference counted, null-terminated arrays, but they are implemented as arrays of **WideChars** instead of regular **Chars**. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The UnicodeString naming is a bit ambiguous but probably due to its use in Delphi on Windows, where the OS uses UTF16 encoding; it's not the only string type that can hold Unicode string data (see also UTF8String)...

|  |  |  |  |  |  |  |  | a |  | b |  | c | #0 | #0   
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---  
RefCount | Length   
  
On 64-bit targets, fields RefCount/Length consume 8 bytes each, not 4. 

#### Reference

  * [FPC Multi-byte String types documentation](<http://www.freepascal.org/docs-html/ref/refsu10.html>)



### UTF8String

In FPC 2.6.5 and below the type **UTF8String** was an alias to the type **AnsiString**. In FPC 2.7.1 and above it is defined as `UTF8String = type AnsiString(CP_UTF8);`

It is meant to contain UTF-8 encoded strings (i.e. unicode data) ranging from 1..4 bytes per character. Note that **String** can also contain UTF-8 encoded characters. 

#### Reference

  * [FPC UTF8String documentation](<https://www.freepascal.org/docs-html/ref/refsu9.html#x32-400003.2.4>)



### UTF16String

The type **UTF16String** is an alias to the type **WideString**. In the LCL unit _lclproc_ it is an alias to **UnicodeString**. 

#### Reference

  * [LCL UTF16String documentation](<http://lazarus-ccr.sourceforge.net/docs/lcl/lclproc/utf16string.html> "doc:lcl/lclproc/utf16string.html")



### WideString

Variables of type **[WideString](<Widestrings.md> "Widestrings")** (used to represent unicode character strings in COM applications) resemble those of type **UnicodeString** , but unlike them they are not reference-counted. On Windows they are allocated with a special windows function which allows them to be used for OLE automation. 

WideStrings consist of COM compatible UTF16 encoded bytes on Windows machines (UCS2 on Windows 2000), and they are encoded as plain UTF16 on Linux, Mac OS X and iOS. 

|  |  |  |  | a |  | b |  | c | #0 | #0   
---|---|---|---|---|---|---|---|---|---|---|---  
Length   
  
#### Reference

  * [FPC Multi-byte String types documentation](<http://www.freepascal.org/docs-html/ref/refsu10.html>)



## String-derived types

### PShortString

A variable of type **PShortString** is a pointer that points to the first byte of a **ShortString** -type variable (which defines the length of the ShortString). 

#3 | a | b | c   
---|---|---|---  
^   
  
#### Reference

  * [RTL PShortString documentation](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/pshortstring.html> "doc:rtl/system/pshortstring.html")



### PAnsiString

Variables of type **PAnsiString** are pointers to **AnsiString** -type variables. However, unlike **PShortString** -type variables they don't point to the first byte of the header, but to the first **char** of the **AnsiString**. 

|  |  |  |  |  |  |  | a | b | c | #0   
---|---|---|---|---|---|---|---|---|---|---|---  
RefCount | Length | ^   
  
#### Reference

  * [RTL PAnsiString documentation](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/pansistring.html> "doc:rtl/system/pansistring.html")



### PUnicodeString

Variables of type **PUnicodeString** are pointers to variables of type **UnicodeString**. 

|  |  |  |  |  |  |  |  | a |  | b |  | c | #0 | #0   
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---  
RefCount | Length | ^   
  
#### Reference

  * [RTL PUnicodeString documentation](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/punicodestring.html> "doc:rtl/system/punicodestring.html")



### PWideString

Variables of type **PWideString** are pointers. They point to the first char of a **WideString** -typed variable. 

|  |  |  |  | a |  | b |  | c | #0 | #0   
---|---|---|---|---|---|---|---|---|---|---|---  
Length | ^   
  
#### Reference

  * [RTL PWideString documentation](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/pwidestring.html> "doc:rtl/system/pwidestring.html")



## String constants

If you use only English (ASCII) constants your strings work the same with all types, on all platforms and all compiler versions. Non English strings can be loaded via resourcestrings or from files. If you want to use non English strings in code then you should read further. 

There are various encodings for non English strings. By default Lazarus saves Pascal files as **UTF-8 without BOM**. UTF-8 supports the full Unicode range. That means all string constants are stored in UTF-8. Lazarus also supports to change the encoding of a file to other encoding, for example under Windows your local codepage. The Windows codepage is limited to your current language group. 

String Type, UTF-8 Source | With `{$codepage utf8}`? | FPC ≤ 2.6.5 | FPC ≥ 2.7.1 | FPC ≤ 2.7.1, [UTF8 as default CodePage](<Unicode_Support_in_Lazarus.md> "Unicode Support in Lazarus")  
---|---|---|---|---  
`AnAnsiString:='ãü';` | No | Needs UTF8ToAnsi in RTL/WinAPI. Ok in LCL | Needs UTF8ToAnsi in RTL/WinAPI. Ok in LCL | Ok in RTL/W-WinAPI/LCL. Needs UTF8ToWinCP in A-WinAPI.   
`AnAnsiString:='ãü';` | Yes | System cp ok in RTL/WinAPI. Needs SysToUTF8 in LCL | Ok in RTL/WinAPI/LCL. Mixing causes conversion. | Ok in RTL/W-WinAPI/LCL. Needs UTF8ToWinCP in A-WinAPI   
`AnUnicodeString:='ãü';` | No | Wrong everywhere | Wrong everywhere | Wrong everywhere   
`AnUnicodeString:='ãü';` | Yes | System cp ok in RTL/WinAPI. Needs UTF8Encode in LCL | Ok in RTL/WinAPI/LCL.Mixing causes conversion. | Ok in RTL/W-WinAPI/LCL. Needs UTF8ToWinCP in A-WinAPI   
`AnUTF8String:='ãü';` | No | Same as AnsiString | Wrong everywhere | Wrong everywhere   
`AnUTF8String:='ãü';` | Yes | Same as AnsiString | Ok in RTL/WinAPI/LCL. Mixing causes conversion. | Ok in RTL/W-WinAPI/LCL. Needs UTF8ToWinCP in A-WinAPI.   
  
  * W-WinAPI = Windows API "W" functions, UTF-16
  * A-WinAPI = Windows API non "W" functions, 8bit system ("ANSI") code page
  * System CP = The 8bit system code page of the OS. For example code page [1252](<http://en.wikipedia.org/wiki/Windows-1252>).


    
    
    const 
      c='ãü';
      cstring: string = 'ãü'; // see AnAnsiString:='ãü';
    var
      s: string;
      u: UnicodeString;
    begin
      s:=c; // same as s:='ãü';
      s:=cstring; // does not change encoding
      u:=c; // same as u:='ãü';
      u:=cstring; // fpc 2.6.1: converts from system cp to UTF-16, fpc 2.7.1+: depends on encoding of cstring
    end;
    

The rules for conversion are laid out in a ["Code page conversions"](<https://freepascal.org/docs-html/ref/refsu9.html#x32-380003.2.4>) section in the FPC manual. The basic point is that assigning an AnsiString (including the `CP_UTF8` specialization) to another AnsiString converts what is in the source to match the code page of the target string. A quirk for compatibility with presumably fpc ≤ 2.6.5 is that no such conversion will be done if one matches the source code CP and the other matches the system CP. In this case, forced (likely incorrect) interpretation as the target code page will occur. 

## See also

  * [FPC Unicode support](<FPC_Unicode_support.md> "FPC Unicode support")
  * [LCL Unicode Support](<LCL_Unicode_Support.md> "LCL Unicode Support")
  * [TStringList-TStrings Tutorial](<TStringList-TStrings_Tutorial.md> "TStringList-TStrings Tutorial")
  * [A Brief History of Strings](<http://web.archive.org/web/20151125114234/http://www.codexterity.com/delphistrings.htm>)

---

_Source: [https://wiki.freepascal.org/Character_and_string_types](https://web.archive.org/web/20250424182503/https://wiki.freepascal.org/Character_and_string_types)_
