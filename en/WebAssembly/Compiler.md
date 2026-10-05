# WebAssembly/Compiler

## Contents

  * 1 Prerequisites (optional!)
    * 1.1 Fedora
    * 1.2 Ubuntu
    * 1.3 macOS
      * 1.3.1 Catalina
      * 1.3.2 Monterey
    * 1.4 Windows 10
  * 2 Obtaining the compiler sources
    * 2.1 Linux and macOS
    * 2.2 Windows
  * 3 Building and installing
    * 3.1 Compilation target: WASIp1
      * 3.1.1 Linux and macOS
      * 3.1.2 Windows
    * 3.2 Compilation target: WASIp1threads
    * 3.3 Compilation target: Embedded
      * 3.3.1 Linux and macOS
      * 3.3.2 Windows
  * 4 Using the compiler
  * 5 Using the compiler with Lazarus
  * 6 See Also



# Prerequisites (optional!)

The initial FPC port for WebAssembly required the _wasm-ld_ from the LLVM project. LLVM 11.0 and 12.0.1 are known to work. The latest FPC version contains an internal linker, making this requirement strictly **optional**. You can skip this part if you want to. 

## Fedora

Development was done, using the Fedora packages llvm-11.0.0-1.fc33.x86_64 and lld-11.0.0-1.fc33.x86_64. 

## Ubuntu

Tested on Ubuntu 20.04. _wasm-ld_ is part of package _lld-12_. Install it and set necessary symlinks: 
    
    
    % sudo apt install lld-12
    % ln -sf /usr/lib/llvm-12/bin/wasm-ld ~/bin/wasm32-wasi-wasm-ld
    % ln -sf /usr/lib/llvm-12/bin/wasm-ld ~/bin/wasm32-embedded-wasm-ld
    

Note: This assumes you have a $HOME/bin directory and it is in PATH. 

## macOS

### Catalina

