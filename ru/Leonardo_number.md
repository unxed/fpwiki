# Leonardo number

│ **[English (en)](<../en/Leonardo_number.md>)** │  **русский (ru)** │

# Числа Leonardo

Числа Leonardo задаются следующей последовательностью: 
    
    
    1, 1, 3, 5, 9, 15, 25 ...
    

## Рекурсивный способ
    
    
    function LeonardoNumber( n : integer ):integer;
    begin
      if n > 1 then result := LeonardoNumber( n - 1 ) + LeonardoNumber( n - 2 ) + 1
        else result := 1;
    end;
    

  


## Вычисление с помощью [чисел Fibonacci](<Fibonacci_number.md> "Fibonacci number/ru")
    
    
    function LeonardoNumber2( n : integer ):integer;
    begin
      result := 2 * FibonacciNumber( n + 1) - 1
    end;

---

_Source: [https://wiki.freepascal.org/Leonardo_number/ru](https://web.archive.org/web/20250122165804/https://wiki.freepascal.org/Leonardo_number/ru)_
