# Embedded

│ **English (en)** │

This page is about embedded systems with operating system (Nintendo platforms, Linux Embedded, Windows embedded). For microcontroller programming, i.e. embedded systems without operating system, see [TARGET Embedded](<TARGET_Embedded.md> "TARGET Embedded"). 

  
An **embedded system** is a computer system designed to perform one or a few dedicated functions, often with real-time computing constraints. It is embedded as part of a complete device often including hardware and mechanical parts. 

In contrast, a **general-purpose computer** (such as a PC), is designed to be flexible and to meet a wide range of end-user needs. Embedded systems control many devices in common use today. See [Wikipedia:Embedded System](<http://www.wikipedia.org/wiki/Embedded_System> "wikipedia:Embedded System") for further description. 

As general purpose systems (PCs) are becoming smaller and smaller and embedded systems become more and more powerful (and universally usable devices), the border between embedded systems and general purpose computers will blur in future. Also a point of discussion would be to which section smart phones will end up. 

We can divide embedded systems into several sections: 

## Contents

  * 1 Embedded systems without general purpose operating system (OS)
  * 2 Nintendo platforms
  * 3 Embedded systems with a general purpose operating system
    * 3.1 Embedded Linux
    * 3.2 Embedded Windows



## Embedded systems without general purpose operating system (OS)

For these devices, a special target in FPC exists: [TARGET Embedded](<TARGET_Embedded.md> "TARGET Embedded"). 

Also see: [Ultibo core](<Ultibo_core.md> "Ultibo core"). 

## Nintendo platforms

For these devices, special targets exist in FPC. Also there exist specially built cross-compilers for the x86 Windows platform: 

  * [Gameboy Advance](<http://en.wikipedia.org/wiki/Game_Boy_Advance>) (OS_TARGET=gba CPU_TARGET=arm BINUTILSPREFIX=arm-eabi)
  * Main processor: [ARM7tdmi](</index.php?title=ARM7tdmi&action=edit&redlink=1> "ARM7tdmi \(page does not exist\)")
  * Sub processor: [Z80](<Z80.md> "Z80"); FPC build: arm-gba-fpc-2.4.2.i386-win32.zip
  * [Nitendo DS](<http://en.wikipedia.org/wiki/Nintendo_DS>) (OS_TARGET=nds CPU_TARGET=arm BINUTILSPREFIX=arm-eabi)
  * Main processor: [ARM](</index.php?title=ARM-Architektur&action=edit&redlink=1> "ARM-Architektur \(page does not exist\)")946E-S (67 MHz, in DSi: 133 MHz)
  * Sub processor: ARM7TDMI (33 MHz); FPC build: arm-nds-fpc-2.4.2.i386-win32.zip



## Embedded systems with a general purpose operating system

There are several operating systems commonly used in embedded systems 

### Embedded Linux

Here several flavors exist: from (slightly to radical) slimmed down desktop Linux distributions (like Debian) to even slimmer variants consisting of a Linux kernel and some compact tools packages like [BusyBox](<http://www.busybox.net/>). Other variants like: 

  * [Open RTLinux](<http://www.rtlinuxfree.com/RTLinuxFree>) aim on enhancements of the real-time behaviour.
  * [ARM Linux Embedded Systems](<ARM_Linux_Embedded_Systems.md> "ARM Linux Embedded Systems").
  * [i386 Linux Embedded Systems](<i386_Linux_Embedded_Systems.md> "i386 Linux Embedded Systems").



### Embedded Windows

We have **Windows XP Embedded** which behaves mostly like its desktop counterpart (i386 only) but offers the following benefits: 

  * System Builder allows to include/exclude components to slim down the system
  * Readonly file-systems are supported for more robust operation
  * Licensing done in System Builder; no individual licensing at product rollout



Successors: 

  * **Windows Embedded Standard 7**
  * **Windows Embedded 8 Standard**
  * **Windows 10 IoT**
  * **Windows 11 IoT**



Another branch is **Windows Mobile** / **Windows CE** family of operating systems. 

Variants: 

  * i386
  * ARM
  * MIPS

---

_Source: [https://wiki.freepascal.org/Embedded](https://web.archive.org/web/20250124220726/https://wiki.freepascal.org/Embedded)_
