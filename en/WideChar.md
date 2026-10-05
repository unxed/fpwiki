# WideChar

A variable of type **WideChar** , which has a synonym of [UnicodeChar](<UnicodeChar.md> "UnicodeChar") _(type UnicodeChar = WideChar;)_ , is exactly 2 bytes in size, and usually contains one Unicode character in UTF-16 encoding. As it is impossible to encode all Unicode code points (a code point normally corresponds to a character) in 2 bytes, two WideChars may be needed to encode a single code point. 

As of version 3 of Free Pascal, the [Char](<Char.md> "Char") datatype is a synonym for an [AnsiChar](<AnsiChar.md> "AnsiChar"). However, in the future the Free Pascal compiler may consider Char a synonym for WideChar. 

## See also

  * [Character and string types](<Character_and_string_types.md> "Character and string types")

---

_Source: [https://wiki.freepascal.org/WideChar](https://web.archive.org/web/20240716030517/https://wiki.freepascal.org/WideChar)_
