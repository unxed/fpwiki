# Character and string types

│ **[Deutsch (de)](</Character_and_string_types/de> "Character and string types/de")** │  **[English (en)](<../en/Character_and_string_types.md> "Character and string types")** │  **[español (es)](</Character_and_string_types/es> "Character and string types/es")** │  **[français (fr)](</Character_and_string_types/fr> "Character and string types/fr")** │  **русский (ru)** │  **[中文（中国大陆） (zh_CN)](</Character_and_string_types/zh_CN> "Character and string types/zh CN")** │    
****

Free Pascal поддерживает несколько **[символьных](<../en/Char.md> "Char") и [строковых](<../en/String.md> "String") типов**, от одиночных символов ANSI до юникодных строк, включая указатели. Различия также касаются кодировок и подсчёта ссылок. 

## Contents

  * 1 AnsiChar
    * 1.1 Ссылки
  * 2 WideChar
    * 2.1 Ссылки
  * 3 Символьный массив
    * 3.1 Статический символьный массив
    * 3.2 Динамический символьный массив
  * 4 PChar
    * 4.1 Ссылки
  * 5 PWideChar
    * 5.1 Ссылки
  * 6 String
    * 6.1 Ссылки
  * 7 ShortString
    * 7.1 Ссылки
  * 8 AnsiString
    * 8.1 Ссылки
  * 9 UnicodeString
    * 9.1 Ссылки
  * 10 UTF8String
    * 10.1 Ссылки
  * 11 UTF16String
    * 11.1 Ссылки
  * 12 WideString
    * 12.1 Ссылки
  * 13 PShortString
    * 13.1 Ссылки
  * 14 PAnsiString
    * 14.1 Ссылки
  * 15 PUnicodeString
    * 15.1 Ссылки
  * 16 PWideString
    * 16.1 Ссылки
  * 17 Строковые константы
  * 18 См. также



## AnsiChar

Переменная типа **AnsiChar** , называемого также **[Char](<../en/Char.md> "Char")** , имеет размер ровно 1 байт и содержит один символ ANSI. 

a   
---  
  
