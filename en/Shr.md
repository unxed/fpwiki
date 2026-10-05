# Shr

│ **[Deutsch (de)](</Shr/de> "Shr/de")** │  **English (en)** │  **[français (fr)](</Shr/fr> "Shr/fr")** │  **[русский (ru)](<../ru/Shr.md> "Shr/ru")** │    
****

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  


## Overview

The reserved word **Sh** ift **r** ight (shr) performs a logical right bit-shift operation (opposite than [shl](<Shl.md> "Shl")). 

Example: 
    
    
    Command is: 00000100 shr 2 (shift right 2 bits)
     
    Action is:  00000100 -> 00 (00 gets added to the left of the value; right 00 "disappears")
     
    Result is:  00000001
    

## Shr with signed types

Note: unlike the >> operator in the C language, the shr operator is a logical (not arithmetic) bit shift, even if the left operand is a signed integer. An implicit [typecast](<Typecast.md> "Typecast") and extension to a larger unsigned type may be performed before the shift operation. Check what the following program actually prints. 
    
    
    program ShrTest;
    begin
      WriteLn(ShortInt(-3) shr 1);
    end.
    

## Is a bit set
    
    
    function isBitSet(AValue, ABitNumber:integer):boolean;
    begin
       result:=odd(AValue shr ABitNumber);
    end;
    

  


navigation bar: Pascal logical operators  [operators](<Operator.md> "Operator") |  [`and`](<And.md> "And") • [`or`](<Or.md> "Or") • [`not`](<Not.md> "Not") • [`xor`](<Xor.md> "Xor")  
[`shl`](<Shl.md> "Shl") • `shr`  
`and_then` (N/A)• `or_else` (N/A)   
---|---  
see also  |  [`{$boolEval}`](<$boolEval.md> "$boolEval") • [Reference: § “boolean operators”](<https://www.freepascal.org/docs-html/ref/refsu46.html>) • [Reference: § “logical operators”](<https://www.freepascal.org/docs-html/ref/refsu45.html>)  
  
  * [Const](<Const.md> "Const")
  * [Function](<Function.md> "Function")
  * [Integer](<Integer.md> "Integer")


  *[N/A]: not available

---

_Source: [https://wiki.freepascal.org/Shr](https://web.archive.org/web/20250218054909/https://wiki.freepascal.org/Shr)_
