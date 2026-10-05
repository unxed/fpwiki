# ARM Embedded Tutorial - Raspberry Pi Pico Blinking the onboard LED

│ **English (en)** │

## Introduction

Finally we are ready to craft some code. To make things easier, I have created a git repository that hosts the examples and extra binaries needed for successfully building the code. 

Please clone this repository: 

<https://github.com/michael-ring/pico-fpcexamples>

to access the working examples plus the extra prerequisites. 

Our first version of the blinking LED theme will stay close to the example provided by the Raspberry Foundation in their Pico examples repository on GitHub. The code will look pretty much the same as the C code; we will also re-use code already done by the Foundation by linking to object files from their SDK. 

This technique is helpful when you want to get working results fast, we can already get something going without the need for us to fully understand the new microcontroller architecture. 

## Load the blinky Project in lazarus

To follow along, open the blinky-raspi_pico.lpi project from the 'blinky directory in Lazarus: 

[![lazarus-pico-blinky.png](https://wiki.freepascal.org/images/b/b8/lazarus-pico-blinky.png)](</File:lazarus-pico-blinky.png>)

  
The main Pascal Code will look like this: 
    
    
    program blinky;
    {$MODE OBJFPC}
    {$H+}
    {$MEMORY 10000,10000}
    
    uses
      pico_gpio_c,
      pico_timer_c;
    
    begin
      gpio_init(TPicoPin.LED);
      gpio_set_dir(TPicoPin.LED,TGPIODirection.GPIO_OUT);
      repeat
        gpio_put(TPicoPin.LED,true);
        busy_wait_us_32(500000);
        gpio_put(TPicoPin.LED,false);
        busy_wait_us_32(500000);
      until 1=0;
    end.
    

The first few lines should look familiar: 

We want OBJFPC mode, and use AnsiStrings ({$H+}). 

then we define the values for Stacksize and Heapsize. Raspberry Pico has lots of memory so be generous here... 

Strictly speaking we do not use any heap memory in this example because we do not use any AnsiStrings or any other dynamic memory allocation. 

Next thing we do is to include the bindings to the C-Api provided by Raspberry Foundation. 

pico_gpio_c has all definitions for the gpio port and pico_timer_c provides the busy_wait functions we use. 

Please also note that we use TPicoPin record defined in pico_gpio_c so that we do not need to remember all those pin names available to us. 

TPicoPin.LED is for example the equivalent of GPIO Pin 25. 

Hit 'Build' and after a second or two the code should be ready for testing.... 

  


## Running your first Pico App

We now have a binary, but does it work? 

The easiest way to test is to press and hold the 'BootSel' button on your Pico and to plug it into an USB-Port of your computer. 

Then navigate to the blinky/ directory and Drag&Drop the 'blinky.uf2' file on the RPI-RP2 drive 

[![pico-drag-drop-uf2.png](https://wiki.freepascal.org/images/2/2a/pico-drag-drop-uf2.png)](</File:pico-drag-drop-uf2.png>)

Shortly after dropping the uf2 file the Pico will reboot and the LED will start blinking.... 

[Back to main Pico Page](<ARM_Embedded_Tutorial_-_FPC_and_the_Raspberry_Pi_Pico.md> "ARM Embedded Tutorial - FPC and the Raspberry Pi Pico")

---

_Source: [https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Blinking_the_onboard_LED](https://web.archive.org/web/20230929012701/https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Blinking_the_onboard_LED)_
