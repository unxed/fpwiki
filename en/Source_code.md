# Source code

│ **[Deutsch (de)](</Source_code/de> "Source code/de")** │  **English (en)** │  **[español (es)](</Source_code/es> "Source code/es")** │  **[suomi (fi)](</Source_code/fi> "Source code/fi")** │  **[Bahasa Indonesia (id)](</Source_code/id> "Source code/id")** │    
****  
  
**Source Code** is the file or group of [text](<Text.md> "Text") files which are processed by a [compiler](<Compiler.md> "Compiler") or an [assembler](<Assembler.md> "Assembler") and translated into either an [executable program](<Executable_program.md> "Executable program") or an [object module](<Object_module.md> "Object module"), or into another source [file](</File> "File") for subsequent translation into an executable program or an object module by another compiler or an assembler. 

For the purposes of this system, generally source code is written in [Pascal](<Pascal.md> "Pascal"), and is processed by the FPC Pascal [Compiler](<Compiler.md> "Compiler"), to produce assembly language source code which is then passed to the assembler, which then produces the executable program. 

The FPC Pascal compiler does not directly produce an executable program; instead it translates Pascal code into [assembly language](<Assembly_language.md> "Assembly language"), then transfers control to the [assembler](<Assembler.md> "Assembler") to translate the generated assembly code into an executable program. This allows the compiler to be relatively similar for all target machines, it does not have to know the object file formats and [binary](<Binary.md> "Binary") file write routines for every target system, it simply has to know how to generate assembly code. 

If this compiler were for another programming language such as Basic, C or [Fortran](<Fortran.md> "Fortran"), source code would be written in that language rather than in Pascal.

---

_Source: [https://wiki.freepascal.org/Source_code](https://web.archive.org/web/20240920204134/https://wiki.freepascal.org/Source_code)_
