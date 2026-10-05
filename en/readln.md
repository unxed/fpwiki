# Read

│ **English (en)** │


The procedures read and readLn retrieve a date from a [text](<Text.md> "Text") or typed [file](</File> "File"). They are defined as part of the [Pascal](<Standard_Pascal.md> "Standard Pascal") programming language. Everyone can expect them to work no matter which compiler has been used. 

In [property](</Property> "Property") definitions the [reserved word](<Reserved_word.md> "Reserved word") read is used to direct read access. This article deals with the procedures read and readLn. See [object](<Object.md> "Object") and related articles for the occurrence of read in the context of properties. 

## Contents

  * 1 behavior
    * 1.1 signature
    * 1.2 execution
    * 1.3 interpretation
    * 1.4 difference between read and readLn
  * 2 production usage
  * 3 see also



## behavior

### signature

Read as well as readLn share almost the same identical formal signature. However a formal signature is omitted here, since you can not write their signatures in Pascal. Therefore a description follows: As an optional first parameter a text variable can be specified where data are read from. Read is additionally capable of reading from a typed file variable (file of recordType). If no source is specified, [input](<Input.md> "Input") is assumed. Thereafter any number of variables can be specified, but at least one has to be present. They have to be either [char](<Char.md> "Char"), [integer](<Integer.md> "Integer"), [real](<Real.md> "Real"), or [string](<String.md> "String"). Earlier versions of [FPC](<FPC.md> "FPC") also allowed reading variables of the type [PChar](<PChar.md> "PChar"). This has been removed, since no buffer checking is possible with those. In the case of typed files as source, only variables of the file's record type can be specified. 

### execution

Calling read/readLn will place the read (and possibly accordingly interpreted) values to the given variables. 

The order of variables matters. For instance, when the following program 
    
    
    program readDemo(input, output, stderr);
    var
    	i: integer;
    	c: char;
    begin
    	readLn(i, c);
    end.

is supplied with 
    
    
    42 x

everything is fine. i will become 42 and c will become 'x'. But the reverse input order 
    
    
    x 42

will yield a [run-time error](</index.php?title=Run-time_error&action=edit&redlink=1> "Run-time error \(page does not exist\)") (in this case RTE 106). 

Once data are read and stored, they are “consumed”, thus cannot be retrieved otherwise, but via the variables only. However, data are read up to the variable's size limits. E.g. a fixed length string[24] will stop reading beyond the 24th character. 

Leading blanks in front of numeric types are skipped. 

If no data is available, possibly because the end of file has already been reached, default values for the remaining variables are loaded. 

### interpretation

Read and readLn are so powerful, because they interpret given data. For instance, a readLn storing an integer does not expect the binary value to be entered, but their decimal representation with ASCII numerals suffices (e.g. 42 instead of * [[asterisk](<_.md> "*") has the numeric value 42]). 

While char and string can be stored (sort of) directly, the numeric types integer and real are converted following certain rules. The rules are those, you normally write literals of such types within your [(Standard) Pascal](<Standard_Pascal.md> "Standard Pascal") source code. However, some compiler's read implementation (here FPC) allow additional formats: 

An integer's hexadecimal base can be indicated by prepending 0x, or just x (case insensitive) instead of the usual [$ (dollar sign)](<Dollar_sign.md> "Dollar sign"). 

### difference between read and readLn

ReadLn will in contrast to read consume a trailing [line feed](<End_of_Line.md> "End of Line"). It is discarded and does not have any influence on how to save supplied data. The read line ending is platform-independent. A line ending typical for Windows-platforms will be read and does not pose a problem, even if the program is run on [Linux](<Linux.md> "Linux") or any other platform. 

Note, the notion of “line” applies only for text files. Functions like [eoLn](<https://www.freepascal.org/docs-html/rtl/system/eoln.html>) and readLn only work on such files. In consequence readLn can not be used on typed files (file of recordType variables). 

## production usage

Read and readLn have a major drawback in that they expect the user to supply data in a given order. If they do not comply a run-time error will terminate the program. 

This is quite unsatisfactory, since a run-time error number won't enlighten the end user. You usually want to design your error messages in a way the user is capable in correcting her behavior. When reading ordinal types one can make use of the [val procedure](<Val.md> "Val"). 

  1. program readNumbers(input, output, stderr);

  2.  

  3. {$modeSwitch out+}

  4.  

  5. {**

  6. 	reads an integer from input

  7.  

  8. 	\param destination the variable to store the read value in

  9. 	\returns true if reading was successful

  10. *}

  11. function readLnInteger(out destination: integer): longbool;

  12. var

  13. 	/// temporarily stores input string

  14. 	userInput: ansistring;

  15. 	/// stores return code of val

  16. 	errorPosition: valSInt;

  17. begin

  18. 	readLn(userInput);

  19. 	val(userInput, destination, errorPosition);

  20.  

  21. 	// val is successful, if no character caused problems

  22. 	readLnInteger := errorPosition = 0;

  23.  

  24. 	if not readLnInteger then

  25. 	begin

  26. 		// set an arrow right below

  27. 		// the character causing troubles

  28. 		writeLn(space(errorPosition-1), '⇡');

  29. 	end;

  30. end;

  31.  

  32. { === M A I N ================================================ }

  33. var

  34. 	i: integer;

  35. begin

  36. 	repeat

  37. 	begin

  38. 		writeLn('Enter an integer:');

  39. 	end

  40. 	{$push}

  41. 	{$boolEval off} // lazy evaluation

  42. 	until eof() or readLnInteger(i);

  43. 	{$pop}

  44.  

  45. 	if eof() then

  46. 	begin

  47. 		halt(1);

  48. 	end

  49. end.




Beware, it is necessary to check for end of file. Unlike readLn no default value is loaded. 

Of course, it would be even better to catch wrong key strokes right when they are made, but this is not possible when utilizing read or readLn. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Therefore the main application of read or readLn is non-interactive programs reading (generated) data files.

## see also

  * [system.read](<https://www.freepascal.org/docs-html/rtl/system/read.html>) and [system.readLn](<https://www.freepascal.org/docs-html/rtl/system/readln.html>)
  * [Why use Pascal, § “the readLn and writeLn effect”](<Why_use_Pascal.md> "Why use Pascal")
  * [Secure programming](<Secure_programming.md> "Secure programming") regarding how to properly treat input 
  * [write](<Write.md> "Write") performing the opposite action

---

_Source: [https://wiki.freepascal.org/readln](https://web.archive.org/web/20190523043829/https://wiki.freepascal.org/readln)_
