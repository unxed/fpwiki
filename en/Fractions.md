# Fractions

│ **English (en)** │

## Contents

  * 1 What is Fractions?
  * 2 Subversion
  * 3 Features
    * 3.1 The TFraction type
      * 3.1.1 Porperties of TFraction
      * 3.1.2 Methods of TFraction
    * 3.2 Assigning and creating Fractions
    * 3.3 Supported calculations
      * 3.3.1 Overloaded operators
      * 3.3.2 Mathematical functions
    * 3.4 General purpose functions
  * 4 Conversion routines
  * 5 Example program



## What is Fractions?

Fractions is a [unit](<Unit.md> "Unit") for doing calculations with fractions.  
The unit was inspired by a [discussion on the Lazarus forum](<http://forum.lazarus.freepascal.org/index.php/topic,27805.0.html>). 

## Subversion

You can get a copy of fractions on 
    
    
    svn <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/fractions>
    

  
Or you can use viewcv to preview the files: 

<http://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/fractions/>

## Features

### The TFraction type

TFraction is an "advanced record": a [record](<Record.md> "Record") with [methods](<Method.md> "Method") and [properties](</Property> "Property"). 

#### Porperties of TFraction

  * property Numerator: [Int64](<Int64.md> "Int64"); 
    * Gets and sets the Numerator part of the fraction
  * property Denominator: Int64; 
    * Gets and sets the Denominator part of the fraction.
    * Trying to assign 0 (zero) to TFraction.Denomiator will raise an EZeroDivide [exception](<Exceptions.md> "Exceptions")



#### Methods of TFraction

  * [procedure](<Procedure.md> "Procedure") Normalize; 
    * Normalizes the fraction by dividing the Numerator and Dominatr by their [greatest common divisor](<Greatest_common_divisor.md> "Greatest common divisor"), e.g. 10/5 becomes 2/1
  * [function](<Function.md> "Function") ToString: [String](<String.md> "String"); 
    * returns the string representation of the fraction (no normalizaltion is done), e.g. when Numerator = 10 and Denominator = 5, the function returns '10/5'
  * function Resolve: String; 
    * retruns the string representation of the fraction **after normalization** , e.g. when Numerator = 4 and Denominator = 3, the function returns '1 1/3'
  * function ToFloat: [Double](<Double.md> "Double"); 
    * returns the floating point representation of the fraction, e.g. when Numerator = 1 and Denominator = 3, the function returns 0.3333333333333



### Assigning and creating Fractions

  * function Fraction(ANumerator, ADenominator: Int64): TFraction; 
    * creates the fraction ANumerator/ADenominator, e.g. Fraction(1,2) gives 1/2 (one half).
  * function Fraction(AIntPart, ANumerator, ADenominator: Int64): TFraction; 
    * creates the fraction ANumerator/ADenominator, e.g. Fraction(1,1,2) gives 1 1/2 (one and one half).



  


### Supported calculations

#### [Overloaded](<Operator_overloading.md> "Operator overloading") [operators](<Operator.md> "Operator")

  * = (equals)
  * [`<`](<Less_than.md> "Less than") (less than)
  * [`>`](<Greater_than.md> "Greater than") (greater than)
  * <= (less than or equal)
  * >= (greater than or equal)
  * [`:=`](<Becomes.md> "Becomes") (assign): this operator allows an Int64 or a String on the right hand side
  * [`+`](<Plus.md> "Plus") (add)
  * [`-`](<Minus.md> "Minus") (subtract)
  * [`*`](<Asterisk.md> "Asterisk") (multiply): overloaded for fractions and Int64
  * [`/`](<Slash.md> "Slash") (divide): overloaded for fractions and Int64
  * ** (power): only allowed for Integers (on right hand side)



  


#### Mathematical functions

  * function Min(a, b: TFraction): TFraction; [inline](<Inline.md> "Inline"); [overload](</index.php?title=Overload&action=edit&redlink=1> "Overload \(page does not exist\)"); 
    * returns a if (a <= b), otherwise retruns b
  * function Max(a, b: TFraction): TFraction; inline; overload; 
    * returns a if (a >= b), otherwise returns b
  * function InRange(const AValue, AMin, AMax: TFraction): [Boolean](<Boolean.md> "Boolean"); inline; overload; 
    * returns [True](<True.md> "True") if (AValue >= AMin) and (AValue <= AMax), otherwise returns [False](<False.md> "False")
  * function EnsureRange(const AValue, AMin, AMax: TFraction): TFraction; inline; overload; 
    * returns AMin if (AValue < AMin), otherwise returns AMax if (AValue > AMax), otherwise returns AValue
  * function Sign(const AValue: TFraction): TValueSign; inline; overload; 
    * retruns NegativeValue if (AValue < 0), otherwise returns PositiveValue if (AValue >0), otherwise returns ZeroValue
  * function IsZero(const AValue: TFraction): Boolean; overload; 
    * returns True if (AValue = 0), otherwise returns False
  * function Abs(const AValue: TFraction): TFraction; overload; 
    * return AValue if (AValue >= 0), otherwise returns -AValue;



  


### General purpose functions

  * function GreatestCommonDivisor(a, b: Int64): Int64; 
    * returns the greates common divisor of a and b
  * function Floor(D: Double): Int64; overload; 
    * returns the integer part of D (subtracted with 1, if (D < 0))



## Conversion routines

  * function FloatToFraction(Value, Precision: Double): TFraction; 
    * returns an approximation of Value in the form of a fraction, e.g. FloatTofraction(0.5,0.01) returns 1/2 (one half)
    * Precision defines the acceptable value for which (Abs(Result) - Abs(Value) <= Precision.
    * Precision is bound to Value by a magnitude of 15 at most
    * FloatToFraction in fact is a function varable which is initialized to the MF_FloatTofraction() function. You can assign your own function to it if wanted.
  * function TryFloatToFraction(Value, Precision: Double; out F: TFraction; AcceptPrecisionError: Boolean): Boolean; 
    * return value depends on AcceptPrecisionError value: 
      * if (AcceptPrecisionError = True) then the function retruns True if an approxiamtion can be found
      * if (AcceptPrecisionError = False) the functions returns True if an approximation can be found AND Abs(Abs(Result)-Abs(Value)) <= Precision
    * the value of F only makes sense if the function returns True
  * function FloatToFractionDef(Value, Precision: Double; Def: TFraction; AcceptPrecisionError: Boolean): TFraction; 
    * returns the found approximation (F) if TryFloatToFraction(Value, Precision, F, AcceptPrecisionError: Boolean) succeeds, otherwise returns Def
  * function StrToFraction(const S: String): TFraction; 
    * returns fraction that is represented in S, raises an EConvertError upon failure
  * function TryStrToFraction(const S: String; out F: TFraction): Boolean; 
    * retruns True if S can be converted to a fraction, otherwise returns False
    * the value of F only makes sense f the function returns True
  * function StrToFractionDef(const S: String; Def: TFraction): TFraction; 
    * returns the fraction represented in S if conversion succeeds, otherwise returns Def



## Example program
    
    
    program example;
    
    {$mode objfpc}{$H+}
    {$apptype console}
    
    uses
      Classes, sysutils, fractions, math;
    
    var
      F1, F2: TFraction;
      D, Prec: Double;
      i: Integer;
    begin
      F1 := Fraction(1,1,3); // 1 1/3
      F2 := Fraction(4,3);   // 4/3
      writeln('F1.ToString = ',F1.ToString); // '4/3'
      writeln('F1.Resolve  = ',F1.Resolve);  // '1 1/3'
      writeln('F1.ToFloat  = ',F1.ToFloat:16:16); // 1.3333333333333333
      writeln('F2.ToString = ',F2.ToString); // '4/3'
      writeln('(F1 = F2)   = ',F1=F2);         //True
    
      F1 := Fraction(1,2);
      F2 := Fraction(1,3);
      writeln(F1.ToString,' * ',F2.ToString,'   = ',(F1*F2).Resolve);  // '1/6'
      writeln(F1.ToString,' / ',F2.ToString,'   = ',(F1/F2).Resolve);  // '1 1/2'
      writeln(F1.ToString,' + ',F2.ToString,'   = ',(F1+F2).Resolve);  // '5/6'
      writeln(F1.ToString,' - ',F2.ToString,'   = ',(F1-F2).Resolve);  // '1/6'
      writeln(F1.ToString,' ** 2    = ',(F1**2).Resolve);  // '1/6'
    
      D := 0.25;
      F1 := FloatToFraction(D, 0.000001);
      writeln('FloatTofraction(0.25) -> ',F1.ToString);   // '1/4'
      writeln;
      writeln('Approximations of Pi:');
      writeln('                                              [Pi = ',Pi:16:16,']');
      Prec := 1.0;
      for i := 1 to 10 do
      begin
        Prec := Prec / 10;
        F2 := FloatTofraction(Pi, Prec);
        writeln('FloatTofraction(Pi,',Prec:10:10,') = ',Format('%-13s',[F2.Resolve]),'   [',F2.ToFloat:16:16,']');
      end;
    end.
    

This outputs: 
    
    
    F1.ToString = 4/3
    F1.Resolve  = 1 1/3
    F1.ToFloat  = 1.3333333333333333
    F2.ToString = 4/3
    (F1 = F2)   = TRUE
    1/2 * 1/3   = 1/6
    1/2 / 1/3   = 1 1/2
    1/2 + 1/3   = 5/6
    1/2 - 1/3   = 1/6
    1/2 ** 2    = 1/4
    FloatTofraction(0.25) -> 1/4
    
    Approximations of Pi:
                                                  [Pi = 3.1415926535897932]
    FloatTofraction(Pi,0.1000000000) = 3 1/7           [3.1428571428571428]
    FloatTofraction(Pi,0.0100000000) = 3 1/7           [3.1428571428571428]
    FloatTofraction(Pi,0.0010000000) = 3 16/113        [3.1415929203539825]
    FloatTofraction(Pi,0.0001000000) = 3 16/113        [3.1415929203539825]
    FloatTofraction(Pi,0.0000100000) = 3 16/113        [3.1415929203539825]
    FloatTofraction(Pi,0.0000010000) = 3 16/113        [3.1415929203539825]
    FloatTofraction(Pi,0.0000001000) = 3 4703/33215    [3.1415926539214212]
    FloatTofraction(Pi,0.0000000100) = 3 4703/33215    [3.1415926539214212]
    FloatTofraction(Pi,0.0000000010) = 3 4703/33215    [3.1415926539214212]
    FloatTofraction(Pi,0.0000000001) = 3 14093/99532   [3.1415926536189365]

---

_Source: [https://wiki.freepascal.org/Fractions](https://web.archive.org/web/20250115200044/https://wiki.freepascal.org/Fractions)_
