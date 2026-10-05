# PChar

│ **[Deutsch (de)](</PChar/de> "PChar/de")** │  **English (en)** │  **[español (es)](</PChar/es> "PChar/es")** │  **[français (fr)](</PChar/fr> "PChar/fr")** │  **[русский (ru)](<../ru/PChar.md> "PChar/ru")** │    
****

The data type [`PChar`](<https://www.freepascal.org/docs-html/rtl/system/pchar.html>) is a [pointer](<Pointer.md> "Pointer") to a single [`char`](<Char.md> "Char"). [`PAnsiChar`](<https://www.freepascal.org/docs-html/rtl/system/pansichar.html>) is an alias for `PChar`. 
    
    
    type
    	PChar = ^char;
    	PAnsiChar = PChar;
    

## Contents

  * 1 special support
    * 1.1 direct assignments
    * 1.2 routines
    * 1.3 data types
  * 2 caveats
  * 3 comparative remarks
  * 4 see also



## special support

A `PChar` _could_ be interpreted as a pointer to NULL-terminated string data type value on the heap. This means this string value ends with `chr(0)`. This kind of storing data is rather unsafe and Pascal’s own string data types always indicate an explicit (maximum) length. However, many libraries written in other programming languages make use of this dangerous data type and in order to facilitate interaction with them the [FPC](<FPC.md> "FPC") provides special support (only available in _non_ -ISO modes). 

### direct assignments

A string literal can be assigned directly to a `PChar`: 
    
    
    program PCharDemo(output);
    var
    	p: PChar;
    begin
    	p := 'Foobar';
    	writeLn(p);
    end.
    

This assigns `p` the address of an invisible _constant_ value. That means you are not allowed to alter any component of this value. Something like `p[0] := 'f'` will crash. (Enforced since [FPC 3.0.0](<User_Changes_3.md> "User Changes 3.0")) 

### routines

[`Write`/`writeLn`](<Write.md> "Write") can accept a `PChar` and take it as a null-terminated string (provided the destination is a [`text` file](<Text.md> "Text")). Conversely [`writeStr`](<WriteStr.md> "WriteStr") does not support `PChar`. 

The standard [run-time library](<RTL.md> "RTL") provides many handy functions: 

  * [`system.strlen`](<https://www.freepascal.org/docs-html/rtl/system/strlen.html>) determines the length of a null-terminated string. The `length` function does this too.
  * [`system.strPas`](<https://www.freepascal.org/docs-html/rtl/system/strpas.html>) converts a null-terminated string to a [`shortString`](<Shortstring.md> "Shortstring").
  * All functions in the [`strings` unit](<https://www.freepascal.org/docs-html/rtl/strings/index.html>).



There is also `{$modeSwitch PCharToString}`/`‑MPCharToString` _automatically_ interpreting `PChar` values as null-terminated strings and _automatically_ converting them _to_ the destination’s `string` data type. 

### data types

The [`ANSIString` data type](<Ansistring.md> "Ansistring") is a pointer to a `char` and always ends with a _complimentary_ null-Byte which allows direct [typecasting](<Typecast.md> "Typecast") to `PChar`: 
    
    
    program ansiStringAsPCharDemo(output);
    var
    	s: ANSIString;
    	p: PChar;
    begin
    	s := 'foobar';
    	p := PChar(s);
    	writeLn(p);
    end.
    

Beware that this works well only as long as you are merely _reading from_ the typecasted value. Also note that an `ANSIString` _is allowed_ to contain `chr(0)` _as_ payload. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If you intend to _alter_ data _via_ the pointer, you possibly want to call [`uniqueString`](<https://www.freepascal.org/docs-html/rtl/system/uniquestring.html>) before you do the typecast.

## caveats

The `PChar` data type used as a null-terminated string data type is rather an anomaly in the Pascal world. It has many caveats: 

  * The definition of `char` depends on `{$modeSwitch unicodeStrings}`, that means in FPC 3.2.0 it could either refer to a 1‑Byte or 2‑Byte quantity. Most noticeably, this affects the offset if you are using pointer arithmetic, e. g. [`inc(PCharVariable)`](</index.php?title=Inc_and_DecSpecial_behaviors&action=edit&redlink=1> "Inc and DecSpecial behaviors \(page does not exist\)"). See [FPC Unicode support](<FPC_Unicode_support.md> "FPC Unicode support") for more details.
  * _In Pascal_ a `PChar` is just that: a _pointer_ to a single `char` value. 
    * That means the _string concatenation_ [operator `+`](<Plus.md> "Plus") does _not_ work. If `{$pointerMath on}` it will rather refer to arithmetic addition. You will need to use, for instance, [`strings.strCat`](<https://www.freepascal.org/docs-html/rtl/strings/strcat.html>) instead.
    * Also the comparison operator `=` will in fact compare _addresses_. Use, for instance, [`strings.strComp`](<https://www.freepascal.org/docs-html/rtl/strings/strcomp.html>) to compare the referenced null-terminated string values, unless you really want to ensure two null-terminated strings are _identical_ (= share the same memory).
  * Debugging is more difficult, confer [FPDebug](<FpDebug.md> "FpDebug").



Using a `PChar` _just_ as a _regular_ pointer to a _single_ `char`, however, does not have any special caveats. 

## comparative remarks

  * Delphi supports `PChar` as a null-terminated string.
  * The [GPC](<GNU_Pascal.md> "GNU Pascal") has a special data type `CString` which behaves like a null-terminated string. In GPC `PChar` is otherwise just a pointer.



## see also

  * [Character and string types](<Character_and_string_types.md> "Character and string types")
  * [Pascal for C users](<Pascal_for_C_users.md> "Pascal for C users")

---

_Source: [https://wiki.freepascal.org/Pansichar](https://web.archive.org/web/20250121212304/https://wiki.freepascal.org/Pansichar)_
