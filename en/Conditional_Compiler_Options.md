# Conditional Compiler Options

│ **English (en)** │

## Overview

Since Lazarus 0.9.29 you can define build modes and conditional compiler options. These options depend on current target OS, CPU, widgetset and project's build mode. 

For conditionals see here [Macros and Conditionals](<Macros_and_Conditionals.md> "Macros and Conditionals"). 

You can set the build macros for a package via [package editor / Compiler Options / Build Macros](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options"). 

You can set the build macros for a project via [Project / Project Options / Compiler Options / Build Macros](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options"). 

## Target specific units - Doable via FPC directives, no Lazarus extras needed

Concrete situation: LCL extensions package has a unit (OleUtils) that implements TOLEStream. It only makes sense in Windows. Currently the unit is added in all widgetsets but all code is wrapped around a ifdef Windows define so is seen as a dummy unit in other widgetsets. 

Notes: 

  * All units should be added to the package, independent if they are used. If a unit should not always be compiled (not added to the uses section of the package), just unset the 'uses unit' checkbox and add for example {$IFDEF win32}uses oleutils{$ENDIF} to a package unit.
  * You can put units and include files specific to targets into sub directories as demonstrated in the Lazarus and FPC sources (units/$(TargetOS)).
  * Since 0.9.31 multiple files with the same name can be added to packages.



+=What will not be implemented== 

  * A global Debug/Verbose buildmode. Almost no one wants to increase verbosity of all packages, nor debug all packages. But you can define one for your own packages.
  * Conditional units. This is already flexible doable via FPC directives and IDE macros.



## Future development / Wishes

  * Use a package only under one target, e.g. only under Windows and OS X, but not under Linux or BSD.

Directives, definitions and conditionals definitions   
---  
[global compiler directives](<global_compiler_directives.md> "global compiler directives") • [local compiler directives](<local_compiler_directives.md> "local compiler directives")  
Conditional Compiler Options • [Conditional compilation](<Conditional_compilation.md> "Conditional compilation") • [Macros and Conditionals](<Macros_and_Conditionals.md> "Macros and Conditionals") • [Platform defines](<Platform_defines.md> "Platform defines")  
[$IF](<$IF.md> "$IF")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Conditional_Compiler_Options](https://web.archive.org/web/20190920182919/https://wiki.freepascal.org/Conditional_Compiler_Options)_
