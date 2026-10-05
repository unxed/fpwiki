# The parser

│ **English (en)** │  **[français (fr)](</The_parser/fr> "The parser/fr")** │    
****

back to contents [FPC internals](<FPC_internals.md> "FPC internals")

## Contents

  * 1 The parser
    * 1.1 Module information
    * 1.2 Parse types
      * 1.2.1 Entry
      * 1.2.2 program or library parsing
      * 1.2.3 unit parsing
      * 1.2.4 routine parsing
      * 1.2.5 label declarations
      * 1.2.6 constant declarations
      * 1.2.7 type declarations
      * 1.2.8 variable declarations
      * 1.2.9 thread variable declarations
      * 1.2.10 resource string declarations
      * 1.2.11 exports declaration
      * 1.2.12 expression parsing
      * 1.2.13 typed constant declarations
    * 1.3 Parser interface
      * 1.3.1 variables
        * 1.3.1.1 AktProcSym
        * 1.3.1.2 LexLevel
        * 1.3.1.3 Current_Module
        * 1.3.1.4 VoidDef
        * 1.3.1.5 cCharDef
        * 1.3.1.6 cWideCharDef
        * 1.3.1.7 BoolDef
        * 1.3.1.8 u8BitDef
        * 1.3.1.9 u16BitDef
        * 1.3.1.10 u32BitDef
        * 1.3.1.11 s32BitDef
        * 1.3.1.12 cu64BitDef
        * 1.3.1.13 cs64BitDef
        * 1.3.1.14 s64FloatDef
        * 1.3.1.15 s32FloatDef
        * 1.3.1.16 s80FloatDef
        * 1.3.1.17 s32FixedDef
        * 1.3.1.18 cShortStringDef
        * 1.3.1.19 cLongStringDef
        * 1.3.1.20 cAnsiStringDef
        * 1.3.1.21 cWideStringDef
        * 1.3.1.22 OpenShortStringDef
        * 1.3.1.23 OpenCharArrayDef
        * 1.3.1.24 VoidPointerDef
        * 1.3.1.25 CharPointerDef
        * 1.3.1.26 VoidFarPointerDef
        * 1.3.1.27 cFormalDef
        * 1.3.1.28 cfFileDef



# The parser

_(last updated for fpc version 1.0.x)_

The task of the parser is to read the token fed by the scanner, and make sure that the pascal syntax is respected. It also populates the symbol table, and creates the intermediate nodes (the tree) which will be used by the code generator. 

An overview of the parsing process, as well as its relationship with the tree the type checker and the code generator is shown in the following diagram: 

<http://www.pjh2.de/fpc/CompilerInternalsFigure08.png>

## Module information

_(last updated for fpc version 1.0.x)_

Each module being compiled, be it a library , unit or main program has some information which is required. This is stored in the tmodule object in memory. To avoid recompilation of already compiled module, the dependencies of the modules is stored in a PPU file, which makes it easier to determine which modules to recompile. 
    
    
    type
      PModule = ^TModule;
      TModule = object(TLinkedList_Item)
        PPUFile: PPPUFile;                        // Pointer to PPU file object (unit file)
        Crc: Longint;                             // CRC-32 bit of the whole PPU file
        Interface_CRC: Longint;                   // CRC-32 bit of the interface part of
                                                  // the PPU file
        Flags: Longint;                           // Unit file flags
        Compiled: Boolean;                        // TRUE if module is already compiled
        Do_Reload: Boolean;                       // TRUE if the PPU file must be reloaded
        Do_Assemble: Boolean;                     // Only assemble, don’t recompile unit
        Sources_Avail: Boolean;                   // TRUE if all sources of module are available
        Sources_Checked: Boolean;                 // TRUE if the sources has already been checked
        Is_Unit: Boolean;                         // TRUE if this is a unit (otherwise a library 
                                                  // or a main program)
        In_Compile: Boolean;                      // module is currently being recompiled
        In_Second_Compile: Boolean;               // module is being compiled for second time
        In_Second_Load: Boolean;                  // module is being reloaded a second time
        In_Implementation: Boolean;               // currently compiling implementation part 
                                                  // (units only)
        In_Global: Boolean;                       // currently compiling implementation part
                                                  // (units only)
        Recompile_Reason: TRecompile_Reason;      // Reason why module should be recompiled
        Islibrary: Boolean;                       // TRUE if this module is a shared library
        Map: PUnitMap;                            // Map of all used units for this unit
        Unitcount: Word;                          // Internal identifier of unit (for GDB support)
        Unit_index: Word;
        Globalsymtable: Pointer;                  // Symbol table for this module of externally
                                                  // visible symbols
        Localsymtable: Pointer;                   // Symbol table for this module of locally
                                                  // visible symbols
        Scanner: Pointer;                         // Scanner object pointer
        Loaded_From: PModule;                     // Module which referred to this module
        Uses_Imports: Boolean;                    // TRUE if this module imports symbols
                                                  // from a shared library
        Imports: PLinkedList;                     // Linked list of imported symbols
        _Exports: PLinkedList;                    // Linked list of exported symbols (libraries only)
        SourceFiles: PFileManager;                // List of all source files for this module
        ResourceFiles: TStringContainer;          // List of all resource files for this module
        Used_Units: TLinkedList;                  // Information on units used by this module 
                                                  // (pused_unit)
        Dependent_Units: TLinkedList;
        LocalUnitSearchPath,                      // Search path for obtaining module source code
        LocalObjectSearchPath,
        LocalIncludeSearchPath,                   // Search path for includes for this module
        LocalLibrarySearchPath: TSearchPathList;
        Path: PString;                            // Path were module is located or created
        OutputPath: PString;                      // Path where object files (unit), 
                                                  // executable (program) or 
                                                  // shared library (library) is created
        ModuleName: PString;                      // Name of the module in uppercase
        ObjFileName: PString;                     // Full name of object file or executable file
        AsmFileName: PString;                     // Full name of the assembler file
        PPUFileName: PString;                     // Full name of the PPU file
        StaticLibFilename: PString;               // Full name of the static library name
                                                  // (used when smart linking is used)
        SharedLibFilename: PString;               // Filename of the output shared library
                                                  // (in the case of a library)
        ExeFileName: PString;                     // Filename of the output executable
                                                  // (in the case of a program)
        AsmPrefix: PString;                       // Filename prefix of output assembler
                                                  // files when using smartlinking
        MainSource: PString;                      // Name of the main source file
      end;
    

