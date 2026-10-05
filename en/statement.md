# statement

│ **English (en)** │

Statements are parts of a [program](<Executable_program.md> "Executable program") that alter its state, e. g. by changing a [variable’s](<Variable.md> "Variable") value. 

## classification

One may distinguish between simple and structured statements. 

Elementary statements are those, which can not be broken down into smaller pieces – “sub-statements”. These are 

  * [assignments](<Becomes.md> "Becomes"),
  * [procedure calls](<Routine.md> "Routine"),
  * [`raise`](<Raise.md> "Raise") control transfer, and
  * [`goto` jumps](<Goto.md> "Goto").



Complex statements are 

  * compound statements (also called _sequence_) delimited by [`begin`](<Begin.md> "Begin") and [`end`](<End.md> "End"),
  * inline assembler blocks delimited by [`asm`](<Asm.md> "Asm") and `end`,
  * [branches](<Branch.md> "Branch"), and
  * [loops](<Loops.md> "Loops").



## remarks

In contrast to statements, instructions are the building blocks in low-level [assembly language](<Assembly_language.md> "Assembly language"). 

[Empty statements](</;> ";") are not statements in a formal sense, since they merely exist to satisfy syntax requirements in a non-superfluous manner. 

## see also

  * [block](<Block.md> "Block")
  * [expression](<expression.md> "expression")
  * [Chapter “Statements” in the _Free Pascal reference guide_](<https://www.freepascal.org/docs-html/ref/refch13.html>)

---

_Source: [https://wiki.freepascal.org/statement](https://web.archive.org/web/20240920204216/https://wiki.freepascal.org/statement)_
