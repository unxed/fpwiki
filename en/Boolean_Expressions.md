# Basic Pascal Tutorial/Chapter 3/Boolean Expressions

│ **English (en)** │

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_3/Sequential_control.md> "Basic Pascal Tutorial/Chapter 3/Sequential control") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_3/IF.md> "Basic Pascal Tutorial/Chapter 3/IF")  
---|---|---  
  
3B - Boolean Expressions (author: Tao Yue, state: unchanged) 

[Boolean](<Boolean.md> "Boolean") expressions are used to compare two values and get a `true-or-false` answer: 
    
    
    value1 **relational_operator** value2
    

The following relational operators are used: 
    
    
    <	less than
    >	greater than
    =	equal to
    <=	less than or equal to
    >=	greater than or equal to
    <>	not equal to
    

You can assign Boolean expressions to Boolean variables. Here we assign a `true` expression to some_bool: 
    
    
     some_bool := 3 < 5;
    

Complex Boolean expressions are formed by using the Boolean operators: 
    
    
    not	negation (~)
    and	conjunction (^)
    or	disjunction (v)
    xor	exclusive-or
    

`NOT` is a unary operator — it is applied to only one value and inverts it: 

  * `not true = false`
` `
  * `not false = true`



`AND` yields `TRUE` only if both values are `TRUE`: 

  * `TRUE and FALSE = FALSE`
` `
  * `TRUE and TRUE = TRUE`



`OR` yields `TRUE` if at least one value is `TRUE`: 

  * `TRUE or TRUE = TRUE`
`
  * TRUE or FALSE = TRUE
  * FALSE or TRUE = TRUE
`
  * `FALSE or FALSE = FALSE`



`XOR` yields `TRUE` if one expression is `TRUE` and the other is `FALSE`. Thus: 

  * `TRUE xor TRUE = FALSE`
`
  * TRUE xor FALSE = TRUE
  * FALSE xor TRUE = TRUE
`
  * `FALSE xor FALSE = FALSE`



When combining two Boolean expressions using relational and Boolean operators, be careful to use parentheses. 
    
    
    (3>5) or (650<1)
    

This is because the Boolean operators are higher on the order of operations than the relational operators: 

  1. `not`
`
  2. * / div mod and
  3. + - or
`
  4. `< > <= >= = <>`



So `3 > 5 or 650 < 1` becomes evaluated as `3 > (5 or 650) < 1`, which makes no sense, because the Boolean operator or only works on Boolean values, not on integers. 

The Boolean operators (`AND, OR, NOT, XOR`) can be used on Boolean variables just as easily as they are used on Boolean expressions. 

Whenever possible, don't compare two real values with the equals sign. Small round-off errors may cause two equivalent expressions to differ. 

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_3/Sequential_control.md> "Basic Pascal Tutorial/Chapter 3/Sequential control") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_3/IF.md> "Basic Pascal Tutorial/Chapter 3/IF")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Boolean_Expressions](https://web.archive.org/web/20250319142409/https://wiki.freepascal.org/Boolean_Expressions)_
