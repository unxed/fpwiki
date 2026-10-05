# ARM Embedded Tutorials

│ **[Deutsch (de)](</ARM_Embedded_Tutorials/de> "ARM Embedded Tutorials/de")** │  **English (en)** │  **[中文（中国大陆） (zh_CN)](</ARM_Embedded_Tutorials/zh_CN> "ARM Embedded Tutorials/zh CN")** │    
****

## Contents

  * 1 Overview
  * 2 Set up driver / cross compiler / IDE
    * 2.1 STM32
    * 2.2 Raspberry Pi Pico
  * 3 ARM programming Examples
    * 3.1 STM32
    * 3.2 Raspberry Pi Pico
  * 4 See also



## Overview

Tutorials for programming ARM microcontrollers with FPC and Lazarus. This applies, for example, to the STM32 microcontrollers and RP2040 (Raspberry Pi Pico) microcontroller. 

## Set up driver / cross compiler / IDE

### STM32

  * [Introduction to STM32 and FPC](<ARM_Embedded_Tutorial_-_Entry_FPC_and_STM32.md> "ARM Embedded Tutorial - Entry FPC and STM32") \- How do I set up FPC / IDE (MSEide) to program an STM32F103C?



### Raspberry Pi Pico

To best use this tutorial you will need to buy (at least) two Raspberry Pi Pico, we will use one as a target and the second one as a debug probe. Do yourself a favour, invest $4 for a second device, being able to debug is worth so much more. 

As the Pico is brand new and support for the board is a work in progress I'd recommend that you set up a dedicated installation of Lazarus and Free Pascal as you will need to use both trunk version of Lazarus and a specially patched version of FPC that includes the necessary adjustments so that FPC knows about the Pico. Also expect changes as we all learn along the way. 

  * [Installing Lazarus and Free Pascal](<ARM_Embedded_Tutorial_-_Installing_Lazarus_and_Free_Pascal.md> "ARM Embedded Tutorial - Installing Lazarus and Free Pascal")
  * [Raspberry Pi Pico Setting up for Development](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Setting_up_for_Development.md> "ARM Embedded Tutorial - Raspberry Pi Pico Setting up for Development")
  * [Raspberry Pi Pico Debugging the onboard LED](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Debugging_the_onboard_LED.md> "ARM Embedded Tutorial - Raspberry Pi Pico Debugging the onboard LED")



To access the Raspberry Pi Pico examples below, together with all needed dependencies, clone this repository: 

<https://github.com/michael-ring/pico-fpcexamples>

Create issues and add feature requests on Github: 

<https://github.com/michael-ring/pico-fpcexamples/issues>

## ARM programming Examples

### STM32

  * [GPIO - output and input](<ARM_Embedded_Tutorial_-_Simple_GPIO_on_and_off_output.md> "ARM Embedded Tutorial - Simple GPIO on and off output") \- How to make a GPIO output
  * [Simple timer](<ARM_Embedded_Tutorial_-_Simple_Timer.md> "ARM Embedded Tutorial - Simple Timer") \- A simple timer



### Raspberry Pi Pico

  * [Raspberry Pi Pico blinking the onboard LED](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Blinking_the_onboard_LED.md> "ARM Embedded Tutorial - Raspberry Pi Pico Blinking the onboard LED")
  * [Raspberry Pi Pico saying Hello via UART](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_saying_Hello_via_UART.md> "ARM Embedded Tutorial - Raspberry Pi Pico saying Hello via UART")
  * [Raspberry Pi Pico using the ADC](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_using_the_ADC.md> "ARM Embedded Tutorial - Raspberry Pi Pico using the ADC")
  * [Raspberry Pi Pico Scanning for I2C Devices](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Scanning_for_I2C_Devices.md> "ARM Embedded Tutorial - Raspberry Pi Pico Scanning for I2C Devices")
  * [Raspberry Pi Pico using Displays and I2C](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_using_Displays_and_I2C.md> "ARM Embedded Tutorial - Raspberry Pi Pico using Displays and I2C")
  * [Raspberry Pi Pico using Displays and SPI](</index.php?title=ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_using_Displays_and_SPI&action=edit&redlink=1> "ARM Embedded Tutorial - Raspberry Pi Pico using Displays and SPI \(page does not exist\)")



## See also

  * [Examples](<https://github.com/sechshelme/Lazarus-Embedded>) (external)
  * [ARM](<ARM.md> "ARM") \- Cross compiler
  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Tutorials for AVR Embedded/Arduino

---

_Source: [https://wiki.freepascal.org/ARM_Embedded_Tutorials](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/ARM_Embedded_Tutorials)_
