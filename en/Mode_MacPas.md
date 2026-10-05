# Mode MacPas

│ **English (en)** │  [**français (fr)**](</Mode_MacPas/fr> "Mode MacPas/fr") │    


Mode MacPas tries to be compatible with the Pascal dialects commonly used on [Macintosh](</Category:Mac_OS_Classic> "Category:Mac OS Classic"), that is [THINK Pascal](<THINK_Pascal.md> "THINK Pascal"), [Metrowerks Pascal](</index.php?title=Metrowerks_Pascal&action=edit&redlink=1> "Metrowerks Pascal \(page does not exist\)") and [MPW Pascal](</index.php?title=MPW_Pascal&action=edit&redlink=1> "MPW Pascal \(page does not exist\)"). For tips on porting from traditional [Mac Pascal](<Mac_Pascal.md> "Mac Pascal") compiler to [Free Pascal](<Free_Pascal.md> "Free Pascal"), see [Porting from Mac Pascal](<Porting_from_Mac_Pascal.md> "Porting from Mac Pascal")

## Contents

  * 1 Differences compared to standard Free Pascal
    * 1.1 Implemented (in 2.0.2)
      * 1.1.1 Compile time constructs
      * 1.1.2 Language
    * 1.2 Implemented (in 2.1.1)
    * 1.3 Planned
    * 1.4 Not Supported
    * 1.5 Free Pascal constructs allowed in Mode MacPas
  * 2 Exportable macros
  * 3 Apple's Universal Interfaces



## Differences compared to standard Free Pascal

Here differences between the Macintosh dialect of Pascal and the standard dialect of Free Pascal (which is Turbo Pascal) are listed. 

### Implemented (in 2.0.2)

  * The file extension .p is supported, in mode MacPas. 
  * The unit MacPas.pp is automatically included whenever mode MacPas is used. It should contain such stuff that is normally built in into mac pascal compiler. 
  * $CALLING MWPASCAL Tells the compiler to use calling conventions according to Metrowerks Pascal. The only difference to the default is that, in MWPASCAL, CONST record parameters are always passed by reference. 
  * Type checking of compile time expression (actually this is not MacPas specific) 
  * Compile time variable can be given on the command line, syntax -dMYCOMPVAR:=3 or -dMYCOMPVAR2:=TRUE (no spaces). Hexadecimal values are not supported, though. 



#### Compile time constructs

  * Compile time variables are now different from macros. However, they are related. In the FPC documentation, what is written about macros, is often also valid for compile time variables. The difference is that macro substitution is only done with real macros (which are defined by $DEFINEC). Compile time variables on the other hand is defined by $SETC. 
  * Exportable compile time variables/macros. Compile time variables and macros defined in the interface part of a unit are exported to other MACPAS units, in the same manner as ordinary pascal constructs. See more below under **Exportable macros**. 
  * Compile time variables may be assigned to hexadecimal numbers, like $2345. 
  * $SETC <compvar>:= <expr> \-- sets a compiler variable's value to an expression. ":=" as well as "=" are allowed. If not already defined, it is defined. 
  * $DEFINEC <macro> <content> defines a macro <macro>, whose substitution text is <content>. -- as its counterpart $DEFINE. Parameterised macros are not supported. 
  * $IFC, $ELSEC, $ENDC -- as their counterparts $IF, $ELSE, $END 
  * $ERRORC directive, similar to $ERROR 
  * UNDEFINED <compvar> \-- in compiler variable expressions 
  * Compile time function OPTION(X), which returns, whether the compiler option X is set. Works for one letter options only. 
  * TRUE, FALSE -- as values in compiler variable expressions 
  * Compiler directive $PUSH and $POP, which saves/restores the current state of all local compiler switches. 
  * Compiler option $J makes a variable external 
  * Compiler option $Z makes variables and procedures externally visible 
  * Compiler option $OV (+/-) sets overflow checking (fpc 2.5.1) 
  * $ALIGN MAC68K, POWER, RESET (also POWERPC as equivalent of POWER in fpc 2.5.1) 
  * Compiler directive LibExport is recognized, but does nothing at the moment. It may be implemented in the future. 
  * Metrowerks compiler directive $UNDEFC and $ELIFC 
  * Metrowerks compile time expression DEFINED <compvar>
  * Support for some Turbo/Delphi compiler directives as well, $DEFINE $UNDEF $IFDEF $IFNDEF $IF $ELSE $ENDIF $ELSEIF 



#### Language

  * Object Pascal, Mac style. New, Dispose, Member are supported. 
  * A procedure passed as parameter can be directly declared in the formal parameter list (anonymous procedure types). 
  * OTHERWISE in case constructs (actually it was already supported even for Turbo Pascal) 
  * Procedure Leave does the same as Break 
  * Procedure Cycle does the same as Continue 
  * Procedure Exit(<procname>), which works like Exit, but accepts the procname as parameter. <procname> must be the name of the procedure, in which Exit is used in. Non-local exit (as allowed in some Mac pascal implementations) is not allowed. Exit with return value as a parameter is not allowed. You can use "return <exit_value>;" though, as in Metrowerks Pascal. 
  * Operators | and & (boolean operators, which are always evaluated as short-circuit boolean expressions) 
  * UNIV modifer for types in parameter lists is accepted, but does nothing (update, it has been implemented in fpc 2.5.1) 
  * C directive for procedure declarations. Same as CDECL. 
  * "..." in procedure declaration, denoting a C var arg style funtion. 
  * IMPLEMENTATION is not needed in units, if empty. 
  * External directive 
  * Procedures declared in the interface section, which do not have a counterpart in the implementation section, are considered external (we call them implicit externals). Their external name is prefixed with the C_prefix of the target. On Mac OS this is an empty string, whereas on Darwin/Mac OS X it is an underscore, so that it will properly link with Mac OS X' libraries (which are compiled with gcc). 
  * Ord function can take a pointer as argument 
  * Use of FourCharCode ( e. g. OSType, ResType) constants directly as parameters. This is done by an overloaded assignment operator defined in the file MacPas.pp 
  * Ord4 
  * All kinds of procedure variable stuff, compatible with Think Pascal and Metrowerks Pascal. 



