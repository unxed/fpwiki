# Real

│ **English (en)** │

` real` is a [standard type](<Standard_type.md> "Standard type") of the [Pascal](<Pascal.md> "Pascal") programming language. Despite its name, the data type `real` only provides a “reasonable approximation” of ℝ, the set of real numbers. For example the real number [math]\displaystyle{ \sqrt{2} }[/math] may have the `real` value of `1.4` in Pascal. 

## Contents

  * 1 usage
  * 2 internal representation
  * 3 alternatives
  * 4 see also



## usage

Floating-point literals can be assigned to `real` variables. 
    
    
    program realDemo(input, output, stderr);
    
    var
    	r: real;
    
    begin
    	r := 0.0; { r becomes zero }
    	writeLn(r, ' ', r:8:4);
    	
    	r := 1e2; { r becomes 1*(10^2) [a hundred] }
    	writeLn(r, ' ', r:8:4);
    	
    	r := r / r; { r becomes one }
    	writeLn(r, ' ', r:8:4);
    end.
    

`real`, like all floating-point types, supports [`/` division operator](<Slash.md> "Slash"), while integer types support `div`-operator. 

## internal representation

The internal representation of the type `real` (i.e. number of bytes and byte ordering) and the resulting range and precision are platform dependent. 

Quote from the “FreePascal Programmer's Manual” (Chapter 8.2.5 [Floating point types](<https://www.freepascal.org/docs-html/prog/progsu158.html>)): 

> Contrary to Turbo Pascal, where the `real` type had a special internal format, under Free Pascal the `real` type simply maps to one of the other real types. It maps to the [`double` type](<Double.md> "Double") on processors which support floating point operations, while it maps to the [`single` type](<Single.md> "Single") on processors which do not support floating point operations in hardware. 

## alternatives

Instead of specifying `real` in your code, write [`ValReal`](<https://www.freepascal.org/docs-html/rtl/system/valreal.html>), a type alias [defined by](<https://gitlab.com/freepascal.org/fpc/source/-/tree/release_3_2_0/rtl/inc/systemh.inc#L123-332>) the [System unit](<System_unit.md> "System unit"). It automatically maps to the largest available floating point type available: `single`, `double` or `extended`. 

## see also

  * [Fractions](<Fractions.md> "Fractions")



  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") `real` [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/real](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/real)_
