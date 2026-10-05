# With

│ **[Deutsch (de)](</With/de> "With/de")** │  **English (en)** │  **[suomi (fi)](</With/fi> "With/fi")** │  **[русский (ru)](<../ru/With.md> "With/ru")** │    
****

The [reserved word](<Reserved_word.md> "Reserved word") `with` allows overriding the scope lookup routing for named scopes for the duration of one [statement](<statement.md> "statement"). 

## Routing

[Identifiers](<Identifier.md> "Identifier") are searched in the following order, until there is a hit: 

  1. current [block](<Block.md> "Block")
  2. enclosing block, if any
  3. the block enclosing the enclosing block, if any
  4. … (and so on)
  5. the most recently imported module, that means for instance the [unit](<Unit.md> "Unit") that appears at the end of the [`uses`-clause](<Uses.md> "Uses") list, if any
  6. the penultimate module that has been imported, if any
  7. … (and so on)
  8. the first imported module, that means for instance the first unit appearing in a `uses`-clause, if any
  9. additional automatically loaded units, for example, in a [`program`](<Program.md> "Program") if enabled, the [`heapTrc` unit](<heaptrc.md> "heaptrc") (see procedure [`loaddefaultunits` in `compiler/pmodules.pas`](<https://gitlab.com/freepascal.org/fpc/source/-/tree/release_3_2_0/compiler/pmodules.pas#L318-L412>) for a full list)
  10. the [system unit](<System_unit.md> "System unit") (unless implicit inclusion has been disabled)



## Override

The lookup order can be temporarily overridden with a `with`-clause. It looks like this: 
    
    
    	with namedScope do
    	begin
    		…
    	end;
    

This puts `namedScope` at the top of the routing. Identifiers are looked up in `namedScope` first, before other scopes are considered. 

`namedScope` may be 

  * the name of a [`unit`](<Unit.md> "Unit") that has previously been imported via a [`uses`-clause](<Uses.md> "Uses") in the current section
  * the name of a structured variable, that could have named members, i. e. 
    * a [`record`](<Record.md> "Record")
    * [`object`](<Object.md> "Object"), or
    * [`class`](<Class.md> "Class").



If multiple `with`-clauses ought to be nested, there is the short notation: 
    
    
    	with snakeOil, sharpTools do
    	begin
    		…
    	end;
    

which is equivalent to:
    
    
    	with snakeOil do
    	begin
    		with sharpTools do
    		begin
    			…
    		end;
    	end;
    

Note, [`begin`](<Begin.md> "Begin")-[`end`](<End.md> "End") are not part of the syntax, but `with` … [`do`](<Do.md> "Do") has to be followed by exactly _one_ statement. In practice this will always be a compound statement, though. 

## See also

  * [namespaces](<Namespaces.md> "Namespaces")

---

_Source: [https://wiki.freepascal.org/With](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/With)_
