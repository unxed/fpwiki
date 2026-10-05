# AVR Programming

│ **English (en)** │

The FPC commands for programming AVR microcontrollers such as ATMega or ATTiny are the same commands as always, but because of its character of an embedded system, i.e. no underlaying operating system and direct hardware access, there are several specific topics to know. 

For building the FPC compiler see article [AVR](<AVR.md> "AVR"). 

## Contents

  * 1 LCL/FCL
  * 2 Variables
    * 2.1 Integer variables
    * 2.2 Floating point variables
    * 2.3 String variables
  * 3 Accessing hardware configuration registers
  * 4 Register name aliases
  * 5 Bit arithmetic
  * 6 Classes/objects
  * 7 Interrupts
  * 8 Inline assembler
    * 8.1 Inline asm block
    * 8.2 Asm procedure
  * 9 Sections
    * 9.1 Data
    * 9.2 Code
    * 9.3 Example
  * 10 Resources
    * 10.1 Wiki
    * 10.2 Forums



## LCL/FCL

LCL is not available and FCL only partially. 

## Variables

Variables declaration etc. works in the same way as always. As there is no operating system which provides memory allocation, dynamic types like AnsiString could not be used. 

### Integer variables

The AVR is a 8 bit architecture, the native datatypes are _[Byte](<Byte.md> "Byte")_ (= _uInt8_) with a range from 0 to 255 and _[Shortint](<Shortint.md> "Shortint")_ (= _Int8_) ranging from -128 to 127. Calculations and operations with these two datatypes are always the fastest. A difference in comparison to e.g. target win32 is that the _Integer_ type has 16 bit with range between -32768 and 32767 (instead of 32bit for win32). Using the integer type will increase the calculation time in comparison to the native types, but may be reasonable depending on the application. For loops with iteration of less than 256 cycles the iteration variable with type byte is appropriate. 

For clarity reasons if not using Byte or Integer, the type names Int8, uInt8, Int16, uInt16, Int32, uInt32, Int64 and uInt64 may be prefered. 

### Floating point variables

The AVR architecture doesn’t have a floating point unit. Not sure if using soft float is possible by now. 

### String variables

Only strings with fixed length can be used, as AnsiString and WideString types are dynamically allocated. 

Some Examples: 
    
    
    // passing a string as a procedure variable
    procedure UARTSend(const AText: ShortString);
    // using a variable with fixed length string
    var myString: String[5];
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The string type ShortString is always 255 chars in length. If you need fewer characters, use a string with fixed length to not waste the scarce memory. Use ShortString only if you don't know how long your string may be at maximum length.

## Accessing hardware configuration registers

All hardware configuration registers are defined in the controler specific configuration file which is located in the pascal sources under … \rtl\embedded\avr. The configuration file is named after the device, e.g. Atmega32.pp for the device Atmega32. Besides the configuration registers it also contains the predefined interrupt names. The file is included automatically for all units, so the register variables and constants are available in each unit. The device and thus the configuration file is defined by the command line parameter –wp _devicename_ (e.g. –wpatmega32). 

The names of the hardware registers correspond to the register names defined in the device datasheet. The hardware registers can be accessed as easy as variables. E.g. 
    
    
    UDR:= 25;
    

writes the value 25 in the UARTDataRegister. Bit manipulation can effectively be done with shift operations ([shl](<Shl.md> "Shl") = „shift left“), e.g. 
    
    
    UCSRB:= (1 shl TXEN) or (1 shl RXEN) or (1 shl RXCIE); 
    UCSRB:= (1 shl 3) or (1 shl 4) or (1 shl 7);   
    UCSRB:= %10011000;                             //Bit 3,4 and 7 are set
    

All three lines are identical in the generated code: The bits TXEN, RXEN and RXCIE are set in the „USART Control and Status Register B“, thus the UART hardware is enabled for sending and receiving and for interrupts on receiving. 

## Register name aliases

