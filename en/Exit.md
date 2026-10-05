# Exit

│ **English (en)** │

The pseudo-[`procedure`](<Procedure.md> "Procedure") **[`exit`](<https://www.freepascal.org/docs-html/rtl/system/exit.html>)** immediately leaves the surrounding [block](<Block.md> "Block"). It is similar to [C](<Pascal_for_C_users.md> "Pascal for C users")’s `return`. 

## Contents

  * 1 behavior
    * 1.1 basic
    * 1.2 functions
    * 1.3 exceptions
  * 2 notes
  * 3 see also



## behavior

`Exit` is an [UCSD Pascal](<UCSD_Pascal.md> "UCSD Pascal") extension. As of FPC version 3.2.0 it is available in _all_ [compiler compatibility modes](<Compiler_Mode.md> "Compiler Mode"). 

### basic

Invoking `exit` has the same effect as a [`goto`](<Goto.md> "Goto") to an invisible [`label`](<Label.md> "Label") right before the block’s [`end`](<End.md> "End"). The `program`
    
    
     1program exitDemo(input, output, stdErr);
     2var
     3	i: integer;
     4begin
     5	readLn(i);
     6	if i = 0 then
     7	begin
     8		exit;
     9	end;
    10	writeLn(123 div i);
    11end.
    

is effectively identical to 
    
    
     1program exitDemo(input, output, stdErr);
     2label
     3	9999;
     4var
     5	i: integer;
     6begin
     7	readLn(i);
     8	if i = 0 then
     9	begin
    10		goto 9999;
    11	end;
    12	writeLn(123 div i);
    139999:
    14end.
    

Confer the respective [assembly language](<Assembly_language.md> "Assembly language") output. In a manner of speaking, using `exit` merely avoids the “taboo word” _`goto`_. 

### functions

_Inside_ a [`function`](<Function.md> "Function") definition `exit` optionally accepts one argument. This argument must be assignment-compatible to and defines the functions result before actually transferring control to the function’s call site. 
    
    
     3function ackermann(const m, n: ALUUInt): ALUUInt;
     4begin
     5	if m = 0 then
     6	begin
     7		exit(n + 1);
     8	end;
     9	if n = 0 then
    10	begin
    11		exit(ackermann(m - 1, 1));
    12	end;
    13	exit(ackermann(m - 1, ackermann(m, n - 1)));
    14end;
    

In this example the line 
    
    
    7		exit(n + 1);
    

is equivalent to 
    
    
    7		ackermann := n + 1;
    8		exit;
    

### exceptions

Any accompanying [`finally`](<Finally.md> "Finally") frame is executed before actually jumping to the block’s `end`. Consider the following example: 
    
    
     1program tryExitDemo(input, output, stdErr);
     2{$modeSwitch exceptions+}
     3begin
     4	try
     5		exit;
     6		writeLn('Try.');
     7	finally
     8		writeLn('Finally.');
     9	end;
    10	writeLn('Bye.');
    11end.
    

This `program` outputs _one_ line: 
    
    
    Finally.
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** `Exit` may not appear in a `finally` [frame](<Frame.md> "Frame").

## notes

  * `Exit` is a regular [identifier](<Identifier.md> "Identifier"). You _can_ redefine it. You can refer to its special meaning via the FQI (fully-qualified identifier) `system.exit` at any time. NB: `Exit` is compiler intrinsic and not actually defined in the [`system` `unit`](<System_unit.md> "System unit").
  * `Exit` can be implemented as an unconditional `jmp` instruction. In higher [optimization](<Optimization.md> "Optimization") levels it might get eliminated.
  * The [FPC](<FPC.md> "FPC") does not support _naming_ the block to leave like the [GNU Pascal](<GNU_Pascal.md> "GNU Pascal") Compiler does. FPC’s implementation will always leave the _closest containing_ block. GPC’s implementation allows to leave _any_ surrounding block by supplying the respective [routine](<Routine.md> "Routine")’s name as an argument.



## see also

  * [`break`](<Break.md> "Break") – leave a [loop](<Loops.md> "Loops")
  * [`halt`](</index.php?title=halt&action=edit&redlink=1> "halt \(page does not exist\)") – terminate an entire [`program`](<Program.md> "Program")
  * [`system.exitCode`](<https://www.freepascal.org/docs-html/rtl/system/exitcode.html>)

---

_Source: [https://wiki.freepascal.org/Exit](https://web.archive.org/web/20240701000000/https://wiki.freepascal.org/Exit)_
