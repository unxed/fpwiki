# Porting from FPC/Delphi to pas2js

This page is for Delphians and FPC porting code to pas2js. It contains useful tips and common traps. 

## Contents

  * 1 Numbers
  * 2 Strings
  * 3 Asynchronous vs Waiting
  * 4 Details



# Numbers

  * Internally all numbers are _double_. Therefore integers range from -9,007,199,254,740,991 to 9,007,199,254,740,991, so about 1/1024th of a 64bit value.
  * There is **no** _Int64_ and no _QWord_.
  * All **bitwise operators are limited to 32bit** , including the _mod_ operator, which is limited to signed 32bit.
  * **Integers overflows'_at runtime differ from Delphi/FPC. For example adding_ var i: byte = 200; ... i:=i+100;** will result in _i=300_ instead of _i=44_ as in Delphi/FPC. When range checking _{$R+}_ is enabled _i:=300_ will raise an _ERangeError_.
  * **Division by zero** does not raise _EDivByZero_ , instead it results in _NaN_.
  * **Currency** has only 54 bits. Currency is internally a double, multiplied by 10000 and truncated. The below values are the safe limits, within every step exists. Since currency is a double it can take much larger values, but the result may differ from Delphi/FPC: 
    * MaxCurrency = 900719925474.0991; // fpc: 922337203685477.5807;
    * MinCurrency = -900719925474.0991; // fpc: -922337203685477.5808;



# Strings

  * String is UnicodeString and there are no other string types.
  * Strings are immutable in JS. That means changing a single character creates a new string. That's why some fast Delphi/FPC string functions are much slower in pas2js.



# Asynchronous vs Waiting

JavaScript enforces an absolute async manner of programming: 

  * Many calls are asynchronous and return immediately. For example loading a resource.
  * There is **no** Application.ProcessMessages. You cannot wait till some event occurs. You must set an event. That's why anonymous functions are so frequently used in JS - they keep the local variables accessible.
  * There is no multithreading, no shared memory. Many browsers/JS engines support webworkers, but that is more like processes than threads.



# Details

A more detailed list can be found in the [translation.html](<https://gitlab.com/freepascal.org/fpc/source/-/tree/3.3.1/utils/pas2js/docs/translation.html>).file in the sources.

---

_Source: [https://wiki.freepascal.org/Porting_from_FPC/Delphi_to_pas2js](https://web.archive.org/web/20250314122359/https://wiki.freepascal.org/Porting_from_FPC/Delphi_to_pas2js)_
