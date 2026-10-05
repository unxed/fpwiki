# Virtual Pascal

│ **[Deutsch (de)](</Virtual_Pascal/de> "Virtual Pascal/de")** │  **English (en)** │  **[français (fr)](</Virtual_Pascal/fr> "Virtual Pascal/fr")** │    
****  
  


## Overview

Virtual Pascal is a 32-bit compiler for DOS, Windows and OS/2 that later got retargeted to Linux/x86 using a pe2elf postprocessor. Its dialect is a subset of older Delphi's and FPC 1.x. VP goes a bit further in its support for TP's DOS and 16 bit specific behaviour, and under certain circumstances existing TurboPascal code requires less modifications. 

The main strengths of VP are the very TP compatible textmode IDE and its OS/2 support. One of the other main strengths, stability is also its main weakness: it is fairly static, and hasn't changed significantly since at least 2005. 

VP was declared EOL in 2005, and a brief revival attempt failed. The site is gone (but can still be found using wayback), but the owner set up a community at ning.com. (see links section) 

## Using FPC/Delphi code under VP

Most notably missing (compared to FPC 1.x) are int64 and overloading support. The way external procedures are declared isn't entirely compatible with Delphi either. Modifiers like "stdcall" and the "external 'dllname.dll' name 'symbol' syntax are not supported, which makes porting headers hard. 

At a certain point is was attempted to update VP's partially closed source sysutils with FPC's, but the number of modifications needed was quite high, and VPs revival was short lived. 

## See also

  * [VP community](<http://vpascal.ning.com>)

Various [Pascal](<Pascal.md> "Pascal") [Compilers](<Compiler.md> "Compiler"):  [AAEC Pascal](</index.php?title=AAEC_Pascal&action=edit&redlink=1> "AAEC Pascal \(page does not exist\)") | [Alice Pascal](<Alice_Pascal.md> "Alice Pascal") | [Apple Pascal](<Apple_Pascal.md> "Apple Pascal") | [Borland Pascal](<Borland_Pascal.md> "Borland Pascal") | [Clascal](<Clascal.md> "Clascal") | [Delphi](<Delphi.md> "Delphi") | [Free Pascal Compiler (FPC)](<FPC.md> "FPC") | [GNU Pascal](<GNU_Pascal.md> "GNU Pascal") | [Kylix](<Kylix.md> "Kylix") | [Lisa Pascal](<Lisa_Pascal.md> "Lisa Pascal") | [Mac Pascal](<Mac_Pascal.md> "Mac Pascal") | [Metrowerks Pascal](</index.php?title=Metrowerks_Pascal&action=edit&redlink=1> "Metrowerks Pascal \(page does not exist\)") | [NBS Pascal](<NBS_Pascal.md> "NBS Pascal") | [OMSI Pascal](</index.php?title=OMSI_Pascal&action=edit&redlink=1> "OMSI Pascal \(page does not exist\)") | [PascalABC.net](<PascalABC.md> "PascalABC.net") | [P32](</index.php?title=P32&action=edit&redlink=1> "P32 \(page does not exist\)") | [Sibyl](<Sibyl.md> "Sibyl") | [Smart Pascal](</index.php?title=Smart_Pascal&action=edit&redlink=1> "Smart Pascal \(page does not exist\)") | [Stanford Pascal Compiler](<Stanford_Pascal_Compiler.md> "Stanford Pascal Compiler") | [Swedish Pascal](</index.php?title=Swedish_Pascal&action=edit&redlink=1> "Swedish Pascal \(page does not exist\)") | [THINK Pascal](<THINK_Pascal.md> "THINK Pascal") | [Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal") | [UCSD Pascal](<UCSD_Pascal.md> "UCSD Pascal") | [VAX Pascal](</index.php?title=VAX_Pascal&action=edit&redlink=1> "VAX Pascal \(page does not exist\)") | Virtual Pascal | [winsoft PocketStudio](<winsoft_PocketStudio.md> "winsoft PocketStudio")  
---  
An extensive list of compilers was maintained at [Pascaland (Internet Archive Version)](<https://web.archive.org/web/20230529010938/https://www.pascaland.org/pascall.htm>) up to January 2018.   
  
  
****

---

_Source: [https://wiki.freepascal.org/Virtual_Pascal](https://web.archive.org/web/20240811052731/https://wiki.freepascal.org/Virtual_Pascal)_
