# Trunc

│ **English (en)** │  **[suomi (fi)](</Trunc/fi> "Trunc/fi")** │  **[русский (ru)](<../ru/Trunc.md> "Trunc/ru")** │    
****

The [Standard Pascal](<Standard_Pascal.md> "Standard Pascal") function [`trunc`](<https://www.freepascal.org/docs-html/rtl/system/trunc.html>) returns the integer part of a [`real`-type value](<Real.md> "Real") rounded toward zero. 

[![trunc.png](https://wiki.freepascal.org/images/e/e0/trunc.png)](</File:trunc.png>)

[](</File:trunc.png> "Enlarge")

`trunc` is short for “truncate”. The fraction part of the `real` value is, so to speak, cut off. However, unlike the [`int` function](<Int.md> "Int") the result is of [`integer`](<Integer.md> "Integer") type. 

[FPC](<FPC.md> "FPC") implements this [function](<Function.md> "Function") as a compiler function. 

## definition

The mathematical definition reads as follows: 

[math]\displaystyle{ \text{trunc}(x) := \begin{cases} \lfloor x \rfloor & \text{if } x \geq 0 \\\ \lceil x \rceil & \text{if } x \lt 0 \end{cases} }[/math]

The function `trunc` is defined for all `real` values in the open interval (`low(integer)-1`, `high(integer)+1`). If the supplied parameter is out of range, a program compiled with FPC will stop with the [run-time error](<runtime_error.md> "runtime error") 207 “Invalid floating point operation”. If the [`sysUtils`](<sysutils.md> "sysutils") is included, this RTE becomes the [`eInvalidOp` exception](<https://www.freepascal.org/docs-html/rtl/sysutils/einvalidop.html>). 

## application

`Trunc` is used to retrieve an integer value. It is primarily used where no loss in accuracy is expected if the fractional part (if any) is removed. 

For example: [Pascal](<Pascal.md> "Pascal") does not have a power operator or function built in. Instead, one has to define their own function doing this task using already existing function. One can utilize the rule [math]\displaystyle{ a^x = e^{x \times \ln a} }[/math] and the pre-existing `exp` and `ln` functions for that. However, if the operands are integers, it is guaranteed the result will be an integer to, yet `exp` will return a `real` value. The result can be truncated, without loss in precision. For source code, see [asterisk § “exponentiation”](<_.md> "*"). 

## see also

  * [`round`](<Round.md> "Round")
  * [`math.ceil`](<https://www.freepascal.org/docs-html/rtl/math/ceil.html>) – round up
  * [`math.floor`](<https://www.freepascal.org/docs-html/rtl/math/floor.html>) – round down
  * [`frac`](<Frac.md> "Frac") – returns the fractional part of a floating point value
  * [`int`](<Int.md> "Int") – returns the integer part of a floating point value
  * [`div`](<Div.md> "Div") – integer division
  * [Comparison of approaches for rounding to an integer](<Comparison_of_approaches_for_rounding_to_an_integer.md> "Comparison of approaches for rounding to an integer")

---

_Source: [https://wiki.freepascal.org/Trunc](https://web.archive.org/web/20240920204117/https://wiki.freepascal.org/Trunc)_
