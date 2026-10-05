# Loops

│ **English (en)** │

A **loop** control structure repeats a [statement](<statement.md> "statement") as long as a certain condition is met. 

## Contents

  * 1 properties
  * 2 types
  * 3 comparative remarks
  * 4 see also



## properties

A loop is sectioned into a 

  * loop head, and a
  * loop body.



The loop body is a statement, and the head (possibly implicitly) contains a [Boolean expression](<Boolean_Expressions.md> "Boolean Expressions") that determines whether the loop body is executed (again). Every time the statements in the loop body are executed, an _iteration_ occurs. 

Loops are particularly useful as a programming construct, since the loop body is only inserted once in the final program. The so-called “loop unrolling” [compiler](<Compiler.md> "Compiler") optimization may _copy_ the loop body multiple times anyway, but still you do not need to literally repeat the statements in your [source code](<Source_code.md> "Source code"). Some processors perform particularly well (fast) if a repeating series of instructions occupies a quite _small_ chunk of memory. 

## types

[Pascal](<Standard_Pascal.md> "Standard Pascal") defines 

  * counting loops [`for … to|downto do`](<For.md> "For")
  * conditional loops 
    * [`while … do`](<While.md> "While"), and
    * [`repeat … until`](<Repeat.md> "Repeat") where the loop head appears at the tail.



The [FPC](<FPC.md> "FPC") also supports [`for … in … do`](<for-in_loop.md> "for-in loop") loops, which are similar to counting loops. 

If the loop body has a predictable number of iterations, the loop can be written with any loop type, but a counting loop is usually the most reasonable choice. Likewise, conditional loops are interchangeable, too, but in any given situation either one is more suitable. 

Note that the value of the "for loop variable" is undefined after a loop has completed or if a loop is not executed at all. However, if the loop was terminated prematurely with an exception or a break (or even a goto statement), the loop variable retains the value it had when the loop was exited. 

## comparative remarks

Unlike in some programming languages, in Pascal a loop itself is a statement; it does not yield a value. Also, a loop body does not create a new [scope](<Scope.md> "Scope"). 

## see also

  * [Pascal basics](<Pascal_basics.md> "Pascal basics")
  * [Main Loop Hooks](<Main_Loop_Hooks.md> "Main Loop Hooks")

---

_Source: [https://wiki.freepascal.org/Loops](https://web.archive.org/web/20250114064731/https://wiki.freepascal.org/Loops)_
