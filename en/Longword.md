# LongWord

│ **English (en)** │    
****

A **[`longWord`](<https://www.freepascal.org/docs-html/rtl/system/longword.html>)** is an unsigned integer data type which is _larger_ than a [`word`](<Word.md> "Word").[[1]](<https://www.gnu-pascal.de/gpc/LongWord.html>) Larger refers to both the permissible range of values and the [size](<SizeOf.md> "SizeOf") occupied in a [`packed`](<Packed.md> "Packed") structure. 

## Contents

  * 1 Definition
    * 1.1 GNU Pascal Compiler
    * 1.2 Free Pascal Compiler
  * 2 Application
  * 3 See Also



## Definition

### GNU Pascal Compiler

In [GNU Pascal](<GNU_Pascal.md> "GNU Pascal") a `longWord` is compatible to GNU [C](<Pascal_for_C_users.md> "Pascal for C users")’s [`long long unsigned int`](<https://www.freepascal.org/docs-html/rtl/ctypes/culonglong.html>). On _some_ platforms it is 64 bits wide and thus has a range of `0..18446744073709551615` ([math]\displaystyle{ \mathbb{Z} \cap \left[0, 2^{64}\right) }[/math]). 

### Free Pascal Compiler

For compatibility with [Delphi](<Delphi.md> "Delphi"), the [FPC](<FPC.md> "FPC") _always_ defines `longWord` as a 32-bit quantity, thus it may assume a value in the range `0..4294967295` ([math]\displaystyle{ \mathbb{Z} \cap \left[0, 2^{32}\right) }[/math]). The defunct [`{$mode GPC}`](<Mode_GPC.md> "Mode GPC") [compiler compatibility mode](<Compiler_Mode.md> "Compiler Mode") did not change this. In this definition, [`dWord`](<https://www.freepascal.org/docs-html/rtl/system/dword.html>) (double word) is an alias for `longWord`. As of version 3.2.0 the data type [`cardinal`](<Cardinal.md> "Cardinal") is an _unconditional_ alias for `longWord`. 

## Application

  * Because the FPC’s definition is fixed in size, it is frequently used when defining the _exact_ memory layout of [`record`](<Record.md> "Record") data types that will be passed to foreign language libraries. If the external library is written C, use of the [`cTypes unit`](<https://www.freepascal.org/docs-html/rtl/ctypes/index.html>) is recommended.
  * Likewise, the GPC guarantees compatibility of certain integer data types with GNU C.[[2]](<https://www.gnu-pascal.de/gpc/Summary-of-Integer-Types.html>) For maximum range, the GPC also defines `longestWord`.



## See Also

  * [`longInt`](<Longint.md> "Longint")
  * [`pLongWord`](<https://www.freepascal.org/docs-html/rtl/system/plongword.html>)
  * [`types.tLongWordDynArray`](<https://www.freepascal.org/docs-html/rtl/types/tlongworddynarray.html>)



  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/Longword](https://web.archive.org/web/20250517153459/https://wiki.freepascal.org/Longword)_
