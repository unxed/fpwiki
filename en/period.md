# period

│ **English (en)** │

## Contents

  * 1 radix mark
  * 2 identifier scope selector
  * 3 range
  * 4 module end
  * 5 namespaces
  * 6 ASCII value



## radix mark

[Pascal](<Pascal.md> "Pascal") uses the `.` (dot) to separate the integer and fractional part in literal decimal integers. 
    
    
    myReal := 6.28318
    

At least one digit in front of the period is mandatory. A `0` integer part must not be omitted. 

Numbers noted in a non-decimal base can not be noted in that way. E. g. the numeric value “a half” can not be written as `%0.1` ([`%`](<Percent_sign.md> "Percent sign") being the prefix marking [binary numbers](<Binary_numeral_system.md> "Binary numeral system")). 

## identifier scope selector

For structured data types the dot separates the data structure identifier from its individual components, i. e. methods or data fields. 
    
    
    program recordDemo(input, output, stderr);
    
    uses
    	Linux;
    
    var
    	info: TSysInfo;
    begin
    	if sysInfo(@info) <> 0 then
    	begin
    		halt(1);
    	end;
    	
    	writeLn('uptime: ', info.uptime, ' seconds');
    	
    	with info do
    	begin
    		writeLn('total free: ', freeram, ' bytes');
    	end;
    end.
    

## range

Two consecutive dots `..` let you specify an ordinal [type sub-range](<subrange_types.md> "subrange types"). 
    
    
    type
    	signumCodomain = -1..1;
    

This is the same as [`math.TValueSign`](<https://www.freepascal.org/docs-html/rtl/math/tvaluesign.html>).

## module end

The main block of any module, i. e. [`program`](<Program.md> "Program"), [`unit`](<Unit.md> "Unit") or [`library`](<Library.md> "Library"), has to be closed with an [`end`](<End.md> "End") “dot”: 
    
    
    program hiWorld(input, output, stderr);
    
    begin
    	writeLn('Hi world!');
    end.
    

It can be seen as an adoption of natural (written) languages, where a full stop marks an end of a sentence. 

Anything else after the final `end.`, assuming syntactical correctness, will be ignored by the [compiler](<Compiler.md> "Compiler"). 

## namespaces

[Unit](<Unit.md> "Unit") names containing dots create [namespaces](<Namespaces.md> "Namespaces"). 

## ASCII value

In [ASCII](<ASCII.md> "ASCII"), the character code decimal `46` (or [hexadecimal](<Hexadecimal.md> "Hexadecimal") `2E`) is defined to be `.` (full stop). 

  


navigation bar: topic: Pascal symbols  single characters |  [`+` (plus)](<Plus.md> "Plus") • [`-` (minus)](<Minus.md> "Minus") • [`*` (asterisk)](<_.md> "*") • [`/` (slash)](<Slash.md> "Slash")   
[`=` (equal)](<Equal.md> "Equal") • [`>` (greater than)](<Greater_than.md> "Greater than") • [`<` (less than)](<Less_than.md> "Less than")   
`.` (period) • [`:` (colon)](<Colon.md> "Colon") • [`;` (semi colon)](</;> ";")   
[`^` (hat)](<^.md> "^") • [`@` (at)](<@.md> "@")   
[`$` (dollar sign)](<Dollar_sign.md> "Dollar sign") • [`&` (ampersand)](<&.md> "&") • [`#` (hash)](</index.php?title=Hash&action=edit&redlink=1> "Hash \(page does not exist\)")   
[`'` (single quote)](<'.md> "'")  
---|---  
character pairs |  [`<>` (not equal)](<Not_equal.md> "Not equal") • [`<=` (less than or equal)](<Less_than_or_equal.md> "Less than or equal") • [`:=` (becomes)](<Becomes.md> "Becomes") • [`>=` (greater than or equal)](<Greater_than_or_equal.md> "Greater than or equal") • [`><` (symmetric difference)](<symmetric_difference.md> "symmetric difference") • [`//` (double slash)](<Slash.md> "Slash")  
  
  
****

---

_Source: [https://wiki.freepascal.org/period](https://web.archive.org/web/20240920204054/https://wiki.freepascal.org/period)_
