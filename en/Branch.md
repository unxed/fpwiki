# Branch

│ **English (en)** │

A **branch** , also called **conditional** or **conditional statement** , is a [statement](<statement.md> "statement") that is executed depending on an [expression’s](<expression.md> "expression") value. Usually this expression depends on the [program’s](<Program.md> "Program") state. 

## Types of branches

[Pascal](<Pascal.md> "Pascal") knows two types of branches: 

  * [`if … then …`](<If_and_Then.md> "If and Then") conditional statements, and
  * [`case … of …`](<Case.md> "Case") statements.



They differ in how many alternatives can be named, how many “routes” could be taken. 

In an `if … then …` branch there exists exactly one alternative. It is either executed or not. In conjunction with the word `else` one can specify exactly _two_ “paths” with regard to program flow. Due to its binary nature, `if … then` requires a [Boolean expression](<Boolean_Expressions.md> "Boolean Expressions") to be evaluated. 

`Case … of` on the other hand is n-ary, where n is greater than one. Unlike `if` the supplied expression the program flow depends on can (and has to) be of _any_ integral type, e. g. an [integer](<Integer.md> "Integer") or [enumeration](<Enum_Type.md> "Enum Type") (in FreePascal also [strings](<String.md> "String") are legal). 

## Usage

`Case … of` branches should be considered where appropriate since they [aide compiler optimizations](<Case_Compiler_Optimization.md> "Case Compiler Optimization"). By using them [FPC](<FPC.md> "FPC") may re-arrange statements, the given alternatives, in order to optimize for speed or size. 

## See also

  * [conditional compilation](<Conditional_compilation.md> "Conditional compilation") and related compiler directives do _not_ create branches

---

_Source: [https://wiki.freepascal.org/Branch](https://web.archive.org/web/20250119211500/https://wiki.freepascal.org/Branch)_
