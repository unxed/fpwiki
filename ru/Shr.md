# Shr

│ **[Deutsch (de)](</Shr/de> "Shr/de")** │  **[English (en)](<../en/Shr.md> "Shr")** │  **[français (fr)](</Shr/fr> "Shr/fr")** │  **русский (ru)** │    
****

## Contents

  * 1 Обзор
  * 2 Shr со знаковыми типами
  * 3 Проверить установлен ли бит
  * 4 См. также



## Обзор

**Sh** ift **r** ight (shr) выполняет операцию битового сдвига вправо (противоположное действие [shl](<Shl.md> "Shl/ru")). 

## Shr со знаковыми типами

Примечание: в отличие от оператора **> >** в языке C, оператор shr является логическим (не арифметическим) оператором сдвига, даже если левый операнд является знаковым целым числом. Неявное приведение типов и расширение до большего беззнакового типа может быть выполнено до операции сдвига. Проверьте, что в действительности напечатает следующая программа. 
    
    
    program ShrTest;
    begin
      WriteLn(ShortInt(-3) shr 1);
    end.
    

## Проверить установлен ли бит
    
    
    function isBitSet(AValue, ABitNumber:integer):boolean;
    begin
       result:=odd(AValue shr ABitNumber);
    end;
    

## См. также

  * [And](<And.md> "And/ru")
  * [Boolean](<Boolean.md> "Boolean/ru")
  * [Const](</index.php?title=Const/ru&action=edit&redlink=1> "Const/ru \(page does not exist\)")
  * [Function](<Function.md> "Function/ru")
  * [Integer](<Integer.md> "Integer/ru")
  * [Odd](</index.php?title=Odd/ru&action=edit&redlink=1> "Odd/ru \(page does not exist\)")
  * [Or](<Or.md> "Or/ru")
  * [Shl](<Shl.md> "Shl/ru")

---

_Source: [https://wiki.freepascal.org/Shr/ru](https://web.archive.org/web/20250315101233/https://wiki.freepascal.org/Shr/ru)_
