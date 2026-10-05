# Platform list

│ **English (en)** │  **[русский (ru)](<../ru/Platform_list.md>)** │

This list presents all processor architectures and operating system platforms supported by [Free Pascal](<Free_Pascal.md> "Free Pascal") (including experimental implementations). 

## Contents

  * 1 Supported architectures
  * 2 Former ports which were removed
  * 3 Supported targets for i386
  * 4 Supported targets for AMD64 (x86-64)
  * 5 Supported targets for i8086
  * 6 Supported targets for ARM
  * 7 Supported targets for AArch64
  * 8 Supported targets for PowerPC
  * 9 Supported targets for PowerPC64
  * 10 Supported targets for m68k
  * 11 Supported targets for SPARC
  * 12 Supported targets for SPARC64
  * 13 Supported targets for MIPS
  * 14 Supported targets for PIC
  * 15 Supported targets for AVR
  * 16 Supported targets for RISC-V
  * 17 Supported targets for Xtensa
  * 18 Supported targets for Z80
  * 19 Supported targets for WebAssembly 32-bit
  * 20 Unofficial 3rd party ports
  * 21 Stalled ports
  * 22 Unlikely to be ported
  * 23 Resources for porting to new platforms...
  * 24 Cross compilation



| Classic home computers | Gaming consoles | Mobile systems | Desktop | Universal systems (desktop, workstation, server etc.) | Embedded | Mainframe | Virtual machines   
---|---|---|---|---|---|---|---|---  
OS  
Processor | ZX Spectrum | MSX | Play Station 1 | GameBoy Advance | Nintendo DS | Nintendo Wii | Android | iOS | Palm OS / Garnet OS | Symbian OS | DOS / Go32 | AmigaOS | AROS | Haiku | MorphOS | TOS | BeOS | FreeBSD | NetBSD | OpenBSD | Solaris | Mac OS Classic | macOS (OS X) | [AIX](<FPC_AIX_Port.md> "FPC AIX Port") | Linux | Win16 | Win32 | Win64 | OS/2 | Netware | FreeRTOS | Embedded | [z/OS](<ZSeries.md> "ZSeries") | WASI | Java   
i386 |  |  |  |  |  |  | + |  |  |  | + |  | + |  |  |  | + | + | + | + | + |  | + |  | + |  | + |  | + | + |  |  |  |  |   
x86-64 |  |  |  |  |  |  |  |  |  |  |  |  | O | + |  |  |  | + | + | + | + |  | + |  | + |  |  | + |  |  |  |  |  |  |   
i8086 |  |  |  |  |  |  |  |  |  |  | + |  |  |  |  |  |  |  |  |  |  |  |  |  |  | + |  |  |  |  |  | + |  |  |   
[ARM](<ARM.md> "ARM") (AArch32) |  |  |  | O | O |  | + | + | O | O |  |  | + |  |  |  |  |  |  |  |  |  |  |  | + |  |  |  |  |  | + | + |  |  |   
ARM (AArch64) |  |  |  |  |  |  |  | + |  |  |  |  |  |  |  |  |  |  |  |  |  |  | + |  | + |  |  | + |  |  |  |  |  |  |   
[PowerPC32](<PowerPC.md> "PowerPC") |  |  |  |  |  | O |  |  |  |  |  | + |  |  | + |  |  |  | + |  |  | + | + | + | + |  |  |  |  |  |  |  |  |  |   
[PowerPC64](<PowerPC64_Port.md> "PowerPC64 Port") |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | + | + | + |  |  |  |  |  |  |  |  |  |   
[m68k](<m68k.md> "m68k") |  |  |  |  |  |  |  |  | + |  |  | + |  |  |  | + |  |  | + |  |  | + |  |  | + |  |  |  |  |  |  |  |  |  |   
SPARC32 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | + |  |  |  | + |  |  |  |  |  |  |  |  |  |   
SPARC64 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | + |  |  |  |  |  |  |  |  |  |   
[MIPS](<MIPS_port.md> "MIPS port") |  |  | + |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | + |  |  |  |  |  |  |  |  |  |   
PIC |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | + |  |  |   
[AVR](<AVR.md> "AVR") |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | + |  |  |   
RISC-V |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | + |  |  |  |  |  |  | + |  |  |   
[Xtensa](<Xtensa.md> "Xtensa") |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | + |  |  |  |  |  | + | + |  |  |   
[Z80](<Z80.md> "Z80") | + | + |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | + |  |  |   
[WebAssembly 32-bit](<WebAssembly.md> "WebAssembly") |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | + |  | + |   
[Z Systems](<ZSeries.md> "ZSeries") |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | O |  |   
[Java bytecode](<FPC_JVM.md> "FPC JVM") |  |  |  |  |  |  | + |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | \+   
  
