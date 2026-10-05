# End

│ **[Deutsch (de)](</End/de> "End/de")** │  **English (en)** │  **[suomi (fi)](</End/fi> "End/fi")** │  **[français (fr)](</End/fr> "End/fr")** │  **[русский (ru)](<../ru/End.md> "End/ru")** │    
****

The [keyword](<Keyword.md> "Keyword") `end` terminates an entity. It appears at several occasions: 

  * to mark the end of a module, i.e. a [`program`](<Program.md> "Program"), [`unit`](<Unit.md> "Unit") or [`library`](<Library.md> "Library")
  * to conclude a [block](<Block.md> "Block") of statements or instructions respectively 
    * either started by [`begin`](<Begin.md> "Begin"), or
    * started by [`asm`](<Asm.md> "Asm")
  * to wrap up some language constructs: 
    * most prominently [`if … then … end`](<If_and_Then.md> "If and Then"), or
    * [`case`](<Case.md> "Case") … [`of`](<Of.md> "Of") … `end`, but also
    * [`try … except … finally … end`](</index.php?title=Try,_Except_and_Finally&action=edit&redlink=1> "Try, Except and Finally \(page does not exist\)")
  * to finish off certain [type](<Type.md> "Type") declarations, such as [`object`](<Object.md> "Object"), [`record`](<Record.md> "Record") and [`class`](<Class.md> "Class")
  * in [extended Pascal](<Extended_Pascal.md> "Extended Pascal") `to end do …` starts the definition of the [`finalization` part of a module](<Finalization.md> "Finalization")



For example: 
    
    
    procedure proc0;
    var
    	a, b: integer;
    begin
    	…
    end;
    

The `end` gloss is one of the exceptions to the rule that every statement must be followed by a [semicolon](<Semicolon.md> "Semicolon"). The statement immediately preceding an `end` does not require a semicolon. 

It is also used to end a Pascal module, in which case it is followed by a [period](<period.md> "period") rather than a semicolon (in the example below, the last semicolon is optional): 
    
    
    program proc1;
    var
    	SL: TStrings;
    begin
    	SL := TStringlist.create;
    	try
    		…
    	finally
    		SL.free;
    	end;
    end.
    

`end` is used to indicate the end of the unit: 
    
    
      unit detent;
      uses math;
     
      procedure delta(r:real);
     
      implementation
     
      procedure delta;
      begin
     
      ...
     
      end;
     
      ...
      (* Note: No corresponding '''begin''' statement *)
     
      end.
    

It also closes a [record](<Record.md> "Record"): 
    
    
     Type
       ExampleRecord = Record
                         Values: array [1..200] of real;
                         NumValues: Integer; { holds the actual number of points in the array }
                         Average: Real { holds the average or mean of the values in the array }
                       End;
    

  
**Keywords:** [begin](<Begin.md> "Begin") — [do](<Do.md> "Do") — [else](<Else.md> "Else") — end — [for](<For.md> "For") — [if](<If.md> "If") — [repeat](<Repeat.md> "Repeat") — [then](<Then.md> "Then") — [until](<Until.md> "Until") — [while](<While.md> "While")

---

_Source: [https://wiki.freepascal.org/End](https://web.archive.org/web/20240920204055/https://wiki.freepascal.org/End)_
