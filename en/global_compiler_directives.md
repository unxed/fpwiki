# global compiler directives

│ **[Deutsch (de)](</global_compiler_directives/de> "global compiler directives/de")** │  **English (en)** │  **[français (fr)](</global_compiler_directives/fr> "global compiler directives/fr")** │  **[русский (ru)](<../ru/global_compiler_directives.md> "global compiler directives/ru")** │    
****

Free Pascal supports [compiler directives](<Compiler_directive.md> "Compiler directive") in the [source file](<Source_code.md> "Source code"). Basically the same directives as in Turbo Pascal, Delphi and Apple Pascal (Mac OS) pascal compilers are supported. Some are recognized for compatibility only, and have no effect. 

## Contents

  * 1 Syntax
  * 2 Code generation
  * 3 Data inclusion
  * 4 Paths
  * 5 Target-dependent
    * 5.1 Novell NetWare only
    * 5.2 Palm OS and Garnet OS only
    * 5.3 Windows-based systems
    * 5.4 Miscellaneous
  * 6 Compile-time data
  * 7 Ignored
  * 8 See also



## Syntax

General: 

  * [`{$mode}`](</index.php?title=$mode&action=edit&redlink=1> "$mode \(page does not exist\)") selects the compiler mode
  * [`{$modeSwitch}`](</index.php?title=$modeSwitch&action=edit&redlink=1> "$modeSwitch \(page does not exist\)") turns on or off specific mode features



