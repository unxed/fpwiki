# WebAssembly

## Contents

  * 1 WebAssembly
  * 2 Free Pascal and WebAssembly
  * 3 Demos
    * 3.1 Running in Web Browsers
    * 3.2 With Standalone Runtime
  * 4 See Also



# WebAssembly

WebAssembly (abbreviated Wasm) is a binary instruction format for a stack-based virtual machine. Wasm is designed as a portable compilation target for programming languages, enabling deployment on the web for client and server applications. See the [WebAssembly](<https://webassembly.org>) website for more information. 

# Free Pascal and WebAssembly

FPC supports three Wasm compilation targets: WASIp1 (WASI 0.1, also known as WASI Preview 1), WASIp1threads (WASI 0.1 with the wasi-threads proposal) and Embedded. See [WebAssembly/Compiler](<WebAssembly/Compiler.md> "WebAssembly/Compiler") on how to build and install FPC for Wasm. 

[WASI](<https://wasi.dev>) \- the WebAssembly System Interface - defines an API for operating system-like features, including files and filesystems, network sockets, clocks and random numbers. These features, when implemented in web browsers as well as standalone Wasm runtimes on desktops, servers, and serverless cloud computing units, are available to Pascal programs and libraries compiled by FPC to Wasm for the WASI target. 

The FPC WASI RTL consists of these packages: 

  * bzip2
  * chm
  * fcl-base
  * fcl-css
  * fcl-db
  * fcl-fpcunit
  * fcl-fpterm
  * fcl-hash
  * fcl-image
  * fcl-js
  * fcl-json
  * fcl-jsonschema
  * fcl-mustache
  * fcl-openapi
  * fcl-passrc
  * fcl-pdf
  * fcl-registry
  * fcl-res
  * fcl-sdo
  * fcl-sound
  * fcl-stl
  * fcl-web
  * fcl-xml
  * fcl-yaml
  * hash
  * hermes
  * libtar
  * pasjpeg
  * pastojs
  * paszlib
  * regexpr
  * rtl
  * rtl-extra
  * rtl-generics
  * rtl-objpas
  * rtl-unicode
  * symbolic
  * tplylib
  * vcl-compat
  * wasm-job
  * wasm-oi
  * wasm-utils
  * webidl



The FPC Wasm embedded RTL consists of these packages: 

  * rtl
  * rtl-extra
  * tplylib



With respect to the embedded target, there are presently (2022) early efforts to create Wasm-related standards for cross-device/platform/architecture embedded applications. 

Overall, FPC's Wasm support adds to FPC's already [extensive list of compilation targets](<https://www.freepascal.org/>), potentially allowing Pascal programs to run in even more environments than they already do. 

# Demos

## Running in Web Browsers

In each demo, the driver program is transpiled from Pascal to Javascript using [pas2js](<pas2js.md> "pas2js"), and the worker program/library is compiled from Pascal to Wasm using FPC WASI target. 

  * [Simulated terminal input and output](<https://www.freepascal.org/~michael/pas2js-demos/wasienv/terminal/>)
  * [Drawing on HTML canvas](<https://www.freepascal.org/~michael/pas2js-demos/wasienv/canvas/>)
  * [Conway's Game of Life](<https://github.com/PierceNg/wasm-demo/>)



The Free Pascal compiler itself run in the browser: [![fpcwasm-1.png](https://wiki.freepascal.org/images/6/64/fpcwasm-1.png)](</File:fpcwasm-1.png>)

For more information on how to build and use the native WebAssembly compiler, see: [WebAssembly/Native_Compiler](<WebAssembly/Native_Compiler.md> "WebAssembly/Native Compiler")

## With Standalone Runtime

Free Pascal's source tree contains [examples](<https://gitlab.com/freepascal.org/fpc/source/-/tree/main/packages/wasmtime/examples>) embedding the [wasmtime](<https://github.com/bytecodealliance/wasmtime>) standalone Wasm runtime in Pascal host programs. 

# See Also

  * [WebAssembly/Compiler](<WebAssembly/Compiler.md> "WebAssembly/Compiler")
  * [WebAssembly/Native_Compiler](<WebAssembly/Native_Compiler.md> "WebAssembly/Native Compiler")
  * [WebAssembly/Debugging](<WebAssembly/Debugging.md> "WebAssembly/Debugging")
  * [WebAssembly/Roadmap](<WebAssembly/Roadmap.md> "WebAssembly/Roadmap")
  * [WebAssembly/JS](<WebAssembly/JS.md> "WebAssembly/JS")
  * [WebAssembly/Files](<WebAssembly/Files.md> "WebAssembly/Files")
  * [WebAssembly/DOM](<WebAssembly/DOM.md> "WebAssembly/DOM")
  * [WebAssembly/Threads](<WebAssembly/Threads.md> "WebAssembly/Threads")
  * [WebAssembly/JS-Promise_Integration](<WebAssembly/JS-Promise_Integration.md> "WebAssembly/JS-Promise Integration")
  * [WebAssembly/Reference types](<WebAssembly/Reference_types.md> "WebAssembly/Reference types")



There is an external pet project to create a pascal interpreter, not related to Free Pascal: 

  * [Making a budget Pascal compiler to WebAssembly](<https://faizilham.github.io/making-budget-pascal-compiler>)

---

_Source: [https://wiki.freepascal.org/WebAssembly](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/WebAssembly)_