## Parse types

_(last updated for fpc version 1.0.x)_

### Entry

### program or library parsing

### unit parsing

### routine parsing

### label declarations

### constant declarations

### type declarations

### variable declarations

### thread variable declarations

### resource string declarations

### exports declaration

### expression parsing

### typed constant declarations

## Parser interface

_(last updated for fpc version 1.0.x)_

### variables

#### AktProcSym

Declaration: | `**var** AktProcSym: PProcSym;`  
---|---  
Description: | Pointer to the symbol information for the routine currently being parsed.   
  
  


#### LexLevel

Declaration: | `**var** LexLevel: Longint;`  
---|---  
Description: | Level of code currently being parsed and compiled  
0 = for main program  
1 = for subroutine  
2 = for local / nested subroutines.   
  
  


#### Current_Module

Declaration: | `**var** Current_Module: PModule;`  
---|---  
Description: | Information on the current module (program, library or unit) being compiled.   
  
  
The following variables are default type definitions which are created each time compilation begins (default system-unit definitions), these definitions should always be valid: 

#### VoidDef

Declaration: | `**var** VoidDef: POrdDef;`  
---|---  
Description: | Pointer to nothing type   
Notes: | This is loaded as a default supported type for the compiler   
  
  


#### cCharDef

Declaration: | `**var** cCharDef: POrdDef;`  
---|---  
Description: | Type definition for a character (char)   
Notes: | This is loaded as a default supported type for the compiler   
  
  


#### cWideCharDef

Declaration: | `**var** cWideCharDef: POrdDef;`  
---|---  
Description: | Type definition for a unicode character (WideChar)   
Notes: | This is loaded as a default supported type for the compiler   
  
  


#### BoolDef

Declaration: | `**var** BoolDef: POrdDef;`  
---|---  
Description: | Type definition for a boolean value (boolean)   
Notes: | This is loaded as a default supported type for the compiler   
  
  


#### u8BitDef

Declaration: | `**var** u8BitDef: POrdDef;`  
---|---  
Description: | Type definition for an 8-nit unsigned value (byte)   
Notes: | This is loaded as a default supported type for the compiler   
  
  


#### u16BitDef

Declaration: | `**var** u16BitDef: POrdDef;`  
---|---  
Description: | Type definition for an unsigned 16-bit value (word)   
Notes: | This is loaded as a default supported type for the compiler   
  
  


#### u32BitDef

Declaration: | `**var** u32BitDef: POrdDef;`  
---|---  
Description: | Type definition for an unsigned 32-bit value (cardinal)   
Notes: | This is loaded as a default supported type for the compiler   
  
  


#### s32BitDef

Declaration: | `**var** s32BitDef: POrdDef;`  
---|---  
Description: | Type definition for a signed 32-bit value (Longint)   
Notes: | This is loaded as a default supported type for the compiler   
  
  


#### cu64BitDef

