# Div

│ **[Deutsch (de)](</Div/de> "Div/de")** │  **English (en)** │  **[español (es)](</Div/es> "Div/es")** │  **[suomi (fi)](</Div/fi> "Div/fi")** │  **[français (fr)](</Div/fr> "Div/fr")** │  **[русский (ru)](<../ru/Div.md> "Div/ru")** │    
****

` div` is division in which the fractional part (remainder) is discarded. The expression `a div b` returns the integer part of the result of dividing two integers. This is in contrast to the expression `a / b` which returns a [`real`](<Real.md> "Real") result. 

Both sides of an expression using `div` must be one of the integer types. Using a `real` operand with `div` will result in a compile-time error: 
    
    
     Error: Operator is not overloaded: […]
    

To get an integer result with a [`real`](<https://www.freepascal.org/docs-html/rtl/system/real.html>) operand, use [`trunc`](<https://www.freepascal.org/docs-html/rtl/system/trunc.html>) or [`round`](<https://www.freepascal.org/docs-html/rtl/system/round.html>) with the [`/` operator](<Slash.md> "Slash"). 

To demonstrate what `div` does, consider the following example: 
    
    
    program divDemo(input, output, stderr);
    
    var
    	i: shortInt;
    	j: shortInt;
    	q: qWord;
    	r: qWord;
    
    begin
    	i := 16;
    	j := 3;
    	q := 1000;
    	r := 300;
    	
    	writeLn(i div j);
    	writeLn(i / j);
    	writeLn(q div r);
    	writeLn(q / r);
    end.
    

outputs: 
    
    
    $ ./divDemo
    5
     5.3333333333333330E+000
    3
     3.3333333333333335E+000
    

## See also

  * [`mod`](<Mod.md> "Mod") – remainder of integer division
  * [`trunc`](<Trunc.md> "Trunc") – retrieve integer part of real numbers
  * [`round`](<Round.md> "Round") – round to integer
  * [`math.divmod`](<https://www.freepascal.org/docs-html/rtl/math/divmod.html>) – retrieve both quotient and remainder

---

_Source: [https://wiki.freepascal.org/Div](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/Div)_
