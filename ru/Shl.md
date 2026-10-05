# Shl

│ **[Deutsch (de)](</Shl/de> "Shl/de")** │  **[English (en)](<../en/Shl.md> "Shl")** │  **[suomi (fi)](</Shl/fi> "Shl/fi")** │  **[français (fr)](</Shl/fr> "Shl/fr")** │  **русский (ru)** │    
****

## Обзор

**Sh** ift **l** eft (**shl**) выполняет операцию битового сдвига влево, сдвигая значение на количество бит, указанное в аргументе (противоположное действие [shr](<Shr.md> "Shr/ru")). 

Например 
    
    
    Команда: 00000100 shl 2 (сдвиг влево на 2 бита)
     
    Действие:  00000100 <- 00 (00 добавляется справа к значению; слева 00 "теряется")
     
    Результат:  00010000
    

## Сбросить бит
    
    
    function ClearBit( const aValue, aBitNumber : integer ) : integer;
    begin
      result := aValue and not( 1 shl aBitNumber );
    end;
    

Если вы вызовете ClearBit(%1111,1), то получите %1101 ([двоичное число](<Binary_numeral_system.md> "Binary numeral system/ru") %1111 это 15, а %1101 = 13). 

Если вызовете ClearBit(13,2), то получите 9 (9 = %1001) . 

## См. также

  * [And](<And.md> "And/ru")
  * [Not](<Not.md> "Not/ru")
  * [Установка бита](<Or.md> "Or/ru")
  * [Переключение битов](<Xor.md> "Xor/ru")
  * [Shr](<Shr.md> "Shr/ru")
  * [Bit manipulation](</index.php?title=Bit_manipulation/ru&action=edit&redlink=1> "Bit manipulation/ru \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/Shl/ru](https://web.archive.org/web/20250210102021/https://wiki.freepascal.org/Shl/ru)_
