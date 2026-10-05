# Procedure

│ **[Deutsch (de)](</Procedure/de> "Procedure/de")** │  **[English (en)](<../en/Procedure.md> "Procedure")** │  **[suomi (fi)](</Procedure/fi> "Procedure/fi")** │  **[français (fr)](</Procedure/fr> "Procedure/fr")** │  **[italiano (it)](</Procedure/it> "Procedure/it")** │  **русский (ru)** │    
****

## Contents

  * 1 Обзор
  * 2 Параметры процедуры
  * 3 Пример
  * 4 См. также



## Обзор

[Ключевое слово](<Keyword.md> "Keyword/ru") **procedure** предназначено для объявления [подпрограммы](<Routine.md> "Routine/ru"), которая может быть вызвана 

  * из [модуля](<Unit.md> "Unit/ru"), в котором она объявлена
  * из внешнего модуля, если она объявлена в секции [interface](</index.php?title=Interface/ru&action=edit&redlink=1> "Interface/ru \(page does not exist\)") модуля,
  * или из [программы](<Program.md> "Program/ru")



Если подпрограмма объявлена как процедура, то она не возвращает значение. Процедура, которая возвращает значение, называется _[функцией](<Function.md> "Function/ru")_. 

Процедура, являющаяся частью объекта, называется [методом](<Method.md> "Method/ru"). Функция, являющаяся частью объекта, также называется [методом](<Method.md> "Method/ru"), если с её помощью не может быть присвоено значение из внешней функции и [свойством](</Property/ru> "Property/ru"), если с её помощью может быть присвоено значение из внешней функции. 

## Параметры процедуры

  * Передаваемые по значению
  * [Параметры-переменные](</index.php?title=Variable_parameter/ru&action=edit&redlink=1> "Variable parameter/ru \(page does not exist\)") (передаваемые по ссылке)
  * Выходные параметры (**Out**)
  * Константные параметры
  * [Параметры по умолчанию](<Default_parameter.md> "Default parameter/ru")



## Пример

Пример использования [параметров-переменных](</index.php?title=Variable_parameter/ru&action=edit&redlink=1> "Variable parameter/ru \(page does not exist\)"): 
    
    
     // процедура обмена значений двух переменных (параметры передаются по ссылке)
     procedure swap(var c1,c2:char);
     var c:char; 
     begin
       c:=c1;
       c1:=c2;
       c2:=c;
     end;
    
     var s:string;
    
     begin
       s:='pit'; 
       swap(s[1],s[3]);
       writeln (s); // результатом будет 'tip'
     end.
    

## См. также

  * [Procedure statements](<http://www.freepascal.org/docs-html/ref/refsu49.html>)
  * [Процедурные типы](<http://www.freepascal.org/docs-html/ref/refse17.html>)

---

_Source: [https://wiki.freepascal.org/Procedure/ru](https://web.archive.org/web/20250122175557/https://wiki.freepascal.org/Procedure/ru)_
