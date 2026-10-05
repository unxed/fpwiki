# Round

│ **English (en)** │  **[русский (ru)](<../ru/Round.md>)** │

The [RTL](<RTL.md> "RTL") [System unit](<System_unit.md> "System unit") contains [function](<Function.md> "Function") **Round** , which rounds a [Real](<Real.md> "Real")-type value to an [Integer](<Integer.md> "Integer")-type value. It's input parameter is a real-type [expression](<expression.md> "expression") and Round returns a [Int64](<Int64.md> "Int64") value that is the value of the input rounded to the nearest whole number. If the input value is exactly halfway between two whole numbers - N.5 - then "bankers rounding" is used, with the result being the nearest even number. 

## Contents

  * 1 Declaration
  * 2 Example Usage
    * 2.1 Output
  * 3 See also



## Declaration
    
    
    function Round(X: Real): int64;
    

## Example Usage
    
    
    begin
       WriteLn( Round(8.7) );
       WriteLn( Round(8.3) );
       // examples of "bankers rounding" - .5 is adjusted to the nearest even number
       WriteLn( Round(2.5) );
       WriteLn( Round(3.5) );
    end.
    

### Output
    
    
     9
     8
     2
     4
    

## See also

  * [`system.round`](<https://www.freepascal.org/docs-html/rtl/system/round.html>)
  * [`math.ceil`](<https://www.freepascal.org/docs-html/rtl/math/ceil.html>) \- round up
  * [`math.floor`](<https://www.freepascal.org/docs-html/rtl/math/floor.html>) \- round down
  * [`frac`](<Frac.md> "Frac") \- returns the fractional part of a floating point value
  * [`trunc`](<Trunc.md> "Trunc") \- round towards zero
  * [`int`](<Int.md> "Int") \- returns the integer part of a floating point value
  * [`div`](<Div.md> "Div") \- integer division
  * [Comparison of approaches for rounding to an integer](<Comparison_of_approaches_for_rounding_to_an_integer.md> "Comparison of approaches for rounding to an integer")

---

_Source: [https://wiki.freepascal.org/Round](https://web.archive.org/web/20250114170626/https://wiki.freepascal.org/Round)_
