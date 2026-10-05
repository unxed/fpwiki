# ARM Embedded Tutorial - Installing Lazarus and Free Pascal

│ **English (en)** │

## Contents

  * 1 Quickstart Installation via FPCUPdeluxe
  * 2 Steps for people mainly interested in Wio Terminal or Raspberry Pico
  * 3 Steps for people interested in other targets like arm avr or espressif microcontrollers
  * 4 Install extra devel tools



## Quickstart Installation via FPCUPdeluxe

The easiest way to install Embedded Support for a huge variety of embedded controllers is to use fpcupdeluxe. 

Download latest(!!!) FPCUPdeluxe from 

[fpcupdeluxe releases on github](<https://github.com/LongDirtyAnimAlf/fpcupdeluxe/releases>). 

Make sure that you use a version >= v1.8.2r of FPCUPdeluxe, the tutorial references features that are only in this and newer versions. 

As we are using trunk versions we highly recommend to create an extra installation for embedded targets so that issues in compiler/lazarus do not effect your other work. 

## Steps for people mainly interested in Wio Terminal or Raspberry Pico

When your primary targts of interest are the Wio Terminal or the Raspberry Pico, then installation consists of entering an installation directory for fpcupdeluxe on top left and then selecting the speedbutton for 'Wio' or 'Pico' on the bottom of the screen: 

[![SelectEmbeddedandTrunk.png](https://wiki.freepascal.org/images/a/a5/SelectEmbeddedandTrunk.png)](</File:SelectEmbeddedandTrunk.png>)

## Steps for people interested in other targets like arm avr or espressif microcontrollers

For all other targets select embedded (or trunk.git) for fpc and trunk.git for lazarus. 

The reason to choose embedded would be to have support for more arm-embedded controllers that are currently not yet available in fpc trunk and installing trunk.git instead of stable lazarus is a good idea because debugging for embedded targets is much improved in lazarus trunk. 

For all platforms besides arm trunk.git is likely the best choice for both fpc and lazarus. 

To build your embedded version of Lazarus follow those simple steps: 

  * Select the target directory for your installation
  * Select embedded or trunk.git for fpc
  * Select trunk.git for lazarus



Hit the Install/update FPC+Laz button. 

  
[![SelectEmbeddedandTrunk.png](https://wiki.freepascal.org/images/a/a5/SelectEmbeddedandTrunk.png)](</File:SelectEmbeddedandTrunk.png>)

After installation of FPC+Lazarus is done switch to the 'Cross' Tab 

In the Cross tab select the desired CPU and the desired OS und then click on the 'Set subarch' button to define the subarch that you would like to use: 

  
[![fpcupdeluxe manual install 1.png](https://wiki.freepascal.org/images/d/df/fpcupdeluxe_manual_install_1.png)](</File:fpcupdeluxe_manual_install_1.png>)

Now you can select 'Install compiler' button and the selected subarch will be installed..... 

You may repeat this step for all CPU's subarchs you wish to install. 

Some hints on Subarchs: 

  * armv6m Subarch is good for Raspberry Pico, SAMD21 based board like the Arduino Zero or STM32F0/G0/L0 families


  * armv7m is for the Bluepill, Arduino Due or STM32F1/L1


  * armv7em is for the Wio Terminal, Blackpill or SAMD51 / STM32F3/F4/G4/H7/L4 families


  * avr5 is for Arduino Uno


  * lx6 is for ESP32 based boards



  


## Install extra devel tools

There is an extra module available to ease development by providing pre-compiled binaries for some usefull tools: 

For avr development: 

  * avarice (gdbserver for several debug probes manufactured by Microchip)


  * bossac (upload firmwares to avr controllers)



for arm development: 

  * openocd (gdbserver for all kinds of debug probes)


  * st-util (gdbserver specially for st-link v2/v3 debug probes)



for the Raspberry Pico: 

  * openocd-picodebug (gdbserver for running a debug probe on the 2nd cpu of your pico)


  * openocd-rp2040 (gdbserver for using a 2nd pico as a debug probe)



general: 

  * gdb-multiarch (one gdb to debug all embedded archs available)


  * gdb-multiarch-8.3.1 (an older version of gdb that works better for avr)



Install it by selection develtools4fpc on the Modules tab of fpcupdeluxe by selection develtools4fpc and clicking 'Install Module': 

[![modules devtools4fpc.png](https://wiki.freepascal.org/images/5/57/modules_devtools4fpc.png)](</File:modules_devtools4fpc.png>)

the files will then get deployed to the ccr/devtools4fpc directory inside of your fpcupdeluxe installation 

  


  


More information on fpcupdeluxe can be found here: [FpcUpDeluxe Wiki](<fpcupdeluxe.md> "fpcupdeluxe").

---

_Source: [https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Installing_Lazarus_and_Free_Pascal](https://web.archive.org/web/20241012103555/https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Installing_Lazarus_and_Free_Pascal)_
