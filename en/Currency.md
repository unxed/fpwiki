# Currency

│ **[Deutsch (de)](</Currency/de> "Currency/de")** │  **English (en)** │  **[suomi (fi)](</Currency/fi> "Currency/fi")** │  **[français (fr)](</Currency/fr> "Currency/fr")** │  **[русский (ru)](<../ru/Currency.md> "Currency/ru")** │    
****

The `Currency` type is a real [data type](<Data_type.md> "Data type") with 4 digits to the right of the decimal point and a range of -922337203685477.5808 to 922337203685477.5807 . The purpose of the `Currency` data type is to give arithmetic results that exactly correspond to decimal calculations on the input values. 

Real values are normally stored in the [binary number system](<Binary_numeral_system.md> "Binary numeral system") internally, and calculations are performed in the CPU in binary arithmetic. Since humans desire input and output numbers to be in decimal number format, there must be a conversion made between external decimal numbers and their binary internal representation. As a result of the conversion to/from binary numbers and the calculations being done in binary arithmetic, the results of normal real arithmetic can differ from a decimal arithmetic calculation. This discrepancy is not critical in many applications, but _financial applications_ want their arithmetic operations to match a decimal arithmetic calculation. The `currency` data type is designed to give arithmetic results corresponding to decimal arithmetic on the real values given. 

## See also

  * [function](<Function.md> "Function") [`CurrToStr`](<https://www.freepascal.org/docs-html/rtl/sysutils/currtostr.html>)
  * function [`FormatCurr`](<https://www.freepascal.org/docs-html/rtl/sysutils/formatcurr.html>)
  * function [`StrToCurr`](<https://www.freepascal.org/docs-html/rtl/sysutils/strtocurr.html>)



  
  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") `currency` [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/Currency](https://web.archive.org/web/20240920204107/https://wiki.freepascal.org/Currency)_
