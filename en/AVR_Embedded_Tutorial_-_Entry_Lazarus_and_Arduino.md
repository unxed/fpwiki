# AVR Embedded Tutorial - Entry Lazarus and Arduino

│ **[Deutsch (de)](</AVR_Embedded_Tutorial_-_Entry_Lazarus_and_Arduino/de> "AVR Embedded Tutorial - Entry Lazarus and Arduino/de")** │  **English (en)** │ 

## Contents

  * 1 Build Lazarus with AVR crosscompiler for Arduino Nano/Uno
  * 2 Tools required
    * 2.1 Software
    * 2.2 Hardware
  * 3 Install FPCUPdeluxe
  * 4 Build Lazarus with FPCUPdeluxe
    * 4.1 Build trunk
    * 4.2 Build AVR Cross compiler
  * 5 Install drivers
    * 5.1 Windows
    * 5.2 Linux
  * 6 Prepare Lazarus
    * 6.1 Start with F9
    * 6.2 Code tools
    * 6.3 Readable ASM code creation
    * 6.4 Faulty optimization
  * 7 First project
    * 7.1 Create a new project
  * 8 Start project
  * 9 Different sources for getting started
    * 9.1 Pin 13 blinker
    * 9.2 Serial input/output
    * 9.3 Simple timer interrupt
  * 10 Reinstalling the OS
  * 11 Conclusion
  * 12 See also



## Build Lazarus with AVR crosscompiler for Arduino Nano/Uno

