# Currency

│ **[English (en)](<../en/Currency.md>)** │  **русский (ru)** │

Тип `Currency` является вещественным [типом данных](<../en/Data_type.md> "Data type") с фиксированной точкой (4 десятичных знака после точки), представляющий значения в диапазоне от -922337203685477.5808 до 922337203685477.5807. Тип данных [data type](<../en/Data_type.md> "Data type") используется с целью получения точного результата при арифметических вычислениях. 

Вещественные значения обычно хранятся во внутренней [двоичной системе](<../en/Binary_numeral_system.md> "Binary numeral system") и вычисления с ними выполняются в центральном процессоре с использованием двоичной арифметики. Поскольку людям хочется вводить и выводить числа в десятичной системе счисления, они должны быть преобразованы из десятичной системы во внутреннее двоичное представление. Из-за преобразований в двоичные числа (и обратно) и выполнения арифметических действий над ними в двоичной системе, результаты арифметических вычислений с вещественными числами могут отличаться от вычислений с десятичными числами. Во многих приложениях это не критично, но для _финансовых приложений_ необходимо соответствие вычислений для десятичных чисел. Тип данных `currency` разработан для того, чтобы результаты арифметических операций с вещественными числами соответствовали результатам арифметических операций с десятичными числами. 

## См.также

  * [function](<../en/Function.md> "Function") [`CurrToStr`](<https://www.freepascal.org/docs-html/rtl/sysutils/currtostr.html>)
  * function [`FormatCurr`](<https://www.freepascal.org/docs-html/rtl/sysutils/formatcurr.html>)
  * function [`StrToCurr`](<https://www.freepascal.org/docs-html/rtl/sysutils/strtocurr.html>)



  


Типы данных   
---  
Простые типы  | [Boolean](<Boolean.md> "Boolean/ru") | [Byte](<Byte.md> "Byte/ru") | [Cardinal](<Cardinal.md> "Cardinal/ru") | [Char](<Char.md> "Char/ru") | Currency | [Extended](<Extended.md> "Extended/ru") | [Int64](<Int64.md> "Int64/ru") | [Integer](<Integer.md> "Integer/ru") | [Longint](<Longint.md> "Longint/ru") | [Pointer](<Pointer.md> "Pointer/ru") | [Real](<Real.md> "Real/ru") | [Shortint](<Shortint.md> "Shortint/ru") | [Smallint](<Smallint.md> "Smallint/ru") | [Word](<Word.md> "Word/ru")  
Сложные типы  | [Array](<Array.md> "Array/ru") | [Class](<Class.md> "Class/ru") | [Record](<Record.md> "Record/ru") | [Set](<Set.md> "Set/ru") | [String](<String.md> "String/ru") | [Shortstring](</index.php?title=Shortstring/ru&action=edit&redlink=1> "Shortstring/ru \(page does not exist\)")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Currency/ru](https://web.archive.org/web/20250211185836/https://wiki.freepascal.org/Currency/ru)_
