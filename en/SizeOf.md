# SizeOf

│ **English (en)** │  **[русский (ru)](<../ru/SizeOf.md>)** │

  
The [compile-time](<Compile_time.md> "Compile time") [function](<Function.md> "Function") [`sizeOf`](<https://www.freepascal.org/docs-html/rtl/system/sizeof.html>) evaluates to the size in Bytes of a given [data type](<Data_type.md> "Data type") name or [variable](<Variable.md> "Variable") [identifier](<Identifier.md> "Identifier"). 

`sizeOf` can appear in compile-time expressions, inside [compiler directives](<Compiler_directive.md> "Compiler directive"), too. 

## Contents

  * 1 usage
  * 2 comparative remarks
    * 2.1 dynamic arrays and alike
    * 2.2 classes
  * 3 see also



## usage

`sizeOf` is especially encountered in [assembly language](<Assembler.md> "Assembler") or when doing manual allocation of memory: 
    
    
    program sizeOfDemo(input, output, stderr);
    
    {$typedAddress on}
    
    uses
    	heaptrc;
    
    type
    	s = record
    		c: char;
    		i: longint;
    	end;
    
    var
    	x: ^s;
    
    begin
    	returnNilIfGrowHeapFails := true;
    	
    	getMem(x, sizeOf(x));
    	
    	if not assigned(x) then
    	begin
    		writeLn(stderr, 'malloc for x failed');
    		halt(1);
    	end;
    	
    	x^.c := 'r';
    	x^.i := -42;
    	
    	freeMem(x, sizeOf(x));
    end.
    

Direct handling of structured data types in assembly language requires awareness of data sizes, too: 
    
    
    program sizeOfDemo(input, output, stderr);
    
    type
    	integerArray = array of integer;
    
    function sum(const f: integerArray): int64;
    {$ifdef CPUx86_64}
    assembler;
    {$asmMode intel}
    asm
    	// ensure f is in a particular register
    	mov rsi, f                       // rsi := f  (pointer to an array)
    	
    	// check for nil pointer (i.e. empty array)
    	test rsi, rsi                    // rsi = 0 ?
    	jz @sum_abort                    // if rsi = nil then goto abort
    	
    	// load last index of array [theoretically there is highF]
    	mov rcx, [rsi] - sizeOf(sizeInt) // rcx := (rsi - sizeOf(sizeInt))^
    	
    	// load first element, since loop condition won't reach it
    	{$if sizeOf(integer) = 4}
    	mov eax, [rsi]                   // eax := rsi^
    	{$elseif sizeOf(integer) = 2}
    	mov ax, [rsi]                    // ax := rsi^
    	{$else} {$error unexpected integer size} {$endif}
    	
    	// we're done, if f doesn't contain any more elements
    	test rcx, rcx                    // rcx = 0 ?
    	jz @sum_done                     // if high(f) = 0 then goto done
    	
    @sum_iterate:
    	{$if sizeOf(integer) = 4}
    	mov edx, [rsi + rcx * 4]         // edx := (rsi + 4 * rcx)^
    	{$elseif sizeOf(integer) = 2}
    	mov dx, [rsi + rcx * 2]          // dx := (rsi + 2 * rcx)^
    	{$else} {$error unexpected scale factor} {$endif}
    	
    	add rax, rdx                     // rax := rax + rdx
    	
    	jo @sum_abort                    // if OF then goto abort
    	
    	loop @sum_iterate                // dec(rcx)
    	                                 // if rcx <> 0 then goto iterate
    	
    	jmp @sum_done                    // goto done
    	
    @sum_abort:
    	// load neutral element for addition
    	xor rax, rax                     // rax := 0
    	
    @sum_done:
    end;
    {$else}
    unimplemented;
    begin
    	sum := 0;
    end;
    {$endif}
    
    begin
    	writeLn(sum(integerArray.create(2, 5, 11, 17, 23)));
    end.
    

With [FPC](<FPC.md> "FPC") the size of an [`integer`](<https://www.freepascal.org/docs-html/rtl/system/integer.html>) depends on the used [compiler mode](<Compiler_Mode.md> "Compiler Mode"). However, `sizeOf(sizeInt)` was inserted for demonstration purposes only. In the `{$ifdef CPUx86_64}` branch `sizeOf(sizeInt)` is always `8`. 

## comparative remarks

### dynamic arrays and alike

Since [dynamic arrays](<Dynamic_array.md> "Dynamic array") are realized as pointers to a block on the heap, `sizeOf` evaluates to the pointer's size. In order to determine the array's size – of its data – `sizeOf` has to be used in conjunction with the function [`length`](<https://www.freepascal.org/docs-html/rtl/system/length.html>). 
    
    
    program dynamicArraySizeDemo(input, output, stderr);
    
    uses
    	sysUtils;
    
    resourcestring
    	enteredN = 'You''ve entered %0:d integers';
    	totalData = 'occupying a total of %0:d Bytes.';
    
    var
    	f: array of longint;
    
    begin
    	setLength(f, 0);
    	
    	while not eof() do
    	begin
    		setLength(f, length(f) + 1);
    		readLn(f[length(f)]);
    	end;
    	
    	writeLn(format(enteredN, [length(f)]));
    	writeLn(format(totalData, [length(f) * sizeOf(f[0])]));
    end.
    

The approach is the same for [ANSI strings](<Ansistring.md> "Ansistring") (depending on the `{$longstrings}` compiler switch state possibly denoted by [`string`](<String.md> "String"), too). Do not forget that dynamic arrays have management data in front of the referenced payload block. So if you _really_ want to know, how much memory has been reserved for one array, you would have to take the high (last index in array) and reference count fields into account, too. 

### classes

[Classes](<Class.md> "Class") as well are pointers. The class [`TObject`](<https://www.freepascal.org/docs-html/rtl/system/tobject.html>) provides the function [`instanceSize`](<https://www.freepascal.org/docs-html/rtl/system/tobject.instancesize.html>). It returns an object's size as it is determined by the class's type definition. Additional memory that's allocated by [constructors](<Constructor.md> "Constructor") or any [method](<Method.md> "Method"), is not taken into account. Note, that classes might contain dynamic arrays or ANSI strings, too. 

## see also

  * [article “`sizeOf`” in the English Wikipedia](<https://en.wikipedia.org/wiki/Sizeof>) (primarily about C’s unary operator)

---

_Source: [https://wiki.freepascal.org/SizeOf](https://web.archive.org/web/20250427095547/https://wiki.freepascal.org/SizeOf)_