+: supported; O: under development. 

## Supported architectures

SVN trunk contains support at various levels of completeness for the following architectures: 

  * i386
  * i8086
  * [ARM](<ARM.md> "ARM")
  * ARM64 (AArch64)
  * AMD64 (x86_64)
  * [AVR](<AVR.md> "AVR")
  * LLVM IR
  * [m68k](<m68k.md> "m68k")
  * [MIPS](<MIPS_port.md> "MIPS port")
  * [PowerPC](<PowerPC.md> "PowerPC")
  * [PowerPC64](<PowerPC64_Port.md> "PowerPC64 Port")
  * SPARC
  * SPARC64
  * RISC-V
  * [Z80](<Z80.md> "Z80")
  * [Java bytecode](<FPC_JVM.md> "FPC JVM")
  * [Xtensa](<Xtensa.md> "Xtensa")
  * [WebAssembly](<WebAssembly.md> "WebAssembly")



## Former ports which were removed

  * iA64/Itanium: 
    * Non-compiling compiler, only some basic units for the compiler were implemented
    * Itanium has been officially discontinued as of January 30th, 2019
  * Alpha: 
    * Non-compiling compiler, only some basic units for the compiler were implemented



## Supported targets for i386

  * [Android](<Android.md> "Android") for i386
  * [AROS](<AROS.md> "AROS") for i386
  * [BeOS, Zeta and Haiku](<BeOS_port.md> "BeOS port") for i386
  * [FreeBSD](<FreeBSD.md> "FreeBSD") for i386
  * [GO32V2](<GO32V2.md> "GO32V2") DOS extender
  * Linux for i386
  * [macOS](<Target_Darwin.md> "Target Darwin") for i386
  * NetBSD for i386
  * [Netware](<Netware.md> "Netware") for i386 (clib and libc)
  * OpenBSD for i386
  * [OS/2](<Target_OS2.md> "Target OS2") / eComStation
  * OS/2 and DOS via EMX (currently not completely up to date)
  * [Solaris for i386](<Solaris_Port.md> "Solaris Port")
  * Watcom compatible DOS extenders
  * WDOSX DOS extender
  * [Win32 for i386](<Win32/64_Interface.md> "Win32/64 Interface")



## Supported targets for AMD64 (x86-64)

  * [AROS](<AROS.md> "AROS") for AMD64 (experimental)
  * [FreeBSD for AMD64](<FreeBSD.md> "FreeBSD")
  * OpenBSD for AMD64
  * NetBSD for AMD64
  * DragonFlyBSD for AMD64
  * Solaris for AMD64
  * [Haiku for AMD64](<Installing_Lazarus_on_Haiku.md> "Installing Lazarus on Haiku")
  * [Linux for AMD64](<Linux_for_AMD64.md> "Linux for AMD64")
  * [macOS for AMD64](<Target_Darwin.md> "Target Darwin")
  * [Win64 for AMD64](<Win64_for_AMD64.md> "Win64 for AMD64")



## Supported targets for i8086

  * [DOS](<DOS.md> "DOS")
  * Windows 16 bit
  * Embedded



## Supported targets for ARM

  * [Android](<Android.md> "Android")
  * [AROS](<AROS.md> "AROS")
  * [Embedded](<Embedded.md> "Embedded")
  * FreeRTOS
  * [GameBoy Advance](<GameBoy_Advance.md> "GameBoy Advance") (under development)
  * [Target Darwin](<iPhone/iPod_development.md> "iPhone/iPod development") (iOS) (2.3.x and later)
  * [Linux](<Linux_for_ARM.md> "Linux for ARM")
  * [Native ARM Systems](<Native_ARM_Systems.md> "Native ARM Systems") (not cross-development)
  * [Nintendo DS](<Nintendo_DS.md> "Nintendo DS") (under development)
  * [PalmOS port](<PalmOS_port.md> "PalmOS port") (under development)
  * [SymbianOS](<SymbianOS.md> "SymbianOS") (development abandoned)



