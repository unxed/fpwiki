# ;

│ **English (en)** │  **[suomi (fi)](</;/fi> ";/fi")** │  **[français (fr)](</;/fr> ";/fr")** │    
****

;

The **semicolon** `;` is used to 

  * conclude a [declaration](<Declaration.md> "Declaration"),
  * conclude a constant, `resourceString`, or [`type`](<Type.md> "Type") definition,
  * separate formal parameters in a routine signature,
  * separate a routine declaration from its attributes,
  * terminate the program header,
  * separate alternatives in variant records, and to
  * _separate_ statements, in contrast to other programming language where its purpose is to _terminate_ a statement.



## Contents

  * 1 statement separator
    * 1.1 necessity
    * 1.2 empty statement
  * 2 other remarks



## statement separator

### necessity

Since language constructs only in their entirety constitute statements, semicolons may not split their components. Most notably `;` cannot appear immediately before an [`else`](<Else.md> "Else") that is part of an [`if … then` branch](<If_and_Then.md> "If and Then"). However, `;` in front of an [`end`](<End.md> "End") usually is not necessary, but optional and it does not harm insert one anyway. 

As a demonstration, that a single semicolon can make the difference, consider the following listings: 
    
    
    	case c of
    		0: if false then c := 42;
    		else c := -1;
    	end;
    

If `c` is zero, it remains zero, but becomes `-1` otherwise. 
    
    
    	case c of
    		0: if false then c := 42
    		else c := -1;
    	end;
    

Here, `c` only becomes `-1` if it has been zero before. As a consequence, and general advice, always put everything in compound statements (i. e. embrace your statements by [`begin`](<Begin.md> "Begin") and `end`) where it is allowed, in order to mitigate such issues. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Using `otherwise` (an [Extended Pascal](<Extended_Pascal.md> "Extended Pascal") extension) instead of `else` in `case`-statements may prevent such mistakes.

### empty statement

In a sequence a semicolon without a preceding (qualified) statement indicates an **empty statement**. 

Historically empty statements were used in conjunction with [labels](<Label.md> "Label"). Originally labels can only defined where a statement exists. If for instance a whole list of statements had to be bypassed, but no qualified statement followed thereafter, the empty statement still provided the possibility. 

Also historically, [`case`-statements](<Case.md> "Case") had to list all possible values the selector variable theoretically could have. Now, if a value or range did not imply any action, yet had to be listed inside the `case`-statement in order to fulfill this requirement, an empty statement is the shortest possible way to implement the situation. 

## other remarks

In the [ASCII](<ASCII.md> "ASCII") character set the semicolon takes the value `59` ([hexadecimal](<Hexadecimal.md> "Hexadecimal") `$3B`). 

  


navigation bar: topic: Pascal symbols  single characters |  [`+` (plus)](<Plus.md> "Plus") • [`-` (minus)](<Minus.md> "Minus") • [`*` (asterisk)](<_.md> "*") • [`/` (slash)](<Slash.md> "Slash")   
[`=` (equal)](<Equal.md> "Equal") • [`>` (greater than)](<Greater_than.md> "Greater than") • [`<` (less than)](<Less_than.md> "Less than")   
[`.` (period)](<period.md> "period") • [`:` (colon)](<Colon.md> "Colon") • `;` (semi colon)   
[`^` (hat)](<^.md> "^") • [`@` (at)](<@.md> "@")   
[`$` (dollar sign)](<Dollar_sign.md> "Dollar sign") • [`&` (ampersand)](<&.md> "&") • [`#` (hash)](</index.php?title=Hash&action=edit&redlink=1> "Hash \(page does not exist\)")   
[`'` (single quote)](<'.md> "'")  
---|---  
character pairs |  [`<>` (not equal)](<Not_equal.md> "Not equal") • [`<=` (less than or equal)](<Less_than_or_equal.md> "Less than or equal") • [`:=` (becomes)](<Becomes.md> "Becomes") • [`>=` (greater than or equal)](<Greater_than_or_equal.md> "Greater than or equal") • [`><` (symmetric difference)](<symmetric_difference.md> "symmetric difference") • [`//` (double slash)](<Slash.md> "Slash")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Semicolon](https://web.archive.org/web/20250424204204/https://wiki.freepascal.org/Semicolon)_
