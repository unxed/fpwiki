# Div

│ **[English (en)](<../en/Div.md>)** │  **русский (ru)** │

**Div** является операцией деления, при которой дробная часть (остаток от деления) отбрасывается. Выражение (a **div** b) возвращает целую часть от деления двух целых чисел. Данная операция является противоположным действием выражения (a / b), которые возвращает результат в виде [вещественного](<Real.md> "Real/ru") числа. Обе части выражения, использующиеся в операторе **div** должны иметь целые типы. Использование операндов [вещественного](<Real.md> "Real/ru") типа в операторе **div** приведет к ошибке в процессе компиляции: "Error: Operator is not overloaded:". Для получения целочисленного результата при использовании оператора '/' и операндов [вещественного](<Real.md> "Real/ru") типа необходимо использовать функции [Trunc](<Trunc.md> "Trunc/ru") или [Round](<Round.md> "Round/ru"). 

Пример: 
    
    
    var 
      i : ShortInt = 16; 
      j : ShortInt = 3;
      q : QWord = 1000;
      r : QWord = 300;
    
    begin
      WriteLn(i div j);
      WriteLn(i / j);
      WriteLn(q div r);
      Writeln(q / r)
    end.
    

**Output:**  
5  
5.3333333333333330E+000  
3  
3.3333333333333335E+000  


## См. также

  * [Mod](</index.php?title=Mod/ru&action=edit&redlink=1> "Mod/ru \(page does not exist\)")
  * [Trunc](<Trunc.md> "Trunc/ru")
  * [Round](<Round.md> "Round/ru")

---

_Source: [https://wiki.freepascal.org/Div/ru](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/Div/ru)_
