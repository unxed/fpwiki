# TARGET Embedded

│ **[English (en)](<Embedded.md> "Embedded")** │  **[español (es)](</Embedded/es> "Embedded/es")** │  **[中文（中国大陆） (zh_CN)](</Embedded/zh_CN> "Embedded/zh CN")** │  **[中文（臺灣） (zh_TW)](</Embedded/zh_TW> "Embedded/zh TW")** │    
****

This is an overview of using FPC on embedded systems without an operating system. The embedded target is still under development and only a few microcontroller families are supported. These are ARM (several architectures), AVR, PIC32MX (MIPSel), Xtensa (ESP32) and Z80. The embedded target targets systems which are _without any operating system_ and typically only have several kBs of RAM and several dozens of kB of flash. A typical target is the LPC family of NXP with popular members like the LPC2124 with 16 kB RAM and 256 kB flash using the ARM7 instruction set. 

## Contents

  * 1 Embedded Port Status
  * 2 How to use heap
  * 3 ARM Embedded
    * 3.1 Build
    * 3.2 NXP LPC2124 example
    * 3.3 Nordic nRF51822 example
  * 4 Adding new microcontrollers
    * 4.1 Add the microcontroller type to the compiler
    * 4.2 Add the linker script to the compiler
    * 4.3 Create an RTL unit with startup code and register definitions
  * 5 AVR
  * 6 MIPSel (PIC32)
  * 7 Xtensa
  * 8 Z80
  * 9 Useful links
  * 10 See Also



## Embedded Port Status

  * Best support is available in the Git "main" branch.
  * Compilers are working, register definitions are available, but libs for peripheral access are rare to non-existent. Hardware can be accessed via hardware registers.
  * Not all microcontrollers of each family are supported. If you're interested in support for further microcontrollers, please subscribe to the [fpc-devel mailing list](<http://lists.freepascal.org/mailman/listinfo/fpc-devel>) and write a mail so adding the support can be discussed.



## How to use heap

By default, heap is not initialized on embedded targets, so any app trying to allocate on the heap, will crash. To fix that, add the **heapmgr** unit to the `uses` clause. 