Tested on **macOS Catalina** with LLVM 11. For [MacPorts](<https://www.macports.org>) _wasm-ld_ is part of package _llvm-11_ ; upon installation it exists as _wasm-ld-mp-11_. Install the package and set necessary symlinks: 
    
    
    % sudo port install llvm-11
    % ln -sf /opt/local/bin/wasm-ld-mp-11 ~/bin/wasm32-wasi-wasm-ld
    % ln -sf /opt/local/bin/wasm-ld-mp-11 ~/bin/wasm32-embedded-wasm-ld
    

Note: This assumes you have a $HOME/bin directory and it is in PATH. 

### Monterey

Tested on **macOS Monterey 12.5.1** : wasm-ld is part of the official clang release for macOS: <https://releases.llvm.org/download.html#11.1.0>

Unpack it. When running wasm-ld, macOS might warn, that ldd cannot be verified. Open the System Preferences / Security / General. There should be ldd, unlock, then click Open Anyway. 

## Windows 10

Tested on Windows 10 Home 21H2. Download the [LLVM 12.0.1](<https://github.com/llvm/llvm-project/releases/tag/llvmorg-12.0.1>) 64-bit Windows installer. You are encouraged to verify the GPG signature on the installer. As administrator, run the installer. Let's say you installed into C:\llvm. Enable Windows 'developer mode' if you have not done so. Then set necessary symlinks: 
    
    
     C:\> cd llvm\bin
     C:\llvm\bin> mklink wasm32-wasi-wasm-ld.exe wasm-ld.exe
     C:\llvm\bin> mklink wasm32-embedded-wasm-ld.exe wasm-ld.exe
    

Add C:\llvm\bin to your PATH. 

# Obtaining the compiler sources

## Linux and macOS

The WebAssembly compiler target is part of the 'main' branch of the GitLab FPC source repo. Get the source: 
    
    
    % git clone <https://gitlab.com/freepascal.org/fpc/source.git> fpc
    

Enter the sources directory: 
    
    
    % cd fpc
    

## Windows

The WebAssembly compiler target is part of the 'main' branch of the GitLab FPC source repo. Get the source: 
    
    
    C:\Users\OnlyMe> git clone <https://gitlab.com/freepascal.org/fpc/source.git> fpc
    

Enter the sources directory: 
    
    
    C:\Users\OnlyMe> cd fpc
    

# Building and installing

If you are new to building the compiler, see [Installing the Free Pascal Compiler](<../Installing_the_Free_Pascal_Compiler.md> "Installing the Free Pascal Compiler") and [Cross compiling](<../Cross_compiling.md> "Cross compiling"). 

FPC supports two WebAssembly compilation targets: WASI and embedded. 

## Compilation target: WASIp1

This target supports the WebAssembly System Interface Preview 1 (WASI 0.1). 

### Linux and macOS

Build the compiler and FPC units for the WASIp1 target: 
    
    
    % make clean all OS_TARGET=wasip1 CPU_TARGET=wasm32 BINUTILSPREFIX= OPT="-O-" PP=fpc
    

Then, install, using INSTALL_PREFIX to specify a destination within your home directory: 
    
    
    % make crossinstall OS_TARGET=wasip1 CPU_TARGET=wasm32 INSTALL_PREFIX=$HOME/fpcwasm
    

Note: On macOS you might have to add _-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk_ to _OPT_. 

Make a link to the cross compiler executable _ppcrosswasm32_ : 
    
    
    % ln -sf ~/fpcwasm/lib/fpc/3.3.1/ppcrosswasm32 ~/bin/ppcrosswasm32
    

Note: This assumes you have a $HOME/bin directory and it is in PATH. 

### Windows

Make sure the Windows native compiler fpc.exe and the FPC-bundled make.exe are in your PATH. Especially ensure that the FPC make.exe comes first in your PATH; using another make.exe (such as the one installed by Delphi) that happens to be ahead in PATH may not work. 

Build the compiler and FPC units for the WASIp1 target: 
    
    
    C:\Users\OnlyMe\fpc> make clean all OS_TARGET=wasip1 CPU_TARGET=wasm32 BINUTILSPREFIX= OPT="-O-" PP=fpc
    

Then, install, using INSTALL_PREFIX to specify a destination folder: 
    
    
    C:\Users\OnlyMe\fpc> make crossinstall OS_TARGET=wasip1 CPU_TARGET=wasm32 INSTALL_PREFIX=c:\fpcwasm PP=fpc
    

Add C:\fpcwasm\bin\x86_64-win64 to your PATH to easily run the compiler _ppcrosswasm32.exe_. 

## Compilation target: WASIp1threads

This target supports the WebAssembly System Interface Preview 1 (WASI 0.1) with the [wasi-threads](<https://github.com/WebAssembly/wasi-threads>) proposal. 

Building the compiler is the same, as described in the previous section, except that _OS_TARGET=_**wasip1** needs to be replaced with _OS_TARGET=_**wasip1threads**. 

To build programs for this platform, add the 
    
    
    -Twasip1threads
    

compiler option, otherwise it will target **wasip1** by default. 

## Compilation target: Embedded

### Linux and macOS

Build the compiler and FPC units for the 'embedded' target: 
    
    
    % make clean all OS_TARGET=embedded CPU_TARGET=wasm32 BINUTILSPREFIX= OPT="-O-" PP=fpc
    

Then, install, using INSTALL_PREFIX to specify a destination within your home directory: 
    
    
    % make crossinstall OS_TARGET=embedded CPU_TARGET=wasm32 INSTALL_PREFIX=$HOME/fpcwasm
    

If you have not built and installed the WASI cross compiler, then make a link to the cross compiler executable _ppcrosswasm32_ ; 
    
    
    % ln -sf ~/fpcwasm/lib/fpc/3.3.1/ppcrosswasm32 ~/bin/ppcrosswasm32
    

Note: This assumes you have a $HOME/bin directory and it is in PATH. 

### Windows

Make sure the Windows native compiler fpc.exe and the FPC-bundled make.exe are in your PATH. Especially ensure that the FPC make.exe comes first in your PATH; using another make.exe (such as the one installed by Delphi) that happens to be ahead in PATH may not work. 

Build the compiler and FPC units for the 'embedded' target: 
    
    
    C:\Users\OnlyMe\fpc> make clean all OS_TARGET=embedded CPU_TARGET=wasm32 BINUTILSPREFIX= OPT="-O-" PP=fpc
    

Then, install, using INSTALL_PREFIX to specify a destination folder: 
    
    
    C:\Users\OnlyMe\fpc> make crossinstall OS_TARGET=embedded CPU_TARGET=wasm32 INSTALL_PREFIX=c:\fpcwasm PP=fpc
    

Add C:\fpcwasm\bin\x86_64-win64 to your PATH if you have not already done so. 

# Using the compiler

For Linux and macOS, if you have installed the cross compiler and units into $HOME/fpcwasm (as per the commands used above), then you need to configure $HOME/.fpc.cfg so that the cross compiler can locate its units. 

For Ubuntu, add these lines: 
    
    
    #ifdef cpuwasm32
    -Fu/home/yourusername/fpcwasm/lib/fpc/$fpcversion/units/$fpctarget/*
    -Fu/home/yourusername/fpcwasm/lib/fpc/$fpcversion/units/$fpctarget/rtl
    #endif
    

For macOS, add these lines: 
    
    
    #ifdef cpuwasm32
    -Fu/Users/yourusername/fpcwasm/lib/fpc/$fpcversion/units/$fpctarget/*
    -Fu/Users/yourusername/fpcwasm/lib/fpc/$fpcversion/units/$fpctarget/rtl
    #endif
    

On Windows, if you have set C:\fpcwasm\bin\x86_64-win64 directory in your PATH, then _ppcrosswasm32.exe_ will be able to find its units. 

# Using the compiler with Lazarus

When you create a new Lazarus project for a wasm program you can use the _Simple Program_ template and then adjust the _Project / Project Options / Compiler Options_ : 

  * Paths / Target file name: Lazarus 2.3.0 (Dec 28th 2021) appends the required .wasm file extension to the output file. To support older IDEs disable _Apply conventions_ and set _< programname>.wasm_.
  * Config and Target / Target platform: 
    * Target OS: Wasi
    * Target CPU: wasm32
  * Debugging / Generate info for debugger: disable (Dec 28th 2021: otherwise you will get can't find unit linfodwrf)
  * Compiler Commands / Compiler / Command: If you are using different fpc versions / folders you can here set the path to ppcrosswasm32 (e.g. Linux: /usr/lib/fpc/3.3.1/ppcrosswasm32)



# See Also

  * [WebAssembly](<../WebAssembly.md> "WebAssembly")

---

_Source: [https://wiki.freepascal.org/WebAssembly/Compiler](https://web.archive.org/web/20250321030759/https://wiki.freepascal.org/WebAssembly/Compiler)_
