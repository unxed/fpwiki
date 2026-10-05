# The assembler output

back to contents [FPC internals](<FPC_internals.md> "FPC internals")

## The assembler output

_(last updated for fpc version 1.0.x)_

All code is generated via special linked lists of instructions. The base of this is a special object, an abstract assembler which implements all directives which are usually implemented in the different assemblers available on the market. When the code generator and parser generates the final output, it is generated as a linked list for each of the sections available for the output assembler. Each entry in the linked list is either an instruction, or one of the abstract directives for the assembler. 

<http://www.pjh2.de/fpc/CompilerInternalsFigure11.png>

Assembler generation organisation 

The different possible sections which are output are as follows: 

Internal section name | Description   
---|---  
ExparAsmList | temporary list   
DataSegment | initialized variables   
CodeSegment | instructions and general code directives   
DebugList | debugging information   
WithDebugList | ???????????????   
Consts read | only constants   
ImportSection | imported symbols   
ExportSection | exported symbols   
ResourceSection | Resource data   
RttiList | runtime type information data   
ResourceStringList | resource string data   
  
  
The following directives for the abstract assembler currently exist: 

Abstract assembler node types: 

Node entry Type | Description   
---|---  
Ait_None | This entry in the linked list is invalid (this should normally never occur)   
Ait_Direct | Direct output to the resulting assembler file (as string)   
Ait_String | Shortstring with a predefined length   
Ait_Label | Numbered assembler label used for jumps   
Ait_Comment | Assembler output comment   
Ait_Instruction | Processor specific instruction   
Ait_DataBlock | Unitialized data block (BSS)   
Ait_Symbol | Entry represents a symbol (exported, imported, or other public symbol type) Possible symbol types : NONE, EXTERNAL, LOCAL and GLOBAL eg : A symbol followed by an Ait_const_32bit   
Ait_Symbol_End | Symbol end (for example the end of a routine)   
Ait_Const_32bit | Initialized 32-bit constant (without a symbol)   
Ait_Const_16bit | Initialized 16-bit constant (without a symbol)   
Ait_Const_8bit | Initialized 8-bit constant (without a symbol)   
Ait_Const_symbol | ????????????   
Ait_Real_80bit | (x86) Initialized 80-bit floating point constant (without symbol)   
Ait_Real_64bit | Initialized Double IEEE floating point constant (without symbol)   
Ait_Real_32bit | Initialized Single IEEE floating point constant (without symbol)   
Ait_Comp_64bit | (x86) Initialized 64-bit floating point integer (without symbol)   
Ait_Align Alignment | directive   
Ait_Section Section | directive   
Ait_const_rva | (Win32)   
Ait_Stabn | stabs debugging information (numerical value)   
Ait_Stabs | stabs debugging information (string)   
Ait_Force_Line stabs | debugging line information   
Ait_Stab_Function_Name | stabs debug information routine name   
Ait_Cut | Cut in the assembler files (used for smartlinking)   
Ait_RegAlloc | Debugging information for the register allocator   
Ait_Marker | ????????????   
Ait_Frame | (Alpha)   
Ait_Ent | (Alpha)   
Ait_Labeled_Instruction | (m68k)   
Ait_Dummy | Unused - should never appear

---

_Source: [https://wiki.freepascal.org/The_assembler_output](https://web.archive.org/web/20211204223650/https://wiki.freepascal.org/The_assembler_output)_