For hardware abstraction purpose its common to define application specific names for hardware configuration registers, especially for port registers. This can be done by a variable definition using the absolute modifier. In the below example LEDPort is just an other name for the hardware register PortA, such definitions are neutral in memory consumption and performance. 
    
    
    var
      LEDPort: byte absolute PortA;
    const
      LEDPin = 3;
    
    implementation
      LEDPort:= LEDPort or (1 shl LEDPin); //LED=on
    

## Bit arithmetic

Bit arithmetic is the Free Pascal standard, but as it is especially important for an embedded target and quite rarely used in desktop applications it is mentioned here. Available operators for bit manipulation: [Not](<Not.md> "Not"), [And](<And.md> "And"), [Or](<Or.md> "Or"), [Xor](<Xor.md> "Xor"), [Shl](<Shl.md> "Shl") and [Shr](<Shr.md> "Shr"). 

Example: 
    
    
    ByteVal1:= (ByteVal1 and %11110000) or %00001000;
    

A leading ‚[%](<Percent_sign.md> "Percent sign")’ indicates a [binary number](<Binary_numeral_system.md> "Binary numeral system"), a leading ‚[$](<Dollar_sign.md> "Dollar sign")’ a [hexadecimal number](<Hexadecimal.md> "Hexadecimal"). For inline assembler ‚0b’ and ‚0x’ are the indicators respectively. 

## Classes/objects

Again, classes are dynamically allocated, and thus are not available. But static classes (keyword _object_) can be used for structuring and reusing code. 

Example: 
    
    
    Type TUART = object
      Procedure Init;
      Procedure Send(ByteVal:Byte);
    end;
    
    var
      UART: TUART;
    

Usage: 
    
    
    UART.Init;
    UART.Send(125);
    

## Interrupts

When using hardware interrupts one has to configure and enable a interrupt source, enable interrupts globally and define an interrupt service routine (ISR) which is called by the CPU when the interrupt occurs. The configuration is done via the hardware registers. For globally enabling interrupts the assembler command SEI is available, it can be easily used with inline assembler: 
    
    
    asm SEI end;
    

Globally disabling interrupts is done with the assembler command CLI: 
    
    
    asm CLI end;
    

**Note:** Since FPC 3.2.0 one can also clear/set interrupts without resorting to inline assembler: 
    
    
    uses
      intrinsics;
    
    procedure unInterruptableCode;
    begin
      avr_cli;
      // code that shouldn't be interrupted
      avr_sei; 
    end;
    

The code above may introduce the side effect of enabling interrupts, even if disabled initially. To fix this, call avr_save to save the status register (it also disables interrupts), perform uninterrupted actions, then restore status register: 
    
    
    uses
      intrinsics;
    
    procedure unInterruptableCodeImproved;
    var
      prevState: byte;
    begin
      prevState := avr_save;
      // code that shouldn't be interrupted
      avr_restore(prevState);
    end;
    

For definition of the ISRs, predefined names have to be used. Depending on the device, different interrupts are available. The proper names can be found in the device specific unit (e.g. „Atmega32.pp“) and have to be used in the _Alias_ modifier. A working ISR looks as follows (example: External Interrupt 0): 
    
    
    procedure ExternalInterrupt0_ISR; Alias: 'INT0_ISR'; Interrupt; Public;
    begin
      //Do some stuff…
    end;
    

The procedure name itself can be chosen randomly, important is the name behind _Alias_. The _Alias_ modifier forces the compiler to use the name _INT0_ISR_ for the procedure without name mangling. If this name fits a predefined interrupt name, then the procedure will be automatically registered in the interrupt vector table. For the automatism to be possible, the procedure has to be globally visible, which is realised by the Public modifier. But this is only needed if the procedure is defined in a unit. Note: There is no procedure prototype to be defined in the INTERFACE section of the unit, but only the bare procedure in the IMPLEMENTATION section. If the procedure is placed in the program section instead of inside a unit, then the Public modifier is omitted. The Interrupt modifier makes sure that all registers and the status register are saved and recovered by the ISR and that interrupts are further enabled. Omitting the Interrupt modifier will result in the malfunction of and crashes of the program. 

