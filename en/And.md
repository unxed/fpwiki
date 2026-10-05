# And

│ **English (en)** │  **[русский (ru)](<../ru/And.md>)** │

The binary operator `and` performs a logical conjunction. [FPC](<FPC.md> "FPC") also does a bitwise `and` when supplied with ordinal types. 

## Contents

  * 1 Boolean operation
  * 2 Bitwise operation
  * 3 comparative remarks
  * 4 see also



## Boolean operation

The operator `and` accepts to two Boolean type values. It is the logical conjunction written in classic logic as [math]\displaystyle{ A \land B }[/math]. Electrical engineers may write [math]\displaystyle{ A \times B }[/math] or [math]\displaystyle{ A \cdot B }[/math], or eliminating the multiplication sign altogether writing [math]\displaystyle{ AB }[/math]. However, the [asterisk](<_.md> "*") has a different meaning in programming. The Boolean `and` evaluates to [`true`](<false_and_true.md> "false and true") if and only if both operands are `true`. 

`A` | `B` | `A and B`  
---|---|---  
`false` | `false` | `false`  
`false` | `true` | `false`  
`true` | `false` | `false`  
`true` | `true` | `true`  
truth table for logical conjunction 

## Bitwise operation

FPC also defines a bitwise `and`. Taking two ordinal operands logical `and` is calculated bit by bit: 
    
    
        1010'1100
    and 0011'0100
    ――――――――――――
        0010'0100
    

## comparative remarks

Depending on the compiler's specific implementation of the data type [`set`](<Set.md> "Set"), the [intersection of sets](<Asterisk.md> "Asterisk") virtually does the same as the bitwise `and`. 

## see also

  * [`or`](<Or.md> "Or")

---

_Source: [https://wiki.freepascal.org/And](https://web.archive.org/web/20240308084938/https://wiki.freepascal.org/And)_
