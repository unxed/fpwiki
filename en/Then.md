# If and Then

│ **English (en)** │  **[русский (ru)](<../ru/Then.md>)** │

The `if` [keyword](<Keyword.md> "Keyword") precedes a condition, must be followed by `then` and a [statement](<statement.md> "statement"). The statement may optionally be followed by [`else`](<Else.md> "Else") and another statement. This creates a binary [branch](<Branch.md> "Branch"). 

## Contents

  * 1 If then
    * 1.1 Multiple statements in if then branch
  * 2 Usage
  * 3 See also



## `If then`
    
    
    if condition
    	then true_statement
    else false_statement;
    

`condition` is a [`Boolean`](<Boolean.md> "Boolean") expression that evaluates to [`true`](<True.md> "True") xor [`false`](<False.md> "False"). `true_statement` is executed if `condition` evaluates to `true`. `false_statement` is executed if `condition` evaluates to `false`. A [compile-time error](<compile-time_error.md> "compile-time error") occurs if the type of `condition` does not evaluate to a `Boolean` value. 

### Multiple statements in `if then` branch

If you need two or more statements for `true_statement` or `false_statement`, enclose them within a [`begin`](<Begin.md> "Begin") … [`end`](<End.md> "End") frame (“compound statement”). 
    
    
    if boolean_condition then
    begin
    	statement_zero;
    	statement_one;
    	statement_two;
    end;
    

## Usage

In order to optimize for speed in an `if … then … else` branch, try to write your expression so that the `then`-part gets executed most often. This improves the rate of successful `jump` predictions. 

## See also

  * Official documentation: [Reference guide: § “The `If..then..else` statement”](<https://www.freepascal.org/docs-html/ref/refsu57.html>)
  * [Basic Pascal Tutorial/Chapter 3/IF](<Basic_Pascal_Tutorial/Chapter_3/IF.md> "Basic Pascal Tutorial/Chapter 3/IF"), Tao Yue, Basic Pascal Introduction
  * [If statement and semicolon](</;#If_statement_and_semicolon> ";")
  * [`case`](<Case.md> "Case")



  
**Keywords:** [begin](<Begin.md> "Begin") — [do](<Do.md> "Do") — [else](<Else.md> "Else") — [end](<End.md> "End") — [for](<For.md> "For") — [if](<If.md> "If") — [repeat](<Repeat.md> "Repeat") — [then](<Then.md> "Then") — [until](<Until.md> "Until") — [while](<While.md> "While")

---

_Source: [https://wiki.freepascal.org/Then](https://web.archive.org/web/20250123144341/https://wiki.freepascal.org/Then)_
