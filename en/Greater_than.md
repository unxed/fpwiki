# Greater than

│ **English (en)** │  **[русский (ru)](<../ru/Greater_than.md>)** │

>

In [ASCII](<ASCII.md> "ASCII"), the character code decimal `62` (or [hexadecimal](<Hexadecimal.md> "Hexadecimal") `3E`) is defined to be `>` (greater-than sign). 

## Contents

  * 1 comparison operator
    * 1.1 true greater than
    * 1.2 greater than or equal to
  * 2 bit shift operator
  * 3 template list



## comparison operator

### true greater than

The greater than symbol `>` is used to compare whether the value on the left side of the symbol, exceeds that of the value to the right side of the symbol. The [expression](<expression.md> "expression") results in a [`boolean` value](<Boolean.md> "Boolean"). 
    
    
     1program comparisonDemo(input, output, stderr);
     2
     3type
     4	size = (tiny, small, medium, large, huge);
     5
     6var
     7	n, m: longint;
     8	c, d: char;
     9
    10begin
    11	// of course, integers can be compared
    12	n :=  2;
    13	m := -2;
    14	
    15	if n > m then
    16	begin
    17		writeLn(n, ' > ', m);
    18	end;
    19	
    20	// also enumerative types can be ordered
    21	if huge > medium then
    22	begin
    23		writeLn('huge is greater than medium');
    24	end;
    25	
    26	// but the constants true and false are also comparable
    27	if true > false then
    28	begin
    29		writeLn('true is greater than false');
    30	end;
    31	
    32	// characters stay in a relation to each other, too
    33	// (result depends on the used character set)
    34	c := 'a';
    35	d := 'Z';
    36	
    37	if c > d then
    38	begin
    39		writeLn(c, ' > ', d);
    40	end;
    41end.
    

All ordinal types can be compared to values of the same type. Integers can be compared to any integer. 
    
    
     1program comparisonSignedDemo(input, output, stderr);
     2
     3var
     4	n: int64;
     5	m: qword;
     6
     7begin
     8	n := -1; // n stored as %111...111
     9	m :=  2; // m stored as %000...010
    10	
    11	// signed comparison
    12	if n > m then
    13	begin
    14		writeLn(n, ' > ', m);
    15	end;
    16	
    17	// "unsigned" comparison
    18	if qword(n) > m then
    19	begin
    20		writeLn('qword(', n, ') > ', m);
    21	end;
    22end.
    

### greater than or equal to

If the greater than symbol is followed by an [equal sign](<Equal.md> "Equal") `>=`, the comparison also results in [`true`](<True.md> "True") if both operands are equal to each other. 

## bit shift operator

Two consecutive greater-than signs `>>` act as the [`shr` operator](<Shr.md> "Shr"). 

## template list

In [generic type](<Generics.md> "Generics") definitions the template list is delimited by a closing `>`. 

  


navigation bar: topic: Pascal symbols  single characters |  [`+` (plus)](<Plus.md> "Plus") • [`-` (minus)](<Minus.md> "Minus") • [`*` (asterisk)](<_.md> "*") • [`/` (slash)](<Slash.md> "Slash")   
[`=` (equal)](<Equal.md> "Equal") • `>` (greater than) • [`<` (less than)](<Less_than.md> "Less than")   
[`.` (period)](<period.md> "period") • [`:` (colon)](<Colon.md> "Colon") • [`;` (semi colon)](</;> ";")   
[`^` (hat)](<^.md> "^") • [`@` (at)](<@.md> "@")   
[`$` (dollar sign)](<Dollar_sign.md> "Dollar sign") • [`&` (ampersand)](<&.md> "&") • [`#` (hash)](</index.php?title=Hash&action=edit&redlink=1> "Hash \(page does not exist\)")   
[`'` (single quote)](<'.md> "'")  
---|---  
character pairs |  [`<>` (not equal)](<Not_equal.md> "Not equal") • [`<=` (less than or equal)](<Less_than_or_equal.md> "Less than or equal") • [`:=` (becomes)](<Becomes.md> "Becomes") • [`>=` (greater than or equal)](<Greater_than_or_equal.md> "Greater than or equal") • [`><` (symmetric difference)](<symmetric_difference.md> "symmetric difference") • [`//` (double slash)](<Slash.md> "Slash")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Greater_than](https://web.archive.org/web/20230328021112/https://wiki.freepascal.org/Greater_than)_
