# Real

│ **[Deutsch (de)](</Real/de> "Real/de")** │  **[English (en)](<../en/Real.md> "Real")** │  **[français (fr)](</Real/fr> "Real/fr")** │  **русский (ru)** │    
****

Тип **real** является [стандартным типом](<Standard_type.md> "Standard type/ru") данных языка программирования [Pascal](<../en/Pascal.md> "Pascal"). Он применяется для представления вещественных чисел, которые могут состоять из десятичной точки и экспоненты, в отличие от типа [Integer](<Integer.md> "Integer/ru"), который используется для представления целых чисел. 

Внутреннее представление типа **real** (т.е. количество байт и порядок их размещения в оперативной памяти), диапазон значений и точность зависят от целевой платформы. 

Цитата из Руководства программиста по FreePascal (параграф 8.2.5 Типы с плавающей точкой): 
    
    
    Contrary to Turbo Pascal, where the real type had a special internal format, under Free Pascal the
    real type simply maps to one of the other real types. It maps to the double type on processors
    which support floating point operations, while it maps to the single type on processors which do
    not support floating point operations in hardware.
    

Таким образом, наиболее общим представлением является **double** (8 байт, 64 бита), состоящим из 1 знакового бита, 11 бит экспоненты и 52 бит мантиссы. Диапазон значений чисел получается от самого большого положительного до самого маленького отрицательного числа: ±1.7976931348623157×10308. Наименьшее положительное ненулевое и наибольшее отрицательное ненулевое числа равны, соответственно: ±2.2250738585072020×10−308. 52 бита мантиссы обеспечивают точность в 15 десятичных знаков после точки. 

Более подробное описание чисел с плавающей точкой дается на странице wikipedia - [floating point numbers](<http://en.wikipedia.org/wiki/Floating_point>) и [IEEE 754](<http://en.wikipedia.org/wiki/IEEE_754>) . 

Типы данных   
---  
Простые типы  | [Boolean](<Boolean.md> "Boolean/ru") | [Byte](<Byte.md> "Byte/ru") | [Cardinal](<Cardinal.md> "Cardinal/ru") | [Char](<Char.md> "Char/ru") | [Currency](<Currency.md> "Currency/ru") | [Extended](<Extended.md> "Extended/ru") | [Int64](<Int64.md> "Int64/ru") | [Integer](<Integer.md> "Integer/ru") | [Longint](<Longint.md> "Longint/ru") | [Pointer](<Pointer.md> "Pointer/ru") | Real | [Shortint](<Shortint.md> "Shortint/ru") | [Smallint](<Smallint.md> "Smallint/ru") | [Word](<Word.md> "Word/ru")  
Сложные типы  | [Array](<Array.md> "Array/ru") | [Class](<Class.md> "Class/ru") | [Record](<Record.md> "Record/ru") | [Set](<Set.md> "Set/ru") | [String](<String.md> "String/ru") | [Shortstring](</index.php?title=Shortstring/ru&action=edit&redlink=1> "Shortstring/ru \(page does not exist\)")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Real/ru](https://web.archive.org/web/20250122174230/https://wiki.freepascal.org/Real/ru)_
