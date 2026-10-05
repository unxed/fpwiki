# loop instruction

│ **English (en)** │

Pascal has three types of loop instruction, one increments a variable, and the other two test a boolean value to see if it is true. One performs the test before executing the loop, the other two after executing it. 

## Contents

  * 1 Kinds
    * 1.1 For
    * 1.2 Repeat
    * 1.3 While



## Kinds

There are three kinds of loop statements, `FOR`, `REPEAT`, and `WHILE`. The `repeat` statements always execute the loop at least once, and the `while` statement executes it zero or more times. 

### For

The [For](<For.md> "For") statement sets a control variable to an initial value, then performs the loop. at the end of the loop, the control variable is either incremented by 1 or decremented by 1 (depending on whether the loop is increasing or decreasing), and if the value of the control variable is less than the `to` value specified (for increasing value loops), or the control variable value is greater than the `to` value (for decreasing value loops), the loop will be executed again. 

### Repeat

The [`repeat`](<Repeat.md> "Repeat") statement works similarly to the `for` statement. The repeat loop is executed, then a test condition is checked, and if the test result remains true, the loop is executed again. 

### While

The [`while`](<While.md> "While") statement tests a condition, and if true, the loop is executed. At the end of the loop, control returns back to the start to test if the condition is still true, in which case the loop is repeated. The Wile loop is the only one where the loop can conceivably be executed zero times, i.e. not executed at all.

---

_Source: [https://wiki.freepascal.org/loop_instruction](https://web.archive.org/web/20210118203745/https://wiki.freepascal.org/loop_instruction)_
