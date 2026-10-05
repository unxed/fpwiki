# Val

│ **[Deutsch (de)](</Val/de> "Val/de")** │  **[English (en)](<../en/Val.md> "Val")** │  **русский (ru)** │    
****

Процедура **Val** преобразовывает [строку](<String.md> "String/ru") **S** в её числовое представление. 

_S_ \- выражение строкового типа; оно должно быть последовательностью символов, образующей знаковое целое число. _V_ \- переменная целого или вещественного типа. _Code_ \- переменная типа [Integer](<Integer.md> "Integer/ru"). Если строка некорректна, позиция ошибочного символа указывается в _Code_ ; в противном случае _Code_ устанавливается равным 0. 

  
Объявление 
    
    
    procedure Val(S; var V; var Code: Integer);
    

  
См. также: 

  * [Str](<Str.md> "Str/ru")

---

_Source: [https://wiki.freepascal.org/Val/ru](https://web.archive.org/web/20230324145645/https://wiki.freepascal.org/Val/ru)_
