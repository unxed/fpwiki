# Pure functions

This page has been set up as a collaboration and design specification for the proposal and support of 'pure' functions, which allows for massive speed gains in situations where constant parameters are passed into such subroutines. 

[CuriousKit](</index.php?title=User:CuriousKit&action=edit&redlink=1> "User:CuriousKit \(page does not exist\)") ([talk](</index.php?title=User_talk:CuriousKit&action=edit&redlink=1> "User talk:CuriousKit \(page does not exist\)")) 16:45, 8 July 2018 (CEST) 

## Contents

  * 1 Definition
  * 2 Theoretical examples of pure functions
  * 3 Examples of impure functions
  * 4 Design Proposals
    * 4.1 Eligibility
    * 4.2 Proposed Compiler Warnings and Errors
    * 4.3 Extreme Conditions
  * 5 Development Test Cases
  * 6 Compiler Performance Improvements
    * 6.1 Remembering Pre-calculated Values
  * 7 See Also



## Definition

A pure function is a subroutine that is entirely self-contained and contains no side-effects or dependencies on external values (e.g. global variables). As such, a pure function is deterministic and will always return the same value(s) for a given input, analogous to a mathematical function. As such, if a function is called with constant arguments, the result can be pre-computed by the compiler and the entire call replaced with the return value. 

