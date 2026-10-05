# WebAssembly/Reference types

The WebAssembly reference types extension adds support for two opaque types - externref and funcref. They are managed by the host and cannot be stored in linear memory. They can be used as function parameters (they can only be passed by value), local variables, function results, and as [WebAssembly globals](<Globals.md> "WebAssembly/Globals"). 

## Contents

  * 1 Free Pascal support
    * 1.1 externref
    * 1.2 funcref
    * 1.3 Restrictions
    * 1.4 Allowed operations
    * 1.5 Future features (Not yet implemented)
  * 2 See also



# Free Pascal support

Partial support for reference types was merged into the main FPC branch on Jun 26 2023. 

## externref

There is a new type in the System unit, called _WasmExternRef_. It represents the _externref_ WebAssembly type. 

## funcref

Procedural types can now be declared as _WasmFuncRef_. For example: 
    
    
    type
      TMyWasmFuncRef = function(a: longint; b: int64): longint; WasmFuncRef;
    

These types represent the _funcref_ WebAssembly type. 

## Restrictions

The WebAssembly reference types don't have an in-memory representation. Therefore, the following is not allowed: 

  * Taking their address
  * Taking their size, using sizeof() or bitsizeof()
  * Using them as a field in a record, object or class
  * Passing them as _var_ , _constref_ or _out_ parameters
  * Typecasting them to pointer, or integer, or any other type that is not a WebAssembly reference type
  * Comparing them by value (they can only be compared to _nil_)



## Allowed operations

The WebAssembly reference types can be used in the following ways: 

  * as procedure and function parameters, passed by value
  * as function results
  * as local variables
  * as [WebAssembly globals](<Globals.md> "WebAssembly/Globals")
  * they can be assigned the value _nil_
  * they can be compared to _nil_ , or used as a parameter to assigned()
  * they can be copied (a := b)



## Future features (Not yet implemented)

The following seem to be possible to implement in the future, but is not yet implemented: 

  * putting reference types inside tables, which can be modeled in Pascal as dynamic arrays, declared as [WebAssembly globals](<Globals.md> "WebAssembly/Globals")
  * calling funcrefs
  * converting a normal Pascal function to a funcref
  * with the [multivalue extension](<https://github.com/WebAssembly/spec/blob/master/proposals/multi-value/Overview.md>), it is possible to allow them as _out_ or _var_ parameters. In this case, they would need to be converted as extra function results.



An additional WebAssembly proposal might allow equality comparison between reference types (currently, only comparison to _nil_ is allowed). 

# See also

  * [The official specification](<https://github.com/WebAssembly/reference-types/blob/master/proposals/reference-types/Overview.md>)
  * [Proposal for adding WebAssembly Reference Types to Clang](<https://discourse.llvm.org/t/rfc-webassembly-reference-types-in-clang/66939>)

---

_Source: [https://wiki.freepascal.org/WebAssembly/Reference_types](https://web.archive.org/web/20250323002252/https://wiki.freepascal.org/WebAssembly/Reference_types)_
