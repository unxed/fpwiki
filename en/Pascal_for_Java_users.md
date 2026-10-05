# Pascal for Java users

│ **English (en)** │

## Contents

  * 1 Translating common program snippets
  * 2 Translating Java keywords/concepts
  * 3 Translating Java data types
  * 4 See also



## Translating common program snippets

The following site shows how common problems are solved in various programming languages, including Javan and FreePascal/Object Pascal: [Rosetta Code](<http://rosettacode.org/wiki/Category:Pascal>)

## Translating Java keywords/concepts

Java | [Pascal](<Pascal.md> "Pascal") | Additional comments   
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
public static void main(String[] args) { }  |  [Program](<Program.md> "Program") ProgramName; [Begin](<Begin.md> "Begin") [End](<End.md> "End").  |  Note the period after End   
someType arrayVar[];  |  arrayVar: [Array](<Array.md> "Array") [Of](<Of.md> "Of") someType;  |   
someType arrayVar[#];  |  arrayVar: [Array](<Array.md> "Array")[MINRANGE..MAXRANGE] [Of](<Of.md> "Of") someType;  |   
null  |  [Nil](<Nil.md> "Nil") |   
abstract  |  [Abstract](</index.php?title=Abstract&action=edit&redlink=1> "Abstract \(page does not exist\)") |   
break  |  [Break](<Break.md> "Break") |   
class TheClass { }  |  TheClass = [Class](<Class.md> "Class") [End](<End.md> "End");  |  Delphi OOP   
class TheClass { }  |  TheClass = [Object](<Object.md> "Object") [End](<End.md> "End");  |  Turbo Pascal OOP   
class TheClass<T> { }  |  [Generic](</index.php?title=Generic&action=edit&redlink=1> "Generic \(page does not exist\)") TheClass = [Class](<Class.md> "Class")<T> [End](<End.md> "End");  |  Generics are classes only as of 2.2.2, likely to support more in the future   
TheClass()  |  [Constructor](<Constructor.md> "Constructor") CtorName  |  Constructor name is by convention either Init or Create   
continue  |  [Continue](<Continue.md> "Continue") |   
do while  |  [Repeat](<Repeat.md> "Repeat") [Until](<Until.md> "Until") [Not](<Not.md> "Not") |   
do while !  |  [Repeat](<Repeat.md> "Repeat") [Until](<Until.md> "Until") |   
enum TheEnum  |  TheEnum = ( MINVALUE .. MAXVALUE );  |   
enum TheEnum = {MINVALUE, MAXVALUE}  |  TheEnum = ( MINVALUE, MAXVALUE );  |   
TheEnum enumVar;  |  enumVar := [Set](<Set.md> "Set") [Of](<Of.md> "Of") TheEnum;  |   
extends  |  SubClass = [Class](<Class.md> "Class")(BaseClass)  |  class SubClass extends BaseClass --> SubClass(BaseClass)   
final  |  [Const](<Const.md> "Const") |  for constants, not uninheritables   
for ++  |  [For](<For.md> "For") [To](<To.md> "To") [Do](<Do.md> "Do") |   
for --  |  [For](<For.md> "For") [Downto](<Downto.md> "Downto") [Do](<Do.md> "Do") |   
if()  |  [If](<If.md> "If") [Then](<Then.md> "Then") |   
if() else  |  [If](<If.md> "If") [Then](<Then.md> "Then") [Else](<Else.md> "Else") |   
implements  |  SomeClass = [Class](<Class.md> "Class")(SomeInterface)  |   
import  |  [Uses](<Uses.md> "Uses") |   
instanceof  |  [Is](<Is.md> "Is") |   
interface  |  TheInterface = [Interface](<Interface.md> "Interface") |   
native  |  [StdCall](</index.php?title=StdCall&action=edit&redlink=1> "StdCall \(page does not exist\)") |  For Windows   
native  |  [CDecl](</index.php?title=CDecl&action=edit&redlink=1> "CDecl \(page does not exist\)") |  For Unix   
new primitive_type[#];  |  [SetLength](</index.php?title=SetLength&action=edit&redlink=1> "SetLength \(page does not exist\)")(ArrayVar, #);  |   
new Class();  |  InstanceVar := TheClass.Create;  |   
package pkgName;  |  [Unit](<Unit.md> "Unit") unitName; [Interface](<Interface.md> "Interface") [Implementation](<Implementation.md> "Implementation") [End](<End.md> "End").  |  Note the period after End   
private  |  [Private](<Private.md> "Private") |   
protected  |  [Protected](<Protected.md> "Protected") |   
public  |  [Public](<Public.md> "Public") |   
return  |  FunctionName :=  |   
return  |  [Result](<Result.md> "Result") :=  |  ObjFPC or Delphi modes   
return  |  [Exit()](<Function.md> "Function") |  ObjFPC modes   
static  |  [Static](</index.php?title=Static&action=edit&redlink=1> "Static \(page does not exist\)") |   
static  |  [Class](<Class.md> "Class") [Function](<Function.md> "Function") |   
static  |  [Class](<Class.md> "Class") [Procedure](<Procedure.md> "Procedure") |   
super  |  [Inherited](<Inherited.md> "Inherited") |  Parent constructor call   
switch case break  |  [Case](<Case.md> "Case") [Of](<Of.md> "Of") [End](<End.md> "End") |   
switch case break default  |  [Case](<Case.md> "Case") [Of](<Of.md> "Of") [Else](<Else.md> "Else") [End](<End.md> "End") |   
synchronized  |  [TCriticalSection](</index.php?title=TCriticalSection&action=edit&redlink=1> "TCriticalSection \(page does not exist\)") |   
this  |  [Self](<Self.md> "Self") |   
throw  |  [Raise](<Raise.md> "Raise") |   
throws  |  |   
transient  |  |   
try { } catch  |  [Try](<Try.md> "Try") [Except](<Except.md> "Except") |   
try { } catch finally  |  [Try](<Try.md> "Try") [Finally](<Finally.md> "Finally") |   
void  |  [Procedure](<Procedure.md> "Procedure") |   
volatile  |  |   
while  |  [While](<While.md> "While") [Do](<Do.md> "Do") |   
  
## Translating Java data types

Java type | [Pascal](<Pascal.md> "Pascal") [type](<Data_type.md> "Data type") | Size (bits) | Range |   
---|---|---|---|---  
byte  |  [Shortint](<Shortint.md> "Shortint") |  8-bit  |  -128 .. 127  |   
short  |  [Smallint](<Smallint.md> "Smallint") |  16-bit  |  -32768 .. 32767   
int  |  [Longint](<Longint.md> "Longint") |  32-bit  |  -2147483648..2147483647  |   
int  |  [Integer](<Integer.md> "Integer") |  32-bit  |  -2147483648..2147483647  |   
long  |  [Int64](<Int64.md> "Int64") |  64-bit  |  -9 223 372 036 854 775 808 .. 9 223 372 036 854 775 807  |   
float  |  [Single](<Single.md> "Single") |  32-bit  |  1.5E-45 .. 3.4E+38  |   
double  |  [Double](<Double.md> "Double") |  64-bit  |  5.0E-324 .. 1.7E+308  |   
boolean  |  [Boolean](<Boolean.md> "Boolean") |  |  [False](<False.md> "False") [True](<True.md> "True") |   
char  |  [WideChar](<WideChar.md> "WideChar") |  16-bit  |  |   
String  |  [String](<String.md> "String") |  |   
  
## See also

  * [Using Pascal Libraries with Java](<Using_Pascal_Libraries_with_Java.md> "Using Pascal Libraries with Java")

---

_Source: [https://wiki.freepascal.org/Pascal_for_Java_users](https://web.archive.org/web/20231001123640/https://wiki.freepascal.org/Pascal_for_Java_users)_
