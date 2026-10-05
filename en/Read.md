# Read

│ [**Deutsch (de)**](</Read/de> "Read/de") │  **English (en)** │    
****

The procedures `read` and `readLn` retrieve data from a [`text` file](<Text.md> "Text") (such as `input`, the command line interface, or any file on disk). They are defined as part of the [Pascal](<Standard_Pascal.md> "Standard Pascal") programming language. Everyone can expect them to work no matter which compiler has been used. 

In [`property`](</Property> "Property") definitions the [reserved word](<Reserved_word.md> "Reserved word") `read` is used to direct read access. This article deals with the procedures `read` and `readLn`. See [`object`](<Object.md> "Object") and related articles for the occurrence of `read` in the context of properties. 

## Contents

  * 1 Behavior
    * 1.1 Signature
    * 1.2 Execution
    * 1.3 Interpretation
    * 1.4 Difference between read and readLn
  * 2 Production usage
  * 3 See also



## Behavior

### Signature

`Read` as well as `readLn` share almost the same identical formal signature. However a formal signature is omitted here, since you can not write their signatures in Pascal. Therefore a description follows: 

As an optional first parameter a `text` variable can be specified where data are read from. `Read` is additionally capable of reading from a [typed `file`](<typed_files.md> "typed files") variable (`file of recordType`). 

If no source is specified, [`input`](<Input.md> "Input") is assumed. Thereafter any number of variables can be specified, but at least one has to be present. They have to be either [`char`](<Char.md> "Char"), [`integer`](<Integer.md> "Integer"), [`real`](<Real.md> "Real"), or [`string`](<String.md> "String"). If you have specified a typed file as the source, all variables have to be of the file’s underlying base type. 

Earlier versions of [FPC](<FPC.md> "FPC") also allowed reading variables of the type [`PChar`](<PChar.md> "PChar"). This has been removed, since no buffer checking is possible with those. In the case of typed files as source, only variables of the [file’s](</File> "File") record type can be specified. 

### Execution

Calling `read`/`readLn` will place the read (and possibly accordingly interpreted) values to the given variables. 

The order of variables matters. For instance, when the following program: 
    
    
    program readDemo(input, output, stderr);
    var
    	i: integer;
    	c: char;
    begin
    	readLn(i, c);
    end.
    

is supplied with: 
    
    
    42 x
    

everything is fine. 

`i` will become `42` and `c` will become `'x'`. But the reverse input order 
    
    
    x 42
    

will yield a [run-time error](<runtime_error.md> "runtime error") (in this case RTE 106). 

Once data are read and stored, they are “consumed”, thus cannot be retrieved otherwise, but via the variables only. However, data are read up to the variable’s size limits. E. g. a fixed length `string[24]` will stop reading beyond the 24th character. 

Leading blanks in front of numeric types are skipped. 

