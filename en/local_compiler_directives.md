# local compiler directives

│ **[Deutsch (de)](</local_compiler_directives/de> "local compiler directives/de")** │  **English (en)** │  **[français (fr)](</local_compiler_directives/fr> "local compiler directives/fr")** │    
****

Local [compiler directives](<Compiler_directive.md> "Compiler directive") may be used more than once in a Pascal [source code](<Source_code.md> "Source code") file. 

## Contents

  * 1 Syntax
  * 2 Data layout
  * 3 Code generation
    * 3.1 $R versus $Q
  * 4 Platform-specific
    * 4.1 For x86 processors only
    * 4.2 Other
  * 5 Data inclusion
  * 6 Compile-time context
  * 7 Conditional compilation
  * 8 Compile-time behavior
  * 9 Ignored
  * 10 Historical
  * 11 See also



## Syntax

  * `{$cOperators}` enables usage of [operators](<Operator.md> "Operator") similar to conventions of in the [C](<Pascal_for_C_users.md> "Pascal for C users") language.
  * [`{$goto}`](<sGoto.md> "sGoto") enables [`goto`](<Goto.md> "Goto") and [`label`](<Label.md> "Label").
  * `{$inline}` allows the [`inline`](<Inline.md> "Inline") [modifier](<modifier.md> "modifier").
  * [`{$LongStrings}` or `{$H}`](<$H.md> "$H") determines the data type referenced by the reserved word [`string`](<String.md> "String").
  * `{$macro}` enables usage of [macros](</index.php?title=Macros&action=edit&redlink=1> "Macros \(page does not exist\)").
  * [`{$scopedEnums}`](</index.php?title=$scopedEnums&action=edit&redlink=1> "$scopedEnums \(page does not exist\)") whether enumeration type members have to be referred to by the data types name as a scope. (since [FPC 2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0"))
  * [`{$static}`](</index.php?title=$static&action=edit&redlink=1> "$static \(page does not exist\)") enable usage of the reserved word `static` (until [FPC 2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0")).
  * [`{$typedAddress}` or `{$T}`](</index.php?title=$typedAddress&action=edit&redlink=1> "$typedAddress \(page does not exist\)") determines, if the address operator [`@`](<@.md> "@") results in a typed or untyped pointer.
  * [`{$varStringChecks}` and `{$V}`](</index.php?title=$varStringChecks&action=edit&redlink=1> "$varStringChecks \(page does not exist\)") enables strict checking of assignment compatibility of string variables.
  * [`{$writableConst}` or `{$J}`](</index.php?title=$writableConst&action=edit&redlink=1> "$writableConst \(page does not exist\)") enables assignment of values to typed [constants](<Constant.md> "Constant") during [run-time](<runtime.md> "runtime").



## Data layout

  * [`{$align}`](<$A.md> "$A") and [`{$A}`](<$A.md> "$A") determine the alignment of data in records
  * [`{$bitpacking}`](<$Bitpacking.md> "$Bitpacking") determines, whether `packed` is interpreted as `bitpacked`.
  * [`{$codeAlign}`](</index.php?title=$codeAlign&action=edit&redlink=1> "$codeAlign \(page does not exist\)") determines the code-alignment in memory.
  * `{$minEnumSize}` recognized for Delphi-compatibility and has the same effect as the `{$packEnum}` directive.
  * [`{$minFPConstPrec}`](</index.php?title=$minFPConstPrec&action=edit&redlink=1> "$minFPConstPrec \(page does not exist\)") sets the minimum accuracy floating-point constants are stored at.
  * [`{$packEnum}` or `{$Z}`](</index.php?title=$packEnum&action=edit&redlink=1> "$packEnum \(page does not exist\)") enables packing of enumeration types.
  * [`{$packRecords}`](</index.php?title=$packRecords&action=edit&redlink=1> "$packRecords \(page does not exist\)") determines alignment of [records](<Record.md> "Record") in memory.
  * [`{$packSet}`](</index.php?title=$packSet&action=edit&redlink=1> "$packSet \(page does not exist\)") determines packing of [sets](<Set.md> "Set").



## Code generation

  * [`{$boolEval}` and `{$B}`](<$boolEval.md> "$boolEval") controls short-cut evaluation of Boolean expressions
  * [`{$assertions}`](<$Assertions.md> "$Assertions") or `{$C}` control, whether [`assert` statements](<assert.md> "assert") are compiled into the [executable program](<Executable_program.md> "Executable program")
  * [`{$calling}`](</index.php?title=$calling&action=edit&redlink=1> "$calling \(page does not exist\)") determines the calling conventions for routines.
  * [`{$checkPointer}`](</index.php?title=$checkPointer&action=edit&redlink=1> "$checkPointer \(page does not exist\)") in conjunction with [`‑gh`](<heaptrc.md> "heaptrc") inserts checks ascertain validity of [pointers](<Pointer.md> "Pointer").
  * [`{$FPUType}`](</index.php?title=$FPUtype&action=edit&redlink=1> "$FPUtype \(page does not exist\)") compiles according to FPU type.
  * [`{$ieeeErrors}`](</index.php?title=$ieeeERRORS&action=edit&redlink=1> "$ieeeERRORS \(page does not exist\)") turns on IEEE error checking for floating-point constants.
  * [`{$implicitExceptions}`](</index.php?title=$implicitExceptions&action=edit&redlink=1> "$implicitExceptions \(page does not exist\)") controls insertion of implicit exceptions which aid prevention of memory leaks.
  * [`{$interfaces}`](</index.php?title=$interfaces&action=edit&redlink=1> "$interfaces \(page does not exist\)")
  * [`{$IOChecks}` or `{$I}`](</index.php?title=$IOChecks&action=edit&redlink=1> "$IOChecks \(page does not exist\)") enables checks of input/output.
  * [`{$objectChecks}`](</index.php?title=$objectChecks&action=edit&redlink=1> "$objectChecks \(page does not exist\)") inserts code ensuring [`self`](<Self.md> "Self") is non-[`nil`](<Nil.md> "Nil")
  * [`{$optimization}`](</index.php?title=$optimization&action=edit&redlink=1> "$optimization \(page does not exist\)") switches on certain optimizations.
  * [`{$overflowChecks}` or `{$Q}`](</index.php?title=$overflowChecks&action=edit&redlink=1> "$overflowChecks \(page does not exist\)") determines if overflow checks are inserted after arithmetic operations. In `{$mode MacPas}` the directive `{$OV}` is available, too.
  * [`{$rangeChecks}` or `{$R}`](<$rangeChecks.md> "$rangeChecks") determines insertion of code ensuring a value is within the permitted range.
  * [`{$S}`](</index.php?title=$S&action=edit&redlink=1> "$S \(page does not exist\)") creates code to check for stack overflows
  * [`{$stackFrames}` or `{$W}`](</index.php?title=$stackFrames&action=edit&redlink=1> "$stackFrames \(page does not exist\)") determines conditions for the creation of stack frames.



### $R versus $Q

There is the completely artificial distinction between the meaning of "overflow" and "range check" errors as defined by Borland. They are basically exactly the same error, except that one is detected by checking the overflow flag of the CPU and the other by explicitly comparing ranges. 

On 64 bit CPUs, integer arithmetic is performed using 64 bit integers because of Pascal's convention to evaluate expressions using the native signed integer type. As a result, overflows cannot occur there when performing 32 bit arithmetic and you will get a range error instead when assigning the result to a 32 bit variable. Ideally, there would be only a single switch that governs both range/overflow checking, but because of historical reasons (as explained above) there are two. 

## Platform-specific

### For x86 processors only

  * [`{$asmMode}`](</index.php?title=$asmMode&action=edit&redlink=1> "$asmMode \(page does not exist\)") determines the syntax the assembler reader expects. Previously, this has been the `{$i386…}` directives.
  * [`{$MMX}`](</index.php?title=$MMX&action=edit&redlink=1> "$MMX \(page does not exist\)") enables optimizations for MMX processors.
  * [`{$safeFPUExceptions}`](</index.php?title=$safeFPUExceptions&action=edit&redlink=1> "$safeFPUExceptions \(page does not exist\)"), whether `fwait` instructions are inserted
  * [`{$saturation}`](</index.php?title=$saturation&action=edit&redlink=1> "$saturation \(page does not exist\)") (in conjunction with `{$MMX}`) enables saturation operations.
  * [`{$maxFPUregisters}`](</index.php?title=$maxFPUregisters&action=edit&redlink=1> "$maxFPUregisters \(page does not exist\)") determines the maximum number of floating-points registers to use.



### Other

  * [`{$linkFramework}`](</index.php?title=$linkFramework&action=edit&redlink=1> "$linkFramework \(page does not exist\)") inserts a framework. This directive is only available on Darwin-based operating systems.



## Data inclusion

  * [`{$link}` or `{$L}`](</index.php?title=$link&action=edit&redlink=1> "$link \(page does not exist\)") inserts an object file during linking.
  * [`{$linkLib}`](</index.php?title=$linkLib&action=edit&redlink=1> "$linkLib \(page does not exist\)") inserts a library during linking.
  * [`{$typeInfo}` or `{$M}`](</index.php?title=$typeInfo&action=edit&redlink=1> "$typeInfo \(page does not exist\)") creates [run-time type information](<Runtime_Type_Information_\(RTTI\).md> "Runtime Type Information \(RTTI\)").
  * [`{$resource}` or `{$R}`](</index.php?title=$resource&action=edit&redlink=1> "$resource \(page does not exist\)") inserts a resource file.



## Compile-time context

  * [`{$define}`](</index.php?title=$define&action=edit&redlink=1> "$define \(page does not exist\)") defines a symbol. In `{$mode MacPas}` the directive `{$defineC}` is considered, too.
  * [`{$include}` or `{$I}`](<$include.md> "$include") reads a file as source or includes certain compile-time/compiler information.
  * [`{$push}` and `{$pop}`](</index.php?title=$push_and_$pop&action=edit&redlink=1> "$push and $pop \(page does not exist\)") store and restore the compiler settings.
  * `{$setC}` sets a compile-time variable, if the current mode allows it.
  * `{$undef}` dismisses the definition of a previously defined symbol. In `{$mode MacPas}` the directive `{$undefC}` is recognized, too.



## Conditional compilation

[Conditional compilation](<Conditional_compilation.md> "Conditional compilation") can be achieved via the directives 

  * `{$if}`
  * `{$else}`
  * `{$elseIf}`
  * `{$endIf}`
  * `{$ifDef}`
  * `{$ifNDef}`
  * `{$ifOpt}`



Additionally, in `{$mode MacPas}` the directives 

  * `{$ifC}`,
  * `{$elseC}`,
  * `{$elIfC}`, and
  * `{$endC}`



are allowed, too. 

## Compile-time behavior

With `{$wait}`, the compiler waits till the user hits ↵ Enter, and then resumes compilation. 

Self-defined messages can be triggered with the directives: 

  * [`{$message}`](<$message.md> "$message"), and the shortcuts 
    * `{$stop}`, which also aborts compilation
    * `{$fatal}`, which also [aborts compilation](<compile-time_error.md> "compile-time error")
    * `{$error}` (in [`{$mode MacPas}`](<Mode_MacPas.md> "Mode MacPas") the directive `{$errorC}` is accepted, too)
    * `{$warning}`
    * `{$hint}`
    * `{$note}`
  * `{$info}`



Emission of messages can be controlled via the directives: 

  * [`{$warn}`](<$warn.md> "$warn") for specific warnings, or
  * all messages of one kind in one go: 
    * `{$warnings}`
    * `{$hints}`
    * `{$notes}`



## Ignored

Since [FPC](<FPC.md> "FPC") intends to be sort of compatible to some other compilers, some very common compiler directives stemming from the non-FPC-lands are recognized – not generating an illegal directive error – and ignored. Those are: 

  * `{$F}` (far or near functions)
  * `{$extendedSym}`
  * `{$externalSym}`
  * `{$hppEmit}`
  * `{$libExport}`
  * `{$noDefine}`
  * `{$region}` and `{$endRegion}`
  * `{$stringChecks}` which in Delphi, this would control the generation of code that checks the sanity of string variables and arguments.



## Historical

Following directives were recognized in earlier versions of FPC and are now illegal: 

  * `{$output_format}` determined the output format of an object file.



## See also

  * [Pascal basics](<Pascal_basics.md> "Pascal basics")
  * [§ “Local directives” in the _Free Pascal programmer’s guide_](<https://www.freepascal.org/docs-html/prog/progse2.html>)

Directives, definitions and conditionals definitions   
---  
[global compiler directives](<global_compiler_directives.md> "global compiler directives") • local compiler directives  
[Conditional Compiler Options](<Conditional_Compiler_Options.md> "Conditional Compiler Options") • [Conditional compilation](<Conditional_compilation.md> "Conditional compilation") • [Macros and Conditionals](<Macros_and_Conditionals.md> "Macros and Conditionals") • [Platform defines](<Platform_defines.md> "Platform defines")  
[$IF](<$IF.md> "$IF")  
  
  
****

---

_Source: [https://wiki.freepascal.org/local_compiler_directives](https://web.archive.org/web/20241107055702/https://wiki.freepascal.org/local_compiler_directives)_
