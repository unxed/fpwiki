# Or

│ **[Deutsch (de)](</Or/de> "Or/de")** │  **English (en)** │  **[suomi (fi)](</Or/fi> "Or/fi")** │  **[français (fr)](</Or/fr> "Or/fr")** │  **[русский (ru)](<../ru/Or.md> "Or/ru")** │    
****

The [reserved word](<Reserved_word.md> "Reserved word") `or` is a binary [operator](<Operator.md> "Operator"). Originally it stood for the logical disjunction of two [boolean values](<Boolean.md> "Boolean") only, but with the advent of [operator overloading](<Operator_overloading.md> "Operator overloading") [FPC](<FPC.md> "FPC") allows everything else, too. FPC also defines the `or` operator accepting two ordinal types while performing calculations on their internal binary representation. 

## Contents

  * 1 boolean operation
  * 2 bitwise operation
    * 2.1 setting a bit
    * 2.2 comparative remarks
  * 3 see also



## boolean operation

The expression `A or B` represents the term [math]\displaystyle{ A \lor B }[/math] as it is familiar from classical logic. In electrical engineering writing [math]\displaystyle{ A + B }[/math] is common, too, but in programming the meaning of the plus sign differs. `A` and `B` are both boolean values. The expression evaluates to either [`true` or `false`](<false_and_true.md> "false and true"). It is only `false` if both operands are `false`: 

`A` | `B` | `A or B`  
---|---|---  
`false` | `false` | `false`  
`false` | `true` | `true`  
`true` | `false` | `true`  
`true` | `true` | `true`  
truth table for logical disjunction 

## bitwise operation

Since virtually all instruction sets have an `or` instruction, it is no surprise some high-level languages, especially those which aim to be suitable for hardware programming, provide some comparable functionality by itself. In FPC the `or` operator is defined appropriately. Such an expression, also known as bitwise or, requires two ordinal operands. The operation virtually performs a logical or taking each corresponding bit from both operands. 
    
    
       0101'1010
    or 0000'1011
    ――――――――――――
       0101'1011
    

For a typical usage example of `or` confer [`program messageNo`](<&.md> "&")’s passing remark. 

### setting a bit

A common task is to set a specific bit. To achieve this utilizing the `or` operator elicits a smart implementation: 
    
    
    type
    	integerBitIndex = 0..bitSizeOf(integer)-1;
    
    {$push}
    {$rangeChecks on} // instead of raising an exception, generate an RTE
    function maskOn(const x: integer; const i: integerBitIndex): integer;
    begin
    	maskOn := x or (%1 shl i);
    end;
    {$pop}
    

For example calling `maskOn(%1000, 1)` will result in `%1010` (`%1000` equals decimal eight, and `%1010` is ten). 

### comparative remarks

Note: The concept of [sets](<Set.md> "Set") is an integral part of Pascal. Whilst in other programming languages considering operations on the bit level is not unusual, Pascal provides you with a powerful notion that relieves you from the burden of thinking about bits. Take it under advisement whether your programming task can be modeled with sets even better. 

When using sets a [plus sign `+`](<Plus.md> "Plus") virtually does the same as the bitwise `or` does (depends on the compiler’s implementation of the data type `set`). This notation is, as already mentioned above, familiar from electrical engineering. However, when handling sets the bitwise `or` is called a _union_ (of sets). 

## see also

  * [§ “boolean operators” in the “Free Pascal Reference Guide”](<https://www.freepascal.org/docs-html/ref/refsu46.html>) regarding the logical disjunction
  * [§ “logical operators” in the “Free Pascal Reference Guide”](<https://www.freepascal.org/docs-html/ref/refsu45.html>) regarding the bitwise `or`
  * [`and`](<And.md> "And")
  * [`xor`](<Xor.md> "Xor")
  * [`system.logicalor`](<https://www.freepascal.org/docs-html/rtl/system/.op-logicalor-variant-ariant-ariant.html>)

---

_Source: [https://wiki.freepascal.org/Or](https://web.archive.org/web/20241209235217/https://wiki.freepascal.org/Or)_
