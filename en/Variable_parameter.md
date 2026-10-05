# Variable parameter

│ **English (en)** │

A **variable parameter** is a [routine](<Routine.md> "Routine") parameter that is a [variable](<Variable.md> "Variable"). To call a routine with a variable parameter, you need to specify a variable at the proper position. The variable will (temporarily) be in the scope of the routine through the parameter’s name. 

## Usage

A variable parameter is declared by preceding a formal parameter declaration with the [keyword `var`](<Var.md> "Var"). 
    
    
    procedure xorSwap(var left, right: integer);
    begin
    	left := left xor right;
    	right := left xor right;
    	left := left xor right;
    end;
    

Variable parameters can serve as both input and output, meaning they can be used for passing a value _to_ a routine, _and_ getting a value _from_ it. After `procedure xorSwap` has been called the variables _at the call site_ will have changed. [Assignments](<Becomes.md> "Becomes") to variable parameters have an effect in the scope where the routine is called. Thus, a variable parameter can be considered as an alias for the actual argument given by the calling routine. When a routine changes the value of a variable parameter, it is actually changing the variable in the code that called the routine. 

Since a variable parameter may appear on the left hand side of an assignment, only _variables_ may be supplied as arguments when calling the routine, never [constants](<Constant.md> "Constant") or [expressions](<expression.md> "expression"). 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** [`file`](</File> "File") and [`text`](<Text.md> "Text") variables always have to be declared as variable parameters.

With [GNU Pascal](<GNU_Pascal.md> "GNU Pascal") and Free Pascal, variable parameters may have no associated [data type](<Data_type.md> "Data type") (FPC [internally] calls this “formal type”). Such parameters do not allow any operations on them, but [typecasting](<Typecast.md> "Typecast") has to be used. Only the [`@`-address-operator](<@.md> "@") is available: 
    
    
    procedure printAddress(var x);
    begin
    	write(sysBackTraceStr(@x));
    end;
    

## Implementation

The specific implementation of variable parameters is only of concern for those who are programming (pure) assembler routines. 

In [FPC](<FPC.md> "FPC"), variable parameters are implemented by passing a reference to the variable at the call site (call by reference). For this reason, variable parameters are also referred to as _reference parameters_. If a routine is [inlined](<Inline.md> "Inline"), the extra level of indirection is eliminated. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** In order to pass an address to the routine, all parameters have to be addressable (by _one_ address). 

Although a [`property`](</Property> "Property") may be readable _and_ writable, it does not have a single address in one memory block, thus it cannot be used as a variable parameter.

## See also

  * [§ “Variable Parameters” in the FPC Reference Guide](<https://freepascal.org/docs-html/current/ref/refsu65.html>)
  * [`out`](</index.php?title=Out&action=edit&redlink=1> "Out \(page does not exist\)") designates a parameter as non-readable, only writable
  * [`constRef`](<Constref.md> "Constref")

---

_Source: [https://wiki.freepascal.org/Variable_parameter](https://web.archive.org/web/20250208191425/https://wiki.freepascal.org/Variable_parameter)_
