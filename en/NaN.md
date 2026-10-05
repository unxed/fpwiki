# NaN

│ **English (en)** │  **[suomi (fi)](</NaN/fi> "NaN/fi")** │  **[русский (ru)](<../ru/NaN.md> "NaN/ru")** │    
****

` NaN` (not a number) is a numeric data type value representing an undefined or unrepresentable value. These values result from operations which have undefined numerical results. `NaN` is not the same as infinity. 
    
    
    program NotANumber(input, output, stderr);
    begin
    	// writes 'Nan' (with spacing) on its own line
    	writeLn(0/0);
    end.
    

Note, `NaN` exists only in the context of floating point number calculations: `0 div 0` ([integer division](<Div.md> "Div")) is not allowed, though. 

## See also

  * [Value “is not a number”](<https://www.freepascal.org/docs-html/rtl/math/nan.html>)
  * [`IsNan`](<https://www.freepascal.org/docs-html/rtl/math/isnan.html>) checks whether value is “not a number”.
  * [`TAChart` documentation, § “skipping source items”](<TAChart_documentation.md> "TAChart documentation")

---

_Source: [https://wiki.freepascal.org/NaN](https://web.archive.org/web/20250427003155/https://wiki.freepascal.org/NaN)_
