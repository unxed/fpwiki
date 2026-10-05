# Lucas number

│ **English (en)** │  **[suomi (fi)](</Lucas_number/fi> "Lucas number/fi")** │  **[français (fr)](</Lucas_number/fr> "Lucas number/fr")** │    
****

The Lucas series is the sequence of numbers: 
    
    
    2, 1, 3, 4, 7, 11, 18, 29, 47, …
    

The idea is, that the next number is produced by summing the two preceding ones. 

## Contents

  * 1 generation
    * 1.1 recursive implementation
    * 1.2 based on Fibonacci sequence
    * 1.3 iterative implementation
  * 2 lookup
  * 3 see also



## generation

### recursive implementation
    
    
    type
    	/// domain for Lucas number function
    	/// where result fits within a nativeUInt
    	// You can not name it lucasDomain,
    	// since the Lucas number function itself
    	// is defined for all whole numbers
    	// but the result beyond L(n) exceeds high(nativeUInt).
    	lucasLeftInverseRange =
    		{$ifdef CPU64} 0..92 {$else} 0..46 {$endif};
    
    {**
    	calculates Lucas number recursively
     
    	\param n the index of the Lucas number to calculate
    	\return the Lucas number at n,
    	        unless n is out of range, then 0
    }
    function lucas(const n: lucasLeftInverseRange): nativeUInt;
    begin
    	case n of
    		// recursive case
    		2..high(n):
    		begin
    			lucas := lucas(n - 2) + lucas(n - 1);
    		end;
    		// base cases
    		1:
    		begin
    			lucas := 1;
    		end;
    		0:
    		begin
    			lucas := 2;
    		end;
    		// abort case
    		otherwise
    		begin
    			// neutral element of addition
    			// indicating n is out of range
    			// [there is no n satisfying L(n) = 0]
    			lucas := 0;
    		end;
    	end;
    end;
    

### based on Fibonacci sequence

The Lucas numbers can be calculated by using the `fibonacci` function shown in the article [Fibonacci number](<Fibonacci_number.md> "Fibonacci number"). 
    
    
    {**
    	calculates Lucas number based on Fibonacci numbers
     
    	\param n the index of the Lucas number to calculate
    	\return the Lucas number at n,
    	        unless n is out of range, then 0
    }
    function lucas(const n: lucasLeftInverseRange): nativeUInt;
    begin
    	case n of
    		1..high(n):
    		begin
    			lucas := fibonacci(n - 1) + fibonacci(n + 1);
    		end;
    		0:
    		begin
    			// We can not deduce L(0) from Fibonacci
    			// since that function is not defined for negative n
    			// [we would call fibonacci(-1) + fibonacci(1)].
    			lucas := 2;
    		end;
    		otherwise
    		begin
    			// neutral element of addition
    			// indicating n is out of range
    			// [there is no n satisfying L(n) = 0]
    			lucas := 0;
    		end;
    	end;
    end;
    

### iterative implementation

This is in line with the [iterative implementation of `fibonacci`](<Fibonacci_number.md> "Fibonacci number") but with differing start values. Therefore the code is not repeated here. 

## lookup

Calculating Lucas numbers every time they are needed is time-consuming. While the code required for generation takes almost no space, a lookup table is the means of choice when an application needs them a lot. Since Lucas numbers can be derived from Fibonacci numbers, usually only one table (those with the Fibonacci series) is stored, being a compromise between efficiency and memory utilization. In order to use the `fibonacci` table without margin case treatment, it has to be at least expanded to `fibonacci[-1]`. 

An actual implementation is omitted here, since everyone wants it differently. Also, do not program, what's already been programmed. For instance, the Lucas number functions of the [GNU multiple precision arithmetic library](<gmp.md> "gmp") take this approach. 

## see also

  * [Lucas numbers in “the on-line encyclopedia of integer sequences”](<https://oeis.org/A000032>)
  * [Lucas sequence § “Pascal” on RosettaCode.org](<https://rosettacode.org/wiki/Lucas_sequence#Pascal>)
  * [GNU multiple precision arithmetic library](<gmp.md> "gmp")'s functions [`mpz_lucnum_ui` and `mpz_lucnum2_ui`](<https://gmplib.org/manual/Lucas-Numbers-Algorithm.html#Lucas-Numbers-Algorithm>)

---

_Source: [https://wiki.freepascal.org/Lucas_number](https://web.archive.org/web/20250514122543/https://wiki.freepascal.org/Lucas_number)_
