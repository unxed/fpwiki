# Delphi compatible LCG Random

### Delphi compatible random numbers

Many Free Pascal programmers also maintain sourcecode in Delphi.  
Even if you have moved to Free Pascal from Delphi you may have data that relies on Delphi's Random.  
Here are cross-platform functions that generate Delphi-identical pseudo-random numbers given the same RandSeed: 
    
    
    unit lcg_random;
    // Delphi compatible LCG random number generator routines for Free Pascal.
    // (c)2017, Thaddy de Koning. Use as you like
    // Algorithm, Delphi multiplier and increment taken from:
    // https://en.wikipedia.org/wiki/Linear_congruential_generator
    // The default Delphi RandomSeed is determined as zero.
    {$ifdef fpc}{$mode objfpc}{$endif}
    
    interface
    
    function LCGRandom: extended; overload;inline;
    function LCGRandom(const range:longint):longint;overload;inline;
    
    implementation
    
    function IM:cardinal;inline;
    begin
      RandSeed := RandSeed * 134775813  + 1;
      Result := RandSeed;
    end;
    
    function LCGRandom: extended; overload;inline;
    begin
      Result := IM * 2.32830643653870e-10;
    end;
    
    function LCGRandom(const range:longint):longint;overload;inline;
    begin
      Result := IM * range shr 32;
    end;
    
    end.
    

  
A small demo 
    
    
    program lcgdemo;
    // Compile and run in both Delphi and FPC and compare the output.
    // It is exactly the same
    {$ifdef fpc}{$mode objfpc}
    {$Macro on}{$define random := LCGRandom}
    {$endif}
    {$ifdef mswindows}{$apptype console}{$endif}
    uses lcg_random;
    
    var i:integer;
    begin
      RandSeed := 999;  // default delphi randseed is zero
      for i := 1 to 20 do
      begin
        write(Random(100):4); // Delphi: Random FPC:LCGRandom. See macro
        if i mod 5 = 0 then writeln;
      end;
    
      RandSeed := 999;  // default delphi randseed is zero
      for i := 1 to 20 do
      begin
        write(Random:4:8,' ');// Delphi: Random FPC:LCGRandom
        if i mod 5 = 0 then writeln;
      end;
      Readln;
    end.
    

Here is another one with the randseed factored out. You can use that for non-delphi compatible purposes: 
    
    
    unit lcg_random2;
    // Delphi compatible LCG random number generator routines for Freepascal.
    // (c)2017, Thaddy de Koning. Use as you like
    // Algorithm, Delphi multiplier and increment taken from:
    // https://en.wikipedia.org/wiki/Linear_congruential_generator
    // The default Delphi RandomSeed is determined as zero.
    //
    // In this version the system randseed is factored out.
    {$ifdef fpc}{$mode objfpc}{$endif}{$J+}
    
    interface
    const 
      RandSd:cardinal = 0; 
      
    function LCGRandom: extended; overload;inline;
    function LCGRandom(const range:longint):longint;overload;inline;
    
    implementation
    
    function IM:cardinal;inline;
    begin
      RandSd := RandSd * 134775813  + 1;
      Result := RandSd;
    end;
    
    function LCGRandom: extended; overload;inline;
    begin
      Result := IM * 2.32830643653870e-10;
    end;
    
    function LCGRandom(const range:longint):longint;overload;inline;
    begin
      Result := IM * range shr 32;
    end;
    
    end.
    

You can use this for example for simple encryption and decryption based on the predictability and repeatability of a any Pseudo Random Number Generator: 
    
    
    program cryptrandom;
    {$mode objfpc}{$modeswitch typehelpers}{$H+}
    { This is demo that you can actually use random to encrypt/decrypt.
      This is due to the nature of a Pseudo Random Number Generator,
      where given a certain random seed, the sequence is predictable.
      Here I use the Random Seed as a key to encrypt/decrypt a string.
      The same code would also work with the Default random from FreePascal,
      but it is better not to touch system.randseed, So I used a different
      Random.
      
      Have fun, Thaddy}
    uses 
    lcg_random2;
    
      
    function Crypt(const value:AnsiString;const Key:Cardinal):AnsiString;
    var
      i:integer;
    begin
      RandSd := Key;
      SetLength(Result,Length(Value));
      for i := 1 to length(Value) do
        Result[i] := Chr(Ord(Value[i]) xor lcgrandom(255));
    end;
    
    var 
     a:AnsiString;
    begin
     a:='This is the text to encrypt/decrypt.';
     writeln(a);
     writeln('Encrypting..');
     a:=Crypt(a, 12345);
     writeln(a);
     writeln('Decrypting..');
     a:=Crypt(a, 12345); 
     writeln(a);
    end.
    

### See also

  1. [LCG on wikipedia](<https://en.wikipedia.org/wiki/Linear_congruential_generator>)
  2. [Generating Random Numbers](<Generating_Random_Numbers.md> "Generating Random Numbers")



## See also

  * [Wikipedia article on the linear congruential generator algorithm](<http://en.wikipedia.org/wiki/Linear_congruential_generator>)

---

_Source: [https://wiki.freepascal.org/Delphi_compatible_LCG_Random](https://web.archive.org/web/20250122071406/https://wiki.freepascal.org/Delphi_compatible_LCG_Random)_
