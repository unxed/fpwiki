# AnsiString

│ **English (en)** │

**`AnsiString`** is a variable-length string [data type](<Data_type.md> "Data type"). It can store characters that have a size of one Byte. 

## Contents

  * 1 implementation
  * 2 application
  * 3 caveats
  * 4 see also



## implementation

In [FPC](<FPC.md> "FPC") an `AnsiString` is implemented as a [pointer](<Pointer.md> "Pointer"). It is a managed data type. As such it is initialized with [`nil`](<Nil.md> "Nil") as soon as it enters the scope. Memory for the character sequence is dynamically allocated and freed. 

An `AnsiString` points to the _first_ character. This facilitates interfacing to libraries or foreign functions expecting [`pChar` strings](<PChar.md> "PChar"). For that, an `AnsiString` always concludes with a null [Byte](<Byte.md> "Byte"). _In[Pascal](<Pascal.md> "Pascal")_, this terminating null Byte has no significance as to the string’s value (including its length). An `AnsiString` always entails some management data _before_ the first character. These are 

  * a code page
  * the size of a character
  * a reference count
  * the length of the string.

`253` | `233` |  `0` |  `1` |  `0` |  `0` |  `0` |  `1` |  `0` |  `0` |  `0` |  `3` | `'F'` | `'o'` | `'o'` |  `#0`  
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---  
code page  | maximum character size  | reference count  | length  | payload  | complimentary Null   
pointer points here ⤴  |   
`AnsiString` memory layout sample ([32-bit](<32_bit.md> "32 bit") platform) 

Only the length field has significance in Pascal. In Pascal, an `AnsiString` may contain `#0` characters. 

An `AnsiString` can furthermore be associated with a code page (since [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0")). 

## application

The data type `AnsiString` can be used like any other string data type. You may [assign](<Becomes.md> "Becomes") string literals to an `AnsiString` variable as normal. String values can be compared ([`=`](<Equal.md> "Equal")) just as usual. The entire pointer-characteristic is transparent. 

Characters in `AnsiString` have a 1-based index. `myAnsiString[1]` refers to the first character. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The linear character index is only guaranteed to work for strings that have a maximum character size of `1`. That means, using an integer index for example on an UTF-8 encoded string (not exclusively containing ASCII characters) will produce erroneous results.

The [`length` function](<https://www.freepascal.org/docs-html/rtl/system/length.html>), and for that matter also `high`, will return a string’s length by examining the length data field. 

Because an `AnsiString` is essentially a pointer, _copying_ strings of this type is fast, since only the reference is copied and the reference count increased. Modifications may trigger a COW. 

## caveats

  * The compiler directive [`{$longStrings on}`](<$H.md> "$H") (or `{$H+}`) [aliases `string`](<Defensive_programming_techniques.md> "Defensive programming techniques") (without a specified length) to `AnsiString`.
  * `AnsiString` as a _managed_ data type introduces a certain overhead. See [Avoiding implicit try finally section](<Avoiding_implicit_try_finally_section.md> "Avoiding implicit try finally section") for more explanations.
  * The [`sizeOf`](<SizeOf.md> "SizeOf") value of an `AnsiString` variable is merely the size of a pointer.
  * Assigning an empty string `''` to an `AnsiString` variable will in fact assign `nil` to the variable and, if the reference count hit zero, release underlying memory (if any was previously allocated at all). Empty strings are _not_ stored as described above.



## see also

  * [Character and string types](<Character_and_string_types.md> "Character and string types")
  * [Ansistrings](<https://www.freepascal.org/docs-html/current/ref/refsu9.html#x32-370003.2.4>) in the reference guide
  * [Ansistrings](<https://www.freepascal.org/docs-html/current/prog/progsu161.html#x205-2160008.2.7>) in the programmer’s guide


  *[COW]: copy-on-write

---

_Source: [https://wiki.freepascal.org/Ansistring](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Ansistring)_
