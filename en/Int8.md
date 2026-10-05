# Integer

│ **[Deutsch (de)](</Integer/de> "Integer/de")** │  **English (en)** │  **[suomi (fi)](</Integer/fi> "Integer/fi")** │  **[français (fr)](</Integer/fr> "Integer/fr")** │  **[italiano (it)](</Integer/it> "Integer/it")** │  **[русский (ru)](<../ru/Integer.md> "Integer/ru")** │    
****

The [data type](<Data_type.md> "Data type") `integer` is a [built-in](<Standard_type.md> "Standard type") data type of the programming language [Pascal](<Pascal.md> "Pascal"). It can store a subset of ℤ, the set of whole numbers. 

## Contents

  * 1 integer literal
    * 1.1 basics
    * 1.2 varying base
  * 2 characteristics
  * 3 application
  * 4 Free Pascal deviations
  * 5 see also



## `integer` literal

### basics

An `integer` is specified as a non-empty series of consecutive Western-Arabic digits. 
    
    
    1234
    

The value of `1234` is [math]\displaystyle{ 1 \times 10^3 + 2 \times 10^2 + 3 \times 10^1 + 4 \times 10^0 }[/math]. The `integer` may be preceded by a sign, [`+`](<Plus.md> "Plus") or [`−`](<Minus.md> "Minus"), even if _mathematically speaking_ the value is signless (this concerns the value zero). 
    
    
    +0   { ✔ syntactically correct }
    

If no sign is specified, a positive sign is presumed. 

