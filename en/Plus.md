# Plus

│ **English (en)** │  **[русский (ru)](<../ru/Plus.md>)** │

+

The symbol `+` (pronounced “plus”) is used to: 

  * explicitly indicate the positive sign of a number,
  * add two numbers resulting to a number,
  * form a union of [sets](<Set.md> "Set"),
  * (FPC) concatenate two strings (or characters; except [`pchar`](<PChar.md> "PChar")).
  * if `{$modeSwitch arrayOperators+}` (default in [`{$mode Delphi}`](<Mode_Delphi.md> "Mode Delphi")), concatenate arrays (since FPC 3.2.0)
  * enable a feature in a `{$modeSwitch}`


    
    
    program plusDemo(input, output, stderr);
    
    var
    	x: longint;
    	g: set of (foo, bar);
    	m: string;
    begin
    	// unary operator: positive sign
    	x := +7;                    // x becomes positive 7
    	x := +$100;                 // x becomes 256
    	                            // (dollar sign denotes hexadecimal base)
    	
    	// addition
    	x := 7 + 7;                 // x becomes 14
    	x := 7 + 7 + 7 + 7 + 7 + 7; // x becomes 42
    	
    	// union of sets
    	g := [foo] + [bar];         // g becomes [foo, bar]
    	
    	// concatenation of strings and/or characters (FPC/Delphi extension)
    	m := 'Hello ' + 'world!';   // m becomes 'Hello world!'
    end.
    

The plus sign is also a unary operator. One can write such stupid expressions as `++++++++++++++42` which will evaluate to positive 42. 

In [ASCII](<ASCII.md> "ASCII"), the character code decimal `43` (or [hexadecimal](<Hexadecimal.md> "Hexadecimal") `2B`) is defined to be `+` (plus sign). 

## see also

  * [`system.add`](<https://www.freepascal.org/docs-html/rtl/system/.op-add-variant-ariant-ariant.html>)
  * [`system.concat`](<https://www.freepascal.org/docs-html/rtl/system/concat.html>) returns concatenation of strings
  * [`strings.strcat`](<https://www.freepascal.org/docs-html/rtl/strings/strcat.html>) returns concatenation of [`pchar`](<PChar.md> "PChar") strings



  


navigation bar: topic: Pascal symbols  single characters |  `+` (plus) • [`-` (minus)](<Minus.md> "Minus") • [`*` (asterisk)](<_.md> "*") • [`/` (slash)](<Slash.md> "Slash")   
[`=` (equal)](<Equal.md> "Equal") • [`>` (greater than)](<Greater_than.md> "Greater than") • [`<` (less than)](<Less_than.md> "Less than")   
[`.` (period)](<period.md> "period") • [`:` (colon)](<Colon.md> "Colon") • [`;` (semi colon)](</;> ";")   
[`^` (hat)](<^.md> "^") • [`@` (at)](<@.md> "@")   
[`$` (dollar sign)](<Dollar_sign.md> "Dollar sign") • [`&` (ampersand)](<&.md> "&") • [`#` (hash)](</index.php?title=Hash&action=edit&redlink=1> "Hash \(page does not exist\)")   
[`'` (single quote)](<'.md> "'")  
---|---  
character pairs |  [`<>` (not equal)](<Not_equal.md> "Not equal") • [`<=` (less than or equal)](<Less_than_or_equal.md> "Less than or equal") • [`:=` (becomes)](<Becomes.md> "Becomes") • [`>=` (greater than or equal)](<Greater_than_or_equal.md> "Greater than or equal") • [`><` (symmetric difference)](<symmetric_difference.md> "symmetric difference") • [`//` (double slash)](<Slash.md> "Slash")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Plus](https://web.archive.org/web/20240920204117/https://wiki.freepascal.org/Plus)_
