# Packed

│ **[English (en)](<../en/Packed.md>)** │  **русский (ru)** │

## Уплотнение структур

Зарезервированное слово **packed** сообщает компилятору по возможности использовать минимальное количество байтов памяти для составных типов данных. Без указания **packed** компилятор может вставить дополнительные неиспользуемые байты между элементами для выравнивания расположения элементов в памяти кратно машинному слову процессора, чтобы ускорить доступ. 

Кроме экономии памяти, упаковка элементов позволяет избежать несовместимости структур: 

  * при передаче составных типов между разными системами, например, между 32-х и 64-х битными
  * при обмене данными между программами, собранными разными версиями компиляторов
  * при подключении библиотек, написанных на других языках программирования



[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Предупреждение:** Следует учитывать, что некоторые базовые типы (Integer, Cardinal, Real, Pointer) на разных системах могут иметь разный размер. Поэтому для обмена данными с другими системами используйте только базовые типы с фиксированным размером.

Директива [{$PACKRECORDS n}](<https://www.freepascal.org/docs-html/prog/progsu60.html>) или [{$ALIGN n}](<https://www.freepascal.org/docs-html/prog/progsu1.html>) задает размер в байтах для выравнивания элементов структур, для которых не указан определитель **packed**. 

{$PACKRECORDS 1} эквивалентно указанию **packed**. 

Пример:  

    
    
      type TPArray = packed array [1..9] of LongInt;
      type TPClass = packed class  ... end;
      type TPObject = packed object ... end;
      type TPRecord = packed record ... end;
    

## Битовое уплотнение

Директива [{$BITPACKING ON|OFF}](<https://www.freepascal.org/docs-html/prog/progsu6.html>) указывает компилятору, что он должен уплотнять элементы до минимального размера в битах. Если значение ON, то компилятор будет уплотнять побитно структуры, имеющие ключевое слово **packed**. 

Ключевое слово **bitpacked** указывает, что элементы составного типа должны уплотнятся побитно независимо от значения директивы {$BITPACKING}. 

Пример:  

    
    
    type TSomeBitLevelStructure = bitpacked record
      SingleBit: Boolean;
      OneBit: 0..1;
      TwoBits: 0..3;
      FourBits: 0..15;
      EightBits: 0..255
    end;
    

## Смотри также

  * Процедуры [pack()](<https://www.freepascal.org/docs-html/rtl/system/pack.html>) и [unpack()](<https://www.freepascal.org/docs-html/rtl/system/unpack.html>)
  * [Упаковка и распаковка массивов](<https://www.freepascal.org/docs-html/ref/refse14.html#QQ2-38-60>)

---

_Source: [https://wiki.freepascal.org/Packed/ru](https://web.archive.org/web/20221130130607/https://wiki.freepascal.org/Packed/ru)_
