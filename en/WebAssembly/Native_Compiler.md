# WebAssembly/Native Compiler

Recently, Free Pascal's support for WebAssembly has improved so much, that the compiler is capable to compile itself for this platform and the new compiler is fully capable of compiling itself again. This page describes how to produce a native compiler for WebAssembly and how it can be used to compile other programs. 

## Building the native compiler

To obtain a working WebAssembly-hosted compiler, the following compiler options are recommended: 
    
    
    -O- -g- -CTbfexceptions -CTsaturatingfloattoint
    

Example commands for Linux: 
    
    
    make -j `nproc` clean OS_TARGET=wasip1 CPU_TARGET=wasm32 BINUTILSPREFIX= PP=/usr/bin/ppcx64
    make -j `nproc` all OS_TARGET=wasip1 CPU_TARGET=wasm32 BINUTILSPREFIX= PP=/usr/bin/ppcx64 CROSSOPT="-O- -g- -CTbfexceptions -CTsaturatingfloattoint"
    

This will produce a native compiler binary in the _compiler_ directory: **ppcwasm32.wasm**

## Running the native compiler with WasmTime

In order to compile programs, the compiler needs access to a file system. Filesystem access is completely restricted by default, so you need to specify the _-dir_ option, in order to give it access to certain directories. Note that you can also rename/remap the host directory names, so they are visible under a different path inside the guest. Another important thing to note is that the WASI API doesn't have the concept of a current directory, so the Free Pascal RTL emulates this internally. Since there's no official way to pass the name of the current directory from the host to the guest, the Free Pascal RTL uses the first shared directory as the default. This means that if you want to share the entire file system with the guest via the _\--dir /_ option, the guest will see _/_ as the default directory. If you want to share the entire file system, plus the current directory, you can do this: 
    
    
    wasmtime run --dir `pwd` --dir / ppcwasm32.wasm <options>
    

## Running a full native make cycle

Under Linux it is possible to register a handler for .wasm files, so they can be run natively, and thus do a full native **make all** , where the compiler compiles itself several times, then compiles the rtl with the final compiler. After that it compiles fpmake and tries to start building the packages in the _packages_ directory, but this fails, because the WASI API lacks an API function for launching a process, so the native fpmake.wasm doesn't work. 

Replicating this process isn't entirely trivial, because there are several caveats, that need to be worked around: 

  * The WASI API doesn't support setting the executable attribute on files, so the .wasm files, produced by the compiler don't have this attribute set. Because of that, Linux refuses to execute them.
  * The Free Pascal makefiles detects the source OS as Linux, instead of WASI (which is partially correct, it is indeed Linux, running WASI only via an emulation layer). This causes an inconsistency with the OS's expected executable file extension, so in certain places the makefiles try executing _ppcwasm32_ instead of _ppcwasm32.wasm_ and _fpmake_ instead of _fpmake.wasm_.
  * The default stack size of wasmtime needs to be increased, otherwise compilation of fpmake blows the stack.



Here's an example wrapper script **wasmtime_wrapper.sh** that contains hacks that workaround these issues: 
    
    
    #! /bin/sh
    
    set -e
    
    FPC_WASM_DIR=**/path/to/fpc**
    
    wasmtime run -W max-wasm-stack=16000000 --dir `pwd` --dir / "$@"
    # UGLY HACK 1: set the executable attribute of .wasm files, produced during compilation
    chmod --quiet +x $FPC_WASM_DIR/compiler/*.wasm $FPC_WASM_DIR/packages/fpmake.wasm || true
    # UGLY HACK 2: create symlinks, so that ppcwasm32.wasm and fpmake.wasm can be invoked without the .wasm extension
    ln -f -s ppcwasm32.wasm $FPC_WASM_DIR/compiler/ppcwasm32
    ln -f -s fpmake.wasm $FPC_WASM_DIR/packages/fpmake
    

Here's a script that registers a handler for executing .wasm files (needs to be executed as root): 
    
    
    #! /bin/sh
    
    { echo ':wasm32-wasi:M::\x00asm:\xff\xff\xff\xff:**/path/to/wasmtime_wrapper.sh** :
    ' > /proc/sys/fs/binfmt_misc/register; } 2>/dev/null
    

Here's a make command that should make the compiler rebuild itself, and the rtl: 
    
    
    make clean
    make all PP=**/path/to/ppcwasm32.wasm** OPT="-CTbfexceptions -CTsaturatingfloattoint" OVERRIDEVERSIONCHECK=1 NOWPOCYCLE=1

---

_Source: [https://wiki.freepascal.org/WebAssembly/Native_Compiler](https://web.archive.org/web/20250314060932/https://wiki.freepascal.org/WebAssembly/Native_Compiler)_
