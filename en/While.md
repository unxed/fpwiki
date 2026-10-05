# While

│ **[Deutsch (de)](</While/de> "While/de")** │  **English (en)** │  **[suomi (fi)](</While/fi> "While/fi")** │  **[français (fr)](</While/fr> "While/fr")** │  **[русский (ru)](<../ru/While.md> "While/ru")** │    
****

` while` in conjunction with [`do`](<Do.md> "Do") repeats a statement as long as a condition evaluates to [`true`](<True.md> "True"). The condition [expression](<expression.md> "expression") is evaluated prior each iteration, determining whether the following [statement](<statement.md> "statement") is executed. This is the main difference to a [`repeat … until`-loop](<Repeat.md> "Repeat"), where the [loop](<Loops.md> "Loops") body is executed at any rate, but succeeding iterations do not necessarily happen, though. 

The following example contains unreachable code: 
    
    
    program whileFalse(input, output, stderr);
    
    begin
    	while false do
    	begin
    		writeLn('never gets printed');
    	end;
    end.
    

You usually use `while`-loops where, in contrast to [`for`-loops](<For.md> "For"), a running index [variable](<Variable.md> "Variable") is not required, the statement executed can't be deduced from an index that's incremented by one, or to avoid a [`break`-statement](<Break.md> "Break") (which usually indicates bad programming style). 
    
    
    program whileDemo(input, output, stderr);
    
    var
    	x: integer;
    begin
    	x := 1;
    	
    	// prints non-negative integer powers of two
    	while x < high(x) div 2 do
    	begin
    		writeLn(x);
    		inc(x, x); // x := x + x
    	end;
    end.
    

## see also

  * [Infinite loop](<Infinite_loop.md> "Infinite loop")



  
**Keywords:** [begin](<Begin.md> "Begin") — [do](<Do.md> "Do") — [else](<Else.md> "Else") — [end](<End.md> "End") — [for](<For.md> "For") — [if](<If.md> "If") — [repeat](<Repeat.md> "Repeat") — [then](<Then.md> "Then") — [until](<Until.md> "Until") — while

---

_Source: [https://wiki.freepascal.org/While](https://web.archive.org/web/20250321011239/https://wiki.freepascal.org/While)_
