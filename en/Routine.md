# Routine

│ **English (en)** │  **[русский (ru)](<../ru/Routine.md>)** │

A routine is a re-usable piece of [source code](<Source_code.md> "Source code") having a name (called an [identifier](<Identifier.md> "Identifier")) that performs some functionality. Pascal distinguishes between two kinds of routines: [procedures](<Procedure.md> "Procedure") and [functions](<Function.md> "Function"). Functions are capable of returning a result value, while procedures do not. In consequence functions can appear in expressions, but procedures cannot. 

A routine that is part of an [object](<Object.md> "Object") or [class](<Class.md> "Class") is called a [method](<Method.md> "Method"). [Properties](</Property> "Property") of objects or classes can redirect read and/or write access to methods in that property or class handling the read or write request, if their signatures have a certain structure. 

## Contents

  * 1 Routine declarations and definitions
    * 1.1 Functions
    * 1.2 Procedures
    * 1.3 Signature
    * 1.4 Parameters
      * 1.4.1 Default values
      * 1.4.2 Parameter hints
      * 1.4.3 Parameter types
    * 1.5 Routine overloading
    * 1.6 Definition
  * 2 Invoking routines
  * 3 Comparative remarks



## Routine declarations and definitions

### Functions

A function is a routine that may return a value, and its signature indicates this. The signature of a function is as follows: 
    
    
    function *name*: *result*;
    

or 
    
    
    function *name* (*parameter list*): *result*;
    

The meaning of these terms will be explained below under Signature. 

### Procedures

A procedure is a routine that cannot return a value. The signature of a procedure is as follows: 
    
    
    procedure *name*;
    

or 
    
    
    procedure *name* (*parameter list*);
    

### Signature

The items in the signature are: 

    *name* is an identifier than gives the name of the routine.
    *parameter list* (if one is included) is one or more parameters, separated by semicolons, describing each argument to be passed to the routine.
    *result type* (for functions only) defines what type of result the function may return.

### Parameters

Routines may be parameterized, that means they can have arguments supplied to them when called. When introducing a new routine identifier a parameter list (one or more arguments) can be appended. For instance the following procedure signature tells the compiler, that `doSomething` requires an integer as first parameter. 
    
    
    procedure doSomething(const someParameter: integer);
    

The following procedure signature tells the compiler, That `doSomethingelse` requires a real as first parameter and an integer as second parameter. 
    
    
    procedure doSomethingelse(const someParameter: Real; SomeOtherParameter: Integer);
    

#### Default values

Parameters can become optional when they are supplied with a [default value](<Default_parameter.md> "Default parameter") like so: 
    
    
    procedure doSomething(const someParameter: integer = 42);
    

Defining default values is only possible in [`{$mode objFPC}`](<Mode_ObjFPC.md> "Mode ObjFPC") or [`{$mode Delphi}`](<Mode_Delphi.md> "Mode Delphi"), and they are only for simple types. 

Optional parameters if any, have to appear at the end of the formal parameter list. Consequently, mandatory parameters cannot appear after any optional parameter. 

#### Parameter hints

While defining the formal parameters in front of each identifier(s), type tuple, the compiler can be supplied with additional hints. The compiler then can make further optimizations. 

  * [`const`](<Const.md> "Const") informs the compiler, that the named parameter(s) won't be changed in the routine definition.
  * [`constref`](<Constref.md> "Constref") imposes further restrictions.



#### Parameter types

By default each routine receives an own copy of each parameter (value parameter). 

  * If the routine is supposed to work on the original, that means on the [variable](<Variable.md> "Variable") as it exists in the place the routine is called, the modifier [`var`](<Var.md> "Var") will allow that. Thereby the named parameter becomes a [variable parameter](<Variable_parameter.md> "Variable parameter").
  * Furthermore, if `{$modeswitch out+}` (automatically set by various modes), the output parameter type `out` exists. The routine will not, or is not supposed to read from such parameters, but only write.



### Routine overloading

Routines can be overloaded. That means, one and the same identifier can be associated with varying definitions provided the formal signatures differ. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Routine overloading is not possible for routines having the [`cDecl`](</index.php?title=Cdecl&action=edit&redlink=1> "Cdecl \(page does not exist\)") calling convention [modifier](<modifier.md> "modifier"). The FPC implements routine overloading by name mangling [= encoding parameter data types in a modified version of the routine’s identifier]. However, `cDecl` will _disable_ such.

### Definition

In order to define a routine, its signature is followed by a [block](<Block.md> "Block"). 

The parameters are available by their identifiers inside the statement-frame and nested routines. In functions additional identifiers are available, in order to set the function's return value. 

## Invoking routines

Routines are called by stating their identifiers, followed by the list of (mandatory) parameter literals or variables their types are compatible. Depending on the routine's type, i.e. either [`procedure`](<Procedure.md> "Procedure") or [`function`](<Function.md> "Function"), routine calls are only allowed as statements or in expressions, or even both. 

## Comparative remarks

The definition of a routine always concludes with a `ret`urn-instruction. Thus the control flow is always handed back to the LOC the routine was invoked at. In contrast to that behavior, [`goto` jumps](<Goto.md> "Goto") can be abused to circumvent that “limitation”. 

Every routine call is preceded by proper allocation of parameter values.

---

_Source: [https://wiki.freepascal.org/Routine](https://web.archive.org/web/20241002130429/https://wiki.freepascal.org/Routine)_
