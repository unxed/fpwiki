# Shl

│ **[Deutsch (de)](</Shl/de> "Shl/de")** │  **English (en)** │  **[suomi (fi)](</Shl/fi> "Shl/fi")** │  **[français (fr)](</Shl/fr> "Shl/fr")** │  **[русский (ru)](<../ru/Shl.md> "Shl/ru")** │    
****

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

## Overview

The reserved word **Sh** ift **l** eft (**shl**) performs a left bit-shift operation, shifting the value by the amount of bits specified as an argument (opposite of [shr](<Shr.md> "Shr")). 

Example: 
    
    
    Command is: 00000100 shl 2 (shift left 2 bits)
     
    Action is:  00000100 <- 00 (00 gets added to the right of the value; left 00 "disappears")
     
    Result is:  00010000
    

## Clear a bit
    
    
    function ClearBit( const aValue, aBitNumber : integer ) : integer;
    begin
    // sanity check supplied value
      if (aBitNumber <0) or  (aBitNumber >15) then
          result :=0
      else
    // value ok
         result := aValue and not( 1 shl aBitNumber );
    end;
    

If you call ClearBit(%1111,1), then you get %1101 (The [binary number](<Binary_numeral_system.md> "Binary numeral system") %1111 is 15 and %1101 = 13). 

If you call ClearBit(13,2), then you get 9 (9 = %1001). 

In this case, bits are numbered right to left from 0 to 15, bit 0 being the ones bit and bit 15 being the sign bit. 

  


navigation bar: Pascal logical operators  [operators](<Operator.md> "Operator") |  [`and`](<And.md> "And") • [`or`](<Or.md> "Or") • [`not`](<Not.md> "Not") • [`xor`](<Xor.md> "Xor")  
`shl` • [`shr`](<Shr.md> "Shr")  
`and_then` (N/A)• `or_else` (N/A)   
---|---  
see also  |  [`{$boolEval}`](<$boolEval.md> "$boolEval") • [Reference: § “boolean operators”](<https://www.freepascal.org/docs-html/ref/refsu46.html>) • [Reference: § “logical operators”](<https://www.freepascal.org/docs-html/ref/refsu45.html>)  
  
  * [Set a bit](<Or.md> "Or")
  * [Toggle a bit](<Xor.md> "Xor")
  * [Bit manipulation](<Bit_manipulation.md> "Bit manipulation")
  * [$Bitpacking](<$Bitpacking.md> "$Bitpacking")


  *[N/A]: not available

---

_Source: [https://wiki.freepascal.org/Shl](https://web.archive.org/web/20250114101955/https://wiki.freepascal.org/Shl)_
