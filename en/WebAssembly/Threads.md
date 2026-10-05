# WebAssembly/Threads

## Contents

  * 1 Thread support
  * 2 Memory limits
  * 3 Host implementations
    * 3.1 iwasm
    * 3.2 wasmtime
    * 3.3 Browsers
      * 3.3.1 Chrome
      * 3.3.2 Firefox
      * 3.3.3 Safari
  * 4 Technical details
    * 4.1 Atomic instructions
    * 4.2 Thread synchronization primitives
    * 4.3 Shared memory and passive segments
    * 4.4 Thread Local Storage (threadvars)
    * 4.5 Actually starting a thread
      * 4.5.1 Now
      * 4.5.2 Historically



# Thread support

Free Pascal implements the WASI threads proposal: 

<https://github.com/WebAssembly/wasi-threads>

To enable it, you must use the **wasip1threads** compiler target. A WASI host implementation that is known to work with threads is _iwasm_ from the [WebAssembly Micro Runtime](<https://github.com/bytecodealliance/wasm-micro-runtime>) project. 

# Memory limits

WebAssembly thread support requires the program to specify the maximum amount of memory it is going to use. By default, FPC specifies 256 MiB as the maximum amount of memory for multithreaded applications and 8 MiB for the default stack size. Thеsе valueс can be changed via the _$M_ directive: 
    
    
    {$M stacksize,initialmem,maxmem}
    

For example, to specify 1 MiB stack size, 256 MiB initial memory, and 512 MiB max memory, use: 
    
    
    {$M 1048576,268435456,536870912}
    

Setting too high value for max memory may result in too much wasted memory, especially on mobile browsers. It is also possible that some WebAssembly hosts (browsers) reject the module completely. 

Setting the value too low can result in out of memory errors in the Free Pascal program. 

For programs that don't know in advance the maximum amount of memory that they need, as of now (Sep 2024) WebAssembly still offers no solution, see the issue [Wasm needs a better memory management story](<https://github.com/WebAssembly/design/issues/1397>). 

# Host implementations

This section contains a list of known WebAssembly host implementations that support threads. 

## iwasm

From the [WebAssembly Micro Runtime](<https://github.com/bytecodealliance/wasm-micro-runtime>) project. 

Works pretty well. 

## wasmtime

[Project website](<https://wasmtime.dev/>)

Written in Rust. Claims to support WASI threads since version 15, but testing showed it didn't work. Even testing with C code, compiled with the official WASI C/C++ SDK didn't work. 

For this reason, FPC's WASI thread support was developed and tested against iwasm. However, after FPC's internal linker was updated to support threads, surprisingly, it turned out that wasmtime also works with threads with FPC compiled programs with the internal linker. But it doesn't work when the program was linked with the external linker (which is LLVM's linker). The reason for that is not yet known. 

## Browsers

TODO: add info about browser support. Setting up pas2js, etc. 

### Chrome

Threads are supported since version 74. 

### Firefox

Threads are supported since version 79. 

### Safari

Threads are supported since version 14.1 for desktop Safari and version 14.5 for iOS Safari. 

# Technical details

This page contains some collected informations on the features needed for thread support in WebAssembly (in the browser). 

Thread support consists of 5 parts: 

  * Atomic instructions.
  * Thread synchronization primitives.
  * Shared memory and passive segments.
  * Thread Local Storage (threadvars)
  * Actually starting a thread.



  


## Atomic instructions

[The proposed specs](<https://github.com/WebAssembly/threads/blob/master/proposals/threads/Overview.md>)

When the Free Pascal RTL is compiled with -CTwasmthreads, the following RTL functions will use the new atomic instructions and thus should be thread safe in a multithreaded environment: 
    
    
    [InterlockedDecrement](<https://www.freepascal.org/docs-html/current/rtl/system/interlockeddecrement.html>)
    [InterlockedIncrement](<https://www.freepascal.org/docs-html/current/rtl/system/interlockedincrement.html>)
    [InterlockedExchange](<https://www.freepascal.org/docs-html/current/rtl/system/interlockedexchange.html>)
    [InterlockedCompareExchange](<https://www.freepascal.org/docs-html/current/rtl/system/interlockedcompareexchange.html>)
    [InterlockedExchangeAdd](<https://www.freepascal.org/docs-html/current/rtl/system/interlockedexchangeadd.html>)
    

Note that these require proper alignment (4 bytes) of the target, otherwise they trap (i.e. terminate the program with a stack trace). 

In addition to that, there are many more atomic functions available in the WebAssembly unit: 
    
    
    const
      { Special values for the TimeoutNanoseconds parameter of AtomicWait }
      awtInfiniteTimeout = -1;
      { AtomicWait result values }
      awrOk = 0;       { woken by another agent in the cluster }
      awrNotEqual = 1; { the loaded value did not match the expected value }
      awrTimedOut = 2; { not woken before timeout expired }
    
    procedure AtomicFence; inline;
    
    function AtomicLoad(constref Mem: Int8): Int8; inline;
    function AtomicLoad(constref Mem: UInt8): UInt8; inline;
    function AtomicLoad(constref Mem: Int16): Int16; inline;
    function AtomicLoad(constref Mem: UInt16): UInt16; inline;
    function AtomicLoad(constref Mem: Int32): Int32; inline;
    function AtomicLoad(constref Mem: UInt32): UInt32; inline;
    function AtomicLoad(constref Mem: Int64): Int64; inline;
    function AtomicLoad(constref Mem: UInt64): UInt64; inline;
    
    procedure AtomicStore(out Mem: Int8; Data: Int8); inline;
    procedure AtomicStore(out Mem: UInt8; Data: UInt8); inline;
    procedure AtomicStore(out Mem: Int16; Data: Int16); inline;
    procedure AtomicStore(out Mem: UInt16; Data: UInt16); inline;
    procedure AtomicStore(out Mem: Int32; Data: Int32); inline;
    procedure AtomicStore(out Mem: UInt32; Data: UInt32); inline;
    procedure AtomicStore(out Mem: Int64; Data: Int64); inline;
    procedure AtomicStore(out Mem: UInt64; Data: UInt64); inline;
    
    function AtomicAdd(var Mem: Int8; Data: Int8): Int8; inline;
    function AtomicAdd(var Mem: UInt8; Data: UInt8): UInt8; inline;
    function AtomicAdd(var Mem: Int16; Data: Int16): Int16; inline;
    function AtomicAdd(var Mem: UInt16; Data: UInt16): UInt16; inline;
    function AtomicAdd(var Mem: Int32; Data: Int32): Int32; inline;
    function AtomicAdd(var Mem: UInt32; Data: UInt32): UInt32; inline;
    function AtomicAdd(var Mem: Int64; Data: Int64): Int64; inline;
    function AtomicAdd(var Mem: UInt64; Data: UInt64): UInt64; inline;
    
    function AtomicSub(var Mem: Int8; Data: Int8): Int8; inline;
    function AtomicSub(var Mem: UInt8; Data: UInt8): UInt8; inline;
    function AtomicSub(var Mem: Int16; Data: Int16): Int16; inline;
    function AtomicSub(var Mem: UInt16; Data: UInt16): UInt16; inline;
    function AtomicSub(var Mem: Int32; Data: Int32): Int32; inline;
    function AtomicSub(var Mem: UInt32; Data: UInt32): UInt32; inline;
    function AtomicSub(var Mem: Int64; Data: Int64): Int64; inline;
    function AtomicSub(var Mem: UInt64; Data: UInt64): UInt64; inline;
    
    function AtomicAnd(var Mem: Int8; Data: Int8): Int8; inline;
    function AtomicAnd(var Mem: UInt8; Data: UInt8): UInt8; inline;
    function AtomicAnd(var Mem: Int16; Data: Int16): Int16; inline;
    function AtomicAnd(var Mem: UInt16; Data: UInt16): UInt16; inline;
    function AtomicAnd(var Mem: Int32; Data: Int32): Int32; inline;
    function AtomicAnd(var Mem: UInt32; Data: UInt32): UInt32; inline;
    function AtomicAnd(var Mem: Int64; Data: Int64): Int64; inline;
    function AtomicAnd(var Mem: UInt64; Data: UInt64): UInt64; inline;
    
    function AtomicOr(var Mem: Int8; Data: Int8): Int8; inline;
    function AtomicOr(var Mem: UInt8; Data: UInt8): UInt8; inline;
    function AtomicOr(var Mem: Int16; Data: Int16): Int16; inline;
    function AtomicOr(var Mem: UInt16; Data: UInt16): UInt16; inline;
    function AtomicOr(var Mem: Int32; Data: Int32): Int32; inline;
    function AtomicOr(var Mem: UInt32; Data: UInt32): UInt32; inline;
    function AtomicOr(var Mem: Int64; Data: Int64): Int64; inline;
    function AtomicOr(var Mem: UInt64; Data: UInt64): UInt64; inline;
    
    function AtomicXor(var Mem: Int8; Data: Int8): Int8; inline;
    function AtomicXor(var Mem: UInt8; Data: UInt8): UInt8; inline;
    function AtomicXor(var Mem: Int16; Data: Int16): Int16; inline;
    function AtomicXor(var Mem: UInt16; Data: UInt16): UInt16; inline;
    function AtomicXor(var Mem: Int32; Data: Int32): Int32; inline;
    function AtomicXor(var Mem: UInt32; Data: UInt32): UInt32; inline;
    function AtomicXor(var Mem: Int64; Data: Int64): Int64; inline;
    function AtomicXor(var Mem: UInt64; Data: UInt64): UInt64; inline;
    
    function AtomicExchange(var Mem: Int8; Data: Int8): Int8; inline;
    function AtomicExchange(var Mem: UInt8; Data: UInt8): UInt8; inline;
    function AtomicExchange(var Mem: Int16; Data: Int16): Int16; inline;
    function AtomicExchange(var Mem: UInt16; Data: UInt16): UInt16; inline;
    function AtomicExchange(var Mem: Int32; Data: Int32): Int32; inline;
    function AtomicExchange(var Mem: UInt32; Data: UInt32): UInt32; inline;
    function AtomicExchange(var Mem: Int64; Data: Int64): Int64; inline;
    function AtomicExchange(var Mem: UInt64; Data: UInt64): UInt64; inline;
    
    function AtomicCompareExchange(var Mem: Int8; Compare, Data: Int8): Int8; inline;
    function AtomicCompareExchange(var Mem: UInt8; Compare, Data: UInt8): UInt8; inline;
    function AtomicCompareExchange(var Mem: Int16; Compare, Data: Int16): Int16; inline;
    function AtomicCompareExchange(var Mem: UInt16; Compare, Data: UInt16): UInt16; inline;
    function AtomicCompareExchange(var Mem: Int32; Compare, Data: Int32): Int32; inline;
    function AtomicCompareExchange(var Mem: UInt32; Compare, Data: UInt32): UInt32; inline;
    function AtomicCompareExchange(var Mem: Int64; Compare, Data: Int64): Int64; inline;
    function AtomicCompareExchange(var Mem: UInt64; Compare, Data: UInt64): UInt64; inline;
    
    function AtomicWait(constref Mem: Int32; Compare: Int32; TimeoutNanoseconds: Int64): Int32; inline;
    function AtomicWait(constref Mem: UInt32; Compare: UInt32; TimeoutNanoseconds: Int64): Int32; inline;
    function AtomicWait(constref Mem: Int64; Compare: Int64; TimeoutNanoseconds: Int64): Int32; inline;
    function AtomicWait(constref Mem: UInt64; Compare: UInt64; TimeoutNanoseconds: Int64): Int32; inline;
    
    function AtomicNotify(constref Mem: Int32; Count: UInt32): UInt32; inline;
    function AtomicNotify(constref Mem: UInt32; Count: UInt32): UInt32; inline;
    function AtomicNotify(constref Mem: Int64; Count: UInt32): UInt32; inline;
    function AtomicNotify(constref Mem: UInt64; Count: UInt32): UInt32; inline;
    

## Thread synchronization primitives

The RTL provides (and internally uses) several thread synchronization primitives: 

  * critical sections
  * auto reset events
  * manual reset events



These are all implemented using the atomic instructions, described in the previous section. 

## Shared memory and passive segments

First, the memory needs to be declared shared. 

Secondly, the data segments need to be declared passive segments and extra startup code should be generated by the compiler to initialize them only once. Without this, when the module is instantiated on a new WebWorker (in order to start a new thread), this will cause memory to be initialized again to the initial state, which is not what we want when starting a thread. 

Some info: [Shared Memory and Passive Segments](<https://github.com/WebAssembly/tool-conventions/blob/main/Linking.md#shared-memory-and-passive-segments>)

Turns out, this is all done by the LLVM linker (including the initialization startup code), when you pass the appropriate command line options. The compiler now passes these options to the linker, when a program is compiled with -CTbfexceptions. As a side effect, such programs no longer work with "wasmtime run --enable-features threads", but that's because wasmtime's threads support is incomplete. 

## Thread Local Storage (threadvars)

Special consideration is needed to support threadvars. 

More info here: [Thread Local Storage](<https://github.com/WebAssembly/tool-conventions/blob/main/Linking.md#thread-local-storage>)

Code generation for threadvar access is now implemented in FPC. It follows the ABI convention for TLS from Emscripten. However, it causes the LLVM 14 linker to crash. The LLVM 15 (release candidate) linker from Emscripten seems to work. The following intrinsics exist in the system unit, they should be used for things like setting up the TLS of the new thread, or for allocating memory for the TLS in the calling thread, before passing it to the JavaScript helper: 
    
    
    function fpc_wasm32_tls_size: SizeUInt; - returns the TLS size in bytes for the entire program
    function fpc_wasm32_tls_align: SizeUInt; - returns the alignment requirements for the TLS block for the program
    function fpc_wasm32_tls_base: Pointer; - the start of the TLS block for the current thread. Only becomes valid after calling __wasm_init_tls (which is a special function, generated by the linker).
    procedure fpc_wasm32_init_tls(memory: Pointer);external name '__wasm_init_tls';
    

TLS for the main thread works on startup, thanks to linker magic. The linker allocates a static area for the thread vars and sets initial values for the global variables that point to them, so when a module is instantiated, the threadvars already point to this fixed location. Therefore, it is not necessary to call _fpc_wasm32_init_tls_ for the main thread. However, it is absolutely crucial that _fpc_wasm32_init_tls_ is called on new non-main threads, before the first access to a threadvar, otherwise they would access the threadvars of the main thread, causing all sorts of trouble and race conditions. That's why FPC implements the _wasi_thread_start_ routine in inline asm and calls _fpc_wasm32_init_tls_ before executing any Pascal code. Note that in the branchful exceptions mode (when using _-CTbfexceptions_), a threadvar is accessed every function call, called from Pascal code. 

## Actually starting a thread

Webassembly relies on the hosting environment to actually start threads. 

### Now

The [WASI threads](<https://github.com/WebAssembly/wasi-threads>) proposal has been updated to include enough details, and now FPC implements it. A thread is started, by calling the following new API function: 
    
    
    Function wasi_thread_spawn(start_arg: Pointer) : LongInt; external 'wasi' name 'thread-spawn';
    

In turn, the host calls: 
    
    
    procedure wasi_thread_start(tid: longint; start_arg: Pointer);
    

Which must be exported by the current module: 
    
    
    exports wasi_thread_start;
    

This is all done internally in the FPC RTL now. Users shouldn't call these functions directly, but instead use the standard FPC APIs for threads (e.g. the _TThread_ class, or calling _BeginThread_ from the system unit). 

### Historically

This section is outdated and left here for historical purposes. 

Some extra info: 

  * [something like specs](<https://web.dev/webassembly-threads/>)
  * [A demo](<https://rreverser.com/wasm-bindgen-rayon-demo/>)
  * [related](<https://wicg.github.io/file-system-access/>)
  * [WASI Native Threads API proposal](<https://github.com/WebAssembly/wasi-native-threads>)



Unfortunately, the WASI Native Threads API proposal is very incomplete. Emscripten implements threads using a different API/ABI, but it's quite messy and poorly documented. 

Starting a thread requires the following steps: 

  * **WebAssembly** : allocating a block of memory for the stack and TLS (threadvar) block for the new thread. This needs to be done in a thread safe manner (can we use the heap?). TODO: How do we determine the stack size for the new thread? Do we use the same main stack size as specified by the {$M _stacksize_} directive?
  * **WebAssembly** : calling an external (JavaScript) function and passing at least the following data, that needs to be passed to the new thread by JavaScript code:


    
    
     - the start address of the new thread procedure
     - the args that need to be passed to the new thread procedure
     - the stack and TLS address
     - TODO: how/where do we determine the thread ID?
    

What does Emscripten do? 
    
    
    int __pthread_create_js(struct pthread *thread, const pthread_attr_t *attr, void *(*start_routine) (void *), void *arg);
    

Basically, it exposes the pthread structure to the JavaScript code, which expects certain things (stack start, stack limit, TLS start, thread ID, etc.) to be at certain fixed offsets. 

What does the WASI Native Threads API proposal do? 
    
    
    status thread_spawn(thread_id* thread_id, const thread_attributes* attrs, thread_start_func* function, thread_args* arg);
    

It doesn't (yet) define what thread_id or thread_attributes are. 

  * **JavaScript** : TBD...delegate the new thread to a Web Worker, pass the arguments, etc. Decisions: Should the main thread be started in the main JavaScript GUI thread or inside a worker? Emscripten supports both (via PROXY_TO_PTHREAD). Should we support both as well? Other caveats: [[1]](<https://emscripten.org/docs/porting/pthreads.html#blocking-on-the-main-browser-thread>) and [[2]](<https://emscripten.org/docs/porting/pthreads.html#special-considerations>)
  * **WebAssembly** : a function that sets up the new thread:


    
    
     - setting up the stack pointer in linear memory (might need inline asm, an external wasm module or some compiler magic helper code generation, because no local variables can be used, before this is set up)
     - initialize global variables that hold the TLS (threadvar) block (calling __wasm_init_tls). Threadvars should not be used before this point.
     - maybe initialize some threadvars, that contain information about the current thread (is it the main thread, is it run in the main browser thread or in a worker, can it use atomic wait, or should it busy-wait instead?)
     - call the actual thread function and pass its parameters
    

What does Emscripten do? 
    
    
    void _emscripten_thread_init(pthread_t ptr, int is_main, int is_runtime, int can_block, int start_profiling);
    

What does the WASI Native Threads API proposal do? 
    
    
    - It only mentions a function, called __start_thread_ , but doesn't specify its parameters.

---

_Source: [https://wiki.freepascal.org/WebAssembly/Threads](https://web.archive.org/web/20250314060932/https://wiki.freepascal.org/WebAssembly/Threads)_
