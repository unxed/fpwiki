# Fibonacci number

│ **[Deutsch (de)](</Fibonacci_number/de> "Fibonacci number/de")** │  **English (en)** │  **[suomi (fi)](</Fibonacci_number/fi> "Fibonacci number/fi")** │  **[français (fr)](</Fibonacci_number/fr> "Fibonacci number/fr")** │  **[русский (ru)](<../ru/Fibonacci_number.md> "Fibonacci number/ru")** │    
****

The Fibonacci Sequence is the series of numbers: 
    
    
     0, 1, 1, 2, 3, 5, 8, 13, 21, …
    

The idea is to add the two last numbers in order to produce the next value. 

## Contents

  * 1 generation
    * 1.1 recursive implementation
    * 1.2 iterative implementation
  * 2 lookup
  * 3 see also



## generation

The following implementations show the _principle_ of how to calculate Fibonacci numbers. They lack of input checks. Depending on your preferences you might either want to generate a [run-time error](<runtime_error.md> "runtime error") (e. g. by [`{$rangeChecks on}`](<$rangeChecks.md> "$rangeChecks") or utilizing [`system.runError`](<https://www.freepascal.org/docs-html/rtl/system=/runerror.html>)), [raise](<Raise.md> "Raise") an [exception](<Exceptions.md> "Exceptions"), or simply return a bogus value indicating something went wrong. 

### recursive implementation
    
    
    type
    	/// domain for Fibonacci function
    	/// where result is within nativeUInt
    	// You can not name it fibonacciDomain,
    	// since the Fibonacci function itself
    	// is defined for all whole numbers
    	// but the result beyond F(n) exceeds high(nativeUInt).
    	fibonacciLeftInverseRange =
    		{$ifdef CPU64} 0..93 {$else} 0..47 {$endif};
    
    {**
    	implements Fibonacci sequence recursively
    	
    	\param n the index of the Fibonacci number to retrieve
    	\returns the Fibonacci value at n
    }
    function fibonacci(const n: fibonacciLeftInverseRange): nativeUInt;
    begin
    	// optimization: then part gets executed most of the time
    	if n > 1 then
    	begin
    		fibonacci := fibonacci(n - 2) + fibonacci(n - 1);
    	end
    	else
    	begin
    		// since the domain is restricted to non-negative integers
    		// we can bluntly assign the result to n
    		fibonacci := n;
    	end;
    end;
    

### iterative implementation

This one is preferable for its [run-time](<runtime.md> "runtime") behavior. 
    
    
    {**
    	implements Fibonacci sequence iteratively
    	
    	\param n the index of the Fibonacci number to calculate
    	\returns the Fibonacci value at n
    }
    function fibonacci(const n: fibonacciLeftInverseRange): nativeUInt;
    type
    	/// more meaningful identifiers than simple integers
    	relativePosition = (previous, current, next);
    var
    	/// temporary iterator variable
    	i: longword;
    	/// holds preceding fibonacci values
    	f: array[relativePosition] of nativeUInt;
    begin
    	f[previous] := 0;
    	f[current] := 1;
    	
    	// note, in Pascal for-loop-limits are inclusive
    	for i := 1 to n do
    	begin
    		f[next] := f[previous] + f[current];
    		f[previous] := f[current];
    		f[current] := f[next];
    	end;
    	
    	// assign to previous, bc f[current] = f[next] for next iteration
    	fibonacci := f[previous];
    end;
    

## lookup

While calculating the Fibonacci number every time it is needed requires almost no space, it takes a split second of time. [Applications](<Application.md> "Application") heavily relying on Fibonacci numbers definitely want to use a lookup table instead. And yet in general, do not calculate what is already a known fact. Since the Fibonacci sequence doesn’t change, actually calculating it is a textbook demonstration but not intended for production use. 

Nevertheless an actual implementation is omitted here, since everyone wants to have it differently, a different flavor. 

## see also

  * [Fibonacci numbers in the on-line encyclopedia of integer sequences](<https://oeis.org/A000045>)
  * [Some assembly routine which uses the C calling convention that calculates the nth Fibonacci number](<https://freepascal.org/docs-html/current/prog/progsu150.html>)
  * [Fibonacci sequence § “Pascal” on RosettaCode.org](<https://rosettacode.org/wiki/Fibonacci_sequence#Pascal>)
  * [GNU multiple precision arithmetic library](<gmp.md> "gmp")’s functions [`mpz_fib_ui` and `mpz_fib2_ui`](<https://gmplib.org/manual/Fibonacci-Numbers-Algorithm.html#Fibonacci-Numbers-Algorithm>)
  * [Lucas number](<Lucas_number.md> "Lucas number"), a similar series with different initial values
  * [Tao Yue Solution to Fibonacci Sequence Problem](<Solution_3.md> "Solution 3")

---

_Source: [https://wiki.freepascal.org/Fibonacci_number](https://web.archive.org/web/20250419142614/https://wiki.freepascal.org/Fibonacci_number)_
