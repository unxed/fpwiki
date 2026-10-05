# SetRoundMode

│ **English (en)** │

With [`math.setRoundMode`](<https://www.freepascal.org/docs-html/rtl/math/setroundmode.html>) you can set the FPU’s method of rounding. If no FPU is present, the global variable [`system.softFloat_rounding_mode`](<https://www.freepascal.org/docs-html/rtl/system/softfloat_rounding_mode.html>) will determine the rounding mode for floating-point operations implemented by software. 

This also affects the run-time behavior of the `round` function. (Note, constant expressions can and will be evaluated _during_ compile-time, thus the presented techniques do not affect compile-time results.) 

## invocation

`SetRoundMode` resides in the [`math` unit](<https://www.freepascal.org/docs-html/rtl/math/index.html>). You need to include the `math` unit in the [`uses` clause](<Uses.md> "Uses"). 

`SetRoundMode` is a unary function. The function’s signature reads: 
    
    
    function setRoundMode(const roundMode: TFPURoundingMode): TFPURoundingMode
    

The following values are permissible for `roundMode` (cf. [`TFPURoundingMode`](<https://www.freepascal.org/docs-html/rtl/system/tfpuroundingmode.html>)): 

`rmNearest`
    rounds to the nearest _even_ integer (Banker‘s Rounding: `0.5` rounds down to `0`; `1.5` rounds up to `2`). This is the default.
`rmDown`
    generally rounds to the next smaller integer
`rmUp`
    generally rounds to the next larger integer
`rmTruncate`
    cuts off the decimal places

The function will return the previous rounding mode, before it was changed. Thus, it returns the same value as [`math.getRoundMode`](<https://www.freepascal.org/docs-html/rtl/math/getroundmode.html>). 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The setting of the rounding mode applies to all _internal_ floating point calculations. In particular, it determines how numbers that cannot be represented exactly as the native floating-point type’s values are to be rounded to the internal representation within the scope of the available bits. Therefore, the usage of `setRoundMode` for general rounding purposes is not recommended. 

## see also

  * [`round`](<Round.md> "Round")
  * [`int`](<Int.md> "Int")
  * [`trunc`](<Trunc.md> "Trunc")


  *[FPU]: Floating Point Unit

---

_Source: [https://wiki.freepascal.org/SetRoundMode](https://web.archive.org/web/20240304010302/https://wiki.freepascal.org/SetRoundMode)_
