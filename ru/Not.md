# Not

│ [**Deutsch (de)**](</Not/de> "Not/de") │  [**English (en)**](<../en/Not.md> "Not") │  [**suomi (fi)**](</Not/fi> "Not/fi") │  [**français (fr)**](</Not/fr> "Not/fr") │  **русский (ru)** │    
****  
  


## Contents

  * 1 Логическая операция
    * 1.1 Таблица истинности
  * 2 Побитовая операция
    * 2.1 Единичное дополнение
  * 3 См. также



## Логическая операция

Логическая операция **Not** выдает значение [true](<True.md> "True/ru") если значение равно [false](<False.md> "False/ru"). 

### Таблица истинности

A | Not A   
---|---  
false  |  true   
true  |  false   
  
  


## Побитовая операция

Побитовая операция **not** устанавливает бит в 1 если соответствующий бит операнда равен 0, и в 0 если бит равен 1. 

### Единичное дополнение
    
    
     function OnesComplement ( const aValue : byte ): byte;
     begin
       result := Not AValue;
     end;

Если вы вызовете OnesComplement([%](<Percent_sign.md> "Percent sign/ru")10000000), то получите %01111111 (%10000000 = 128 и %01111111 = 127). Если вы вызовете OnesComplement(%00000111), то результатом будет 248 (248 = %11111000). 

  

    
    
     function OnesComplement2 ( const aValue : shortint ): shortint;
     begin
       result := Not AValue;
     end;

Если вы вызовете OnesComplement2(%00000010), то получится %11111101 (%00000010 = 2 и %11111101 = -3, когда [тип](<Type.md> "Type/ru") операнда shortint). Если вы вызовете OnesComplement2(7), то получите -8 (-8 = %11111000, когда тип операнда shortint и 7 = %00000111 ). 

## См. также

  * [And](<And.md> "And/ru")
  * [Shl](<Shl.md> "Shl/ru")
  * [Const](</index.php?title=Const/ru&action=edit&redlink=1> "Const/ru \(page does not exist\)")
  * [Function](<Function.md> "Function/ru")
  * [Byte](<Byte.md> "Byte/ru")
  * [Shortint](<Shortint.md> "Shortint/ru")
  * [ Clear_a_bit](<../en/Shl.md> "Shl") (bitwise example)
  * [Bit manipulation](</index.php?title=Bit_manipulation/ru&action=edit&redlink=1> "Bit manipulation/ru \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/Not/ru](https://web.archive.org/web/20190822045446/https://wiki.freepascal.org/Not/ru)_
