# Function

│ **English (en)** │  **[русский (ru)](<../ru/Function.md>)** │

A **function** is a [routine](<Routine.md> "Routine") that, in contrast to [procedures](<Procedure.md> "Procedure"), returns a value. A call of a function is virtually substituted by its return value. If the [`{$extendedSyntax}` compiler switch](<$extendedSyntax.md> "$extendedSyntax") state is off, function calls can not appear as non-productive statements, but have to be or be part of an [expression](<expression.md> "expression"). 

The word `function` is a [reserved word](<Reserved_word.md> "Reserved word"). 

## Return value

In addition to a normal procedure, a function’s formal signature contains a return [type](<Data_type.md> "Data type"): The formal parameter list has to be succeeded by a [colon](<Colon.md> "Colon") and return type. For instance the following function returns a [`Boolean`](<Boolean.md> "Boolean"). 
    
    
    function myFunction(const firstParameter: real): Boolean;
    

When implementing functions there are several ways to define the function’s return value. 
    
    
    program functionDemo(input, output, stderr);
    
    {$mode objFPC}
    
    // traditional syntax:
    // the result is stored in the variable
    // its name is the same as the function's
    function myLine(const x: real): real;
    begin
    	myLine := 0.5 * x + 2;
    end;
    

If `{$modeswitch result+}`, which is set by [`{$mode objFPC}`](<Mode_ObjFPC.md> "Mode ObjFPC") and [`{$mode Delphi}`](<Mode_Delphi.md> "Mode Delphi"), inside the implementation block the special [identifier](<Identifier.md> "Identifier") `result` is available, too: 
    
    
    // using special `result` identifier
    function myParabola(const x: real): real;
    begin
    	result := sqr(x) - 1;
    end;
    

Additionally, in `{$mode objFPC}` the routine [`exit`](<https://www.freepascal.org/docs-html/rtl/system/exit.html>) will set the return value, too, _and_ leave the stack frame. In the previous two examples further statements could have appeared, and they would have been executed, whilst after an `exit` the routine is _done_. This is the behavior a `return` statement in C or other programming languages has. 
    
    
    // using exit routine
    function even(const x: longint): Boolean;
    begin
    	exit(not odd(x));
    end;
    

In [assembly language](<Assembly_language.md> "Assembly language") other rules apply. If the return type is an integral value, the accumulator register is used, provided it fits in there: 
    
    
    // in assembly language:
    // return type fits into a single register => use accumulator register
    function zero(const x: int64): Boolean;
    {$ifDef CPUx86_64}
    assembler; register;
    {$asmMode intel}
    asm
    	// xor modifies flags => put it in front of test
    	xor rax, rax     // rax := 0    (remove residue)
    	
    	test x, x        // x = 0 ?
    	setz al          // rax := ZF
    	
    	// When you examine the assembler output
    	// you will notice the compiler automatically inserts code
    	// that moves the contents of rax to the right spot on the stack,
    	// unless the noStackFrame hint
    	// (automatically set by some optimization levels)
    	// instructs the compiler to omit the stack frame if possible.
    end;
    {$else}
    begin
    	result := x = 0;
    end;
    {$endIf}
    

Otherwise, depending on which [`{$asmMode}`](</index.php?title=sAsmmode&action=edit&redlink=1> "sAsmmode \(page does not exist\)") is active, the `@result` (Intel) or `__result` (AT&T) macro can be used. 
    
    
    type
    	bodyAttributes = record
    		surfaceArea: real;
    		volume: real;
    	end;
    
    // in assembly language:
    // return type doesn't fit into accumulator => @result macro gives address
    function sphere(const radius: real): bodyAttributes;
    {$ifDef CPUx86_64}
    assembler;
    {$asmMode intel}
    const
    	three: longint = 3;
    	four: longint = 4;
    var
    	r: real;
    asm
    	pextrq r, radius, 0 // r := (@radius+0)^
    	lea rax, @result    // rax := @result
    	fld r               // radius
    	
    	fld st(0)           // radius radius
    	fild four           // 4 radius radius
    	fldpi               // pi 4 radius radius
    	fmul                // 4*pi radius radius
    	fxch                // radius 4*pi radius
    	fld st(0)           // radius radius 4*pi radius
    	fmul                // radius^2 4*pi radius
    	fmul                // 4*pi*radius^2 radius
    	fst [rax].bodyAttributes.surfaceArea
    	
    	fmul                // 4*pi*radius^3
    	fild three          // 3 4*pi*radius^3
    	fdivp               // 4/3*pi*radius^3
    	fst [rax].bodyAttributes.volume
    end;
    {$else}
    begin
    	sphere.surfaceArea := 4 * pi() * sqr(radius);
    	sphere.volume := 4 / 3 * pi() * sqr(radius) * abs(radius);
    end;
    {$endIf}
    

Originally Pascal expected _exact one_ [assignment](<Becomes.md> "Becomes") to the result variable (whichever is used). [FPC](<FPC.md> "FPC") however does not prohibit multiple assignments. It will emit a warning if none of the possible result identifiers were used or the exit routine is not written. 

> Warning
>     Function result does not seem to be set
>     You can get this warning if the [compiler](<Compiler.md> "Compiler") thinks that a function return value is not set. This will not be displayed for assembler procedures, or procedures that contain assembler blocks.
    
    
    begin
    	writeLn(sphere(2.0).surfaceArea);
    end.
    

Note, in the case of [operator overloads](<Operator_overloading.md> "Operator overloading"), a special form of a function, a result variable identifier has to be declared in the formal signature. See the article for details. 

## Remarks

[Pure functions](<Pure_functions.md> "Pure functions") are functions that do not depend on an external state. 

## See also

  * [Pascal basics](<Pascal_basics.md> "Pascal basics")
  * [§ “Using functions and procedures” in the Reference Guide](<https://www.freepascal.org/docs-html/ref/refch14.html>)
  * [§ “Intel 80x86 Inline assembler” in the Programmer’s Guide](<https://www.freepascal.org/docs-html/prog/progse10.html>)

---

_Source: [https://wiki.freepascal.org/Function](https://web.archive.org/web/20241004025955/https://wiki.freepascal.org/Function)_
