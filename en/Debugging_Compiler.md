# Debugging Compiler

This page is about debugging the FPC compiler and thus it provides a tutorial and some hints that might be useful. 

## Step-by-step tutorial

  1. There is a FPC installation in `D:\Fcpupdeluxe\fpc`
  2. The FPC repository is checked out (same revision as the FPC installation) in another folder `D:\fpcsrc` and used to work on the compiler, thus the .lpi file is opened from this folder.
  3. There are multiple .lpi files for different architectures in the compiler directory to be used with the Lazarus IDE. Open the one which fits your architecture. In this tutorial we're using the `ppcx64.lpi` on Windows 10 64-bit.
  4. Now, if you build the compiler an executable called _pp_ will be created in the `compiler\x86_64` folder of your FPC source files (the folder where you opened the .lpi file from). This executable is your new built compiler that can be used to build any other program/unit/project while it is debugged from this Lazarus project.
  5. In the menu bar go to _Run_ and then click _Run parameters ..._ to specify the _Command line parameters_. 

    An example of command line parameters could look like this: 

    `-FuD:\Fcpupdeluxe\fpc\units\x86_64-win64\* D:\Delphi\tests\test.pas`
    Explanation of the command line parameters:
     1. `-FuD:\Fcpupdeluxe\fpc\units\x86_64-win64\*` <\- path to the units provided by FPC, otherwise the error _Fatal: Can't find unit system used by test_ will be shown and an _ECompilerAbort_ exception in verbose.pas is raised
     2. `D:\Delphi\tests\test.pas` <\- program or unit that should be compiled
  6. Now you can set breakpoints in Lazarus, e.g. in the `parser.compile` function and follow the flow of compiling something by using the functionality of Step Over (F8) or Step Into (F7).
  7. Once you added e.g. a new intrinsic to the compiler you need to recompile the RTL: 
     1. open Windows CMD
     2. switch to your FPC source folder, type `D:` and `cd D:\fpcsrc\`
     3. compile the system units, type `make rtl PP=D:\fpcsrc\compiler\x86_64\pp`
     4. change the _Command line parameters_ in Lazarus to use the new system units in _D:\fpcsrc\rtl\units\x86_64-win64_

    `-FuD:\fpcsrc\rtl\units\x86_64-win64`


  * Notes: 
    * The FPC installation in `D:\Fcpupdeluxe\fpc` is needed to bootstrap the initial compiler executable
    * The system units from `D:\Fcpupdeluxe\fpc` can be used as long as nothing in the compiler is changed that is related to these units, otherwise you need to recompile the system units as well (see steps above)
    * If you change any of the compiler source files the _pp_ executable will be overwritten to reflect the changes made
    * The compiled _pp_ executable can also be used to recompile the RTL (or packages) if needed, e.g. `make rtl PP=compiler\x86_64\pp`



## Debugging FPC Hints

  * There are some verbosity options (like gcc) to be specified for the command line to show internal representations like 
    * _-vp_ : Write tree.log with parse tree
    * _-vv_ : Write fpcdebug.txt with lots of debugging info


  * Place a breakpoint at the `parser.compile` function in Lazarus as this is the entry point for parsing.


  * Another useful procedure in which to place a breakpoint is `GenerateError` in verbose.pas. It's called for any kind of compilation error (syntax error, type error, assembler reader error, internal error, ...).


  * Steps to investigate internal error codes:


  1. search for the error code in the source folder (e.g. _rgrep_ on Linux or _grep -r_ from the git bash on Windows)
  2. open the related file and place a break point on the internal error call
  3. look at the stack trace to see the call history leading up to this point


  * Analyse code that leads to an error or warning:


  1. search for the error/warning code or a short text snippet of the message in the _compiler/msg_ folder
  2. the English message will be in _errore.msg_ file, e.g.: 

    `% The constants are out of their allowed range.`
    `parser_e_double_caselabel=03037_E_Duplicate case label`
  3. locate the constant name of the message (first part of the message before "="), e.g.: 

    `parser_e_double_caselabel`
  4. search for this constant in the compiler source files, example output: 

    `nset.pas: Message(parser_e_double_caselabel);`
    `nset.pas: Message(parser_e_double_caselabel);`


  * The subroutine `printnode(<node>)` allows to print a node and all it's children, this is often usefull when examining node trees at particular locations.


  * If code is generated that absolutely does not match with what should be generated, a good starting point is to compile with _-al -sr_ as this generates the assembly file pre-register allocation. If that one looks alright, you know the issue is in the register allocator/spilling code.


  * The XML Node output feature can be enabled via `-dDEBUG_NODE_XML` during compiler compilation and it generates a _< NAME>-node-dump.xml_ file for each unit, program or library compiled, containing a XML description of the nodes handled during compilation of the unit, program or library.

---

_Source: [https://wiki.freepascal.org/Debugging_Compiler](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Debugging_Compiler)_
