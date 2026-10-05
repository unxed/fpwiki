# String

│ **[Deutsch (de)](</String/de> "String/de")** │  **[English (en)](<../en/String.md> "String")** │  **[español (es)](</String/es> "String/es")** │  **[français (fr)](</String/fr> "String/fr")** │  **русский (ru)** │    
****

**String** является [типом данных](<Type.md> "Type/ru"), который может содержать [символы](<Character_and_string_types.md> "Character and string types/ru"). 

## Contents

  * 1 Использование
  * 2 Синонимы
  * 3 Строковые типы
  * 4 См. также



## Использование
    
    
    var
      s, str1, str2, str3, str4: string;
      c: char;
      n: integer;
    
    str1 := 'abc';     // присваивание
    str2 := '123';     // строка содержит символы 1, 2 и 3
    str3 := #13#10;    // cr lf (символы перевода строки и возврата каретки)
    str4 := 'это строка, заключенная в ''кавычки''';  // использование кавычек внутри строки
    s := str1 + str2;  // конкатенация (объединение) строк
    c := s[1];         // использование индекса массива для доступа к символу
    n := length( s );  // длина строки s
    

## Синонимы

**String** является синонимом типам [ShortString](<Character_and_string_types.md> "Character and string types/ru"), [AnsiString](<Character_and_string_types.md> "Character and string types/ru") или [UnicodeString (UTF16)](<Character_and_string_types.md> "Character and string types/ru") в зависимости от настроек компилятора. 

Если [директивы](<http://freepascal.org/docs-html/current/prog/progch1.html#x5-40001>) [компилятора](<../en/Compiler.md> "Compiler") {$H} или {$LongStrings} _включены_ ( {$H+} или {$LongStrings ON} ), то тип **String** соответствует типу **AnsiString** , в случае если директивы _выключены_ ( {$H-} или {$LongStrings OFF} ), то тип **String** соответствует типу **ShortString**. Какой тип строки является синонимом типа **String** также может быть установлено с помощью [опции командной строки](<http://www.freepascal.org/docs-html/user/userap1.html>) -Sh. Компилятор FPC также поддерживает директиву {$mode delphiunicode} для совместимости с Delphi UTF16. 

  
Примечание: директива компилятора {$mode} также устанавливает синоним типа **String**. После указания режима компилятора _FPC_ (по умолчанию), _ObjFPC_ , _MacPAS_ или _TP_ тип **String** будет эквивалентен типу **ShortString**. После указания режима компилятора _Delphi_ тип **String** будет эквивалентен типу **AnsiString**. Таким образом настройка параметра синонима для типа **String** должна быть выполнена после указания режима компилятора, чтобы предотвратить её переопределение: 
    
    
    {$H+}            // тип String является синонимом типа AnsiString
    {$mode ObjFPC}   // также влияет на синоним типа String - после данной директивы тип String будет синонимом типа ShortString
    {$H+}            // тип String снова является синонимом типа AnsiString
    

Переменная типа **String** , объявленная с указанием длины, всегда будет эквивалентна типу **ShortString** , независимо от настройки компилятора для синонима типа **String**. 
    
    
    {$H+}            // тип String является синонимом типа AnsiString
    var
       name : String[25]; // переменная name является ShortString, поскольку указание длины переопределяет настройку синонима
    

Учтите, что все типы _longstring_ являются управляемыми, в то время как тип **ShortString** таковым не является: у него нет счетчика ссылок. 

## Строковые типы

Различные строковые типы - **ShortString** , **AnsiString** , **WideString** и **UnicodeString** \- отличаются максимальной _длиной_ строки и её _содержимым_ : 

  * [ShortString](<Character_and_string_types.md> "Character and string types/ru") имеет _фиксированную максимальную длину_ , определяемую программистом (например _name : String[25];_), но при этом ограниченную 255 символами. Если длина переменной типа **ShortString** не указана явно, то длина устанавливается равной 255. В этом типе нет счетчика ссылок.
  * [AnsiString](<Character_and_string_types.md> "Character and string types/ru") имеет _переменную длину_ , ограниченную значением High(SizeInt) (которое зависит от платформы) и доступной памятью. В этом типе есть счетчик ссылок.
  * [WideString](<Character_and_string_types.md> "Character and string types/ru") имеет _переменную длину_ , как и тип **AnsiString** , но состоит из символов [WideChar](</index.php?title=WideChar/ru&action=edit&redlink=1> "WideChar/ru \(page does not exist\)") вместо символов [Char](<Char.md> "Char/ru"). Данный тип совместим со строковым типом BWSTR и в нем нет счетчика ссылок.
  * [UnicodeString](<Character_and_string_types.md> "Character and string types/ru") похож на тип [WideString](<Character_and_string_types.md> "Character and string types/ru"), но тип **UnicodeString** является управляемым типом и содержит счетчик ссылок, в то время как **WideString** является совместимым с типом BWSTR, который совместим с COM и не содержит счетчик ссылок.  
  




Note that BWSTR types rely on COM marshaling or - when used alone - copy semantics instead of reference counting. In a COM context they are governed by the COM marshaling subsystem if available. (i.e. Windows) 

## См. также

  * [Символьные и строковые типы](<Character_and_string_types.md> "Character and string types/ru") \- подробная справка, с описанием размещения символов в памяти и параметров доступа.

Типы данных   
---  
Простые типы  | [Boolean](<Boolean.md> "Boolean/ru") | [Byte](<Byte.md> "Byte/ru") | [Cardinal](<Cardinal.md> "Cardinal/ru") | [Char](<Char.md> "Char/ru") | [Currency](<Currency.md> "Currency/ru") | [Extended](<Extended.md> "Extended/ru") | [Int64](<Int64.md> "Int64/ru") | [Integer](<Integer.md> "Integer/ru") | [Longint](<Longint.md> "Longint/ru") | [Pointer](<Pointer.md> "Pointer/ru") | [Real](<Real.md> "Real/ru") | [Shortint](<Shortint.md> "Shortint/ru") | [Smallint](<Smallint.md> "Smallint/ru") | [Word](<Word.md> "Word/ru")  
Сложные типы  | [Array](<Array.md> "Array/ru") | [Class](<Class.md> "Class/ru") | [Record](<Record.md> "Record/ru") | [Set](<Set.md> "Set/ru") | String | [Shortstring](</index.php?title=Shortstring/ru&action=edit&redlink=1> "Shortstring/ru \(page does not exist\)")  
  
  
****

---

_Source: [https://wiki.freepascal.org/String/ru](https://web.archive.org/web/20250122172320/https://wiki.freepascal.org/String/ru)_
