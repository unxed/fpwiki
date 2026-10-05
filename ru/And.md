# And

│ **[English (en)](<../en/And.md>)** │  **русский (ru)** │

## Contents

  * 1 Логическая операция
    * 1.1 Таблица истинности
  * 2 Побитовая операция
    * 2.1 Является ли операнд степенью двойки
  * 3 См. также



## Логическая операция

Логическая операция **And** выдает значение [true](<True.md> "True/ru") тогда и только тогда, когда оба логических операнда равны true. 

### Таблица истинности

A | B | A and B   
---|---|---  
false  |  false  |  false   
false |  true  |  false   
true |  false  |  false   
true |  true  |  true   
  
## Побитовая операция

Для логической операции **And** (также известна, как Побитовое И) требуются операнды порядкового типа и в результирующей переменной бит устанавливается в 1 тогда и только тогда, когда оба соответствующих бита равны 1. 

### Является ли операнд степенью двойки
    
    
    function IsPowerOfTwo( const aValue : longint ): boolean;
    var
      x : longint;
      b : boolean;
    begin
      b := false;
      if aValue <> 0 then
        begin
          x := aValue - 1;
          x := x and aValue;
          if x = 0 then b := true;
        end;
      result := b;
    end;
    

Если вы вызовете IsPowerOfTwo(4), то получите результат **true**. Если вызовете IsPowerOfTwo(5), то результатом будет **false**. 

## См. также

  * [Not](<Not.md> "Not/ru")
  * [Or](<Or.md> "Or/ru")
  * [Shl](<Shl.md> "Shl/ru")


  * [Const](</index.php?title=Const/ru&action=edit&redlink=1> "Const/ru \(page does not exist\)")
  * [Function](<Function.md> "Function/ru")
  * [Integer](<Integer.md> "Integer/ru")


  * [Clear a bit](<../en/Shl.md> "Shl")
  * [Bit manipulation](</index.php?title=Bit_manipulation/ru&action=edit&redlink=1> "Bit manipulation/ru \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/And/ru](https://web.archive.org/web/20250426100447/https://wiki.freepascal.org/And/ru)_
