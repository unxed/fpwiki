# AVR Embedded Tutorial - Various programmers

│ **English (en)** │

## Contents

  * 1 General
    * 1.1 Arduino IDE
    * 1.2 Lazarus IDE
    * 1.3 Install Avrdude
      * 1.3.1 Linux
    * 1.4 Shortened parameters
  * 2 Programming via USB
  * 3 Programming via ISP
    * 3.1 USBasp
      * 3.1.1 Linux
      * 3.1.2 Windows
    * 3.2 Arduino ISP
    * 3.3 Release the USB port
  * 4 See also



## General

### Arduino IDE

The easiest way is to determine the necessary parameters for avrdude is to first program the appropriate AVR with the Arduino IDE. 

Please refer to [Set up Lazarus for ATmega and ATTiny](<AVR_Embedded_Tutorial_-_Set_up_Lazarus_for_ATmega_and_ATTiny.md> "AVR Embedded Tutorial - Set up Lazarus for ATmega and ATTiny"). 

When programming the AVR for the first time, a boot loader is needed and this is best burned with the Arduino IDE. 

If you want to change the clock frequency or other parameters, you also have to re-burn the boot loader so that these FUSE parameters are also re-written. 

### Lazarus IDE

The parameters which you determined using the Arduino IDE are entered in Lazarus under: 

  * "Project > Project settings ... > Compiler commands > Execute after" is entered.



The parameters look something like this for an Arduino Nano: 
    
    
    /home/tux/Programme/arduino-1.8.5/hardware/tools/avr/bin/avrdude -C/home/tux/Programme/arduino-1.8.5/hardware/tools/avr/etc/avrdude.conf -v -patmega328p -carduino -P/dev/ttyUSB0 -b57600 -Uflash:w:Project1.hex:i
    

### Install Avrdude

If you install avrdude, you can shorten the starting parameter list for Lazarus a lot. 

#### Linux
    
    
      sudo apt-get install avrdude
    

Note: When it comes to Linux package management, avrdude is usually somewhat older than the current version. Usually this is not a problem unless you are using brand new hardware. 

### Shortened parameters

Since avrdude is in the search path, you can save the entire path specification. The line with avrdude.conf is also not required, since the default avrdude.conf is used, with Linux this is located at /etc/avrdude.conf. 
    
    
     avrdude -v -patmega328p -carduino -P/dev/ttyUSB0 -b57600 -D -Uflash:w:Project1.hex:i
    

## Programming via USB

Most Arduinos are programmed via USB which looks like this for the following Arduinos: 

  * Arduino UNO: 
    * avrdude -v -patmega328p -carduino -P/dev/ttyACM0 -b115200 -Uflash:w:Project1.hex:i
  * Arduino Nano, old boot loader (available on most Chinese clones): 
    * avrdude -v -patmega328p -carduino -P/dev/ttyUSB0 -b57600 -Uflash:w:Project1.hex:i
  * Arduino Nano, new boot loader (will be used from early 2018): 
    * avrdude -v -patmega328p -carduino -P/dev/ttyUSB0 -b115200 -Uflash:w:Project1.hex:i



## Programming via ISP

Arduinos can also be programmed via ISP where the Arduino is connected to an ISP programmer via the 6-pin header. With naked AVR chips, this is done in the same way, only you have to solder the 6-pin connector yourself. 

### USBasp
    
    
     Atmega328p or Arduino Nano, with an aspISP programmer. 
    

#### Linux
    
    
      avrdude -v -patmega328p -cusbasp -Pusb -Uflash:w:Project1.hex:i
    

#### Windows

The following driver must be installed <https://zadig.akeo.ie/> \- USBasp driver 
    
    
     [Path]\avrdude.exe -v -patmega328p -cusbasp -Pusb -Uflash:w:Project1.hex:i
    

### Arduino ISP

Homemade ISP programmer with an Arduino, according to these tutorials: 

  * <https://flazer.de/blog/14/arduino-atmel-flash-station> \- with control LEDs
  * <https://playground.boxtec.ch/doku.php/arduino/attiny> \- without control LEDs, or search for "arduino attiny isp" .
  * ATTiny2313 with modified Arduino as an ISP programmer.


    
    
      avrdude -v -pattiny2313 -cstk500v1 -P / dev / ttyUSB0 -b19200 -Uflash:w:Project1.hex: i
    

### Release the USB port

The USB port may be blocked which causes the following error to occur: 
    
    
     avrdude: Warning: cannot open USB device: Permission denied
     avrdude: error: could not find USB device with vid=0x16c0 pid=0x5dc vendor='www.fischl.de' product='USBasp'
    

With Ubuntu-based Linux you can fix this by installing AVRdude as follows: 
    
    
     sudo apt-get install avrdude
    

Note: a PC restart may be necessary. 

## See also

  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorials.md> "AVR Embedded Tutorials") \- Overview

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Various_programmers](https://web.archive.org/web/20250403151535/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Various_programmers)_
