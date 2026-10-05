# Pascal for Visual Basic users

## Contents

  * 1 Overview
  * 2 Comparison and assignment of values
  * 3 Beginning and ending statements
  * 4 Variables
    * 4.1 Declaring variables
    * 4.2 Types of variables
  * 5 Loops
    * 5.1 For... do...
  * 6 Functions and procedures
    * 6.1 Visibility
    * 6.2 Using default parameters in functions and procedures
  * 7 Dynamic arrays
  * 8 Translation of some commonly sought commands
  * 9 VB commands, which are not present in FPC/Lazarus
  * 10 Debugging
  * 11 Substitution tables
    * 11.1 String functions
  * 12 Component prefixes
  * 13 Arrays of controls
  * 14 Visual Basic to Delphi (Pascal) Converters



## Overview

This wiki page has been created to help you if you are switching from Visual Basic to Free Pascal (the Pascal dialect and compiler used by Lazarus). It is not meant to convince you that Lazarus is better than VB (or vice versa). Although VB and Pascal have superficial similarities these two languages differ greatly. Since VB programmers beginning to use Pascal face immediate difficulties in trying to do things which in VB have nice syntax that Pascal does not provide, we focus here on those differences. 

## Comparison and assignment of values

Values in Pascal are compared using the = operator. 

Values in Pascal are assigned using the := operator (there is a : added). 

For example: 
    
    
    if a = 1 then b := 2;
    

## Beginning and ending statements

Blocks of Pascal code have no distinguishing end statements for functions, procedures, loops, etc. Pascal needs only **begin** and **end**. You can easily add a comment manually following **end** statements to clarify which named routine corresponds to this particular **end;**. 

For example: 
    
    
    for i := 0 to 100 do
    begin
       ...
    end; // for i do
    

## Variables

### Declaring variables

In Pascal **all** variables have to be declared before use, and you have to make the declarations in a special **var** section which must precede the code that makes use of the variable. You cannot declare a new variable in the middle of code. 

For example: 
    
    
    procedure VarDecl;
    var
      MyVar1: string;
      Myvar2, MyVar3: integer; //Multiple variables can be declared on the same line.
    begin
      WriteLn('Print something');
      var MyVar4: string = 'else'; // This declaration is not allowed here - it must be moved three lines above to the var section
      WriteLn('Print something ', MyVar2);
    end; //VarDecl
    

Local variables do not get initialized upon declaration. They get random value (whatever value happen to be on the stack where they landed). Global variables are initialized to zero. 

To initialize a local variable upon declaration the following syntax can be used in procedures and functions only. 
    
    
    var
      MyVar1: string  = '';
      MyVar2: integer = 0;
    

### Types of variables

Besides the variables known in VB, Free Pascal supports unsigned integers. You should pay special attention not to mix them with signed integers. 

## Loops

Free Pascal has four kinds of loops. 

