# Least common multiple

│ **[English (en)](<../en/Least_common_multiple.md> "Least common multiple")** │  **[suomi (fi)](</Least_common_multiple/fi> "Least common multiple/fi")** │  **[français (fr)](</Least_common_multiple/fr> "Least common multiple/fr")** │  **русский (ru)** │    
****

Наименьшим общим кратным (**НОК**) двух целых чисел [math]\displaystyle{ a }[/math] и [math]\displaystyle{ b }[/math] является наименьшее положительное целое число, которое делится на оба числа [math]\displaystyle{ a }[/math] и [math]\displaystyle{ b }[/math]. 

Например: для чисел 12 и 9 наименьшим общим кратным будет число 36. 

## Функция leastCommonMultiple
    
    
    function leastCommonMultiple(a, b: Int64): Int64;
    begin
      result := b * (a div greatestCommonDivisor(a, b));
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** [Функция greatestCommonDivisor](<Greatest_common_divisor.md> "Greatest common divisor/ru") должна быть объявлена перед функцией leastCommonMultiple.

## См. также

  * [Наибольший общий делитель](<Greatest_common_divisor.md> "Greatest common divisor/ru")
  * mpz_lcm в [GMP](<../en/gmp.md> "gmp") (библиотека высокой точности GNU)

---

_Source: [https://wiki.freepascal.org/Least_common_multiple/ru](https://web.archive.org/web/20240302035136/https://wiki.freepascal.org/Least_common_multiple/ru)_
