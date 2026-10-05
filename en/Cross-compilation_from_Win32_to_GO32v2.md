# Cross-compilation from Win32 to GO32v2

│ **English (en)** │  **[magyar (hu)](</Cross-compilation_from_Win32_to_GO32v2/hu> "Cross-compilation from Win32 to GO32v2/hu")** │  **[Bahasa Indonesia (id)](</Cross-compilation_from_Win32_to_GO32v2/id> "Cross-compilation from Win32 to GO32v2/id")** │    
****

## Contents

  * 1 Multiple options for cross compilation
  * 2 Use real cross-compile binutils
  * 3 Use GO32v2 binutils
  * 4 Use emulation
  * 5 FPC RTL issues
    * 5.1 Creating GO32v2 binaries under WinXX DOS box
  * 6 See also



## Multiple options for cross compilation

In general, FPC support of cross-compilation expects you to have "cross-tools" (assembler and linker), i.e. tools running on the source platform, but creating binaries for (a different) target platform in your usual "bin" directory (i.e. i386-win32 for your Win32 installation). Filenames of these tools should be prefixed with the target platform specification (e.g. "i386-linux-as.exe" or "i386-go32v2-ld.exe"). 

When talking specifically about GO32v2 target under Win32, there are two possible ways how to get this. 

## Use real cross-compile binutils

The general way (possible for GO32v2 too) is to compile real cross-tools, i.e. compile the GO32v2 version of as.exe with GNU C for Win32 (Cygwin or MingW32) to get real Win32 binary producing outputs for GO32v2. This is the only possible solution for platforms for which there isn't emulation layer for running "their" binaries (Linux, FreeBSD, Netware etc.). Pre-compiled cross-tools for some host-guest combinations are available on [our FTP server](<ftp://ftp.freepascal.org/pub/fpc/contrib/cross/>) \- if you build those tools for other combinations, or build (and successfully test) newer versions of existing ones, you are welcome to contribute them. 

## Use GO32v2 binutils

The second possibility is to take the advantage of WinXX being able to run DOS binaries and just copy the GO32v2 versions of GNU tools to a new location and different name. This has some limitations (e.g. with regard to maximum length of the command line), but it works in general (although I wouldn't consider this solution really supported one - however, it's an easier one). 

## Use emulation

The third possibility is using DOS emulation provided by WinXX to avoid cross-compilation. If this works for you (see "Creating GO32v2 binaries under WinXX DOS box" below for more detailed information), this is the easiest solution. However, you are completely on your own there, since the WinXX emulation of DOS is far from being perfect and different WinXX versions impose various different limitations and issues one may need to face. 

## FPC RTL issues

In any case, you need both FPC RTLs (source) available, of course. One additional issue is that the GO32v2 compiler expects the RTL in /units/go32v2/rtl (to allow it to run even in plain DOS which suffers from 8.3 name length constraint), whereas the Win32 compiler (including that one integrated in our IDE) expects directories with full target names (i386-go32v2). 

This means you need to either rename the directory distributed together with the native GO32v2 release (if you don't intend to use the native GO32v2 compiler any more), or create a copy. 

Finally, cross-debugging (debugging of GO32v2 binaries in Win32 IDE) isn't possible at the moment. The only possibility is to use GO32v2 IDE (or standalone GO32v2 gdb) directly. 

### Creating GO32v2 binaries under WinXX DOS box

A simple way to generate plain DOS programs under 32-bit Win32 is the following: 

  * Install the Go32V2 version under Win32 in e.g. \ppdos.
  * Install a current Win32 version of FPC in e.g. \ppwin.
  * Do your development with the Win32 version.
  * If everything runs ok, use a DOS box (or a command shell) and compile your source code via the command line version of Go32v2. You have to inform the compiler (via command line switches) about the location of your units.



The resulting *.exe runs fine as _long_ as DPMI is present (you may distribute your *.exe together with cwsdpmi.exe which is in your FPC directory). If you don't want a separate DPMI host you may use one of the ways described below or compile your program under plain DOS (e.g. using FreeDOS). 

To avoid problems with long file names (larger than 8.3) you may use the "subst" command to access your files: e.g. "subst b: c:\documents...." would let you find all files in b: which are located in c:\documents. 

## See also

  * [Cross compiling](<Cross_compiling.md> "Cross compiling")
  * [Cross compiling for Windows under Linux](<Cross_compiling_for_Windows_under_Linux.md> "Cross compiling for Windows under Linux") another cross compiling setup
  * [buildfaq](<buildfaq.md> "buildfaq") information for building the compiler and cross compilers

---

_Source: [https://wiki.freepascal.org/Cross-compilation_from_Win32_to_GO32v2](https://web.archive.org/web/20240502140213/https://wiki.freepascal.org/Cross-compilation_from_Win32_to_GO32v2)_
