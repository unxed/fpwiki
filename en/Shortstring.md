# ShortString

│ **English (en)** │

**Memory requirement:** 256 bytes (1 byte for the length specification and 255 bytes for the characters). 

**Property:** The [data field](<Data_field.md> "Data field") of the [data type](<Data_type.md> "Data type") Shortstring is an array, made up of data fields of the data type [Char](<Char.md> "Char"). Its length is defined as: `ShortString = String[255];`

ShortString has the same properties as the string in Turbo Pascal. 

Definition of a data field of the data type ShortString: 
    
    
      var 
        s : ShortString;
    

Examples for the valid assignment values: 
    
    
        s := '0123ABCabc456';
        s := s + '! "§ $% & / () =?';
        s := 'c';
        s := s + IntToStr (45);
    

Examples of invalid assignment of values: 
    
    
        s := True;
        s := 4;
    

## Caveats

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** `low(myShortString)` returns `0`, i. e. the index of the length Byte, not the first character’s index. Likewise, `high(myShortString)` always returns `255`. Use `length` instead.

Nevertheless, a [`for … in` loop](<for-in_loop.md> "for-in loop") will work as expected. 

## See also

  * [Data types](<Data_type.md> "Data type")
  * [Character and string types](<Character_and_string_types.md> "Character and string types")

---

_Source: [https://wiki.freepascal.org/Shortstring](https://web.archive.org/web/20240711001437/https://wiki.freepascal.org/Shortstring)_
