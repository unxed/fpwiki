# Int

│ **English (en)** │  **[русский (ru)](<../ru/Int.md>)** │

The [`function`](<Function.md> "Function") **[`Int`](<https://www.freepascal.org/docs-html/rtl/system/int.html>)** returns the [integer](<Integer.md> "Integer") part of the argument. It is an [UCSD Pascal](<UCSD_Pascal.md> "UCSD Pascal") extension supported by the [FPC](<FPC.md> "FPC") as an `InternProc`. 

## Behavior

`Int` has effectively the following signature ([actual declaration](<https://gitlab.com/freepascal.org/fpc/source/blob/release_3_2_0/rtl/inc/mathh.inc#L116>) uses the compiler intrinsic): 
    
    
    function Int(X: Real): Real;
    

`X` must be a [real](<Real.md> "Real")-type expression. The result is the integer part of X, rounded toward zero _as a` real` value_. It is _semantically_ equivalent to `Trunc(X) * 1.0` (`Trunc` would cause an error if there did not exist an appropriate `integer` value). 

## Application

  * `Int` is used to do a [`Trunc`](<Trunc.md> "Trunc") without leaving the domain of `real` numbers. This speeds up things and virtually allows a larger range of permissible values. For an example see [the implementation of](<https://gitlab.com/freepascal.org/fpc/source/blob/release_3_2_0/rtl/objpas/math.pp#L2476-2496>) [`math.FMod`](<https://www.freepascal.org/docs-html/rtl/math/fmod.html>).



## See also

  * [`Frac`](<Frac.md> "Frac") – return fractional part as `real`
  * [`Round`](<Round.md> "Round") – return rounded `integer` value
  * [`Trunc`](<Trunc.md> "Trunc") – return truncated `integer` value

---

_Source: [https://wiki.freepascal.org/Int](https://web.archive.org/web/20240225160555/https://wiki.freepascal.org/Int)_