## Supported targets for AArch64

  * Linux for AArch64
  * [Target Darwin](<Target_Darwin.md> "Target Darwin") (iOS 64bit, macOS 64 bit)
  * Windows for ARM64



## Supported targets for PowerPC

  * Linux for PowerPC
  * [Darwin](<Target_Darwin.md> "Target Darwin") (Mac OS X)
  * NetBSD (core done, but not kept up to speed)
  * [MacOS](<Target_MacOS.md> "Target MacOS") (classic)
  * [MorphOS](<MorphOS.md> "MorphOS")
  * [AmigaOS 4.x](<AmigaOS.md> "AmigaOS") (maintainerless, but kept in a buildable state)
  * [Nintendo Wii](<Wii.md> "Wii") (under development)



## Supported targets for PowerPC64

  * Linux (2.1.x and later)
  * [Target Darwin](<Target_Darwin.md> "Target Darwin") (Mac OS X) (2.3.x and later)



## Supported targets for m68k

  * [Commodore Amiga](<Amiga.md> "Amiga")
  * [Linux](<Portal_Linux.md> "Portal:Linux") for m68k
  * NetBSD (ELF only)
  * [Atari TOS](<Atari.md> "Atari") (compiler itself works, but it's still in early stage)
  * [MacOS](<Target_MacOS.md> "Target MacOS") (classic, planned)
  * [Palm OS / Garnet OS](<PalmOS_port.md> "PalmOS port") (works, but experimental)
  * [Embedded](<Embedded.md> "Embedded") (planned)



See page [m68k](<m68k.md> "m68k") for details. 

## Supported targets for SPARC

  * [Solaris](<SunOS/ELF.md> "SunOS/ELF") for SPARC (in maintenance mode)
  * Linux for SPARC



## Supported targets for SPARC64

  * Linux for SPARC64



## Supported targets for MIPS

  * Linux for MIPS
  * [PlayStation 1](<PS1.md> "PS1") for MIPSEL



## Supported targets for PIC

  * Embedded



See [MIPSEL](<TARGET_Embedded_Mipsel.md> "TARGET Embedded Mipsel") page for details 

## Supported targets for AVR

  * Embedded



See [AVR](<AVR.md> "AVR") page for details. 

## Supported targets for RISC-V

  * Linux
  * Embedded



Both 32- and 64-bit are supported. 

## Supported targets for Xtensa

  * Linux
  * FreeRTOS
  * Embedded



## Supported targets for Z80

  * ZX Spectrum
  * MSX-DOS
  * Embedded



## Supported targets for WebAssembly 32-bit

  * WASI
  * Embedded



See the [WebAssembly/Compiler](<WebAssembly/Compiler.md> "WebAssembly/Compiler") page for details. 

## Unofficial 3rd party ports

  * [GP2X](<GP2X.md> "GP2X") (under development)


  * [UEFI](<UEFI.md> "UEFI") Unified Extensible Firmware Interface (under early development)


  * [ZSeries](<ZSeries.md> "ZSeries") IBM System/370, S/390 and zSeries mainframes (under development as "i370")



## Stalled ports

  * [GameCube](<GameCube.md> "GameCube")
  * [Xbox](<xbox.md> "xbox")



## Unlikely to be ported

  * [Sanos](<Sanos.md> "Sanos") Win32-compatible console-mode operating system
  * MUSIC/SP OS-compatible IBM mainframe operating system, using EBCDIC. [Qemu and other emulators#MUSIC/SP using Sim/390 or Hercules](<Qemu_and_other_emulators.md> "Qemu and other emulators")



## Resources for porting to new platforms...

... and keeping existing ones up to date. 

  * [FPC HowToDo](<FPC_HowToDo.md> "FPC HowToDo") \- new additions requiring attention of platform maintainers
  * [System unit structure](<System_unit_structure.md> "System unit structure") \- (work in progress - only skeleton finished) description of System unit internals



## Cross compilation

Information about compilation for a different platform as the one running the compiler may be found in [Cross compiling](<Cross_compiling.md> "Cross compiling").

---

_Source: [https://wiki.freepascal.org/Platform_list](https://web.archive.org/web/20250312023230/https://wiki.freepascal.org/Platform_list)_
