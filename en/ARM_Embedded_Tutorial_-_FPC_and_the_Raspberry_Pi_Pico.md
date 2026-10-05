# ARM Embedded Tutorial - FPC and the Raspberry Pi Pico

│ **English (en)** │    
****

The Raspberry Pi Foundation has released the Raspberry Pi Pico, a very cheap Microcontroller board with quite interesting specs. 

## Contents

  * 1 Introduction
  * 2 Setup
  * 3 PicoProbe
  * 4 The Examples from Github
  * 5 Example Programs
    * 5.1 Blinky
    * 5.2 Debugging
    * 5.3 ADC
    * 5.4 I2C
    * 5.5 SPI
  * 6 Building a Pico Compiler
  * 7 Finally



### Introduction

[Raspberry Pi Pico Specifications](<https://www.raspberrypi.org/products/raspberry-pi-pico/specifications/>)

To best use this tutorial you will need to buy (at least) two Raspberry Pi Pico, we will use one as a target and the second one as a debug probe. Do yourself a favour, invest $4 for a second device, being able to debug is worth so much more. 

As the Pico is brand new and support for the board is a work in progress I'd recommend that you set up a dedicated installation of Lazarus and Free Pascal as you will need to use both trunk version of Lazarus and a specially patched version of FPC that includes the necessary adjustments so that FPC knows about the Pico. Also expect changes as we all learn along the way. 

  


### Setup

To install the required versions of Lazarus and Free Pascal please see here: 

[ARM Embedded Tutorial - Installing Lazarus and Free Pascal](<ARM_Embedded_Tutorial_-_Installing_Lazarus_and_Free_Pascal.md> "ARM Embedded Tutorial - Installing Lazarus and Free Pascal")

  


### PicoProbe

To prepare a PicoProbe and to setup Lazarus please follow this guide: 

[ARM Embedded Tutorial - Raspberry Pi Pico Setting up for Development](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Setting_up_for_Development.md> "ARM Embedded Tutorial - Raspberry Pi Pico Setting up for Development")

### The Examples from Github

To access the examples together with all needed dependencies clone this repository: 

<https://github.com/michael-ring/pico-fpcexamples>

when you find errors in the code or would like to request another demo please enter an issue on github: 

<https://github.com/michael-ring/pico-fpcexamples/issues>

### Example Programs

Now we are ready for our first Program, as practice in the embedded programming world we start with blinking the on-board LED: 

##### Blinky

[ARM Embedded Tutorial - Raspberry Pi Pico Blinking the onboard LED](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Blinking_the_onboard_LED.md> "ARM Embedded Tutorial - Raspberry Pi Pico Blinking the onboard LED") Not suitable for the Raspberry Pi W 

##### Debugging

The next step in this tutorial is to set up Debugging from within Lazarus 

[ARM Embedded Tutorial - Raspberry Pi Pico Debugging the onboard LED](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Debugging_the_onboard_LED.md> "ARM Embedded Tutorial - Raspberry Pi Pico Debugging the onboard LED")

The next peripheral to join the party is the UART: 

[ARM Embedded Tutorial - Raspberry Pi Pico saying Hello via UART](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_saying_Hello_via_UART.md> "ARM Embedded Tutorial - Raspberry Pi Pico saying Hello via UART")

##### ADC

Time to go Analog: 

[ARM Embedded Tutorial - Raspberry Pi Pico using the ADC](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_using_the_ADC.md> "ARM Embedded Tutorial - Raspberry Pi Pico using the ADC")

##### I2C

Scanning the I2C Bus for Devices: 

[ARM Embedded Tutorial - Raspberry Pi Pico Scanning for I2C Devices](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Scanning_for_I2C_Devices.md> "ARM Embedded Tutorial - Raspberry Pi Pico Scanning for I2C Devices")

Talking to a Display via I2C: 

[ARM Embedded Tutorial - Raspberry Pi Pico using Displays and I2C](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_using_Displays_and_I2C.md> "ARM Embedded Tutorial - Raspberry Pi Pico using Displays and I2C")

##### SPI

Talking to a Display via SPI: 

[ARM Embedded Tutorial - Raspberry Pi Pico using Displays and SPI](</index.php?title=ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_using_Displays_and_SPI&action=edit&redlink=1> "ARM Embedded Tutorial - Raspberry Pi Pico using Displays and SPI \(page does not exist\)")

  


### Building a Pico Compiler

As indicated about, the most common way to get a working Pico compiler is to use fpcupdeluxe but you can, of course, build it yourself [Building a Pico Compiler](<Building_a_Pico_Compiler.md> "Building a Pico Compiler"). 

### Finally

This page is WIP, I received my Boards on 29.01.2021, upgrading it as I go....

---

_Source: [https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_FPC_and_the_Raspberry_Pi_Pico](https://web.archive.org/web/20241213022630/https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_FPC_and_the_Raspberry_Pi_Pico)_