The size of the heap is controlled with the [{$Memory <StackSize>, <HeapSize>}](<https://www.freepascal.org/docs-html/current/prog/progsu102.html#x110-1110001.3.19>) directive or the -Ch option. Additional regions beside the initial one can be registered using `HeapMgr.RegisterHeapBlock`. 

## ARM Embedded

### Build

Get latest FPC from git: 
    
    
    git clone https://gitlab.com/freepascal.org/fpc/source.git fpc
    

Get arm-embedded binutils. 

For windows, they are available at <http://svn.freepascal.org/svn/fpcbuild/binaries/i386-win32>. You can either checkout the whole directory with or download only arm-embedded-ar.exe, arm-embedded-as.exe, arm-embedded-ld.exe, arm-embedded-strip.exe, arm-embedded-objdump.exe and arm-embedded-objcopy.exe manually. Put these utils in a directory included in your path. 

To build binutils from sources <http://wiki.lazarus.freepascal.org/Binutils> has instructions, for example, ./configure --target=arm-linux --prefix=/opt/embarm/ --program-prefix=arm-embedded- --disable-werror, has been known to work on binutils 2.21.1 to build the binaries in the form that the fpc target embedded build desires. 

Build FPC for arm-embedded (OABI): 
    
    
    cd fpc
    make clean buildbase installbase OS_TARGET=embedded CPU_TARGET=arm SUBARCH=armv4
    

This builds only the compiler and the RTL instead of the whole FPC. Because of the limited capabilities of the embedded systems supposed to be used, building all packages is not useful. However, one must be careful to avoid overwriting an existing arm compiler on the system. If this is the case, INSTALL_PREFIX should be used to install into a different directory and switch between those. 

SUBARCH is required and should match your target processor, e.g.: 

  * for the LPC 21xx parts which are ARM7TDMI (not to be confused with armv7!) based use SUBARCH=armv4
  * for cortex-m3 parts (stellaris, stm32, etc) use SUBARCH=armv7m
  * for cortex-m devices that support DSP extensions use SUBARCH=armv7em. The compiler does not yet support the cortex-m* FPU
  * for cortex-m0 devices use -Cparmv6m



### NXP LPC2124 example

Below you find a simple example program for the LPC2124 microcontroller, save it as tled1.pp to follow the description. The program is made for development boards like the LPC-WEB from OLIMEX (<http://www.olimex.com/dev/lpc-e2124.html>). When you press button 1, led 1 lights for a certain time, same for button 2 and led 2. 
    
    
    procedure Wait(d : dword);
    begin
      while d<>0 do
        dec(d);
    end;
    
    begin
      { initialize PLL }
      InitPLL(2,1);
    
      { initialize LEDs }
      { port 0.8: output }
      TBitvector32(GPIO0_IODIR)[8]:=1;
      { port 0.10: output }
      TBitvector32(GPIO0_IODIR)[10]:=1;
    
      { turn off both LEDs }
      TBitvector32(GPIO0_IOSET)[8]:=1;
      TBitvector32(GPIO0_IOSET)[10]:=1;
    
      { initialize button inputs }
      { port 0.9: input }
      TBitvector32(GPIO0_IODIR)[9]:=0;
      { port 0.15: input }
      TBitvector32(GPIO0_IODIR)[15]:=0;
    
      { endless loop}
      while true do
        begin
    
          { button 1 pressed }
          if TBitvector32(GPIO0_IOPIN)[15]=0 then
            begin
              { turn on LED, inverse logic }
              TBitvector32(GPIO0_IOCLR)[8]:=1;
              { wait }
              Wait(500000);
              {  turn off LED, inverse logic }
              TBitvector32(GPIO0_IOSET)[8]:=1;
          end;
    
          { button 2 pressed }
          if TBitvector32(GPIO0_IOPIN)[9]=0 then
            begin
              { turn on LED, inverse logic }
              TBitvector32(GPIO0_IOCLR)[10]:=1;
              { wait }
              Wait(500000);
              { turn off LED, inverse logic }
              TBitvector32(GPIO0_IOSET)[10]:=1;
          end;
        end;
    end.
    

If FPC for arm-embedded is installed as described above, the program can be compiled by 
    
    
    fpc -Parm -Tembedded -Wplpc2124 tled1.pp
    
    
    
    -Parm
    

tells the compiler to compile for arm. 
    
    
    -Tembedded
    

tells the compiler to compile for the embedded target. 
    
    
    -Wplpc2124
    

tells the compiler to compile for the NXP LPC 2124. This has two effects: first, a unit (lpc21x4 in this case) containing the startup code and the port etc. definitions for this controller is loaded. Further, the compiler uses a linker script which fits the needs of this controller. 

The result of the compiler is a .hex file which can be programmed by the NXP flash programming utility. 

When using a board like the mbed (<http://mbed.org>) which is cortex-m3 based. Another ARM core but uses only thumb/thumb2 instructions not ARM/thumb instructions. A few subtle differences, first building fpc 
    
    
    make clean buildbase installbase CROSSINSTALL=1 OS_TARGET=embedded CPU_TARGET=arm SUBARCH=armv7m
    

Second when compiling your program 
    
    
    fpc -Parm -Tembedded -Wplpc1768 -Cparmv7m myprogram.pp
    

The -Cparmv7m option causes thumb2 code to be generated instead of the default (ARM based), probably not a bad idea to use the -Cp for processors using ARM instructions as well. -Wp as with the example above specifies the specific target, in this case microcontroller, to build for. 

### Nordic nRF51822 example

See the [micro:bit](<micro_bit.md> "micro:bit") page. 

## Adding new microcontrollers

Adding a new microcontroller type requires basically three steps: first, extend the compiler so it knows about the name of the controller then add a linker script and finally create an RTL unit with the register definitions and the startup code. 

#### Add the microcontroller type to the compiler

This is done in the file `compiler/arm/cpuinfo.pas`. Here you need to add an identifier for the controller in the tcontrollertype enum At the same time you must update the embedded_controllers array. The array holds information about the amount of flash and sram in the controller, and what rtl unit should be included by default when compiling for that controller. 

_Note: Make sure the string of the two arrays are long enough to contain the strings you write_

#### Add the linker script to the compiler

Adding a linker script consists of adding the controllertype enum identifier to the case statement in compiler/systems/t_embed.pas If this is not done the compiler will generate an internalerror 

#### Create an RTL unit with startup code and register definitions

Creating an RTL unit means creating a unit that'll automatically be used in a project compiled for the given controller type. This unit should supply an entry point and basic initialization code. Enough to get the controller running 

This unit, which must be named the same as the string written in the controllerunitstr field in the embedded_controllers array, goes into the directory rtl/embedded/arm/ where there are some examples already 

After that the rtl/embedded/Makefile.fpc should be modified, and translated with [Fpcmake](<Fpcmake.md> "Fpcmake") so the newly added rtl unit is compiled when building the cross compiler. The commandline for fpcmake (to be executed in rtl/embedded) is "fpcmake.exe -w -Tall". 

  


## AVR

See the [AVR](<AVR.md> "AVR") wiki page. 

## MIPSel (PIC32)

See the [MIPSel](<TARGET_Embedded_Mipsel.md> "TARGET Embedded Mipsel") wiki page. 

## Xtensa

See the [Xtensa](<Xtensa.md> "Xtensa") wiki page. 

## Z80

See the [Z80](<Z80.md> "Z80") wiki page. 

## Useful links

  * <http://de.wikipedia.org/wiki/Intel_HEX>
  * <http://www.nxp.com/products/microcontrollers/support/software_download/lpc2000/>
  * <http://www.nxp.com/acrobat_download/usermanuals/UM10114_3.pdf>
  * Jim Lynch: Using Open Source Tools for AT91SAM7 Cross Development - Revision B <http://www.atmel.com/dyn/resources/prod_documents/atmel_tutorial_source.zip>



## See Also

  * [Small Virtual Machines#WinXp_virtual_machine](<Small_Virtual_Machines.md> "Small Virtual Machines") as have **simple** example for Teensy. **Files released here:** [[[1]](<http://turbocontrol.com/simpleteensy.htm>)]

---

_Source: [https://wiki.freepascal.org/TARGET_Embedded](https://web.archive.org/web/20250124214355/https://wiki.freepascal.org/TARGET_Embedded)_
