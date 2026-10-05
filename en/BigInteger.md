# BigInteger

There are several Pascal libraries for BigInteger (and Big Decimals, like 1.23) functionality. The C-based [GMP library](<gmp.md> "gmp") can also be used in Free Pascal, it's not listed here. 

## Contents

  * 1 fpTlsBigInt in fcl-hash package in Free Pascal
  * 2 TForge
  * 3 Big Decimal Math
  * 4 FNX
  * 5 MPArith
  * 6 BigInt
  * 7 HugeInt
  * 8 LongMathForPascal
  * 9 GInt
  * 10 GNURZ
  * 11 BigIntegers for Delphi
  * 12 Multi-Word-Int
  * 13 See Also



## fpTlsBigInt in fcl-hash package in Free Pascal

  * By: Mattias Gaertner
  * Source Code: <https://gitlab.com/freepascal.org/fpc/source/-/blob/main/packages/fcl-hash/src/fptlsbigint.pas>



Refactored from Ultibo Big Integer unit Copyright (C) 2018 - SoftOz Pty Ltd.  
Inspired by AXTLS - \crypto\bigint.c - Copyright (c) 2007, Cameron Rich.  
Reference: Handbook of Applied Cryptography (Chapter 14) - <http://cacr.uwateloo.ca/hac/about/chap14.pdf>

## TForge

  * By: Sergey Kasandrov
  * Homepage: <https://github.com/Alexey-T/TForge-Pascal>



Modern cryptographic library for Delphi and Lazarus/FPC implemented as a set of runtime packages. Project development was stopped, the last found snapshot v0.76 was put to GitHub. 

## Big Decimal Math

  * By: Benito van der Zander
  * Homepage: <https://benibela.de/sources_en.html#bigdecimalmath>



This unit provides a BigDecimal-Type which stores arbitrary precision (BCD) decimal numbers and can be used like any native numeric type. 

## FNX

  * By: Marcel Martin
  * Homepage: <https://www.ellipsa.eu/public/fnx/fnx.html>



Library of multi-precision numbers (currently, only big integers) written for Free Pascal and Linux. The assembler code that can be enabled with a compiler directive, is for x86-64 processors only. 

## MPArith

  * By: Wolfgang Ehrhardt
  * Homepage: <https://github.com/Alexey-T/Wolfgang_Ehrhardt_codes/tree/master/Misc/MPArith>



Project development was stopped in about 2018. The last found snapshot was put to GitHub. 

