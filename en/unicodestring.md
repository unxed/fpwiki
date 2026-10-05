# UnicodeString

│ **English (en)** │

  
Back to [data types](<Data_type.md> "Data type"). 

Back to [Character and string types](<Character_and_string_types.md> "Character and string types"). 

  
A [data field](<Data_field.md> "Data field") of the data type **UnicodeString** has no size restriction and consists internally of an array of the [UniCodeChar](<Unicodechar.md> "Unicodechar") data type. 

The functions of the LazUtf8 unit are required for problem-free type conversion from [AnsiString](<Ansistring.md> "Ansistring") to UnicodeString and from UnicodeString to AnsiString. 

Unicode strings are used to display strings from the Unicode character set. Unicode strings are implemented in the same way as AnsiStrings and can be cast (converted) to the [PUnicodeChar](<Punicodechar.md> "Punicodechar") data type. 

Definition of a data field of data type **UnicodeString** : 
    
    
    var 
      u : UnicodeString;
      a : AnsiString;
    

The examples below apply to the Windows operating system! 

Examples for the valid assignment of [AnsiString](<Ansistring.md> "Ansistring") to [WideString](<Widestring.md> "Widestring"): 
    
    
      u := UTF8ToUTF16('0123ABCabc456AöU!, .-');
      u := u + UTF8ToUTF16(IntToString(45));
    

Example of the valid assignment of [WideString](<Widestring.md> "Widestring") to [AnsiString](<Ansistring.md> "Ansistring"): 
    
    
      a := UTF16ToUTF8(u);

---

_Source: [https://wiki.freepascal.org/unicodestring](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/unicodestring)_
