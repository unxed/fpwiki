# Binutils

│ **English (en)** │  **[русский (ru)](<../ru/Binutils.md>)** │

## Overview

In most cases of cross-compiling from one platform to another you need cross binutils, i.e. assembler, linker, and so on. 

## Building your own binutils

If they are not readily available for your combination of host and target platform, you can compile them from the sources from their home page (<http://www.gnu.org/software/binutils/>). 

This is a quite typical configure for the binutils, in particular useful together with fpc: 
    
    
    ./configure \
    --target=arm-linux \
    --prefix=/usr/local/lib/arm-linux \
    --bindir=/usr/local/bin --mandir=/usr/local/share/man --infodir=/usr/local/share/info \
    --program-prefix=arm-linux \
    --disable-werror
    

for Unix/Linux-like targets, the options target and prefix are fairly straight forward, but not for others. 

List of options: 

Platform | Target | Prefix   
---|---|---  
Windows (32 bit) | i386-mingw32 | i386-win32   
Win CE (Intel) | i386-wince | i386-wince   
Win CE (arm) | arm-wince-pe | arm-wince   
Win CE (i386) | i386-wince | i386-wince   
Solaris | i386-solaris | i386-solaris   
OS/2 | i386-os2-emx | i386-os2   
go32v2 | i386-go32 | i386-go32v2   
i386-netbsd | i386-netbsdelf-gnu | i386-netbsd   
Symbian OS | i386-? | i386-?   
Mac OS X (i386) | i386-darwin | i386-darwin   
Mac OS X (PowerPC) | powerpc-darwin | powerpc-darwin   
Mac OS X (x86-64) | x86_64-darwin | x86_64-darwin   
iPhoneOS | arm-darwin? | arm-darwin?   
Amiga (classic) | m68k-amiga | m68k-amiga   
Amiga OS | powerpc-? | powerpc-?   
AROS (i386) | i386-aros | i386-aros   
MorphOS | ppc-morphos | powerpc-morphos   
arm-gba | arm-thumb-elf | arm-gba   
arm-nds | arm-thumb-elf | arm-nds   
arm-embedded | arm-elf | arm-embedded   
arm-symbian | ? | ?   
arm-PalmOS | ? | ?   
arm-linux | arm-linux | arm-linux   
sparc-linux | sparc-linux | sparc-linux   
sparc-solaris | sparc-solaris | sparc-solaris   
m68k-linux | m68k-linux | m68k-linux   
m68k-netbsd | m68k-netbsdelf | m68k-netbsd   
m68k-embedded | m68k-elf | m68k-embedded   
mipsel-linux | mipsel-linux | mipsel-linux   
i8086-msdos | msdos-nasm and linker | 8086-msdos   
x86_64-w64 | x86_64-w64-mingw32 | x86_64-w64

---

_Source: [https://wiki.freepascal.org/Binutils](https://web.archive.org/web/20210922021136/https://wiki.freepascal.org/Binutils)_
