# Xor

│ **[Deutsch (de)](</Xor/de> "Xor/de")** │  **English (en)** │  **[suomi (fi)](</Xor/fi> "Xor/fi")** │  **[français (fr)](</Xor/fr> "Xor/fr")** │  **[русский (ru)](<../ru/Xor.md> "Xor/ru")** │    
****Back to[Reserved words](<Reserved_words.md> "Reserved words"). 

The **xor** [`operator`](<Operator.md> "Operator") compares two [`boolean`](<Boolean.md> "Boolean") values, and returns true if and only if one of them is true. 

## Contents

  * 1 Boolean operation
    * 1.1 Truth table
  * 2 Bitwise operation
    * 2.1 Toggle a bit



# Boolean operation

Exclusive or (**xor**) results in a value of [true](<True.md> "True") if and only if exactly one of the operands has a value of true. 

## Truth table

A | B | A xor B   
---|---|---  
false  |  false  |  false   
false |  true  |  true   
true |  false  |  true   
true |  true  |  false   
  
# Bitwise operation

Bitwise xor sets the bit to 1 where the corresponding bits in its operands are different, and to 0 if they are the same. 

Example: 
    
    
        0101'1010
    xor 0011'0100
    ―――――――――――――
        0110'1110
    

## Toggle a bit
    
    
    function ToggleBit(const AValue,ABitNumber:integer):integer;
    begin
       result := AValue xor 1 shl ABitNumber;
    end;
    

If you call `ToggleBit(11,0)` then get `10`. If you call `ToggleBit(10,2)` then get `14`. 

  


navigation bar: Pascal logical operators  [operators](<Operator.md> "Operator") |  [`and`](<And.md> "And") • [`or`](<Or.md> "Or") • [`not`](<Not.md> "Not") • `xor`  
[`shl`](<Shl.md> "Shl") • [`shr`](<Shr.md> "Shr")  
`and_then` (N/A)• `or_else` (N/A)   
---|---  
see also  |  [`{$boolEval}`](<$boolEval.md> "$boolEval") • [Reference: § “boolean operators”](<https://www.freepascal.org/docs-html/ref/refsu46.html>) • [Reference: § “logical operators”](<https://www.freepascal.org/docs-html/ref/refsu45.html>)  
  
  * [ XOR swap](<Variable_parameter.md> "Variable parameter")
  * [Const](<Const.md> "Const")
  * [Function](<Function.md> "Function")
  * [Integer](<Integer.md> "Integer")


  *[N/A]: not available

---

_Source: [https://wiki.freepascal.org/Xor](https://web.archive.org/web/20240308084942/https://wiki.freepascal.org/Xor)_
