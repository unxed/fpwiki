# expression

│ **English (en)** │

An **expression** is a non-productive rule that resolves by calculation into a value. They consist of at least one operand, and additional operands may be linked via non-unary [operators](<Operator.md> "Operator"). An operand may be 

  * a literal value of any type,
  * a [variable](<Variable.md> "Variable") or [constant](<Constant.md> "Constant") referred to by its [identifier](<Identifier.md> "Identifier"), or
  * a [function](<Function.md> "Function") call.



Examples of expressions are: 

  * `x + 5`
  * `'Z'`
  * `response` [`<>`](<Not_equal.md> "Not equal") `42`



Expressions, and parts thereof, can be classified by their result type. Usually primarily arithmetic and logic expressions are distinguished. An arithmetic expression results in a numeric value. A logic expression results in a [Boolean value](<Boolean.md> "Boolean"). 

## remarks

With [compiler directive](<Compiler_directive.md> "Compiler directive") [`{$extendedSyntax on}`](<$extendedSyntax.md> "$extendedSyntax") a function call as an expression can appear as a [statement](<statement.md> "statement"), too. This is useful if the function triggers side-effects, but the return value is not needed. 

## see also

  * [article „expression“](<https://en.wikipedia.org/wiki/Expression_\(computer_science\)>) in Wikipedia, the free online encyclopedia
  * Tutorial: [Boolean expressions](<Boolean_Expressions.md> "Boolean Expressions")
  * Tutorial: [How to use `tFPExpressionParser`](<How_To_Use_TFPExpressionParser.md> "How To Use TFPExpressionParser") (if an expression entered by the user shall be interpreted)

---

_Source: [https://wiki.freepascal.org/expression](https://web.archive.org/web/20250121211829/https://wiki.freepascal.org/expression)_
