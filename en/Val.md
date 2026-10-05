# Val

│ [**Deutsch (de)**](</Val/de> "Val/de") │  **English (en)** │  [**русский (ru)**](<../ru/Val.md> "Val/ru") │    
****The[procedure](<Procedure.md> "Procedure") [`system.val`](<https://www.freepascal.org/docs-html/rtl/system/val.html>) attempts to convert a string representation of a numeric value into a numeric value variable. It is part of the default [run-time library](<RTL.md> "RTL") delivered with the [FreePascal compiler](<FPC.md> "FPC"), but otherwise not standardized. 

## usage

The formal signature reads:
    
    
    procedure val(const s: string; var v; var code: word)
    

`S` is a [string](<String.md> "String")-type expression; it must be a sequence of characters that form a (possibly signed) number or enumerated type value. `V` is an ordinal or [real](<Real.md> "Real")-type variable. `Code` is a [word](<Word.md> "Word") integer variable. 

If the string `s` can not be converted to a numeric value, `code` holds the index of the first character causing troubles; otherwise `code` is zero, indicating success. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** In contrast to `read`, the value of `v` remains unchanged if no data (i.e. empty string `s`) is available. No `default` value is assigned.

The procedure `val` is so useful, since it does not trigger [run-time errors](<runtime_error.md> "runtime error") or [exceptions](<Exceptions.md> "Exceptions"). Unlike [`read`](<Read.md> "Read") the user can be supplied with more useful error messages telling which character is not a numeric symbol. The power of `val` lies in its capability to also convert enumerated types _by their named values_. 
    
    
    program valDemo(input, output, stdErr);
    type
    	decision = (no, yes, maybe);
    var
    	s: string;
    	r: decision;
    	c: word;
    begin
    	writeLn('Are you OK?');
    	readLn(s);
    	val(s, r, c);
    	writeLn('So ', r, '.');
    end.
    

Note, as a general design principle this feature should not be used for user responses like this example shows, since it thwarts internationalization. Localization in general is not possible with `val`, most notably the decimal separator – in some regions a [comma](<Comma.md> "Comma"), in others a [period](<period.md> "period") – can not be customized; it is always a period, as it is standard in your [Pascal](<Standard_Pascal.md> "Standard Pascal") source code. However, `val` still may be useful for reading and parsing _generated_ data. 

## see also

  * [`system.str`](<Str.md> "Str") performs the reverse action
  * [RTTI](<RTTI.md> "RTTI")

---

_Source: [https://wiki.freepascal.org/Val](https://web.archive.org/web/20230326061938/https://wiki.freepascal.org/Val)_
