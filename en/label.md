# Label

│ **English (en)** │

The `label` keyword is used for declaration of labels (markers for unconditional jumps using [`goto`](<Goto.md> "Goto") keyword) used further in the [`unit`](<Unit.md> "Unit")/[`program`](<Program.md> "Program"). It is a [reserved word](<Reserved_words.md> "Reserved words"). 

## Pascal

To declare a label (not defining it) `label` starts a section with (comma separated) identifiers: 
    
    
    program labelDemo(input, output, stderr);
    
    {$goto on}
    // familiarize the compiler with symbols
    // that are meant as 'goto' targets
    label
    	// the symbol 'done' becomes of "type" label
    	done;
    

You then define the label, that means associating a symbol with a statement and effectively in the compiled code with an address, by writing the previously declared label in a statement block followed by a colon: 
    
    
    var
    	n: qword;
    	x, sum, arithmeticMean: real;
    begin
    	n := 0;
    	sum := 0;
    	while true do
    	begin
    		readLn(x);
    		if x < 0 then
    		begin
    			// leave loop and continue at 'done'
    			goto done;
    		end;
    		sum := sum + x;
    		inc(n);
    	end;
    done:
    	arithmeticMean := sum / n;
    	writeLn('arithemetic mean = ', arithmeticMean);
    end.
    

Pascal imposes further restrictions on labels: Labels have to be associated with statements: Putting a label right before an [`end`](<End.md> "End") is not allowed. To bypass that, you can insert an empty instruction [`;`](</;> ";") right after the label definition. 

In addition to normal [identfiers](<Identifier.md> "Identifier"), labels may be non-negative integers. 

## Assembler

A `label` section is also required for jump targets in [`asm`-blocks](<Asm.md> "Asm"), unless the [`@`-address-operator](<@.md> "@") is used. 
    
    
    program sumExample(input, output, stderr);
    
    { iteratively calculates the sum over first n integers }
    function iterativeSumFirstNIntegers(const n: longword): qword;
    {$ifdef CPUX86_64} // ============= optimized implementation
    // assembler modifier appended to routine declaration
    assembler;
    // you have to familiarize the compiler with symbols
    // which are meant to be jump targets
    {$goto on}
    label
    	isfni_iterate;
    {$asmMode intel}
    asm
    	xor rax, rax // rax := 0
    	// ecx is used as counter by loop instruction
    	mov ecx, n   // ecx := n
    isfni_iterate:
    	add rax, qword(ecx) // rax := rax + ecx
    	loop isfni_iterate  // dec(ecx)
    	// if ecx <> 0 then goto isfni_iterate
    	
    	// the @result macro represents the functions return value
    	mov @result, rax // result := rax
    // note, a list of modified registers (here ['rax', 'ecx'])
    //       is ignored for pure assembler routines
    end;
    {$else} // ========================== default implementation
    var
    	i: longword;
    	x: qword;
    begin
    	x := 0; // mov rax, 0
    	for i := n downto 1 do // mov ecx, n
    	begin
    		x := x + i; // add rax, ecx
    	end; // loop isfni_iterate
    	iterativeSumFirstNIntegers := x; // mov @result, rax
    end;
    {$endif}
    
    // M A I N =================================================
    var
    	n: longword;
    begin
    	readLn(n);
    	writeLn(iterativeSumFirstNIntegers(n));
    end.
    

Of course in a production program, you would use an algorithm applying the formula `sum := (n * (n + 1)) div 2` (“Gaussian sum formula”). 

## See also

  * [`goto` command](<Goto.md> "Goto")
  * [`{$goto}` compiler directive](<sGoto.md> "sGoto")
  * [`system.setJmp`](<https://www.freepascal.org/docs-html/rtl/system/setjmp.html>)

---

_Source: [https://wiki.freepascal.org/label](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/label)_
