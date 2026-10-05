# Sinclair QL

The Sinclair QL or QDOS port is an experimental "just for fun" [m68k](<m68k.md> "m68k") target. It targets the Sinclair QL line of computers from the mid-80s, and their operating system QDOS. This port is currently a cross-compiler only target. This is mostly due to hardware limitations of the original unexpanded platform. Later QL clones could run the compiler in theory, however, there are no plans to support these due to their extreme rarity. 

The initial version of the QL port was implemented by [Karoly Balogh](</User:Chain-Q> "User:Chain-Q") during #QLvember 2020, when various online retro communities and YouTubers were running Sinclair QL related projects. Several other people from [qlforum.co.uk](<https://qlforum.co.uk>) contributed to it since. 

## Contents

  * 1 Requirements
  * 2 Identification
  * 3 Defaults
    * 3.1 CPU
    * 3.2 Alignment
    * 3.3 Stack
  * 4 External Dependencies
    * 4.1 Assembler
      * 4.1.1 Building vasm
    * 4.2 Linker
    * 4.3 Licensing
  * 5 Building
  * 6 Compiler Arguments
    * 6.1 Setting metadata format
  * 7 QL Specific API in the System unit
    * 7.1 Job Name
    * 7.2 Console Opening/Closing
  * 8 Accessing the native APIs
  * 9 Object Pascal Notes



# Requirements

The compiled binaries - depending on the size - should be able to run on any Sinclair QL. Memory expansion for more complex programs is highly recommended. 

# Identification

To identify the Sinclair QL compile-time, use **{$IFDEF SINCLAIRQL}**. 

# Defaults

This section documents several compiler default settings, which are specific to the QL. Settings not documented here are usually the default for the m68k CPU architecture. 

## CPU

The default CPU target for the Sinclair QL is the 68000. This is different from most other [m68k](<m68k.md> "m68k") targets where the default is the 68020. The SoftFPU is enabled by default for the QL. 

## Alignment

Record elements are aligned to WORD (2 bytes) on the Sinclair QL by default. Use the **{$PACKRECORDS n}** compiler directive, if you want to change the default aligment. For byte aligned records, a packed record is also possible. Free Pascal also has support for 68000/68008 [special alignment requirements](<m68k.md> "m68k"). 

## Stack

The default stack size is set to 8 KiB, which is quite small for a Pascal program. For more complex programs you'll probably need to increase it. To change the stack size, use the **{SMEMORY <stacksize>,<heapsize>}** directive, or the **-Cs** command line argument. See the details in the documentation. 

  


# External Dependencies

The Sinclair QL port depends on **vasm** and **vlink** , as external assembler and linker. 

## Assembler