[![mini](https://wiki.freepascal.org/images/3/3a/lazarus-avr.png)](</File:lazarus-avr.png> "mini")

The tutorial was tested with: 

  * Linux Mint 18.2 64bit
  * Arduino IDE 1.8.5
  * FPCUPdeluxe V1.6.6e (Should always be the newest!)
  * svn 1.9.3
  * FPC 3.1.1 (created with FPCUPdeluxe)
  * Lazarus 1.9 (created with FPCUPdeluxe)



The same tools are required under Windows , except the Linux Mint . 

## Tools required

### Software

FPCUPdeluxe: 

  * <https://github.com/newpascal/fpcupdeluxe/releases> \- fpcupdeluxe for creating the AVR cross compiler. (Download: fpcupdeluxe- [OS]).



Arduino software: 

  * <https://www.arduino.cc/en/Main/Software> \- Arduino software. Used for avrdude.



Subversion: 

  * Ubuntu / Debian:


    
    
     sudo apt-get install subversion
    

  * SuSE:


    
    
     sudo zypper install subversion
    

  * Windows:



Should be installed automatically by FPCUPdeluxe, otherwise download and install from [here](<https://sourceforge.net/projects/win32svn/?source=typ_redirect>). 

### Hardware

  * Arduino Uno or Nano, preferably with an ATmega328p.



## Install FPCUPdeluxe

It is best to copy the download of FPCUPdeluxe into a new empty folder and then start the copied file in the file manager with a double click. Linux asks if the file should be made executable, answer yes. The FPCUPdeluxe GUI should then start. 

## Build Lazarus with FPCUPdeluxe

### Build trunk

  * Set the top left list boxes **FPC version** and **Lazarus version** to **trunk** or **embedded** which are both in the **Basic tab**.
  * Build the Lazarus base trunk/embedded version with `Install/update FPC+Laz`.



If everything goes through without errors you get the current trunk from FPC and Lazarus. Normal desktop applications can now be created with this. There should be an icon for Lazarus_fpcupdeluxe on the Desktop. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Note: SVN repositories may experience difficulties. If so, don't give up, just try again later ;-) What also helps, run FPCUPdeluxe twice in a row. A non-current FPCUPdeluxe can also cause difficulties. If something doesn't work, you can ask in the [Lazarus forum](<https://forum.lazarus.freepascal.org/index.php/topic,34645.0.html>), the author is very helpful.

### Build AVR Cross compiler

Press the `Setup+` button in the **Cross tab** to open a dialog with further options. Set the following there: 

  * CPU: avr
  * OS: embedded
  * Cross Build Options Overide: -Cpavr5
  * Cross SubArch Overide: avr5
  * Close dialog with Ok



Select the following check boxes in the **Cross tab** : 

  * CPU: avr
  * OS: embedded



Press the `Install compiler` button and the cross compiler will be built. If everything went correctly, you can exit from FPCUPdeluxe. 

## Install drivers

### Windows

  * For original Arduinos, the drivers are supplied with the Arduino IDE.
  * For clones it may be necessary to install drivers. Mostly this is the 340-341 driver. From Windows 10, these drivers are also included with the operating system.



### Linux

  * With current Linux, eg Mint/Ubuntu/Debian the drivers are already installed. (SusE: someone would need to test.)



Most of the time, the COM ports (USB-UART) have to be accessible by users. This is done with the following command: 
    
    
     sudo adduser $USER dialout
    

## Prepare Lazarus

### Start with `F9`

If you try to start with `F9`, you always get an error message. You can prevent this. Change the following in Lazarus: 

  * "Tools -> Settings ... -> Keyboard layout -> Commands from the menu 'Start' -> Start program", set to Unknown .
  * "Tools -> Settings ... -> Keyboard layout -> Commands from the menu 'Start' -> Start without debugger", remove the ticks from Shift and Ctrl.



So you can upload and start normally with `F9`. 

But you have to consider that the debugger is also disabled if you then write a normal PC program with Lazarus! 

### Code tools

It is possible that the code tools cannot find the Atmega328p unit. If this happens, then the following step is necessary: 

  * "Tools -> Reread FPC source code directory"



### Readable ASM code creation

If you want to see the source as assembler code, the following step is necessary: 

  * Enter "Project -> Project Settings ... -> User-defined settings -> User-defined settings" -al , this creates an *.s file that contains the program as ASM code.



### Faulty optimization

I have experienced a program not working properly because the compiler incorrectly optimised the code. So you should switch off code optimization with {$ O-} at the beginning of the program. 

## First project

### Create a new project

  * "File -> New ... -> Simple Program" - create a new project.
  * "Project -> Project Settings ... -> Configuration and Goals" - set the following: 
    * Target operating system (-T): Embedded
    * Target CPU family (-P): avr
    * Target processor (-Cp): AVR5
  * "Project -> Project Settings ... -> Debug" : Remove the check mark "Generate debugger information for GDB" .
  * "Project -> Project settings ... -> User-defined settings -> User-defined settings still enter -WpATMEGA328P .
  * "Project -> Project Settings ... -> Compiler Commands -> Execute After" 
    * Remove the Compile and Recompile ticks.
    * Enter the following into the command: `/home/tux/Programme/arduino-1.8.5/hardware/tools/avr/bin/avrdude -C/home/tux/Programme/arduino-1.8.5/hardware/tools/avr/etc/avrdude.conf -v -patmega328p -carduino -P/dev/ttyUSB0 -b57600 -Uflash:w:Project1.hex:i `
    * When using a [USB ISP-Programmer Stick for AVR](<https://www.diamex.de/dxshop/DIAMEX-USB-ISP-Programmer-Stick-fuer-AVR>) and installing AVRdude WITHOUT Arduino, the command above is recommended as follows: `/usr/bin/avrdude -cstk500v2 -pm16 -P/dev/ttyACM0 -b57600 -Uflash: w:$Path($(OutputFile))$NameOnly($(TargetFile)). hex` _ATTENTION - The µC used for the command above is an ATMega16_
    * or see [Various programmers](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial").



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Note: Arduino nanos have been in circulation with new bootloaders since the beginning of 2018. It may be enough to change this parameter: -b115200

More information in the [Arduino Forum](<https://forum.arduino.cc/index.php?topic=542414.0>). 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Note: The paths still have to be adapted to your own installation.

Windows: It must be avrdude with ".exe", "avrdude" alone is not enough, even if it is with Arduino without .exe. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Note: The Com-Port is called differently under Linux and Windows: 

  * Linux: /dev/ttyUSBx or /dev/ttyACMx
  * Windows: COMx



As a recommendation, I would take this project as a template and build all new projects on top of it. Setting the whole thing up for a new project each time is a bit tedious. 

## Start project

Nothing should now be standing in the way of your first blink program (source in the next section). Compile and upload the program with `F9`. If everything worked, the LED at pin 13 should flash about every second. 

## Different sources for getting started

### Pin 13 blinker
    
    
    program Project1;
     const
       BP5 = 5;      // Pin 13 of the Arduino
       dl = 250000;  // About 1/2 sec
    
       procedure delay(t : int32);
       var
         i : Int32;
       begin
         for i := 0 to t do begin
           asm
             nop;
           end;
         end;
       end;
    
     begin
       DDRB := DDRB or (1 shl BP5);      // switch port to output
       repeat
         PORTB := PORTB or (1 shl BP5);  // port to high
         delay(dl);                      // waiting
    
         PORTB := PORTB and not (1 shl BP5);  // port on low
         delay(dl);                           // waiting
       until 1 = 2;                           // infinite loop
     end.
    

### Serial input/output

A terminal with the following settings is required for serial output: 

  * Baud rate: 9600
  * Bits: 8
  * Stop bits: 1
  * Parity: none


    
    
    program Project1;
     const
       CPU_Clock = 16000000;  // Arduino clock frequency, default 16MHz.
       Baud = 9600;           // baud rate
       Divider = CPU_Clock div(16 * baud) - 1;
    
       procedure UARTInit;
       begin
         UBRR0 := divider ;
    
         UCSR0A := (0 shl U2X0);
         UCSR0B := (1 shl TXEN0) or (1 shl RXEN0);
         UCSR0C := (% 011 shl UCSZ0);
       end;
    
       procedure UARTSendChar(c : char);
       begin
         while UCSR0A and (1 shl UDRE0) = 0 do begin
         end;
         UDR0 := byte(c);
       end;
    
       function UARTReadChar : char;
       begin
         while UCSR0A and (1 shl RXC0) = 0 do begin
         end;
         Result := char(UDR0);
       end;
    
       procedure UARTSendString(s : ShortString);
       var
         i : integer;
       begin
         for i := 1 to length(s) do begin
           UARTSendChar(s[i]);
         end;
       end;
    
     begin
       UARTInit;
       repeat
         if UARTReadChar = #32 then begin
           UARTSendString('Hello World!'#13#10);
         end;
       until 1 = 2;
     end.
    

  


### Simple timer interrupt

Pin 13 is switched to HIGH every second. 
    
    
    program Project1;
     const
       BP5 = 5;  // Pin 13 of the Arduino
    
       procedure sei;  assembler;
       asm
         Sei
       end;
    
       procedure Timer2_Interrupt;  alias : 'TIMER2_OVF_ISR';  interrupt;  public;
       const
         counter : integer = 0;
         cl = 16000000 div 1024 div 256 div 2;  // 16'000'000Hz / Clock / TCNT / 2 = 0.5sec
       begin
         Inc(counter);
         if count = cl then begin
           PORTB := PORTB or (1 shl BP5);
         end;
         if count > = cl * 2 then begin
           PORTB := PORTB and not (1 shl BP5);
           count := 0;
         end;
       end;
    
     begin
       DDRB := DDRB or (1 shl BP5) ;  // Pin 13 output
    
       TCCR2A := %00;             // Normal timer
       TCCR2B := % 111;           // clock / 1024
       TIMSK2 := (1 shl TOIE2);   // enable timer2 overflow interrupt
       sei();                     // turn on interrupts
    
       repeat
         // do something
       until 1 = 2;
     end.
    

## Reinstalling the OS

  * Linux: If you reinstall the OS, it is not enough to back up the cross-compiler. Remedy: Run FPCUdeluxe again.
  * Windows: Unknown.



## Conclusion

For those who have already programmed with the Arduino IDE, they will have certainly found that there are a lot of ready-made libraries. You can code something very easily and quickly, but there is a lot hidden there. 

With Lazarus, you have to do everything yourself and thus deal with the registers of the AVR microcontroller. But this also has a big advantage, you know what you are doing. You can also generate more optimized code in this way. 

Thank you 

The most important thing at the end: 

  * A big thank you to the developer of FPCUPdeluxe!

  * Without this ingenious tool, AVR with Lazarus would still be a forceps delivery!



## See also

  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Overview

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Entry_Lazarus_and_Arduino](https://web.archive.org/web/20240914150348/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Entry_Lazarus_and_Arduino)_