## Inline assembler

In some cases performance can be highly increased by a few assembler statements. The inline assembler provides a very convenient way of mixing Free Pascal and assembler. 

### Inline asm block

The [`ASM` statement](<Asm.md> "Asm") starts an assembler block which is finished with an END statement. 
    
    
    asm
      // do some assembly
    end;
    

In the assembler block modified registers have to be published to the compiler in the end statement, so that the compiler can take care of pushing or avoiding registers. The resulting code is more effective than pushing registers manually inside the asm section. Modified but unpublished registers may lead to malfunction. 
    
    
    asm
      ldi r20,35        // r20 modified
      ldi r21,12        // r21 modified
    end['r20','r21'];   // publish modified registers to compiler
    

  
The register r1 is assumed by the compiler to always be zero. If modified, e.g. by a _mul_ operation, it must be cleared afterwards. 
    
    
      mul r16,r17        // result stored in r1 and r0
      movw r1:r0,r17:r16 
      clr r1             // clear r1
    

  
Constants and variables can be accessed within the inline assembler. The variables have to be distinguished between local variables (stack allocated), which are loaded and stored with the instructions LDD and STD (limited to 64 byte offset), and global variables (statically allocated), which are loaded and stored with the instructions LDS and STS. Variables passed by procedures are allocated on the stack and thus are handled like local variables. Its always useful to check the assembler listing (.s-file extension). 

When using jumps in the assembly then labels are used. They have to be declared in the label section of the procedure. Allowing this, the compiler directive _{$goto on}_ is mandatory. 

Note: The peripheral configuration registers defined in the controller-specific file (e.g. ‚atmega32.pp’) are variables with addresses in the memory address space and NOT in the I/O-space. There the addresses are shifted by 0x20, as the general registers are the bottom of the address space. Thus I/O assembler commands like **in** , **out** , **sbis** , **sbi** (and so on) doesn’t work with the register names. Compare with the memory map in the controller datasheet. 

