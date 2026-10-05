# Free Pascal supported targets

## Contents

  * 1 Free Pascal supported targets
    * 1.1 Main targets
    * 1.2 Fully supported targets
    * 1.3 Self-build supported targets
    * 1.4 Unsupported but probably working targets
    * 1.5 Unsupported, abandoned targets



# Free Pascal supported targets

Here's a list of all targets that are supported by the Free Pascal compiler. They are classified into five groups. 

## Main targets

These targets are fully supported, do have their own installer and are included into all releases. 

Darwin/iOS - ARM
Darwin/iPhoneSimulator - i386
Darwin/macOS - i386/x86-64 (Intel 32 and 64 bit)
Darwin/macOS - PowerPC (32 and 64 bit)
Dos/Go32v2 - i386
    To install unzip the archive and run INSTALL.EXE, which will guide you through the installation-process. After the installation you have to add the "<selected-install-path>\BIN\GO32V2" directory to the PATH environment variable.
    Note that the compiler will work on FreeDos, but that it is not possible to compile (cycle) the compiler itself on FreeDos.
FreeBSD - i386/x86_64 (32 and 64 bit)
Linux - i386 (tar and rpm-releases)
    To install from the tar-archive, unpack the archive and run the install.sh script. When run as a non-root user, by default Free Pascal is installed into the user's home directory. In that case it is necessary to add the "<selected-install-path>/bin" directory to the binary-search path. To install from the rpm-package, use your system's package-manager. Note that both the tar- and rpm-releases do not install any dependencies, such as the binutils for the compiler and ncurses and gpm for the text-mode ide.
    It is also possible to Install this version of the compiler on x86_64 systems to build i386-executables.
Linux - x86_64 (tar and rpm-releases)
    To install from the tar-archive, unpack the archive and run the install.sh script. When run as a non-root user, by default Free Pascal is installed into the user's home directory. In that case it is necessary to add the "<install-path>/bin" directory to the binary-search path. To install from the rpm-package, use your system's package-manager. Note that both the tar- and rpm-releases do not install any dependencies, such as the binutils for the compiler and ncurses and gpm for the text-mode ide.
OS/2 - i386
Win32 - i386
    To install just download and run the installer.
Win64 - x86_64
    To install just download and run the installer.

## Fully supported targets

These targets are fully supported, do have their own installer but might be missing in some releases. 

Amiga - m68k (68020+ only at the moment)
AROS - i386
DOS - i8086 (x86 16-bit, cross compiled from win32-i386 and linux-i386)
Embedded - ARM (various micro-controllers)
FreeBSD - i386/x86_64 (32 and 64 bit)
Linux - PowerPC (32 and 64 bit)
Linux - Sparc (32 bit)
Linux - ARM (EABI, softfp and hardfloat)
MorphOS - PowerPC
WinCE - ARM (cross compiled from win32-i386)

## Self-build supported targets

These targets are supported but are not distributed with their own installer. You have to build the compiler and units yourself. 

Android - ARM
Android - i386
Android - MIPS
Linux - m68k (ColdFire and 68020+ only)
Win16 - i8086

## Unsupported but probably working targets

These targets are not officially supported, but there is at least one person working on them. 

NativeNT - i386

## Unsupported, abandoned targets

These targets did work some day, but not anymore. Or someone started on them, but they were never completed. 

Dos/Watcom extender - i386
EMX - i386 (binaries working on both OS/2 and DOS with EMX extender)
Linux - ARM (OABI)
Mac OS ("Classic", v9 and below) - PowerPC
Netware - i386
WDOSX

---

_Source: [https://wiki.freepascal.org/Free_Pascal_supported_targets](https://web.archive.org/web/20200919230859/https://wiki.freepascal.org/Free_Pascal_supported_targets)_
