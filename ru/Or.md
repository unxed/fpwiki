# Or

│ **[Deutsch (de)](</Or/de> "Or/de")** │  **[English (en)](<../en/Or.md> "Or")** │  **[suomi (fi)](</Or/fi> "Or/fi")** │  **[français (fr)](</Or/fr> "Or/fr")** │  **русский (ru)** │    
****

## Contents

  * 1 Логическая операция
    * 1.1 Таблица истинности
  * 2 Побитовая операция
    * 2.1 Установка бита
    * 2.2 См. также



# Логическая операция

Логическая операция **Or** выдает значение [true](<True.md> "True/ru") в случае, если любой из операндов имеет значение true и [false](<False.md> "False/ru"), если оба логических операнда равны false. 

## Таблица истинности

A | B | A or B   
---|---|---  
false  |  false  |  false   
false |  true  |  true   
true |  false  |  true   
true |  true  |  true   
  
# Побитовая операция

Для логической операции **Or** (также известна, как Побитовое ИЛИ) требуются операнды порядкового типа и в результирующей переменной бит устанавливается в 1, если один из соответствующих битов в операндах равен 1, и в 0 если оба бита равны 0. 

## Установка бита
    
    
    function SetBit(const AValue, ABitNumber:integer):integer;
    begin
       result := AValue or (1 shl ABitNumber);
    end;
    

Если вы вызовете SetBit(%1000,1), то получится %1010 (%1000 = 8 and %1010 = 10). Если вызовете SetBit(10,2), то получится 14 (14 = %1110). Если вызовете SetBit(10,1), то результатом будет 10. 

## См. также

  * [And](<And.md> "And/ru")
  * [Const](</index.php?title=Const/ru&action=edit&redlink=1> "Const/ru \(page does not exist\)")
  * [Function](<Function.md> "Function/ru")
  * [Integer](<Integer.md> "Integer/ru")
  * [Shl](<Shl.md> "Shl/ru")
  * [Shr](<Shr.md> "Shr/ru")

---

_Source: [https://wiki.freepascal.org/Or/ru](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/Or/ru)_
