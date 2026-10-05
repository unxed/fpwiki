# WebAssembly/Files

The WASI target supports filesystem access and this is implemented in the Free Pascal RTL. However, there are some caveats, due to the way WASI is designed. 

## Contents

  * 1 File access using wasmtime
  * 2 Caveats
    * 2.1 Current directory
    * 2.2 Relative paths



## File access using wasmtime

By default wasmtime disallows any file access, only standard input and output. This is because WebAssembly can be used for sandboxing and running untrusted code. To enable access to a directory, you need to run your program like that: 
    
    
    wasmtime run --dir _< DIRECTORY>_ program.wasm
    

You can also specify the way the directory is visible inside the program, like this: 
    
    
    wasmtime run --mapdir _< GUEST_DIR::HOST_DIR>_ program.wasm
    

For example, if you want _/home/blabla/tralala_ on your host system to be visible as the root directory in your WebAssembly program, you can do: 
    
    
     wasmtime run --mapdir /::/home/blabla/tralala program.wasm
    

## Caveats

### Current directory

There is no WASI API function to obtain or change the current directory. All the Free Pascal RTL functions for dealing with the current directory are actually emulated inside the RTL. However, there's no way to specify the initial directory, so, for example, if you run your program like that: 
    
    
    wasmtime run --dir / program.wasm
    

To grant it access to the root directory and therefore to the entire filesystem, you would expect that your program would also inherit the current directory, however that's not the case. Your program will assume the current directory is /. 

### Relative paths

If you give access to a directory, using a relative path to wasmtime, e.g.: 
    
    
    wasmtime run --dir . program.wasm
    

Then the full path will not be known to your program. It will simply see the current directory as ".". Some programs might not expect this, so it's better to use --mapdir in this case, e.g.: 
    
    
    wasmtime run --mapdir /::. program.wasm
    

This will make the current directory to be visible as / in your program.

---

_Source: [https://wiki.freepascal.org/WebAssembly/Files](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/WebAssembly/Files)_
