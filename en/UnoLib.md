# UnoLib

## About

UnoLib is set of routines translated to Pascal from Arduino sources dedicated for Arduino Uno (ATMega328p). Although the library is distributed with AVRPascal, it can be used independently by for example FPC IDE or Lazarus with FPC built for use with AVR microcontrollers. 

[![UnoLib.png](https://wiki.freepascal.org/images/b/b5/UnoLib.png)](</File:UnoLib.png>)

Written by @ackarwow in collaboration with @Dzandaa (from 2024) 

## Downloads

UnoLib releases are available on SourceForge: <https://sourceforge.net/projects/unolib/>

Current changes to UnoLib are on GitHub: <https://github.com/ackarwow/unolib>

AVRPascal editor: [http://akarwowski.pl/index.php?page=electronics&lang=en](<http://akarwowski.pl/index.php?page=electronics&lang=en>)

## Modules

The library modules are located in the "lib" directory: 

  * analog.pas - support for analog pins
  * defs.pas - definitions of constants, bit manipulations, port support
  * dht.pas - support for DHT11/22 sensors
  * digital.pas - support for digital pins
  * ds1302rtc.pas - support for ds1302 real time clock
  * fix16.pas - support for fixed point numbers
  * float32.pas - support for floating point numbers, basic arithmetic and trigonometric operations (in collaboration with @Dzandaa)
  * hardwareserial.pas - support for serial communication
  * i2c.pas - support for I2C communication bus (by @Dzandaa, many thanks to @ccrause)
  * liquidcrystal.pas - support for LCD
  * stringutils.pas - string conversion routiones (by @Dzandaa)
  * timer.pas - time-related routines



The "examples" directory contains simple programs using UnoLib modules: 

  * DS137ZN_RTC_Test.pas - Real Time Clock test using I2C (by @Dzandaa)
  * HMC5883L_Magnetometer_Test.pas - HMC5883L Magnetometer test using I2C (by @Dzandaa)
  * I2CScan.pas - I2C bus scan (by @Dzandaa)
  * pcf8591t_ACDC_Read.pas - ACDC read test (by @Dzandaa)
  * pcf8591t_ACDC_Write.pas - ACDC write test (by @Dzandaa)
  * TestBlink.pas - turns on and off the built-in LED
  * TestBlinkWithoutDelay.pas - turns on and off the built-in LED using Millis
  * TestDHT11.pas - displays information about the temperature and humidity of the air from the DHT11 sensor on an external LCD display
  * TestDigital.pas - turns on and off the built-in LED based on the button state
  * TestLCAutoscroll.pas - scrolls text on the LCD display
  * TestLCBlink.pas - displays the text "hello, world!" on the LCD display
  * TestLCChars.pas - displays non-standard characters on the LCD display
  * TestLCCursor.pas - turns the cursor on and off on the LCD display
  * TestLCDisplay.pas - displays and turns off the text "hello, world!"
  * TestLCSerialDisplay.pas - displays characters taken from the serial port on the LCD display
  * TestLCTextDirection.pas - changes the direction of text display on the LCD display
  * TestLM35.pas - displays the temperature value from the LM35 sensor on the LCD display
  * TestSerial.pas - sends and receives data via the serial port



Some extra code is placed in "extras" directory: 

  * ccrause_blink_leonardo.pas - blink example for Arduino Leonardo based on code written by @ccrause
  * ccrause_delay.pas - delay module based on code written by @ccrause
  * cdc.pas - CDC support for Arduino Leonardo
  * usb.pas - USB support for Arduino Leonardo
  * wdt.pas - watchdog timer routines



Lazarus test projects are placed in "tests" directory: 

  * conversion - routines for numeric and string conversion and data preparation for CORDIC algorithm (by @Dzandaa with collaboration of @ackarwow)
  * float32 - test programs for testing TFloat32 type both on Lazarus and on Arduino side (based on serial port communication program by @Dzandaa)
  * serial - serial port communication test (by @Dzandaa)



Before compiling a given program, please make sure that the library modules listed in the uses section have already been compiled.

---

_Source: [https://wiki.freepascal.org/UnoLib](https://web.archive.org/web/20250418103149/https://wiki.freepascal.org/UnoLib)_
