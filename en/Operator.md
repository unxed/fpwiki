# Operator

│ **English (en)** │

****An**operator** is a special kind of [function](<Function.md> "Function"). It can be invoked by placing keywords adjacent to suitable operands. 

The word _operator_ colloquially refers to the symbol or [keyword](<Keyword.md> "Keyword") identifying the function that implements the actual operation. `operator` is also a [reserved word](<Reserved_word.md> "Reserved word") that appears in the course of [operator overloading](<Operator_overloading.md> "Operator overloading"). 

## Contents

  * 1 usage
  * 2 operator precedence
    * 2.1 evaluation order
  * 3 see also



## usage

Most [operators](<Operators.md> "Operators") are binary, that means require two operands. Binary operator symbols appear between two operands, like this: 
    
    
    x + 5
    

The plus character identifies the addition operation (Standard Pascal). `x` and `5` are its operands. This style is called infix notation. 

Unary operators – those which require only one operand – appear in front of their operands (prefix notation), with the exception of `^`, the de-referencing operator, which appears after its operand (postfix notation). Example: 
    
    
    -x
    

Here, the minus character identifies the sign inversion operation (Standard Pascal). Blanks between operator and operand do not harm, confer the following section regarding precedence. However, it is common practice to place unary operator symbols back to back with their operands. 

Operator symbols always appear explicitly, except the implicit typecast and enumerator operator. Unlike in mathematics, no invisible “times” is assumed between two identifiers. 

Implicit typecasts occur, where a value has to be stored in a memory location. Note, that _calling_ a routine triggers implicit typecasts, too. In order to pass the parameters to the routine, they are stored somewhere. Implicit typecasts are not limited to run-of-the-mill assignment statements. 

## operator precedence

Operands and operators are the building blocks of [expressions](<expression.md> "expression"). The use of operators is a powerful notational tool, since [LOCs](</index.php?title=LOC&action=edit&redlink=1> "LOC \(page does not exist\)") do not get cluttered by function calls consisting of function identifiers and parentheses, but instead the operands and (ideally) a short symbol achieve the same. Instead of, for example, `sum(x, -8)` the expression `x - 8` evaluates to the same value, while writing five less characters. 

In order to increase conciseness subexpressions of expressions propagate their intermediate result in a predefined hierarchy. This hierarchy can be superseded by placing parentheses `( )` around subexpressions that shall be treated as _one_ expression, so the regular precedence rules apply in the enclosed part remaining uninfluenced from the expression’s rest. Operators that have have higher precedence, bind to operands stronger. 

operator precedence  precedence | operators | category   
---|---|---  
highest  
(first) | 

  * [`not`](<Not.md> "Not") (both, logical and bitwise variant)
  * unary [`+`](<Plus.md> "Plus") (sign identity, “positive sign”)
  * unary [`-`](<Minus.md> "Minus") (sign inversion, “negative sign”)
  * `**`
  * `pow`
  * [`@`](<@.md> "@")†
  * [`^`](<^.md> "^")† (de-referencing operator)
  * `explicit` typecasts

| 

  * unary operators (except  
destination-dependent operators)
  * power

  
second | 

  * [`*`](<_.md> "*")
  * [`and`](<And.md> "And") (both, logical and bitwise variant)
  * `and_then`
  * [`/`](<Slash.md> "Slash")
  * [`div`](<Div.md> "Div")
  * [`mod`](<Mod.md> "Mod")
  * [`shl`](</index.php?title=shl&action=edit&redlink=1> "shl \(page does not exist\)") (`<<`)
  * `shr` (`>>`)
  * `as`
  * ~~`is` (until FPC 3.3.1, trunk revision 44266)~~

| 

  * multiplication operators
  * conditional typecast

  
third | 

  * [`+`](<Plus.md> "Plus")
  * [`-`](<Minus.md> "Minus")
  * [`or`](<Or.md> "Or") (both, logical and bitwise variant)
  * `or_else`
  * [`xor`](<Xor.md> "Xor") (both, logical and bitwise variant)

| 

  * addition operators
  * complex logical operators

  
fourth | 

  * [`<`](<Less_than.md> "Less than")
  * [`>`](<Greater_than.md> "Greater than")
  * [`=`](<Equal.md> "Equal")
  * [`<>`](<Not_equal.md> "Not equal")
  * `<=` (both, ⊆ as well as ≤)
  * `>=`
  * `in`
  * [`><`](<symmetric_difference.md> "symmetric difference")
  * [`is`](<Is.md> "Is") (since FPC 3.3.1, cf. [Issue #35909](<https://bugs.freepascal.org/view.php?id=35909>))

| 

  * relational operators
  * inheritance test

  
lowest  
(last) | 

  * [`:=`](<Becomes.md> "Becomes") (implicit [typecasts](<Typecast.md> "Typecast"))
  * `enumerator` (if applicable)

| 

  * destination-dependent conversions

  
  
†) This symbol does not refer to an actual operator, but is (imprecisely) called as one. 

[`Inc` and `dec`](<Inc_and_Dec.md> "Inc and Dec") are pseudo operators: They may be redefined via the operator overloading mechanism, but they can not appear in expressions, like any function could. Therefore their precedence is moot. 

`Include` and `exclude` are also not operators, but shorthand for common LOCs. 

`@` and `^` albeit being called operators, are _not_ operators, but rather instruct the [compiler](<Compiler.md> "Compiler") to interpret an [identifier](<Identifier.md> "Identifier") differently than usual. This is not done via any function, but compiler intrinsics. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Operator precedence is slightly modified by `{$modeSwitch ISOUnaryMinus}`, which is enabled by default in [`{$mode ISO}`](<Mode_iso.md> "Mode iso") and [`{$mode extendedPascal}`](<Mode_extendedpascal.md> "Mode extendedpascal"). There, the unary minus is on the same level as other addition operators are. 

### evaluation order

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Operator precedence does not infer evaluation order. The compiler may re-arrange expressions according to associative and commutative properties of operators. 

No assumptions shall be made, and there is no guarantee, that expressions are evaluated from left to right (or the reverse direction). The [FPC](<FPC.md> "FPC") for instance, evaluates more “complex” subexpressions first before moving on to “trivial” parts (ratio: avoiding register spilling). 

If a subexpression _has_ to be evaluated first, e.g. a function triggering some side-effects, the expression has to be split up into two separate statements. For example: 
    
    
    x := foo() + bar();
    

The compiler may evaluate `foo()` or `bar()` first. If `bar()` ought to be evaluated first at all events, the statement has to be split into two separate ones: 
    
    
    x := bar();
    x := x + foo();
    

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** The [compiler directive](<Compiler_directive.md> "Compiler directive") [`{$boolEval off}`](<$boolEval.md> "$boolEval") enables “lazy” evaluation. Parts of expressions may not be evaluated at all. 

## see also

  * [“Operators”](<https://www.freepascal.org/docs-html/ref/refse88.html>) in the FreePascal Reference Guide
  * Tutorial: [assignment and operations](<Basic_Pascal_Tutorial/Chapter_1/Assignment_and_Operations.md> "Basic Pascal Tutorial/Chapter 1/Assignment and Operations")
  * [management operators](<management_operators.md> "management operators") – a collection of specially supported routines

---

_Source: [https://wiki.freepascal.org/Operator](https://web.archive.org/web/20230512101427/https://wiki.freepascal.org/Operator)_
