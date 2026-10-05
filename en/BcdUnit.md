# BcdUnit

## Contents

  * 1 Welcome
  * 2 Types
    * 2.1 Precision
    * 2.2 SignSpecialPlaces
    * 2.3 Fraction
  * 3 test cases
    * 3.1 integers
    * 3.2 floats
  * 4 Const
  * 5 Exceptions
  * 6 Utility
  * 7 Arithmetic
  * 8 Creation
  * 9 Conversions
    * 9.1 to BCD
    * 9.2 from BCD
  * 10 Resources



## Welcome

Welcome to information page about FMTBcd. Goal of this page is to contain information needed for implementing FPC version of the FMTBcd. 

  


* * *

FMTBcd unit defines a number of binary-coded decimal (BCD) routines: 

## Types
    
    
    TBcd  = packed record
       Precision: Byte;
       SignSpecialPlaces: Byte;
       Fraction: packed array [0..31] of Byte;
     end;
    
    
    
    PBcd: pointer to TBcd
    

### Precision

Precision states how many decimal digits are valid out of the 64 nibbles available for use. 

### SignSpecialPlaces

SignSpecialPlaces is three fields packed into a byte. 

  * The first bit is the sign (bit value 128).
  * The second bit is 'Special' (So special, I can't work out what it's for).
  * The third - eights bits contain the number of decimals (less the number of default digits???). 6 bits gives us up to 32 digits.



### Fraction

array of BCD Nibbles, 00..99 per Byte, high Nibble 1st 

Can anybody correct the following test cases? 

    

    What do you mean by 'correct'?

## test cases

### integers

If you look at a Tbdc record in the debugger, it will display the Precision as an integer, the SignSpecialPlaces as an interger and the Fraction as an array of integers. The fraction parts of the TBCD are really arrays of nibbles - one nibble per digit, two per byte. Following are some 'debug displays' and their translations. There is a space between bytes. So... 