[for... do... ](<Basic_Pascal_Tutorial/Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..DO")

[while... do](<Basic_Pascal_Tutorial/Chapter_3/WHILE..md> "Basic Pascal Tutorial/Chapter 3/WHILE..DO")

[repeat... until](<Until.md> "Until")

[for.. in..](<for-in_loop.md> "for-in loop")

**Note:** In FPC there is the [Continue](<Continue.md> "Continue") procedure to jump to the end of the loop. 

### For... do...

This is quite similar to the For.. Next loop in VB, but more limited in syntax. The following restrictions apply in Pascal: 

1\. A for loop counter must be an integer (it cannot be a floating point type). 

2\. There is no Step property. To emulate the VB Step syntax you have to introduce a second counter variable yourself. 

3\. To decrement the value of the loop counter from a positive value, you use the keyword DOWNTO in place of TO. 

For example: 
    
    
    procedure ForLoop;
    var
      i: integer;
    begin
      for i := 50 downto 0 do
      begin
         ...
      end; // for i
    end.   // procedure ForLoop
    

4\. In Lazarus it is possible to loop down when the index is a **byte** , which is **not** possible in VB6. 
    
    
    procedure ForLoop;
    var
      i: byte;
    begin
       for i := 200 downto 195 do
       begin
          ShowMessage (IntToStr (i));
       end;
    end;
    

5\. The value of the counter (i in the example above) can**not** be changed programmatically inside the loop. Attempting to do so will throw a compiler error. Instead one could use _Continue_ to skip execution of a loop: 
    
    
    procedure ForLoop;
    var
      i: integer;
    begin
      for i := 0 to 5 do
      begin
        if i=2 then Continue; //Following part of the loop will not be executed
        ShowMessage (IntToStr (i));
      end; // for i
    end.   // procedure ForLoop
    

6\. In order to interrupt execution of a loop one could use _Break_. 
    
    
    procedure ForLoop;
    var
      i: integer;
    begin
      for i := 0 to 5 do
      begin
        if i=2 then 
        begin
          ShowMessage (IntToStr (i));
          break;
        end; //if
      end; // for i
    end.   // procedure ForLoop
    

## Functions and procedures

Output of functions is supplied as: 
    
    
    Result:=Value
    

Note that _Result:=_ does **not** act as _Return_. Function will **not** exit after assigning it. 

One should use 
    
    
    exit(Value)
    

instead. 

### Visibility

In FPC a function or a procedure cannot see another one, which is written **after** it. 

Example: 
    
    
    procedure Proc1;
    begin
      Proc2;  //This is not possible
    end;
    
    procedure Proc2;
    begin
      Proc1; //This is possible
    end;
    

To enable visibility, disregarding writing order, functions and procedures have to be declared. 
    
    
    unit MyUnit;
    ...
    procedure Proc1;
    procedure Proc2;
    ...
    implementation
    
    procedure Proc1;
    begin
      Proc2;  //This is possible now
    end;
    
    procedure Proc2;
    begin
      Proc1; //This is possible
    end;
    

### Using default parameters in functions and procedures

In Pascal functions and procedures it is possible for parameters to automatically take default values. However, in the parameter list the parameters that might take default values must be declared as a contiguous list at the end of the parameter declaration part. Also, when a routine with default parameter(s) is subsequently called in your code, you can only specify non-default values beginning at the first parameter which would otherwise take a default value. You cannot specify a non-default value for a later parameter, if there are preceding parameters you have to specify values for which have not also been specified explicitly in the function or procedure call. 

For example: 

If a procedure is declared (correctly) as follows with two default parameters: 
    
    
    procedure SampleProc(parm1: integer; parm2: string= 'something'; parm3:Boolean= True);
    begin
    end. //proc
    

it is not possible to call SampleProc as in VB like this: 
    
    
      SampleProc(5,,False);
    

This will produce a compiler error. 

When calling a procedure (or function), if you specify a parameter which has a default value, you must also specify all parameters that have a default value that are in front of it. So, in the example above if you want the third parameter to be False, then you must also specify the value of second parameter. 

These are valid calls: 
    
    
      SampleProc(5,'something',False);
    
      SampleProc(5,'nothing');
    
      SampleProc(5,'');
    
      SampleProc(5);
    

## Dynamic arrays

In Pascal the minimum index of a dynamic array is 0, i.e. _MyArray (1)_ will return the **second** elements of an array named _MyArray_. 

Arrays are resized using: 
    
    
    setlength (MyArray, NewDimension);
    

_SetLength_ does **not** clear the array, i.e. it acts like _ReDim Preserve_ , not like _ReDim_. 

## Translation of some commonly sought commands

DoEvents = TApplication.ProcessMessages 
    
    
    var
      app: TApplication;
      i: integer;
    begin
      app := TApplication.Create(nil);
    
      for i := 0 to 1000000 do
      begin
        app.ProcessMessages; // Handling process messages.
        OutputDebugString(PChar(IntToStr(i)));
      end
    end;
    

While the _OutputDebugString_ printing the number to _Event Log_ , you may do any events with Form without _Freezing_ except when you close the form, you must wait the loop until finished. 

## VB commands, which are not present in FPC/Lazarus

_Split_ \- Split is not a separate function in FPC. It shall be used the following way: 
    
    
    String1D := '1;2,3;4;5,6'.Split([',',';']);
    

_Join_ works the following way: 
    
    
    MyString := ''.Join(';',String1D);
    

## Debugging

Lazarus IDE has no Instant panel. 

In order to debug in Windows the dos prompt windows shall be enabled the following way: _Lazarus IDE- > Main menu -> Project -> Project options... -> Compiler Options -> Config and Target ->_ uncheck _Win32 gui application (-WG)_

In Linux the console invokes itself. 

Then the _write_ and _writeln_ commands can be used to print debug messages. 

Compiler might give some more information if the following items are checked in the IDE main menu -> _Project_ -> _Project Option_ -> _Compiler Option_ -> _Debugging_ -> -Ci, -Cr, -Co, -St, -Cr, -Sa. 

## Substitution tables

VB | [Pascal](<Pascal.md> "Pascal") | Remark   
---|---|---  
=  |  :=  |  [Becomes](<Becomes.md> "Becomes")  
=  |  =  |  [Equal](<Equal.md> "Equal")  
/  |  /  |  [Float Division](<Slash.md> "Slash")  
\  |  [Div](<Div.md> "Div") |   
& |  \+  |  String concatenation   
MOD  |  [Mod](<Mod.md> "Mod") |  Modulo operation   
<> |  <> |  [Not equal](<Not_equal.md> "Not equal")  
Not  |  [Not](<Not.md> "Not") |   
And  |  [And](<And.md> "And") |   
Or  |  [Or](<Or.md> "Or") |   
Xor  |  [Xor](<Xor.md> "Xor") |   
>> |  [Shr](<Shr.md> "Shr") |  bit **sh** ift **r** ight   
<< |  [Shl](<Shl.md> "Shl") |  bit **sh** ift **l** eft   
New line  |  ;  |  In Pascal lines end with the ; character not by a new line.   
_  |  |  No symbol is required for wrapping a line and new line symbols are acceptable within it.   
'  |  //  |  End of line [Comment](<Comments.md> "Comments") (only one line comment)   
Exit For  |  Break  |  Interrupt execution of a loop   
  
  


VB type | [Pascal](<Pascal.md> "Pascal") [type](<Data_type.md> "Data type") | Size (bits) | Range | Remark   
---|---|---|---|---  
SByte  |  [Shortint](<Shortint.md> "Shortint") |  8-bit  |  -128 .. 127  |   
Byte  |  [Byte](<Byte.md> "Byte") |  8-bit  |  0 .. 255  |   
Short  |  [SmallInt](<Smallint.md> "Smallint") |  16-bit  |  -32768 .. 32767  |   
Char  |  [Word](<Word.md> "Word") |  16-bit  |  0 .. 65535  |   
Integer  |  [Integer](<Integer.md> "Integer") |  32-bit  |  -2147483648..2147483647  |   
UInteger  |  [Cardinal](<Cardinal.md> "Cardinal") |  32-bit  |  0 .. 4294967295  |   
Single  |  [Single](<Single.md> "Single") |  32-bit  |  1.5E-45 .. 3.4E+38  |  Decimal delimiter in both languages, applied **in source code** is dot on the line.   
Double  |  [Double](<Double.md> "Double") |  64-bit  |  5.0E-324 .. 1.7E+308  |   
Boolean  |  [Boolean](<Boolean.md> "Boolean") |  |  [True](<True.md> "True") [False](<False.md> "False") |   
  
### String functions

VB | [Pascal](<Pascal.md> "Pascal") | Remark   
---|---|---  
& |  \+  |  String concatenation   
Format(item, formatstring)  |  format(formatstring, items)  |   
left(some_string,n)  |  leftstr(some_string, n) or copy (some_string,1,n)  |   
Len(some_string)  |  length(some_string)  |   
LCase(some_string)  |  lowercase(some_string)  |   
Mid(some_string, startpos, numChars)  |  copy(some_string, startpos, numChars)  |   
StrReverse(string)  |  See [Palindrome](<Palindrome.md> "Palindrome") |   
UCase(some_string)  |  uppercase(some_string)  |   
  
## Component prefixes

There is a [list of component prefixes](<http://kodu.ut.ee/~jellen/delphi/cs.html#Components_Instance>) for Delphi, which can be used in Lazarus. Using them might provide better code readability. 

It is also usual to start the name of a type with the capital letter _T_. 

## Arrays of controls

Unlike in VB6 and like in VB.NET arrays of controls cannot be created statically (trough the IDE), and they can be created only dynamically. 

Example, creating 6 buttons above each other: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      i: Integer;
      btnArray: array of TButton;
    begin
      SetLength(btnArray, 6);
      for i := 0 to 5 do
      begin
        btnArray[i]        := TButton.Create(Self);
        btnArray[i].Left   := 0;
        btnArray[i].Top    := i * 30;
        btnArray[i].Parent := Self;
      end;
    end;
    

## Visual Basic to Delphi (Pascal) Converters

  * <https://www.marcocantu.com/tools/vb2delphi.htm>
  * <https://cc.embarcadero.com/item/17064>

---

_Source: [https://wiki.freepascal.org/Pascal_for_Visual_Basic_users](https://web.archive.org/web/20240907080107/https://wiki.freepascal.org/Pascal_for_Visual_Basic_users)_
