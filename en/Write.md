# Write

│ **[Deutsch (de)](</Write/de> "Write/de")** │  **English (en)** │  **[español (es)](</Write/es> "Write/es")** │  **[русский (ru)](<../ru/Write.md> "Write/ru")** │    
****The procedures` write` and `writeLn` store a date in a [`text`](<Text.md> "Text") or typed [file](</File> "File"). They are defined as part of the [Pascal](<Standard_Pascal.md> "Standard Pascal") programming language, thus everyone can expect them to work no matter which [compiler](<Compiler.md> "Compiler") is used. 

In [`property`](</Property> "Property") definitions the [reserved word](<Reserved_word.md> "Reserved word") `write` is used to direct write access. This article deals with the procedures `write` and `writeLn`. See [`object`](<Object.md> "Object") and related articles for the occurrence of `write` in the context of properties. 

## Contents

  * 1 Behavior
    * 1.1 Signature
    * 1.2 Execution
    * 1.3 Representation
    * 1.4 Difference between write and writeLn
  * 2 See also



## Behavior

### Signature

`Write` as well as `writeLn` share almost the same identical formal signature. However a formal signature is omitted here, since you can not state their signatures in Pascal. Therefore a description follows: 

As an optional first parameter a `text` variable can be specified where data are written to. `Write` is additionally capable of writing to a [typed `file` variable](<typed_files.md> "typed files") (`file of recordType`). If no destination is specified, [`output`](<Output.md> "Output") is assumed. 

Thereafter any number of variables can be specified, but in the case of `write` at least one has to be present. They have to be [`char`](<Char.md> "Char"), [`integer`](<Integer.md> "Integer"), [`real`](<Real.md> "Real"), [`string`](<String.md> "String"), or any other data type that can be rendered as (a sequence of) character(s) via implicit typecasts ([operator overloading](<Operator_overloading.md> "Operator overloading")). In the case of typed files as destination, only variables of the file’s record type can be specified. 

If the destination is a `text` file, each data variable identifier may be followed by a [colon](<Colon.md> "Colon") and a non-negative integer value. This [value specifies](<Basic_Pascal_Tutorial/Chapter_2/Formatting_output.md> "Basic Pascal Tutorial/Chapter 2/Formatting output") the minimum width in characters the representation of the respective variable will acquire. It will be padded with space characters, so it becomes right-justified. In [`{$mode ISO}`](<Mode_iso.md> "Mode iso") and [`{$mode extendedPascal}`](<Mode_extendedpascal.md> "Mode extendedpascal") this value specifies the _exact_ width of `Boolean`, `char` and `string` values, thus `'X':0` will emit nothing. 

Floating-point variables may have another colon and non-negative integer value followed, thus two in total, that will specify the number of decimal places after the decimal-period. Also, by the specifying the second format specifier, the default scientific notation is turned off. 

### Execution

Calling `write`/`writeLn` will write the variables’ values to the destination, and if a `text` variable is the destination, possibly convert them into a representation suitable for humans before doing so. 

If the destination file is not open, the [run-time error](<runtime_error.md> "runtime error") 103 “File not open” will stop program execution. This RTE may be converted to an [`eInOutErrorr`](<https://www.freepascal.org/docs-html/rtl/sysutils/einouterror.html>) exception if the [`sysUtils` unit](<sysutils.md> "sysutils") is included (via a [`uses`-clause](<Uses.md> "Uses")). However, if the destination file is `output`, no error may be raised at all. You simply will not see any output emitted by `write`/`writeLn` calls. 

### Representation

If the destination is a `text` file, all ordinal type arguments are converted to human-readable representation. Strings and characters are already considered to be human-readable regardless of their value, e. g. control characters will be written directly without conversion. 

Decimal representations of floating-point values may be rounded. All numerical types may be preceded by a negative sign, but a positive sign is never printed. 

`write` will try to convert the value of enumerated types into their canonical names. If such does not exist the [run-time error](<runtime_error.md> "runtime error") 107 “invalid enumeration” occurs. 
    
    
    program writeDemo(input, output, stderr);
    
    type
    	direction = (left, straightOn, right);
    
    var
    	heading: direction;
    begin
    	// 15 characters in total (including period)
    	//  6 places after period
    	//    rounded
    	writeLn(pi():15:6);
    	
    	heading := straightOn;
    	// heading as enumeration will be left-aligned
    	// but still use 15 characters
    	writeLn(heading:15, '.');
    end.
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The default style of formatting numbers may differ depending on whether [`{$mode ISO}`](<Mode_iso.md> "Mode iso") is chosen.

### Difference between `write` and `writeLn`

`writeLn` will automatically write a [line feed](<End_of_Line.md> "End of Line") after all other data variables (if any). This line feed is the one suitable for the platform the program runs on. Remember, the notion of “line” applies only for `text` files. 

## See also

  * [`system.write`](<https://www.freepascal.org/docs-html/rtl/system/write.html>) and [`system.writeLn`](<https://www.freepascal.org/docs-html/rtl/system/writeln.html>)
  * [Why use Pascal, § “the `readLn` and `writeLn` effect”](<Why_use_Pascal.md> "Why use Pascal")
  * [`read`](<Read.md> "Read") performs the opposite action
  * [`writeStr`](<WriteStr.md> "WriteStr") works like `write` but outputs into a string

---

_Source: [https://wiki.freepascal.org/Write](https://web.archive.org/web/20241209223122/https://wiki.freepascal.org/Write)_
