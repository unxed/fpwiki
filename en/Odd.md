# Odd

│ **[Deutsch (de)](</Odd/de> "Odd/de")** │  **English (en)** │  **[polski (pl)](</Odd/pl> "Odd/pl")** │    
****

The [standard function](<Basic_Pascal_Tutorial/Chapter_1/Standard_Functions.md> "Basic Pascal Tutorial/Chapter 1/Standard Functions") [**`odd`**](<https://www.freepascal.org/docs-html/rtl/system/odd.html>) returns [`true`](<True.md> "True") if and only if the passed [`integer`](<Integer.md> "Integer") parameter is odd, that means it is not divisible by `2`. `Odd(x)` is by definition equivalent to the [expression](<expression.md> "expression")
    
    
    abs(x) mod 2 = 1
    

The `abs` serves the purpose of any potential for confusion regarding the sign of the [`mod` operator](<Mod.md> "Mod") in conjunction with a _negative_ operand. It is _unnecessary_ in a _fully-ISO‑compliant_ compiler (for [FPC](<FPC.md> "FPC") this means [`{$modeSwitch ISOMod+}`](<modeswitch.md> "modeswitch")). 

## application

Beside using `odd` directly, in combination with [`ord`](<Ord.md> "Ord") it can be useful to simplify calculations. The following [`program`](<Program.md> "Program") calculates [math]\displaystyle{ 1 \cdot 3^1 + 2 \cdot 3^0 + 3 \cdot 3^1 + 4 \cdot 3^0 }[/math] (= [math]\displaystyle{ 3 + 2 + 9 + 4 }[/math]) without the need of [branches](<Branch.md> "Branch"): 
    
    
    {$mode extendedPascal}
    program oddDemo(output);
    var
    	i, sum: integer value 0;
    begin
    	for i := 1 to 4 do
    	begin
    		{ alternating scale factor 3^0, 3^1 based on Boolean }
    		sum := sum + i * 3 pow ord(odd(i));
    	end;
    	writeLn(sum);
    end.
    

NB: Despite [`{$mode extendedPascal}`](<Mode_extendedpascal.md> "Mode extendedpascal") the shown `integer` power operator `pow` and initial-value specification are not yet supported by FPC as of version 3.2.0. 

## notes

  * There is no standard built‑in function “`even`”. Obviously the expression [`not odd(x)`](<Not.md> "Not") determines whether an `integer` is even.
  * For constant expressions, `odd` can be evaluated at compile-time, thus it can appear in the [`const` section](<Const.md> "Const").
  * On most architectures `odd` can be implemented as a simple `and x, 1`, that is by masking the least-significant bit in a value. For _these_ platforms, using [type helpers](</index.php?title=Type_Helper&action=edit&redlink=1> "Type Helper \(page does not exist\)") from the [`sysUtils` unit](<sysutils.md> "sysutils"), `x.testBit(0)` has the same return value.

---

_Source: [https://wiki.freepascal.org/Odd](https://web.archive.org/web/20241209225242/https://wiki.freepascal.org/Odd)_
