# Assembly language

│ **[Deutsch (de)](</Assembly_language/de> "Assembly language/de")** │  **English (en)** │  **[español (es)](</Assembly_language/es> "Assembly language/es")** │  **[suomi (fi)](</Assembly_language/fi> "Assembly language/fi")** │    
****

Assembly language is the [source code](<Source_code.md> "Source code") which is written to be translated by an [assembler](<Assembler.md> "Assembler") into the binary executable program which is then run to produce the desired results. 

Programs written in assembly are of three types: 

  * Directly written assembly language files created by someone.
  * [Inline assembly language](<Asm.md> "Asm") which is included as part of a Pascal source code file which was created by someone.
  * Assembly language output of a compiler, e.g. the FPC Pascal Compiler which was automatically generated from the Pascal source code supplied to the [compiler](<Compiler.md> "Compiler").



Note that directly written/inline assembly code often only works on one specific processor type/family (e.g. Intel i386) or even processor/OS combination (amd64/Windows x64 and amd64/Linux x64) and is therefore not as portable as Pascal code. 

However, hand written optimized assembler code may often be faster than machine/compiler-generated assembler code and therefore it is used in tight performance-critical program code at the expense of portability and maintainability.

---

_Source: [https://wiki.freepascal.org/Assembly_language](https://web.archive.org/web/20250323151215/https://wiki.freepascal.org/Assembly_language)_
