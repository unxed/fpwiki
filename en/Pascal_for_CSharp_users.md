# Pascal for CSharp users

## Contents

  * 1 Syntax Comparison
  * 2 Types Comparison
  * 3 C# Language Features Not available in Pascal
  * 4 FPC Language Features not available in C #



## Syntax Comparison

C# | [Pascal](<Pascal.md> "Pascal") | Additional comments   
---|---|---  
{  |  [Begin](<Begin.md> "Begin") |   
}  |  [End](<End.md> "End") |   
=  |  :=  |  [Becomes](<Becomes.md> "Becomes")  
==  |  =  |  [Equal](<Equal.md> "Equal")  
/  |  /  |  [Division](<Slash.md> "Slash") (or sometimes [div](<Div.md> "Div"))   
%  |  [Mod](<Mod.md> "Mod") |  **Mod** ulo operation   
!  |  [Not](<Not.md> "Not") |   
!=  |  <> |  [Not equal](<Not_equal.md> "Not equal")  
&& |  [And](<And.md> "And") |   
||  |  [Or](<Or.md> "Or") |   
^  |  [Xor](<Xor.md> "Xor") |   
>> |  [Shr](<Shr.md> "Shr") |  bit **sh** ift **r** ight   
<< |  [Shl](<Shl.md> "Shl") |  bit **sh** ift **l** eft   
++  |  [Inc](<Inc.md> "Inc") |   
\--  |  [Dec](<Dec.md> "Dec") |   
/*  |  {  |  [Comment](<Comments.md> "Comments") start   
/*  |  (*  |  [Comment](<Comments.md> "Comments") start   
*/  |  }  |  Comment end   
*/  |  *)  |  Comment end   
//  |  //  |  End of line comment (only one line comment)   
#define  |  {$Define }  |   
#define  |  {$SetC }  |  Mac Pascal   
#elif  |  |   
#else  |  {$Else}  |   
#else  |  {$ElseC }  |  Mac Pascal   
#endif  |  {$EndIf }  |   
#endregion  |  |   
#error  |  {$Fatal }  |   
#if  |  {$If }  |   
#if  |  {$IfC }  |  Mac Pascal   
#ifdef  |  {$IfDef }  |   
#ifndef  |  {$IfNDef }  |   
#pragma  |  {$IfOpt }  |   
#region  |  |   
#undef  |  {$Undef }  |   
#warning  |  {$Hint }  |   
public static void Main(string[] args) { }  |  [Program](<Program.md> "Program") ; [Begin](<Begin.md> "Begin") [End](<End.md> "End").  |  Note the period after End   
[];  |  : [Array](<Array.md> "Array") [Of](<Of.md> "Of") ;  |   
[#];  |  : [Array](<Array.md> "Array")[#..#] [Of](<Of.md> "Of") ;  |   
null  |  [Nil](<Nil.md> "Nil") |   
abstract  |  [Abstract](</index.php?title=Abstract&action=edit&redlink=1> "Abstract \(page does not exist\)") |   
break  |  [Break](<Break.md> "Break") |   
class { }  |  = [Class](<Class.md> "Class") [End](<End.md> "End");  |  Delphi OOP   
class { }  |  = [Object](<Object.md> "Object") [End](<End.md> "End");  |  Turbo Pascal OOP   
class <T> { }  |  [Generic](</index.php?title=Generic&action=edit&redlink=1> "Generic \(page does not exist\)") = [Class](<Class.md> "Class")<T> [End](<End.md> "End");  |  Generics are classes only as of 2.2.2, likely to support more in the future   
Class()  |  [Constructor](<Constructor.md> "Constructor") |  Constructor name is by convention either Init or Create   
continue  |  [Continue](<Continue.md> "Continue") |   
do while  |  [Repeat](<Repeat.md> "Repeat") [Until](<Until.md> "Until") [Not](<Not.md> "Not") |   
do while !  |  [Repeat](<Repeat.md> "Repeat") [Until](<Until.md> "Until") |   
enum { }  |  = (# .. #);  |   
enum = { = #, }  |  = ( = #, );  |   
TheEnum enumVar;  |  := [Set](<Set.md> "Set") [Of](<Of.md> "Of") ;  |   
class :  |  = [Class](<Class.md> "Class")(TObject)  |   
const  |  [Const](<Const.md> "Const") |  for constants, not uninheritables   
for( = ; ; ++)  |  [For](<For.md> "For") [To](<To.md> "To") [Do](<Do.md> "Do") |   
for( = ; ; --)  |  [For](<For.md> "For") [Downto](<Downto.md> "Downto") [Do](<Do.md> "Do") |   
foreach( in )  |  [For](<For.md> "For") [In](<In.md> "In") |  [For .. in loop](<for-in_loop.md> "for-in loop")  
if()  |  [If](<If.md> "If") [Then](<Then.md> "Then") |   
if() else  |  [If](<If.md> "If") [Then](<Then.md> "Then") [Else](<Else.md> "Else") |   
using  |  [Uses](<Uses.md> "Uses") |   
interface { }  |  = [Interface](<Interface.md> "Interface")(IInterface) End;  |   
new [#];  |  [SetLength](</index.php?title=SetLength&action=edit&redlink=1> "SetLength \(page does not exist\)")( , #);  |   
new ();  |  := .Create;  |   
namespace Name { }  |  [Unit](<Unit.md> "Unit") ; [Interface](<Interface.md> "Interface") [Implementation](<Implementation.md> "Implementation") [End](<End.md> "End").  |  Note the period after End   
out  |  [Out](</index.php?title=Out&action=edit&redlink=1> "Out \(page does not exist\)") |   
override  |  [Override](<Override.md> "Override") |   
private  |  [Private](<Private.md> "Private") |   
protected  |  [Protected](<Protected.md> "Protected") |   
public  |  [Public](<Public.md> "Public") |   
ref  |  [Var](<Variable_parameter.md> "Variable parameter") |   
return  |  FunctionName :=  |   
return  |  [Result](<Result.md> "Result") :=  |  ObjFPC or Delphi modes   
sealed  |  sealed  |  starting from fpc 2.5.1   
static  |  [Static](</index.php?title=Static&action=edit&redlink=1> "Static \(page does not exist\)") |   
static  |  [Class](<Class.md> "Class") [Function](<Function.md> "Function") |   
static  |  [Class](<Class.md> "Class") [Procedure](<Procedure.md> "Procedure") |   
struct { }  |  = [Record](<Record.md> "Record") [End](<End.md> "End");  |   
base  |  [Inherited](<Inherited.md> "Inherited") |  Parent constructor call   
switch () { case: break; }  |  [Case](<Case.md> "Case") [Of](<Of.md> "Of") [End](<End.md> "End") ;  |   
switch() { case: break; default: }  |  [Case](<Case.md> "Case") [Of](<Of.md> "Of") [Else](<Else.md> "Else") [End](<End.md> "End") |   
this  |  [Self](<Self.md> "Self") |   
try { } catch  |  [Try](<Try.md> "Try") [except](</index.php?title=except&action=edit&redlink=1> "except \(page does not exist\)") |   
try { } catch finally  |  [Try](<Try.md> "Try") [Finally](<Finally.md> "Finally") |   
unsafe  |  |   
virtual  |  [Virtual](<Virtual.md> "Virtual") |   
void  |  [Procedure](<Procedure.md> "Procedure") |   
volatile  |  |   
while  |  [While](<While.md> "While") [Do](<Do.md> "Do") |   
|  [Asm](<Asm.md> "Asm") [End](<End.md> "End");  |   
  
## Types Comparison

C# type | [Pascal](<Pascal.md> "Pascal") [type](<Data_type.md> "Data type") | Size (bits) | Range |   
---|---|---|---|---  
sbyte  |  [ShortInt](<Shortint.md> "Shortint") |  8-bit  |  -128 .. 127  |   
byte  |  [Byte](<Byte.md> "Byte") |  8-bit  |  0 .. 255  |   
short  |  [SmallInt](<Smallint.md> "Smallint") |  16-bit  |  -32768 .. 32767   
ushort  |  [Word](<Word.md> "Word") |  16-bit  |  0 .. 65535  |   
int  |  [LongInt](<Longint.md> "Longint") |  32-bit  |  -2147483648..2147483647  |   
uint  |  [LongWord](<Longword.md> "Longword") |  32-bit  |  0..4294967295  |   
long  |  [Int64](<Int64.md> "Int64") |  64-bit  |  -9 223 372 036 854 775 808 .. 9 223 372 036 854 775 807  |   
ulong  |  [QWord](<QWord.md> "QWord") |  64-bit  |  0 .. 18 446 744 073 709 551 615  |   
float  |  [Single](<Single.md> "Single") |  32-bit  |  1.5E-45 .. 3.4E+38  |   
double  |  [Double](<Double.md> "Double") |  64-bit  |  5.0E-324 .. 1.7E+308  |   
bool  |  [Boolean](<Boolean.md> "Boolean") |  |  [False](<False.md> "False") [True](<True.md> "True") |   
char  |  [WideChar](<WideChar.md> "WideChar") |  16-bit  |  |   
string  |  [String](<String.md> "String") |  |   
datetime  |  [TDateTime](<TDateTime.md> "TDateTime") |  |   
  
## C# Language Features Not available in Pascal

  * automatic memory management while - not available, except for reference counted types: strings (except for shortstrings), dynamic arrays
  * lambda functions
  * serialization (as syntax language) - you'd need to use explicit functions/procedures to convert to/from data structures
  * LINQ



## FPC Language Features not available in C #

  * [Assembler](<Assembler.md> "Assembler"), [Assembly language](<Assembly_language.md> "Assembly language")
  * Case-insensitive

---

_Source: [https://wiki.freepascal.org/Pascal_for_CSharp_users](https://web.archive.org/web/20240920204107/https://wiki.freepascal.org/Pascal_for_CSharp_users)_
