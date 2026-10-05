# ARM Embedded Tutorial - Entry FPC and STM32

│ **English (en)** │

## Contents

  * 1 Introduction to STM32 and FPC
    * 1.1 Driver for ST-LINK V2
      * 1.1.1 Linux
      * 1.1.2 Windows/macOS
    * 1.2 Hardware connections
    * 1.3 Set up Arduino IDE (optimal)
    * 1.4 First start
    * 1.5 Build Lazarus with FPCUPdeluxe
      * 1.5.1 Build trunk
      * 1.5.2 Build ARM cross compiler
    * 1.6 Prepare Lazarus
      * 1.6.1 Start with F9
      * 1.6.2 Code tools
      * 1.6.3 Readable ASM code creation
      * 1.6.4 Faulty optimization
    * 1.7 First project
      * 1.7.1 Create a new project
      * 1.7.2 Start project
    * 1.8 Manually with make
    * 1.9 MSEide and MSEgui
  * 2 See also



# Introduction to STM32 and FPC

This tutorial was tested using 64 bit Linux Mint. 

## Driver for ST-LINK V2

### Linux

Download the package as a zip with **clone or download** from <https://github.com/texane/stlink>

Follow the instructions at <https://github.com/texane/stlink/blob/master/doc/compiling.md>

The following packages must be installed: 

  * CMake (minimum v2.8.7)
  * C compiler (gcc, clang, mingw)
  * Libusb 1.0 (minimum v1.0.9)


    
    
     sudo apt-get install cmake
     sudo apt-get install gcc
     sudo apt-get install libusb-1.0-0.dev
     // sudo apt-get install libusb-dev // ???
    

Compile and install stlink: 
    
    
     $ make release
     $ make debug
    
     $ mkdir build # may already exist.
     $ cd build
     $ cmake -DCMAKE_BUILD_TYPE = Debug ..
     $ make
    

To install in your home directory: 
    
    
     $ cd build/release
     $ make install DESTDIR=$HOME
    

To install for all users: 
    
    
     $ cd build/release
     $ sudo make install
    

Reboot or: 
    
    
     $ sudo udevadm control --reload-rules
     $ sudo udevadm trigger
    

Then there should be a device file /dev/stlinkv2_3 or similar. 

Run the following as a test: 
    
    
     st-util
    

This should produce several lines of information, including how much SRAM the controller has. You can then cancel it with Ctrl+C. 

If st-util aborts with the following error: 
    
    
     st-util: error while loading shared libraries: libstlink.so.1: cannot open shared object file: No such file or directory
    

This is probably the problem: <https://github.com/texane/stlink/issues/700> so run the following: 
    
    
     $ sudo ldconfig
    

### Windows/macOS
    
    
     <https://github.com/texane/stlink/releases/tag/1.3.0> (not tested) 
    

## Hardware connections

Connect the ST-LINK V2 to the STM32F103C as follows: 

ST LINK V2 | -> | STM32F103C   
---|---|---  
SWDIO | -> | SWO   
GND | -> | GND   
SWCLK | -> | SWCLK   
3.3V | -> | 3.3V   
  
## Set up Arduino IDE (optimal)

This is not absolutely necessary, but it has the advantage that you can look up how problems have been solved there, eg I²C access. 

Note the following page (does not work with Sloeber (Eclipse for Arduino)) 
    
    
     <https://github.com/stm32duino/Arduino_Core_STM32> 
    

JSON link: 
    
    
     <https://github.com/stm32duino/BoardManagerFiles/raw/master/STM32/package_stm_index.json> 
    

The following JSON also works with Sloeber. Insert this JSON link in the Arduino IDE under File > Preferences: 
    
    
     <http://dan.drown.org/stm32duino/package_STM32duino_index.json> (Version 2018.4.2 has been tested.) 
    

It is also best to activate the detailed output for compilation and upload. Then you can see any error uploading. Attention: Uploading is completed with the green bar which is displayed, even if an error occurred in the detailed report. 

Install the following in the board management: 
    
    
     stm32duino/STM32F1xx ... 
    

Configure the board under Tools: 

  * Board **Generic STM32F103 series**
  * Serial Interface **No Serial**
  * Upload method **STLINK**



## First start

Upload the Arduino blink sketch, then the LED PC13 should blink. Possibly. Set port to PC13. 

## Build Lazarus with FPCUPdeluxe

### Build trunk

Download FPCUPdeluxe-[OS] from <https://github.com/newpascal/fpcupdeluxe/releases> It is best to copy the download of FPCUPdeluxe into a new empty folder and then start the copied file in the file manager with a double click. Linux asks if the file should be made executable, answer yes. The FPCUPdeluxe GUI should then start. 

  * Set the top left list boxes **FPC version** and **Lazarus version** to **trunk** or **embedded** which are both in the **Basic tab**.
  * Build the Lazarus base trunk/embedded version with `Install/update FPC+Laz`.



