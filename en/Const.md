# Const

│ **[Deutsch (de)](</Const/de> "Const/de")** │  **English (en)** │  **[español (es)](</Const/es> "Const/es")** │  **[suomi (fi)](</Const/fi> "Const/fi")** │  **[français (fr)](</Const/fr> "Const/fr")** │  **[中文（中国大陆） (zh_CN)](</Const/zh_CN> "Const/zh CN")** │    
****

The **const** [keyword](<Keyword.md> "Keyword") has three uses in a [Pascal](<Pascal.md> "Pascal") [program](<Program.md> "Program"): 

  * to start a [constant](<Constant.md> "Constant") declaration section
  * to declare _const parameter_ for a [function](<Function.md> "Function") or [procedure](<Procedure.md> "Procedure")
  * to declare a function with a variable number of parameters passed via a variable-sized array of different element types



## Contents

  * 1 Const Section
  * 2 Const Parameter
  * 3 Array of Const
  * 4 See also



## Const Section

The declaration **const** in a Pascal program is used to inform the [compiler](<Compiler.md> "Compiler") that certain [identifiers](<Identifier.md> "Identifier") which are being declared are [constants](<Constant.md> "Constant"), that is, they are initialized with a specific value at [compile time](<Compile_time.md> "Compile time") as opposed to a [variable](<Var.md> "Var") which is initialized at [run time](<runtime.md> "runtime"). 

However, the default setting in [Free Pascal](<FPC.md> "FPC") is to allow const identifiers to be re-assigned to. In order to make them unchangeable, the `{$J}` (short form) or `{$WriteableConst}` (long form) [compiler directives](<Compiler_directive.md> "Compiler directive") must be used to turn off the ability to assign to constant identifiers. That is `{$J-}` or `{$WriteableConst OFF}`. 

In some Pascal compilers, the Const declaration is used to define variables which are initialized at compile time to a certain specific value, and that the variables so defined can change as the program executes. This can be used for initializing arrays at compile time as opposed to setting values when the program is executed. 

## Const Parameter

A function or procedure parameter may be declared **const**. Any assignment to a **const** parameter within a procedure or function and the compiler will flag it as an error: "Can't assign values to const variable". Declaring a parameter as const allows the compiler the possibility to do optimizations it couldn't do otherwise, such as passing by reference while retaining the semantics of passing by value. A const parameter cannot be passed to another function or procedure that requires a [variable parameter](<Variable_parameter.md> "Variable parameter"). 

## Array of Const

A function can declare a parameter as an **array of const**. This allows a a routine to effectively take a variable amount of different types of parameters, via a single variable-length array parameter. At run-time when the function/procedure is called, the actual elements of the array are turned into variant records of type [TVarRec](<TVarRec.md> "TVarRec"). The called routine can use High() to determine the number of elements in the array and look at the VType field of each TVarRec element, to determine the type it contains. Record types cannot be passed as part of an **array of const** , but simple types, classes and interfaces can be. You create an array of const by placing the components within []. For example: 
    
    
    function MyFunction( array of const ) : Boolean;
      ...
    
    MyFunction( [10, 'global', True, my_var] );
    

This feature is only available in [compiler mode](<Compiler_Mode.md> "Compiler Mode") [ObjFPC](<Mode_ObjFPC.md> "Mode ObjFPC") or [Delphi](<Mode_Delphi.md> "Mode Delphi"). 

Note: Due to Delphi compatibility, arrays of const can't have an unsigned 32-bit variable type. Trying to pass a Dword with the highest bit set will cause a range check error, or gets interpreted as a Longint. 

## See also

  * [Constref](<Constref.md> "Constref")
  * [Constants](<Basic_Pascal_Tutorial/Chapter_1/Constants.md> "Basic Pascal Tutorial/Chapter 1/Constants")
  * [Class constants](<Class_constants.md> "Class constants")

---

_Source: [https://wiki.freepascal.org/Const](https://web.archive.org/web/20240920204055/https://wiki.freepascal.org/Const)_
