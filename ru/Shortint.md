# Shortint

│ **[English (en)](<../en/Shortint.md>)** │  **русский (ru)** │

Тип **shortint** является знаковым целым типом, поддерживающим значения в диапазоне от -128 до 127. Переменная типа **shortint** занимает 8 бит. 

Тип [Byte](<Byte.md> "Byte/ru") тоже 8-битный, но тип **byte** является беззнаковым типом. Это означает, что он представляет числа от 0 до 255. 

  

    
    
    var
      a_shortint: shortint;
      a_byte : byte;
      s1, s2 : string;
    begin
      a_shortint := %11110001;   // двоичное число
      a_byte     := %11110001;
      s1 := IntToStr(a_shortint); // s1 = '-15'
      s2 := IntToStr(a_byte);     // s2 = '241'
    

## См. также

  * [функция OnesComplement2](<Not.md> "Not/ru")
  * [Smallint](<Smallint.md> "Smallint/ru") \- целочисленный тип данных, поддерживающий значения в диапазоне от -32768 до 32767
  * [ двоичные числа](<Binary_numeral_system.md> "Binary numeral system/ru")
  * [IntToStr](</index.php?title=IntToStr/ru&action=edit&redlink=1> "IntToStr/ru \(page does not exist\)") \- преобразовывает целое число в строку

Типы данных   
---  
Простые типы  | [Boolean](<Boolean.md> "Boolean/ru") | [Byte](<Byte.md> "Byte/ru") | [Cardinal](<Cardinal.md> "Cardinal/ru") | [Char](<Char.md> "Char/ru") | [Currency](<Currency.md> "Currency/ru") | [Extended](<Extended.md> "Extended/ru") | [Int64](<Int64.md> "Int64/ru") | [Integer](<Integer.md> "Integer/ru") | [Longint](<Longint.md> "Longint/ru") | [Pointer](<Pointer.md> "Pointer/ru") | [Real](<Real.md> "Real/ru") | Shortint | [Smallint](<Smallint.md> "Smallint/ru") | [Word](<Word.md> "Word/ru")  
Сложные типы  | [Array](<Array.md> "Array/ru") | [Class](<Class.md> "Class/ru") | [Record](<Record.md> "Record/ru") | [Set](<Set.md> "Set/ru") | [String](<String.md> "String/ru") | [Shortstring](</index.php?title=Shortstring/ru&action=edit&redlink=1> "Shortstring/ru \(page does not exist\)")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Shortint/ru](https://web.archive.org/web/20250122171728/https://wiki.freepascal.org/Shortint/ru)_