The only supported assembler for the Sinclair QL port is **vasm** , which is also the default. The Sinclair QL uses the **vasm** -specific _.vobj_ object format. This means using **vasm** also requires **vlink** as linker. **vasm** is open source, and it is available [here](<http://sun.hasenbraten.de/vasm>). Only **vasm** versions 1.8 and newer were tested with the Sinclair QL port, older versions might not work. 

**WARNING:** **vasm** versions between 1.9a and 1.9c - these versions inclusive - contain a bug, which makes it impossible to use these versions with Free Pascal on any [m68k](<m68k.md> "m68k") platform. **Please stick to vasm versions 1.8 (any letter), 1.9 (no letter) or 1.9d, or newer.**

### Building vasm

Note that **vasm** for [m68k](<m68k.md> "m68k") needs to be built with the _standard_ syntax module to work with Free Pascal. This is not the default, as most programmers and compilers prefer the _Motorola_ syntax module. A **vasm** version with the _standard_ syntax module can be built with the following command, issued in the root of the **vasm** source tree: 
    
    
     make CPU=m68k SYNTAX=std
    

The resulting _vasmm68k_std_ executable file is the assembler Free Pascal needs. 

## Linker

The Sinclair QL port defaults to **vlink** by Frank Wille as the default linker, which is the only supported linker for this target. **vlink** is open source, and it is available [here](<http://sun.hasenbraten.de/vlink>). 

Please make sure you use **vlink** version 0.16h or newer. Version 0.16h is the first version which contains a QL binary target, which Free Pascal now requires. Earlier versions will no longer work. 

## Licensing

Please note that the license of **vasm** and **vlink** does not allow development of commercial applications. If you plan to develop and sell a commercial application using these tools, you need their author's written consent. Please see the documentation of [vasm](<http://sun.hasenbraten.de/vasm/release/vasm.html>) and [vlink](<http://sun.hasenbraten.de/vlink/release/vlink.pdf>) for details. 

# Building

A very detailed build document for beginners is provided by Norman Dunbar, [available from his GitHub page](<https://github.com/NormanDunbar/FPC-CrossCompiler-QL/releases/latest>). It's a more detailed step-by-step description of the summarized process that is described in this wiki. 

To build a Sinclair QL cross-compiler, follow these steps: 

  1. Install the latest stable FPC version for your host platform. This will be used as the startup compiler.
  2. Check out FPC git main branch into a directory.
  3. Make sure you have the binaries **vasmm68k_std** and **vlink** in the **PATH**. _(See above.)_ Their binaries must be named (or symlinked) as **m68k-sinclairql-vasmm68k_std** and **m68k-sinclairql-vlink**.
  4. Go to the trunk's main directory and execute the following commands:


    
    
      make clean OS_TARGET=sinclairql CPU_TARGET=m68k
      make crossall OS_TARGET=sinclairql CPU_TARGET=m68k
      make crossinstall OS_TARGET=sinclairql CPU_TARGET=m68k INSTALL_PREFIX="<path/to/install>"
    

If you did everything right, you will find a working Sinclair QL cross-compiler with the pre-built RTL and Packages units in the install path you've specified. If you plan to rebuild FPC often, you might want to put the above commands in a script for easy rebuilding. 

Now, lets create a default **fpc.cfg** for Sinclair QL cross compiling. Create a file called **< path/to/install>/lib/fpc/etc/fpc.cfg**. _(Note: this is the path on Unix systems. Free Pascal on other platforms might expect a different path.)_ Put the following lines into that file, and fix up the paths: 
    
    
     #IFDEF CPUM68K
     -Fu<path/to/install>/lib/fpc/$fpcversion/units/$fpccpu-$fpcos
     -Fu<path/to/install>/lib/fpc/$fpcversion/units/$fpccpu-$fpcos/*
     #IFDEF SINCLAIRQL
     -FD</path/to/vasm-and-vlink>
     -XPm68k-sinclairql-
     #ENDIF
     #ENDIF
    

Optionally add **< path/to/install>/lib/fpc/3.3.1/** directory to the **PATH** , so you'll have direct access to the cross-compiler. _(Note: this is the path on Unix systems. Free Pascal on other platforms the**ppcross68k** compiler binary might be in a different path.)_ If everything went right, you now should be able to build Sinclair QL executables with: 
    
    
     ppcross68k -Tsinclairql <source.pas>
    

If the Sinclair QL is the only target you want to target for m68k with this config file, you may want to add the option **-Tsinclairql** into the config file as well, just make sure you add it as the first argument, right after **#IFDEF CPUM68K**. 

# Compiler Arguments

The Sinclair QL port has a few platform specific compiler command line arguments. 

## Setting metadata format

Using the **-WQ <x>** argument it's possible to set the generated metadata style for the executable, to either Q-emuLator/QPC v5 compatible "QDOS File Header" style or to XTcc style, which can be used by [SMSQmulator](<https://wlenerz.com/smsqmulator/>), InfoZIP and various other tools: 
    
    
         -WQ<x>     Set executable metadata format (Sinclair QL)
            -WQqhdr    Set metadata to QDOS File Header style (default)
            -WQxtcc    Set metadata to XTcc style
    

The default is "qhdr", and doesn't need to be explicitly specified. 

# QL Specific API in the System unit

## Job Name

The allowed maximum job name is 48 ($30) characters in length. The job name as wired into the binary header/start is always **FPC_PROG**. The Sinclair QL compiler has an additional feature, where it stores the main program name into the resulting binary, then the System unit sets it as the Job name during initialization. 

For example: 
    
    
    program Hello;
    

results in "Hello" as the job name. Capitalization is kept. In case the "program" keyword is omitted, the compiler uses "Program" as the default name. 

The System unit in the Sinclair QL RTL provides the following functions to allow easy run time manipulation of the Job name: 
    
    
    { sets the Job name directly from a Pascal string }
    { returns the number of characters successfully set as the Job name, or -1 if there was an error }
    function SetQLJobName(const s: string): longint;
    
    { return the current Job name as a Pascal string, or empty string if there was an error }
    function GetQLJobName: string;
    
    { return a pointer to the Job name stored as a QL string (2 byte length + series of characters), or nil if there was an error }
    function GetQLJobNamePtr: pointer;
    

## Console Opening/Closing

On the QL, the console of the program is opened and managed by the runtime library. This causes an issue, because different programs running in different environments might have special needs, which the RTL console opening/closing code code didn't or couldn't account for. To solve this issue, Free Pascal provides hooks into the console opening/closing of the system unit via _weak linking_. This allows overriding console opening/closing from an user program. 
    
    
    { define to override the default QL console opening function of the System unit }
    function QLOpenCon(var console: QLConHandle): boolean; public name 'QLOpenCon';
    
    { define to override the default QL console closing function of the System unit }
    procedure QLCloseCon(var console: QLConHandle); public name 'QLCloseCon';
    

The **QLOpenCon** call must completely fill out the "console" variable of **QLConHandle** type. The same record will be passed to **QLCloseCon** on program exit. The **QLConHandle** type is a 16 bytes long record, and it's defined as follows: 
    
    
    type
      QLConHandle = record
        inputHandle: longint; { channel ID to be used as standard input }
        outputHandle: longint; { channel ID to be used as standard output }
        errorHandle: longint; { channel ID to be used as standard error output }
        userData: pointer; { pointer to user specific data, must be set to nil when not used }
      end;
    

It's considered unsafe to override only one of **QLOpenCon** and **QLCloseCon**. 

Additionally, a helper function is proved to change the _Press any key to exit_ message at program termination. 
    
    
    { override the default "Press any key to exit" message of the System unit }
    { if msg is set to nil or empty string, the exit message is omitted }
    procedure SetQLDefaultConExitMessage(const msg: PChar);
    

Call **SetQLDefaultConExitMessage** from your main program. If **QLCloseCon** is overridden, this call doesn't have any effect. 

# Accessing the native APIs

Free Pascal provides a **qlunits** package, which is found in **packages/qlunits** , which contains the native API definitions for QDOS and SMSQ, and various helper units to interact with the native API functions from Pascal code, including functions to convert _Ansistring_ to _qlstring_ and _integer_ to _qlfloat_ and _IEEE 754 double_ to _qlfloat_. The **qlunits** package is an early stage at this point, and needs to be extended to cover more parts of the native API. 

# Object Pascal Notes

Object Pascal as a language is fully available on the Sinclair QL. However, a lot of the advanced Object Pascal features and especially existing code depend on units **sysutils** and **classes** , and a series of other units depending on these. The additional code weight of these units can add up to several hundred KiB, making the application impractical or even impossible to execute on an unexpanded Sinclair QL. 

Additionally, most of the external API provided by the **sysutils** unit is stub and not functional at this point. It will be implemented in the future.

---

_Source: [https://wiki.freepascal.org/Sinclair_QL](https://web.archive.org/web/20241206071807/https://wiki.freepascal.org/Sinclair_QL)_
