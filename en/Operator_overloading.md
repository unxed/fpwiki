# Operator overloading

│ **English (en)** │

[Operator](<Operator.md> "Operator") overloading refers to re-defining already defined [operators](<Operators.md> "Operators") with new definitions. The term is – although imprecisely – used for operator definitions, that have not yet been defined, too. 

## Contents

  * 1 Availability
  * 2 Overloadable operators
  * 3 Definition
  * 4 Routing
  * 5 See also



## Availability

The [FPC](<FPC.md> "FPC") supports [PXSC](<PXSC.md> "PXSC")-style operator overloading, however it is not allowed in [`{$mode TP}`](<Mode_TP.md> "Mode TP") or [`{$mode MacPas}`](<Mode_MacPas.md> "Mode MacPas"). In [`{$mode Delphi}`](<Mode_Delphi.md> "Mode Delphi"), operator overloading can only be done in the context of classes or advanced records (confer [Delphi documentation](<http://docwiki.embarcadero.com/RADStudio/Rio/en/Operator_Overloading_\(Delphi\)>)), so the Delphi mode does not support global operator overloads as [`{$mode FPC}`](<Mode_FPC.md> "Mode FPC") and [`{$mode objFPC}`](<Mode_ObjFPC.md> "Mode ObjFPC") do. 

## Overloadable operators

Almost all operators can be (re-)defined: 

  * assignment operators 
    * [`:=`](<Becomes.md> "Becomes")
    * the special operator `explicit` (referring to explicit [typecasts](<Typecast.md> "Typecast"))
  * arithmetic operators 
    * [`+`](<Plus.md> "Plus") (only binary operation)
    * [`-`](<Minus.md> "Minus") (both unary and binary)
    * [`*`](<Asterisk.md> "Asterisk")
    * `**`
    * [`div`](<Div.md> "Div")
    * [`mod`](<Mod.md> "Mod")
    * [`/`](<Slash.md> "Slash")
    * [`inc`](<Inc_and_Dec.md> "Inc and Dec")
    * [`dec`](<Inc_and_Dec.md> "Inc and Dec")
    * (As of 2020 in the future: `pow`)
  * comparison operators 
    * [`<`](<Less_than.md> "Less than")
    * [`>`](<Greater_than.md> "Greater than")
    * `>=`
    * `<=`
    * [`=`](<Equal.md> "Equal")
    * [`<>`](<Not_equal.md> "Not equal")
    * [`><`](<symmetric_difference.md> "symmetric difference")
    * [`in`](<In.md> "In")
  * logical operators 
    * [`and`](<And.md> "And")
    * [`or`](<Or.md> "Or")
    * [`not`](<Not.md> "Not")
    * [`xor`](<Xor.md> "Xor")
    * [`shl`](<Shl.md> "Shl")
    * [`shr`](<Shr.md> "Shr")
    * (As of 2020 in the future: `and_then`, `or_else`)
  * and the special operator [`enumerator`](<for-in_loop.md> "for-in loop").



The only operators that can not be overloaded are [`@` (address operator)](<@.md> "@"), [`^` (de-referencing operator)](<^.md> "^"), `as` and [`is`](<Is.md> "Is"). 

## Definition

An operator is declared as if it was a [function](<Function.md> "Function"), with a few differences: 

  * Instead of the word `function` it starts with `operator`.
  * The function's [identifier](<Identifier.md> "Identifier") is always one of the available operators e. g. `><`, although this would not constitute a valid identifier anywhere else. However, in `{$mode Delphi}` a spelled out operator name _has_ to be used. These are allowed in other modes, too (if the specific overload is allowed in a Delphi mode):

Delphi compatibility operator names  operator symbol | symbolic name   
---|---  
`:=` | `implicit`  
`+` | `add`, `positive`  
`-` | `negative`, `subtract`  
`*` | `multiply`  
`div` | `intDivide`  
`mod` | `modulus`  
`/` | `divide`  
`=` | `equal`  
`<>` | `notEqual`  
`<` | `lessThan`  
`<=` | `lessThanOrEqual`  
`>` | `greaterThan`  
`>=` | `greaterThanOrEqual`  
`not` | `logicalNot`  
`and` | `logicalAnd`  
`or` | `logicalOr`  
`and` | `bitwiseAnd`  
`or` | `bitwiseOr`  
`xor` | `bitwiseXor`  
`shl` | `leftShift`  
`shr` | `rightShift`  
  
    Note, `><`, `**` and the bitwise `not` have no equivalent in Delphi, but the symbolic operator designation is allowed instead.

  * The parameter list has to name exactly one or two parameters, depending on the operator. The first formal parameter always refers, if applicable, to the operand on the left-hand side.
  * A result identifier has to be specified in front of the colon separating the result type, but can be omitted where the special identifier `result` is available.
  * Comparisons _have_ to yield a [`Boolean` value](<Boolean.md> "Boolean").
  * The “source” and “target” types of assignment operator overloads must differ.
  * Note, the concept of “default parameters,” that means a [default value](<Routine.md> "Routine") for a parameter, only applies to “real” functions. When an operator is used, e. g. in an [expression](<expression.md> "expression"), you can not just skip naming an operand, hoping the compiler will insert “something”. Therefore, the concept of [default parameter](<Default_parameter.md> "Default parameter") values does not apply to operator overloads.



The following shows a valid operator declaration (in `{$mode Delphi}` the `:=` has to be replaced by `implicit` and the declaration may only appear in the context of class or advanced record definitions). 
    
    
    operator := (x: myNewType) y: someOtherType;
    

Operators are defined the same way as any other function, by following the signature with a [block](<Block.md> "Block"). 

Some operator overloads are not allowed: 

  * [overloading `shortstring` assignments with lengths other than `255`](<User_Changes_2.4.md> "User Changes 2.4.0")
  * [`+` in conjunction with dynamic arrays](<User_Changes_3.md> "User Changes 3.2") if `{$modeSwitch arrayOperators+}` (since FPC 3.2)
  * `+` and `-` in conjunction with enumeration types



## Routing

How an operator overload definition is chosen differs in many aspects how a function is chosen. 

  * The assignment operator `:=` is used for _implicit_ typecasts. Everywhere, where a value has to be stored in memory, an implicit typecast may occur. Note, that _calling_ a [routine](<Routine.md> "Routine") requires storing its parameters in memory, too, thus the parameter list might become subject of implicit typecasts as well, even though on the surface they are not part of an assignment statement.
  * Unlike regular function overloads, assignment operator overloads are chosen _by their result type_.
  * Operator overloads can not be chosen explicitly by their [scope](<Scope.md> "Scope") they are defined in. Something like `unitDefiningOverloads.+` is not possible. The last operator definition _always_ wins and this can not be changed.
  * Operator precedence remains as usual for all operator symbols. When defining operators for a new custom type from scratch, the `*` will still bind stronger than the `+`. The operator precedence system can not be altered. FPC does not support [PXSC](<PXSC.md> "PXSC")-style `priority` clauses.



Operator overloads should be used with caution. They potentially make it harder to identify problems, since it is not necessarily obvious that an operator overload applies. 

## See also

  * [chapter “Operator overloading” in the “Free Pascal Reference Guide”](<https://www.freepascal.org/docs-html/ref/refch15.html>)
  * [article “operator overloading” in the English Wikipedia](<https://en.wikipedia.org/wiki/Operator_overloading>)

---

_Source: [https://wiki.freepascal.org/Operator_overloading](https://web.archive.org/web/20250513142502/https://wiki.freepascal.org/Operator_overloading)_
