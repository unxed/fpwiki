# Inc and Dec

│ **English (en)** │

The [procedures](<Procedure.md> "Procedure") `inc` and `dec` increment or decrement a given variable by default by one. 

## Contents

  * 1 usage
  * 2 background
  * 3 special behaviors
  * 4 see also



## usage

The first parameter specifies an ordinal value variable (e. g. an [`integer`](<Integer.md> "Integer") or enumeration type) and the second optional parameter may specify a different addend/subtrahend. 
    
    
    program incDecDemo(input, output, stderr);
    
    type
    	primaryColor = (red, green, blue);
    
    var
    	phase: primaryColor;
    	x: longint;
    
    begin
    	// enumeration type
    	phase := red;
    	inc(phase); // phase becomes green
    	writeLn(phase);
    	
    	// integer
    	x := 1;
    	inc(x, -1); // x becomes zero
    	writeLn(x);
    	
    	x := 1;
    	dec(x); // same as above: x becomes zero
    	writeLn(x);
    end.
    

## background

In [FPC](<FPC.md> "FPC")’s [system unit](<System_unit.md> "System unit") the [`inc`](<https://www.freepascal.org/docs-html/rtl/system/inc.html>) and [`dec`](<https://www.freepascal.org/docs-html/rtl/system/dec.html>) procedures are compiler procedures. They exist in order to optimize for certain architectures where dedicated `inc` and `dec` [assembler](<Assembly_language.md> "Assembly language") instructions are available. 

## special behaviors

If `{$rangeChecks}` are turned on, those procedures may generate a run-time error (RTE 201). Also `{$overflowChecks}` may be generated (with a small difference in [TP mode](<Mode_TP.md> "Mode TP")). 

If [`pointer`](<Pointer.md> "Pointer") arithmetics are allowed by the `{$pointerMath}` compiler switch, `inc` and `dec` work on pointers, too. 

In the case of typed pointers, e. g. a pointer to a [`record`](<Record.md> "Record"), the target’s type size is considered automatically. For instance: 
    
    
    program pointerIncDemo(input, output, stderr);
    
    {$pointerMath on}
    
    var
    	p: PQWord;
    
    begin
    	inc(p);
    	inc(p, 3);
    end.
    

will generate (excerpt): 
    
    
    ; [pointerIncDemo.pas]
    ; [8] begin
    	leaq    -8(%rsp),%rsp
    .Lc3:
    ; Var p located in register rax
    	call    FPC_INITIALIZEUNITS
    	movq    $0,%rax
    ; [9] inc(p);
    	addq    $8,%rax
    ; [10] inc(p, 3);
    	addq    $24,%rax
    ; [11] end.
    	call    FPC_DO_EXIT
    	leaq    8(%rsp),%rsp
    	ret
    

## see also

  * [`succ`](<https://www.freepascal.org/docs-html/rtl/system/succ.html>) and [`pred`](<https://www.freepascal.org/docs-html/rtl/system/succ.html>)
  * [`for`-loops](<For.md> "For")

---

_Source: [https://wiki.freepascal.org/Inc_and_Dec](https://web.archive.org/web/20250324062313/https://wiki.freepascal.org/Inc_and_Dec)_
