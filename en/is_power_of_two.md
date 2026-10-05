# is power of two

Checking whether a given number is an integer power of two is a classical demonstration showing how conclusions can be drawn from a number’s internal representation. 

## integer
    
    
    uses
    	// for math.log2
    	math;
    

[`system.popCnt`](<https://www.freepascal.org/docs-html/rtl/system/popcnt.html>) counts the number of set bits within an integer. The function’s name derives from “population count.” _Population count_ , because [sets](<Set.md> "Set") are usually implemented by utilizing integers, where a set bit means a certain element is part of a set. 

`popCnt` applied on an integer representing a set will return its cardinality (what the `card` function defined by [Extended Pascal](<Extended_Pascal.md> "Extended Pascal") does). 
    
    
    (**
    	\brief Determines whether a given number
    	is an integer power of two.
    	
    	\param x the number to check
    	\return true iff \f$2^n=x\f$ where \f$n \in \mathbb{Z}\f$
    *)
    function isPowerOfTwo(const x: qword): longbool;
    begin
    	isPowerOfTwo := popCnt(x) = 1;
    end;
    

Now, you want to overload your function, since it is not necessarily the case you can limit your calculation to non-negative integers only. Ensure you are typecasting the given value, so the compiler chooses the right, our “base” function. 
    
    
    function isPowerOfTwo(const x: int64): longbool;
    begin
    	// a) 2^n is always positive.
    	// b) There is no 2^n that equals zero.
    	if x < 1 then
    	begin
    		isPowerOfTwo := false;
    	end
    	// c) Theoretically you could shortcut for x < 3
    	else
    	begin
    		isPowerOfTwo := isPowerOfTwo(qword(x));
    	end
    end;
    

## non-integer

Although integer operations are nice, sometimes you have to deal with floating point numbers. Since we wanna know the exponent in the expression [math]\displaystyle{ 2^n = x }[/math], and check it is an integer, we can use [`math.log2`](<https://www.freepascal.org/docs-html/rtl/math/log2.html>) in conjunction with [`system.frac`](<https://www.freepascal.org/docs-html/rtl/system/frac.html>). 
    
    
    function isPowerOfTwo(const x: float): longbool;
    begin
    	{$push}
    	{$boolEval off}
    	if (x <= 0) or isInfinite(x) or isNan(x) then
    	{$pop}
    	begin
    		isPowerOfTwo := false;
    	end
    	else
    	begin
    		isPowerOfTwo := frac(log2(x)) = 0;
    	end;
    end;
    

Alternatively one could use the following implementation, utilizing [`math.frExp`](<https://www.freepascal.org/docs-html/rtl/math/frexp.html>). It is even neater, since it does not really calculate the logarithm but just [extracts the numbers](<IEEE_754_formats.md> "IEEE 754 formats") (`fxtract` instruction on x86 platforms). However, (without writing [inline assembly](<Asm.md> "Asm") blocks) one needs to allocate two variables, where only one is actually needed. 
    
    
    function isPowerOfTwo(const x: float): longbool;
    var
    	mantissa: extended;
    	exponent: longint;
    begin
    	{$push}
    	{$boolEval off}
    	if isInfinite(x) or isNan(x) then
    	{$pop}
    	begin
    		isPowerOfTwo := false;
    	end
    	else
    	begin
    		frExp(x, mantissa, exponent);
    		isPowerOfTwo := mantissa = 0.5;
    	end;
    end;
    

## real `popcnt`

Note, despite its name the `system.popCnt` function does not simply use the `popcnt` [x86 instruction](<Assembly_language.md> "Assembly language"), because it is not available on all CPUs and availability has to be checked first. Instead, the generic implementation of `system.popCnt` uses a table-lookup which is guaranteed to work on all platforms and is still really fast. 

If you are checking millions or _billions_ of numbers whether they are a power of two, the following would be of course [on most CPUs] faster: 
    
    
    function isPowerOfTwo(const x: qWord): longBool;
    {$ifDef CPUx86_64}
    {$asmMode Intel}
    assembler;
    asm
    	// !!! you need to ensure popcnt is a legal instruction first
    	popcnt rax, x   // rax := # of set bits in x
    	dec al          //  al := al - 1;  ZF := al = 0
    	setz al         //  al := ZF
    end;
    {$else}
    begin
    	isPowerOfTwo := popCnt(x) = 1;
    end;
    {$endIf}
    

An alternative algorithm not relying on `popcnt`’s availability would be (for example): 
    
    
    function isPowerOfTwo(const x: qWord): longBool; assembler;
    asm
    	mov rax, x                  // rax := rdi
    	// Do not enter loop, because dividing zero will never emit a `1`.
    	test rax, rax               //  ZF := rax = 0
    	jz @is_power_of_zero_done   // if ZF then goto done
    	
    	// Repeated division by two: The first bit we've carried out (CF)
    	// must have also been the _only_ set bit in the entire word (ZF).
    @is_power_of_two_divide:
    	shr rax, 1                  // rax := rax div 2
    	jnc @is_power_of_two_divide // if not CF goto divide
    	
    	setz al                     //  al := ZF
    @is_power_of_zero_done:
    end;

---

_Source: [https://wiki.freepascal.org/is_power_of_two](https://web.archive.org/web/20250122120425/https://wiki.freepascal.org/is_power_of_two)_
