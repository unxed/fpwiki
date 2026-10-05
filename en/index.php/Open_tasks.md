# Open tasks

│ **English (en)** │    
****

Here is a list of open tasks in FPC. 

There is a special page about the [Systems 2005](<../Systems_2005.md> "Systems 2005") fair in Munich, October 2005, regarding topics discussed between the FPC and Lazarus core developers. 

## Contents

  * 1 Compiler development
  * 2 RTL development
  * 3 FCL development
  * 4 FVISION development
  * 5 IDE development
  * 6 Demoes
  * 7 Misc



## Compiler development

  * Document the compiler
  * SSA framework
  * Instruction scheduler
  * Rework exception handling to "gcc style"
  * ~~Port the m68k code generator to 2.3.x:[M68k port](</index.php?title=M68k_port&action=edit&redlink=1> "M68k port \(page does not exist\)")~~
  * ~~Create a MIPS code generator for 2.3.x:[MIPS port](<../MIPS_port.md> "MIPS port")~~
  * For missing language constructs, see: [Language related articles](<../Language_related_articles.md> "Language related articles")
  * Package support and dynamic library handling in general (partially language, but has a lot of implications)
  * (more) DWARF debugformat support. This might cut back the size of binaries with debuginfo
  * Auto inlining
  * improve PowerPC port: 
    * improve AIX abi-compatibility (e.g., we don't return records as the AIX abi prescribes)
    * improve the PPC-optimizer (only a peephole optimizer available)
  * improve ARM port: 
    * create a ARM-optimizer
    * optimize helper routines some can use libgcc if linked against libc
    * optimize concatcopy code generation
    * optimize set operations
  * improve SPARC port 
    * write code optimizer
    * improve RTL implementation
  * provide [localization](<../Localization.md> "Localization") to other languages



Information about compiler development can be found here: [Compiler development articles](<../Compiler_development_articles.md> "Compiler development articles")

## RTL development

  * ~~port the 1.0.10 BeOS port to 2.1.x:[BeOS port](<../BeOS_port.md> "BeOS port")~~
  * ~~port the 1.0.6/1.0.10 SunOS port to 2.1.x:[SunOS port](</index.php?title=SunOS_port&action=edit&redlink=1> "SunOS port \(page does not exist\)") (combine with Sparc port)~~
  * port the 1.0.6/1.0.10 QNX port to 2.3.x: [QNX port](</index.php?title=QNX_port&action=edit&redlink=1> "QNX port \(page does not exist\)")
  * Implement crossplatform 64-bit fileroutines
  * ~~ARM port and RTL more userfriendly. (need users/contributors/betatesters first)~~
  * improve Mac OS X rtl: 
    * Port the missing RTL units (mainly text console handling)
    * Improve support for the Mac Pascal dialect
    * Finish Classic Mac OS support
  * ~~create a WinCE rtl:[WinCE port](<../WinCE_port.md> "WinCE port")~~



## FCL development

  * New classes
  * debug classes
  * more database support and abstraction



## [FVISION](<../FVISION.md> "FVISION") development

  * getting it fully up to speed and compatible with TV
  * endianness and 64-bit clean work.



## IDE development

  * The text mode IDE needs currently a maintainer, have a look at this page for more information: [Textmode IDE development](<../Textmode_IDE_development.md> "Textmode IDE development")
  * there are a lot of bugs to fix, have a look at the bug repository: <http://www.freepascal.org/bugs.html>
  * ~~get it working with[FVISION](<../FVISION.md> "FVISION")~~
  * Related to this is debugging the platform dependant parts, the former "API" (video, mouse,keyboard). Help from platform maintainers can be expected. (more about isolating bugs than solving in these units) See also [KVM API and Crt future](<../KVM_API_and_Crt_future.md> "KVM API and Crt future")
  * DWARF support for the IDE
  * Colour selection dialogue compatible as available in TP/BP IDE (previously used code was not clean from copyright point of view)



## Demoes

  * we can nearly always use more demoes.



## Misc

  * Packaging system
  * some more tasks are listed on <http://www.freepascal.org/future.html> near the bottom

---

_Source: [https://wiki.freepascal.org/index.php/Open_tasks](https://web.archive.org/web/20250121222259/https://wiki.freepascal.org/index.php/Open_tasks)_
