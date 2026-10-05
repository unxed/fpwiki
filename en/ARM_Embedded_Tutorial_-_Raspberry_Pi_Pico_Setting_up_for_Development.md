# ARM Embedded Tutorial - Raspberry Pi Pico Setting up for Development

## Contents

  * 1 Download Documentation
  * 2 Setting up a Debug Probe
  * 3 Preparing usage of OpenOCD
  * 4 See also



## Download Documentation

After having Lazarus and FPC in place the next step is to download information related to the RP2040 chip and the Pico board. 

Raspberry Foundation has done a pretty good job at documentation, to get our feet wet it makes sense to read through their pdf documentation that can be found on this page: 

[Getting Started with Raspberry Pi Pico](<https://www.raspberrypi.org/documentation/pico/getting-started/>)

scroll down to the Documentation Section and download at least the "RP2040 Datasheet" and "Getting started with Raspberry Pi Pico" documents. 

You can later use the RP2040 Datasheet for reference on implementation details of the microcontroller, for now we only need the 'Getting Started' pdf 

Now read through the Getting started and if you want to get your feet really wet also clone their SDK and Examples repositories from github. 

This will not be strictly necessary but when you want to dig really deep into development on the Pico it makes sense that you also dip your feet into the C/C++ world (Brrrr...... cold water, indeed....) 

## Setting up a Debug Probe

For our next steps we need to either set up a Raspberry Pi as a debug probe or, when you followed my advice and bought a second board, convert our 2nd Raspberry Pi Pico to a debug Probe. 

Setting up the Raspberry Pi is very well covered in the Getting started pdf documentation, I will not cover that here. 

Converting a Pico to a Picoprobe is very easy, so here are the steps in a nutshell: 

First download an UF2 image for the Pico, you can find the link on the getting Started webpage, scroll down to Utilities and download the UF2 under "Debugging using another Raspberry Pi Pico". 

Then push the BOOTSEL button on your Pico and while keeping it pressed connect it to your computer via USB or re-plug the already connected USB cable. The Pico will now statup as a flash drive RPI-RP2. 

Now drop the downloaded UF2 file in that drive and the device will flash the software and reboot as a Picoprobe. 

When you are on Windows then follow the steps in the 'Appendix A' of the getting started pdf about installing the Picoprobe driver. 

For wiring up your freshly created Picoprobe to your development board work through the 'Picoprobe Wiring' chapter. 

Here's a setup on a Breadboard for your reference: 

[![picoPicoDebug Steckplatine.png](https://wiki.freepascal.org/images/d/d6/picoPicoDebug_Steckplatine.png)](</File:picoPicoDebug_Steckplatine.png>)

and a new version using a Pimoroni Tiny2040 board as debug probe which fits nicely on a half-size breadboard: 

[![PicoTinyDebug.png](https://wiki.freepascal.org/images/5/5a/PicoTinyDebug.png)](</File:PicoTinyDebug.png>)

The picoprobe is the pico/tiny on the right, the pico that runs the program to be debugged is on the left side. 

## Preparing usage of OpenOCD

Together with the installation Lazarus/FPC you have also automagically downloaded a pre-compiled version of openocd that you can use together with your PicoProbe. 

If this version is missing then open fpcupdeluxe and select the 'Modules' Tab. Browse for develtools4fpc and hit 'Install Module' button 

[![modulesDevtoolsFPC.png](https://wiki.freepascal.org/images/3/34/modulesDevtoolsFPC.png)](</File:modulesDevtoolsFPC.png>)

  
**Windows only:**

before we continue you need to install an USB driver for your probe, as described in the Raspberry Pico manual. 

Read the instructions in the Getting started manual (End of Appendix A) or follow them here: 

Download zadig from this link: 

<http://zadig.akeo.ie/>

and then connect your pico to your computer. 

Start zadig, select picoprobe (Interface 2) and libusb-win32 

  
[![pico-zadic.png](https://wiki.freepascal.org/images/1/19/pico-zadic.png)](</File:pico-zadic.png>)

Hit install driver.... done.. 

  


Navigate to the directory where FPCUPdeluxe installed you FPC/Lazarus and check the directory ccr/develtool4fpc/bin: 

[![pico-included-openocd.png](https://wiki.freepascal.org/images/7/79/pico-included-openocd.png)](</File:pico-included-openocd.png>)

or on Windows: 

[![pico-included-openocd2.png](https://wiki.freepascal.org/images/d/d2/pico-included-openocd2.png)](</File:pico-included-openocd2.png>)

Note down the path and open a fresh terminal / cmd shell. 

Start openocd-rp2040 with the full path to openocd-rp2040 as found on your computer: 
    
    
     ~/fpcupdeluxe/ccr/develtools4fpc/bin/openocd-rp2040 -f board/pico.cfg
    

[![pico-start-openocd2.png](https://wiki.freepascal.org/images/7/7e/pico-start-openocd2.png)](</File:pico-start-openocd2.png>)

  

    
    
     C:\fpcupdeluxe\ccr\develtools4fpc\bin\openocd-rp2040.exe -f board/pico.cfg
    

[![pico-start-openocd.png](https://wiki.freepascal.org/images/f/f3/pico-start-openocd.png)](</File:pico-start-openocd.png>)

## See also

  * [ARM Embedded Raspberry Pi Pico Tutorials](<ARM_Embedded_Tutorial_-_FPC_and_the_Raspberry_Pi_Pico.md> "ARM Embedded Tutorial - FPC and the Raspberry Pi Pico")

---

_Source: [https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Setting_up_for_Development](https://web.archive.org/web/20250401083744/https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Setting_up_for_Development)_
