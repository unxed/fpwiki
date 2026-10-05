# complex number

│ **English (en)** │

Complex numbers is a mathematical concept providing solutions to equations such as [math]\displaystyle{ x^2 = -1 }[/math]. In [FPC](<FPC.md> "FPC")'s default [runtime library](<RTL.md> "RTL") the unit `uComplex` defines a type `complex` and lots of operator and other functions. `u` in `uComplex` stands for the Greek letter μ, meaning “micro”, as the implementation is kept as simple as possible. 

In [Extended Pascal](<Extended_Pascal.md> "Extended Pascal"), which FPC plans to implement one day, the data type `complex` is defined as part of the language. 
    
    
    program ComplexDemo;
    uses
    	uComplex;
    var
    	x, y: complex;
    begin
    	// specifying real and imaginary part
    	x := -5 + 2 * i;
    	
    	// specifying magnitude and phase angle
    	// y := sqrt(2) * (cos(pi/4) + i * sin(pi/4))
    	y.re :=  1;
    	y.im :=  1;
    	
    	x := x + y;
    	// there is no toString functionality:
    	writeLn('x = ', x.re, ' + ', x.im, 'i');
    end.
    

Another demo, which shows how to use functions `cinit` (assign value), `cstr` (convert value to string), `csin` (sinus): 
    
    
    program ComplexDemo2;
    uses
      uComplex;
    const
      L = 0;
      D = 3;
    var
      c1, c2: complex;
    begin
      c1 := cinit(1.0, 1.0);
      c2 := cinit(2.0, -2.0);
      WriteLn('c1      = ' + cstr(c1, L, D));
      WriteLn('c2      = ' + cstr(c2, L, D));
      WriteLn('c1+c2   = ' + cstr(c1+c2, L, D));
      WriteLn('c1-c2   = ' + cstr(c1-c2, L, D));
      WriteLn('c1*c2   = ' + cstr(c1*c2, L, D));
      WriteLn('c1/c2   = ' + cstr(c1/c2, L, D));
      WriteLn('sin(c1) = ' + cstr(csin(c1), L, D));
      ReadLn;
    end.
    

## see also

  * [DMath](<DMath.md> "DMath"), a mathematical library also containing a complex number implementation.
  * [LMath](<LMath.md> "LMath"), further development of LMath library, with completer support for complex numbers, where operators over them are defined.
  * [NumLib documentation](<NumLib_Documentation.md> "NumLib Documentation"), where `typ.complex` is an [`object`](<Object.md> "Object").



## external references

  * [“complex number” on Wikipedia, the free online encyclopedia](<https://en.wikipedia.org/wiki/Complex_number>)
  * [`packages/rtl-extra/src/inc/ucomplex.pp` source code](<https://gitlab.com/freepascal.org/fpc/source/-/blob/main/packages/rtl-extra/src/inc/ucomplex.pp>)
  * [`packages/numlib/src/typ.pas` source code](<https://gitlab.com/freepascal.org/fpc/source/-/blob/main/packages/numlib/src/typ.pas>)

---

_Source: [https://wiki.freepascal.org/complex_number](https://web.archive.org/web/20241213024832/https://wiki.freepascal.org/complex_number)_
