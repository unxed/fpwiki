# Becomes

│ **English (en)** │  **[русский (ru)](<../ru/Becomes.md>)** │

:=

The character pair `:=`, that are a [colon](<Colon.md> "Colon") and an [equal sign](<Equal.md> "Equal") back to back, is pronounced as “becomes” and used by [Pascal](<Pascal.md> "Pascal") as the assignment operator.   


## Contents

  * 1 assignment
  * 2 handling of special data types
  * 3 syntax justification
  * 4 index specification
  * 5 see also



## assignment

For valid assignments `:=` is surrounded by a _single_ [variable](<Variable.md> "Variable") [identifier](<Identifier.md> "Identifier") on the left hand (possibly doing a [variable typecast](<Typecast.md> "Typecast")), and an [expression](<expression.md> "expression") evaluating to the [data type](<Data_type.md> "Data type") the variable is declared as on the right hand. 
    
    
    program assignmentDemo(input, output, stderr);
    
    const
    	diameter = 6;
    
    var
    	n: integer;
    	area: real;
    	givenName: string;
    
    begin
    	n := 42;
    	area := pi() * diameter;
    	givenName := 'Smith';
    	n := 1000 - n div 2;
    end.
    

Assignments to variables of a [subrange type](<subrange_types.md> "subrange types") should be handled with care. The [compiler](<Compiler.md> "Compiler") can only yield out-of-range errors for _[constant](<Constant.md> "Constant")_ expressions, i. e. not depending on any [run-time](<runtime.md> "runtime") data. With [`{$rangeChecks}`](</index.php?title=sRangechecks&action=edit&redlink=1> "sRangechecks \(page does not exist\)") enabled a [run-time error](<runtime_error.md> "runtime error") can be generated. 
    
    
    program assignmentRange(input, output, stderr);
    
    type
    	naturalNumber = 1..high(longword);
    
    var
    	n: naturalNumber;
    
    begin
    	{$rangechecks on}
    	n := 1;           // is OK
    	n := -42 + n;     // will cause RTE 201
    end.
    

## handling of special data types

Where simple data types like [`integer`s](<Integer.md> "Integer") and [`char`acters](<Char.md> "Char") are realized as `mov` instructions or alike, data types that require initialization and finalization such as [`ansistring`s](<Ansistring.md> "Ansistring") or [`class`es](<Class.md> "Class") need special care. 

The compiler will generate appropriate code _copying_ data for following data types. 

  * [`string`](<String.md> "String")
  * [`object`](<Object.md> "Object")
  * [`record`](<Record.md> "Record")



You don’t have to iterate over all components copying each element by hand as it is required in other programming languages. 

## syntax justification

The rationale of using two characters for assignment instead of just one, say the `=` sign, is to distinguish between assigning values and comparing for equality. It’s got its roots in the field of mathematics where a single equal sign is read as an expression, but has no imperative connotation. 

In comparison other languages than Pascal allow to write 
    
    
    n = m = x;
    

The semantics of this line of code varies among every programming language. For instance, in [Fortran](<Fortran.md> "Fortran") and in Basic this line means “Compare the values `m` and `x`, and if they are equal to each other `n` becomes `true`, `false` otherwise.” In contrast to that the C programming language will assign `m` the value of `x`, and subsequently assign the `n` the value of `m`, so `n` and `m` both have the value `x`. This sort of code has been a common source of errors, compilers nowadays even emit warnings when encountering multiple assignment in a single line. 

In Pascal the code excerpt above is illegal. Non-productive statements are not allowed, i. e. _something_ has to be _done_. 

## index specification

In declarations of enumerated data types, `:=` can be used to explicitly specify the ordinal value of a member. See [enumeration types § “Indices”](<Enum_Type.md> "Enum Type") for details. 

## see also

  * [single assignment](<Single_assignment.md> "Single assignment"), a compiler optimization



  


navigation bar: topic: Pascal symbols  single characters |  [`+` (plus)](<Plus.md> "Plus") • [`-` (minus)](<Minus.md> "Minus") • [`*` (asterisk)](<_.md> "*") • [`/` (slash)](<Slash.md> "Slash")   
[`=` (equal)](<Equal.md> "Equal") • [`>` (greater than)](<Greater_than.md> "Greater than") • [`<` (less than)](<Less_than.md> "Less than")   
[`.` (period)](<period.md> "period") • [`:` (colon)](<Colon.md> "Colon") • [`;` (semi colon)](</;> ";")   
[`^` (hat)](<^.md> "^") • [`@` (at)](<@.md> "@")   
[`$` (dollar sign)](<Dollar_sign.md> "Dollar sign") • [`&` (ampersand)](<&.md> "&") • [`#` (hash)](</index.php?title=Hash&action=edit&redlink=1> "Hash \(page does not exist\)")   
[`'` (single quote)](<'.md> "'")  
---|---  
character pairs |  [`<>` (not equal)](<Not_equal.md> "Not equal") • [`<=` (less than or equal)](<Less_than_or_equal.md> "Less than or equal") • `:=` (becomes) • [`>=` (greater than or equal)](<Greater_than_or_equal.md> "Greater than or equal") • [`><` (symmetric difference)](<symmetric_difference.md> "symmetric difference") • [`//` (double slash)](<Slash.md> "Slash")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Becomes](https://web.archive.org/web/20241212100507/https://wiki.freepascal.org/Becomes)_
