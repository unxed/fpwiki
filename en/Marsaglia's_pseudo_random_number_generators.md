# Marsaglia's pseudo random number generators

[![fpc source logo.png](https://wiki.freepascal.org/images/e/e1/fpc_source_logo.png)](</File:fpc_source_logo.png>)

## Contents

  * 1 Marsaglia's pseudo random number generators
    * 1.1 Multiply with carry
    * 1.2 fibonaci
    * 1.3 Congruential generator
    * 1.4 four lagged Fibonaci
    * 1.5 3-shift-register generator
    * 1.6 Subtract with borrow
    * 1.7 Keep it simple sir
    * 1.8 XorShift
    * 1.9 Implementation
    * 1.10 Example
    * 1.11 Tests
    * 1.12 Can we have one of these as the standard random, plz?
    * 1.13 References
  * 2 See also
    * 2.1 TODO



## Marsaglia's pseudo random number generators

Although Free Pascal has a reasonably good pseudo random number generator (PRNG), a [Mersenne Twister](<A_simple_implementation_of_the_Mersenne_twister.md> "A simple implementation of the Mersenne twister"), it is rather slow. 

In 1999 (and 2003), Professor George Marsaglia described a set of PRNGs with good properties that are much faster and often just as good. He was kind enough to provide sourcecode - in C - and a description of each. The XorShift PRNG from the 2003 paper is added as well. Some of these PRNGs, either stand-alone or in combination, have over time been proven to be equal or superior to either Delphi's LNG or even Free Pascal's Mersenne twister. Notably SWR, XorShift and combined e.g. SWB+KISS which has a period of 2^7700 and passes all tests for randomness. 

Here we present a class that encapsulates these PRNGs for 32 bit random values with a short explanation. The class contains the following PRNGs with Marsaglia's comments from his usenet post. It is extremely fast. 

Marsaglia describes them as follows: 

### Multiply with carry
    
    
         The MWC generator concatenates two 16-bit multiply-
         with-carry generators, x(n)=36969x(n-1)+carry,
         y(n)=18000y(n-1)+carry mod 2^16, has period about
         2^60 and seems to pass all tests of randomness. A
         favorite stand-alone generator---faster than KISS,
         which contains it.  
    

### fibonaci
    
    
         FIB is the classical Fibonacci sequence
         x(n)=x(n-1)+x(n-2),but taken modulo 2^32.
         Its period is 3*2^31 if one of its two seeds is odd
         and not 1 mod 8. It has little worth as a RNG by
         itself, but provides a simple and fast component for
         use in combination generators.
         The classical Fibonacci sequence mod 2^32 from FIB
         fails several tests. It is not suitable for use by
         itself, but is quite suitable for combining with
         other generators.
    

### Congruential generator
    
    
         CONG is a congruential generator with the widely used 69069
         multiplier: x(n)=69069x(n-1)+1234567. It has period
         2^32. The leading half of its 32 bits seem to pass
         tests, but bits in the last half are too regular.
         The last half of the bits of CONG are too regular,
         and it fails tests for which those bits play a
         significant role. CONG+FIB will also have too much
         regularity in trailing bits, as each does. But keep
         in mind that it is a rare application for which
         the trailing bits play a significant role. CONG
         is one of the most widely used generators of the
         last 30 years, as it was the system generator for
         VAX and was incorporated in several popular
         software packages, all seemingly without complaint.
         (note: this is about the same as Delphi uses)
    

### four lagged Fibonaci
    
    
         LFIB4 is an extension of what (Marsaglia) have previously
         defined as a lagged Fibonacci generator:
         x(n)=x(n-r) op x(n-s), with the x's in a finite
         set over which there is a binary operation op, such
         as +,- on integers mod 2^32, * on odd such integers,
         exclusive-or(xor) on binary vectors. Except for
         those using multiplication, lagged Fibonacci
         generators fail various tests of randomness, unless
         the lags are very long. (See SWB below).
         To see if more than two lags would serve to overcome
         the problems of 2-lag generators using +,- or xor, I
         have developed the 4-lag generator LFIB4 using
         addition: x(n)=x(n-256)+x(n-179)+x(n-119)+x(n-55)
         mod 2^32. Its period is 2^31*(2^256-1), about 2^287,
         and it seems to pass all tests---in particular,
         those of the kind for which 2-lag generators using
         +,-,xor seem to fail. For even more confidence in
         its suitability, LFIB4 can be combined with KISS,
         with a resulting period of about 2^410: just use
         (KISS+LFIB4) in any Pascal expression.
    

### 3-shift-register generator
    
    
         SHR3 is a 3-shift-register generator with period
         2^32-1. It uses y(n)=y(n-1)(I+L^17)(I+R^13)(I+L^5),
         with the y's viewed as binary vectors, L the 32x32
         binary matrix that shifts a vector left 1, and R its
         transpose. SHR3 seems to pass all except those
         related to the binary rank test, since 32 successive
         values, as binary vectors, must be linearly
         independent, while 32 successive truly random 32-bit
         integers, viewed as binary vectors, will be linearly
         independent only about 29% of the time.
         (note: this is an ancestor of XORSHIFT (2003) and very fast)
    

### Subtract with borrow
    
    
         SWB is a subtract-with-borrow generator that I
         developed to give a simple method for producing
         extremely long periods:
         x(n)=x(n-222)-x(n-237)- borrow mod 2^32.
         The 'borrow' is 0, or set to 1 if computing x(n-1)
         caused overflow in 32-bit integer arithmetic. This
         generator has a very long period, 2^7098(2^480-1),
         about 2^7578. It seems to pass all tests of
         randomness, except for the Birthday Spacings test,
         which it fails badly, as do all lagged Fibonacci
         generators using +,- or xor. I would suggest
         combining SWB with KISS, MWC, SHR3, or CONG.
         KISS+SWB has period >2^7700 and is highly
         recommended.
         Subtract-with-borrow has the same local behaviour
         as lagged Fibonacci using +,-,xor---the borrow
         merely provides a much longer period.
         SWB fails the birthday spacings test, as do all
         lagged Fibonacci and other generators that merely
         combine two previous values by means of =,- or xor.
         Those failures are for a particular case: m=512
         birthdays in a year of n=2^24 days. There are
         choices of m and n for which lags >1000 will also
         fail the test. A reasonable precaution is to always
         combine a 2-lag Fibonacci or SWB generator with
         another kind of generator, unless the generator uses
         *, for which a very satisfactory sequence of odd
         32-bit integers results.
    

### Keep it simple sir
    
    
         The KISS generator, (Keep It Simple Stupid), is
         designed to combine the two multiply-with-carry
         generators in MWC with the 3-shift register SHR3 and
         the congruential generator CONG, using addition and
         exclusive-or. Period about 2^123.
         It is one of (Marsaglia's) favorite generators.
    

### XorShift

This was not in the original post, it was only published in 2003, but since it is a similar algorithm and it is of importance for its speed and properties.  
I feel it is warranted to include it here.  
It is similar to the SHR3 PRNG and is described in detail in the pdf you find in the link below. 

### Implementation

The class is a fully static sealed class so you do not have to instantiate it or derive from it. If you use just one of the PRNGs it is easy to factor it out. Note that several Prng's maintain state: 

Here I have implemented it as a static class, but it is also possible to write them as a function with writable typed constants. I would recommend the static class approach, though, since calling the class constructor can reset the PRNG, Something that is not (easily) possible with the functional approach unless you would use global variables or double the state fields. Note that if you assign the output to a signed integer, you will get signed result, if needed. 
    
    
    unit uMarsaglia;
    { author: Thaddy de Koning, based on the original C sourcecode in the original post by George Marsaglia }
    { http://www.ciphersbyritter.com/NEWS4/RANDC.HTM#369B5E30.65A55FD1@stat.fsu.edu }
    {$mode objfpc}
    interface
    
    type
      { It is not meant to be extended, since it is based on a single article so I sealed the class }
      TMarsagliaPrngs = class sealed 
      strict private
        {static variables:}
        class var T: array [0..255] of dword;
        class var a,b,v,w,x,y,z,bro,jcong,jsr:dword;
        class var c:byte;
        class function wnew:dword;inline;static;    
        class function znew:dword;inline;static;
      public
        { You can also call the create explicitly to reset the static 
          variables.}
        class constructor create;
        class procedure settable(const i1,i2,i3,i4,i5,i6:dword);static;
        class procedure Randomize;inline;static;
        class function MWC:dword;inline;static;  
        class function SHR3:dword;inline;static;  
        class function CONG:dword;inline;static;  
        class function FIB:dword;inline;static;  
        class function KISS:dword;inline;static; 
        class function LFIB4:dword;inline;static;
        class function SWB:dword;inline;static;  
        // From XSorShift Prng's, 2003
        class function XOS:dword;inline;static;
      end;
      
      { a shorthand alias: }
      Prng = type TMarsagliaPrngs;     
      
      { Because many simulations call for uniform
        random variables in 0<x<1 or -1<x<1, here's a utility function.
        Using Signed = false will provide a random single in range (0,1), 
        while Signed true will provide one in range (-1,1).
        Simply feed it the output of one of the Prng's}
      function ScaleToFloat(const value:dword; Signed:Boolean=true):single;inline;
         
    
    implementation
      
        class function TMarsagliaPrngs.znew:dword;inline;static;
        begin
          z:=36969*(z and 65535)+(z >> 16);
          Result:= z;
        end;
            
        class function TMarsagliaPrngs.wnew:dword;inline;static;
        begin
          w:=18000*(w and 65535)+(w >> 16);
          Result := w;
        end;
        
        
        class constructor TMarsagliaPrngs.create;
        begin
          a:=224466889; b:=7584631;c:=0;v:=2463534242;w:=521288629;x:=0;y:=0;
          z:=362436069;jsr:=123456789; jcong:=380116160;  
          settable(12345,65435,34221,12345,9983651,95746118);  
        end;
        
        { Use random seeds to reset z,w,jsr,jcong,a,b, and the table t[256]}  
        class procedure TMarsagliaPrngs.settable(const i1,i2,i3,i4,i5,i6:dword);inline;static;
        var
          i:integer;
        begin
          z:=i1; w:=i2; jsr:=i3; jcong:=i4; a:=i5; b:=i6;
        { Example procedure to set the table using KISS:}
          for i:=0 to 255 do t[i]:=KISS;
        end;
        
        class function TMarsagliaPrngs.MWC:dword;inline;static;
        begin
          Result:= (znew << 16)+wnew; 
        end;
        
        class function TMarsagliaPrngs.SHR3:dword;inline;static;
        begin
          jsr:=jsr xor(jsr << 17); jsr:= jsr xor(jsr >> 13); jsr:=jsr xor(jsr << 5);
          result := jsr;
        end;
        
        class function TMarsagliaPrngs.CONG:dword;inline;static;
        begin
         jcong:=69069*jcong+1234567;
         Result := jcong;
        end;
        
        class function TMarsagliaPrngs.FIB:dword;inline;static;
        begin
           b:=a+b;a:=b-a;
           Result := a;
        end;
        
        class function TMarsagliaPrngs.KISS:dword;inline;static;
        begin
          Result:= (MWC xor CONG)+SHR3;
        end;
        
        class function TMarsagliaPrngs.LFIB4:dword;inline;static;
        begin
          inc(c);
          t[c]:=t[c]+t[(c+58) and 255]+t[(c+119) and 255]+t[(c+178) and 255];
          Result:=t[c];
        end;
        
        class function TMarsagliaPrngs.SWB:dword;inline;static;
        begin
          inc(c);bro:=ord(x<y);
          x:=t[(c+34) and 255];
          y:=t[(c+19) and 255]+bro;
          t[c] := x-y;
          Result:= t[c];
        end;
    
        class function TMarsagliaPrngs.XOS:dword;inline;static;
        var tmp:dword;
        begin
          tmp:=(x xor (x<<15)); 
          x:=y; 
          y:=z; 
          z:=w; 
          w:=(w xor (w>>21)) xor (tmp xor(tmp>>4));
          Result := w;
        end;
        
        class procedure TMarsagliaPrngs.Randomize;inline;static;
        begin
          {$WARNING This is not implemented here yet}
        end;
        
       function ScaleToFloat(const value:dword; Signed:Boolean=true):single;inline;
       begin
         case Signed of
         False:Result:= dword(value) * 2.328306e-10;
         True :Result:= longint(value) * 4.656613e-10;
         end;
      end;
         
     end.
    

### Example

The original C code contained an example, here translated to Object Pascal. It is just a proof that we get the same results as intended. 
    
    
    program marsagliatest;
    {$mode objfpc}
    { This is a test main program. It should compile and print 9 TRUE's. 
      That means it renders the same values as Marsaglia intended, given the
      seeds provided. It is basically Marsaglia's C test program in Pascal }
    uses 
      uMarsaglia;
    var
      i:integer;
      k:dword;
    begin 
      // Set the table to known values, so we can verify the results.
      // The one million iterations per prng are  not a mistake.
      // The whole program will finish in less than .5 seconds even on a Raspberry pi.
      // note the extreme lag for the FPC random.... 
      // order is important: some seeds are taken from other prng's
      Prng.settable(12345,65435,34221,12345,9983651,95746118);
      for i:=1 to 1000000 do k:=Prng.LFIB4;writeln('LFIB4 ','Pass: ',k-1064612766=0);
      for i:=1 to 1000000 do k:=Prng.SWB;  writeln('SWB   ','Pass: ',k- 627749721=0);
      for i:=1 to 1000000 do k:=Prng.KISS; writeln('KISS  ','Pass: ',k-1372460312=0);
      for i:=1 to 1000000 do k:=Prng.CONG; writeln('CONG  ','Pass: ',k-1529210297=0);
      for i:=1 to 1000000 do k:=Prng.SHR3; writeln('SHR3  ','Pass: ',k-2642725982=0);
      for i:=1 to 1000000 do k:=Prng.MWC;  writeln('MWC   ','Pass: ',k- 904977562=0);
      for i:=1 to 1000000 do k:=Prng.FIB;  writeln('FIB   ','Pass: ',k-3519793928=0);   
      for i:=1 to 1000000 do k:=Prng.XOS;  writeln('XOS   ','Pass: ',k-1110212780=0);
      for i:=1 to 1000000 do k:=Random(high(Dword));writeln('FPC   ','Pass: ',k-1314949284=0);
      // Scale to float demo 
      for i := 0 to 9 do writeln(ScaleToFloat(Prng.SHR3):2:10);
      writeln('..........................');
      for i := 0 to 9 do writeln(ScaleToFloat(Prng.SHR3, false):2:10);
    end.
    

### Tests

The above routines are tested with FIPS-140-2 and 1000 series, which is just _**indicative**_. A full Dieharder test for any of them can be found on the web. 

Note there can always be a "failure" since statistics predict that a failure can always happen by chance. Any of the algorithms with few failures is usable. Particulary the fast KISS+SWB that passes Dieharder and XOS that is blazing fast and as good as FPC's Mersenne twister. ote you can also test against Crush/BigCrush, but that test suite is not suitable for 32 bit Prng's as per their documentation. 

  
[![Selection 018.png](https://wiki.freepascal.org/images/5/52/Selection_018.png)](</File:Selection_018.png>)

### Can we have one of these as the standard random, plz?

**No you can't.**

Programming languages tend not to change their PRNG and Free Pascal has a good one. It is important for a programming language to have a predictable PRNG. Many software relies on that. It would break backwards compatibility if the PRNG in a programming language would change. This is not a minor, but a very major issue. E.g. many existing datasets would become untestible and unverifiable. Even if some of the prng's that are presented here are superior in speed and properties, because of the above I hope the default Random will never be replaced. And you should hope so too.. 

But if you need higher speed or better properties, just use one or a combination of these... 

### References

  1. [The original post by George Marsaglia](<http://www.ciphersbyritter.com/NEWS4/RANDC.HTM#369B5E30.65A55FD1@stat.fsu.edu>)  

  2. [George Marsaglia, XorShift RNG's, Journal of Statistical Software volume 8 issue 14, July 2003](<http://www.jstatsoft.org/v08/i14/paper>)
  3. [Diehard tests](<https://en.wikipedia.org/wiki/Diehard_tests>)
  4. [Dieharder tests homepage, diegarder available for most linux distro's](<http://www.phy.duke.edu/~rgb/General/dieharder.php>)
  5. [FIPS-140-2](<https://en.wikipedia.org/wiki/FIPS_140-2>)
  6. [rngtest 2, available for most linux distro's](<https://wiki.archlinux.org/index.php/Rng-tools>)



## See also

  * [Dev random](<Dev_random.md> "Dev random")
  * [Functions for descriptive statistics](<Functions_for_descriptive_statistics.md> "Functions for descriptive statistics")
  * [Generating Random Numbers](<Generating_Random_Numbers.md> "Generating Random Numbers")



### TODO

Provide X-platform randomize (other than FPC's random) based on timer, Windows RtlGenRandom and Unix /dev/random, /dev/urandom to seed the prng's

---

_Source: [https://wiki.freepascal.org/Marsaglia%27s_pseudo_random_number_generators](https://web.archive.org/web/20240308084837/https://wiki.freepascal.org/Marsaglia%27s_pseudo_random_number_generators)_
