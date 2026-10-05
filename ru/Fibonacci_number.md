# Fibonacci number

│ **[English (en)](<../en/Fibonacci_number.md>)** │  **русский (ru)** │

## Contents

  * 1 Числа Fibonacci
    * 1.1 Рекурсивный способ
    * 1.2 Итеративный способ
    * 1.3 См. также



# Числа Fibonacci

Числа Fibonacci задаются следующей последовательностью: 
    
    
    0, 1, 1, 2, 3, 5, 8, 13, 21, ...
    

Идея заключается в сложении двух последних чисел, и этот результат является следующим значением последовательности. 

## Рекурсивный способ
    
    
    function FibonacciNumber( n : integer ): integer;
    begin
      if n > 1 then result := ( FibonacciNumber( n - 1 ) + FibonacciNumber( n - 2 ) )
        else
          if n = 0 then result := 0
            else result := 1;
    end;
    

## Итеративный способ

Вот один из предпочтительных вариантов. 
    
    
    function Fibonacci(n: Integer): Integer;
    var
      i,u,v,w: Integer;
    begin
      if n <= 0 then
        exit(0);
      if n = 1 then 
         exit(1);
      u := 0;
      v := 1;
      for i := 2 to n do 
      begin
        w := u + v;
        u := v;
        v := w;
      end;
      Result := v;
    End;
    

## См. также

  * [Some assembly routine which uses the C calling convention that calculates the nth Fibonacci number](<http://www.freepascal.org/docs-html/prog/progsu151.html>)
  * [ Tao Yue Solution to Fibonacci Sequence Problem](<../en/Solution_3.md> "Solution 3")

---

_Source: [https://wiki.freepascal.org/Fibonacci_number/ru](https://web.archive.org/web/20250418040457/https://wiki.freepascal.org/Fibonacci_number/ru)_
