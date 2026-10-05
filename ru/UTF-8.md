# UTF-8

│ **[English (en)](<../en/UTF-8.md>)** │  **русский (ru)** │

UTF-8 (8-bit **U** CS/Unicode **T** ransformation **F** ormat, или _8-битный Формат Преобразования UCS/Unicode_) - это кодировка символов переменной длины для Unicode. Символы Unicode от U+0000 до U+007F кодируются просто как байты от 00h до 7Fh. Это означает, что файлы и строки, содержащие только 7-битные символы [ASCII](<ASCII.md> "ASCII/ru"), имеют одинаковую кодировку как в ASCII, так и в UTF-8. 

Все символы > U+007F кодируются как последовательность из нескольких байтов, в каждом из которых установлены два старших значащих бита. Никакая другая последовательность байтов одного символа не содержится в более длинной последовательности байтов другого символа. Это позволяет легко искать подстроки. Первый байт многобайтовой последовательности, который представляет не-ASCII-символ, всегда находится в диапазоне от C0h до FDh, и он указывает, сколько байтов следует за этим символом. Все последующие байты в многобайтовой последовательности находятся в диапазоне от 80h до BFh. Это обеспечивает легкую ресинхронизацию и надежность. 

UTF-8 byte Sequences  Code points (Кодовые точки)  | 1st byte  | 2nd byte  | 3rd byte  | 4th byte  | старшие значащие биты первого байта многобайтовой последовательности  |   
---|---|---|---|---|---|---  
U+0000..U+007F  |  00..7F  |  |  |  |  0  |  [ASCII](<../en/ASCII.md> "ASCII")  
U+0080..U+07FF  |  C2..DF  |  80..BF  |  |  |  110  |  \- [UTF-8 Latin characters](<../en/UTF-8_Latin_characters.md> "UTF-8 Latin characters")  
U+0800..U+0FFF  |  E0  |  A0..BF  |  80..BF  |  |  1110  |   
U+1000..U+FFFF  |  E1..EF  |  80..BF  |  80..BF  |  |  1110  |  \- [подстрочные и надстрочные символы UTF8](<../en/UTF-8_subscripts_and_superscripts.md> "UTF-8 subscripts and superscripts")  
U+10000..U+3FFFF  |  F0  |  90..BF  |  80..BF  |  80..BF  |  11110  |   
U+40000..U+FFFFF  |  F1..F3  |  80..BF  |  80..BF  |  80..BF  |  11110  |   
U+100000..U+10FFFF  |  F4  |  80..BF  |  80..BF  |  80..BF  |  11110  |   
  
## Contents

  * 1 Функции UTF8
    * 1.1 FreePascal
    * 1.2 Lazarus
  * 2 См. также



## Функции UTF8

### FreePascal

Модуль system содержит несколько основных функций: 

  * UnicodeToUtf8
  * Utf8ToUnicode
  * UTF8Encode
  * UTF8Decode
  * AnsiToUtf8
  * Utf8ToAnsi



  


### Lazarus

Lazarus также содержит функции UTF8. Подробности см. [LCL Unicode Support](<Unicode_Support_in_Lazarus.md> "Unicode Support in Lazarus/ru")

## См. также

  * [Dealing with directory and filenames](<../en/LCL_Unicode_Support.md> "LCL Unicode Support") \- UTF8 функции для файлов
  * [LCL Unicode Support](<../en/LCL_Unicode_Support.md> "LCL Unicode Support") \- UTF8 в графических приложениях
  * [Console mode Pascal: Unicode (UTF8) output](<../en/Console_Mode_Pascal.md> "Console Mode Pascal") \- Отображение вывода UTF8 в программах консольного или текстового режима
  * [UTF8 strings and characters](<../en/UTF8_strings_and_characters.md> "UTF8 strings and characters")

---

_Source: [https://wiki.freepascal.org/UTF-8/ru](https://web.archive.org/web/20250327092237/https://wiki.freepascal.org/UTF-8/ru)_