If everything goes through without errors you get the current trunk from FPC and Lazarus. Normal desktop applications can now be created with this. There should be an icon for **Lazarus_fpcupdeluxe** on the Desktop. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Note: SVN repositories may experience difficulties. If so, don't give up, just try again later ;-) What also helps, run FPCUPdeluxe twice in a row. An out of date FPCUPdeluxe can also cause difficulties. If something doesn't work, you can ask in the [Lazarus forum](<https://forum.lazarus.freepascal.org/index.php/topic,34645.0.html>), the author is very helpful.

### Build ARM cross compiler

Press the `Setup+` button in the **Cross tab** to open a dialog with further options. Set the following there: 

  * "Use jobs for GNU make": Remove hooks.
  * CPU: **arm**
  * OS: **embedded**
  * Cross SubArch Overide: **armv7m**
  * Close dialog with Ok .



Select the following check boxes in the **Cross tab** : 

  * CPU: **arm**
  * OS: **embedded**



Press the `Install cross compiler` button, the cross compiler will be built. If everything worked correctly, exit FPCUPdeluxe. 

## Prepare Lazarus

### Start with `F9`

If you try to start with `F9`, you always get an error message. You can prevent this. Change the following in Lazarus: 

  * "Tools > Settings ... > Keyboard layout > Commands from the menu 'Start' > Start program", set to Unknown.
  * "Tools > Settings ... > Keyboard layout > Commands from the menu 'Start' > Start without debugger", remove the ticks from Shift and Ctrl.



Now you can upload and start a program normally with `F9`, but consider that the debugger is also disabled if you then write a normal PC program with Lazarus! 

### Code tools

It may be that the code tools does not find the stm32f103xb unit. If so, then the following step is necessary: 

  * "Tools > Reread FPC source code directory"



### Readable ASM code creation

If you want to see the source as assembler code, the following step is necessary: 

  * Enter "Project > Project Settings ... > User-defined settings > User-defined settings" **-al** , this creates an *.s file that contains the program as ASM code.



### Faulty optimization

I have experienced a program not working properly because the compiler incorrectly optimised the code. So you should switch off code optimization with **{$O-}** at the beginning of the program. 

## First project

### Create a new project

  * "File -> New ... > Simple Program" - create a new project.
  * "Project > Project Settings ... > Configuration and Goals" - set the following: 
    * Target operating system (-T): **Embedded**
    * Target CPU family (-P): **arm**
    * Target processor (-Cp): **ARMV7M**
  * "Project > Project Settings ... > Debug" : Remove the check mark "Generate debugger information for GDB" .
  * "Project > Project settings ... > User-defined settings -> User-defined settings still enter **-WpSTM32F103X8**.
  * "Project > Project Settings ... > Compiler Commands -> Execute After" 
    * Remove the Compile and Recompile ticks.
    * Enter the following into the command: **st-flash write stm32Blink.bin 0x8000000**



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The paths still have to be adapted to your own installations.

As a recommendation, I would take this project as a template and build all new projects on top of it. Setting the whole thing up for a new project each time is a bit tedious. 

### Start project

Nothing should now be standing in the way of your first blink program (source below). Compile and upload the program with `F9`. If everything worked, the LED at pin 13 should flash about every second. 
    
    
    program stm32Blink;
    
    {$O-}
    
    procedure delay;
    var
      i : uint32;
    begin
      for i := 0 to 500000 do 
        begin
          asm 
            nop   // empty instruction
          end;
        end;
    end;
    
    begin
      // turn on Port C
      RCC.APB2ENR := RCC.APB2ENR or (%1 shl 4);
    
      // Pin 13 from Port C to Output
      PortC.CRH := $00300000;
    
      // Note: The LED lights up at LOW.
      while true do 
        begin
          // Pin 13 - High
          PortC.BSRR := 1 shl 13;
          Delay;
    
          // Pin 13 - Low
          PortC.BRR := 1 shl 13;
          Delay;
        end;
     end.
    

## Manually with make

Not tested. 
    
    
      make clean all OS_TARGET=embedded CPU_TARGET=arm SUBARCH=armv7m
    

## MSEide and MSEgui

As an alternative to Lazarus you can also use the MSEide and MSEgui. 

The following tools are necessary: 

  * <https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads> \- GNU Arm Embedded Toolchain (gcc for ARM)
  * <https://sourceforge.net/projects/mseide-msegui/files/fpccrossarmembedded/> \- cross-compiler for ARM Embedded.
  * <https://sourceforge.net/projects/mseide-msegui/files/mseide-msegui/4.6/> \- mseGUI (download bin and sources)
  * <https://gitlab.com/mseide-msegui/mseuniverse/tree/master/samples/embedded/arm/stm32l100c> \- demo project



Unzip these archives into a folder, eg /home/tux/STM3-FPC 

Start MSEide under /home/tux/STM32-FPC/msegui/bin. 

  * Settings > Configure MSEide 
    * adjust the following path: ${MSEDIR} - /home/tux/STM32-FPC/msegui/
  * Project > open - Open the demo project in the extracted folder. (/home/tux/STM32-FPC/samples/embedded/arm/stm32l100c/stm32l100c.prj)
  * Project > Options > Macros, adjust the following paths: 
    * TOOLCHAINDIR - /home/tux/STM32-FPC/gcc-arm-none-eabi-7-2017-q4-major/
    * CROSSFPCDIR - / home/tux/STM32-FPC/crossfpc-x86_64_linux_eabi_embedded_3_1_1/



If a red break point is set in the left column, deactivate it with a click. Start the program with `F9`. If everything went well, this should work without errors. 

# See also

  * [ARM Embedded Tutorials](<ARM_Embedded_Tutorials.md> "ARM Embedded Tutorials") \- Overview

---

_Source: [https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Entry_FPC_and_STM32](https://web.archive.org/web/20250114071207/https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Entry_FPC_and_STM32)_
