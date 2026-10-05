# Standard Pascal

│ **English (en)** │  **[русский (ru)](<../ru/Standard_Pascal.md>)** │

In 1974, the creator of the [Pascal](<Pascal.md> "Pascal") language, [Niklaus Wirth](<Niklaus_Wirth.md> "Niklaus Wirth") wrote a book with Kathleen Jensen, titled _[Pascal User Manual and Report](<Pascal_User_Manual_and_Report.md> "Pascal User Manual and Report")_ published by Springer-Verlag. This book became a de-facto standard for the Pascal language. In 1983 the International Standards Organization (ISO) formalized the de-facto standard as ISO 7185:1983. In 1990 ISO released an updated version - ISO 7185:1990 - that didn't introduce any new concepts, but cleared up ambiguities and corrected errors that were in the earlier version. The ISO 7185 standard is referred to as **Standard Pascal**. The standard defines the minimum level that a [Pascal compiler](<Compiler.md> "Compiler") must support in order to be a true compiler of the Pascal language. 

  


## Contents

  * 1 Reserved Words
  * 2 Symbols
  * 3 Functions
    * 3.1 Arithmetic Functions
    * 3.2 Transfer Functions
    * 3.3 Ordinal Functions
    * 3.4 Boolean Functions
  * 4 Procedures
    * 4.1 File handling procedures
    * 4.2 Dynamic allocation procedures
    * 4.3 Transfer procedures
  * 5 Extensions
  * 6 Types
  * 7 Modes supported by Free Pascal
  * 8 External links



## [Reserved Words](<Reserved_word.md> "Reserved word")

The following are the standard [keywords](<Keyword.md> "Keyword") (referred to as _word-symbols_ in the ISO 7185) that all compilers must support: 

  * [`and`](<And.md> "And")
  * [`array`](<Array.md> "Array")
  * [`begin`](<Begin.md> "Begin")
  * [`case`](<Case.md> "Case")
  * [`const`](<Const.md> "Const")
  * [`div`](<Div.md> "Div")
  * [`do`](<Do.md> "Do")
  * [`downto`](<Downto.md> "Downto")
  * [`else`](<Else.md> "Else")
  * [`end`](<End.md> "End")
  * [`file`](</File> "File")
  * [`for`](<For.md> "For")
  * [`function`](<Function.md> "Function")
  * [`goto`](<Goto.md> "Goto")
  * [`if`](<If.md> "If")
  * [`in`](<In.md> "In")
  * [`label`](<Label.md> "Label")
  * [`mod`](<Mod.md> "Mod")
  * [`nil`](<Nil.md> "Nil")
  * [`not`](<Not.md> "Not")
  * [`of`](<Of.md> "Of")
  * [`or`](<Or.md> "Or")
  * [`packed`](<Packed.md> "Packed")
  * [`procedure`](<Procedure.md> "Procedure")
  * [`program`](<Program.md> "Program")
  * [`record`](<Record.md> "Record")
  * [`repeat`](<Repeat.md> "Repeat")
  * [`set`](<Set.md> "Set")
  * [`then`](<Then.md> "Then")
  * [`to`](<To.md> "To")
  * [`type`](<Type.md> "Type")
  * [`until`](<Until.md> "Until")
  * [`var`](<Var.md> "Var")
  * [`while`](<While.md> "While")
  * [`with`](<With.md> "With")



## Symbols

The following symbols (which the standard refers to as _special-symbols_) are also part of the language: 

  * \+ ([plus](<Plus.md> "Plus"))
  * \- ([minus](<Minus.md> "Minus"))
  * * ([asterisk](<_.md> "*"))
  * / ([slash](<Slash.md> "Slash"))
  * = ([equal](<Equal.md> "Equal"))
  * < ([less than](<Less_than.md> "Less than"))
  * > ([greater than](<Greater_than.md> "Greater than"))
  * [
  * ]
  * . ([period](<period.md> "period"))
  * , ([comma](<Comma.md> "Comma"))
  * : ([colon](<Colon.md> "Colon"))
  * ; ([semicolon](</;> ";"))
  * ↑
  * (
  * )
  * <> ([not equal](<Not_equal.md> "Not equal"))
  * <=
  * >=
  * := (named [becomes](<Becomes.md> "Becomes"))
  * ..



## Functions

All of the functions defined by Standard Pascal are implemented in Free Pascal in the [System unit](<System_unit.md> "System unit") of the standard [Runtime Library](<RTL.md> "RTL"). 

### Arithmetic Functions

Function | Description   
---|---  
abs(x)  | Calculate the absolute value of x   
arctan(x)  | arctan returns the arctangent of x, which can be any Real type. The resulting angle is in radial units.   
cos(x)  | Calculate cosine of angle x ([radians](<Radian.md> "Radian"))   
exp(x)  | Exp returns the exponent of x, i.e. the number e to the power x   
ln(x)  | returns the natural logarithm of the Real parameter x. x must be positive.   
sin(x)  | Calculate the sine of angle x (radians)   
sqr(x)  | Calculate the square of x   
sqrt(x)  | Calculate the square root of x. x must be positive.   
  
