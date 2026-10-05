# Pascal for C users

│ **English (en)** │

## Contents

  * 1 Overview
  * 2 Translation
    * 2.1 C++
  * 3 See also



## Overview

This page gives some translations between C(++) and Pascal concepts. 

## Translation

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Logical operators (AND/OR/XOR etc) are logical or boolean depending on the type of the arguments (boolean or integer). Since Pascal had a separate boolean type from the start, boolean operators on integers were not necessary, and are always logical.

C | [Pascal](<Pascal.md> "Pascal") | Notes   
---|---|---  
{  |  [Begin](<Begin.md> "Begin") |   
}  |  [End](<End.md> "End") |   
=  |  :=  |  [Becomes](<Becomes.md> "Becomes")  
==  |  =  |  [Equal](<Equal.md> "Equal")  
/  |  /  |  [Float Division](<Slash.md> "Slash") (Integer: [div](<Div.md> "Div"))   
%  |  [Mod](<Mod.md> "Mod") |  **Mod** ulo operation   
!  |  [Not](<Not.md> "Not") |  Logical not   
!=  |  <> |  [Not equal](<Not_equal.md> "Not equal")  
&& |  [And](<And.md> "And") |  Logical and   
||  |  [Or](<Or.md> "Or") |  Logical or   
& |  [And](<And.md> "And") |  Bitwise and   
|  |  [Or](<Or.md> "Or") |  Bitwise or   
^  |  [Xor](<Xor.md> "Xor") |  Exclusive or   
~  |  [Not](<Not.md> "Not") |  One's complement   
>> |  [Shr](<Shr.md> "Shr") |  bit **sh** ift **r** ight Note: shr is a **logical bitshift** , not arithmetic. If the left operand is a negative value of a signed type, then the result of an shr operation may not be what you expect.   
<< |  [Shl](<Shl.md> "Shl") |  bit **sh** ift **l** eft   
++  |  [Inc](<Inc.md> "Inc") |   
\--  |  [Dec](<Dec.md> "Dec") |   
/*  |  { or (*  |  [Comment](<Comments.md> "Comments") start   
*/  |  } or *)  |  Comment end   
//  |  //  |  End of line comment (only one line comment)   
0x  |  [$](<Dollar_sign.md> "Dollar sign") |  prefix for hex-number e.g. $FFFFFF   
0  |  [&](<&.md> "&") |  prefix for oct-number e.g. &77777777   
0b  |  [%](<Percent_sign.md> "Percent sign") |  prefix for bin-number e.g. %11111111   
& |  [@](<@.md> "@") |  [address operator](<@.md> "@")  
*  |  [^](<^.md> "^") |  See [Pointer](<Pointer.md> "Pointer") and [Pointers](<Pointers.md> "Pointers")  
if()  |  [If](<If.md> "If") [Then](<Then.md> "Then") |   
if() else  |  [If](<If.md> "If") [Then](<Then.md> "Then") [Else](<Else.md> "Else") |   
while  |  [While](<While.md> "While") [Do](<Do.md> "Do") |   
do while  |  [Repeat](<Repeat.md> "Repeat") [Until](<Until.md> "Until") [Not](<Not.md> "Not") |   
do while !  |  [Repeat](<Repeat.md> "Repeat") [Until](<Until.md> "Until") |   
for ++  |  [For](<For.md> "For") [To](<To.md> "To") [Do](<Do.md> "Do") |   
for --  |  [For](<For.md> "For") [Downto](<Downto.md> "Downto") [Do](<Do.md> "Do") |   
switch case break  |  [Case](<Case.md> "Case") [Of](<Of.md> "Of") [End](<End.md> "End") |   
switch case break default  |  [Case](<Case.md> "Case") [Of](<Of.md> "Of") [Else](<Else.md> "Else") [End](<End.md> "End") |   
const a_c_struct *arg  |  [Constref](<Constref.md> "Constref") arg : a_c_struct  |   
a_c_struct *arg  |  [Var](<Var.md> "Var") arg : a_c_struct  |   
  
  


C type | [Pascal](<Pascal.md> "Pascal") [type](<Data_type.md> "Data type") | Size (bits) | Range | Notes   
---|---|---|---|---  
char  |  [Char](<Char.md> "Char") |  8-bit  |  |  [ASCII](<ASCII.md> "ASCII")  
signed char  |  [Shortint](<Shortint.md> "Shortint") |  8-bit  |  -128 .. 127  |   
unsigned char  |  [Byte](<Byte.md> "Byte") |  8-bit  |  0 .. 255  |   
char*  |  [PChar](<PChar.md> "PChar") |  (32-bit)  |  |  Pointer to a null-terminated string   
short int  |  [Smallint](<Smallint.md> "Smallint") |  16-bit  |  -32768 .. 32767  |   
unsigned short int  |  [Word](<Word.md> "Word") |  16-bit  |  0 .. 65535  |   
int  |  [Integer](<Integer.md> "Integer") |  (16-bit or) 32-bit  |  -2147483648..2147483647  |  Generic integer types   
unsigned int  |  [Cardinal](<Cardinal.md> "Cardinal") |  (16-bit or) 32-bit  |  0 .. 4294967295  |  Generic integer types   
long int  |  [Longint](<Longint.md> "Longint") |  32-bit  |  -2147483648..2147483647  |   
unsigned long int  |  [Longword](<Longword.md> "Longword") |  32-bit  |  0 .. 4294967295  |   
float  |  [Single](<Single.md> "Single") |  32-bit  |  1.5E-45 .. 3.4E+38  |   
double  |  [Double](<Double.md> "Double") |  64-bit  |  5.0E-324 .. 1.7E+308   
unsigned long long int  |  [uInt64](</index.php?title=uInt64&action=edit&redlink=1> "uInt64 \(page does not exist\)") |  64-bit  |  0 .. 18446744073709551616  |   
  
  


