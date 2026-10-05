# Not

│ **English (en)** │  **[русский (ru)](<../ru/Not.md>)** │

The unary [operator](<Operator.md> "Operator") `not` negates a Boolean value. [FPC](<FPC.md> "FPC") also knows the bitwise `not` when supplied with an ordinal type. 

`not` is a [reserved word](<Reserved_word.md> "Reserved word"). 

## Boolean operation

The operator `not` represents the logical negation [math]\displaystyle{ \neg A }[/math]. In electrical engineering one might write [math]\displaystyle{ -A }[/math] or [math]\displaystyle{ \overline{A} }[/math] instead, however the unary [minus sign](<Minus.md> "Minus") has a different meaning in programming. 

`A` | `not A`  
---|---  
`false` | `true`  
`true` | `false`  
Truth table for logical negation 

`not` has the highest precedence among logical operators. 

## Bitwise operation

The bitwise `not` flips every bit in an ordinal type. 
    
    
    not 1100'1010
    ―――――――――――――
        0011'0101
    

It effectively calculates the one’s complement. On virtually all platforms it is implemented by the `not` instruction. On NAND-gate-based architectures the `not` instruction can be calculated by the expression [math]\displaystyle{ A &#8965; A }[/math]. 

Note, that only `not %0` will _definitely_ result in a value interpretable as `true`. However, not every other `not x` will result in a value interpretable as `false`, since only `0` is considered as `false` and every other value as `true`. For example, `boolean(not %1)` will evaluate as `true`, but only `boolean(not high(nativeUInt))` will evaluate to `false`. 

  


navigation bar: Pascal logical operators  [operators](<Operator.md> "Operator") |  [`and`](<And.md> "And") • [`or`](<Or.md> "Or") • `not` • [`xor`](<Xor.md> "Xor")  
[`shl`](<Shl.md> "Shl") • [`shr`](<Shr.md> "Shr")  
`and_then` (N/A)• `or_else` (N/A)   
---|---  
see also  |  [`{$boolEval}`](<$boolEval.md> "$boolEval") • [Reference: § “boolean operators”](<https://www.freepascal.org/docs-html/ref/refsu46.html>) • [Reference: § “logical operators”](<https://www.freepascal.org/docs-html/ref/refsu45.html>)  
  
  * [`system.logicalNot`](<https://www.freepascal.org/docs-html/rtl/system/.op-logicalnot-variant-ariant.html>)


  *[N/A]: not available

---

_Source: [https://wiki.freepascal.org/Not](https://web.archive.org/web/20240920204107/https://wiki.freepascal.org/Not)_