## Theoretical examples of pure functions

  * Trigonometric sine
  * Returning the larger of two values (i.e. the Max function)
  * The length of a static array
  * The factorial function (an example of a pure function that is also recursive)
  * The bitwise functions **or** , **and** , **not** and **xor**.
  * Converting an integer into a string (though a programmer could easily replace, say, IntToStr(1) with just '1', another function that calls IntToStr with a variable argument might be able to take advantage of IntToStr's purity if data flow analysis shows that the argument is deterministic)
  * A hash function



## Examples of impure functions

  * A random number generator (reads and writes to an external seed)
  * The length of a dynamic array (the input is a pointer that has to be dereferenced, hence is not self-contained or deterministic)
  * GetMem (output is non-deterministic and might raise an exception that is highly dependent on the system state)
  * Reading from a file (has no control over the IO state or the contents of the file in question)



## Design Proposals

While it is possible to treat all functions as pure until proven otherwise, this is prohibitively slow for a compiler that might have to deal with many hundreds of functions and thousands of calls in one sitting. Furthermore, most functions will either be impure or contain a non-deterministic parameter (this includes the value of Self in object methods). As such, to take pressure off the compiler, I propose the directive "**pure** " that can be specified after the function header, as a hint to the compiler that the function should be checked for purity. 

### Eligibility

Most functions cannot be pure due to their interaction with memory and system resources that they don't have direct control over. Rather than list the requirements for a function to be pure, it is easier, and faster for the compiler, to list those whose appearance immediately removes a function's eligibility. 

The following list is by no means exhaustive, but a function cannot be pure if... 

  * ...it calls another function, unless that function is also pure — this includes object constructors and the functions that allocate and deallocate memory
  * ...it accesses memory that is external to the function, not just because it has no control over what it contains, but because its value might change while the function is being executed due to race hazards associated with multithreading
  * ...it dereferences a pointer (this includes the use of Self in object methods)
  * ...it uses the @ operator to get the address of something
  * ...it contains a **var** parameter, since these are, by definition, not constant — **out** parameters are fine though
  * ...a local variable or the function result is not initialized



### Proposed Compiler Warnings and Errors

There will be cases where functions are hinted to be pure but are really not, either due to programmer error or deliberate malice (or stress testing). If analysis of the function determines that the function is impure (which might only occur when it tries to replace a call to that function), then I propose the compiler returning a warning to say that the function is not pure and to remove any flags that say it is pure, so it doesn't attempt any further optimisation on that front. This would definitely be a warning that the programmer should fix because there may be function calls that are successfully optimised prior to the detection of something that makes the function impure (due to a different branch being taken in the function), and marking the function as impure at this point will prevent future optimisations that would otherwise be valid. 

List of possible warning messages: 

  * "Procedure marked as pure accesses external variable"
  * "Procedure marked as pure calls an impure subroutine"
  * "Procedure marked as pure dereferences a pointer"
  * "Procedure is too complex to be pure, or contains an infinite loop" (See Extreme Conditions below)



There are two exceptions though where I propose an error instead of a warning, the first one is where you try to assign the value of a pure function to a constant, and the function is proven impure, and the second is where a particular call to a pure function would raise an exception. The reason why I propose an error in these situations is because many of these functions are perfectly valid mathematical constructs and are often useful (e.g. division, which raises an exception if the denominator is zero, or the natural logarithm, which raises an exception if the input is negative), and to disable their purity for something that is likely to be a programmer error would be unfair. 

Other scenarios can raise errors where **pure** would be invalid, such as on object methods, but this can reuse the procedure directive error that appears when **virtual** and **inline** are paired, for example. 

Possible error message: 

  * "Call to pure function would raise exception"



(A combined warning and error would be raised for the constant assignment situation, first the warning that the function isn't pure, and then the fact you're trying to assign a (impure) function value to a constant) 

### Extreme Conditions

There are cases where a function is pure but which could easily cause the compiler to take a prohibitively long time to evaluate. Infinite loops are an example of a malicious case, but there are situations where the function will complete, but will take far too long in practice. A constructed example is the [Ackermann function](<https://en.wikipedia.org/wiki/Ackermann_function>), which has a very deep level of recursion. As a result, it is only sensible to include an upper limit in how complex a function can be before the compiler decides it shouldn't be treated as pure. In this case, a warning should be thrown if it reaches a node count or stack depth limit, which will also catch infinite loops. Relying on a temporal timeout is too unreliable and random. 

## Development Test Cases

The first test is a simple function with a single conditional check. It is also a function that can easily be calculated by hand for given values _a_ and _b_ to determine compiler correctness. 
    
    
    function Max(a, b: Double): Double; inline; pure;
    begin
      if a > b then
        Result := a
      else
        Result := b;
    end;
    

Once Max and similar functions are tested, I propose a more complex function that relies on recursion, hence cannot be inlined, but which is still pure: 
    
    
    function Factorial(n: QWord): QWord; pure;
    begin
      if n <= 1 then
        Result := 1
      else
        Result := n * Factorial(n - 1);
    end;
    

As a final test case, I propose the [Ackermann Function](<https://en.wikipedia.org/wiki/Ackermann_function>). This will stress-test the compiler to see how it handles functions with deep recursion but which don't have infinite loops and will eventually finish but maybe in a prohibitively long time. 
    
    
    function Ackermann(m, n: QWord): QWord; pure;
    begin
      if (m = 0) then
        Result := n + 1
      else if (n = 0) then
        Result := Ackermann(m - 1, 1)
      else
        Result := Ackermann(m - 1, Ackermann(m, n - 1)); 
    end;
    

## Compiler Performance Improvements

### Remembering Pre-calculated Values

Even if extreme constructs such as the Ackermann function are ignored, analysing a pure function every time it is called might cause the compiler to be too slow to be practical. One way around this is to store results of functions that have already been calculated, so if an identical call appears elsewhere in the code, the answer can be looked up without having to perform data flow analysis again. Take the following pair of functions: 
    
    
    function Factorial(n: QWord): QWord; pure;
    begin
      if n <= 1 then
        Result := 1
      else
        Result := n * Factorial(n - 1);
    end;
    
    function RoughExp(x: Double): Double; pure;
    var
      p: Integer;
    begin
      Result := 0;
      { Assume that IntPower is also a pure function }
      for p := 0 to 7 do
        Result := Result + (IntPower(x, p) / Factorial(p));
    end;
    

(This function encodes the first eight terms of the MacLaurin expansion for the exponential function). 

The first value passed into the Factorial function is 0, which returns 1, Factorial(1) is calculated to also be 1, then Factorial(2) is calculated to be 2 * 1 = 2. When the value of Factorial(3) is calculated, it only has to get as far as 3 * Factorial(2), since the compiler knows Factorial(2) to be 2, and hence can return the correct answer of 6 early. By the time Factorial(7) has to be calculated, the compiler has calculated and stored all the previous values, and so only has to calculate 7 * Factorial(6) = 7 * 720 = 5,040 instead of 7 * (6 * (5 * (4 * (3 * (2 * 1))))) and all the function calls and additional data flow analysis associated with it. 

Not only does this provide a speed gain within a single evaluation of RoughExp and remove the need to analyse other calls to RoughExp with the same value of _x_ , it improves performance for calls to RoughExp with different values for _x_ , as all the factorial quotients have already been pre-calculated. 

## See Also

  * [Wikipedia definition of "Pure function"](<https://en.wikipedia.org/wiki/Pure_function>)
  * [constexpr specifier (C++)](<https://en.cppreference.com/w/cpp/language/constexpr>)
  * [Turing's halting problem](<https://en.wikipedia.org/wiki/Halting_problem>), a limiting factor when evaluating functions at compile time

---

_Source: [https://wiki.freepascal.org/Pure_functions](https://web.archive.org/web/20240226045252/https://wiki.freepascal.org/Pure_functions)_