Declaration: | `**var** cu64BitDef: POrdDef;`  
---|---  
Description: | Type definition for an unsigned 64-bit value (QWord)   
Notes: | This is loaded as a default supported type for the compiler   
  
  


#### cs64BitDef

Declaration: | `**var** cs64BitDef: POrdDef;`  
---|---  
Description: | Type definition for a signed 64-bit value (Int64)   
Notes: | This is loaded as a default supported type for the compiler   
  
  
The following variables are default type definitions which are created each time compilation begins (default system-unit definitions), these definitions should always be valid: 

#### s64FloatDef

Declaration: | `**var** s64FloatDef: PFloatDef;`  
---|---  
Description: | Type definition for a 64-bit IEEE floating point type (double)   
Notes: | This is loaded as a default supported type for the compiler. This might not actually really point to the double type if the cpu does not support it.   
  
  


#### s32FloatDef

Declaration: | `**var** s32FloatDef: PFloatDef;`  
---|---  
Description: | Type definition for a 32-bit IEEE floating point type (single)   
Notes: | This is loaded as a default supported type for the compiler. This might not actually really point to the single type if the cpu does not support it.   
  
  


#### s80FloatDef

Declaration: | `**var** s80FloatDef : PFloatDef;`  
---|---  
Description: | Type definition for an extended floating point type (extended)   
Notes: | This is loaded as a default supported type for the compiler. This might not actually really point to the extended type if the cpu does not support it.   
  
  


#### s32FixedDef

Declaration: | `**var** s32FixedDef: PFloatDef;`  
---|---  
Description: | Type definition for a fixed point 32-bit value (fixed)   
Notes: | This is loaded as a default supported type for the compiler. This is not supported officially in FPC 1.0   
  
  
The following variables are default type definitions which are created each time compilation begins (default system-unit definitions), these definitions should always be valid: 

#### cShortStringDef

Declaration: | `**var** cShortStringDef: PStringDef;`  
---|---  
Description: | Type definition for a short string type (ShortString)   
Notes: | This is loaded as a default supported type for the compiler.   
  
  


#### cLongStringDef

Declaration: | `**var** cLongStringDef: PStringDef;`  
---|---  
Description: | Type definition for a long string type (LongString)   
Notes: | This is loaded as a default supported type for the compiler.   
  
  


#### cAnsiStringDef

Declaration: | `**var** cAnsiStringDef: PStringDef;`  
---|---  
Description: | Type definition for an ansistring type (AnsiString)   
Notes: | This is loaded as a default supported type for the compiler.   
  
  


#### cWideStringDef

Declaration: | `**var** cWideStringDef: PStringDef;`  
---|---  
Description: | Type definition for an wide string type (WideString)   
Notes: | This is loaded as a default supported type for the compiler.   
  
  


#### OpenShortStringDef

Declaration: | `**var** OpenShortStringDef: PStringDef;`  
---|---  
Description: | Type definition for an open string type (OpenString)   
Notes: | This is loaded as a default supported type for the compiler.   
  
  


#### OpenCharArrayDef

Declaration: | `**var** OpenCharArrayDef: PArrayDef;`  
---|---  
Description: | Type definition for an open char array type(OpenCharArray)   
Notes: | This is loaded as a default supported type for the compiler.   
  
  
The following variables are default type definitions which are created each time compilation begins (default system-unit definitions), these definitions should always be valid: 

#### VoidPointerDef

Declaration: | `**var** VoidPointerDef: PPointerDef;`  
---|---  
Description: | Type definition for a pointer which can point to anything (Pointer)   
Notes: | This is loaded as a default supported type for the compiler   
  
  


#### CharPointerDef

Declaration: | `**var** CharPointerDef: PPointerDef;`  
---|---  
Description: | Type definition for a pointer which can point to characters (PChar)   
Notes: | This is loaded as a default supported type for the compiler   
  
  


#### VoidFarPointerDef

Declaration: | `**var** VoidFarPointerDef: PPointerDef;`  
---|---  
Description: | Type definition for a pointer which can point to anything (intra-segment) (far Pointer)   
Notes: | This is loaded as a default supported type for the compiler   
  
  


#### cFormalDef

Declaration: | `**var** cFormalDef: PFormalDef;`  
---|---  
Description: |   
Notes: | This is loaded as a default supported type for the compiler   
  
  


#### cfFileDef

Declaration: | `**var** cfFileDef: PFileDef;`  
---|---  
Description: | This is the default file type (file)   
Notes: | This is loaded as a default supported type for the compiler   
  
  
Next chapter: [The inline assembler parser](<The_inline_assembler_parser.md> "The inline assembler parser")

---

_Source: [https://wiki.freepascal.org/The_parser](https://web.archive.org/web/20231204164237/https://wiki.freepascal.org/The_parser)_
