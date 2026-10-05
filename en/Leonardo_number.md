# Leonardo number

│ **[Deutsch (de)](</Leonardo_number/de> "Leonardo number/de")** │  **English (en)** │  **[suomi (fi)](</Leonardo_number/fi> "Leonardo number/fi")** │  **[français (fr)](</Leonardo_number/fr> "Leonardo number/fr")** │  **[русский (ru)](<../ru/Leonardo_number.md> "Leonardo number/ru")** │    
****

# Leonardo number

The Leonardo Sequence is the series of numbers: 
    
    
    1, 1, 3, 5, 9, 15, 25 ...
    

## Recursive way
    
    
    function LeonardoNumber( n : integer ):integer;
    begin
      if n > 1 then result := LeonardoNumber( n - 1 ) + LeonardoNumber( n - 2 ) + 1
        else result := 1;
    end;
    

## Making use of [Fibonacci numbers](<Fibonacci_number.md> "Fibonacci number")
    
    
    function LeonardoNumber2( n : integer ):integer;
    begin
      result := 2 * FibonacciNumber( n + 1) - 1
    end;

---

_Source: [https://wiki.freepascal.org/Leonardo_number](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Leonardo_number)_
