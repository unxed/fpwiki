# AVR Embedded Tutorials

│ **[Deutsch (de)](</AVR_Embedded_Tutorials/de> "AVR Embedded Tutorials/de")** │  **English (en)** │ 

## Contents

  * 1 AVR embedded tutorials
  * 2 Set up cross compiler/IDE
  * 3 AVR programming examples
  * 4 AVR tutorials
    * 4.1 Software
    * 4.2 Hardware
    * 4.3 Communication
    * 4.4 External modules
  * 5 See also



## AVR embedded tutorials

This is an overview of tutorials for programming AVR microcontrollers with Free Pascal and Lazarus. This includes various ATtiny and ATmega microcontrollers. Most of the examples also run on an Arduino with an ATmega; especially the Uno/Nano. The Arduino-Mega can also be programmed. Basically, all AVR microcontrollers are programmed more or less the same way. Usually only the registers differ a little. 

## Set up cross compiler/IDE

Building the cross compiler and setting up the Lazarus IDE: 

  * [Getting started Lazarus and Arduino (Uno/Nano)](<AVR_Embedded_Tutorial_-_Entry_Lazarus_and_Arduino.md> "AVR Embedded Tutorial - Entry Lazarus and Arduino") \- How do I set up Lazarus to program an Arduino (AVR - Cross compiler).
  * [Set up Lazarus for ATtiny and ATmega](<AVR_Embedded_Tutorial_-_Set_up_Lazarus_for_ATmega_and_ATTiny.md> "AVR Embedded Tutorial - Set up Lazarus for ATmega and ATTiny") \- Lazarus cross compilation for additional Arduino/AVRs.
  * Tool for creating and modifying AVR/Arduino projects: [Embedded GUI Package](<https://github.com/sechshelme/Lazarus-Embedded/tree/master/Lazarus_Embedded_GUI_Package>) \- sechshelme (external link).
  * [Various programmers](<AVR_Embedded_Tutorial_-_Various_programmers.md> "AVR Embedded Tutorial - Various programmers") \- hardware connections to flash the AVR.



## AVR programming examples

  * [sechshelme examples](<https://github.com/sechshelme/Lazarus-Embedded>) (external)
  * [crrause examples](<https://github.com/ccrause/fpc-avr>) (external)



## AVR tutorials

### Software

  * [AVR Programming](<AVR_Programming.md> "AVR Programming") \- Important basics and special features for programming AVR.
  * [Libraries](<AVR_Embedded_Tutorial_-_Library.md> "AVR Embedded Tutorial - Library") \- Units in AVR programming.
  * [Delay](<AVR_Embedded_Tutorial_-_Delays.md> "AVR Embedded Tutorial - Delays") \- Waiting routines (delay/sleep).
  * [Multiplex](<AVR_Embedded_Tutorial_-_Multiplex.md> "AVR Embedded Tutorial - Multiplex") \- Multiplex based on a 4-digit 7-segment display.
  * [Integer to digits](<AVR_Embedded_Tutorial_-_Int_to_digits.md> "AVR Embedded Tutorial - Int to digits") \- Output an integer to digits (7-segment display).
  * [Random Number Generator](<AVR_Embedded_Tutorial_-_Random.md> "AVR Embedded Tutorial - Random") \- A random number generator is quite complex on an AVR.



### Hardware

  * [GPIO - Out / In](<AVR_Embedded_Tutorial_-_Simple_GPIO_on_and_off_output.md> "AVR Embedded Tutorial - Simple GPIO on and off output") \- How do I access the GPIO on the AVR?
  * [GPIO - Interrupt](<AVR_Embedded_Tutorial_-_GPIO-Interrupt.md> "AVR Embedded Tutorial - GPIO-Interrupt") \- Use of GPIO interrupts and pin change.
  * [Timers/Counters](<AVR_Embedded_Tutorial_-_Timer,_Counter.md> "AVR Embedded Tutorial - Timer, Counter") \- Use of hardware timers.
  * [Analog Write/PWM](<AVR_Embedded_Tutorial_-_Analog_Write.md> "AVR Embedded Tutorial - Analog Write") \- Analog output using pulse width modulation (PWM)
  * [Analog Read](<AVR_Embedded_Tutorial_-_Analog_Read.md> "AVR Embedded Tutorial - Analog Read") \- Read the analog pin.
  * [EEPROM](<AVR_Embedded_Tutorial_-_EEPROM.md> "AVR Embedded Tutorial - EEPROM") \- Save and read data to/from an EEPROM.



### Communication

  * [UART](<AVR_Embedded_Tutorial_-_UART.md> "AVR Embedded Tutorial - UART") \- Serial input and output via UART (COM port).
  * [SPI](<AVR_Embedded_Tutorial_-_SPI.md> "AVR Embedded Tutorial - SPI") \- Use of the hardware SPI interface with an ATmega328 / Arduino.
  * [SPI slave](<AVR_Embedded_Tutorial_-_SPI-Slave.md> "AVR Embedded Tutorial - SPI-Slave") \- Use SPI as a slave.
  * [I²C / TWI](<AVR_Embedded_Tutorial_-_I²C,_TWI.md> "AVR Embedded Tutorial - I²C, TWI") \- Communication with I²C / TWI, hardware controlled.
  * [Software I²C/TWI](<AVR_Embedded_Tutorial_-_Software_I2C,_TWI.md> "AVR Embedded Tutorial - Software I2C, TWI") \- Communication with I²C / TWI, software controlled.



### External modules

  * [Shift registers](<AVR_Embedded_Tutorial_-_Shiftregister.md> "AVR Embedded Tutorial - Shiftregister") \- How do I control shift registers?
  * [Control I²C EEPROM](<AVR_Embedded_Tutorial_-_I²C_EEPROM.md> "AVR Embedded Tutorial - I²C EEPROM") \- I²C EEPROM (24LC256).
  * [I²C external clock](<AVR_Embedded_Tutorial_-_I²C_External-Clock.md> "AVR Embedded Tutorial - I²C External-Clock") \- External clock module DS3231.
  * [SPI shift register](<AVR_Embedded_Tutorial_-_SPI_Shiftregister.md> "AVR Embedded Tutorial - SPI Shiftregister") \- Control a 74HC595 shift register via SPI.
  * [Control SPI MCP4922](<AVR_Embedded_Tutorial_-_SPI_MCP4922.md> "AVR Embedded Tutorial - SPI MCP4922") \- Control 12-bit DAC MCP4922 via SPI.
  * [ADS1115](<AVR_Embedded_Tutorial_-_ADS1115.md> "AVR Embedded Tutorial - ADS1115") \- Control the 16-bit converter ADC1115 via I²C.



## See also

  * [AVR](<AVR.md> "AVR") \- Cross compiler with make
  * [AVR Programming](<AVR_Programming.md> "AVR Programming") \- Important basics and special features for programming AVR Embedded/Arduino
  * [Arduino](<Arduino.md> "Arduino") \- Communication with an Arduino
  * [ARM Embedded Tutorials](<ARM_Embedded_Tutorials.md> "ARM Embedded Tutorials") \- Tutorials for ARM Embedded/STM32 and Raspberry Pi Pico/RP2040.

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorials](https://web.archive.org/web/20250124213255/https://wiki.freepascal.org/AVR_Embedded_Tutorials)_
