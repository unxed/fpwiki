# Char

│ **[English (en)](<../en/Char.md>)** │  **русский (ru)** │

Переменная типа **char** хранит один символ и в настоящее время имеет размер 1 байт ([AnsiChar](<AnsiChar.md> "AnsiChar/ru") является псевдонимом типа **char**). Однако, в будущем **char** может стать таким же типом, как [WideChar](</index.php?title=WideChar/ru&action=edit&redlink=1> "WideChar/ru \(page does not exist\)"). В настоящее время [byte](<Byte.md> "Byte/ru") и **char** почти синонимы - имеют размер 1 байт (8 бит), однако, **char** может использоваться только для хранения символов или части [строки](<String.md> "String/ru"), но не может использоваться в арифметических выражениях, в то время как **byte** может использоваться только как числовой тип. 

Например: 
    
    
     var ch: char;
          c: byte; 
    
     begin
       ch := 'A';  c := 65;  { одинаковые и допустимые действия }
       ch := 65;   c := 'A'; { в то время как их внутреннее представление одинаково, 
                               непосредственные присваивания между типами Char и Byte недопустимы }
     end.
    

Использование типов данных **char** или **byte** обеспечивает лучшую документированность при работе с конкретными переменными. Тип **char** может быть [приведен](</index.php?title=coercion/ru&action=edit&redlink=1> "coercion/ru \(page does not exist\)") к типу **byte** с помощью функции [ord](<Ord.md> "Ord/ru"). Значения типа **byte** могут быть приведены к типу **char** с помощью функции [chr](<Chr.md> "Chr/ru"). 

Функции для работы с типом **char** следуют таблице [ASCII](<ASCII.md> "ASCII/ru"). 

Исправленный вариант приведенной выше программы: 
    
    
     var ch: char;
          c: byte; 
    
     begin
        ch := 'A';      c := 65;        { одинаковые и допустимые действия }
        ch := chr(65);  c := ord('A');  { теперь допустимо }
        ch := Char(49); c := Byte('A'); { также допустимо и гарантированно выполнится при компиляции }
     end.
    

Переменная типа **char** также может использоваться в цикле в качестве счетчика: 
    
    
    var
       Loop:Char;
    
    Begin
       For Loop:='a' to 'c' do Writeln(Loop);
    end.
    

## См. также

  * [Символьные и строковые типы](<Character_and_string_types.md> "Character and string types/ru") \- подробное описание внутреннего представления, размещения в памяти и параметров доступа.

Типы данных   
---  
Простые типы  | [Boolean](<Boolean.md> "Boolean/ru") | [Byte](<Byte.md> "Byte/ru") | [Cardinal](<Cardinal.md> "Cardinal/ru") | Char | [Currency](<Currency.md> "Currency/ru") | [Extended](<Extended.md> "Extended/ru") | [Int64](<Int64.md> "Int64/ru") | [Integer](<Integer.md> "Integer/ru") | [Longint](<Longint.md> "Longint/ru") | [Pointer](<Pointer.md> "Pointer/ru") | [Real](<Real.md> "Real/ru") | [Shortint](<Shortint.md> "Shortint/ru") | [Smallint](<Smallint.md> "Smallint/ru") | [Word](<Word.md> "Word/ru")  
Сложные типы  | [Array](<Array.md> "Array/ru") | [Class](<Class.md> "Class/ru") | [Record](<Record.md> "Record/ru") | [Set](<Set.md> "Set/ru") | [String](<String.md> "String/ru") | [Shortstring](</index.php?title=Shortstring/ru&action=edit&redlink=1> "Shortstring/ru \(page does not exist\)")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Char/ru](https://web.archive.org/web/20250119191044/https://wiki.freepascal.org/Char/ru)_
