# Round

│ **[Deutsch (de)](</Round/de> "Round/de")** │  **[English (en)](<../en/Round.md> "Round")** │  **[Esperanto (eo)](</Round/eo> "Round/eo")** │  **[suomi (fi)](</Round/fi> "Round/fi")** │  **русский (ru)** │    
****

[Модуль System](</index.php?title=System_unit/ru&action=edit&redlink=1> "System unit/ru \(page does not exist\)"), входящий в состав [RTL](<RTL.md> "RTL/ru"), содержит функцию **Round** , которая округляет значение типа [real](<Real.md> "Real/ru") до значения типа [integer](<Integer.md> "Integer/ru"). Её входным параметром является выражение вещественного типа, и **Round** возвращает значение типа [longint](<Longint.md> "Longint/ru"), округленное до ближайшего целого числа. Если входное значение находится точно посередине между двух целых чисел - N.5 - то используется "банковское округление", в результате которого значение округляется до ближайшего четного числа. 

## Contents

  * 1 Объявление
  * 2 Пример использования
    * 2.1 Вывод
  * 3 См. также



## Объявление
    
    
     function Round(X: Real): Longint;
    

## Пример использования
    
    
    begin
       WriteLn( Round(8.7) );
       WriteLn( Round(8.3) );
       // примеры "банковского округления" - .5 округляется до ближайшего четного числа
       WriteLn( Round(2.5) );
       WriteLn( Round(3.5) );
    end.
    

### Вывод

9  
8  
2  
4  


## См. также

  * [Int](<Int.md> "Int/ru")
  * [Trunc](<Trunc.md> "Trunc/ru")
  * [Div](<Div.md> "Div/ru")

---

_Source: [https://wiki.freepascal.org/Round/ru](https://web.archive.org/web/20230329110908/https://wiki.freepascal.org/Round/ru)_