Comment by forum user [MathMan](<https://forum.lazarus.freepascal.org/index.php?action=profile;u=55215>): 

  * Functionality 
    * supports 16/32/64 bit, little- & big-endian
    * pure functional implementation
    * dynamic sizing
    * solid error handling (function results or exceptions)
    * extensive set of functions - arith, bitops, comparisons, type conversion, base conversion
    * extensive number theory - gcd, modinv, mulmod, pow, powmod, sqrt+remainder, n-root+remainder, and a lot more


  * Algorithms 
    * basecase multiplication only 33% slower than best known pure Pascal
    * efficient multiplication: Karatsuba, Toom3
    * efficient squaring: Karatsuba, Toom3
    * efficient division: Burnikel-Ziegler
    * div&cong base conversion (both ways)
    * efficient GCD
    * efficient modular ops: based on Montgomery & Barrett reductions
    * efficient roots: quadratically converging


  * Issues 
    * default thresholds & missing tool for auto detection
    * Burnikel-Ziegler efficiency degrades on large sizes
    * no class implementation


  * Nice things 
    * simple to activate under FPC
    * accompanied by a good manual
    * extensive test coverage
    * there is also rationals, reals & complex available with large set of functions



## BigInt

  * By: Franco Milani
  * Homepage: <http://spazioinwind.libero.it/frm/software>



Though this is a Pascal source >90% are coded as x86-32 Win32 assembler routines. 

## HugeInt

  * By: David J Butler
  * Homepage: <https://github.com/fundamentalslib/fundamentals5>



Part of a much larger library but the unit can be used self-contained. 

## LongMathForPascal

  * By: Zsolt Szakaly
  * Homepage: <https://github.com/zsoltszakaly/longmathforpascal>



A quite recent contribution (2021) - unfortunately it does not support any efficient algorithms and becomes slow very fast. 

## GInt

  * By: Walied Othman
  * Homepage: <https://github.com/SnakeDoctor/FGInt>



Again part of a larger library but can be used self-contained. 

## GNURZ

  * By: Alexander Staidl
  * Homepage: <https://github.com/afriess/GNURZ>



Development was stopped in about 2009 year. Almost all identifiers are in German. Additional unit written in Assembler exists, but it's not required. 

## BigIntegers for Delphi

  * By: Rudy Velthuis
  * Homepage: <https://github.com/rvelthuis/DelphiBigNumbers>



Code can be adapted for Free Pascal. How-to from [Rudy's website](<http://www.rvelthuis.de/programs/bigintegers.html>): 
    
    
    In {$mode delphi}, it is possible to compile the units. FreePascal doesn’t seem to use segmented unit names (yet), so you’ll have to rename some of the used units and qualifiers like System.Math or System.SysUtils to simply Math or SysUtils. At first, you’ll have to {$DEFINE PUREPASCAL}, because making the assembler work satisfactorily could be quite a job. It seems to work alright, if you set {$asmmode intel}, but it is a little finicky. Stuff like LEA EAX,[EDX + EDI*CLimbSize] seems to cause problems, but [EDX + CLimbSize*EDI] works. That is a bit of a problem for me, because I most of the time use the former variety. But I guess that, with enough time on your hands, you can make it work.
    
    Some of the tricky routines, e.g. ToString, which uses RecursiveToString, which in its turn needs an exact alignment of bytes to get a usable string, don’t work, but in the meantime — until you solved the problem — you can e.g. use (or wrap or rename) .ToStringClassic(10) instead. I guess the tricky routines just need some work and some debugging to make them work under FreePascal. I only tried a quick and dirty test, did not do any extensive tests, but it is obviously possible to make it work. Just don’t give up immediately.
    
    Types like TArray don’t seem to work. And there is no way you can define an array of BigInteger before BigInteger is defined, so you will have to define a type BigIntegerArray = array of BigInteger inside the BigInteger record, as a nested type. You will meet other, similar problems. Another was the fact that there is no DivMod() function for UInt64 types, so you will have to either use the one for Longint or use div and mod separately. It could be that the change I made there is one of the causes for the failure of RecursiveToString. This needs some investigation.
    
    There will be other, simple problems and it is well possible that there are a few not-so-simple problems. But it looks as if BigIntegers can be used in FreePascal.
    

Comment by forum user [MathMan](<https://forum.lazarus.freepascal.org/index.php?action=profile;u=55215>): 

  * Functionality 
    * supports 32/64 bit, little- & big-endian
    * class implementation with operators & functional replicants
    * dynamic sizing and immutability
    * late binding
    * solid error handling
    * solid set of operators - arith, bitops, comparisons, expl & impl conversions
    * some number theory - gcd, modinv, mulmod, pow, powmod, sqrt+remainder, n-root+remainder
  * Algorithms 
    * basecase multiplication only 33% slower than best known pure Pascal
    * efficient multiplication: Karatsuba, Toom3
    * efficient squaring: Karatsuba
    * efficient division: Burnikel-Ziegler
    * div&cong base conversion (from binary to arb base only)
    * euclidean GCD
  * Issues 
    * tricky to convert to FPC
    * memory management degrades efficient algs on larger sizes (speed reduced by 5)
    * default thresholds & missing tool for auto detection
    * sqr-schoolbook = mul-schoolbook
    * missing efficient GCD algorithm
    * missing efficient mod algorithms (Montgomery, Barrett)
    * the way asm-enhancements are integrated could be improved (subjective)
    * testing depth & breadth could be improved (subjective)
  * Nice things 
    * there is also BigDecimals, BigRationals & BigFloats



## Multi-Word-Int

  * By: Mark Taylor
  * Homepage: <https://github.com/ad1mt/Multi-Word-Int> or <https://mark-taylor.me.uk/index.php?page=Software>



Designed to be: 

  * easy to use, requires minimal changes to existing code
  * written entirely in Pascal, no assembly or C language, so portable and reliable
  * works in both 64bit and 32bit environments
  * compact code base, but has limited maths functions
  * reasonably fast (although not as fast as MPArith which is around 10x faster)



NB this is a recent new development, and should be considered beta code. 

## See Also

  * [Portal:SciTech](<Portal_SciTech.md> "Portal:SciTech")

---

_Source: [https://wiki.freepascal.org/BigInteger](https://web.archive.org/web/20240914044347/https://wiki.freepascal.org/BigInteger)_
