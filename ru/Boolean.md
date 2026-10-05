# Boolean

│ [**Deutsch (de)**](</Boolean/de> "Boolean/de") │  [**English (en)**](<../en/Boolean.md> "Boolean") │  [**suomi (fi)**](</Boolean/fi> "Boolean/fi") │  [**français (fr)**](</Boolean/fr> "Boolean/fr") │  **русский (ru)** │  [**中文（中国大陆）‎ (zh_CN)**](</Boolean/zh_CN> "Boolean/zh CN") │    
****

## Обзор

**Boolean** \- это логический тип данных. Данные типа **boolean** содержат только два значения, либо [true](<True.md> "True/ru"), либо [false](<False.md> "False/ru"). Переменная типа Boolean занимает 1 байт. 

Значение **true** может быть напрямую присвоено булевой переменной или по результату сравнения или проверки успешного выполнения ("true"). Аналогично, значение **false** может быть напрямую присвоено переменной или по результату сравнения или проверки не успешного выполнения ("false"). Процедуры Write() и Writeln() выведут строку с соответствующим значением булевой переменной (либо "TRUE", либо "FALSE"). Булева переменная может использоваться в качестве выражения в условном операторе **if**. Процедура WriteStr() может использоваться для сохранения строки, представляющей собой значение булевой переменной в строковой переменной. 
    
    
    var
        tooLarge   : Boolean = false;
        boolString : ShortString; 
    begin
        Writeln(tooLarge);
        tooLarge := (0 = 0);
        Writeln(tooLarge);
        tooLarge := (3 > 5);
        Writeln(tooLarge);
        tooLarge := true;
        Writeln(tooLarge);
        if tooLarge then
           Writeln('tooLarge is true')
        else
           Writeln('tooLarge is false');
        WriteStr(boolString,tooLarge);
        Writeln(boolString);
    end
    

Будет выведено:  
**FALSE**  
**TRUE**  
**FALSE**  
**TRUE**  
**tooLarge is true**  
**TRUE**  


## См. также

  * [логические выражения](</index.php?title=Boolean_Expressions/ru&action=edit&redlink=1> "Boolean Expressions/ru \(page does not exist\)")

Типы данных   
---  
Простые типы  | Boolean | [Byte](<Byte.md> "Byte/ru") | [Cardinal](<Cardinal.md> "Cardinal/ru") | [Char](<Char.md> "Char/ru") | [Currency](<Currency.md> "Currency/ru") | [Extended](<Extended.md> "Extended/ru") | [Int64](<Int64.md> "Int64/ru") | [Integer](<Integer.md> "Integer/ru") | [Longint](<Longint.md> "Longint/ru") | [Pointer](<Pointer.md> "Pointer/ru") | [Real](<Real.md> "Real/ru") | [Shortint](<Shortint.md> "Shortint/ru") | [Smallint](<Smallint.md> "Smallint/ru") | [Word](<Word.md> "Word/ru")  
Сложные типы  | [Array](<Array.md> "Array/ru") | [Class](<Class.md> "Class/ru") | [Record](<Record.md> "Record/ru") | [Set](<Set.md> "Set/ru") | [String](<String.md> "String/ru") | [Shortstring](</index.php?title=Shortstring/ru&action=edit&redlink=1> "Shortstring/ru \(page does not exist\)")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Boolean/ru](https://web.archive.org/web/20231002022546/https://wiki.freepascal.org/Boolean/ru)_
