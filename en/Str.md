# Str

│ **English (en)** │  **[русский (ru)](<../ru/Str.md>)** │

The [`procedure`](<Procedure.md> "Procedure") [`str`](<https://www.freepascal.org/docs-html/rtl/system/str.html>) converts an ordinal or [`real`](<Real.md> "Real") type value to a string representation thereof. It is a [UCSD Pascal](<UCSD_Pascal.md> "UCSD Pascal") extension that was picked up and generalized by [Borland Pascal](<Borland_Pascal.md> "Borland Pascal"). The [FreePascal compiler](<FPC.md> "FPC") supports it, too, as described here. 

## invocation

The formal signature of `str` cannot be written in [Pascal](<Pascal.md> "Pascal"), hence a description follows: 

`Str` requires two parameters. The first parameter has to be an ordinal or real type expression. The second parameter specifies the destination. It has to be a [variable](<Variable_parameter.md> "Variable parameter") of the type [`string`](<String.md> "String") (or an `array[dimension] of char`). 

The first parameter may optionally be followed by a [colon](<Colon.md> "Colon") and an [integer](<Integer.md> "Integer") expression. This will determine the minimum width of the generated string representation. However, bear in mind if the destination variable is a _fixed_ -length string, no _more_ characters than its maximum capacity can be stored. In this case any surplus characters will be clipped. 

If the first parameter is a `real` expression, the parameter may be followed by another colon and integer after the first colon and integer specifying the minimum width. This number, however, will determine the number of decimal places after the radix mark. 

## formatting

Specifying a second formatting for the `real` number representation will disable the usual scientific notation. 

Enumerated type values will be left-justified, numeric values will be right-justified. 

`Str` cannot produce decimal numbers containing a [comma](<Comma.md> "Comma") as a [radix mark](<DecimalSeparator.md> "DecimalSeparator"). 

`Str` only produces numbers to the base of ten. [FPC’s](<FPC.md> "FPC") standard [run-time library](<RTL.md> "RTL") provides the functions [`binStr`](<https://www.freepascal.org/docs-html/rtl/system/binstr.html>), [`octStr`](<https://www.freepascal.org/docs-html/rtl/system/octstr.html>), and [`hexStr`](<https://www.freepascal.org/docs-html/rtl/system/hexstr.html>), which produce string representations of integers according to base of two, eight and sixteen respectively. `Real` values cannot be easily represented in other bases. 

## see also

  * the standardized [`writeStr` `procedure`](<WriteStr.md> "WriteStr") which does very same operation, but accepts an “infinite” number of arguments
  * [`val`](<Val.md> "Val") which performs the reverse operation
  * [String operations](<https://BorlandPascal.FanDom.com/wiki/String_operations>) in _BorlandPascal Fandom Wiki_

---

_Source: [https://wiki.freepascal.org/Str](https://web.archive.org/web/20240427202349/https://wiki.freepascal.org/Str)_
