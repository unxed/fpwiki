# Colon

│ **English (en)** │  **[suomi (fi)](</Colon/fi> "Colon/fi")** │  **[français (fr)](</Colon/fr> "Colon/fr")** │  **[русский (ru)](<../ru/Colon.md> "Colon/ru")** │  **[中文（中国大陆） (zh_CN)](</Colon/zh_CN> "Colon/zh CN")** │    
****

:

  
The symbol `:`, pronounced “colon”, is used in [Pascal](<Pascal.md> "Pascal") in several ways: 

  * in an [identifier](<Identifier.md> "Identifier") declaration it separates the [type](<Type.md> "Type")
    * most notably in [`var`](<Var.md> "Var") sections
    * but also in [`const`](<Const.md> "Const") sections (in case of _explicitely_ typed constants)
    * [`procedure`](<Procedure.md> "Procedure") and [`function`](<Function.md> "Function") parameters have a type, too
    * and return values of functions (including operator overloads)
    * as well as [properties](</Property> "Property")
    * in general, everywhere where a new identifier associated with some memory, is introduced (also custom structured type definitions)
  * in [`case`](<Case.md> "Case") selectors it closes a match-list
  * in [label](<Label.md> "Label") definitions the colon seperates the instruction from the label name
  * also some routines, especially the [`Str`](<Str.md> "Str"), [`write`](<Write.md> "Write") and `writeLn` procedures accept further parameters via colon separated arguments
  * Offset / segment separation under DOS `Mem[$B800:$0000]`



The following example shows the most prevalent usage scenarios: 
    
    
    program colonDemo(input, output, stderr);
    
    procedure numberReport(const i: int64);
    begin
    	case i of
    		low(i)..-1:
    		begin
    			writeLn('Your number is negative. ☹');
    		end;
    		1..high(i):
    		begin
    			// right aligns to a width of 24 characters
    			writeLn(i:24);
    		end;
    		else
    		begin
    			writeLn('You''ve entered zero.');
    		end;
    	end;
    end;
    
    var
    	i: int64;
    
    begin
    	writeLn('Enter a number:');
    	readLn(i);
    	numberReport(i);
    end.
    

## other remarks

  * Colon appears in the assignment operator [`:=`](<Becomes.md> "Becomes").
  * In [ASCII](<ASCII.md> "ASCII") the character colon `:` has the value `58`.



  
  


navigation bar: topic: Pascal symbols  single characters |  [`+` (plus)](<Plus.md> "Plus") • [`-` (minus)](<Minus.md> "Minus") • [`*` (asterisk)](<_.md> "*") • [`/` (slash)](<Slash.md> "Slash")   
[`=` (equal)](<Equal.md> "Equal") • [`>` (greater than)](<Greater_than.md> "Greater than") • [`<` (less than)](<Less_than.md> "Less than")   
[`.` (period)](<period.md> "period") • `:` (colon) • [`;` (semi colon)](</;> ";")   
[`^` (hat)](<^.md> "^") • [`@` (at)](<@.md> "@")   
[`$` (dollar sign)](<Dollar_sign.md> "Dollar sign") • [`&` (ampersand)](<&.md> "&") • [`#` (hash)](</index.php?title=Hash&action=edit&redlink=1> "Hash \(page does not exist\)")   
[`'` (single quote)](<'.md> "'")  
---|---  
character pairs |  [`<>` (not equal)](<Not_equal.md> "Not equal") • [`<=` (less than or equal)](<Less_than_or_equal.md> "Less than or equal") • [`:=` (becomes)](<Becomes.md> "Becomes") • [`>=` (greater than or equal)](<Greater_than_or_equal.md> "Greater than or equal") • [`><` (symmetric difference)](<symmetric_difference.md> "symmetric difference") • [`//` (double slash)](<Slash.md> "Slash")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Colon](https://web.archive.org/web/20240920204106/https://wiki.freepascal.org/Colon)_
