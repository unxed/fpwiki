# Frame

A **frame** (frequently also referred to as [block](<Block.md> "Block")) is a language construct grouping a (possibly empty) sequence of [statements](<statement.md> "statement") (or instructions in the case of `asm` frames). 

## available types

All frames but [`repeat … until`](<Repeat.md> "Repeat") are terminated by the word [`end`](<End.md> "End"). Frame types are distinguished by their corresponding opening words. 

  * [Pascal](<Pascal.md> "Pascal"): These frames expect Pascal statements or may contain other frames. 

[`begin`](<Begin.md> "Begin")
    This frame begins a (possibly empty) sequence of statements. In the context of [routine](<Routine.md> "Routine") definitions or a [`program`](<Program.md> "Program") it can delimit a [scope](<Scope.md> "Scope").
[`else`](<Else.md> "Else")/`otherwise`
    This frame surrounds a “catch-all”-alternative as part of a [`case` statement](<Case.md> "Case").
`repeat … until`
    `repeat` in conjunction with `until` is used to surround the loop body of a tail-controlled [loop](<Loops.md> "Loops"). It is the only frame type not ending with an `end`.
exception treatment
    If [exceptions](<Exceptions.md> "Exceptions") are supported in the current compiler mode, the following frames are available as well. These frames are in fact “double”-frames: They group _two_ sequences at once. Neither of them can be used independently (e. g. writing `finally … end;` _without_ a proper `try` is illegal). 

[`try`](<Try.md> "Try") … [`except`](<Except.md> "Except") …
    Use this to install exception handlers.
`try` … [`finally`](<Finally.md> "Finally") …
    Use this to ensure a certain code fragment is executed despite any thrown exceptions.

[`unit`](<Unit.md> "Unit") overhead
    

[`initialization`](<Initialization.md> "Initialization") … [`finalization`](<Finalization.md> "Finalization")
    This double-frame designates code being executed when the corresponding unit is loaded or unloaded. Either part of this frame is optional. This frame may also delimit a scope.
`begin`
    If there is no need for a `finalization` part, `initialization` _can_ be replaced by `begin`.
  * [Assembly language](<Assembly_language.md> "Assembly language"): Frames beginning with [`asm`](<Asm.md> "Asm") expect assembly language. In pure assembly routines, this kind of frame may delimit a scope, too. Note, you cannot nest other frames in `asm` frames.



## style

Although not mandatory, it is customary to indent all code surrounded by frame markers by one level. 
    
    
    try
    	openJar;
    except
    	throwATantrum;
    end;
    

Some styles add another indentation level for nested or subordinate frame markers per se. 
    
    
    if apples = oranges then
      begin
        protest;
        halt(123);
      end;
    

As you can see from the examples above, it is customary to put frame delimiters _isolated_ in their _own_ line. Exempt of this guideline are of course frames accepting or requiring additional clauses to be syntactically complete: 
    
    
    repeat
    	write('Enter a non-negative integer: ');
    	readLn(i);
    until i >= 0;
    
    
    
    asm
    	rdrand eax
    	mov n, eax
    end ['eax'];
    

## technical background

Frames frequently, but not always, turn up to be (conditional) `jmp` targets. Some compile-time optimizations require code to be structured in a certain way, frames setting boundaries for that.

---

_Source: [https://wiki.freepascal.org/Frame](https://web.archive.org/web/20250114053101/https://wiki.freepascal.org/Frame)_
