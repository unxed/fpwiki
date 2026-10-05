# gmp

An editor has declared this article to be a stub, meaning that it needs more information. Can you help out and [add some](<gmp.md>)? If you have some useful information, you can [help](</Help:Editing> "Help:Editing") the Free Pascal Wiki by clicking on the edit box on the left and expanding this page. 

FPC 2.4.0 includes an initial version of a Free Pascal interface for the GNU Multiple Precision Arithmetic Library in packages/gmp. 

To use the bindings in your projects (and be able to build them) you have to: 

  * Include gmp in the uses clause of a program/unit source from which you will use the GMP types/functions.
  * Have the gmp library installed on your system 
    * Most, if not all, *nix systems have GMP (libgmp) installed by default but it is still necessary to have the dev package (libgmpX-dev) installed also (needed in the link stage). This package is not included by default everywhere (e.g. Ubuntu).
    * On Windows, you can get the [mingw-dynamic package](<http://cs.nyu.edu/exact/core/gmp/>) and rename it to gmp.dll.
    * On Mac OS X, gmp can be installed as documented on gmplib.org or using the package managers MacPorts or Fink.
  * Have the necessary knowledge of the [GMP documentation](<http://gmplib.org/#DOC>).



Some gmp unit tests and usage examples for both the standard and extensions bindings can be found at the INSTALLATION_PREFIX/share/doc/fpc-x.y.z/gmp/examples folder. 

## Contents

  * 1 Standard bindings & types
    * 1.1 Naming conventions
    * 1.2 Memory issues
    * 1.3 String formatting & I/O functions
      * 1.3.1 Extensions layer
  * 2 Extensions bindings & types
    * 2.1 Naming conventions
    * 2.2 Memory issues
      * 2.2.1 Good news
      * 2.2.2 Bad news
  * 3 Usage example



# Standard bindings & types

The standard bindings cover almost all of the GMP types and functions as they appear in the GMP documentation. The few exceptions are listed near the top of the gmp unit. 

## Naming conventions

The interfaced names of the FPC functions, types and parameters are identical to those in the GMP documentation with the minor deviations of the added underscore suffix to some of the (possibly) conflicting identifiers, e.g. the original documentation C function "void mpz_init (mpz_t integer)" is declared here as: 
    
    
    { Initialise integer, and set its value to 0 }
    procedure mpz_init(out integer_: mpz_t); cdecl; external LIB name '__gmpz_init';
    

## Memory issues

With the standard bindings you have direct access to the full power of the GMP without any overhead. On the same time, to avoid heap corruption and/or memory leaks, the programmer is the only one responsible for the proper initialisation of GMP variables (mpz_t, mpf_t, ... types) before passing them to functions that expect them being already in such state. The same applies to finalisation of (properly invoking *_clear on) those variables. 

Take for granted to run into heap problems and/or program crashes by doing e.g.: 

  * Passing an uninitiated GMP variable to any GMP functions other than *_init.
  * Initialising an already initialised GMP variable without previously clearing it.
  * Clearing an already cleared one. This can easily happen e.g. on a copy of a GMP variable, created by a simple assignment. GMP variables are true value types (declared as record types in gmp) and invoking a *_clear on any of the (assigned) copies creates dangling pointers in all of the other copies.



The last issue generalises to the invocation of any function that modifies the address/size of memory owned by a GMP variable. The net result is thus to avoid existence of any memory image copy (as is the case for assignment of record variables) of any GMP variable. Instead of an assignment to another variable (equals shallow copying), which will be rightfully accepted by the compiler without any warning, use GMP functions like in the following example: 
    
    
    procedure DoSomething;
    var A, B: mpz_t; // multi precision (big) integers
    begin
      mpz_init_set_si(A, 123); // This allocates mem owned by A and initialises it to the given value
      B := A;                  // Wrong: B renders invalid as soon as the mem adr/size owned by A is altered in any way
      mpz_init_set(B, A);      // Now the numeric value of B is equal to that of A. Only this is the safe way of doing it.
      try
        // Number crunching goes here
      finally
        mpz_clear(A);          // Release the mem owned by A and B
        mpz_clear(B);
      end;
    end;
    

As the interns of all GMP variables have a private pointer to a memory block allocated for that variable instance (effectively owned by the instance), the correct handling of them has the same methodology as the proper usage of New/Dispose or GetMem/FreeMem pairs, not limited to but including also the necessity of using try/finally constructs where an exception can occur between a *_init and the corresponding *_clear of any GMP variable as shown in the snippet above. 

## String formatting & I/O functions

All these functions (mp_asprintf, mp_printf, mp_snprintf, mp_sprintf, mp_scanf, mp_sscanf) have two versions with different invocation syntax: 
    
    
    // Declarations in gmp.pas
    { V1: Print to the standard output stdout. Return the number of characters written, or −1 if an error occurred. }
    function mp_printf(fmt: pchar): longint; cdecl; varargs; external LIB name '__gmp_printf';
    { V2: Print to the standard output stdout. Return the number of characters written, or −1 if an error occurred. }
    function mp_printf(fmt: pchar; args: array of const): longint; cdecl; external LIB name '__gmp_printf';
    
    // Can be used as
    var
      f: mpf_t;
      n, bits, digits: integer;
    begin  
      mp_printf('Sqrt(%d) to %d digits (%d bits) = %.*Fg'#10, n, digits, bits, digits, @f);    // V1: from printf_example.pas
      mp_printf('Sqrt(%d) to %d digits (%d bits) = %.*Fg'#10, [n, digits, bits, digits, @f]);  // V2: this produces the same behaviour as the above line
    end;
    

  * It is obligatory to pass **pointers** to GMP variables to these functions, so if a GMP variable is declared as in the snippet above, you must use the @ operator. The compiler has no way to check this, so one has to be careful here.
  * The allocated memory for the C string (PChar) returned by mp_asprintf is owned by the invoker and is not handled automatically as an Object Pascal string.
  * At least on Linux the mp_printf requires a LF character to actually show (flush) the stdout.



For further details about the formatting functions and format strings please consult the GMP manual. 

### Extensions layer

When using the [extensions layer](<gmp.md> "gmp") the above functions can be used, however a pointer to the underlying variable must be obtained using the **ptr** member function. Alternatively the types can be converted to AnsiString. 
    
    
    var
      SomeNumber: MPInteger;
    begin
      { These two produce the same output }
      mp_printf('The value of ''SomeNumber'' is: %Zd'#10, SomeNumber.ptr);
      WriteLn('The value of ''SomeNumber'' is: ', AnsiString(SomeNumber));
    end;
    

# Extensions bindings & types

The possible issues with memory management as discussed in the standard bindings section, are addressed by an extensions layer providing automated memory management for the GMP variables in the style of the Object Pascal String type. The similar approach includes also a comparable overhead both in runtime and size of the generated code as it is with (memory automated) string variables - reference counting, copy on write and protected finalisation blocks. But all of this is now handled by the compiler itself (and partially in the gmp unit) without requesting any additional effort from the programmer. 

## Naming conventions

The standard types and their extensions counterparts: 

std | ext   
---|---  
mpz_t | MPInteger   
mpq_t | MPRational   
mpf_t | MPFloat   
randstate_t | MPRandState   
  
Extensions counterparts of the standard bindings functions are named sans the "mp" or "mp_" prefix, e.g.: 

std | ext   
---|---  
mpz_add | z_add   
mpq_cmp | q_cmp   
mpf_sub | f_sub   
mp_randinit_default | randinit_default   
  
Most of the extensions functions, except for the extensions parameter/result types, are fully equal to the standard ones. There are some minor deviations, e.g. few of the standard bindings functions returning a zero/non-zero indicator value have the usual and expected Boolean type instead in the extensions version. 
    
    
      { std: Return non-zero if n is exactly divisible by d }
      function mpz_divisible_p(var n, d: mpz_t): longint; cdecl; external LIB name '__gmpz_divisible_p';
      { ext: Return true if n is exactly divisible by d }
      function z_divisible_p(var n, d: MPInteger): boolean;
    

Some of the standard bindings numeric procedures are here available as an overloaded function to allow for - among other things - a choice of style, e.g.: 
    
    
      mpz_add(A, B, C); // three ops std procedure
      z_add(D, E, F);   // dtto in ext layer
      D := z_add(E, F); // the functional, two ops overloaded alternative of the above line
    

## Memory issues

### Good news

In common (presumably most) situations you can now just forget the underlying memory handling. The equivalent of the DoSomething snippet (without the assignment operator, which is to be discussed in a later section) using the extensions layer becomes: 
    
    
    procedure DoSomethingExt;
    var A, B: MPInteger; // multi precision (big) integers
    begin
      z_init_set_si(A, 123); // This allocates mem owned by A and initialises it to the given value
      z_init_set(B, A);      // Now the numeric value of B is equal to that of A.
      // Number crunching goes here
    end;
    

Still you get exactly the same functionality as in the former case. That includes proper release of any memory owned by A and B even in the case of an exception generated in the "number crunching" section. 

### Bad news

The similarity with the string type includes the fact, that with the power of the Free Pascal language, it is anyway still possible to break the automatic memory management when certain conditions are met. Don't expect this to happen in math abstraction implementation code, but it can and without taking caution will occur on extensions numeric types I/O and also by improperly mixing the standard and extensions bindings, which is on the other side perfectly possible when done correctly. 

Things not to do or which are to be done in a specific way include e.g.: TODO 

# Usage example

For example, you use GMP on Windows 11, Lazarus 64 bits. 

  * Download the file gmp.pas from Free Pascal repository: <https://gitlab.com/freepascal.org/fpc/source/-/tree/main/packages/gmp/src>
  * Put the file in your project folder.
  * Download GMP library for Windows. For example from here: <https://github.com/gx/gmp/releases/tag/6.1.2-2> (only 6.1.2 version need MSVC 2019 redistribuable library).
  * From gmp-6.1.2-2-msvc2019-x64-Release.zip file extract libgmp-13.dll and rename it gmp.dll. Put it in your project folder or in the Windows System32 folder.



Example to use GMP in Lazarus: 
    
    
    implementation
    uses math, gmp;
    
    {$R *.lfm}
    
    procedure TForm1.Button1Click(Sender: TObject);
    const
      N = 2;
      NBITS = 100;
    var
      f: mpfloat;
      bits, digits: integer;
    begin
      f_set_default_prec(NBITS);
      bits := f_get_default_prec;
      digits := floor(bits * LOG_10_2);
      f := f_sqrt_ui(N);
      Memo1.Append(format('Sqrt(%d) to %d digits (%d bits) = %s', [N, digits, bits, AnsiString(f)]));
      f := f ** 2;
      Memo1.Append(format('Squared back = %s', [AnsiString(f)]));
      f -= N;
      Memo1.Append(format('Minus %d = %s', [N, AnsiString(f)]));
    end;

---

_Source: [https://wiki.freepascal.org/gmp](https://web.archive.org/web/20250123133255/https://wiki.freepascal.org/gmp)_
