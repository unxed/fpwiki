# Basic Pascal Tutorial/Chapter 1/Variables and Data Types

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_1/Variables_and_Data_Types/bg> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types/bg")** │  **[Deutsch (de)](</Basic_Pascal_Tutorial/Chapter_1/Variables_and_Data_Types/de> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types/de")** │  **[English (en)](<../../../en/Basic_Pascal_Tutorial/Chapter_1/Variables_and_Data_Types.md> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types")** │  **[español (es)](</Basic_Pascal_Tutorial/Chapter_1/Variables_and_Data_Types/es> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types/es")** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_1/Variables_and_Data_Types/fr> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_1/Variables_and_Data_Types/ja> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types/ja")** │  **[한국어 (ko)](</Basic_Pascal_Tutorial/Chapter_1/Variables_and_Data_Types/ko> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types/ko")** │  **русский (ru)** │  **[中文（中国大陆）‎ (zh_CN)](</Basic_Pascal_Tutorial/Chapter_1/Variables_and_Data_Types/zh_CN> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types/zh CN")** │    
****

[ ◄ ](<Constants.md> "Basic Pascal Tutorial/Chapter 1/Constants/ru") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Assignment_and_Operations.md> "Basic Pascal Tutorial/Chapter 1/Assignment and Operations/ru")  
---|---|---  
  
Переменные и типы данных

1D - Variables and Data Types (author: Tao Yue, state: _changed_) 

  
Переменные похожи на константы, но их значения могут быть изменены во время выполнения программы. В Pascal переменные должны быть сначала объявлены, прежде чем они могуть быть использованы: 
    
    
    var
      IdentifierList1 : DataType1;
      IdentifierList2 : DataType2;
      IdentifierList3 : DataType3;
      ...
    

`IdentifierList` \- несколько идентификаторов, разделённых запятыми (`,`). Все идентификаторы в списке объявляются как имеющие одинаковый тип. 

Основные типы данных Pascal включают: 

  * integer
  * real
  * char
  * boolean



Стандартный Pascal не предусматривает строкового типа данных, но большинство современных компиляторов имеют его. Опытные программисты на Pascal также используют указатели для динамического распредедения памяти, объекты для объектно-ориентированного прграммирования и многие другие, но эти позволят вам начать. 