### Transfer Functions

Function | Description   
---|---  
[round](<Round.md> "Round")(x)  | Round floating point value to nearest integer number and return as integer.   
[trunc](<Trunc.md> "Trunc")(x)  | Truncate floating point x value and return as integer.   
  
### Ordinal Functions

Function | Description   
---|---  
[chr](<Chr.md> "Chr")(x)  | Convert byte value to a character value   
[ord](<Ord.md> "Ord")(x)  | Return ordinal value of an ordinal type.   
[pred](</index.php?title=Pred&action=edit&redlink=1> "Pred \(page does not exist\)")(x)  | Return previous element for an ordinal type.   
[succ](</index.php?title=Succ&action=edit&redlink=1> "Succ \(page does not exist\)")(x)  | Return next element of ordinal type.   
  
### Boolean Functions

Function | Description   
---|---  
[eof](<EOLN_and_EOF.md> "EOLN and EOF")(f)  | Check for end of file f   
[eoln](<EOLN_and_EOF.md> "EOLN and EOF")(f)  | Check for end of line on textfile f   
[odd](<Odd.md> "Odd")(x)  | Is a x odd or even ?   
  
## Procedures

Procedures defined by Standard Pascal are implemented in Free Pascal in the [System unit](<System_unit.md> "System unit") of the standard [Runtime Library](<RTL.md> "RTL"). 

### File handling procedures

Procedure | Description   
---|---  
[get](<Get.md> "Get")(f)  |   
[page](<Page.md> "Page")()  |   
[put](</index.php?title=Put&action=edit&redlink=1> "Put \(page does not exist\)")(f)  |   
[read](<Read.md> "Read") | Read from a text file or stdin into a [variable](<Variable.md> "Variable")  
[readln](<Read.md> "Read") | Read from a text file into variable and goto next line   
[reset](</index.php?title=Reset&action=edit&redlink=1> "Reset \(page does not exist\)")(f)  | Open file for reading   
[rewrite](<Rewrite.md> "Rewrite")(f)  | Open file for writing   
[write](<Write.md> "Write") | Write variable or literal string to a text file or stdout   
[writeln](<Write.md> "Write") | Write variable or literal string to a text file or stdout and append newline   
  
### Dynamic allocation procedures

Procedure | Description   
---|---  
[dispose](<Dispose.md> "Dispose")(q)  | Release the memory pointed to by q, which was allocated with a call to New.   
dispose(q,k1...kn)  |   
[new](<New.md> "New")(p)  | New allocates a new instance of the type pointed to by p, and puts the address in p.   
new(p,c1...cn)  |   
  
### Transfer procedures

Procedure | Description   
---|---  
pack()  | Create packed array from normal array   
unpack()  | Create unpacked array from packed array   
  
## Extensions

There are additional keywords which are not technically part of the Standard Pascal language but are used by [FPC](<FPC.md> "FPC") either for additional functionality such as for implementing objects, compatibility with the error recovery concepts exposed by C++, or to provide compatibility with [Borland Pascal](<Borland_Pascal.md> "Borland Pascal") and earlier Pascal compilers. These keywords include: 

    [implementation](<Implementation.md> "Implementation") · [finally](<Finally.md> "Finally") · [try](<Try.md> "Try") · [unit](<Unit.md> "Unit").

## Types

There are the standard [types](<Type.md> "Type"):

[integer](<Integer.md> "Integer") · [smallint](<Smallint.md> "Smallint") · [longint](<Longint.md> "Longint") · [real](<Real.md> "Real") · [boolean](<Boolean.md> "Boolean") · [string](<String.md> "String") · [char](<Char.md> "Char") · [byte](<Byte.md> "Byte")

## Modes supported by Free Pascal

Free Pascal supports ISO 7185 Standard Pascal with the [compiler mode](<Compiler_Mode.md> "Compiler Mode") [command line](<Command-line_interface.md> "Command-line interface") option **-Miso** or with the [source code](<Source_code.md> "Source code") [compiler directive](<Compiler_directive.md> "Compiler directive") [`{$mode ISO}`](<Mode_iso.md> "Mode iso"). Support of ISO 7185 started with version 3.0.0. It is planned to have a mode support ISO/IEC 10206 [Extended Pascal](<Extended_Pascal.md> "Extended Pascal") in future versions of Free Pascal. 

## External links

  * [Standard Pascal](<http://www.standardpascal.org>), reference information about the ANSI ISO 7185 standard.
  * [ISO 7185:1990](<http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=13802>), official normative version of the Pascal standard.
  * [ISO/IEC 10206:1991](<http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=18237>): Extended Pascal standard

---

_Source: [https://wiki.freepascal.org/Standard_Pascal](https://web.archive.org/web/20250208181916/https://wiki.freepascal.org/Standard_Pascal)_