#### Ссылки

  * [AnsiChar в документации FPC](<http://www.freepascal.org/docs-html/ref/refsu6.html>)
  * [Использование Char](<../en/Char.md> "Char")



## WideChar

Переменная типа **WideChar** , называемого также **UnicodeChar** , имеет размер ровно 2 байта и обычно содержит одну [юникодную](<LCL_Unicode_Support.md> "LCL Unicode Support/ru") кодовую точку (обычно один символ) в кодировке UTF-16. Примечание: все юникодные кодовые точки закодировать 2-я байтами невозможно. Поэтому для представления одной кодовой точки может потребоваться пара WideChar. 

| a   
---|---  
  
#### Ссылки

  * [WideChar в документации FPC](<http://www.freepascal.org/docs-html/ref/refsu7.html>)
  * [Сведения об UTF-16 в Wikipedia](<http://ru.wikipedia.org/wiki/UTF-16>)
  * [UnicodeChar в документации RTL](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/unicodechar.html> "doc:rtl/system/unicodechar.html")



## Символьный массив

Ранние реализации языка Паскаль, использовавшиеся до 1978 года, не поддерживали строковый тип (за исключением строковых констант). Единственной возможностью сохранять строки в переменных было использование массивов символов. Этот подход имеет множество недостатков и более не рекомендуется. Тем не менее, он по-прежнему поддерживается для обеспечения обратной совместимости с устаревшим кодом. 

### Статический символьный массив
    
    
    type
      TOldString4 = array [0..3] of Char;
    var
      aOldString4: TOldString4; 
    begin
      aOldString4[0] := 'a';
      aOldString4[1] := 'b';
      aOldString4[2] := 'c';
      aOldString4[3] := 'd';
    end;
    

Статический символьный массив теперь содержит: 

a | b | c | d   
---|---|---|---  
  
![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Неинициализированные символы могут иметь любые значения, которые зависят от содержимого памяти при её выделении под массив.

### Динамический символьный массив
    
    
    var
      aOldString: Array of Char; 
    begin
      SetLength(aOldString, 5);
      aOldString[0] := 'a';
      aOldString[1] := 'b';
      aOldString[2] := 'c';
      aOldString[3] := 'd';
    end;
    

Динамический символьный массив теперь содержит: 

a | b | c | d | #0   
---|---|---|---|---  
  
![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Неинициализированные символы динамического массива содержат #0, поскольку содержимое динамического массива изначально инициализируется 0 (или #0, или nil, или ...)

## PChar

Переменные типа **[PChar](<../en/PChar.md> "PChar")** являются указателями на **Char** , а также позволяют дополнительные действия. Они могут быть использованы для доступа к [нуль-терминированным строкам](<http://ru.wikipedia.org/wiki/Нуль-терминированная_строка>) в стиле C, например, для взаимодействия с библиотеками ОС или сторонними программами. 

a | b | c | #0   
---|---|---|---  
^   
  
#### Ссылки

  * [PChar в документации FPC](<http://www.freepascal.org/docs-html/ref/refsu12.html>)
  * [Функции для PChar](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/pcharfunctions.html> "doc:rtl/sysutils/pcharfunctions.html")



## PWideChar

Переменная типа **PWideChar** является указателем на WideChar. 

| a |  | b |  | c | #0 | #0   
---|---|---|---|---|---|---|---  
^   
  
#### Ссылки

  * [PWideChar в документации RTL](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/pwidechar.html> "doc:rtl/system/pwidechar.html")



## String

Тип **String** может означать **ShortString** или **AnsiString** , в зависимости от [опции {$H}](<http://www.freepascal.org/docs-html/prog/progsu25.html#x32-310001.2.25>). Когда опция выключена ({$H-}), String означает **ShortString** (короткая строка). Если не указано иное, её размер 255 символов. Когда опция включена ({$H+}), **String** без указания длины означает **AnsiString** , в противном случае -- **ShortString** заданной длины. В режиме **{$mode DelphiUnicode}** **String** означает **UnicodeString**. 

#### Ссылки

  * [Использование String](<../en/String.md> "String")
  * [Строковые функции](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/stringfunctions.html> "doc:rtl/sysutils/stringfunctions.html")
  * [Справка по модулю 'strutils': Процедуры и функции](<http://lazarus-ccr.sourceforge.net/docs/rtl/strutils/index-5.html> "doc:rtl/strutils/index-5.html")



## ShortString

Максимальная длина коротких строк не превышает 255 символов в [кодовой странице](<../en/FPC_Unicode_support.md> "FPC Unicode support") CP_ACP. Длина хранится в символе с индексом 0. 

#3 | a | b | c   
---|---|---|---  
  
#### Ссылки

  * [AnsiString в документации FPC](<http://lazarus-ccr.sourceforge.net/docs/ref/refsu12.html> "doc:ref/refsu12.html")



## AnsiString

AnsiString является строкой без ограничения длины, с [подсчётом ссылок](<http://ru.wikipedia.org/wiki/Подсчёт_ссылок>) и гарантированно [нуль-терминирована](<http://ru.wikipedia.org/wiki/Нуль-терминированная_строка>). Переменная типа **AnsiString** устроена как указатель -- фактическое содержимое строки хранится в куче, памяти выделяется по объёму содержимого. 

|  |  |  |  |  |  |  | a | b | c | #0   
---|---|---|---|---|---|---|---|---|---|---|---  
RefCount | Length   
  
#### Ссылки

  * [AnsiString в документации FPC](<http://www.freepascal.org/docs-html/ref/refsu12.html>)



## UnicodeString

**UnicodeStrings** как и **AnsiStrings** использует подсчет ссылок и завершение нулём, но реализованы как массивы из **WideChar** , а не обычных **Char**. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Вероятно, название UnicodeString двусмысленно из-за его применения в Delphi для Windows, использующей кодировку UTF-16; но это не единственный строковый тип, способный хранить юникодные строки (см. также UTF8String)...

|  |  |  |  |  |  |  |  | a |  | b |  | c | #0 | #0   
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---  
RefCount | Length   
  
#### Ссылки

  * [UnicodeString в документации FPC](<http://www.freepascal.org/docs-html/ref/refsu13.html>)



## UTF8String

В FPC 2.6.5 и ранее тип **UTF8String** был псевдонимом **AnsiString**. В FPC 2.7.1 и далее он определён как 
    
    
    UTF8String          = type AnsiString(CP_UTF8);
    

Он предназначен для хранения строк в UTF-8 (в юникоде), от 1 до 4 байт на символ. Заметьте, **String** тоже может содержать символы в кодировке UTF-8. 

#### Ссылки

  * [UTF8String в документации RTL](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/utf8string.html> "doc:rtl/system/utf8string.html")



## UTF16String

Тип **UTF16String** является псевдонимом типа **WideString**. В модуле LCL _lclproc_ это псевдоним для **UnicodeString**. 

#### Ссылки

  * [UTF16String в документации LCL](<http://lazarus-ccr.sourceforge.net/docs/lcl/lclproc/utf16string.html> "doc:lcl/lclproc/utf16string.html")



## WideString

Тип **[WideString](<../en/Widestrings.md> "Widestrings")** (используется для представления юникодных строк в приложениях COM) похож на **UnicodeString** , но в отличие от него не использует подсчёт ссылок. Под Windows переменные этого типа распределяются специальными функциями системы, что позволяет использовать из для автоматизации OLE. 

Широкие строки под Windows состоят из совместимых с COM и закодированных в UTF-16 (UCS2 под Windows 2000) байт, под Linux, Mac OS X и iOS они кодируются в обычный UTF-16. 

|  |  |  |  | a |  | b |  | c | #0 | #0   
---|---|---|---|---|---|---|---|---|---|---|---  
Length   
  
#### Ссылки

  * [WideString в документации FPC](<http://www.freepascal.org/docs-html/ref/refsu14.html#x37-400003.2.8>)



## PShortString

Переменная типа **PShortString** является указателем на первый байт переменной типа **ShortString** (содержащий длину строки). 

#3 | a | b | c   
---|---|---|---  
^   
  
#### Ссылки

  * [PShortString в документации RTL](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/pshortstring.html> "doc:rtl/system/pshortstring.html")



## PAnsiString

Переменные типа **PAnsiString** являются указателями на переменные типа **AnsiString**. Однако, в отличие от переменных типа **PShortString** , они указывают не на первый байт заголовка, а на первый символ **Char** из **AnsiString**. 

|  |  |  |  |  |  |  | a | b | c | #0   
---|---|---|---|---|---|---|---|---|---|---|---  
RefCount | Length | ^   
  
#### Ссылки

  * [PAnsiString в документации RTL](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/pansistring.html> "doc:rtl/system/pansistring.html")



## PUnicodeString

Переменные типа **PUnicodeString** являются указателями на переменные типа **UnicodeString**. 

|  |  |  |  |  |  |  |  | a |  | b |  | c | #0 | #0   
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---  
RefCount | Length | ^   
  
#### Ссылки

  * [PUnicodeString в документации RTL](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/punicodestring.html> "doc:rtl/system/punicodestring.html")



## PWideString

Переменные типа **PWideString** являются указателями. Они указывают на первый символ переменной типа **WideString**. 

|  |  |  |  | a |  | b |  | c | #0 | #0   
---|---|---|---|---|---|---|---|---|---|---|---  
Length | ^   
  
#### Ссылки

  * [PWideString в документации RTL](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/pwidestring.html> "doc:rtl/system/pwidestring.html")



## Строковые константы

Если вы используете только английские константы, такие строки работают одинаково со всеми типами на всех платформах и на всех версиях компилятора. Неанглийские строки могут быть загружены как ResourceString или из файлов. Если вам нужно использовать неанглийские строки в коде, читайте ниже. 

Для неанглийских строк существует множество кодировок. По умолчанию Лазарус сохраняет файлы как **UTF-8 без[BOM](<http://ru.wikipedia.org/wiki/Маркер_последовательности_байтов>)**. Кодировка UTF-8 поддерживает все символы юникода. Это означает, что все строковые константы также сохранены в UTF-8. Лазарус также поддерживает смену кодировки файла, например, под Windows можно сохранить файл в локальной кодировке. Кодировки Windows ограничены вашей текущей языковой группой. 

Строковый тип, исходник в UTF-8 | С или без {$codepage utf8} | FPC 2.6.5 и ниже | FPC 2.7.1 и выше | FPC 2.7.1+ с кодовой страницей UTF8   
---|---|---|---|---  
AnAnsiString:='ãü'; | Без | Требует UTF8ToAnsi в RTL/WinAPI. Ок в LCL | Требует UTF8ToAnsi в RTL/WinAPI. Ок в LCL | Ок в RTL/W-WinAPI/LCL. Требует UTF8ToWinCP в A-WinAPI   
AnAnsiString:='ãü'; | С | Системная КС в RTL/WinAPI ок. Требует SysToUTF8 в LCL | Ок в RTL/WinAPI/LCL. Смешение с другими строками преобразует в системную КС | Ок в RTL/W-WinAPI/LCL. Требует UTF8ToWinCP в A-WinAPI   
AnUnicodeString:='ãü'; | Без | Неверно везде | Неверно везде | Неверно везде   
AnUnicodeString:='ãü'; | С | Системная КС в RTL/WinAPI ок. Требует UTF8Encode в LCL | Ок в RTL/WinAPI/LCL. Смешение с другими строками преобразует в системную КС | Ок в RTL/W-WinAPI/LCL. Требует UTF8ToWinCP в A-WinAPI   
AnUTF8String:='ãü'; | Без | Как AnsiString | Неверно везде | Неверно везде   
AnUTF8String:='ãü'; | С | Как AnsiString | Ок в RTL/WinAPI/LCL. Смешение с другими строками преобразует в системную КС | Ок в RTL/W-WinAPI/LCL. Требует UTF8ToWinCP в A-WinAPI   
  
  * W-WinAPI = функции Windows API с "W", UTF-16
  * A-WinAPI = функции Windows API без "W", 8-битная кодовая страница
  * Системная КС = 8-битная системная кодовая страница ОС. Например, [1251](<http://ru.wikipedia.org/wiki/Windows-1251>).


    
    
    const 
      c='ãü';
      cstring: string = 'ãü'; // см. AnAnsiString:='ãü';
    var
      s: string;
      u: UnicodeString;
    begin
      s:=c;       // то же, что s:='ãü';
      s:=cstring; // не изменяет кодировку
      u:=c;       // то же, что u:='ãü';
      u:=cstring; // fpc 2.6.1: преобразует из системной КС в UTF-16,
                  // fpc 2.7.1+: зависит от кодировки cstring
    end;
    

## См. также

  * [Поддержка юникода в FPC](<FPC_Unicode_support.md> "FPC Unicode support/ru")
  * [Поддержка юникода в LCL](<LCL_Unicode_Support.md> "LCL Unicode Support/ru")
  * [Пособие по TStringList-TStrings](<TStringList-TStrings_Tutorial.md> "TStringList-TStrings Tutorial/ru")
  * [Краткая история String](<http://www.codexterity.com/delphistrings.htm>)

---

_Source: [https://wiki.freepascal.org/Character_and_string_types/ru](https://web.archive.org/web/20250420040455/https://wiki.freepascal.org/Character_and_string_types/ru)_
