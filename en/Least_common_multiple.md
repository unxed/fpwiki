# Least common multiple

│ **English (en)** │  **[русский (ru)](<../ru/Least_common_multiple.md>)** │

The least common multiple of two integers [math]\displaystyle{ a }[/math] and [math]\displaystyle{ b }[/math] is the smallest positive integer that is divisible by both [math]\displaystyle{ a }[/math] and [math]\displaystyle{ b }[/math]. 

For example: for 12 and 9 then least common multiple is 36. 

## `function leastCommonMultiple`
    
    
    function leastCommonMultiple(a, b: Int64): Int64;
    begin
      result := b * (a div greatestCommonDivisor(a, b));
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** [`function greatestCommonDivisor`](<Greatest_common_divisor.md> "Greatest common divisor") must be at least declared before this function.

## see also

  * [greatest common divisor](<Greatest_common_divisor.md> "Greatest common divisor")
  * `mpz_lcm` in [GMP](<gmp.md> "gmp") (GNU multiple precision)

---

_Source: [https://wiki.freepascal.org/Least_common_multiple](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Least_common_multiple)_
