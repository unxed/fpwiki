# Greatest common divisor

│ **English (en)** │  **[русский (ru)](<../ru/Greatest_common_divisor.md>)** │

The greatest common divisor of two integers is the largest integer that divides them both. If numbers are 121 and 143 then greatest common divisor is 11. 

There are many methods to calculate this. For example, the division-based Euclidean algorithm version may be programmed: 

## `function greatestCommonDivisor`
    
    
    function greatestCommonDivisor(a, b: Int64): Int64;
    var
      temp: Int64;
    begin
      while b <> 0 do
      begin
        temp := b;
        b := a mod b;
        a := temp
      end;
      result := a;
    end;
    
    function greatestCommonDivisor_euclidsSubtractionMethod(a, b: Int64): Int64;
    begin
      // only works with positive integers
      if (a < 0) then a := -a;
      if (b < 0) then b := -b;
      // don't enter loop, since subtracting zero won't break condition
      if (a = 0) then exit(b);
      if (b = 0) then exit(a);
      while not (a = b) do
      begin
        if (a > b) then
         a := a - b
        else
         b := b - a;
      end;
      result := a;
    end;
    

## see also

  * [least common multiple](<Least_common_multiple.md> "Least common multiple")
  * [`mod` operator](<Mod.md> "Mod")
  * `mpz_gcd` in [GMP](<gmp.md> "gmp") (GNU multiple precision)



## external references

  * [Recursive example](<https://rosettacode.org/wiki/Greatest_common_divisor#Pascal_.2F_Delphi_.2F_Free_Pascal>)

---

_Source: [https://wiki.freepascal.org/Greatest_common_divisor](https://web.archive.org/web/20241209231636/https://wiki.freepascal.org/Greatest_common_divisor)_
