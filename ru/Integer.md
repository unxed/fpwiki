# Integer

│ **[Deutsch (de)](</Integer/de> "Integer/de")** │  **[English (en)](<../en/Integer.md> "Integer")** │  **[suomi (fi)](</Integer/fi> "Integer/fi")** │  **[français (fr)](</Integer/fr> "Integer/fr")** │  **[italiano (it)](</Integer/it> "Integer/it")** │  **русский (ru)** │    
****

**Integer** является [стандартным типом данных](<Standard_type.md> "Standard type/ru") языка программирования [Pascal](<../en/Pascal.md> "Pascal"). Он используется для определения целых чисел, в отличие от типа [real](<Real.md> "Real/ru"), применяющегося для представления вещественных чисел, которые могут содержать десятичную точку и, возможно, экспоненту. 

Размер переменных типа **integer** зависит от размера машинных слов на целевой платформе, для которой компилятор генерирует код ([32 bit](<../en/32_bit.md> "32 bit") или [64 bit](<../en/64_bit.md> "64 bit")) и типа [компилятора](<../en/Compiler.md> "Compiler") (16-битный, 32-битный или 64-битный). Типичные размеры типа **integer**

  * 16 бит (2 байта)
  * 32 бита (4 байта) или
  * 64 бита (8 байта)



На текущий момент FPC использует 32 бита (4 байта) для типа **integer** , независимо от того, является ли архитектура машины 32-битной или 64-битной. Это приведет к тому, что в коде ожидается одинаковый размер типа **integer** и указателя на него, и на машинах с 64-битной архитектурой, использующих 64-битные указатели, данный код работать не будет. Для того, что бы вы могли писать переносимый код, в модуле System FPC введены типы [PtrInt](</index.php?title=PtrInt/ru&action=edit&redlink=1> "PtrInt/ru \(page does not exist\)") и [PtrUInt](</index.php?title=PtrUInt/ru&action=edit&redlink=1> "PtrUInt/ru \(page does not exist\)"), являющиеся, соответственно, знаковым и беззнаковым типами данных указателей, имеющие одинаковые размеры с типом **integer**. 

В старых версиях компиляторов тип **integer** был 16-битным и представлял значения от -215 до 215-1 или -32 768 .. 32 767. Аналогичный тип данных [word](<Word.md> "Word/ru") иногда использовался для определения беззнакового целого типа (0..65 535). В таких случаях, где компилятор использовал 16-битный целый тип, 32-битные целые числа обычно выражались с помощью [типов данных](<Data_type.md> "Data type/ru") [long](</index.php?title=Long/ru&action=edit&redlink=1> "Long/ru \(page does not exist\)") или [longint](<Longint.md> "Longint/ru"). 

Для машин с архитектурой x86 тип **integer** обычно определяется как 32-битный и включает значения в диапазоне от -231 до 231-1 или -2 147 483 648 .. 2 147 483 647. Последнее значение также определено в качестве константы [MAXINT](</index.php?title=maxint/ru&action=edit&redlink=1> "maxint/ru \(page does not exist\)"). Беззнаковый 32-битный целый тип [cardinal](<Cardinal.md> "Cardinal/ru") имеет значения в диапазоне от 0 до 232-1 или 0 .. 4 294 967 295. 

В настоящий момент тип **integer** зависит только от режима компиляции (`$mode`), поэтому на современных 64-битных процессорах тип **integer** также 16-битный в режимах [TP](<Mode_TP.md> "Mode TP/ru") или [FPC](<Mode_FPC.md> "Mode FPC/ru") либо 32-битный в режимах [ObjFPC](</index.php?title=Mode_ObjFPC/ru&action=edit&redlink=1> "Mode ObjFPC/ru \(page does not exist\)") или [Delphi](<Mode_Delphi.md> "Mode Delphi/ru"). 

Для 64-битных вычислений FPC поддерживает 64-битный тип [Int64](<Int64.md> "Int64/ru"), который может принимать значения от -263 до 263-1 или -9 223 372 036 854 775 808 ... 9 223 372 036 854 775 807. 

Типы данных   
---  
Простые типы  | [Boolean](<Boolean.md> "Boolean/ru") | [Byte](<Byte.md> "Byte/ru") | [Cardinal](<Cardinal.md> "Cardinal/ru") | [Char](<Char.md> "Char/ru") | [Currency](<Currency.md> "Currency/ru") | [Extended](<Extended.md> "Extended/ru") | [Int64](<Int64.md> "Int64/ru") | Integer | [Longint](<Longint.md> "Longint/ru") | [Pointer](<Pointer.md> "Pointer/ru") | [Real](<Real.md> "Real/ru") | [Shortint](<Shortint.md> "Shortint/ru") | [Smallint](<Smallint.md> "Smallint/ru") | [Word](<Word.md> "Word/ru")  
Сложные типы  | [Array](<Array.md> "Array/ru") | [Class](<Class.md> "Class/ru") | [Record](<Record.md> "Record/ru") | [Set](<Set.md> "Set/ru") | [String](<String.md> "String/ru") | [Shortstring](</index.php?title=Shortstring/ru&action=edit&redlink=1> "Shortstring/ru \(page does not exist\)")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Integer/ru](https://web.archive.org/web/20250114071648/https://wiki.freepascal.org/Integer/ru)_
