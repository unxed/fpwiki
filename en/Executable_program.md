# Executable program

│ **English (en)** │  **[русский (ru)](<../ru/Executable_program.md>)** │

An **executable program** is the translation of (in our case, [Pascal](<Pascal.md> "Pascal")) [source code](<Source_code.md> "Source code") by a [compiler](<Compiler.md> "Compiler") (or [assembly language](<Assembly_language.md> "Assembly language") source code which is translated by an [assembler](<Assembler.md> "Assembler")) into an [object module](<Object_module.md> "Object module"), which has been combined with any necessary Pascal [units](<Unit.md> "Unit"), the Pascal [run time library](<RTL.md> "RTL") and any other object modules which may have been written by others, to produce an actual [binary](<Binary.md> "Binary") program which can be 

  * run directly by the operating system as an [application](<Application.md> "Application"),
  * used by the [operating system](<operating_system.md> "operating system"), such as a device driver, or
  * become part of the operating system itself.



## see also

  * [`rtl/linux/x86_64/si_prc.inc`](<https://gitlab.com/freepascal.org/fpc/source/-/tree/release_3_0_4/rtl/linux/x86_64/si_prc.inc>) for some startup and stop code

---

_Source: [https://wiki.freepascal.org/Executable_program](https://web.archive.org/web/20241004012855/https://wiki.freepascal.org/Executable_program)_
