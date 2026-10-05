# IEEE 754 formats

│ **English (en)** │    
****

` single`, `double` and `extended` are [FPC](<FPC.md> "FPC")'s [data types](<Data_type.md> "Data type") implementing [Pascal](<Pascal.md> "Pascal")’s [`real`](<Real.md> "Real"). All of them are implemented according IEEE standard 754, where `single` is “single-precision”, `double` is “double-precision”, and `extended` has 80 bits. 

## Contents

  * 1 single
    * 1.1 Binary floating-point format
  * 2 double
  * 3 extended
  * 4 Using the maximum precision for constants



## `single`

value range | 1.5E-45 .. 3.4E38   
---|---  
accuracy | 6-9 significant decimal digits precision   
memory requirement | 4 bytes or [32 bits](<32_bit.md> "32 bit")  
property | The single- data-type data field can hold floating-point values ​​and signed and unsigned integer values. Assigning other values ​​will result in error messages from the compiler when the program is compiled, and the compile will be aborted. That is, the executable program is not created.   
  
Definition of a data field of data type Single: 
    
    
    var
      s: single;
    

Examples of assigning valid values: 
    
    
    s := -123.45678;
    // Note: '0' is an integer literal. 0.0 is an real literal.
    // Here, FPC will make an implicit typecast from integer to single:
    s := 0;
    // a positive sign is optional
    s := 123.45678;
    

Examples of assigning invalid values: 
    
    
    s := '-123.45678';
    s := '0';
    s := '123.45678';
    

The difference between the two examples is that the upper example is the assignment of Integer and Floating literals, while the assignment of the lower example is literals of the String type. 

### Binary floating-point format

Any value stored as a single requires 32 bits, formatted as shown in the table below: 

Bits  | Usage   
---|---  
31  | Sign (0 = positive, 1 = negative)   
30 to 23  | Exponent, biased by 127   
22 to 0  | Fraction f of the number 1.f   
  
## `double`

Any value stored as a double requires 64 bits, formatted as shown in the table below: 

Bits  | Usage   
---|---  
63  | Sign (0 = positive, 1 = negative)   
62 to 52  | Exponent, biased by 1023   
51 to 0  | Fraction f of the number 1.f   
  
Example of converting from raw data to _double_ (Data is `array [0..7] of byte`): 
    
    
    function ToDouble(const Data; IntelEndianness: Boolean = False):Double;inline;
    var
      ADouble: Double absolute Data;
      AQWord: QWord absolute Result;
    begin
      Result := ADouble;
      if not IntelEndianness then
        AQWord := SwapEndian(AQWord);
    end;
    

## `extended`

`extended` is a 80-bit wide floating-point data type. There are FPUs that internally use 80 bits for increased precision. FPC allows to use this gain in precision. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If some platform does not support the `extended` data type, it will be mapped to largest available floating-point number data available, i. e. usually `double`.

## Using the maximum precision for constants

In this example: 
    
    
    var
      f: double;
      n: integer = 1758;
      m: integer = 0;
    begin
      f := n * 1.2E6 + (2*m+1) * 50E3; // 2109650048
    

FPC will interpret the constant `1.2E6` as `Single` type, because it fits into the range of the `Single` type. This leads to some precision loss in the calculation: the result is 2109650048 instead of 2109650000. 

You can use the FPC command-line flag `-CF64` to force floating point constants to have at least 64 bits precision. Or you can use `{$MINFPCONSTPREC <n>}` to force the compiler to evaluate all floating point constants always with the precision given by <n> = `32`, `64` or `DEFAULT` (`80` is not supported for implementation reasons). In FPC v3.3.1+, you can also use the directive `{$excessprecision on}` which exists also in Delphi. 

  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

  

  *[FPU]: floating-point unit

---

_Source: [https://wiki.freepascal.org/IEEE_754_formats](https://web.archive.org/web/20241212105837/https://wiki.freepascal.org/IEEE_754_formats)_
