# Cardinal

│ **[English (en)](<../en/Cardinal.md>)** │  **русский (ru)** │

**Cardinal** \- это целочисленный тип, определенный в качестве псевдонима типа **DWord** для 32-битных платформ. Также как и **DWord** (двойное слово) этот тип данных является 32-битным и интерпретируется как беззнаковое целое. Минимальное значение этого типа 0x0000000, а максимальное - [0xFFFFFFFF](<Hexadecimal.md> "Hexadecimal/ru")) (4,294,967,295). 

В системах на базе процессоров x86 тип **Cardinal** часто используется для хранения адресов памяти, как [указатель](<Pointer.md> "Pointer/ru"): 
    
    
      var
        anAddress: Cardinal;
        anObject: TObject;
      begin
        anAddress := Cardinal(Self);
        with TObject(anAddress) do
        begin
          // anAddress is casted as a TObject.
        end;
      end;
    

Однако, из-за того, что в 32- и 64-битных системах порядок следования байт в памяти _от младшего к старшему_ , использование типа **Cardinal** больше не рекомендуется для операций (арифметики) с адресами памяти (указателями). Вместо него рекомендуется использовать типы **NativeInt** или **NativeUInt**. Эти типы соответствуют размерам регистров центрального процессора, которые могут использоваться для работы с адресами памяти, и всегда будут иметь правильный размер. Например, в 64-битных операционных системах тип **NativeUInt** будет аналогичен типу **UInt64** или **QuadWord** , а в 32-битных **NativeUInt** будет аналогичен типу **DWord** или **Cardinal**. 

Типы данных   
---  
Простые типы  | [Boolean](<Boolean.md> "Boolean/ru") | [Byte](<Byte.md> "Byte/ru") | Cardinal | [Char](<Char.md> "Char/ru") | [Currency](<Currency.md> "Currency/ru") | [Extended](<Extended.md> "Extended/ru") | [Int64](<Int64.md> "Int64/ru") | [Integer](<Integer.md> "Integer/ru") | [Longint](<Longint.md> "Longint/ru") | [Pointer](<Pointer.md> "Pointer/ru") | [Real](<Real.md> "Real/ru") | [Shortint](<Shortint.md> "Shortint/ru") | [Smallint](<Smallint.md> "Smallint/ru") | [Word](<Word.md> "Word/ru")  
Сложные типы  | [Array](<Array.md> "Array/ru") | [Class](<Class.md> "Class/ru") | [Record](<Record.md> "Record/ru") | [Set](<Set.md> "Set/ru") | [String](<String.md> "String/ru") | [Shortstring](</index.php?title=Shortstring/ru&action=edit&redlink=1> "Shortstring/ru \(page does not exist\)")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Cardinal/ru](https://web.archive.org/web/20250119193857/https://wiki.freepascal.org/Cardinal/ru)_