### Implemented (in 2.1.1)

  * return <value>
  * compile time expressions are now short circuit evaluated 
  * Bit-level packing of records and arrays 



### Planned

  * Nested procedure parameters (allowing a nested procedure to be actual parameter to another procedure) (update, it has been implemented in fpc 2.5.1) 
  * Propagating uses 
  * Open (for files) 
  * HiWrd, LoWrd (since supported in both Think Pascal and Metrowerks) 



### Not Supported

Here is an (incomplete) list of mac pascal constructs which are not supported in mode mac at the moment, perhaps some of them will be supported in the future: 

  * StringOf 
  * HiWord, LoWord (same as HiWrd, LoWrd) 
  * Implicit forward declaration of objects (In an object declaration, one cannot refer to an object not yet declared) 
  * In object declarations, the method name cannot be preceeded with the class name (as in Think Pascal) 
  * Arithmetic compiler variable expressions 
  * Goto between different nesting levels (e.g., no goto from a nested procedure to a parent procedure) 
  * Exit between different nesting levels. 
  * Working implementation of UNIV keyword (update, it has been implemented in fpc 2.5.1) 



### Free Pascal constructs allowed in Mode MacPas

Free Pascal constructs which currently are allowed in Mode MacPas, but which actually are not in the Macintosh dialect. If conflicts arises, some of them might be disallowed in future. Traditionally some of this functionallity has been offered through Apples Universal Interfaces. The list is not complete. 

  * Assign(filename, file). In macintosh pascal the file name is given as an (optional) parameter directly to Reset(file, filename), Rewrite(file, filename) or Open(file, filename) (Open do open for both reading and writing). 
  * Bitwise operators "shr". "shl", "and", "or", "not" etc. In macintosh pascal the functions BSR, BSL, BAND, BOR, BNOT is used instead. (Boolean operators "and", "or", "not" is of course allowed in macintosh pascal) 
  * Assigned(pointer). An alternative is "pointer <> nil". 
  * Append, Blockread, BlockWrite, Erase (delete files from disk), Filesize, Flush, Rename, SeekEof, SeekEoln, SetTextBuf, Truncate. 
  * Addr, Compare*, *Seg, Fill*, Freemem, Getmem, High, Low, Move, MoveChar0, Ofs, Ptr (Note: In Apples Universal Interfaces, Ptr is a datatype describing a generic pointer) , ReAllocMem, Release. 
  * Hi, Lo. For convertion to 16 bit integers, in macinstosh pascal you can use HiWord, LoWord. Note that Hi/Lo work on 64, 32 and 16 bit integers and return the upper/lower 32/16/8 bits depending on the argument. 
  * Int 
  * BinStr, HexStr, OctStr, SetLength, SetString, StringOfChar, Val, Str 
  * ChDir, GetDir, MkDir, RmDir, Runerror 
  * Paramcount, Paramstr (since classic Mac OS does not have command line interface) 
  * Halt (param). Halt without parameter works in macintosh pascal. 
  * Assert 
  * Exclude, Include (for sets) 
  * LongJmp, SetJmp (exists in Metrowerks but not Think) 
  * Direct pointer arithmetics. In macintosh pascal, you must cast pointers to numbers before doing arithmetics. 



## Exportable macros

Only a mode MacPas unit will export macros and only a mode MacPas unit will import them. As before, MacPas and non-MacPas units can be used by each other but macros will then not be visible for use. 

Macros will start to be exported right after the {$MODE MACPAS} directive. If no such directive is given, but -Mmacpas is given on the command line, macros will start to be exported right after the UNIT keyword. 

To support this, for a unit, {$MODE MACPAS} if forbidden after the UNIT keyword. For other mode switches it is like before. 

Caveat 1: If {$MODE MACPAS} is given before UNIT, the macros defined between this point and UNIT is exported, if the unit is compiled as part of a program or other unit, but is not exported if the unit is compiled from the commandline (at top level). To be deterministic, do not put macros before UNIT. 

Caveat 2: Deviation from Think Pascal and Metrowerks Pascal: Macros from used units is not in effect until after the whole uses clause (incl semicolon) has been parsed. 

## Apple's Universal Interfaces

Starting with FPC 2.04, Apple's Universal interfaces are included with FPC. They are from the same source tree that is available at <http://www.microbizz.nl/gpc.html>, only a slightly more recent version (r165 as of 2.0.4).

---

_Source: [https://wiki.freepascal.org/Mode_MacPas](https://web.archive.org/web/20190515002502/https://wiki.freepascal.org/Mode_MacPas)_