In Free Pascal, you can use the [underscore](<Underscore.md> "Underscore") to group digits if `{$modeSwitch underscoreIsSeparator+}` (as of 2022 only [available in trunk](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")). 
    
    
    1_000_000_000
    

### varying base

To change the base in [Extended Pascal](<Extended_Pascal.md> "Extended Pascal") you prefix the `integer` literal with a base specification: 
    
    
    8#1234
    

This represents the value [math]\displaystyle{ 1 \times 8^3 + 2 \times 8^2 + 3 \times 8^1 + 4 \times 8^0 }[/math]. The base can be any value between `2` and `36` (inclusive) and can _only_ be specified to a _decimal_ base. A base specification possibly extends or restricts the set of allowed digits to those in the set of `0` to `base − 1` from below table: 

`integer` digits in Extended Pascal  digit (case-insensitive)  | `0` | `1` | `2` | `3` | `4` | `5` | `6` | `7` | `8` | `9` | `A` | `B`  
---|---|---|---|---|---|---|---|---|---|---|---|---  
value (decimal)  | [math]\displaystyle{ 0 }[/math] | [math]\displaystyle{ 1 }[/math] | [math]\displaystyle{ 2 }[/math] | [math]\displaystyle{ 3 }[/math] | [math]\displaystyle{ 4 }[/math] | [math]\displaystyle{ 5 }[/math] | [math]\displaystyle{ 6 }[/math] | [math]\displaystyle{ 7 }[/math] | [math]\displaystyle{ 8 }[/math] | [math]\displaystyle{ 9 }[/math] | [math]\displaystyle{ 10 }[/math] | [math]\displaystyle{ 11 }[/math]  
digit (case-insensitive)  | `C` | `D` | `E` | `F` | `G` | `H` | `I` | `J` | `K` | `L` | `M` | `N`  
value (decimal)  | [math]\displaystyle{ 12 }[/math] | [math]\displaystyle{ 13 }[/math] | [math]\displaystyle{ 14 }[/math] | [math]\displaystyle{ 15 }[/math] | [math]\displaystyle{ 16 }[/math] | [math]\displaystyle{ 17 }[/math] | [math]\displaystyle{ 18 }[/math] | [math]\displaystyle{ 19 }[/math] | [math]\displaystyle{ 20 }[/math] | [math]\displaystyle{ 21 }[/math] | [math]\displaystyle{ 22 }[/math] | [math]\displaystyle{ 23 }[/math]  
digit (case-insensitive)  | `O` | `P` | `Q` | `R` | `S` | `T` | `U` | `V` | `W` | `X` | `Y` | `Z`  
value (decimal)  | [math]\displaystyle{ 24 }[/math] | [math]\displaystyle{ 25 }[/math] | [math]\displaystyle{ 26 }[/math] | [math]\displaystyle{ 27 }[/math] | [math]\displaystyle{ 28 }[/math] | [math]\displaystyle{ 29 }[/math] | [math]\displaystyle{ 30 }[/math] | [math]\displaystyle{ 31 }[/math] | [math]\displaystyle{ 32 }[/math] | [math]\displaystyle{ 33 }[/math] | [math]\displaystyle{ 34 }[/math] | [math]\displaystyle{ 35 }[/math]  
  
As of version 3.2.0, the [FPC](<FPC.md> "FPC") intends to ([`{$mode extendedPascal}`](<Mode_extendedpascal.md> "Mode extendedpascal")), but does not yet support a generic base-specification format. Instead only the following bases are recognized: 

`integer` base specifications in FPC 3.2.0  base | indicator | sample (decimal value)   
---|---|---  
[binary](<Binary_numeral_system.md> "Binary numeral system") ([math]\displaystyle{ 2 }[/math]) | [`%`](<Percent_sign.md> "Percent sign") | `%1010` ([math]\displaystyle{ 10 }[/math])   
octal ([math]\displaystyle{ 8 }[/math]) | [`&`](<&.md> "&") | `&644` ([math]\displaystyle{ 420 }[/math])   
decimal ([math]\displaystyle{ 10 }[/math]) | _none_ | `1337` ([math]\displaystyle{ 1337 }[/math])   
[hexadecimal](<Hexadecimal.md> "Hexadecimal") ([math]\displaystyle{ 16 }[/math]) | [`$`](<Dollar_sign.md> "Dollar sign") | `$2A` ([math]\displaystyle{ 42 }[/math])   
  
## characteristics

It is guaranteed that all arithmetic operations in the range [`−maxInt..+maxInt`](<maxint.md> "maxint") work accurately. An `integer` [variable](<Variable.md> "Variable") _may_ possibly store values _beyond_ this range, but once you leave this range it is _not guaranteed_ anymore that arithmetic operations work correctly. Textbook example: 
    
    
    program lordOverflowStrikesAgain(output);
    	{$overflowChecks on}
    	var
    		x: integer;
    	begin
    		x := -maxInt;
    		x := pred(x); { If this doesn’t cause an error, `-maxInt - 1` storable. }
    		writeLn(abs(x));
    	end.
    

Depending on the processor used, this _may_ print: 
    
    
    -9223372036854775808
    

A quite unexpected result since [`abs`](</index.php?title=Abs&action=edit&redlink=1> "Abs \(page does not exist\)") should in principle return a _non-negative_ value, yet _expectable_ since `pred(−maxInt)` is evidently not in the `−maxInt..+maxInt` range. 

## application

`Integer` is the data of choice if arithmetic results have to be _precise_. The data type [`real`](<Real.md> "Real") may provide “reasonable approximations”, but operations on `integer` have to be exact (only guaranteed as long as it is in the `−maxInt..+maxInt` range). Generally speaking `integer` operations are also _faster_ than if done in the domain of `real`. 

The operators [`div`](<Div.md> "Div") and [`mod`](<Mod.md> "Mod") only work on `integer` values (the `math` `unit` provides the [`fMod` `function`](<https://www.freepascal.org/docs-html/rtl/math/fmod.html>) and [overloads](<Operator_overloading.md> "Operator overloading") `mod`). 

## Free Pascal deviations

The FPC does not have _a single_ data type `integer` but a host of `integer` data types. An `integer` literal such as `123` possesses the data type of closest fitting range from the following table. 

integer data types in FPC version 3.2.0  name (aliases)  | smallest storable value  | largest storable value  | [`sizeOf`](<SizeOf.md> "SizeOf")  
---|---|---|---  
[`shortInt`](<https://www.freepascal.org/docs-html/rtl/system/shortint.html>) ([`int8`](<https://www.freepascal.org/docs-html/rtl/system/int8.html>))  | `-128` ([math]\displaystyle{ -2^7 }[/math])  | `127` ([math]\displaystyle{ 2^7-1 }[/math])  | 1   
[`byte`](<https://www.freepascal.org/docs-html/rtl/system/byte.html>) ([`uInt8`](<https://www.freepascal.org/docs-html/rtl/system/uint8.html>))  | `0` ([math]\displaystyle{ 0 }[/math])  | `255` ([math]\displaystyle{ 2^8-1 }[/math])  | 1   
[`smallInt`](<https://www.freepascal.org/docs-html/rtl/system/smallint.html>) ([`int16`](<https://www.freepascal.org/docs-html/rtl/system/int16.html>))  | `-32768` ([math]\displaystyle{ -2^{15} }[/math])  | `32767` ([math]\displaystyle{ 2^{15}-1 }[/math])  | 2   
[`word`](<Word.md> "Word") ([`uInt16`](<https://www.freepascal.org/docs-html/rtl/system/uint16.html>))  | `0` ([math]\displaystyle{ 0 }[/math])  | `65535` ([math]\displaystyle{ 2^{16}-1 }[/math])  | 2   
[`longInt`](<https://www.freepascal.org/docs-html/rtl/system/longint.html>) ([`int32`](<https://www.freepascal.org/docs-html/rtl/system/int32.html>))  | `-2147483648` ([math]\displaystyle{ -2^{31} }[/math])  | `2147483647` ([math]\displaystyle{ 2^{31}-1 }[/math])  | 4   
[`longWord`](<https://www.freepascal.org/docs-html/rtl/system/longword.html>) ([`cardinal`](<https://www.freepascal.org/docs-html/rtl/system/cardinal.html>), [`dWord`](<https://www.freepascal.org/docs-html/rtl/system/dword.html>))  | `0` ([math]\displaystyle{ 0 }[/math])  | `4294967295` ([math]\displaystyle{ 2^{32}-1 }[/math])  | 4   
[`int64`](<https://www.freepascal.org/docs-html/rtl/system/int64.html>) | `-9223372036854775808` ([math]\displaystyle{ -2^{63} }[/math])  | `9223372036854775807` ([math]\displaystyle{ 2^{63}-1 }[/math])  | 8   
[`qWord`](<https://www.freepascal.org/docs-html/rtl/system/qword.html>) ([`uInt64`](<https://www.freepascal.org/docs-html/rtl/system/uint64.html>))  | `0` ([math]\displaystyle{ 0 }[/math])  | `18446744073709551615` ([math]\displaystyle{ 2^{64}-1 }[/math])  | 8   
  
The signed ranges are preferred (i. e. as in the top/down order in the table), thus `123` possesses the data type `shortInt` even though it could be a `byte`, too. 

As of version 3.2.0, the data type [`integer`](<https://www.freepascal.org/docs-html/rtl/system/integer.html>) is simply an alias depending on the currently selected [compiler compatibility mode](<Compiler_Mode.md> "Compiler Mode"). It does _not_ depend on the CPU type, therefore it is quite possible that the CPU could in fact deal with integers having an even larger magnitude than `integer` provides. 

the data type `integer` in FPC version 3.2.0  mode  | `integer` is an alias for  | value of `maxInt`  
---|---|---  
[`{$mode FPC}`](<Mode_FPC.md> "Mode FPC"), [`{$mode macPas}`](<Mode_MacPas.md> "Mode MacPas") and [`{$mode TP}`](<Mode_TP.md> "Mode TP") | `smallInt` | `32767`  
_all other available modes_ | `longInt` | `2147483647`  
  
[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Undocumented feature: General programming advice, do not use what has not been documented. Unlikely as it may be, the following feature may be removed at any time.

Depending on the [platform’s](<Platform_defines.md> "Platform defines") arithmetic logic unit’s word size (ALU) following [aliases are available](<https://gitlab.com/freepascal.org/source/fpc/release_3_2_0/rtl/inc/systemh.inc#L390-L412>). 
    
    
    type
    	{$ifDef CPU16} ALUSInt = smallInt; ALUUInt = word;  {$endIf}
    	{$ifDef CPU32} ALUSInt = longInt;  ALUUInt = dWord; {$endIf}
    	{$ifDef CPU64} ALUSInt = int64;    ALUUInt = qWord; {$endIf}
    

Although it is frequently the case that the ALU’s word size also coincides with the size of a pointer, it is not guaranteed (e. g. the x32-ABI uses 64‑bit ALU, but only 32‑bit pointers). Therefore if an `integer` value is meant to be [typecasted](<Typecast.md> "Typecast") to a [`pointer`](<Pointer.md> "Pointer"), it recommended to use [`ptrUInt`](<https://www.freepascal.org/docs-html/rtl/system/ptruint.html>). Note, the data type [`nativeInt`](<https://www.freepascal.org/docs-html/rtl/system/nativeint.html>) is in fact an alias for [`ptrInt`](<PtrInt.md> "PtrInt") and [not related to `ALUSInt`](<https://gitlab.com/freepascal.org/fpc/source/-/tree/release_3_2_0/rtl/inc/systemh.inc#L414-422>). 

## see also

  * [`real`](<Real.md> "Real"), the data type used to store/process (a subset of) rational numbers
  * [GNU Multiple Precision Arithmetic Library](<gmp.md> "gmp"), precise mathematical operations on arbitrarily large values [if 8-Byte `int64`/`qWord` are not enough]



  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") `integer` [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/Int8](https://web.archive.org/web/20250422075358/https://wiki.freepascal.org/Int8)_
