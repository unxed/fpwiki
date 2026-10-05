# WebAssembly/Exceptions

## Contents

  * 1 Historical background
  * 2 Exception modes
    * 2.1 No exceptions
    * 2.2 Branchful exceptions
    * 2.3 Legacy exceptions
    * 2.4 Wasm exceptions



## Historical background

Initially WebAssembly did not offer an easy way to implement exceptions. Later, an extension to WebAssembly was proposed, to support exceptions. It was supported by all major browsers. Unfortunately, later this WebAssembly exceptions proposal was deprecated and replaced with another one. 

## Exception modes

To support this whole mess, Free Pascal supports 4 different exception modes. 

### No exceptions

The "No exceptions" mode disables the exception support. Exceptions cannot be handled in this mode, so raising an exception will abort the program. This mode is compatible with all WebAssembly implementations and does not require an extension to support exceptions. 

This mode is activated with the _-CTnoexceptions_ compiler option. 

### Branchful exceptions

This mode is activated with the _-CTbfexceptions_ compiler option. It doesn't require WebAssembly extensions for exception support, so it also works in all WebAssembly engines. It implements exceptions at a runtime performance cost - branch instructions are added after each function call, to check for raised exceptions. If an exception is detected, the code branches to the nearest enclosing exception handler in the current function, if there is one. Otherwise, the function returns early, so the next caller can handle the exception, simulating the stack unwinding that happens with exceptions. The downside to this mode is that it makes the "happy path" (when no exceptions are raised) slower. The compiler basically adds code to check a flag after every function call. 

### Legacy exceptions

This uses the first extension to WebAssembly to support exceptions: 

<https://github.com/WebAssembly/exception-handling/blob/master/proposals/exception-handling/legacy/Exceptions.md>

It is activated by the _-CTlegacyexceptions_ option. It is supported by the following engines: 

  * Chrome, since version 95
  * Firefox, since version 100
  * Safari, since version 15.2



### Wasm exceptions

This uses the newest WebAssembly exception support extension: 

<https://github.com/WebAssembly/exception-handling/blob/master/proposals/exception-handling/Exceptions.md>

It is activated by the _-CTwasmexceptions_ compiler option. It is supported by the following engines: 

  * Chrome, since version 137
  * Firefox, since version 131
  * Safari, since version 18.4
  * Wasmtime - experimental support since version 37.0.0. Requires the _-Wexceptions=y_ option to enable.

---

_Source: [https://wiki.freepascal.org/WebAssembly/Exceptions](https://web.archive.org/web/20260926182956/https://wiki.freepascal.org/WebAssembly/Exceptions)_