Specific: 

  * [`{$extendedSyntax}`](<$extendedSyntax.md> "$extendedSyntax") enables multiple syntax extensions
  * [`{$pointerMath}`](</index.php?title=$pointerMath&action=edit&redlink=1> "$pointerMath \(page does not exist\)") automatically defines operators for new pointer data types (since [FPC 2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0"))
  * [`{$openStrings}` or `{$P}`](</index.php?title=$openStrings&action=edit&redlink=1> "$openStrings \(page does not exist\)") determines, whether all routine parameters of type `string` are considered to be open string parameters; this parameter only has effect for short strings, not for `ANSIString`s.
  * [`{$varPropSetter}`](</index.php?title=$varPropSetter&action=edit&redlink=1> "$varPropSetter \(page does not exist\)")



## Code generation

  * [`{$codePage}`](<$codePage.md> "$codePage") determines which code page is used by the program
  * [`{$E}`](</index.php?title=$E&action=edit&redlink=1> "$E \(page does not exist\)") emulate co-processor
  * [`{$extension}`](</index.php?title=$extension&action=edit&redlink=1> "$extension \(page does not exist\)") determines file name suffix of the generated [executable](<Executable_program.md> "Executable program")
  * [`{$libPrefix}`](</index.php?title=$libPrefix_and_$libSuffix&action=edit&redlink=1> "$libPrefix and $libSuffix \(page does not exist\)") determines file name prefix a generated library
  * [`{$libSuffix}`](</index.php?title=$libPrefix_and_$libSuffix&action=edit&redlink=1> "$libPrefix and $libSuffix \(page does not exist\)") determines file name suffix a generated library
  * [`{$memory}`](</index.php?title=$memory&action=edit&redlink=1> "$memory \(page does not exist\)") determines size of memory to use
  * [`{$PascalMainName}`](</index.php?title=$pascalMainName&action=edit&redlink=1> "$pascalMainName \(page does not exist\)") determines name of entry point
  * [`{$PIC}`](</index.php?title=$PIC&action=edit&redlink=1> "$PIC \(page does not exist\)") enables position independent code code generation
  * [`{$smartlink}`](</index.php?title=$smartlink&action=edit&redlink=1> "$smartlink \(page does not exist\)") determines smartlinking
  * [`{$sysCalls}`](</index.php?title=$sysCalls&action=edit&redlink=1> "$sysCalls \(page does not exist\)") determines system call calling conventions on Amiga/MorphOS



## Data inclusion

  * [`{$debugInfo}` or `{$D}`](</index.php?title=$debugInfo&action=edit&redlink=1> "$debugInfo \(page does not exist\)") inserts GNU debugging information into generated code
  * [`{$referenceInfo}` or `{$Y}`](</index.php?title=$referenceInfo&action=edit&redlink=1> "$referenceInfo \(page does not exist\)") creates Delphi-compatible browser information (not yet fully supported)



## Paths

  * [`{$frameworkPath}`](</index.php?title=$framework&action=edit&redlink=1> "$framework \(page does not exist\)") (on Darwin)
  * [`{$includePath}`](<$include.md> "$include") determines path for include files
  * [`{$libraryPath}`](</index.php?title=FPC_paths&action=edit&redlink=1> "FPC paths \(page does not exist\)") determines the path to library files
  * [`{$objectPath}`](</index.php?title=FPC_paths&action=edit&redlink=1> "FPC paths \(page does not exist\)") defines the path to search for object files at
  * [`{$unitPath}`](</index.php?title=FPC_paths&action=edit&redlink=1> "FPC paths \(page does not exist\)") determines search path for units



## Target-dependent

### Novell NetWare only

  * [`{$copyright}`](</index.php?title=$copyright&action=edit&redlink=1> "$copyright \(page does not exist\)") inserts copyright information
  * [`{$screenName}`](</index.php?title=$screenName&action=edit&redlink=1> "$screenName \(page does not exist\)") determines screen name of application
  * [`{$threadName}`](</index.php?title=$threadName&action=edit&redlink=1> "$threadName \(page does not exist\)") defines name of thread



### Palm OS and Garnet OS only

  * `{$appID}` defines four-character application identifier
  * `{$appName}` determines the name of the application



### Windows-based systems

  * [`{$imageBase}`](</index.php?title=$imageBase&action=edit&redlink=1> "$imageBase \(page does not exist\)") specifies DLL image base location
  * `{$minStackSize}`
  * `{$maxStackSize}`
  * `{$setPEFlags}`
  * `{$version}` defines version number of a DLL



### Miscellaneous

  * [`{$appType}`](</index.php?title=$appType&action=edit&redlink=1> "$appType \(page does not exist\)") determines the program type



## Compile-time data

  * [`{$profile}`](</index.php?title=$profile&action=edit&redlink=1> "$profile \(page does not exist\)") enables generation of profile code



## Ignored

  * `{$description}`: introduced for compatibility and as of FPC 3.0.4 ignored
  * `{$G}` would generate 80286 code with [TP](<Turbo_Pascal.md> "Turbo Pascal")
  * `{$localSymbols}` or `{$L}`
  * `{$N}` (numeric processing)
  * `{$O}` enabled level 2 optimizations. It is not recognized anymore since FPC 2.0.0. Use [`{$optimization}`](</index.php?title=$optimization&action=edit&redlink=1> "$optimization \(page does not exist\)") instead.
  * `{$weakPackageUnit}`



## See also

  * [Pascal basics](<Pascal_basics.md> "Pascal basics")
  * [§ “global directives” in the _Free Pascal programmer’s guide_](<https://www.freepascal.org/docs-html/prog/progse3.html>)

Directives, definitions and conditionals definitions   
---  
global compiler directives • [local compiler directives](<local_compiler_directives.md> "local compiler directives")  
[Conditional Compiler Options](<Conditional_Compiler_Options.md> "Conditional Compiler Options") • [Conditional compilation](<Conditional_compilation.md> "Conditional compilation") • [Macros and Conditionals](<Macros_and_Conditionals.md> "Macros and Conditionals") • [Platform defines](<Platform_defines.md> "Platform defines")  
[$IF](<$IF.md> "$IF")  
  
  
****

---

_Source: [https://wiki.freepascal.org/global_compiler_directives](https://web.archive.org/web/20230110142433/https://wiki.freepascal.org/global_compiler_directives)_