C type | [Pascal](<Pascal.md> "Pascal") | Notes   
---|---|---  
struct { }  |  [Record](<Record.md> "Record") [End](<End.md> "End") |   
union { }  |  [Record](<Record.md> "Record") [Case](<Case.md> "Case") [Of](<Of.md> "Of") [End](<End.md> "End") |  [Variant Record](<Case.md> "Case")  
  
  


C | [Pascal](<Pascal.md> "Pascal") | [Unit](<Unit.md> "Unit")  
---|---|---  
abs  |  Abs  |  [System](<System_unit.md> "System unit")  
acos  |  ArcCos  |  [Math](</index.php?title=Math_unit&action=edit&redlink=1> "Math unit \(page does not exist\)")  
asin  |  ArcSin  |  [Math](</index.php?title=Math_unit&action=edit&redlink=1> "Math unit \(page does not exist\)")  
atan  |  ArcTan  |  [System](<System_unit.md> "System unit")  
atof  |  StrToFloat  |  [SysUtils](<sysutils.md> "sysutils")  
atoi  |  StrToInt  |  [SysUtils](<sysutils.md> "sysutils")  
atol  |  StrToInt  |  [SysUtils](<sysutils.md> "sysutils")  
atoll  |  StrToInt64  |  [SysUtils](<sysutils.md> "sysutils")  
ceil  |  Ceil  |  [Math](</index.php?title=Math_unit&action=edit&redlink=1> "Math unit \(page does not exist\)")  
cos  |  Cos  |  [System](<System_unit.md> "System unit")  
exp  |  Exp  |  [System](<System_unit.md> "System unit")  
floor  |  Floor  |  [Math](</index.php?title=Math_unit&action=edit&redlink=1> "Math unit \(page does not exist\)")  
pow  |  Power  |  [Math](</index.php?title=Math_unit&action=edit&redlink=1> "Math unit \(page does not exist\)")  
round  |  Round  |  [System](<System_unit.md> "System unit")  
sin  |  Sin  |  [System](<System_unit.md> "System unit")  
sqrt  |  Sqrt  |  [System](<System_unit.md> "System unit")  
strcpy  |  Copy  |  [System](<System_unit.md> "System unit")  
strlen  |  Length  |  [System](<System_unit.md> "System unit")  
tan  |  Tan  |  [Math](</index.php?title=Math_unit&action=edit&redlink=1> "Math unit \(page does not exist\)")  
toupper  |  UpCase  |  [System](<System_unit.md> "System unit")  
  
### C++

C++ type | [Pascal](<Pascal.md> "Pascal") | Notes   
---|---|---  
class { }  |  [Class](<Class.md> "Class") [End](<End.md> "End") |   
class: { }  |  [Class](<Class.md> "Class")( ) [End](<End.md> "End") |   
template <class T> class { }  |  [Generic](</index.php?title=Generic&action=edit&redlink=1> "Generic \(page does not exist\)") = [Class](<Class.md> "Class")<T> [End](<End.md> "End") |   
struct { }  |  [Object](<Object.md> "Object") [End](<End.md> "End") |  If you want methods   
  
## See also

  * [C to Pascal](<C_to_Pascal.md> "C to Pascal") \- conversion tools and libraries
  * [Short tutorial](<http://gd.tuwien.ac.at/languages/pascal/fpc/docs-pdf/CinFreePascal.pdf>) that describes the process of using C and C++ code in FreePascal, including writing of wrapper code
  * [Creating bindings for C libraries](<Creating_bindings_for_C_libraries.md> "Creating bindings for C libraries")
  * [Common problems when converting C header files](<Common_problems_when_converting_C_header_files.md> "Common problems when converting C header files")
  * [SWIG](<SWIG.md> "SWIG")
  * [“Comparison of Pascal and C” on the English Wikipedia](<https://en.wikipedia.org/wiki/Comparison_of_Pascal_and_C>)

---

_Source: [https://wiki.freepascal.org/Pascal_for_C%2B%2B_users](https://web.archive.org/web/20240308084629/https://wiki.freepascal.org/Pascal_for_C%2B%2B_users)_
