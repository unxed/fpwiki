# AVR

│ **English (en)** │

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The FPC-AVR port is experimental and might be broken from time to time. If this is the case, please fill a [bug report](<http://bugs.freepascal.org>)

**FPC-AVR** is the [Free Pascal Compiler](<FPC.md> "FPC") port for the AVR microcontroller. It uses the GCC AVR tool chain and will be compatible with GCC regarding calling conventions etc. 

For instructions on how to program AVR's with Free Pascal, see the article [AVR Programming](<AVR_Programming.md> "AVR Programming"). 

## Contents

  * 1 Implementation details
  * 2 Building cross-compiler
  * 3 Subarchitecture
  * 4 Resources



## Implementation details

FPC considers AVR to be a 16 bit CPU. So the type TRegister actually describes a register pair with the second register implicitly named. 

## Building cross-compiler

There are some requirements, you should know about: 

  * You should use the latest Free Pascal sources as there is the most recent support for more controllers.
  * Binutils for avr target 
    * Microchip (previously Atmel) provides bintuils binaries for Linux, macOS (Darwin) and Windows.  
[Download](<https://www.microchip.com/mplab/avr-support/avr-and-arm-toolchains-c-compilers>) using the appropriate link for your operating system]
    * On a Linux host, you may either install binutils for the avr target from your distribution or use also the binaries provided by Microchip (previously Atmel).
    * You may also use the binutils distributed with the Arduino IDE; on Windows they are installed to eg C:\Program Files (x86)\Arduino\hardware\tools\avr\avr\bin
    * Keep in mind that the Free Pascal Compiler will search for eg avr-embedded-as. So you must either rename the binary files, create symlinks (on Linux only) or adjust command line switch -XP<prefix> for FPC.
    * You may need to set FPC's command line switch -FD<directory with binutils>
  * Latest _stable_ Free Pascal Compiler.



Then you can build your cross compiler. Please choose SUBARCH according to your Arduino board / AVR microcontroller. 
    
    
    make buildbase installbase CPU_TARGET=avr OS_TARGET=embedded SUBARCH=avr5 CROSSINSTALL=1 INSTALL_PREFIX=~/fpc NOGDB=1 PP=/usr/bin/fpc CROSSBINDIR=/usr/avr/bin
    

You may also set the binutils prefix for this (equivalent to the **-XP** switch) 
    
    
    make buildbase installbase CPU_TARGET=avr OS_TARGET=embedded SUBARCH=avr5 CROSSINSTALL=1 INSTALL_PREFIX=~/fpc NOGDB=1 PP=/usr/bin/fpc CROSSBINDIR=/usr/avr/bin  BINUTILSPREFIX=avr-
    

The instructions above is good if only one subarchitecture is required. To install several subarchitectures, a unique unit install prefix needs to be specified for each subarchitecture using **INSTALL_UNITPREFIX** : 
    
    
    make buildbase installbase CPU_TARGET=avr OS_TARGET=embedded CROSSINSTALL=1 NOGDB=1 FPC=/usr/bin/fpc CROSSBINDIR=/usr/avr/bin  BINUTILSPREFIX=avr- INSTALL_PREFIX=~/fpc INSTALL_UNITPREFIX=~/fpc/lib/fpc/3.2.2/units/avr-embedded/avr5 SUBARCH=avr5
    

Remember to change both **SUBARCH** and **INSTALL_UNITPREFIX** when building a different subarchitecture. 

Together with this change, the fpc.cfg file paths needs to be updated to find units in this new structure. Below is an example of updating the unit specific search paths in fpc.cfg: 
    
    
    # searchpath for units and other system dependent things
    #IFDEF EMBEDDED
    -Fu~/fpc/lib/fpc/$fpcversion/units/$fpctarget/$fpcsubarch
    -Fu~/fpc/lib/fpc/$fpcversion/units/$fpctarget/$fpcsubarch/*
    -Fu~/fpc/lib/fpc/$fpcversion/units/$fpctarget/$fpcsubarch/rtl
    #ELSE
    -Fu~/fpc/lib/fpc/$fpcversion/units/$fpctarget/*
    -Fu~/fpc/lib/fpc/$fpcversion/units/$fpctarget/rtl
    #ENDIF
    

Adapt paths as required. 

## Subarchitecture

The operating system target _embedded_ requires that you specify a subarchitectures when building a cross compiler. The subarchitectures are compatible with the ones used by GCC. 

## Resources

  * [Atmel Studio 7](<https://www.microchip.com/mplab/avr-support/atmel-studio-7>)
  * [Downloads Archive for AVR tools](<https://www.microchip.com/mplab/avr-support/avr-and-sam-downloads-archive>)
  * [General information about Atmel AVR in Wikipedia](<http://en.wikipedia.org/wiki/Atmel_AVR>)
  * [Calling conventions](<http://www.nongnu.org/avr-libc/user-manual/FAQ.html#faq_reg_usage>)
  * [list of subarchitectures](<http://www.nongnu.org/avr-libc/user-manual/using_tools.html>)
  * [Discussion on lazarusforum.de (German)](<http://www.lazarusforum.de/viewtopic.php?f=9&t=5444&start=30>)
  * [AVR Programming](<AVR_Programming.md> "AVR Programming")
  * [Arduino](<Arduino.md> "Arduino")
  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Tutorials with code examples

---

_Source: [https://wiki.freepascal.org/AVR](https://web.archive.org/web/20250420132333/https://wiki.freepascal.org/AVR)_
