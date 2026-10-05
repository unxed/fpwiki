# Dollar sign

│ **English (en)** │  **[suomi (fi)](</Dollar_sign/fi> "Dollar sign/fi")** │  **[français (fr)](</Dollar_sign/fr> "Dollar sign/fr")** │  **[português (pt)](</Dollar_sign/pt> "Dollar sign/pt")** │  **[русский (ru)](<../ru/Dollar_sign.md> "Dollar sign/ru")** │    
****

$

In [ASCII](<ASCII.md> "ASCII"), the character code decimal `36` (or hexadecimal `24`) is defined to be `$` (dollar sign). 

## Pascal

As a PXSC-extension, the symbol `$`, pronounced “dollar sign”, is used to indicate a [hexadecimal](<Hexadecimal.md> "Hexadecimal") base.   

    
    
    program dollarSignDemo(input, output, stderr);
    
    var
    	i: longint;
    
    begin
    	// '$' as well as '0x' are recognized by readLn
    	// as hexadecimal base prefixes
    	readStr('$24', i);
    	writeLn(i);    // will print  36
    	writeLn(-$24); // will print -36
    end.
    

An optional sign is written in front the base indicator. 

## other appearances

For [FPC](<FPC.md> "FPC") and [Delphi](<Delphi.md> "Delphi") [compilers](<Compiler.md> "Compiler"), the dollar sign appears in [compiler directives](<Compiler_directive.md> "Compiler directive") of the form `{$directive}`. 

  


navigation bar: topic: Pascal symbols  single characters |  [`+` (plus)](<Plus.md> "Plus") • [`-` (minus)](<Minus.md> "Minus") • [`*` (asterisk)](<_.md> "*") • [`/` (slash)](<Slash.md> "Slash")   
[`=` (equal)](<Equal.md> "Equal") • [`>` (greater than)](<Greater_than.md> "Greater than") • [`<` (less than)](<Less_than.md> "Less than")   
[`.` (period)](<period.md> "period") • [`:` (colon)](<Colon.md> "Colon") • [`;` (semi colon)](</;> ";")   
[`^` (hat)](<^.md> "^") • [`@` (at)](<@.md> "@")   
`$` (dollar sign) • [`&` (ampersand)](<&.md> "&") • [`#` (hash)](</index.php?title=Hash&action=edit&redlink=1> "Hash \(page does not exist\)")   
[`'` (single quote)](<'.md> "'")  
---|---  
character pairs |  [`<>` (not equal)](<Not_equal.md> "Not equal") • [`<=` (less than or equal)](<Less_than_or_equal.md> "Less than or equal") • [`:=` (becomes)](<Becomes.md> "Becomes") • [`>=` (greater than or equal)](<Greater_than_or_equal.md> "Greater than or equal") • [`><` (symmetric difference)](<symmetric_difference.md> "symmetric difference") • [`//` (double slash)](<Slash.md> "Slash")  
  
  
****
  *[PXSC]: Pascal extension for scientific computing

---

_Source: [https://wiki.freepascal.org/Dollar_sign](https://web.archive.org/web/20230320124716/https://wiki.freepascal.org/Dollar_sign)_
