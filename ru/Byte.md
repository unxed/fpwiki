# Byte

│ **[Deutsch (de)](</Byte/de> "Byte/de")** │  **[English (en)](<../en/Byte.md> "Byte")** │  **[español (es)](</Byte/es> "Byte/es")** │  **[suomi (fi)](</Byte/fi> "Byte/fi")** │  **[français (fr)](</Byte/fr> "Byte/fr")** │  **[italiano (it)](</Byte/it> "Byte/it")** │  **русский (ru)** │  **[中文（中国大陆） (zh_CN)](</Byte/zh_CN> "Byte/zh CN")** │    
****

Тип `byte` (байт) является беззнаковым [`integer`](<Integer.md> "Integer/ru") (целым) типом, представляющим значения в диапазоне `0..255` и занимающим **8 бит**. Типы `byte` и [`char`](<Char.md> "Char/ru") являются одним и тем же в [FPC](<../en/FPC.md> "FPC") версии 3. 

## Contents

  * 1 Корректные значения
  * 2 Стандартные функции
    * 2.1 Преобразование в символ и из него
    * 2.2 Строковое представление



## Корректные значения

Ключевое отличие состоит в том, что `byte` может использоваться только в качестве числового [`type`](<Type.md> "Type/ru")(типа), тогда как `char` может использоваться как символ или как часть строкового типа и не может использоваться в арифметическом выражении. `byte` всегда будет иметь тот же размер, что и [`ansiChar`](<AnsiChar.md> "AnsiChar/ru"), но в будущем `char` может считаться синонимом [`wideChar`](<../en/WideChar.md> "WideChar"), а не [`ansiChar`](<AnsiChar.md> "AnsiChar/ru"). 

Например: 
    
    
    var 
      c: byte; 
      ch: char;
    begin
      c := 65;  ch := 'A';  { одинаковые и допустимые действия }
      c := 'A'; ch := 65;   { в то время как эти действия также одинаковы, но недопустимы }
    end.
    

Использование типов данных `byte` или `byte` обеспечивает лучшую документированность при работе с конкретными переменными. 

## Стандартные функции

### Преобразование в символ и из него

Тип `byte` может быть [приведен](</index.php?title=coercion&action=edit&redlink=1> "coercion \(page does not exist\)") к типу `char` с помощью [функции `chr`](<Chr.md> "Chr/ru"). Значения типа `chr` могут быть приведены к типу `byte` с помощью [функции `ord`](<Ord.md> "Ord/ru"). 

Исправленный вариант приведенной выше программы: 
    
    
    program ordChrDemo(input, output, stderr);
    var
      foo: byte;
      bar: char;
    
    begin
      foo := 65;
      bar := 'A';
    	
      foo := ord('A');
      // chr(65) это эквивалент #65
      bar := chr(65);
      bar := #65;
    	
      // альтернатива: приведения типов
      // приведения типов константных выражений
      // гарантированно произойдет во время компиляции
      foo := byte('A');
      bar := char(65);
    end.
    

### Строковое представление

Функцию [`binStr` function](<https://www.freepascal.org/docs-html/rtl/system/binstr.html>) из модуля [`system`](<../en/System_unit.md> "System unit") можно использовать для получения [`string`](<String.md> "String/ru") (строки), показывающей [двоичное представление](<Binary_numeral_system.md> "Binary numeral system/ru") `byte`: 
    
    
    program binStrDemo(input, output, stderr);
    
    var
    	foo: byte;
    
    begin
    	foo := 10;
    	writeLn(binStr(foo, 8));
    end.
    

Выведет: 
    
    
    00001010
    

Более универсальной функцией является [`intToBin`](<https://www.freepascal.org/docs-html/rtl/strutils/inttobin.html>), предоставляемая модулем `strUtils`. 

Типы данных   
---  
Простые типы  | [Boolean](<Boolean.md> "Boolean/ru") | Byte | [Cardinal](<Cardinal.md> "Cardinal/ru") | [Char](<Char.md> "Char/ru") | [Currency](<Currency.md> "Currency/ru") | [Extended](<Extended.md> "Extended/ru") | [Int64](<Int64.md> "Int64/ru") | [Integer](<Integer.md> "Integer/ru") | [Longint](<Longint.md> "Longint/ru") | [Pointer](<Pointer.md> "Pointer/ru") | [Real](<Real.md> "Real/ru") | [Shortint](<Shortint.md> "Shortint/ru") | [Smallint](<Smallint.md> "Smallint/ru") | [Word](<Word.md> "Word/ru")  
Сложные типы  | [Array](<Array.md> "Array/ru") | [Class](<Class.md> "Class/ru") | [Record](<Record.md> "Record/ru") | [Set](<Set.md> "Set/ru") | [String](<String.md> "String/ru") | [Shortstring](</index.php?title=Shortstring/ru&action=edit&redlink=1> "Shortstring/ru \(page does not exist\)")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Byte/ru](https://web.archive.org/web/20250123175335/https://wiki.freepascal.org/Byte/ru)_
