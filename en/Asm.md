# Asm

│ **English (en)** │

The [reserved word](<Reserved_word.md> "Reserved word") `asm` starts a [frame](<Frame.md> "Frame") of inline [assembly](<Assembly_language.md> "Assembly language") code. 
    
    
    program asmDemo(input, output, stderr);
    
    // The $asmMode directive informs the compiler
    // which syntax is used in asm-blocks.
    // Alternatives are 'att' (AT&T syntax) and 'direct'.
    {$asmMode intel}
    
    var
    	n, m: longint;
    begin
    	n := 42;
    	m := -7;
    	writeLn('n = ', n, '; m = ', m);
    	
    	// instead of declaring another temporary variable
    	// and writing "tmp := n; n := m; m := tmp;":
    	asm
    		mov eax, n  // eax := n
    		// xchg can only operate at most on one memory address
    		xchg eax, m // swaps values in eax and at m
    		mov n, eax  // n := eax (holding the former m value)
    	// an array of strings after the asm-block closing 'end'
    	// tells the compiler which registers have changed
    	// (you don't wanna mess with the compiler's notion
    	// which registers mean what)
    	end ['eax'];
    	
    	writeLn('n = ', n, '; m = ', m);
    end.
    

In order to maintain portability between platforms (i.e. your code still compiles for many targets), while optimizing for specific targets, you want to set up [conditional compilation](<Conditional_compilation.md> "Conditional compilation"): 
    
    
    program sign(input, output, stderr);
    
    type
    	signumCodomain = -1..1;
    
    { returns the sign of an integer }
    function signum({$ifNDef CPUx86_64} const {$endIf} x: longint): signumCodomain;
    {$ifDef CPUx86_64} // ============= optimized implementation
    assembler;
    {$asmMode intel}
    asm
    	xor rax, rax                  // ensure result is not wrong
    	                              // due to any residue
    	
    	test x, x                     // x ≟ 0
    	setnz al                      // al ≔ ¬ZF
    	
    	sar x, 63                     // propagate sign-bit through reg.
    	cmovs rax, x                  // if SF then rax ≔ −1
    end;
    {$else} // ========================== default implementation
    begin
    	// This is what math.sign virtually does.
    	// The compiled code requires _two_ cmp instructions, though. 
    	if x > 0 then
    	begin
    		signum := 1;
    	end
    	else if x < 0 then
    	begin
    		signum := -1;
    	end
    	else
    	begin
    		signum := 0;
    	end;
    end;
    {$endIf}
    
    // M A I N =================================================
    var
    	x: longint;
    begin
    	readLn(x);
    	writeLn(signum(x));
    end.
    

As you can see, you can implement whole routines in assembly language, by adding the `assembler` modifier and writing `asm` instead of `begin` for the implementation block. 

  


## see also

general 

  * [The inline assembler parser](<The_inline_assembler_parser.md> "The inline assembler parser")
  * [Lazarus inline assembler](<Lazarus_inline_assembler.md> "Lazarus inline assembler")
  * [`label` § “assembler”](<Label.md> "Label")



relevant compiler directives 

  * [`{$asmMode}`](</index.php?title=$asmMode&action=edit&redlink=1> "$asmMode \(page does not exist\)")
  * [`{$goto}`](</index.php?title=$goto&action=edit&redlink=1> "$goto \(page does not exist\)")
  * [`{$stackframes}`](</index.php?title=$stackFrames&action=edit&redlink=1> "$stackFrames \(page does not exist\)")



special tasks 

  * [hardware access](<Hardware_Access.md> "Hardware Access")
  * [AVR programming](<AVR_Programming.md> "AVR Programming")

---

_Source: [https://wiki.freepascal.org/Asm](https://web.archive.org/web/20250408214119/https://wiki.freepascal.org/Asm)_
