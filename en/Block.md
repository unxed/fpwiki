# Block

│ **English (en)** │ 

A **block** is a sequence of [declarations](<Declaration.md> "Declaration") followed by a sequence of [statements](<statement.md> "statement"). The declarations are optional. The sequence of statements can be empty, but at least the [`begin`](<Begin.md> "Begin")…[`end`](<End.md> "End")-[frame](<Frame.md> "Frame") has to be present (in [routines](<Routine.md> "Routine") [`asm`](<Asm.md> "Asm")…`end` is allowed, too). The key feature of a block is, that declarations are only valid while the statements are processed. This concept is known as [scope](<Scope.md> "Scope"). 

## Example

The following is a valid block: 
    
    
    const
    	foobar = -1;
    type
    	booleanArray = array of boolean;
    var
    	check: booleanArray;
    begin
    	check := booleanArray.create(true, false, true);
    end;
    

An indication, whether something constitutes a block, is, whether you can use it as part of a routine definition, as well as make a [`program`](<Program.md> "Program") out of it (syntactically; apart from the terminating dot). 

## See also

  * [block comments](<Comments.md> "Comments")

---

_Source: [https://wiki.freepascal.org/Block](https://web.archive.org/web/20240920204059/https://wiki.freepascal.org/Block)_
