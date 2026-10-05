# A simple implementation of the Mersenne twister

## Contents

  * 1 TMersenne32
  * 2 C++ verification test:
  * 3 Verification against FPC's Random prior to 3.3.1
  * 4 Reference
  * 5 See also



### TMersenne32

This is an implementation of the Mersenne Twister that is used in FreePascal versions prior to 3.3.1  
It is verified against several mt19937 implementations in different languages, including Freepascal and C/C++.  
The implementation is fully static and threadsafe in the sense that given the same seed the sequence is the same for all threads.  
Simply call Rnd32.URand, URandf, Srand of Srandf to obtain a random value.  


If you have data that relies on mt19937 and you have a FPC version > 3.2.2 you can use this instead of the new PRNG 
    
    
    unit mersenne;
    {$mode objfpc}{$A8}{$R-}{$Q-}
    interface
    type
       TMersenne32 = class sealed
       strict private
       const 
         // Define MT19937 constants (32-bit RNG)
         N = 624;M = 397;R = 31;A = $9908B0DF;F = 1812433253;
         U = 11;S = 7;B = $9D2C5680;T = 15;C = $EFC60000;L = 18;
         MASK_LOWER = (QWORD(1) << R) - 1;
         MASK_UPPER = QWORD(1) << R;
         class var mt:array[0..N-1] of dword;
         class var index:word;
         class procedure twist;inline;static;
       public
         class constructor create;
         class procedure initialize(const seed:dword);inline;static;
         { 32 bit unsigned integer }
         class function URand:dword;inline;static;
         { 32 bit float in range 0..1 }
         class function URandf:single;inline;static;
         { 32 bit signed integer }
         class function SRand:integer;inline;static;
         {32 bit float in the range -1..1 }
         class function SRandf:single;inline;static;
       end;
       
       Rnd32 = type TMersenne32;
       
    implementation 
    
    class constructor TMersenne32.Create;
    begin
      initialize(5489); // default seed for mt19937
    end;
    
    class procedure TMersenne32.Initialize(const seed:dword);inline;static;
    var 
      i:dword;
    begin
      mt[0] := seed;
     for  i := 1 to pred(N) do 
       mt[i] := F * (mt[i - 1] xor (mt[i - 1] >> 30)) + i;
     index := N;
    end;
       
    class procedure TMersenne32.Twist;inline;static;
    var 
      i:integer;
    begin
      for i:=0 to N-M-1 do
        mt[i]:=mt[i+M] xor {twist} (((mt[i] and MASK_UPPER) or 
        (mt[i+1] and MASK_LOWER)) shr 1)xor(dword(-(mt[i+1] and 1)) and A);
      for i:=N-M to N-2 do
        mt[i]:=mt[i+(M-N)]xor{twist}(((mt[i] and MASK_UPPER) or 
        (mt[i+1] and MASK_LOWER)) shr 1)xor(dword(-(mt[i+1] and 1)) and A);
        mt[N-1]:=mt[M-1] xor {twist} (((mt[n-1] and MASK_UPPER) or (mt[0] and 
        MASK_LOWER)) shr 1)xor(dword(-(mt[0] and 1)) and A);
      index:=0;
    end;
    
    
    class function TMersenne32.URand:dword;inline;static;
    var
      i:integer;
    begin
      i := index;
      if  index >= N then
      begin
        Twist;
        i := index;
      end;
      Result := mt[i];index := i + 1;
      Result := Result xor (mt[i] >> U);
      Result := Result xor (Result << S) and B;
      Result := Result xor (Result << T) and C;
      Result := Result xor(Result >> L);
    end;
    
    class function TMersenne32.URandf:single;inline;static;
    begin
      Result := URand * 2.32830643653869628906e-10;  
    end;
    
    class function TMersenne32.SRandf:single;inline;static;
    begin
      Result :=URand * 4.6566128730773926e-010;  
    end;
    
    class function TMersenne32.SRand:integer;inline;static;
    begin
      {$ifopt D+}Result := 0;{$endif}
      Result := URand;  
    end;
    end.
    

### C++ verification test:

This code is for a 32 bit float (single) and can be used to check against FPC's Random as well, provided you cast the output to single:  
Compile this with g++ mt_cpp_float32.cpp -std=gnu++11 -o mt_cpp_float32 
    
    
    #include <iostream>
    #include <random> 
    using namespace std;
     
    int main()
    {
        mt19937 mt_rand(5489ul);
        for(int i=0;i<5;i++){
        cout << (float) mt_rand() * 2.32830643653869628906e-10 << endl;};
     
        return 0;
    }
    

Compile this with g++ mt_cpp_uint32.cpp -std=gnu++11 -o mt_cpp_uint32  
Run ./mersplusplus and compare the output with TMersenne32. This code is for 32 bit unsigned. It should be (is) equal. 
    
    
    #include <iostream>
    #include <random> 
    using namespace std;
     int main()
    {
        mt19937 mt_rand(5489u);
        for(int i=0;i<5;i++){
        cout << mt_rand() << endl;};
     
        return 0;
    }
    

Left FPC, right C++  
[![Selection 019.png](https://wiki.freepascal.org/images/6/68/Selection_019.png)](</File:Selection_019.png>)

### Verification against FPC's Random prior to 3.3.1

This test is only valid for fpc versions prior to 3.3.1 
    
    
    program mersennetest2;
    {$mode objfpc}
    uses sysutils,dateutils,mersenne;
    var 
      i:integer;
    begin
       Randseed :=5489; // default seed for mt19937
       for i := 0 to 9999999 do Rnd32.urandf;write(Rnd32.urandf :2:10,' ');writeln('Mersenne32');
       for i := 0 to 9999999 do Random ;write(single(Random) :2:10,' ');writeln('FPC Random');  
    end.
    

## Reference

  1. [Mersenne twister in WikiPedia](<https://en.wikipedia.org/wiki/Mersenne_Twister>)



## See also

  * [Dev random](<Dev_random.md> "Dev random")
  * [Functions for descriptive statistics](<Functions_for_descriptive_statistics.md> "Functions for descriptive statistics")
  * [Generating Random Numbers](<Generating_Random_Numbers.md> "Generating Random Numbers")
  * [Marsaglia's pseudo random number generators](<Marsaglia's_pseudo_random_number_generators.md> "Marsaglia's pseudo random number generators")

---

_Source: [https://wiki.freepascal.org/A_simple_implementation_of_the_Mersenne_twister](https://web.archive.org/web/20250219013554/https://wiki.freepascal.org/A_simple_implementation_of_the_Mersenne_twister)_
