# Ultibo core

│ **English (en)** │  **[suomi (fi)](</Ultibo_core/fi> "Ultibo core/fi")** │    
****

[![Raspberry Pi Logo.png](https://wiki.freepascal.org/images/8/85/Raspberry_Pi_Logo.png)](</File:Raspberry_Pi_Logo.png>)

This article applies to [Raspberry Pi](</Category:Raspberry_Pi> "Category:Raspberry Pi") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

Ultibo core is an [embedded](<Embedded.md> "Embedded") run-time platform and development environment written specifically for creating applications with small single board computers. It is built around the [Free Pascal compiler](<FPC.md> "FPC") and the [Lazarus](<Lazarus.md> "Lazarus") [integrated development environment](<IDE.md> "IDE"). Ultibo core is an RTL written entirely in Free Pascal and designed to make embedded or bare metal development accessible to all. 

## Contents

  * 1 Installation
    * 1.1 Windows
    * 1.2 macOS
    * 1.3 Linux
  * 2 Supported Devices
  * 3 Features
  * 4 Videos
    * 4.1 Discovering Ultibo
  * 5 Links



## Installation

### Windows

A Windows installer is available and full [source](<Source_code.md> "Source code") plus a large collection of examples is included. 

### macOS

  * Use FPC 3.1.1 the trunk version (so you have the source)
  * Download arm-none-eabi toolchain from <https://launchpad.net/gcc-arm-embedded/5.0/5-2015-q4-major/+download/gcc-arm-none-eabi-5_2-2015q4-20151219-mac.tar.bz2>
  * Extract the files in ~/Documents/gcc-arm-none-eabi
  * git clone from <https://github.com/ultibohub> at least Core, FPC, and Lazarus
  * You have 3 folders now, Core, FPC and Lazarus. Copy ultibo folder from Core/source/rtl/ultibo/ to FPC/source/rtl/
  * type cd FPC/source
  * type and wait until it finishes without errors:


    
    
     make distclean OS_TARGET=ultibo CPU_TARGET=arm SUBARCH=armv7a CROSSBINDIR=~/Documents/gcc-arm-none-eabi/arm-none-eabi/bin/
     CROSSOPT="-CpARMV7A -CfVFPV3 -CIARM -OoFASTMATH"
    

  * type and wait:


    
    
     make all OS_TARGET=ultibo CPU_TARGET=arm SUBARCH=armv7a CROSSBINDIR=~/Documents/gcc-arm-none-eabi/arm-none-eabi/bin/ CROSSOPT="-CpARMV7A -CfVFPV3 -CIARM -OoFASTMATH"
    

  * type and wait, assuming your FPC 3.1.1 is in the folder /usr/local/lib/fpc/3.1.1/ so option INSTALL_BASEDIR becomes INSTALL_BASEDIR=/usr/local/lib/fpc/3.1.1/


    
    
     sudo make crossinstall CROSSBINDIR=~/Documents/gcc-arm-none-eabi/arm-none-eabi/bin/ CROSSOPT="-CpARMV7A -CfVFPV3 -CIARM -OoFASTMATH" OS_TARGET=ultibo CPU_TARGET=arm SUBARCH=armv7a INSTALL_BASEDIR=/usr/local/lib/fpc/3.1.1/
    

  


### Linux

An installation script is available for [Linux](<Linux.md> "Linux"), full [source](<Source_code.md> "Source code") plus a large collection of examples is included. 

## Supported Devices

  * Raspberry Pi A, B, A+, B+
  * Raspberry Pi Zero, ZeroW and Zero2W
  * Raspberry Pi 2 model B
  * Raspberry Pi 3 model B
  * Raspberry Pi 3 model A+, B+
  * Raspberry Pi 4 model B
  * Raspberry Pi 400
  * QEMU emulator



## Features

Features include: 

  * Support for all models of Raspberry Pi (except Pico)
  * Pre-emptive threading
  * Full range of locking and synchronization primitives
  * Multicore support on Raspberry Pi 2, 3 and 4 with all cores sharing workload
  * Thread priority, affinity and migration support
  * Complete IPv4 stack including TCP, UCP, ICMP and raw sockets as well as DNS and DHCP protocols
  * USB support with drivers for Hub, Keyboard, Mouse, Storage, Network, HID, Touch, Gamepad and Joystick
  * MMC/SD/SDIO device support including eMMC devices on Raspberry Pi Compute Modules
  * Full support for FAT12/16/32, NTFS and CDFS filesystems
  * Interrupt handling
  * Hardware exception handling (catch an exception in code not a crash)
  * Complete RTL with strings, code pages, Unicode, classes, objects and exceptions
  * Clock and Timezone support
  * Console and framebuffer support including Fonts
  * Basic Shell (Console or Telnet) with file system commands and many others
  * HTTP/SMTP/POP3/NTP/Syslog client and server modules included
  * Support for Raspberry Pi devices like Timer, Watchdog and Random
  * Full Winsock 1.1 and Winsock 2 implementation
  * Packaged with a full compiler and IDE for quick start to development
  * Includes example projects plus a full demo image
  * HTTP image update module included (No SD card dance!)



Drivers added for: 

  * DMA
  * GPIO
  * UART and Serial
  * I2C
  * SPI
  * PWM
  * PL2303 and FTDI USB to Serial devices
  * USB support for the LAN7515 (LAN78XX) chip



New and enhanced features: 

  * Raspberry Pi 3 and 4 support (32 bit only)
  * Additional code page support
  * Common cipher and hash algorithms for the beginning of crypto support
  * Real time clock (RTC) support (with driver for DS1307 and similar chips)
  * Static IP support with command line configuration
  * Updated Lazarus IDE with modern docked layout
  * Extra fonts plus new font creation tools
  * Support for common LCD displays like the Adafruit 16x2
  * QEMU ARM emulation support
  * Port of the Asphyre/PXL graphics library
  * Support for the FreeVision windowing package
  * Standard C library support including POSIX threads
  * Fast interrupt (FIQ) support
  * Hardware accelerated OpenGL ES and OpenVG graphics and support for the official Pi camera



## Videos

  * [Ultibo Demo 1.0.0 ](<https://www.youtube.com/watch?v=GF9Xbf3mbf4>)



### Discovering Ultibo

  * [Episode 1 Getting Started ](<https://www.youtube.com/watch?v=TCfpb8M0WeQ>)
  * [Episode 2 Exploring USB](<https://www.youtube.com/watch?v=qPK4NTIKEO4>)
  * [Episode 3 Building the RTL ](<https://www.youtube.com/watch?v=2A7Faeaq72c>)
  * [Episode 4 GPIO Events](<https://www.youtube.com/watch?v=yfsuwbfyjgM>)
  * [Episode 5 LCD 16x2](<https://www.youtube.com/watch?v=M7_dz71Z4xE>)
  * [Episode 6 Remote LED](<https://www.youtube.com/watch?v=jS2YThZWhA4>)



## Links

  * [ultibo.org](<https://ultibo.org/>)
  * [github](<https://github.com/ultibohub/Core>)
  * [Ultibo Quick Start](<Ultibo_Quick_Start.md> "Ultibo Quick Start")

---

_Source: [https://wiki.freepascal.org/Ultibo_core](https://web.archive.org/web/20250418223128/https://wiki.freepascal.org/Ultibo_core)_
