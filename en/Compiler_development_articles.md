# Compiler development articles

│ **English (en)** │  **[Bahasa Indonesia (id)](</Compiler_development_articles/id> "Compiler development articles/id")** │    
****

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** _As of 2021-May-21. the current compiler version is 3.2.2, so it is possible some of the details listed below may have changed._

The current FPC version under developement is 2.5.1. Compiler internals are documented at the evolving [FPC internals](<FPC_internals.md> "FPC internals") page. 

## Contents

  * 1 General
    * 1.1 Enable new style smartlinking (FreeBSD, Linux)
    * 1.2 C++ linking
  * 2 Processor dependent
    * 2.1 i386
    * 2.2 PowerPC
    * 2.3 Internal Linker
  * 3 Packages



## General

[How to start](<How_to_start.md> "How to start")

[FPC internals](<FPC_internals.md> "FPC internals")

[Language related articles](<Language_related_articles.md> "Language related articles")

[Coding style](<Coding_style.md> "Coding style")

[Porting Free Pascal](<Porting_Free_Pascal.md> "Porting Free Pascal")

[Testing FPC](<Testing_FPC.md> "Testing FPC")

### Enable new style smartlinking (FreeBSD, Linux)

  * in i_*.pas add tf_smartlink_sections to the flags field of the platform description record for your OS/CPU combo (in my case i_bsd.pas, the system_i386_freebsd_info record)
  * in ogelf.pas scroll down to nearly the bottom, and add af_smartlink_sections to the flags field of as_i386_elf32_info
  * run "(g)make all OPT='-Aas -k--gc-sections' at the toplevel to build using new smartlinking routines.



### C++ linking

Note that FPC doesn't have negative VMT offsets due to GNU LD limitations. 

The fundament of above C++ <-> OPas lies in COM interfaces: (quote from Rudy Velthuis) 

"Well, all compilers must be able to create COM interfaces. Since in C++, these are simply pure abstract classes, and Delphi must have the same layout, for many reasons, they are all compatible. In fact, a pure abstract class pointer points to a structure of which the first entry is a VMT. This is also how interfaces are built" 

## Processor dependent

See also [platform list](<Platform_list.md> "Platform list"). 

[Passing Pascal Types to C Routines](<Passing_Pascal_Types_to_C_Routines.md> "Passing Pascal Types to C Routines")

### i386

[PIC information](<PIC_information.md> "PIC information")

### PowerPC

[PPC Calling conventions](<PPC_Calling_conventions.md> "PPC Calling conventions")

### Internal Linker

[Internal Linker](<Internal_Linker.md> "Internal Linker")

## Packages

[Packages](<packages.md> "packages")

---

_Source: [https://wiki.freepascal.org/Compiler_development_articles](https://web.archive.org/web/20251226005029/https://wiki.freepascal.org/Compiler_development_articles)_
