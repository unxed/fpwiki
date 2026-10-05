# Xor

│ [**Deutsch (de)**](</Xor/de> "Xor/de") │  [**English (en)**](<../en/Xor.md> "Xor") │  [**suomi (fi)**](</Xor/fi> "Xor/fi") │  [**français (fr)**](</Xor/fr> "Xor/fr") │  **русский (ru)** │    
****

## Contents

  * 1 Логическая операция
    * 1.1 Таблица истинности
  * 2 Побитовая операция
    * 2.1 Переключение битов
    * 2.2 См. также



# Логическая операция

_Исключающее или_ (**xor**) возвращает значение [true](<True.md> "True/ru") тогда и только тогда, когда один из операндов имеет значение true. 

  


## Таблица истинности

A | B | A xor B   
---|---|---  
false  |  false  |  false   
false |  true  |  true   
true |  false  |  true   
true |  true  |  false   
  
  


# Побитовая операция

Побитовая операция xor устанавливает бит в значение 1 в тех местах, где отличаются соответствующие биты в операндах, и в 0, если биты одинаковые. 

## Переключение битов
    
    
    function ToggleBit(const AValue,ABitNumber:integer):integer;
    begin
       result := AValue xor 1 shl ABitNumber;
    end;
    

Если вы вызовете ToggleBit(11,0), то результатом будет 10. Если вызовете ToggleBit(10,2), то результатом будет 14. 

## См. также

  * [ обмен значений с помощью XOR](</index.php?title=Variable_parameter/ru&action=edit&redlink=1> "Variable parameter/ru \(page does not exist\)")
  * [Shl](<Shl.md> "Shl/ru")
  * [Const](</index.php?title=Const/ru&action=edit&redlink=1> "Const/ru \(page does not exist\)")
  * [Function](<Function.md> "Function/ru")
  * [Integer](<Integer.md> "Integer/ru")

---

_Source: [https://wiki.freepascal.org/Xor/ru](https://web.archive.org/web/20210511180047/https://wiki.freepascal.org/Xor/ru)_