zero is boring... 
    
    
          0 -> (Precision: 0;SignSpecialPlaces: 0;  Fraction:(0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
    

1..3 
    
    
          1 -> (Precision: 1;SignSpecialPlaces: 0;  Fraction:(16,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
          1 -> (Precision: 1;SignSpecialPlaces: 0;  Fraction:(1,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,...
    
    
    
          2 -> (Precision: 1;SignSpecialPlaces: 0;  Fraction:(32,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
          2 -> (Precision: 1;SignSpecialPlaces: 0;  Fraction:(2,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,...
    
    
    
          3 -> (Precision: 1;SignSpecialPlaces: 0;  Fraction:(48,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
          3 -> (Precision: 1;SignSpecialPlaces: 0;  Fraction:(3,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,...
    
    
    
    etc...
          4 -> (Precision: 1;SignSpecialPlaces: 0;  Fraction:(64,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
          5 -> (Precision: 1;SignSpecialPlaces: 0;  Fraction:(80,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
          6 -> (Precision: 1;SignSpecialPlaces: 0;  Fraction:(96,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
          7 -> (Precision: 1;SignSpecialPlaces: 0;  Fraction:(112,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
          8 -> (Precision: 1;SignSpecialPlaces: 0;  Fraction:(128,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
          9 -> (Precision: 1;SignSpecialPlaces: 0;  Fraction:(144,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
    

10 - note: the fractional part is the same as for 1, the precision is now 2. i.e. 10 = 1 * 10^2 
    
    
         10 -> (Precision: 2;SignSpecialPlaces: 0;  Fraction:(16,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
         10 -> (Precision: 2;SignSpecialPlaces: 0;  Fraction:(1,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,...
    

100 - note: the fractional part is the same as for 1, the precision is now 3. i.e. 10 = 1 * 10^3 
    
    
        100 -> (Precision: 3;SignSpecialPlaces: 0;  Fraction:(16,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
        100 -> (Precision: 2;SignSpecialPlaces: 0;  Fraction:(1,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,...
    etc
       1000 -> (Precision: 4;SignSpecialPlaces: 0;  Fraction:(16,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
       1000 -> (Precision: 4;SignSpecialPlaces: 0;  Fraction:(1,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,...
    

some more interesting cases: 
    
    
       1111 -> (Precision: 4;SignSpecialPlaces: 0;  Fraction:(17,17,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
       1111 -> (Precision: 4;SignSpecialPlaces: 0;  Fraction:(1,1, 1,1, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,...
    
    
    
       1234 -> (Precision: 4;SignSpecialPlaces: 0;  Fraction:(18,52,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
       1234 -> (Precision: 4;SignSpecialPlaces: 0;  Fraction:(1,2, 3,4, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,...
    

  

    
    
         -5 -> (Precision: 1;SignSpecialPlaces: 128;Fraction:(80,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
         -5 -> (Precision: 1;SignSpecialPlaces: 128;Fraction:(5,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,... 
    
    
    
         -6 -> (Precision: 1;SignSpecialPlaces: 128;Fraction:(96,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
         -6 -> (Precision: 1;SignSpecialPlaces: 128;Fraction:(6,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,... 
    
    
    
       9999 -> (Precision: 4;SignSpecialPlaces: 0;  Fraction:(153,153,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
       9999 -> (Precision: 4;SignSpecialPlaces: 0;  Fraction:(9,9, 9,9, 9,9, 9,9, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,... 
    
    
    
      -9999 -> (Precision: 4;SignSpecialPlaces: 128;Fraction:(153,153,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
      -9999 -> (Precision: 4;SignSpecialPlaces: 128;  Fraction:(9,9, 9,9, 9,9, 9,9, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,... 
    

  
Displaying the structure in HEX helps...(you should never see any hex digits in the fraction part - only 0..9 is used) 
    
    
      32767 -> (Precision: 5;SignSpecialPlaces: 0;  Fraction:($32,$76,$70,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
      32767 -> (Precision: 5;SignSpecialPlaces: 0;  Fraction:(3,2, 7,6, 70,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,...
    
    
    
     -32768 -> (Precision: 5;SignSpecialPlaces: 128;Fraction:($32,$76,$80,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
     -32768 -> (Precision: 5;SignSpecialPlaces: 128;Fraction:(3,2, 7,6, 80,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,...
    
    
    
      2147483647 -> (Precision:10;SignSpecialPlaces: 0;  Fraction:($21,$47,$48,$36,$47,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
      2147483647 -> (Precision:10;SignSpecialPlaces: 0;  Fraction:(2,1, 4,7, 4,8, 3,6, 4,7, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,...
    
    
    
     -2147483647 -> (Precision:10;SignSpecialPlaces: 128;Fraction:($21,$47,$48,$36,$47,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
     -2147483647 -> (Precision:10;SignSpecialPlaces: 128;Fraction:(2,1, 4,7, 4,8, 3,6, 4,7, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0, 0,0,...
    

### floats
    
    
      -0.01234567 ->  (Precision:8; SignSpecialPlaces: 136 {$80+8} Fraction: ( 1,35,69,103, ... 0,0)
    

## Const
    
    
     NullBcd: TBcd = (Precision: 0; SignSpecialPlaces: 0; Fraction:(0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0));
    

it's a zero (according to Delphi documentation: "Use NullBcd to indicate an unknown or missing TBcd value. NullBcd is a TBcd with all its fields set to 0. ") 

## Exceptions
    
    
     EBcdException, EBcdOverflowException
    

?? 
    
    
      EBcdException = Class(Exception);
      EBcdOverflowException = Class(Exception);
    

## Utility
    
    
     function BcdPrecision(const Bcd: TBcd): Word;
     function BcdScale(const Bcd: TBcd): Word;
     function IsBcdNegative(const Bcd: TBcd): Boolean;
    

?? 

## Arithmetic
    
    
     procedure BcdAdd(const bcdIn1, bcdIn2: TBcd; var bcdOut: TBcd);
     procedure BcdSubtract(const bcdIn1, bcdIn2: TBcd; var bcdOut: TBcd);
     function NormalizeBcd(const InBcd: TBcd; var OutBcd: TBcd; const Prec, Scale: Word): Boolean;
    

Returns True if successful, False if Int Digits needed to be truncated 
    
    
     procedure BcdMultiply(const bcdIn1, bcdIn2: TBcd; var bcdOut: TBcd); overload;
     procedure BcdMultiply(const bcdIn: TBcd; const DoubleIn: Double; var bcdOut: TBcd); overload;
     procedure BcdMultiply(const bcdIn: TBcd; const StringIn: string; var bcdOut: TBcd); overload;
     procedure BcdMultiply(StringIn1, StringIn2: string; var bcdOut: TBcd); overload;
     procedure BcdDivide(Dividend, Divisor: string; var bcdOut: TBcd); overload;
     procedure BcdDivide(const Dividend, Divisor: TBcd; var bcdOut: TBcd); overload;
     procedure BcdDivide(const Dividend: TBcd; const Divisor: Double; var bcdOut: TBcd); overload;
     procedure BcdDivide(const Dividend: TBcd; const Divisor: string; var bcdOut: TBcd); overload;
    

## Creation
    
    
     procedure VarFMTBcdCreate(var ADest: Variant; const ABcd: TBcd); overload;
     function VarFMTBcdCreate: Variant; overload;
     function VarFMTBcdCreate(const AValue: string; Precision, Scale: Word):Variant; overload;
     function VarFMTBcdCreate(const AValue: Double; Precision: Word; Scale: Word ): Variant; overload;
     function VarFMTBcdCreate(const ABcd: TBcd): Variant; overload;
     function VarIsFMTBcd(const AValue: Variant): Boolean; overload;
     function VarFMTBcd: TVarType;
    

## Conversions
    
    
     StrToBcd, TryStrToBcd, DoubleToBcd, DoubleToBcd, IntegerToBcd,
     VarToBcd, CurrToBCD, BcdToStr, BcdToDouble, BcdToInteger,
     BCDToCurr, BcdToStrF, FormatBcd, BcdCompare
    

### to BCD

Convert String/Double/Integer to BCD struct 
    
    
     function StrToBcd(const AValue: string): TBcd;
     function TryStrToBcd(const AValue: string; var Bcd: TBcd): Boolean;
     function DoubleToBcd(const AValue: Double): TBcd; overload;
     procedure DoubleToBcd(const AValue: Double; var bcd: TBcd); overload;
     function IntegerToBcd(const AValue: Longint): TBcd;
     function VarToBcd(const AValue: Variant): TBcd;
     function CurrToBCD(const Curr: Currency; var BCD: TBcd; Precision: Integer; Decimals: Integer): Boolean;
     
    

### from BCD

Convert Bcd struct to string/Double/Integer 
    
    
     function BcdToStr(const Bcd: TBcd): string; overload;
     function BcdToDouble(const Bcd: TBcd): Double;
     function BcdToInteger(const Bcd: TBcd; Truncate: Boolean): Longint;
     function BCDToCurr(const BCD: TBcd; var Curr: Currency): Boolean;
     function BcdToStrF(const Bcd: TBcd; Format: TFloatFormat; const Precision, Digits: Integer): string;
    

Formatting Bcd as string 
    
    
     function FormatBcd(const Format: string; Bcd: TBcd): string;
     function BcdCompare(const bcd1, bcd2: TBcd): Integer;
    

Warnings: 1\. All these functions can accept integer as a parameter, but they produce a wrong result. For converting integers, use 
    
    
     function BCDToInt(Value: Integer):Integer; 
    

from the _sysutils_ unit. 

2\. Converting invalid BCD arrays cause exceptions. 

## Resources

  * [Additional information](<http://homepages.borland.com/efg2lab/Library/Delphi/MathInfo/>) available from efg's Computer Lab ([archived](<https://web.archive.org/web/20080208085333/http://homepages.borland.com/efg2lab/Library/Delphi/MathInfo/>) at archive.org).

---

_Source: [https://wiki.freepascal.org/BcdUnit](https://web.archive.org/web/20250516134246/https://wiki.freepascal.org/BcdUnit)_
