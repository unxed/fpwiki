# Greatest common divisor

│ **[English (en)](<../en/Greatest_common_divisor.md>)** │  **русский (ru)** │

Наибольшим общим делителем (**НОД**) двух целых чисел является наибольшее целое число, на которое делятся оба данных числа. Для чисел 121 и 143 наибольшим общим делителем является число 11. 

Существует множество методов вычисления **НОД**. Например, алгоритм Евклида, основанный на делении: 

## Функция greatestCommonDivisor
    
    
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
      result := a
    end;
    
    // алгоритм, основанный на вычитании
    function greatestCommonDivisor_euclidsSubtractionMethod(a, b: Int64): Int64;
    begin
      // работает только с положительными целыми числами
      if (a < 0) then a := -a;
      if (b < 0) then b := -b;
      // без входа в цикл, так как вычитание нуля не приведет к выходу из цикла
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
    

## См. также

  * [Наименьшее общее кратное](<Least_common_multiple.md> "Least common multiple/ru")
  * [Оператор mod](</index.php?title=Mod/ru&action=edit&redlink=1> "Mod/ru \(page does not exist\)")
  * mpz_gcd в [GMP](<../en/gmp.md> "gmp") (библиотека высокой точности GNU)



## Внешние ссылки

  * [Пример рекурсивного алгоритма](<https://rosettacode.org/wiki/Greatest_common_divisor#Pascal_.2F_Delphi_.2F_Free_Pascal>)

---

_Source: [https://wiki.freepascal.org/Greatest_common_divisor/ru](https://web.archive.org/web/20250325130746/https://wiki.freepascal.org/Greatest_common_divisor/ru)_