Instead of **in** /**out** the commands **lds** and **sts** can be used: 
    
    
     lds r16, PORTB   //Reads PORTB into register r16
    

The bit definitions of the perpheral configuration registers can be used in the inline assembler. 

  
If I/O assembler commands are required, the I/O register addresses can be shifted manually by 32 (=0x20): 
    
    
    //Wait until UART data register is empty
    Procedure UARTBusyWait;assembler;
    asm
      .waitBusy:  
        sbis UCSRA+(-32),UDRE
          rjmp .waitBusy 
    end;
    

In the following an example for a delay function with inline assembler, which pauses the program for a certain time making the CPU busy. 
    
    
    unit Wait;     //wait.pas
    {$mode objfpc}{$H+}
    {$goto on}     //Important for using the label directive, the label is needed in the assembler section
    
    INTERFACE
    
    Const
      f_CPU = 4000000; //Hertz
    
    Procedure Wait10m(Time: Byte);
    
    IMPLEMENTATION
    
    // Waitingtime = Time * 10 Milliseconds
    Procedure Wait10m(Time: Byte);
    const
      Faktor = 15 * f_CPU div 1000000;
    label                 // Labels, here for the loop, has to be declared explicitly
      outer, inner1, inner2;
    var
      tmpByte: byte;      // In Inline assembler local variables are accesed by the instructions LDD and STD, global variables are accessed by LDS and STS.
    begin
      asm                 // asm states inline assembly block until the next END statement
        ldd r20,Time      // Variables can be accessed, here a local variable
        outer:
          ldi r21, Faktor
          inner1:         // 640*Faktor= 640*15*1 = 9600 cycles/1MHz
            ldi r22,128
            inner2:       // 5*128 = 640 cycles
              nop         // 1 cycle
              nop         // 1 cycle
              dec r22     // 1 cycle
            brne inner2   // 2 cycles in case of branch //one loop in sum 5 cycles
            dec r21
          brne inner1
          dec r20
        brne outer
      end['r20','r21','r22']; //Used registers to be published to compiler
    end;  //procedure
    
    end.  //unit
    

### Asm procedure

There is a second way to use inline assembler by defining a whole procedure or function as assembler. 
    
    
    Procedure DoSomeAssembly;assembler;  
    asm  
      // do some assembly
    end;
    

But beware, the procedure/function is called according the ABI conventions (Application binary interface). For this reasons an asm block is much easier to use than an asm procedure. For example on the target AVR according to the ABI some parameters may be passed via a register. Then the programmer has to consider this. There is no management code inserted by the compiler to access these parameters as local variable as it is done in 'normal' procedures with an asm block included. Also in opposite to the asm block the programmer has to take care of the used registers himself. Only for local variables (if defined) the compiler will insert management code for the stack frame. 

## Sections

### Data

Data (variables) can be placed in different linker sections by using the _section_ directive. Care should be taken when placing data in _.progmem_ or _.eeprom_ sections, since these sections do not map to the data space for classical AVR controllers and the compiler currently doesn't generate correct code to access these sections. An example could be to place a variable in the _.noinit_ section. This would place the variable in uninitialized RAM, thus a soft restart of the controller would preserve its value. 

Note: the _.noinit_ section requires fpc 3.3.1 to work properly. 

### Code

Support to associate section information with procedures and functions has been implemented in the compiler (currently only available in the development version 3.3.1). The startup code has been modified (so far only for the avrsim controller) to follow more or less the [avr-libc .initN sections](<https://www.nongnu.org/avr-libc/user-manual/mem_sections.html#sec_dot_init>) convention: 

  * entry point - First instruction jumps to the linker provided label __trampolines_end. After the jump instruction follows the interrupt vector table. This code is different for each controller.
  * Progmem data (if defined) is placed next. The __trampolines_end label points to the first address after this location.
  * .init0 - not used, available for user code
  * .init1 - not used, available for user code
  * .init2 - Code to initialize zero register and stack pointer
  * .init3 - not used, available for user code
  * .init4 - Code to copy initialized variables to RAM and zero memory for uninitialized variables.
  * .init4 - .init8 - not used, available for user code
  * .init9 - Jump to main



Note that the finalization code generated by FPC follows directly after the rest of the code, before the .finiN sections start. Currently it isn't possible to replace FPC generated finalization code or insert code via a .fini section that would execute after the finalization code. 

### Example
    
    
    program test;
    
    // Store MCU startup status early and set to 0 so that e.g. watchdog resets can be determined
    
    var
      startupMCUSR: byte; section '.noinit';
    
    // This should be marked noreturn since it should fall through into the next init section
    procedure saveMCUSR; noreturn; section '.init1';
    begin
      startupMCUSR := MCUSR;
      MCUSR := 0;
    end;
    
    begin
      if startupMCUSR = 1 then
        writeln('Reset cause: Power-on');
    end.
    

## Resources

### Wiki

  * [Embedded Portal](<Portal_Embedded.md> "Portal:Embedded")
  * [Arduino](<Arduino.md> "Arduino")
  * [ARM](<ARM.md> "ARM")
  * [AVR](<AVR.md> "AVR")



### Forums

  * [Tutorial for building FPC-AVR and _blinky_ example project](<http://forum.lazarus.freepascal.org/index.php/topic,30960.0.html>)
  * [Forum topic with AVR Project Wizard](<http://forum.lazarus.freepascal.org/index.php/topic,35486.msg>)

---

_Source: [https://wiki.freepascal.org/AVR_Programming](https://web.archive.org/web/20250124214114/https://wiki.freepascal.org/AVR_Programming)_
