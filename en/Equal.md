# Equal

│ **English (en)** │  **[suomi (fi)](</Equal/fi> "Equal/fi")** │  **[français (fr)](</Equal/fr> "Equal/fr")** │  **[русский (ru)](<../ru/Equal.md> "Equal/ru")** │    
****

=

The symbol `=` (pronounced “equal”) is used to 

  * compare two (comparable) values for equality,
  * [constant](<Constant.md> "Constant") declarations (including [`resourcestring` sections](</index.php?title=Resourcestring&action=edit&redlink=1> "Resourcestring \(page does not exist\)")), and
  * [type declarations](<Type.md> "Type"), as well as
  * specifying [default values](<Default_parameter.md> "Default parameter") for formal parameters in method declarations.



Furthermore, if [enumeration data types](<Enum_Type.md> "Enum Type") are defined with explicit indices, [FPC](<FPC.md> "FPC")’s Delphi compatibility modes require an equal sign instead of [`:=`](<Becomes.md> "Becomes") when specifying the indices. 

## Contents

  * 1 application
  * 2 comparitive remarks
  * 3 ASCII value
  * 4 see also



## application
    
    
    program equalDemo(input, output, stderr);
    
    const
    	answer = 42;
    
    resourcestring
    	prompt = 'What''s the answer?';
    
    type
    	number = longint;
    
    var
    	i: number;
    
    begin
    	writeLn(prompt);
    	readLn(i);
    	
    	if not (i = answer) then
    	begin
    		exitCode := 1;
    	end;
    end.
    

Be aware of _what_ you compare: When you compare two references, i.e. [`class`](<Class.md> "Class") or [`pointer`](<Pointer.md> "Pointer") variables, without dereferencing them first, you usually compare two memory addresses, _not_ the actual content at those addresses. Operator overloading may alter this behavior, though. For instance the content of [`ansistring`s](<Ansistring.md> "Ansistring") can be compared without any special treatment, though _internally_ (transparently) they are realized as pointers. 

Note, you usually do not use the equal sign comparison in conjunction with [floating-point numbers](<IEEE_754_formats.md> "IEEE 754 formats"). Instead one uses [`math.compareValue`](<https://www.freepascal.org/docs-html/rtl/math/comparevalue.html>). 

## comparitive remarks

Unlike other programming languages, the symbol is _not_ used to assign a value, for that the character pair [`:=`](<Becomes.md> "Becomes") is used. An exception of this are “initialized variables”, where you specify an initial value inside the [`var` section](<Var.md> "Var") alongside your [variable](<Variable.md> "Variable") declarations: 
    
    
    program initializedVariable(input, output, stderr);
    
    var
    	i: longint;
    	response: string = 'Wrong!';
    
    begin
    	writeLn('What''s the answer?');
    	readLn(i);
    	
    	if i = 42 then
    	begin
    		response := 'Right!';
    	end;
    	
    	writeLn(response);
    end.
    

Also, a comparison _always_ results in a [`boolean` value](<Boolean.md> "Boolean"). 

## ASCII value

In [ASCII](<ASCII.md> "ASCII"), the character code decimal `61` (or [hexadecimal](<Hexadecimal.md> "Hexadecimal") `3D`) is defined to be `=` (equals sign). 

## see also

  * [becomes-operator `:=`](<Becomes.md> "Becomes") assigns values to a variable
  * [not-equal-operator `<>`](<Not_equal.md> "Not equal") checks for inequality



  


navigation bar: topic: Pascal symbols  single characters |  [`+` (plus)](<Plus.md> "Plus") • [`-` (minus)](<Minus.md> "Minus") • [`*` (asterisk)](<_.md> "*") • [`/` (slash)](<Slash.md> "Slash")   
`=` (equal) • [`>` (greater than)](<Greater_than.md> "Greater than") • [`<` (less than)](<Less_than.md> "Less than")   
[`.` (period)](<period.md> "period") • [`:` (colon)](<Colon.md> "Colon") • [`;` (semi colon)](</;> ";")   
[`^` (hat)](<^.md> "^") • [`@` (at)](<@.md> "@")   
[`$` (dollar sign)](<Dollar_sign.md> "Dollar sign") • [`&` (ampersand)](<&.md> "&") • [`#` (hash)](</index.php?title=Hash&action=edit&redlink=1> "Hash \(page does not exist\)")   
[`'` (single quote)](<'.md> "'")  
---|---  
character pairs |  [`<>` (not equal)](<Not_equal.md> "Not equal") • [`<=` (less than or equal)](<Less_than_or_equal.md> "Less than or equal") • [`:=` (becomes)](<Becomes.md> "Becomes") • [`>=` (greater than or equal)](<Greater_than_or_equal.md> "Greater than or equal") • [`><` (symmetric difference)](<symmetric_difference.md> "symmetric difference") • [`//` (double slash)](<Slash.md> "Slash")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Equal](https://web.archive.org/web/20240920204217/https://wiki.freepascal.org/Equal)_
