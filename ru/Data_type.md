# Data type

│ **[English (en)](<../en/Data_type.md>)** │  **русский (ru)** │

## Contents

  * 1 Общее
  * 2 Целочисленные типы
    * 2.1 Беззнаковые типы
    * 2.2 Типы со знаком
  * 3 Типы с плавающей точкой
  * 4 Логические(булевы) типы
  * 5 Перечислимые типы
  * 6 Символьные типы
    * 6.1 Типы символов с однобайтовой кодировкой
    * 6.2 Типы символов с многобайтовой кодировкой
  * 7 Вариантные типы
  * 8 Константы
  * 9 Структурные типы
  * 10 Типы поддиапазонов
  * 11 Указатель
  * 12 Классы и объекты



  


# Общее

На этой странице представлена подборка типов данных в Free Pascal.   
**Тип данных** \- это шаблон для [поля данных](<../en/Data_field.md> "Data field").   
Тип данных поля определяет, как компилятор и процессор интерпретируют его содержимое.   
Видимость [поля данных](<../en/Data_field.md> "Data field") зависит от местоположения его объявления. 

# Целочисленные типы

## Беззнаковые типы

[Поля данных](<../en/Data_field.md> "Data field") целых типов без знака могут содержать только «положительные» целые числа. 

  * [UInt8](<../en/UInt8.md> "UInt8") \- Диапазон: (0 .. 255)
  * [Byte](<Byte.md> "Byte/ru") \- Диапазон: (0 .. 255)
  * [UInt16](<../en/UInt16.md> "UInt16") \- Диапазон: (0 .. 65535)
  * [Word](<Word.md> "Word/ru") \- Диапазон: (0 .. 65535)
  * [NativeUInt](<../en/NativeUInt.md> "NativeUInt") \- Диапазон: зависит от типа процессора.
  * [DWord](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") \- эквивалентно Longword.
  * [Cardinal](<Cardinal.md> "Cardinal/ru") \- эквивалентно Longword.
  * [UInt32](<../en/UInt32.md> "UInt32") \- Диапазон: (0 .. 4294967295)
  * [Longword](<Longword.md> "Longword/ru") \- Диапазон: (0 .. 4294967295)
  * [UInt64](</index.php?title=UInt64&action=edit&redlink=1> "UInt64 \(page does not exist\)") \- Диапазон: (0 .. 18446744073709551615)
  * [QWord](<../en/QWord.md> "QWord") \- Диапазон: (0 .. 18446744073709551615)



  


## Типы со знаком

[Поля данных](<../en/Data_field.md> "Data field") целых типов со знаком могут содержать положительные **и** отрицательные целые числа. 

  * [Int8](<../en/Int8.md> "Int8") \- Диапазон: (-128 .. 127)
  * [ShortInt](<Shortint.md> "Shortint/ru") \- Диапазон: (-128 .. 127)
  * [Int16](<../en/Int16.md> "Int16") \- Диапазон: (-32768 .. 32767)
  * [SmallInt](<Smallint.md> "Smallint/ru") \- Диапазон: (-32768 .. 32767)
  * [Integer](<Integer.md> "Integer/ru") \- Диапазон: это эквивалент либо Smallint, либо Longint (для 16 или 32-разрядных процессоров соответственно).
  * [Int32](<../en/Int32.md> "Int32") \- Диапазон: (-2147483648 .. 2147483647)
  * [NativeInt](<../en/NativeInt.md> "NativeInt") \- Диапазон: зависит от типа процессора.
  * [Longint](<Longint.md> "Longint/ru") \- Диапазон: (-2147483648 .. 2147483647)
  * [Int64](<Int64.md> "Int64/ru") \- Диапазон: (-9223372036854775808 .. 9223372036854775807)



  


# Типы с плавающей точкой

[Поля данных](<../en/Data_field.md> "Data field") типов с плавающей точкой могут содержать: 

  1. положительные **и** отрицательные целые числа с возможными ошибками округления.
  2. положительные **и** отрицательные числа с плавающей точкой.



  


  * [Single](<../en/Single.md> "Single") \- Диапазон: (1.5E-45 .. 3.4E38)
  * [Real](<../en/Real.md> "Real") \- Диапазон: зависит от платформы.
  * [Real48](</index.php?title=Real48&action=edit&redlink=1> "Real48 \(page does not exist\)") \- Диапазон: 2.9E-39 .. 1.7E38
  * [Double](<../en/Double.md> "Double") \- Диапазон: (5.0E-324 .. 1.7E308)
  * [Extended](<../en/Extended.md> "Extended") \- Диапазон: зависит от платформы.
  * [Comp](<../en/Comp.md> "Comp") \- Диапазон: (-2E64+1 .. 2E63-1)
  * [Currency](<../en/Currency.md> "Currency") \- Диапазон: (-922337203685477.5808 .. 922337203685477.5807)



  


# Логические(булевы) типы

[Поля данных](<../en/Data_field.md> "Data field") логического типа содержат значения истинности. 

  * [Boolean](<../en/Boolean.md> "Boolean") \- Диапазон: (True, False), 8 Bit
  * [ByteBool](</index.php?title=Bytebool&action=edit&redlink=1> "Bytebool \(page does not exist\)") \- Диапазон: (True, False), 8 Bit
  * [WordBool](<../en/Wordbool.md> "Wordbool") \- Диапазон: (True, False), 16 Bit
  * [LongBool](<../en/Longbool.md> "Longbool") \- Диапазон: (True, False), 32 Bit



  


# Перечислимые типы

[Поля данных](<../en/Data_field.md> "Data field") перечислимого типа являются «списками» (перечислениями) целочисленных беззнаковых констант. 

  * [Enum Type](<../en/Enum_Type.md> "Enum Type") \- Диапазон: (интегральные типы данных)



  


# Символьные типы

## Типы символов с однобайтовой кодировкой

  * [Char](<../en/Char.md> "Char") \- Постоянная длина: 1 байт, представление: 1 символ.
  * [ShortString](<../en/Shortstring.md> "Shortstring") \- Максимальная длина: 255 символов.
  * [String](<../en/String.md> "String") \- Максимальная длина: Short String или Ansistring (зависит от используемого параметра компилятора).
  * [PChar](<../en/PChar.md> "PChar") \- Указатель на строку с нулевым символом на конце без ограничения длины.
  * [AnsiString](<../en/Ansistring.md> "Ansistring") \- Нет ограничений по длине.
  * [PAnsiChar](<../en/Pansichar.md> "Pansichar") \- Указатель на строку с нулевым символом в конце без ограничения длины.



Смотрите обзор различных [типов символов и строк](<Character_and_string_types.md> "Character and string types/ru").   


## Типы символов с многобайтовой кодировкой

Кодировка с 2 или 4 байтами зависит от [используемой операционной] системы.  


  * [WideChar](</index.php?title=Widechar&action=edit&redlink=1> "Widechar \(page does not exist\)") \- Постоянная длина: 2 или 4 байта, представление: 1 символ.
  * [WideString](<../en/Widestring.md> "Widestring") \- Нет ограничений по длине.
  * [PWideChar](<../en/Pwidechar.md> "Pwidechar") \- Указатель на терминированную строку с нулевым символом на конце без ограничения длины.
  * [UnicodeChar](<../en/Unicodechar.md> "Unicodechar") \- Постоянная длина: 2 или 4 байта, представление: 1 символ.
  * [UnicodeString](<../en/Unicodestring.md> "Unicodestring") \- Нет ограничений по длине.
  * [PUnicodeChar](<../en/Punicodechar.md> "Punicodechar") \- Указатель на терминированную Unicode-строку с нулевым символом на конце без ограничения длины.



Смотрите обзор различных [типов символов и строк](<Character_and_string_types.md> "Character and string types/ru").   


# Вариантные типы

  * [Variant](<../en/Variant.md> "Variant")
  * [Olevariant](<../en/Olevariant.md> "Olevariant")



  


# Константы

  * Нетипизированные константы 
    * [Const](<../en/Const.md> "Const") \- Можно использовать только простые типы данных.
  * Типизированные константы 
    * [Const](<../en/Const.md> "Const") \- Можно использовать простые типы данных, а также записи и массивы.
  * Resource Strings 
    * [Resourcestring](</index.php?title=Resourcestring&action=edit&redlink=1> "Resourcestring \(page does not exist\)") \- Используется для локализации (доступно не во всех режимах компиляции).



  


# Структурные типы

  * [Array](<Array.md> "Array/ru") \- Размер массива зависит от типа и количества элементов, которые он содержит.
  * [Record](<Record.md> "Record/ru") \- Сочетание нескольких типов данных.
  * [Set](<Set.md> "Set/ru") \- Набор элементов порядкового типа; размер зависит от количества элементов в нем.



  


# Типы поддиапазонов

  * [Типы поддиапазонов](<../en/subrange_types.md> "subrange types") являются подмножеством базового типа.



  


# Указатель

  * [Указатель](<Pointer.md> "Pointer/ru") \- Размер зависит от типа процессора.



  


# Классы и объекты

  * [Object](<../en/Object.md> "Object") \- Разработано под Turbo Pascal 5.5 для DOS и предшественников класса.
  * [Class](<Class.md> "Class/ru") \- Разработано под Delphi 1.0 для Windows и наследников объекта.

---

_Source: [https://wiki.freepascal.org/Data_type/ru](https://web.archive.org/web/20240304152845/https://wiki.freepascal.org/Data_type/ru)_
