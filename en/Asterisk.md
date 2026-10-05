# *

│ **English (en)** │  **[suomi (fi)](</*/fi> "*/fi")** │  **[français (fr)](</*/fr> "*/fr")** │  **[русский (ru)](<../ru/_.md> "*/ru")** │    
****

*

## standard Pascal

The symbol `*`, pronounced “asterisk”, is used in [Pascal](<Pascal.md> "Pascal") to 

  * indicate multiplication of numbers, or
  * form the intersection of [sets](<Set.md> "Set").


    
    
    program asteriskDemo(input, output, stderr);
    
    type
    	day = (monday, tuesday, wednesday,
    		thursday, friday, saturday, sunday);
    
    var
    	i: longint;
    	n: real;
    	m: set of day;
    
    begin
    	// multiplication operator
    	i := 6 * 7;      // i becomes 42
    	n := 6.0 * 7.0;  // n becomes 42.0
    	
    	// intersection operator
    	m := [saturday, sunday] * [sunday, monday];
    	// m is now {sunday}
    end.
    

## exponentiation

Furthermore, in [FPC](<FPC.md> "FPC") the exponentiation operator consisting of two consecutive asterisks `**` exists. However, it is only [defined for variants](<https://www.freepascal.org/docs-html/rtl/system/.op-power-variant-ariant-ariant.html>) by the standard system unit, which requires a variant manager being installed. In order to actually use it with integers, one can [define](<Operator_overloading.md> "Operator overloading") it on their own: 
    
    
    program exponentiation(input, output, stdErr);
    
    // make operator overloading available
    {$mode objFPC}
    
    operator ** (const base: integer; const exponent: integer): integer;
    begin
    	if base <> 0 then
    	begin
    		result := trunc(exp(ln(base) * exponent));
    	end;
    end;
    
    begin
    	writeLn(2 ** 10); // will print 1024
    end.
    

For readily available overloads, the [`math`](<https://www.freepascal.org/docs-html/rtl/math/index.html>) and [`matrix` unit](<https://www.freepascal.org/docs-html/rtl/matrix/index.html>) can be [included](<Uses.md> "Uses"). 

## other appearances

In Pascal's years of childhood computer systems did not necessarily knew the [comment](<Comments.md> "Comments") delimiting characters opening and closing curly brace `{ }`. To make block comments available on such systems an alternative syntax, the bigramms `(*` and `*)` are allowed, too, but they can't be interchanged willynilly: `(*` _has_ to match a `*)`, and can _not_ match a `}` even though it is closing a block comment, too. 

Also, if C like operators were allowed by the compiler directive [`{$COperator on}`](</index.php?title=sCoperator&action=edit&redlink=1> "sCoperator \(page does not exist\)"), the short syntax for `i := i * n` reads `i *= n`. But by doing so, you leave the domain of Pascal. Your code technically, mathematically speaking becomes wrong. 

In [ASCII](<ASCII.md> "ASCII"), the character code decimal `42` (or [hexadecimal](<Hexadecimal.md> "Hexadecimal") `2A`) is defined to be `*`. 

  


navigation bar: topic: Pascal symbols  single characters |  [`+` (plus)](<Plus.md> "Plus") • [`-` (minus)](<Minus.md> "Minus") • `*` (asterisk) • [`/` (slash)](<Slash.md> "Slash")   
[`=` (equal)](<Equal.md> "Equal") • [`>` (greater than)](<Greater_than.md> "Greater than") • [`<` (less than)](<Less_than.md> "Less than")   
[`.` (period)](<period.md> "period") • [`:` (colon)](<Colon.md> "Colon") • [`;` (semi colon)](</;> ";")   
[`^` (hat)](<^.md> "^") • [`@` (at)](<@.md> "@")   
[`$` (dollar sign)](<Dollar_sign.md> "Dollar sign") • [`&` (ampersand)](<&.md> "&") • [`#` (hash)](</index.php?title=Hash&action=edit&redlink=1> "Hash \(page does not exist\)")   
[`'` (single quote)](<'.md> "'")  
---|---  
character pairs |  [`<>` (not equal)](<Not_equal.md> "Not equal") • [`<=` (less than or equal)](<Less_than_or_equal.md> "Less than or equal") • [`:=` (becomes)](<Becomes.md> "Becomes") • [`>=` (greater than or equal)](<Greater_than_or_equal.md> "Greater than or equal") • [`><` (symmetric difference)](<symmetric_difference.md> "symmetric difference") • [`//` (double slash)](<Slash.md> "Slash")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Asterisk](https://web.archive.org/web/20230322194538/https://wiki.freepascal.org/Asterisk)_
