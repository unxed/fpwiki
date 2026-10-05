# Operating Systems written in FPC

│ **English (en)** │  **[español (es)](</Operating_Systems_written_in_FPC/es> "Operating Systems written in FPC/es")** │    
****

There is a rich tradition of operating systems written in [Pascal](<Pascal.md> "Pascal"). The probably best-known example is [classical Mac OS](<http://en.wikipedia.org/wiki/History_of_Mac_OS>), a very influential historic operating system, that was nearly entirely written in Pascal (not to be confused with macOS that is written in Objective C). Other operating systems written in Pascal include [Lisa OS](<http://en.wikipedia.org/wiki/Apple_Lisa>) and the [Lillith](<http://en.wikipedia.org/wiki/Lilith_\(computer\)>) system software. The operating system for the famous [Xerox Star](<http://en.wikipedia.org/wiki/Xerox_Star>) was written in the [Mesa](</index.php?title=Mesa_\(programming_language\)&action=edit&redlink=1> "Mesa \(programming language\) \(page does not exist\)") programming language, a modular language of the Pascal family. Today there are several attempts to write a Pascal-based operating system from scratch. The following, probably incomplete, list contains some approaches to use Free Pascal for creating an OS. 

## Contents

  * 1 FPOS
    * 1.1 Implemented features in FPOS
  * 2 StreamOS
  * 3 TORO Kernel
  * 4 FP-RTOS
  * 5 Ultibo core
  * 6 ELERA
  * 7 Asuro
  * 8 External links



## [FPOS](<https://github.com/rezgui/fpos>)

FPOS is a operating system consists of a minimal kernel built on FreePascal, totaly written from scratch with Free Pascal compiler. It contains a Scheme implementation of a hard drive (ATA) driver, keyboard (PS2), serial (8250 UART), FAT32 filesystem and a small real time clock manager. The project was built to experiment with developement of operating system using a high level functional language to study the developement process and the use of Scheme to build a fairly complex system. 

### Implemented features in FPOS

  * GDT, IDT ( no problem... I hope )
  * ISR ( currently, only page fault has additional handler )
  * IRQ ( no problem... I hope )
  * Console ( including simple command processing )
  * Keyboard ( try to be US-std, but I don't know the keymap. Just guessing for now )
  * Memory Manager ( needs testing )
  * Speaker ( just for fun :-) )
  * RTC ( seems wrong at PM, also for DayOfWeek )
  * System calls
  * Multitasking
  * Filesystem ( perhaps FAT12 is the most obvious, or should we create one ourselves? )



## [StreamOS](<https://github.com/necokeine/StreamOS/tree/master>)

StreamOS is 32-bit operating system written in Object Pascal using Free Pascal Compiler. It’s designed for: 

  * scientific tasks such as long calculations;
  * finding the best architecture of mixed kernel;
  * academic interests.



The main advantages are: 

  * 32-bit fast kernel;
  * no global kernel locks;
  * full parallel threading model;
  * easy API;
  * integrated Virtual Filesystem support called StreamFS;
  * Unix-likeness.



## [TORO Kernel](<http://www.torokernel.io//>)

Toro is a kernel created in Pascal using FreePascal compiler. The project was started in the year 2003 by Matias E. Vara using a monolithic kernel.The whole kernel is included in a RTL compliant with FreePascal for i386 and AMD x86-64 processors. The user program is compiled within the kernel and is executed in kernel mode. The intrinsic OS design select strict model in data workflow and pipeline of execution in order to avoid concurrent execution on the same shared memory region. The kernel was optimized for Multicore system and that is the principal objective of the project ,fix the current problems of Modern O.S. in Multicore environment. The main features are: 

  * Supports Multithreading with SMP(Symmetric MultiProcessing).
  * Simple interface with Lazarus IDE for programmers.
  * Supports AMD x86-64 and Intel EMT64 architecture.
  * Designed to optimize the use of AMD HyperTransport technology and Intel Quick Path Interconnect technology.
  * NUMA Memory Model (Non Uniform Memory Access).
  * Stack TCP-IP.
  * Virtual Filesystem.
  * Independent of architecture.



## [FP-RTOS](<https://github.com/Laksen/fp-rtos/tree/master>)

A realtime kernel for embedded development. Primarily developed for ARM devices. Written entirely in Pascal and inline assembler. 

  * Preemptive multitasking
  * Synchronization primitives(Mutex, Critical section, spinlock, and signals)
  * Threadsafe queue and delays
  * Optional safety features: Deadlock detection, priority inheritance, etc



## [Ultibo core](<https://ultibo.org/>)

[Ultibo core](<Ultibo_core.md> "Ultibo core") is a kernel in an RTL first released in 2016, much more than just another OS example it is a complete platform for creating modern full featured embedded applications which run without any operating system. Written 100% in Free Pascal and initially targeting Raspberry Pi, the modular design allows including only the features required for any given project. Packaged in a simple installer download for Windows or as an install script for Linux customized versions of both Free Pascal and Lazarus IDE are included along with full source and a large collection of examples to get started quickly. 

Features include: 

  * Support for all models of Raspberry Pi (A/B/A+/B+/2B/3B/3B+/4B/400/Zero/ZeroW/Zero2W) plus QEMU ARM emulation
  * Pre-emptive threading
  * Full range of locking and synchronization primitives
  * Multicore support on Raspberry Pi 2, 3 and 4 with all cores sharing workload
  * Thread priority, affinity and migration
  * Complete IPv4 stack including TCP, UDP, ICMP and raw sockets as well as DNS and DHCP protocols
  * USB support with drivers for Hub, Keyboard, Mouse, Storage, Network, HID, Touch, Gamepad and Joystick
  * MMC/SD/SDIO device support including eMMC devices on Raspberry Pi Compute Modules
  * Full support for FAT12/16/32, NTFS and CDFS filesystems
  * Interrupt and fast interrupt handling
  * Hardware exception handling
  * Complete RTL with strings, code pages, Unicode, classes, objects and exceptions
  * Full Winsock 1.1 and Winsock 2 implementation
  * Standard C library support including POSIX threads
  * Prebuilt libraries for Freetype2, SQLite, Zlib, Libpng and more included
  * Support for many common FPC packages
  * Hardware accelerated OpenGL ES and OpenVG graphics and support for the official Pi camera
  * Many included extras like HTTP, SMTP, NTP, SysLog, Telnet and Shell



## [ELERA](<https://bitbucket.org/elera0/elerais>)

ELERA is 32-bit operating system that written Free Pascal / Lazarus from stratch. Operating system is: 

  * Pre-emptive multi-tasking
  * VESA 2.0+ GUI interface (up to 1024x768 resolution support)
  * PS/2 mouse, keyboard, pcnet32 network, floppy, IDE disk hardware support
  * FAT12, FAT16, FAT32 file system support
  * ARP, UDP, IP socket layer



## [Asuro](<https://www.spexeah.com/index.php/asuro/>)

Asuro is an x86 operating system that started development in 2015 as somewhat of an academic project. Written almost entirely in Free Pascal, with NASM used only for bootstrapping & minimal low-level routines. Asuro is a purely hobbyist operating system that differs quite majorly in design from any mainstream OS. Asuro doesn't really have an end goal in mind, it's aim is mostly to function as a playground for those that wish to play with low-level computing using Free Pascal. A dockerized build environment allows for minimal configuration to begin building/breaking/fixing Asuro, and we welcome anyone to play around with Asuro for fun or profit. 

# External links

  * [wiki.osdev.org Pascal Bare Bones](<http://wiki.osdev.org/Pascal_Bare_Bones>)
  * [GRUB-bootable "Hello World" OS written by nicksyn](<https://github.com/nickelsworth/paskernel>)

---

_Source: [https://wiki.freepascal.org/Operating_Systems_written_in_FPC](https://web.archive.org/web/20250326021023/https://wiki.freepascal.org/Operating_Systems_written_in_FPC)_
