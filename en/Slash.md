# Slash

│ **[Deutsch (de)](</Slash/de> "Slash/de")** │  **English (en)** │  **[suomi (fi)](</Slash/fi> "Slash/fi")** │  **[français (fr)](</Slash/fr> "Slash/fr")** │  **[русский (ru)](<../ru/Slash.md> "Slash/ru")** │    
****

/

A single slash, surrounded by non-slash characters is regarded as the division operator. Two consecutive slashes are regarded as comment introducers. 

## Contents

  * 1 division
    * 1.1 related exceptions
  * 2 comment
  * 3 see also



## division

The ASCII slash `/` is used in a [Pascal](<Pascal.md> "Pascal") [program](<Program.md> "Program") to perform division (`∕` U+2215 “division slash”). The results are _always_ real values. If you want to perform integer division the [`div` operator](<Div.md> "Div") has to be used. 
    
    
    A := 3 / 4;
    

After this operation the [variable](<Variable.md> "Variable") `A` holds the value `0.75` (assuming `A` is declared as a real value [type](<Type.md> "Type"), otherwise the [compiler](<Compiler.md> "Compiler") generates an incompatible type error). 

### related exceptions

The value on the right side of the slash must not be zero, or a division by zero error occurs. In [modes](<Compiler_Mode.md> "Compiler Mode") where [exceptions](<Exceptions.md> "Exceptions") are available (e.g. [ObjFPC](<Mode_ObjFPC.md> "Mode ObjFPC") and [Delphi](<Mode_Delphi.md> "Mode Delphi") mode) this condition can be caught by using a [`try`](<Try.md> "Try") … [`except`](<Except.md> "Except") [frame](<Frame.md> "Frame"). Otherwise a [run-time error](<runtime_error.md> "runtime error") occurs (RTE 200). 
    
    
    program divZeroDemo(input, output, stderr);
    
    // ObjFPC mode for exceptions
    {$mode objfpc}
    
    uses
    	// make exception EDivByZero known
    	sysutils;
    
    const
    	dividend = 1.1;
    
    resourcestring
    	enterDivisorPrompt = 'Enter divisor:';
    	divisionOperationExceptionless = 'Division did not cause an exception.';
    	zeroDivisionFailure = 'Error: Attempted to divide by zero.';
    
    var
    	divisor, quotient: single;
    
    begin
    	writeLn(enterDivisorPrompt);
    	readLn(divisor);
    	
    	try
    		quotient := dividend / divisor;
    		writeLn(divisionOperationExceptionless);
    	except on EDivByZero do
    		writeLn(zeroDivisionFailure);
    	end;
    end.
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Exception handling is expensive. 

A plain test whether the user input is non-zero would have been in the above example more sophisticated.

## comment

Two slashes back to back introduce [comments](<Comments.md> "Comments") till the [end of line](<End_of_Line.md> "End of Line"). This is also known as “Delphi-style comment”. 
    
    
    while (buf^ in [' ', #9, #10]) do // kill separators
    

[example source](<https://svn.freepascal.org/cgi-bin/viewvc.cgi/tags/release_3_0_4/rtl/inc/system.inc?view=markup#l1345>)

## see also

  * [`round`](<Round.md> "Round")
  * [`trunc`](<Trunc.md> "Trunc")



  


navigation bar: topic: Pascal symbols  single characters |  [`+` (plus)](<Plus.md> "Plus") • [`-` (minus)](<Minus.md> "Minus") • [`*` (asterisk)](<_.md> "*") • `/` (slash)   
[`=` (equal)](<Equal.md> "Equal") • [`>` (greater than)](<Greater_than.md> "Greater than") • [`<` (less than)](<Less_than.md> "Less than")   
[`.` (period)](<period.md> "period") • [`:` (colon)](<Colon.md> "Colon") • [`;` (semi colon)](</;> ";")   
[`^` (hat)](<^.md> "^") • [`@` (at)](<@.md> "@")   
[`$` (dollar sign)](<Dollar_sign.md> "Dollar sign") • [`&` (ampersand)](<&.md> "&") • [`#` (hash)](</index.php?title=Hash&action=edit&redlink=1> "Hash \(page does not exist\)")   
[`'` (single quote)](<'.md> "'")  
---|---  
character pairs |  [`<>` (not equal)](<Not_equal.md> "Not equal") • [`<=` (less than or equal)](<Less_than_or_equal.md> "Less than or equal") • [`:=` (becomes)](<Becomes.md> "Becomes") • [`>=` (greater than or equal)](<Greater_than_or_equal.md> "Greater than or equal") • [`><` (symmetric difference)](<symmetric_difference.md> "symmetric difference") • [`//` (double slash)](<Slash.md> "Slash")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Slash](https://web.archive.org/web/20241206083057/https://wiki.freepascal.org/Slash)_
