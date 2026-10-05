# Ord

│ **[Deutsch (de)](</Ord/de> "Ord/de")** │  **[English (en)](<../en/Ord.md> "Ord")** │  **[français (fr)](</Ord/fr> "Ord/fr")** │  **русский (ru)** │    
****

Функция **Ord** возвращает индекс (порядковый номер, начиная с 0) элемента перечисления. 

Исторически сложилось, что функция **Ord** использовалась для приведения типа **Char** к типу **Byte** для получения [ASCII](<ASCII.md> "ASCII/ru")-кода [символа](<Char.md> "Char/ru") строки. 
    
    
    function Ord(X: TOrdinal): LongInt;
    
    function Ord(c: Char): Byte;
    

### Пример использования
    
    
    Program Example45;
    
    { Программа, демонстрирующая работу функций Ord(), Pred(), Succ(). }
    
    type
      TEnum = (Zero, One, Two, Three, Four);
    
    var
      X: LongInt;
      Y: TEnum;
    
    begin
      X := 125;
      Writeln(Ord(X));  { выводит 125 }
    
      X := Pred(X);
      Writeln(Ord(X));  { выводит 124 }
    
      Y := One;
      Writeln(Ord(y));  { выводит 1 }
    
      Y := Succ(Y);
      Writeln(Ord(Y));  { выводит 2}
    end.
    

### См. также:

  * [Chr](<Chr.md> "Chr/ru") \- преобразует байт в символ ASCII
  * [Succ](</index.php?title=Succ/ru&action=edit&redlink=1> "Succ/ru \(page does not exist\)") \- возвращает значение следующего элемента перечисления
  * [Pred](</index.php?title=Pred/ru&action=edit&redlink=1> "Pred/ru \(page does not exist\)") \- возвращает предыдущий элемент перечисления
  * [High](</index.php?title=High/ru&action=edit&redlink=1> "High/ru \(page does not exist\)") \- возвращает верхний (максимальный) индекс массива или перечисления
  * [Low](</index.php?title=Low/ru&action=edit&redlink=1> "Low/ru \(page does not exist\)") \- возвращает нижний (минимальный) индекс массива или перечисления

---

_Source: [https://wiki.freepascal.org/Ord/ru](https://web.archive.org/web/20250219114215/https://wiki.freepascal.org/Ord/ru)_
