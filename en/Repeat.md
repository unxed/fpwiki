# Repeat

│ **English (en)** │  **[русский (ru)](<../ru/Repeat.md>)** │

This [reserved words](<Reserved_word.md> "Reserved word") `repeat` in conjunction with `until` are used to create tail-controlled [loops](<Loops.md> "Loops"). 

## syntax

A tail-controlled loops start with `repeat`, followed by a possibly empty list of [statements](<statement.md> "statement"), and concluded by `until` and a `Boolean` [expression](<expression.md> "expression"). 

The following ([infinite](<Infinite_loop.md> "Infinite loop")) loops demonstrate the syntax: 
    
    
    // empty loop body is legal:
    repeat
    until false;
    

Note, `repeat` and `until` already form a frame in their own right. You do not need to surround your statements by an extra [`begin`](<Begin.md> "Begin") … [`end`](<End.md> "End")-frame. In fact, you can put as many sequences (also called _compound statements_) between `repeat` and `until` as you want: 
    
    
    repeat
    begin
    	write('x');
    end;
    begin
    	write('o');
    end;
    until false;
    

Note, it is not necessary, but allowed to put a [semicolon](</;> ";") prior `until`: 
    
    
    repeat
    	write('zZ')
    until false;
    

## semantics

Since the loop “head” appears at the tail, the loop body is executed at least once and the loop condition evaluated at the _end_ of every iteration. If the condition evaluates to [`false`](<false_and_true.md> "false and true"), another iteration occurs. 

Therefore, `repeat` … `until` loops are particularly useful to ensure a certain sequence of statements is run _at least_ once. 
    
    
    repeat
    	write('Enter a positive number: ');
    	readLn(i);
    	
    	// readLn loads a default value if the source is EOF.
    	// For integer values the default is zero.
    	// Since our loop condition requires _positive_ values,
    	// this loop would be stuck _indefinitely_ if EOF(input).
    	// Ergo, we check for that:
    	if eof(input) then
    	begin
    		writeLn;
    		writeLn(stdErr, 'error: input has reached EOF');
    		halt(1);
    	end;
    until i > 0;
    

The user will be prompted again and again, but _at least once_ , until he finally enters a positive number. 

Another standard usage example is the _reverse_ Horner scheme as demonstrated in [Base converting](<Base_converting.md> "Base converting"). 

## see also

  * [`while`](<While.md> "While") for _head_ -controlled loops
  * [`break`](<Break.md> "Break")
  * [`continue`](<Continue.md> "Continue")
  * Object Pascal Tutorial on [`repeat…until`](<Basic_Pascal_Tutorial/Chapter_3/REPEAT..md> "Basic Pascal Tutorial/Chapter 3/REPEAT..UNTIL")



  
**Keywords:** [begin](<Begin.md> "Begin") — [do](<Do.md> "Do") — [else](<Else.md> "Else") — [end](<End.md> "End") — [for](<For.md> "For") — [if](<If.md> "If") — repeat — [then](<Then.md> "Then") — [until](<Until.md> "Until") — [while](<While.md> "While")

---

_Source: [https://wiki.freepascal.org/Repeat](https://web.archive.org/web/20240304145403/https://wiki.freepascal.org/Repeat)_