Дополнительная информация о типах данных Pascal: 

  * Тип данных **integer** может содержать целые числа от `-32768` до `32767`. Это знаковый диапазон, который может быть сохранён в 16-битном слове, и является наследием эпохи, когда были распространены 16-битные процессоры. Для обратной совместимости 32-битный знаковый integer является longint и может содержать гораздо больший диапазон значений.
  * Тип данных **real** имеет диапазон от `3.4x10-38` до `3.4x1038`, в дополнение к такому-же диапазону в отрицательной области. Вещественные значения хранятся внутри компьютера сходно с научной нотацией - с мантиссой и экспонентой, с некоторыми усложнениями. В Pascal вы можете выразить вещественое значение в вашем коде либо в нотации с фиксированной точкой, либо в научной нотации с символом `E`, отделяющим мантиссу от экспоненты. Таким образом, `452.13` и `4.5213e2` эквивалентны.
  * Тип данных **char** содержит символы. Обязательно заключайте их в апострофы, например, так: `'a', 'B', '+'`. Стандартный Pascal использует 8-битные символы, а не 16-битные, поэтому Unicode, который используется для представления наборов всех мировых языков в одной системе UNIfied CODE, не поддерживается.
  * **WideChar** \- это двухбайтный символ (элемент DBCS: Double Byte Character Set) и может содержать юникодный символ.
  * Free Pascal поддерживает такую-же реализацию типа **PChar** , как Delphi. PChar определён как указатель на тип Char, но позволяет дополнительные операции. Тип PChar может быть лучше понят как Pascal-эквивалент нуль-терминированной сроки в стиле языка C, т.е. переменная типа PChar является указателем, который указывает на массив типа Char, завершающийся нулевым символом (#0). Free Pascal поддерживает инициализацию типизированных констант PChar, или прямое присваивание. Например, следующие фрагменты кода эквивалентны:


    
    
    program one;  
    var P : PChar;  
    begin  
      P := 'This is a null-terminated string.';  
      WriteLn (P);  
    end.
    
    
    
    program two;  
    const P : PChar = 'This is a null-terminated string.';  
    begin  
      WriteLn (P);  
    end.
    

  * Free Pascal поддерживает тип **String** , как он определён в Turbo Pascal: последовательность символов с необязательным указанием размера. Он также поддерживает **ansistring** (с неограниченной длиной), как в Delphi. Возможные объявления:


    
    
    variable_name : string;                    // если не указана длина, по умолчанию принимается 255
    variable_name : string[length];            // где:  1 < length <= 255
    

  * Предопределённый тип **ShortString** определён как строка длиной 255.
  * **AnsiString** \- строки, которые не имеют ограничения по длине. Это строки с подсчётом ссылок и гарантированно завершающиеся нулевым символом. Внутренне ansistring рассматривается как указатель: реальное содержимое строки хранится в куче, в которой выделяется столько памяти, сколько необходимо для хранения содержимого строки.
  * Тип **Widestring** (используется для представления строк юникодных символов) реализован во многом подобно **ansistring** : с подсчётом ссылок, нуль-терминированный массив, только реализованный как массив `WideChar` вместо обычных `Char`.
  * Тип данных **boolean** может иметь только два значения: **TRUE** (истина) и **FALSE** (ложь).



Пример объявления нескольких переменных: 
    
    
    var
      age, year, grade : integer;
      circumference : real;
      LetterGrade : char;
      DidYouFail : Boolean;
    

**Из документации FPC:**

**Целые типы** Тип | Диапазон | Байтов   
---|---|---  
Byte | 0 .. 255 | 1   
Shortint | -128 .. 127 | 1   
Smallint | -32768 .. 32767 | 2   
Word | 0 .. 65535 | 2   
Integer | smallint или longint | 2 or 4   
Cardinal | longword | 4   
Longint | -2147483648 .. 2147483647 | 4   
Longword | 0..4294967295 | 4   
Int64 | -9223372036854775808 .. 9223372036854775807 | 8   
QWord | 0 .. 18446744073709551615 | 8   
  
Free Pascal осуществлят автоматическое преобразование типов в выражениях, где используются различные виды целых типов. 

  


**Вещественные типы** Тип | Диапазон | Значащих цифр | Байтов   
---|---|---|---  
Real | зависит от платформы | ??? | 4 or 8   
Single | 1.5E-45 .. 3.4E38 | 7-8 | 4   
Double | 5.0E-324 .. 1.7E308 | 15-16 | 8   
Extended* | 1.9E-4932 .. 1.1E4932 | 19-20 | 10   
Comp | -2E64+1 .. 2E63-1 | 19-20 | 8   
Currency | -922337203685477.5808 | 922337203685477.5807 | 8   
  
`*` Обратите внимание, что для 64-разрядных версий Windows и целей, не относящихся к Intel, Extended является псевдонимом для Double. 

**Логические типы** Тип | Байтов | Ord(True)   
---|---|---  
Boolean | 1 | 1   
ByteBool | 1 | Любое ненулевое значение   
WordBool | 2 | Любое ненулевое значение   
LongBool | 4 | Любое ненулевое значение   
[ ◄ ](<Constants.md> "Basic Pascal Tutorial/Chapter 1/Constants/ru") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Assignment_and_Operations.md> "Basic Pascal Tutorial/Chapter 1/Assignment and Operations/ru")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Variables_and_Data_Types/ru](https://web.archive.org/web/20231004140325/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Variables_and_Data_Types/ru)_
