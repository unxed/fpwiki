# TARGET Embedded Mipsel

The mipsel-embedded target was created to target the PIC32MX microcontroller chips produced by Microchip. 

There exist several lines of chips: 

**PIC32MM:** Lightweight parts with configurable logic cells peripheral. 

**PIC32MX1/2:** These are the most maker-friendly chips in the lineup, they come in the form of DIL-28 and SOIC-28, even the TQFP-44 packages are relatively easy to solder. Max. CPU speed is 50MHz. 

**PIC32MX3/4/5/6/7xx:** High Pin Count devices starting with TQFP-48 packages and max. CPU speed of 80-100MHz. 

**PIC32MK:** Motor control parts up to 120MHz using 64 and 100 pin packages. Internals and peripherals closer to MZ than to MX with some updates like all timers 32-bit. 

**PIC32MZ:** High Pin Count devices starting with QFN-64 packages and max. CPU speed 200MHz. uses "MIPS 14K" Architecture which on top of faster processing offers additional features such as eight register sets, enhanced ability for signal processing (eg by four 64 Bit accumulators and SIMD instructions) enhanced hardware support for Interrupts. 

## Contents

  * 1 Status (as at April, 2014)
  * 2 Build
  * 3 Compiling a simple example
  * 4 Flashing and Debugging
  * 5 See also



## Status (as at April, 2014)

  * support is only available in svn trunk version
  * the complete lineup of existing controllers is supported, testing is done with PIC32MX150, PIC32MX460 and PIC32MX795 chips on a Mac Host.



## Build

Get latest FPC from svn: 
    
    
    svn co http://svn.freepascal.org/svn/fpc/trunk fpc
    

Get pic32-embedded tools from [this page](<http://chipkit.s3.amazonaws.com/index.html>). 

You will find precompiled binaries for Mac, Linux and Windows in the directory 'compiler': 

( compilers/pic32-tools-chipKIT-cxx-master-{Platform}-20130228.zip ) 

**Important:** Make sure that you grab the old version from 20130228, the newer versions create a compiler assertion. 

You only need the binaries of pic32-ar(.exe), pic32-as(.exe), pic32-ld(.exe), pic32-strip(.exe), pic32-objdump(.exe) and pic32-objcopy(.exe). Put these utils in a directory included in your path. 

To build binutils from sources check out [chipKIT-cxx.git](<https://github.com/chipKIT32/chipKIT-cxx.git>). 
    
    
    cd binutils
    ./configure --target=pic32mx-elf --prefix=/usr/local --program-prefix=pic32- --disable-werror
    

Build FPC for mipsel-embedded (OABI): 
    
    
    cd fpc
    make clean buildbase installbase CROSSINSTALL=1 OS_TARGET=embedded CPU_TARGET=mipsel SUBARCH=pic32mx BINUTILSPREFIX=pic32-
    

This builds only the compiler and the RTL instead of the whole FPC. Because of the limited capabilities of the embedded systems to be used, building all the packages is not useful. However, one must be careful to avoid overwriting an existing ARM compiler on the system. If this is the case, INSTALL_PREFIX should be used to install into a different directory and switch between those. 

SUBARCH is required and should match your binutils, currently there are two different versions of binutils: 

  * **pic32mx** : chipKIT port with some PIC32 specific patches, has the same output arch as xc32, so linking with xc32-libs should be possible


  * **mips32r2** : plain vanilla binutils, linking with xc32-libs does not work



The Microchip's official version of binutils that is delivered with the xc32 compiler does not work correctly at the moment. 

All testing was done with the chipKIT version, besides the possibility of linking with xc32 generated code one advantage is that binaries are available for all major platforms. 

For Debugging you will also need to build pic32-gdb from sources: 
    
    
    cd gdb
    ./configure --target=mipsel-elf32 --prefix=/usr/local --program-prefix=pic32- --disable-werror
    

## Compiling a simple example
    
    
    program hello;
    var
      i : integer;
    begin
      GPIOB.setConfig([RB0,RB1,RB2,RB3,RB4,RB5,RB6,RB7],GPIO_Output_Push_Pull);
    
    while true do
    begin
      GPIOB.clearBit([RB0,RB1,RB2,RB3,RB4,RB5,RB6,RB7]);
      for i := 0 to 40000 do
        ;
      GPIOB.setBit([RB0,RB1,RB2,RB3,RB4,RB5,RB6,RB7]);
      for i := 0 to 40000 do
        ;
    end;
    end.
    

To build the program use the following commands (on Linux & Mac) 
    
    
    DEBUG="-gw2 -godwarfsets -godwarfmethodclassprefix"
    BINUTILS=pic32-
    SUBARCH=pic32mx
    CPU=pic32mx795f512h
    /usr/local/lib/fpc/2.7.1/ppcrossmipsel -Cp$SUBARCH -Wp$CPU -XP$BINUTILS -MObjFPC -d$CPU -Tembedded -Pmipsel -O2 -Scghi -Ch1024 -Cs1024 $DEBUG -vewnhix -l -FD/usr/local/bin -a hello.pas
    

## Flashing and Debugging

Some history: The pic32 target has been in development for nearly a year now, the main reason for this long development was that there was simply no easy way to reliably flash the chip. But this has changed now, a very simple solution is available from [Segger](<http://www.segger.com>), their Version 9.0 of the J-Link hardware now supports flashing and debugging of all PIC32MX chips. 

The [OpenOCD project](<http://openocd.org/>) also has support for flashing PIC32MX chips but I had mixed results with flashing; debugging on the other side works just fine. 

So my recommendation here is that you invest €50 and buy a J-Link Edu debug probe. It also works great for ARM-based controllers so you only need one tool for debugging both platforms. You must make sure that the hardware revision of the probe is at least V9, older versions do not support PIC32MX. 

The only thing you need in addition to the probe is an adapter to connect the J-Link to the PIC32, a schematic can be found on the [Segger home page](<https://www.segger.com/jlink-adapters.html#MicrochipAdapter>). 

Debugging is now straightforward: 

Start the gdb-server: 
    
    
    JLinkGDBServer -device PIC32MX795F512H -if 2-wire-JTAG-PIC32 -speed auto
    

and connect to it with gdb: 
    
    
    pic32-gdb hello.elf --tui --eval-command="target extended :2331" --eval-command="monitor reset halt" --eval-command="set mem inaccessible-by-default off" --eval-command="load" --eval-command="layout asm" --eval-command="layout regs"  --eval-command="set remotetimeout 3000"
    

## See also

  * [Native MIPS Systems](<Native_MIPS_Systems.md> "Native MIPS Systems")

---

_Source: [https://wiki.freepascal.org/TARGET_Embedded_Mipsel](https://web.archive.org/web/20250124212421/https://wiki.freepascal.org/TARGET_Embedded_Mipsel)_
