# $rangeChecks

The [FPC](<FPC.md> "FPC")’s [local compiler directive](<local_compiler_directives.md> "local compiler directives") **`{$rangeChecks}`** enables or disable range checks of ordinal data type expressions, the default being `{$rangeChecks off}`. The short form `{$R+}` and `{$R-}` is recognized too. 

## Contents

  * 1 range checks
    * 1.1 behavior
    * 1.2 eligibility
    * 1.3 placement
  * 2 caveats
  * 3 application



## range checks

A range check ensures a value is within the domain of the destination operand. As soon as an operand needs to be “squeezed” into a smaller range, the expression or statement is eligible to a range check. 

### behavior

If a range check fails, the [runtime error](<runtime_error.md> "runtime error") 201 “Range check error” is generated. Thus the code sample 
    
    
    	{$rangeChecks on}
    	x := y;
    

is _semantically_ equivalent to 
    
    
    	{$rangeChecks off}
    	x := y;
    	if (x < low(x)) or (x > high(x)) then
    	begin
    		runError(201);
    	end;
    

(The reported memory address at which the error occurred will differ and the compiler may produce optimized code.) 

### eligibility

Range checks can be performed 

  * in [assignments](<Becomes.md> "Becomes") (`:=`), and more specifically 
    * when passing actual parameters, or
    * indicating [array](<Array.md> "Array") indices, but also
  * when doing [typecasts](<Typecast.md> "Typecast").



For assignments involving constant expressions a range check is performed already at [compile-time](<compile-time_error.md> "compile-time error"), but is either a warning or error depending on the current setting. 

### placement

The FPC allows enabling and disabling range checks on a per-statement basis, while Delphi allows changing the setting only a per-routine level. 

## caveats

  * It is important to remember that in [Pascal](<Pascal.md> "Pascal") _arithmetic_ [expressions](<expression.md> "expression") are always evaluated on a computer’s “native” (signed) integer data type. If the destination operand of an arithmetic expression is, for instance a `ALUSInt`, this will never trigger a runtime error.
  * If range checks are meant to be performed when passing parameters, range checks must be enabled _at_ the call site.



## application

Range checks can incur a significant performance penalty. Therefore, the FPC disables range checks by default. However, at least _during_ development and prior releases of software, range checks should be enabled to find “obvious” programming mistakes. As a consequence of this practice, if a code fragment is _meant to_ exceed the permissible range, it should be documented like this: 
    
    
    	{$push}
    		{$rangeChecks off}
    		x := y;
    	{$pop}
    

Use `{$ifOpt}` if code needs to be different on the current range-check-setting: 
    
    
    	{$ifOpt R-} // or R+
    		…
    	{$endIf}

---

_Source: [https://wiki.freepascal.org/$rangeChecks](https://web.archive.org/web/20250324160454/https://wiki.freepascal.org/$rangeChecks)_
