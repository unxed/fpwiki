# Else

│ **English (en)** │  **[русский (ru)](<../ru/Else.md>)** │

` else` is a [reserved word](<Reserved_word.md> "Reserved word") which starts a fallback-branch if all other _named_ cases do not apply. It can occur in 

  * [`if … then … else`-branches](<If_and_Then.md> "If and Then"), and
  * [`case`-statements](<Case.md> "Case").



## Contents

  * 1 semantics
    * 1.1 comparative remarks
    * 1.2 nested if … then … else
  * 2 see also



## semantics

An `else`-branch obtains program flow, if no other condition has been met. It cannot be paired with an explicit [expression](<expression.md> "expression"), but depends on expressions stated at others place, so an `else` per se does not have a condition. In `if … then … else`-statements, instructions are executed to the following scheme: 
    
    
    if expression
    	then trueStatement
    	else falseStatement;
    

Where `falseStatement` is executed if `expression` evaluates to [`false`](<false_and_true.md> "false and true"). 

In `case`-statements an `else`-branch assumes program flow, if no `case`-labels matched `expression`. 
    
    
    case expression of
    	value0: action0;
    	value1: action1;
    	else action2;
    end;
    

Only if `expression` neither evaluates to `value0` nor `value1`, `action2` is executed. 

### comparative remarks

In [Pascal](<Pascal.md> "Pascal") there is no `elsif` or `elif`. However writing `else` and `if` back to back does not pose a problem. Note, that the second `if … then` constitutes on its own a single [statement](<statement.md> "statement"). The requirement that `else` is followed by a statement is therefore fulfilled. 
    
    
    if expression0 then
    begin
    	action0;
    end
    else if expression1 then
    begin
    	action1;
    end;
    

### nested `if … then … else`

`if … then … else` are prone to semantic errors if no compound statements by enclosing a block with [`begin`](<Begin.md> "Begin") and [`end`](<End.md> "End") are used. 
    
    
    if itIsMorning() then
    	if itIsAHoliday() then
    	begin
    		sleep;
    	end
    	else
    	begin
    		wakeUp;
    		dress;
    		brushTeeth;
    		…;
    	end
    else
    begin
    	…;
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** In front of an `else` as part of an `if … then … else`-statement no [semicolon](</;> ";") is permitted.

The reference guide explains, quote: 

> In nested `If.. then .. else` constructs, some ambiguity may [arise] as to which `else` statement pairs with which `if` statement. The rule is that the `else` [keyword](<Keyword.md> "Keyword") matches the first `if` keyword (searching backwards) not already matched by an `else` keyword. 

## see also

  * [“The `If..then..else` statement in “Free Pascal reference guide”](<https://www.freepascal.org/docs-html/ref/refsu56.html>)



  
**Keywords:** [begin](<Begin.md> "Begin") — [do](<Do.md> "Do") — else — [end](<End.md> "End") — [for](<For.md> "For") — [if](<If.md> "If") — [repeat](<Repeat.md> "Repeat") — [then](<Then.md> "Then") — [until](<Until.md> "Until") — [while](<While.md> "While")

---

_Source: [https://wiki.freepascal.org/Else](https://web.archive.org/web/20240701000000/https://wiki.freepascal.org/Else)_
