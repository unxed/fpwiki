# Write

│ **[English (en)](<../en/Write.md>)** │  **русский (ru)** │

**Write** является [ключевым словом](<Keyword.md> "Keyword/ru"), которое указывает, что некоторые данные необходимо вывести на экран (по умолчанию) или в [файл](</index.php?title=File/ru&action=edit&redlink=1> "File/ru \(page does not exist\)"). Например: 
    
    
    var
      a, b: Integer;
    begin
      a := 42;
      b := 23;
      write('a=', a, ' and b=', b);
    end.
    

В результате на экран будет выведено : _a=42 and b=23_

Параметры в процедуре **Write** должны быть разделены [запятой (',')](<Comma.md> "Comma/ru"). 

[Двоеточие (':')](<Colon.md> "Colon/ru") используется для [форматированного вывода](</index.php?title=Formatting_output/ru&action=edit&redlink=1> "Formatting output/ru \(page does not exist\)"): ` write(x:num); ` Num указывает общее количество используемых цифр. Если для значения, содержащегося в переменной **x** необходимо больше разрядов, num игнорируется. Форматированный вывод переменных вещественного типа: ` write(x:num1:num2); ` **X** \- переменная вещественного типа, num1 - общее количество используемых цифр (включая знак и "запятую"), а num2 - количество цифр после запятой. 

  
**WriteLn** ведет себя точно также, как Write, за исключением того, что оставляет символ(-ы) [конца строки](<End_of_Line.md> "End of Line/ru") после текста.

---

_Source: [https://wiki.freepascal.org/Write/ru](https://web.archive.org/web/20250121211606/https://wiki.freepascal.org/Write/ru)_
