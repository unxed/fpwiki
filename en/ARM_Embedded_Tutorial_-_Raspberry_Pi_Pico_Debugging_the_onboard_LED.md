# ARM Embedded Tutorial - Raspberry Pi Pico Debugging the onboard LED

## Contents

  * 1 Introduction
  * 2 Load the demo project into lazarus
  * 3 Start Debugging
  * 4 See also



## Introduction

In this chapter we will learn how to debug a pico with thre help of another pico and openocd-rp2040. 

If you did not yet read the introduction on how to set up openocd on your computer take a look at this page: 

[ARM Embedded Tutorial - Raspberry Pi Pico Setting up for Development](<ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Setting_up_for_Development.md> "ARM Embedded Tutorial - Raspberry Pi Pico Setting up for Development")

Here's a schematic on how to connect the two Pico's needed for this chapter on a breadboard: 

[![picoPicoDebug Steckplatine.png](https://wiki.freepascal.org/images/d/d6/picoPicoDebug_Steckplatine.png)](</File:picoPicoDebug_Steckplatine.png>)

the picoprobe is the device on the right, the pico that runs our program is on the left side. 

## Load the demo project into lazarus

Open the blinky/blinky-raspi_pico.lpi project in Lazarus. 

First thing to do is to configure the debugger, to do this open the IDE options Page and go to 'Debugger Backend' 

[![pico-setup-debugger.png](https://wiki.freepascal.org/images/5/5d/pico-setup-debugger.png)](</File:pico-setup-debugger.png>)

  
First select 'GNU remote debugger (gdbserver)' and then select the gdb to use. 

Here you have two choices, both are provided by FPCUPdeluxe in the installation directory. 

First choice is to use gdb-multiarch for your platform, this allows you to debug arm or avr boards without the need to change the debugger. 

The alternative is to use a gdb that is specific to arm-none-eabi, it is located in the fpcbootstrap/gdb/arm-embedded/ directory. On Windows this directory is pre-selected when you first open up the Debugger Backend Page. 

Depending on your choice you either put 'auto' in the Architecture field, when you decide to use the debugger in the fpcbootrap directory leave the field empty. 

Then set the 'Debugger_Remote_DownloadExe' entry to true and set Debugger_Remote_Hostname to localhost 

Enter 3333 in the Debugger_Remote_Port field (the OpenOCD default port for debugging) 

You will most likely also have to set EncodeCurrentPath to gdfeNone, when this field is set wrong you cannot download your code to the pico. 

The last setting is to set all InternalExceptionBreakpoints to 'False'. **This setting is very important** , the pico has only 4 Hardware Breakpoints and when you waste three of them for this setting then it will be close to impossible to debug even a simple program because you run out of breakpoints. 

Last Step is to go back to the top of the dialog and set the Name field to 'OpenOCD'. 

Hit OK to close the IDE Options, you are now ready to debug! 

## Start Debugging

Special note for linux-users: 

the USB device that is created for picoprobe requires root priviledges, so openocd must be started with sudo unless you do the following: 

As root, create a new file 
    
    
     /etc/udev/rules.d/50-rpi-picoprobe.rules 
    

with the following content: 
    
    
     # Remember to  udevadm control --reload  after any changes.
     
     # Assume that the CDC serial device (typically /dev/ttyACM0) has already been
     # assigned to root:dialout on account of its class.
     
     SUBSYSTEMS=="usb", ATTRS{idVendor}=="2e8a", ATTRS{idProduct}=="0004", ATTRS{product}=="Picoprobe", GROUP="plugdev", MODE="0660"
    

  


Open a cmd window or a Unix shell and start openocd-rp2040: 
    
    
     ~/fpcupdeluxe/ccr/develtools4fpc/bin/openocd-rp2040 -f board/pico.cfg
    

or 
    
    
     C:\fpcupdeluxe\ccr\develtools4fpc\bin\openocd-rp2040.exe -f board/pico.cfg
    

  
(Exchange fpcupdeluxe with the name of the directory where you put your FPCUPdeluxe installation....) 

Click 'Build' to build the project, then set a breakpoint on the first line of code and then hit 'Run' to start debugging: 

[![pico-lazarus-debugging.png](https://wiki.freepascal.org/images/b/bd/pico-lazarus-debugging.png)](</File:pico-lazarus-debugging.png>)

Play around with single stepping, debugging works quite good but not perfect.... I usually set breakpoints where I need them and use 'Run' to reach them. 

Always keep an eye on the list of used breakpoints, when things go crazy you have likely exceeded your 4 hardware breakpoints which makes debugging impossible. Delete unneeded breakpoints and start again! 

## See also

  * [ARM Embedded Raspberry Pi Pico Tutorials](<ARM_Embedded_Tutorial_-_FPC_and_the_Raspberry_Pi_Pico.md> "ARM Embedded Tutorial - FPC and the Raspberry Pi Pico")

---

_Source: [https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Debugging_the_onboard_LED](https://web.archive.org/web/20241206084640/https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Debugging_the_onboard_LED)_