If the source file is not open, the RTE 103 “file not open”, RTE 6 “invalid file handle” (for `input` in a non-[ISO compiler mode](<Mode_iso.md> "Mode iso")) will stop the program. These RTE may be converted to an [`eInOutError`](<https://www.freepascal.org/docs-html/rtl/sysutils/einouterror.html>) exception if the [`sysUtils` unit](<sysutils.md> "sysutils") is included (e. g. via a [`uses`-clause](<Uses.md> "Uses")). 

If the source file is open, but no data is available, possibly because the end of file has already been reached, default values for the remaining variables are loaded. 

### Interpretation

`Read` and `readLn` are so powerful, because they interpret given data. For instance, a `readLn` storing an integer does not expect the binary value to be entered, but their decimal representation with ASCII numerals suffices (e. g. `42` instead of `*` [[asterisk](<_.md> "*") has the numeric value 42]). 

While `char` and `string` can be stored (sort of) directly, the numeric types `integer` and `real` are converted following certain rules. The rules are those, you normally write literals of such types within your (Standard) Pascal source code. However, some compilers’ `read` implementation (here FPC) allow additional formats: 

An integer’s hexadecimal base can be indicated by prepending `0x`, or just `x` (case insensitive) instead of the usual [`$` (dollar sign)](<Dollar_sign.md> "Dollar sign"). 

### Difference between `read` and `readLn`

`ReadLn` will in contrast to `read` consume a trailing [line feed](<End_of_Line.md> "End of Line"). It is discarded and does not have any influence on how to save supplied data. The read line ending is platform-independent. A line ending typical for Windows-platforms will be read and does not pose a problem, even if the program is run on [Linux](<Linux.md> "Linux") or any other platform. 

Note, the notion of “line” applies only for `text` files. Functions like [`eoLn`](<https://www.freepascal.org/docs-html/rtl/system/eoln.html>) and `readLn` only work on such files. In consequence `readLn` can not be used on typed files (`file of recordType` variables). 

## Production usage

`Read` and `readLn` have a major drawback in that they expect the user to supply data in a given order. If users do not comply a run-time error will terminate the program. 

This is quite unsatisfactory, since a run-time error number won’t enlighten the end user. You usually want to design your error messages in a way the user is capable in correcting her behavior. When reading ordinal types one can make use of the [`val` procedure](<Val.md> "Val"). 
    
    
     1 program readNumbers(input, output, stderr);
     2 
     3 {$modeSwitch out+}
     4 
     5 {**
     6 	reads an integer from input
     7 	
     8 	\param destination the variable to store the read value in
     9 	\returns true if reading was successful
    10 	
    11 	This function will inform the user about any mistakes.
    12 *}
    13 function readLnInteger(out destination: integer): longbool;
    14 var
    15 	/// temporarily stores input string
    16 	userInput: ansistring;
    17 	/// stores return code of val
    18 	errorPosition: valSInt;
    19 begin
    20 	readLn(userInput);
    21 	
    22 	val(userInput, destination, errorPosition);
    23 	
    24 	// val is successful, if no character caused problems
    25 	readLnInteger := errorPosition = 0;
    26 	
    27 	// tell the user, what went wrong
    28 	if not readLnInteger then
    29 	begin
    30 		if length(userInput) < 1 then
    31 		begin
    32 			writeLn('Error: There was no input.');
    33 		end
    34 		else
    35 		begin
    36 			writeLn('Error: Could not parse your input as an integer. Your input was:');
    37 			writeLn(userInput);
    38 			
    39 			// set an arrow right below
    40 			// the character causing troubles
    41 			writeLn(space(errorPosition-1), '⇡');
    42 			writeLn('The character with an arrow underneath caused the troubles.');
    43 			
    44 			if length(userInput) > bitSizeOf(nativeInt) then
    45 			begin
    46 				writeLn('Maybe your value is too large.');
    47 			end;
    48 		end;
    49 	end;
    50 end;
    51 
    52 { === M A I N ================================================ }
    53 var
    54 	EOI: Boolean;
    55 
    56 function endOfInput(): Boolean;
    57 begin
    58 	EOI := system.eof();
    59 	endOfInput := EOI;
    60 end;
    61 
    62 var
    63 	i: integer;
    64 
    65 begin
    66 	repeat
    67 	begin
    68 		writeLn('Enter an integer:');
    69 	end
    70 	{$push}
    71 	{$boolEval off} // lazy evaluation [until or_else is supported]
    72 	until endOfInput() or readLnInteger(i) or endOfInput();
    73 	{$pop}
    74 	
    75 	if EOI then
    76 	begin
    77 		halt(1);
    78 	end;
    79 	
    80 	writeLn('Excellent choice!');
    81 end.
    

Beware, it is necessary to check, whether the end of file has been reached _before_ attempting to read data. The `text` file `input` may not be open. Unlike `readLn` _no_ default value is loaded. Hence it is imperative to check `val`’s `code` value in order to determine whether the destination variable `v` now has a legit value. 

Of course, it would be even better to catch wrong key strokes right when they are made, but this is not possible when utilizing `read` or `readLn`. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Therefore the main application of `read` or `readLn` is non-interactive programs reading (generated) data files.

Nevertheless, if the convenient interpretation functionality is desired, without having a file open, the procedure [`system.readStr`](<https://www.freepascal.org/docs-html/rtl/system/readstr.html>) can be used to do so. 

## See also

  * [`system.read`](<https://www.freepascal.org/docs-html/rtl/system/read.html>) and [`system.readLn`](<https://www.freepascal.org/docs-html/rtl/system/readln.html>)
  * [Why use Pascal, § “the `readLn` and `writeLn` effect”](<Why_use_Pascal.md> "Why use Pascal")
  * [Secure programming](<Secure_programming.md> "Secure programming") regarding how to properly treat input
  * [`write`](<Write.md> "Write") performing the opposite action

---

_Source: [https://wiki.freepascal.org/Read](https://web.archive.org/web/20210918012404/https://wiki.freepascal.org/Read)_
