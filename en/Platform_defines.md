# Platform defines

│ **English (en)** │

## Contents

  * 1 Useful tips before using platform defines
  * 2 Architecture defines
  * 3 Operating system defines
  * 4 Notes
  * 5 See also



## Useful tips before using platform defines

  * [Marco van de Voort: Development Tutorial (a.k.a Build FAQ)](<https://www.stack.nl/~marcov/buildfaq.pdf>) – general building guidelines
  * [Marco van de Voort: Creating a portable Pascal codebase.](<https://www.stack.nl/~marcov/porting.pdf>) – general porting guidelines
  * if you need to cast a [pointer](<Pointer.md> "Pointer") to an integer for arithmetic or you want to provide a spare field (like [`tag`](<https://www.freepascal.org/docs-html/rtl/classes/tcomponent.tag.html>) in [`TComponent`](<https://www.freepascal.org/docs-html/rtl/classes/tcomponent.html>)) that may be used to store a pointer, use [`PtrInt`](<https://www.freepascal.org/docs-html/rtl/system/ptrint.html>) as signed type, or better [`PtrUInt`](<https://www.freepascal.org/docs-html/rtl/system/ptruint.html>) as unsigned type.



## Architecture defines

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This is a copy of the official [FPC documentation](<http://www.freepascal.org/docs-html/prog/progap7.html#x333-348000G>) which is the authorative version for the stable FPC version.

define(s) | meaning   
---|---  
General   
ENDIAN_BIG | big endian   
ENDIAN_LITTLE | little endian   
CPU16 | 16 bit CPU   
CPU32 | 32 bit CPU   
CPU64 | 64 bit CPU   
Specific   
CPU86 | Intel 8086 and compatible (original PC)   
CPU87 | Intel 8086 and compatible (original PC)   
CPU386, CPUi386 | 386 and compatible (32 bit PC)   
CPUAMD64, CPUX86_64 | AMD64 or Intel 64-bit processor   
CPUIA64 | Intel itanium 64-bit processor   
CPU68 | Motorola 680x0 or compatible CPU   
CPU68K, CPUM68K | Motorola 680x0 or compatible CPU   
CPUM68020 | Motorola 68020 or compatible CPU   
CPUPOWERPC | [PowerPC processor](<PPC_Calling_conventions.md> "PPC Calling conventions") (32 or 64 bit)   
CPUPOWERPC32 | PowerPC processor (32 bit)   
CPUPOWERPC64 | PowerPC processor (64 bit)   
CPUSPARC, CPUSPARC32 | SPARC v7 or compatible   
CPUARM | ARM 32 bit processor   
CPUAARCH64 | ARM 64 bit processor   
CPUAVR | AVR 8/16-bit processor   
FPU   
FPUSOFT | Software emulation of FPU (all types)   
FPUSSE64 | SSE64 FPU on Intel I386 and higher, AMD64.   
FPUSSE | SSE instructions on Intel I386 and higher.   
FPUSSE2 | SSE 2 instructions on Intel I386 and higher.   
FPUSSE3 | SSE 3 instructions on Intel I386 and higher, AMD64.   
FPULIBGCC | GCC library FPU emulation on ARM and M68K.   
FPU68881 | 68881 on M68K.   
FPUFPA | FPA on ARM.   
FPUFPA10 | FPA 10 on ARM.   
FPUFPA11 | FPA 11 on ARM.   
FPUVFP | VFP on ARM.   
FPUX87 | X87 FPU on Intel I386 and higher.   
FPUITANIUM | On Intel Itanium.   
FPUSTANDARD | On PowerPC (32/64 bit).   
FPUHARD | On Sparc.   
  
## Operating system defines

In the following tree, you have to follow leaves to their respective roots, in order to determine all defines. To check this information, refer to files in FPC sources: "compiler/systems/i_*.pas". 

  * `ATARI`
  * `DPMI`: MS-DOS 
    * `GO32V2`
    * `WATCOM`
  * `EMACS`
  * `embedded`
  * `HASAMIGA`
    * `AMIGA`
      * `AMIGA68K`
      * `AMIGAOS4`
    * `AROS`
    * `MorphOS`
  * `GBA`: GameBoy Advance
  * `Java`
  * `MACOS`: Classic Macintosh (System 7 etc.)
  * `MSDOS` MS-DOS 16-bit real mode (defined since FPC version 2.7.1)
  * `NATIVENT`: [NativeNT](<Target_NativeNT.md> "Target NativeNT")
  * `NDS`: Nintendo DS
  * `NETWARE`: NetWare 
    * `NETWARE_CLIB`: NetWare with classic libc
    * `NETWLIBC`, `NETWARE_LIBC`: NetWare with modern libc
  * `OS2`: OS/2 
    * `EMX`: emx
  * `PALMOS`: PalmOS and Garnet OS
  * `Symbian`: Symbian OS
  * `UNIX`
    * `AIX`
    * `ANDROID`
    * `BEOS`
      * `HAIKU`
    * `BSD`
      * `DARWIN`: [macOS](<Portal_Mac.md> "Portal:Mac") and [iOS](<Portal_iOS.md> "Portal:iOS")
        * `iPhoneSim`
      * `DragonFly`: DragonFlyBSD
      * `FREEBSD`: FreeBSD
      * `NETBSD`: NetBSD
      * `OPENBSD`: OpenBSD
    * `LINUX`
    * `SUNOS`
      * `SOLARIS`
    * `QNX`
  * `WDOSX`
  * `Wii`
  * `WINDOWS`: all [Windows](<Portal_Windows.md> "Portal:Windows")
    * `MSWINDOWS`
      * `WIN16`: 16-bit Windows 3.x
      * `WIN32`: 32-bit Windows
      * `WIN64`: 64-bit Windows
    * `WINCE`, `UNDER_CE`, `UNICODE`: Windows CE, Windows mobile



## Notes

`UNICODE` was defined till FPC 3.0.0, see [User changes 3.0 § “define UNICODE was changed to FPC_OS_UNICODE”](<User_Changes_3.md> "User Changes 3.0"). 

## See also

  * [Delphi Wikia article “FreePascal detection and versioning”](<https://delphi.wikia.com/wiki/FreePascal_detection_and_versioning>)
  * [Free Pascal Documentation: Appendix G: Compiler defines during compilation](<https://www.freepascal.org/docs-html/prog/progap7.html>)

Directives, definitions and conditionals definitions   
---  
[global compiler directives](<global_compiler_directives.md> "global compiler directives") • [local compiler directives](<local_compiler_directives.md> "local compiler directives")  
[Conditional Compiler Options](<Conditional_Compiler_Options.md> "Conditional Compiler Options") • [Conditional compilation](<Conditional_compilation.md> "Conditional compilation") • [Macros and Conditionals](<Macros_and_Conditionals.md> "Macros and Conditionals") • Platform defines  
[$IF](<$IF.md> "$IF")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Platform_defines](https://web.archive.org/web/20250117072216/https://wiki.freepascal.org/Platform_defines)_
