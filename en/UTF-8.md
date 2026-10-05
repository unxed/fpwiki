# UTF-8

│ **English (en)** │  **[suomi (fi)](</UTF-8/fi> "UTF-8/fi")** │  **[français (fr)](</UTF-8/fr> "UTF-8/fr")** │  **[русский (ru)](<../ru/UTF-8.md> "UTF-8/ru")** │    
****

UTF-8 (8-bit UCS/Unicode Transformation Format) is a variable-length character encoding for Unicode. Unicode characters U+0000 to U+007F are encoded simply as bytes 00h to 7Fh. This means that files and strings which contain only 7-bit [ASCII](<ASCII.md> "ASCII") characters have the same encoding under both ASCII and UTF-8. 

All characters > U+007F are encoded as a sequence of several bytes, each of which has the two most significant bits set. No byte sequence of one character is contained within a longer byte sequence of another character. This allows easy search for substrings. The first byte of a multibyte sequence that represents a non-ASCII character is always in the range C0h to FDh and it indicates how many bytes follow for this character. All further bytes in a multibyte sequence are in the range 80h to BFh. This allows easy resynchronization and robustness. 

  


UTF-8 byte Sequences  Code points  | 1st byte  | 2nd byte  | 3rd byte  | 4th byte  | most significant bits of the first byte of a multi-byte sequence  |   
---|---|---|---|---|---|---  
U+0000..U+007F  |  00..7F  |  |  |  |  0  |  [ASCII](<ASCII.md> "ASCII")  
U+0080..U+07FF  |  C2..DF  |  80..BF  |  |  |  110  |  \- [UTF-8 Latin characters](<UTF-8_Latin_characters.md> "UTF-8 Latin characters")  
U+0800..U+0FFF  |  E0  |  A0..BF  |  80..BF  |  |  1110  |   
U+1000..U+FFFF  |  E1..EF  |  80..BF  |  80..BF  |  |  1110  |  \- [UTF-8_subscripts_and_superscripts](<UTF-8_subscripts_and_superscripts.md> "UTF-8 subscripts and superscripts")  
U+10000..U+3FFFF  |  F0  |  90..BF  |  80..BF  |  80..BF  |  11110  |   
U+40000..U+FFFFF  |  F1..F3  |  80..BF  |  80..BF  |  80..BF  |  11110  |   
U+100000..U+10FFFF  |  F4  |  80..BF  |  80..BF  |  80..BF  |  11110  |   
  
## Contents

  * 1 UTF8 functions
    * 1.1 FreePascal
    * 1.2 Lazarus
  * 2 See also



## UTF8 functions

### FreePascal

The system unit contains some basic functions: 

  * UnicodeToUtf8
  * Utf8ToUnicode
  * UTF8Encode
  * UTF8Decode
  * AnsiToUtf8
  * Utf8ToAnsi



  


### Lazarus

Lazarus also contains UTF8 functions. For more details see [LCL Unicode Support](<LCL_Unicode_Support.md> "LCL Unicode Support")

## See also

  * [Dealing with directory and filenames](<LCL_Unicode_Support.md> "LCL Unicode Support") \- UTF8 functions for files
  * [LCL Unicode Support](<LCL_Unicode_Support.md> "LCL Unicode Support") \- UTF8 in graphical applications
  * [Console mode Pascal: Unicode (UTF8) output](<Console_Mode_Pascal.md> "Console Mode Pascal") \- Showing UTF8 output in console mode/text mode programs
  * [UTF8 strings and characters](<UTF8_strings_and_characters.md> "UTF8 strings and characters")

---

_Source: [https://wiki.freepascal.org/UTF-8](https://web.archive.org/web/20250324154200/https://wiki.freepascal.org/UTF-8)_
