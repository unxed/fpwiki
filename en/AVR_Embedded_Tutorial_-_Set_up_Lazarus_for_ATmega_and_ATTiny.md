# AVR Embedded Tutorial - Set up Lazarus for ATmega and ATTiny

│ **English (en)** │

## Contents

  * 1 General information about naked AVRs
  * 2 ATmega328p
    * 2.1 Load ATmega328p library
    * 2.2 Install ATmega328p library
    * 2.3 Prepare ATmega328p
  * 3 More ATmega AVR
  * 4 ATmega2560 (Arduino Mega)
  * 5 ATtiny
  * 6 Other AVR
  * 7 Final word
  * 8 See also



# General information about naked AVRs

The prerequisite is that you have read through Lazarus and Arduino: 

  * [Getting started with Lazarus and Arduino](<AVR_Embedded_Tutorial_-_Entry_Lazarus_and_Arduino.md> "AVR Embedded Tutorial - Entry Lazarus and Arduino") \- How do I set up Lazarus for the Arduino ..



Since these AVRs need a boot loader, the best thing to do is to use the Arduino IDE to burn it. At the same time you can also see which parameters are needed for the AVRDude call. 

# ATmega328p

The procedure is almost the same as for the Arduino Due/Nano but there are some differences. The Arduino IDE must first be prepared for a naked ATmega328. To do this, open the Arduino IDE and set the following: 

## Load ATmega328p library

  * "File -> Preferences -> Settings -> Detailed output during:" - Checks when uploading.
  * "File -> Preferences -> Settings -> Additional Board Administrator URLs:" - Click on the small button with 2 windows.
  * Insert/add line: <https://raw.githubusercontent.com/carlosefr/atmega/master/package_carlosefr_atmega_index.json>



## Install ATmega328p library

  * Search for "Tools -> Board: xxx / Board Administrator" - Barebones ATmega Chips ...
  * Install the latest version.



## Prepare ATmega328p

  * "Tools -> Board: -> ATmega328 / 328p"
  * "Tools -> Processor -> ATmega328p"
  * "Tools -> Clock" - the right clock. (Note if you have an external crystal oscillator)
  * Select "Tools -> Port " - USB.
  * "Tools -> Programmer -> Ardunino as ISP" or "Tools -> Programmer -> aspUSB" - more details here: 
    * [AVR ISP programmer](<AVR_Embedded_Tutorial_-_Various_programmers.md> "AVR Embedded Tutorial - Various programmers")
  * "Tools -> Burn Bootloader"



The ATmega328p is thus prepared for Lazarus. 

If you then upload the sketch with the green arrow, you get detailed output. Approx. the 4th line shows how AVRDude is called. From the content of this line you can see which parameters you need for Lazarus. 

For me it looks like this: 
    
    
     /home/tux/Programme/arduino-1.8.5/hardware/tools/avr/bin/avrdude -C /home/tux/Program/arduino-1.8.5/hardware/tools/avr/etc/avrdude.conf -v -patmega328p -cstk500v1 -P / dev / ttyUSB0 -b19200 -Uflash: w: Project1.hex: i 
    

This AVRDude line is also the only thing you have to adjust in Lazarus. 

See also: [Lazarus and Arduino](<AVR_Embedded_Tutorial_-_Entry_Lazarus_and_Arduino.md> "AVR Embedded Tutorial - Entry Lazarus and Arduino"). 

# More ATmega AVR

The procedure is almost the same as for the ATmega328p. 

The differences: 

  * Find the right **json** and add it to the board management: 
    * <https://github.com/arduino/Arduino/wiki/Unofficial-list-of-3rd-party-boards-support-urls>
  * For Lazarus, adjust the AVRDude line for compiler commands.
  * "Project -> Project settings ... -> User-defined settings -> User-defined settings -> All settings -> -Wp <x>" , select the appropriate ATmega. (-Wp <x> is pretty far down).
  * "Project -> Project Settings ... -> Configuration and Targets -> Target Processor (-Cp):" - **AVR5**



Otherwise it is identical to the ATmega328p. 

There are ATmegas which belong to another family, like the ATmega2560, but you also have to adapt Lazarus with [FPCUpDeluxe](<fpcupdeluxe.md> "fpcupdeluxe"). 

# ATmega2560 (Arduino Mega)

Although this is also an ATmega, it differs from the ATmega328. The procedure is almost the same as with the ATmega328. 

The differences: 

If you want to program the ATmega2560 with Lazarus, you have to set up the cross compiler differently. To do this, you have to change these parameters in fpcupdeluxe and `Setup+` : 

  * Cross Build Options Override: -Cpavr6
  * Cross SubArch Override: avr6



See also the next section. 

# ATtiny

The procedure is almost the same as with the ATmega. 

The differences: 

If you want to program ATtiny with Lazarus, you have to set the cross compiler differently. To do this, you have to change these parameters in fpcupdeluxe and Setup + : 

  * Cross Build Options Override: **-Cpavr25**
  * Cross SubArch Override: **avr25**



See also the next section. 

# Other AVR

The following values ​​are suitable for `Setup+` for: 

  * 25: ATtiny13, ATtinyx313, ATtinyx4, ATtinyx5.
  * 5: ATmega128, ATmega328 (Ardunio Uno / Nano).
  * 6: ATmega2560 (Arduino Mega)



Here is a list of other parameters for other AVRs: <https://gcc.gnu.org/onlinedocs/gcc/AVR-Options.html>

Please note that not all AVRs are supported by the Free Pascal Compiler. 

# Final word

Don't forget the following: 

"Project -> Project Settings ... -> Configuration and Goals -> Target Processor (-Cp):" ??? 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Unfortunately, the whole thing has a shortcoming: You have to compile the cross compiler for every AVR family! A Lazarus for everyone is not possible!

# See also

  * Overview page - [AVR Embedded Tutorial](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial")

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Set_up_Lazarus_for_ATmega_and_ATTiny](https://web.archive.org/web/20250406090723/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Set_up_Lazarus_for_ATmega_and_ATTiny)_
