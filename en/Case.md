# Case

│ **[Deutsch (de)](</Case/de> "Case/de")** │  **English (en)** │  **[español (es)](</Case/es> "Case/es")** │  **[suomi (fi)](</Case/fi> "Case/fi")** │  **[français (fr)](</Case/fr> "Case/fr")** │  **[русский (ru)](<../ru/Case.md> "Case/ru")** │    
****

The [reserved word](<Reserved_word.md> "Reserved word") `case` starts a clause where alternatives are chosen. 

## Contents

  * 1 structure
    * 1.1 case-statements
      * 1.1.1 comparative remarks
    * 1.2 variant part in records
  * 2 see also
  * 3 external references



## structure

The general structure of `case`-clauses looks like this: 
    
    
    case selector of
        caseValue0: <statement>;
        caseValue1,caseValue7: <statement>;
        caseValue10..caseValue20: <statement>;
        else <statements>;
    end;
    

The following constraints have to be met: 

  * The data type of `selector` has to be an ordinal type. [FreePascal](<FPC.md> "FPC") additionally allows [strings](<Character_and_string_types.md> "Character and string types").
  * All case values have to be the same data type as `selector` is.
  * They have to be constant expressions, i.e. known at compile-time.
  * All `case`-labels have to be mutually disjoint. For every discrete value there applies exactly one or no cases.
  * A `case`-clause has to have at least one `case`-label. Imperative `case`-statements can have at most one anonymous cases (at most one `else`/`otherwise`-branches).



The actual order of `case`-labels is not relevant. They may be ascending, descending, or mixed up, it does not matter. 

Originally, standard Pascal also set the constraint, that _all possible_ values of the selector have to have a corresponding match. This is no longer the case with modern compilers. 

`case` can have both, imperative and declarative meanings, depending on where it is written. The former are “statements”, whereas the latter start a “variant part” of records. 

### `case`-statements

`case`-statements are a concise way of writing [branches](<Branch.md> "Branch"). They suit best, where alternative paths are taken _exclusively_. They may contain an [`else`](<Else.md> "Else")-branch catching all cases that are not listed. 
    
    
    program asciiTest(input, output, stderr);
    
    var
    	c: char;
    
    begin
    	read(c);
    	case ord(c) of
    		// empty statement, so the control characters are not
    		// considered by the else-branch as non-ASCII-characters
    		0..$1F, $7F: ;
    		$20..$7E:
    		begin
    			writeLn('You entered an ASCII printable character.');
    		end;
    		else
    		begin
    			writeLn('You entered a non-ASCII character.');
    		end;
    	end;
    end.
    

Note, `case`-statements accept expressions as selector. 

Instead of writing `else` the word `otherwise` is allowed, too. This is an [Extended Pascal](<Extended_Pascal.md> "Extended Pascal") extension. 

While the same semantics can be achieved by consecutive [`if`](<If.md> "If")-[`then`](<Then.md> "Then")-branches, utilizing a [`case`-statement allows the code generator to optimize the branch selection](<Case_Compiler_Optimization.md> "Case Compiler Optimization"). 

#### comparative remarks

There is no “fall-through” as it is the case with other languages such as shell or C. In Pascal exactly one case matches, is processed, and program flow continues after the final [`end`](<End.md> "End") of the `case`-statement. The [`break`-statement](<Break.md> "Break") with its special meaning only appears in loops. 

### variant part in records

A [`record`](<Record.md> "Record") may contain a variant part. The `case`-selector has to be the name of a data type, but an identifier for accessing the current variant can be provided, too. 
    
    
    program variantRecordDemo(input, output, stderr);
    
    type
    	sex = (female, male);
    	clothingSize = record
    			// FIXED PART
    			shoulderWidth: word;
    			armLength: word;
    			bustGirth: word;
    			waistSize: word;
    			hipMeasurement: word;
    			// VARIABLE PART
    			case body: sex of
    				female: (
    					underbustMeasure: word;
    				);
    				male: (
    				);
    		end;
    begin
    end.
    

## see also

  * [“The `case` statement” in the “FreePascal reference manual”](<https://www.freepascal.org/docs-html/ref/refsu56.html>)
  * [“`case`” as part of the “Object Pascal introduction”](<Basic_Pascal_Tutorial/Chapter_3/CASE.md> "Basic Pascal Tutorial/Chapter 3/CASE")



## external references

  * [Tao Yue: “Learn Pascal!”. Chapter “case”](<https://lazarus-ccr.sourceforge.io/pascal/pas3cb.html>)

---

_Source: [https://wiki.freepascal.org/Case](https://web.archive.org/web/20250422000759/https://wiki.freepascal.org/Case)_
