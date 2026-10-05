# $boolEval

│ **English (en)** │ 

The Free Pascal compiler [compiler directive](<local_compiler_directives.md> "local compiler directives") `{$boolEval}` determines, whether short-circuit evaluation (also known as “lazy evaluation”) of [Boolean](<Boolean.md> "Boolean") expressions is performed. 

## Explanation

[Pascal](<Standard_Pascal.md> "Standard Pascal") defines, that terms of logical expressions are always evaluated from start to finish. However, in certain cases of expressions containing [`and`](<And.md> "And") or [`or`](<Or.md> "Or") further evaluation can be skipped, since the final result can be deduced from the intermediate result, thus saving computation time. This is similar to shell’s `&&` and `||`. But Pascal consciously decided against such behavior. Ratio: Expressions can consist of function calls, those returning a `boolean` value, and if their definitions trigger side-effects a short-circuit evaluation could prevent such being executed. That is contrary to Pascal’s paradigm, that the programmer is not required to know of (implicit) optimizations (that actually change a program’s behavior), but what she writes is what is being executed. The order one writes terms in is not supposed to matter, since [math]\displaystyle{ \land }[/math] and [math]\displaystyle{ \lor }[/math] are associative operators, too. 

The following two examples are semantically identical: 
    
    
    {$push}
    {$boolEval off} // enable lazy evaluation
    if itIsLate(now) and isTired(me) then
    {$pop}
    begin
    	brushTeeth;
    	goToBed;
    end;
    

But this requires the reader to _know_ `isTired` is possibly not called, if `itIsLate` returned [`false`](<false_and_true.md> "false and true"). Such implicit “removal” of code, that the order one writes expressions in is relevant, is depreciated by Pascal. Instead one writes: 
    
    
    if itIsLate(now) then
    begin
    	if isTired(me) then
    	begin
    		brushTeeth;
    		goToBed;
    	end;
    end;
    

By default the [FPC](<FPC.md> "FPC") uses shortcut Boolean evaluation. [Extended Pascal](<Extended_Pascal.md> "Extended Pascal"), which as of 2020 FPC plans to support one day, defines the additional logical operators `and_then` and `or_else` which do exactly this, lazy evaluation, but on the _explicit_ request of the programmer. 

## See also

  * [`{$B}` or `{$boolEval}`: complete Boolean evaluation in “Free Pascal programmer's guide”](<https://www.freepascal.org/docs-html/prog/progsu4.html>)
  * [`math.ifThen`](<https://www.freepascal.org/docs-html/rtl/math/ifthen.html>), the mock ternary operator that always computes both possible results.

---

_Source: [https://wiki.freepascal.org/$boolEval](https://web.archive.org/web/20250218064025/https://wiki.freepascal.org/$boolEval)_
