# Lazarus on Raspberry Pi

│ **[Deutsch (de)](</Lazarus_on_Raspberry_Pi/de> "Lazarus on Raspberry Pi/de")** │  **English (en)** │  **[español (es)](</Lazarus_on_Raspberry_Pi/es> "Lazarus on Raspberry Pi/es")** │  **[suomi (fi)](</Lazarus_on_Raspberry_Pi/fi> "Lazarus on Raspberry Pi/fi")** │  **[中文（中国大陆） (zh_CN)](</Lazarus_on_Raspberry_Pi/zh_CN> "Lazarus on Raspberry Pi/zh CN")** │    
****

[![Lazarus on Raspbian Wheezy.](https://wiki.freepascal.org/images/e/ef/Lazarus_on_Raspberry_Pi_Raspian_Wheezy_version_2012-10-28.png)](</File:Lazarus_on_Raspberry_Pi_Raspian_Wheezy_version_2012-10-28.png>)

[](</File:Lazarus_on_Raspberry_Pi_Raspian_Wheezy_version_2012-10-28.png> "Enlarge")

Lazarus on Raspbian Wheezy

[![Raspberry Pi Logo.png](https://wiki.freepascal.org/images/8/85/Raspberry_Pi_Logo.png)](</File:Raspberry_Pi_Logo.png>)

This article applies to [Raspberry Pi](</Category:Raspberry_Pi> "Category:Raspberry Pi") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

The **Raspberry Pi** is a credit-card-sized single-board computer. It has been developed in the UK by the Raspberry Pi Foundation with the intention of stimulating the teaching of basic computer science in schools. Raspberry Pis are also used for multiple other purposes that are as different as media servers, robotics and control engineering. 

The Raspberry Pi Foundation recommends [ the Pasperry Pi OS](<Raspbian.md> "Raspbian") as standard operating system. Alternative systems running on RPI include RISC OS and various [Linux](<Portal_Linux.md> "Portal:Linux") distributions, as well as [Android](<Portal_Android.md> "Portal:Android") and [FreeBSD](<Portal_FreeBSD.md> "Portal:FreeBSD"). 

Lazarus runs natively under the Raspbian operating system. 

## Contents

  * 1 Installing and compiling Lazarus
    * 1.1 Introduction
    * 1.2 Correcting swap file size
    * 1.3 Simple installation
    * 1.4 Not Quite as Simple Install
    * 1.5 Cross compiling for the Raspberry Pi from Windows
    * 1.6 Cross compiling for the Raspberry Pi from Linux
  * 2 Accessing external hardware
    * 2.1 Native hardware access
      * 2.1.1 Switching a device via the GPIO port
      * 2.1.2 Reading the status of a pin
    * 2.2 Hardware access via encapsulated shell calls
    * 2.3 wiringPi procedures and functions
    * 2.4 rpi_hal-Hardware Abstraction Library (GPIO, I2C and SPI functions and procedures)
    * 2.5 PiGpio Low-level native pascal unit (GPIO control instead of wiringPi c library)
    * 2.6 PXL (Platform eXtended Library) for low level native access to GPIO, I²C, SPI, PWM, UART, V4L2, displays and sensors
  * 3 References
  * 4 External and Other Links



## Installing and compiling Lazarus

### Introduction

Installing FPC and Lazarus on a Raspberry Pi is very little different from other Linux boxes, so please refer to [Installing_Lazarus_on_Linux](<Installing_Lazarus_on_Linux.md> "Installing Lazarus on Linux"). How ever, there are some RasPi particularities worth noting. 

  * RasPi do not always have enough memory to build Lazarus, this affects the routine rebuild of the IDE that happens when installing packages for example. The solution is to increase swap size (below).
  * The Lazarus SourceForge repository does not include a pre-made RasPi package.
  * If you are using the Raspberry Pi OS, its based on Debian and Debian has quite a long release cycle. So, at any one time, the repo provided Lazarus is likely to be out of date and, occasionally, so will the FPC be.
  * Notwithstanding the above point, at present, late 2023, the Debian Bookworm based Raspberry Pi OS has the appropriate FPC and a Lazarus that suits most people. This will change !



### Correcting swap file size

(Info from forum user "Thaddy", updated.) If you have RPi with memory size less then 4Gb, and you want to build Lazarus from source or use FPCUPdeluxe or just rebuild the IDE after installing a package you need to allocate at least 1G if not 2G of swap. This appears to be particularly so with Raspberry Pi OS based on Bookworm rather than Bullseye. 

To increase swap you should firstly ensure you have a couple of gig free and - 

  * sudo dphys-swapfile swapoff
  * sudo nano /etc/dphys-swapfile
  * in the file, find CONF_SWAPSIZE and change the value to 2048 (or 1024) and save it.
  * sudo dphys-swapfile setup
  * sudo dphys-swapfile swapon



You should now see the file, /var/swap, increase to 2G (or 1G). On some other operating systems, eg a "pure" debian, you may need to install the dphys-swapfile package. 

If you don't have enough memory (real memory and swap) then, your compile processes (particularly assembling) will stop and the whole machine freezes. You have been warned ! 

While you are fiddling with the swap file, consider, if possible, moving the swap file from /var/swap to somewhere off the SDCard, unfortunately, SDCards are not good at the repetitive writes that happen in a swap file. Not critical but a good idea. 

If you have problems making the swap file, see this <https://forums.raspberrypi.com/viewtopic.php?p=1881546>

### Simple installation

Most distributions have a graphic installer of some sort, look for entries like "Add / Remove Software" in your menus. Alternatively, all systems will have a command line interface. As hinted above, its likely that your distribution will have a current FPC, if so, best to use it. More interesting is Lazarus, as it changes more frequently, consider your needs. The Raspberry Pi OS, late 2023, has Lazarus 2.2.6. While version 3.0 of Lazarus is released now, it will take awhile to make it into distro's repos. Some users will, for the time being, be quite happy with Lazarus 2.2.6. Later on, you can remove the distro supplied Lazarus 2.2.6 and build 3.0 from source yourself, its easy. 

A advantage of using the distro supplied FPC and Lazarus is that they test them, make sure everything needed is there and its very easy. 

See [the Package Manager Model](<Installing_Lazarus_on_Linux.md>). 

### Not Quite as Simple Install

There are two other functional alternatives for installing Lazarus, building from source and using [fpcupdeluxe](<fpcupdeluxe.md> "fpcupdeluxe"), an application that you run on your computer to manage your FPC / Lazarus install. Its very suited to people who just want to get a working install quickly, fpcupdeluxe will do the thinking for them. Fpcupdelux will install (and manage) both FPC and Lazarus and you should use them together. 

Installing a non distro FPC is a touch more complicated still. Its [possible](<Installing_the_Free_Pascal_Compiler.md>) to build a new FPC using an older, existing, FPC, might be a challenge for a less powerful Pi. An better option is to [download and install](<Installing_the_Free_Pascal_Compiler.md>) a tarball. 

Its quite reasonable to use the distro provided FPC and build your own Lazarus. In fact, many users have several lazarus versions installed this way. 

Building Lazarus [from source](<Installing_Lazarus_on_Linux.md>) on a Raspberry Pi is no different from other Linuxes, except, perhaps, for the [ memory issue](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi"). 

  * [![Lazarus "out of the box" on Raspberry Pi 2](https://wiki.freepascal.org/images/c/cc/Lazarus_0_9_30_4_on_RPi_2.png)](</File:Lazarus_0_9_30_4_on_RPi_2.png> "Lazarus "out of the box" on Raspberry Pi 2")

Lazarus "out of the box" on Raspberry Pi 2 




### Cross compiling for the Raspberry Pi from Windows

1\. Using fpcup 

One way is to use fpcup to set up a cross compiler; follow these instructions: [fpcup#Linux_ARM_cross_compiler](<fpcup.md> "fpcup")

2\. Using scripts 

Alternatively, for a more manual approach using batch files, you can follow these steps. 

2.1 Prerequisites 
    
    
    FPC 2.7.1 or higher installed with source code
    Install the Windows version from the Linaro binutils for linux gnueabihf into %FPCPATH%/bin/win32-armhf-linux [[1]](<https://launchpad.net/linaro-toolchain-binaries/trunk/2013.10/+download/gcc-linaro-arm-linux-gnueabihf-4.8-2013.10_win32.zip>)
    

2.2 Example Windows Batch file (adapt paths as needed) 
    
    
    set PATH=C:\pp\bin\i386-win32;%PATH%;
    set FPCMAKEPATH=C:/pp
    set FPCPATH=C:/pp
    set OUTPATH=C:/pp271
    %FPCMAKEPATH%/bin/i386-win32/make distclean OS_TARGET=linux CPU_TARGET=arm  CROSSBINDIR=%FPCPATH%/bin/win32-armhf-linux CROSSOPT="-CpARMV6 -CfVFPV2 -OoFASTMATH" FPC=%FPCPATH%/bin/i386-win32/ppc386.exe
    
    %FPCMAKEPATH%/bin/i386-win32/make all OS_TARGET=linux CPU_TARGET=arm CROSSBINDIR=%FPCPATH%/bin/win32-armhf-linux CROSSOPT="-CpARMV6 -CfVFPV2 -OoFASTMATH" FPC=%FPCPATH%/bin/i386-win32/ppc386.exe
    if errorlevel 1 goto quit
    %FPCMAKEPATH%/bin/i386-win32/make crossinstall CROSSBINDIR=%FPCPATH%/bin/win32-armhf-linux CROSSOPT="-CpARMV6 -CfVFPV2 -OoFASTMATH" OS_TARGET=linux CPU_TARGET=arm FPC=%FPCPATH%/bin/i386-win32/ppc386.exe INSTALL_BASEDIR=%OUTPATH%
    
    :quit
    pause
    

With the resulting ppcrossarm.exe and ARM RTL you will be able to build a cross Lazarus version as usual and compile FPC projects for the Raspberry Pi and other armhf devices. 

Remember that not all - especially Windows - libraries are available for Linux arm. 

### Cross compiling for the Raspberry Pi from Linux

Please see this page [https://wiki.freepascal.org/Cross_Compile_to_RasPi_from_Linux](<Cross_Compile_to_RasPi_from_Linux.md>)

## Accessing external hardware

[![Raspberry Pi pinout](https://wiki.freepascal.org/images/5/50/rpi_pinout_all_50.png)](</File:rpi_pinout_all_50.png>)

[](</File:rpi_pinout_all_50.png> "Enlarge")

Raspberry Pi pinout of external connectors

One of the goals in the development of Raspberry Pi was to facilitate effortless access to external devices like sensors and actuators. There are many ways to access the I/O facilities from Lazarus and Free Pascal: 

  1. [Direct access](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi") using the [BaseUnix](</index.php?title=BaseUnix&action=edit&redlink=1> "BaseUnix \(page does not exist\)") unit
  2. Access through [encapsulated shell calls](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi")
  3. Access through the [wiringPi library](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi").
  4. Access through Unit [rpi_hal](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi").
  5. Access through Unit [PiGpio](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi").
  6. Access through the [PascalIO](<https://github.com/SAmeis/pascalio>) library.
  7. Access through the [PXL](<http://asphyre.net/products/pxl>) library.



### Native hardware access

[![](https://wiki.freepascal.org/images/f/fd/testprogram_for_GPIO_on_RPI_annotated.png)](</File:testprogram_for_GPIO_on_RPI_annotated.png>)

[](</File:testprogram_for_GPIO_on_RPI_annotated.png> "Enlarge")

Simple test program for accessing the GPIO port on Raspberry Pi

[![](https://wiki.freepascal.org/images/0/00/rpi_testcircuit_1d.png)](</File:rpi_testcircuit_1d.png>)

[](</File:rpi_testcircuit_1d.png> "Enlarge")

Test circuit for GPIO access with the described program

[![](https://wiki.freepascal.org/images/1/1f/rpi_testcircuit_1p_annotaded.png)](</File:rpi_testcircuit_1p_annotaded.png>)

[](</File:rpi_testcircuit_1p_annotaded.png> "Enlarge")

Simple demo implementation of the circuit from above on a breadboard

This method provides access to external hardware that doesn't require additional libraries. The only requirement is the BaseUnix library that is part of Free Pascal's RTL. 

#### Switching a device via the GPIO port

The following example lists a simple program that controls the GPIO pin 17 as output to switch an LED, transistor or relays. This program contains a [ToggleBox](<http://lazarus-ccr.sourceforge.net/docs/lcl/stdctrls/ttogglebox.html> "doc:lcl/stdctrls/ttogglebox.html") with name _GPIO17ToggleBox_ and for logging return codes a [TMemo](<http://lazarus-ccr.sourceforge.net/docs/lcl/stdctrls/tmemo.html> "doc:lcl/stdctrls/tmemo.html") called _LogMemo_. 

For the example, the anode of a LED has been connected with Pin 11 on the Pi's connector (corresponding to GPIO pin 17 of the BCM2835 SOC) and the LED's cathode was wired via a 68 Ohm resistor to pin 6 of the connector (GND) as previously described by Upton and Halfacree. Subsequently, the LED may be switched on and off with the application's toggle box. 

The code may at first require to be run as root, i.e. either from a root account (not recommended) or via **su**. A better option is to add the user to the gpio group, the i2c group and the spi group. 
    
    
    sudo adduser pi gpio
    sudo adduser pi i2c
    sudo adduser pi spi
    

_Controlling unit:_
    
    
    unit Unit1;
    
    {Demo application for GPIO on Raspberry Pi}
    {Inspired by the Python input/output demo application by Gareth Halfacree}
    {written for the Raspberry Pi User Guide, ISBN 978-1-118-46446-5}
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
      Unix, BaseUnix;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        LogMemo: TMemo;
        GPIO17ToggleBox: TToggleBox;
        procedure FormActivate(Sender: TObject);
        procedure FormClose(Sender: TObject; var CloseAction: TCloseAction);
        procedure GPIO17ToggleBoxChange(Sender: TObject);
      private
        { private declarations }
      public
        { public declarations }
      end;
    
    const
      PIN_17: PChar = '17';
      PIN_ON: PChar = '1';
      PIN_OFF: PChar = '0';
      OUT_DIRECTION: PChar = 'out';
    
    var
      Form1: TForm1;
      gReturnCode: longint; {stores the result of the IO operation}
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.FormActivate(Sender: TObject);
    var
      fileDesc: integer;
    begin
      { Prepare SoC pin 17 (pin 11 on GPIO port) for access: }
      try
        fileDesc := fpopen('/sys/class/gpio/export', O_WrOnly);
        gReturnCode := fpwrite(fileDesc, PIN_17[0], 2);
        LogMemo.Lines.Add(IntToStr(gReturnCode));
      finally
        gReturnCode := fpclose(fileDesc);
        LogMemo.Lines.Add(IntToStr(gReturnCode));
      end;
      { Set SoC pin 17 as output: }
      try
        fileDesc := fpopen('/sys/class/gpio/gpio17/direction', O_WrOnly);
        gReturnCode := fpwrite(fileDesc, OUT_DIRECTION[0], 3);
        LogMemo.Lines.Add(IntToStr(gReturnCode));
      finally
        gReturnCode := fpclose(fileDesc);
        LogMemo.Lines.Add(IntToStr(gReturnCode));
      end;
    end;
    
    procedure TForm1.FormClose(Sender: TObject; var CloseAction: TCloseAction);
    var
      fileDesc: integer;
    begin
      { Free SoC pin 17: }
      try
        fileDesc := fpopen('/sys/class/gpio/unexport', O_WrOnly);
        gReturnCode := fpwrite(fileDesc, PIN_17[0], 2);
        LogMemo.Lines.Add(IntToStr(gReturnCode));
      finally
        gReturnCode := fpclose(fileDesc);
        LogMemo.Lines.Add(IntToStr(gReturnCode));
      end;
    end;
    
    procedure TForm1.GPIO17ToggleBoxChange(Sender: TObject);
    var
      fileDesc: integer;
    begin
      if GPIO17ToggleBox.Checked then
      begin
        { Swith SoC pin 17 on: }
        try
          fileDesc := fpopen('/sys/class/gpio/gpio17/value', O_WrOnly);
          gReturnCode := fpwrite(fileDesc, PIN_ON[0], 1);
          LogMemo.Lines.Add(IntToStr(gReturnCode));
        finally
          gReturnCode := fpclose(fileDesc);
          LogMemo.Lines.Add(IntToStr(gReturnCode));
        end;
      end
      else
      begin
        { Switch SoC pin 17 off: }
        try
          fileDesc := fpopen('/sys/class/gpio/gpio17/value', O_WrOnly);
          gReturnCode := fpwrite(fileDesc, PIN_OFF[0], 1);
          LogMemo.Lines.Add(IntToStr(gReturnCode));
        finally
          gReturnCode := fpclose(fileDesc);
          LogMemo.Lines.Add(IntToStr(gReturnCode));
        end;
      end;
    end;
    
    end.
    

_Main program:_
    
    
    program io_test;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Interfaces, // this includes the LCL widgetset
      Forms, Unit1
      { you can add units after this };
    
    {$R *.res}
    
    begin
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    end.
    

#### Reading the status of a pin

[![](https://wiki.freepascal.org/images/7/73/2013-04-28-193243_1280x1024_scrot.png)](</File:2013-04-28-193243_1280x1024_scrot.png>)

[](</File:2013-04-28-193243_1280x1024_scrot.png> "Enlarge")

Demo program for reading the status of a GPIO pin

[![](https://wiki.freepascal.org/images/4/4d/rpi_testcircuit_2d.png)](</File:rpi_testcircuit_2d.png>)

[](</File:rpi_testcircuit_2d.png> "Enlarge")

Test circuit for GPIO access with the described program

[![](https://wiki.freepascal.org/images/5/54/rpi_testcircuit_2p_annotaded.png)](</File:rpi_testcircuit_2p_annotaded.png>)

[](</File:rpi_testcircuit_2p_annotaded.png> "Enlarge")

Possible implementation of this test circuit

Of course it is also possible to read the status of e.g. a switch that is connected to the GPIO port. 

The following simple example is very similar to the previous one. It controls the GPIO pin 18 as input for a binary device like a switch, transistor or relais. This program contains a [CheckBox](<http://lazarus-ccr.sourceforge.net/docs/lcl/stdctrls/tcheckbox.html> "doc:lcl/stdctrls/tcheckbox.html") with name _GPIO18CheckBox_ and for logging return codes a [TMemo](<http://lazarus-ccr.sourceforge.net/docs/lcl/stdctrls/tmemo.html> "doc:lcl/stdctrls/tmemo.html") called _LogMemo_. 

For the example, one pole of a push-button has been connected to Pin 12 on the Pi's connector (corresponding to GPIO pin 18 of the BCM2835 SOC) and via a 10k Ohm pull-up resistor with pin 1 (+3.3V, see wiring diagram). The other pole has been wired to pin 6 of the connector (GND). The program senses the status of the button and correspondingly switches the checkbox on or off, respectively. 

Note that the potential of pin 18 is high if the button is released (by virtue of the connection to pin 1 via the pull-up resistor) and low if it is pressed (since in this situation pin 18 is connected to GND via the switch). Therefore, the GPIO pin signals 0 if the button is pressed and 1 if it is released. 

This program has again to be executed as root. 

_Controlling unit:_
    
    
    unit Unit1;
    
    {Demo application for GPIO on Raspberry Pi}
    {Inspired by the Python input/output demo application by Gareth Halfacree}
    {written for the Raspberry Pi User Guide, ISBN 978-1-118-46446-5}
    
    {This application reads the status of a push-button}
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
      ButtonPanel, Unix, BaseUnix;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        ApplicationProperties1: TApplicationProperties;
        GPIO18CheckBox: TCheckBox;
        LogMemo: TMemo;
        procedure ApplicationProperties1Idle(Sender: TObject; var Done: Boolean);
        procedure FormActivate(Sender: TObject);
        procedure FormClose(Sender: TObject; var CloseAction: TCloseAction);
      private
        { private declarations }
      public
        { public declarations }
      end;
    
    const
      PIN_18: PChar = '18';
      IN_DIRECTION: PChar = 'in';
    
    var
      Form1: TForm1;
      gReturnCode: longint; {stores the result of the IO operation}
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.FormActivate(Sender: TObject);
    var
      fileDesc: integer;
    begin
      { Prepare SoC pin 18 (pin 12 on GPIO port) for access: }
      try
        fileDesc := fpopen('/sys/class/gpio/export', O_WrOnly);
        gReturnCode := fpwrite(fileDesc, PIN_18[0], 2);
        LogMemo.Lines.Add(IntToStr(gReturnCode));
      finally
        gReturnCode := fpclose(fileDesc);
        LogMemo.Lines.Add(IntToStr(gReturnCode));
      end;
      { Set SoC pin 18 as input: }
      try
        fileDesc := fpopen('/sys/class/gpio/gpio18/direction', O_WrOnly);
        gReturnCode := fpwrite(fileDesc, IN_DIRECTION[0], 2);
        LogMemo.Lines.Add(IntToStr(gReturnCode));
      finally
        gReturnCode := fpclose(fileDesc);
        LogMemo.Lines.Add(IntToStr(gReturnCode));
      end;
    end;
    
    procedure TForm1.ApplicationProperties1Idle(Sender: TObject; var Done: Boolean);
    var
      fileDesc: integer;
      buttonStatus: string[1] = '1';
    begin
      try
        { Open SoC pin 18 (pin 12 on GPIO port) in read-only mode: }
        fileDesc := fpopen('/sys/class/gpio/gpio18/value', O_RdOnly);
        if fileDesc > 0 then
        begin
          { Read status of this pin (0: button pressed, 1: button released): }
          gReturnCode := fpread(fileDesc, buttonStatus[1], 1);
          LogMemo.Lines.Add(IntToStr(gReturnCode) + ': ' + buttonStatus);
          LogMemo.SelStart := Length(LogMemo.Lines.Text) - 1;
          if buttonStatus = '0' then
            GPIO18CheckBox.Checked := true
          else
            GPIO18CheckBox.Checked := false;
        end;
      finally
        { Close SoC pin 18 (pin 12 on GPIO port) }
        gReturnCode := fpclose(fileDesc);
        LogMemo.Lines.Add(IntToStr(gReturnCode));
        LogMemo.SelStart := Length(LogMemo.Lines.Text) - 1;
      end;
    end;
    
    procedure TForm1.FormClose(Sender: TObject; var CloseAction: TCloseAction);
    var
      fileDesc: integer;
    begin
      { Free SoC pin 18: }
      try
        fileDesc := fpopen('/sys/class/gpio/unexport', O_WrOnly);
        gReturnCode := fpwrite(fileDesc, PIN_18[0], 2);
        LogMemo.Lines.Add(IntToStr(gReturnCode));
      finally
        gReturnCode := fpclose(fileDesc);
        LogMemo.Lines.Add(IntToStr(gReturnCode));
      end;
    end;
    
    end.
    

The main program is identical to that of the example from above. 

### Hardware access via encapsulated shell calls

Another way to access the hardware is by encapsulating terminal commands. This is achieved by using the [fpsystem](<Executing_External_Programs.md> "Executing External Programs") function. This method gives access to functions that are not supported by the BaseUnix unit. The following code implements a program that has the same functionality as the program resulting from the first listing above. 

_Controlling unit:_
    
    
    unit Unit1;
    
    {Demo application for GPIO on Raspberry Pi}
    {Inspired by the Python input/output demo application by Gareth Halfacree}
    {written for the Raspberry Pi User Guide, ISBN 978-1-118-46446-5}
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls, Unix;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        LogMemo: TMemo;
        GPIO17ToggleBox: TToggleBox;
        procedure FormActivate(Sender: TObject);
        procedure FormClose(Sender: TObject; var CloseAction: TCloseAction);
        procedure GPIO17ToggleBoxChange(Sender: TObject);
      private
        { private declarations }
      public
        { public declarations }
      end;
    
    var
      Form1: TForm1;
      gReturnCode: longint; {stores the result of the IO operation}
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.FormActivate(Sender: TObject);
    begin
      { Prepare SoC pin 17 (pin 11 on GPIO port) for access: }
      gReturnCode := fpsystem('echo "17" > /sys/class/gpio/export');
      LogMemo.Lines.Add(IntToStr(gReturnCode));
      { Set SoC pin 17 as output: }
      gReturnCode := fpsystem('echo "out" > /sys/class/gpio/gpio17/direction');
      LogMemo.Lines.Add(IntToStr(gReturnCode));
    end;
    
    procedure TForm1.FormClose(Sender: TObject; var CloseAction: TCloseAction);
    begin
      { Free SoC pin 17: }
      gReturnCode := fpsystem('echo "17" > /sys/class/gpio/unexport');
      LogMemo.Lines.Add(IntToStr(gReturnCode));
    end;
    
    procedure TForm1.GPIO17ToggleBoxChange(Sender: TObject);
    begin
      if GPIO17ToggleBox.Checked then
      begin
        { Swith SoC pin 17 on: }
        gReturnCode := fpsystem('echo "1" > /sys/class/gpio/gpio17/value');
        LogMemo.Lines.Add(IntToStr(gReturnCode));
      end
      else
      begin
        { Switch SoC pin 17 off: }
        gReturnCode := fpsystem('echo "0" > /sys/class/gpio/gpio17/value');
        LogMemo.Lines.Add(IntToStr(gReturnCode));
      end;
    end;
    
    end.
    

The main program is identical to that of the example above. This program has to be executed with root privileges, too. 

### wiringPi procedures and functions

Alex Schaller's wrapper unit for Gordon Henderson's Arduino compatible wiringPi library provides a numbering scheme that resembles that of Arduino boards. 

**Function wiringPiSetup:longint:** Initializes wiringPi system using the wiringPi pin numbering scheme. 

**Procedure wiringPiGpioMode(mode:longint):** Initializes wiringPi system with the Broadcom GPIO pin numbering scheme. 

**Procedure pullUpDnControl(pin:longint;** pud:longint): controls the internal pull-up/down resistors on a GPIO pin. 

**Procedure pinMode(pin:longint; mode:longint):** sets the mode of a pin to either INPUT, OUTPUT, or PWM_OUTPUT. 

**Procedure digitalWrite(pin:longint; value:longint):** sets an output bit. 

**Procedure pwmWrite(pin:longint; value:longint):** sets an output PWM value between 0 and 1024. 

**Function digitalRead(pin:longint):longint:** reads the value of a given Pin, returning 1 or 0. 

**Procedure delay(howLong:dword):** waits for at least howLong milliseconds. 

**Procedure delayMicroseconds(howLong:dword):** waits for at least howLong microseconds. 

**Function millis:dword:** returns the number of milliseconds since the program called one of the wiringPiSetup functions. 

### rpi_hal-Hardware Abstraction Library (GPIO, I2C and SPI functions and procedures)

This Unit with around 1700 Lines of Code provided by Stefan Fischer, delivers procedures and functions to access the rpi HW I2C, SPI and GPIO: 

Just an excerpt of the available functions and procedures: 

**procedure gpio_set_pin (pin:longword;highlevel:boolean);** { Set RPi GPIO pin to high or low level; Speed @ 700MHz -> 0.65MHz } 

**function gpio_get_PIN (pin:longword):boolean;** { Get RPi GPIO pin Level is true when Pin level is '1'; false when '0'; Speed @ 700MHz -> 1.17MHz } 

**procedure gpio_set_input (pin:longword);** { Set RPi GPIO pin to input direction } 

**procedure gpio_set_output(pin:longword);** { Set RPi GPIO pin to output direction } 

**procedure gpio_set_alt (pin,altfunc:longword);** { Set RPi GPIO pin to alternate function nr. 0..5 } 

**procedure gpio_set_gppud (mask:longword);** { set RPi GPIO Pull-up/down Register (GPPUD) with mask } 

... 

**function rpi_snr :string;** { delivers SNR: 0000000012345678 } 

**function rpi_hw :string;** { delivers Processor Type: BCM2708 } 

**function rpi_proc:string;** { ARMv6-compatible processor rev 7 (v6l) } 

... 

**function i2c_bus_write(baseadr,reg:word; var data:databuf_t; lgt:byte; testnr:integer) : integer;**

**function i2c_bus_read (baseadr,reg:word; var data:databuf_t; lgt:byte; testnr:integer) : integer;**

**function i2c_string_read(baseadr,reg:word; var data:databuf_t; lgt:byte; testnr:integer) : string;**

**function i2c_string_write(baseadr,reg:word; s:string; testnr:integer) : integer;**

... 

**procedure SPI_Write(devnum:byte; reg,data:word);**

**function SPI_Read(devnum:byte; reg:word) : byte;**

**procedure SPI_BurstRead2Buffer (devnum,start_reg:byte; xferlen:longword);**

**procedure SPI_BurstWriteBuffer (devnum,start_reg:byte; xferlen:longword);** { Write 'len' Bytes from Buffer SPI Dev startig at address 'reg' } 

... 

  
_Test Program (testrpi.pas):_
    
    
    //Simple Test program, which is using rpi_hal;
    
      program testrpi;
      uses rpi_hal;
      begin
        writeln('Show CPU-Info, RPI-HW-Info and Registers:');
        rpi_show_all_info;
        writeln('Let Status LED Blink. Using GPIO functions:');
        GPIO_PIN_TOGGLE_TEST;
        writeln('Test SPI Read function. (piggy back board with installed RFM22B Module is required!)');
        Test_SPI;
      end.
    

### PiGpio Low-level native pascal unit (GPIO control instead of wiringPi c library)

_This Unit (pigpio.pas[[2]](<https://docs.google.com/file/d/0B-b-5ooIWPvGN1BIcEJzRDgyeUE/edit?usp=sharing>)) with 270 Lines of Code provided by Gabor Szollosi, works very fast (for ex. GPIO pin output switching frequency 8 MHz). To utilize this unit, you must have access to /dev/mem; otherwise, you'll encounter a SIGSEGV exception. There are several methods to obtain the necessary permissions. One of the simple methods is to launch Lazarus as the root user (though this is not generally recommended). For better approaches ,see <https://forum.lazarus.freepascal.org/index.php/topic,48834.msg352366.html#msg352366>.:_
    
    
    unit PiGpio;
    {
     BCM2835 GPIO Registry Driver, also can use to manipulate cpu other registry areas
    
     This code is tested only Broadcom bcm2835 cpu, different arm cpus may need different
     gpio driver implementation
    
     2013 Gabor Szollosi
    }
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils;
    
    const
      REG_GPIO = $3F000;// bcm2835 gpio register 0x2000 0000 (RPi1), 0x3F00 0000 (RPi2)
      //REG_GPIO = $FE000;//for bcm2711(RPI4) 
      // new fpMap uses page offset, one page is 4096 bytes = 0x1000 so simply calculate 0x2000 0000 / 0x1000  = 0x2000 0
      PAGE_SIZE = 4096;
      BLOCK_SIZE = 4096;
      // The BCM2835 has 54 GPIO pins.
      //	BCM2835 data sheet, Page 90 onwards.
      //	There are 6 control registers, each control the functions of a block
      //	of 10 pins.
    
      CLOCK_BASE = (REG_GPIO + $101);
      GPIO_BASE =  (REG_GPIO + $200);
      GPIO_PWM =   (REG_GPIO + $20C);
    
         INPUT = 0;
         OUTPUT = 1;
         PWM_OUTPUT = 2;
         LOW = False;
         HIGH = True;
         PUD_OFF = 0;
         PUD_DOWN = 1;
         PUD_UP = 2;
    
       // PWM
    
      PWM_CONTROL = 0;
      PWM_STATUS  = 4;
      PWM0_RANGE  = 16;
      PWM0_DATA   = 20;
      PWM1_RANGE  = 32;
      PWM1_DATA   = 36;
    
      PWMCLK_CNTL =	160;
      PWMCLK_DIV  =	164;
    
      PWM1_MS_MODE    = $8000;  // Run in MS mode
      PWM1_USEFIFO    = $2000; // Data from FIFO
      PWM1_REVPOLAR   = $1000;  // Reverse polarity
      PWM1_OFFSTATE   = $0800;  // Ouput Off state
      PWM1_REPEATFF   = $0400;  // Repeat last value if FIFO empty
      PWM1_SERIAL     = $0200;  // Run in serial mode
      PWM1_ENABLE     = $0100;  // Channel Enable
    
      PWM0_MS_MODE    = $0080;  // Run in MS mode
      PWM0_USEFIFO    = $0020;  // Data from FIFO
      PWM0_REVPOLAR   = $0010;  // Reverse polarity
      PWM0_OFFSTATE   = $0008;  // Ouput Off state
      PWM0_REPEATFF   = $0004;  // Repeat last value if FIFO empty
      PWM0_SERIAL     = $0002;  // Run in serial mode
      PWM0_ENABLE     = $0001;  // Channel Enable
    
    
    type
    
      { TIoPort }
    
      TIoPort = class // IO bank object
      private         //
    
        //function get_pinDirection(aPin: TGpIoPin): TGpioPinConf;
    
      public
       FGpio: ^LongWord;
       FClk: ^LongWord;
       FPwm: ^LongWord;
       procedure SetPinMode(gpin, mode: byte);
       function GetBit(gpin : byte):boolean;inline; // gets pin bit}
       procedure ClearBit(gpin : byte);inline;// write pin to 0
       procedure SetBit(gpin : byte);inline;// write pin to 1
       procedure SetPullMode(gpin, mode: byte);
       procedure PwmWrite(gpin : byte; value : LongWord);inline;// write pin to pwm value
      end;
    
      { TIoDriver }
    
      TIoDriver = class
      private
    
      public
        destructor Destroy;override;
        function MapIo:boolean;// creates io memory mapping
        procedure UnmapIoRegisrty(FMap: TIoPort);// close io memory mapping
        function CreatePort(PortGpio, PortClk, PortPwm: LongWord):TIoPort; // create new IO port
      end;
    
    var
    
        fd: integer;// /dev/mem file handle
    procedure delayNanoseconds (howLong : LongWord);
    
    
    implementation
    
    uses
      baseUnix, Unix;
    
    procedure delayNanoseconds (howLong : LongWord);
    var
      sleeper, dummy : timespec;
    begin
      sleeper.tv_sec  := 0 ;
      sleeper.tv_nsec := howLong ;
      fpnanosleep (@sleeper,@dummy) ;
    end;
    { TIoDriver }
    //*******************************************************************************
    destructor TIoDriver.Destroy;
    begin
      inherited Destroy;
    end;
    //*******************************************************************************
    function TIoDriver.MapIo: boolean;
    begin
     Result := True;
     fd := fpopen('/dev/mem', O_RdWr or O_Sync); // Open the master /dev/memory device
      if fd < 0 then
      begin
        Result := False; // unsuccessful memory mapping
      end;
     //
    end;
    //*******************************************************************************
    procedure TIoDriver.UnmapIoRegisrty(FMap:TIoPort);
    begin
      if FMap.FGpio <> nil then
     begin
       fpMUnmap(FMap.FGpio,PAGE_SIZE);
       FMap.FGpio := Nil;
     end;
     if FMap.FClk <> nil then
     begin
       fpMUnmap(FMap.FClk,PAGE_SIZE);
       FMap.FClk := Nil;
     end;
     if FMap.FPwm <> nil then
     begin
       fpMUnmap(FMap.FPwm ,PAGE_SIZE);
       FMap.FPwm := Nil;
     end;
    end;
    //*******************************************************************************
    function TIoDriver.CreatePort(PortGpio, PortClk, PortPwm: LongWord): TIoPort;
    begin
      Result := TIoPort.Create;// new io port, pascal calls new fpMap, where offst is page sized 4096 bytes!!!
      Result.FGpio := FpMmap(Nil, PAGE_SIZE, PROT_READ or PROT_WRITE, MAP_SHARED, fd, PortGpio); // port config gpio memory
      Result.FClk:= FpMmap(Nil, PAGE_SIZE, PROT_READ or PROT_WRITE, MAP_SHARED, fd, PortClk);; // port clk
      Result.FPwm:= FpMmap(Nil, PAGE_SIZE, PROT_READ or PROT_WRITE, MAP_SHARED, fd, PortPwm);; // port pwm
    end;
    //*******************************************************************************
    procedure TIoPort.SetPinMode(gpin, mode: byte);
    var
      fSel, shift, alt : byte;
      gpiof, clkf, pwmf : ^LongWord;
    begin
      fSel := (gpin div 10)*4 ;  //Select Gpfsel 0 to 5 register
      shift := (gpin mod 10)*3 ;  //0-9 pin shift
      gpiof := Pointer(LongWord(Self.FGpio)+fSel);
      if (mode = INPUT) then
        gpiof^ := gpiof^ and ($FFFFFFFF - (7 shl shift))  //7 shl shift komplemens - Sets bits to zero = input
      else if (mode = OUTPUT) then
      begin
        gpiof^ := gpiof^ and ($FFFFFFFF - (7 shl shift)) or (1 shl shift);
      end
      else if (mode = PWM_OUTPUT) then
      begin
        Case gpin of
          12,13,40,41,45 : alt:= 4 ;
          18,19          : alt:= 2 ;
          else alt:= 0 ;
        end;
        If alt > 0 then
        begin
          gpiof^ := gpiof^ and ($FFFFFFFF - (7 shl shift)) or (alt shl shift);
          clkf := Pointer(LongWord(Self.FClk)+PWMCLK_CNTL);
          clkf^ := $5A000011 or (1 shl 5) ;                  //stop clock
          delayNanoseconds(200);
          clkf := Pointer(LongWord(Self.FClk)+PWMCLK_DIV);
          clkf^ := $5A000000 or (32 shl 12) ;	// set pwm clock div to 32 (19.2/3 = 600KHz)
          clkf := Pointer(LongWord(Self.FClk)+PWMCLK_CNTL);
          clkf^ := $5A000011 ;                               //start clock
          Self.ClearBit(gpin);
          pwmf := Pointer(LongWord(Self.FPwm)+PWM_CONTROL);
          pwmf^ := 0 ;		 	        // Disable PWM
          delayNanoseconds(200);
          pwmf := Pointer(LongWord(Self.FPwm)+PWM0_RANGE);
          pwmf^ := $400 ;                             //max: 1023
          delayNanoseconds(200);
          pwmf := Pointer(LongWord(Self.FPwm)+PWM1_RANGE);
          pwmf^ := $400 ;                             //max: 1023
          delayNanoseconds(200);
          // Enable PWMs
          pwmf := Pointer(LongWord(Self.FPwm)+PWM0_DATA);
          pwmf^ := 0 ;                                //start value
          pwmf := Pointer(LongWord(Self.FPwm)+PWM1_DATA);
          pwmf^ := 0 ;                                //start value
          pwmf := Pointer(LongWord(Self.FPwm)+PWM_CONTROL);
          pwmf^ := PWM0_ENABLE or PWM1_ENABLE ;
        end;
      end;
    end;
    //*******************************************************************************
    procedure TIoPort.SetBit(gpin : byte);
    var
       gpiof : ^LongWord;
    begin
      gpiof := Pointer(LongWord(Self.FGpio) + 28 + (gpin shr 5) shl 2);
      gpiof^ := 1 shl gpin;
    end;
    //*******************************************************************************
    procedure TIoPort.ClearBit(gpin : byte);
    var
       gpiof : ^LongWord;
    begin
      gpiof := Pointer(LongWord(Self.FGpio) + 40 + (gpin shr 5) shl 2);
      gpiof^ := 1 shl gpin;
    end;
    //*******************************************************************************
    function TIoPort.GetBit(gpin : byte):boolean;
    var
       gpiof : ^LongWord;
    begin
      gpiof := Pointer(LongWord(Self.FGpio) + 52 + (gpin shr 5) shl 2);
      if (gpiof^ and (1 shl gpin)) = 0 then Result := False else Result := True;
    end;
    //*******************************************************************************
    procedure TIoPort.SetPullMode(gpin, mode: byte);
    var
       pudf, pudclkf : ^LongWord;
    begin
      pudf := Pointer(LongWord(Self.FGpio) + 148 );
      pudf^ := mode;   //mode = 0, 1, 2 :Off, Down, Up
      delayNanoseconds(200);
      pudclkf := Pointer(LongWord(Self.FGpio) + 152 + (gpin shr 5) shl 2);
      pudclkf^ := 1 shl gpin ;
      delayNanoseconds(200);
      pudf^ := 0 ;
      pudclkf^ := 0 ;
    end;
    //*******************************************************************************
    procedure TIoPort.PwmWrite(gpin : byte; value : Longword);
    var
       pwmf : ^LongWord;
       port : byte;
    begin
      Case gpin of
          12,18,40 : port:= PWM0_DATA ;
          13,19,41,45 : port:= PWM1_DATA ;
          else exit;
      end;
      pwmf := Pointer(LongWord(Self.FPwm) + port);
      pwmf^ := value and $FFFFFBFF; // $400 complemens
    end;
    //*******************************************************************************
    end.
    

_Controlling Lazarus unit:(Project files[[3]](<https://drive.google.com/folderview?id=0B-b-5ooIWPvGU043ZTZ0cUc2YkE&usp=sharing>))_
    
    
    unit Unit1;
     
    {Demo application for GPIO on Raspberry Pi}
    {Inspired by the Python input/output demo application by Gareth Halfacree}
    {written for the Raspberry Pi User Guide, ISBN 978-1-118-46446-5}
     
    {$mode objfpc}{$H+}
     
    interface
     
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
      ExtCtrls, Unix, PiGpio;
     
    type
     
      { TForm1 }
     
      TForm1 = class(TForm)
        GPIO25In: TButton;
        SpeedButton: TButton;
        LogMemo: TMemo;
        GPIO23switch: TToggleBox;
        Timer1: TTimer;
        GPIO18Pwm: TToggleBox;
        Direction: TToggleBox;
        procedure DirectionChange(Sender: TObject);
        procedure GPIO18PwmChange(Sender: TObject);
        procedure GPIO25InClick(Sender: TObject);
        procedure FormActivate(Sender: TObject);
        procedure FormClose(Sender: TObject; var CloseAction: TCloseAction);
        procedure GPIO23switchChange(Sender: TObject);
        procedure SpeedButtonClick(Sender: TObject);
        procedure Timer1Timer(Sender: TObject);
      private
        { private declarations }
      public
        { public declarations }
      end;
     
    const
         INPUT = 0;
         OUTPUT = 1;
         PWM_OUTPUT = 2;
         LOW = False;
         HIGH = True;
         PUD_OFF = 0;
         PUD_DOWN = 1;
         PUD_UP = 2;
     // Convert Raspberry Pi P1 pins (Px) to GPIO port
         P3 = 0;
         P5 = 1;
         P7 = 4;
         P8 = 14;
         P10 = 15;
         P11 = 17;
         P12 = 18;
         P13 = 21;
         P15 = 22;
         P16 = 23;
         P18 = 24;
         P19 = 10;
         P21 = 9;
         P22 = 25;
         P23 = 11;
         P24 = 8;
         P26 = 7;
     
    var
      Form1: TForm1;
      GPIO_Driver: TIoDriver;
      GpF: TIoPort;
      PWM :Boolean;
      i, d : integer;
      Pin,Pout,Ppwm : byte;
     
    implementation
     
    {$R *.lfm}
     
    { TForm1 }
     
    procedure TForm1.FormActivate(Sender: TObject);
    begin
      if not GPIO_Driver.MapIo then
      begin
         LogMemo.Lines.Add('Error mapping gpio registry');
      end
      else
      begin
        GpF := GpIo_Driver.CreatePort(GPIO_BASE, CLOCK_BASE, GPIO_PWM);
      end ;
      Timer1.Enabled:= True;
      Timer1.Interval:= 25;     //25 ms controll interval
      Pin := P22;
      Pout := P16;
      Ppwm := P12;
      i:=1;
      GpF.SetPinMode(Pout,OUTPUT);
      GpF.SetPinMode(Pin,INPUT);
      GpF.SetPullMode(Pin,PUD_Up);    // Input PullUp High level
    end;
    
    procedure TForm1.GPIO25InClick(Sender: TObject);
    begin
      If GpF.GetBit(Pin) then LogMemo.Lines.Add('In: '+IntToStr(1))
                         else LogMemo.Lines.Add('In: '+IntToStr(0));
    end;
    
    procedure TForm1.GPIO18PwmChange(Sender: TObject);
    begin
      if GPIO18Pwm.Checked then
      begin
        GpF.SetPinMode(Ppwm,PWM_OUTPUT);
        PWM := True;                          //PWM on
      end
      else
      begin
        GpF.SetPinMode(Ppwm,INPUT);
        PWM := False;                          //PWM off
      end;
    end;
    
    procedure TForm1.DirectionChange(Sender: TObject);
    begin
      if Direction.Checked then d:=10 else d:=-10;
    end;
    
     
    procedure TForm1.FormClose(Sender: TObject; var CloseAction: TCloseAction);
    begin
        GpF.SetPinMode(Ppwm,INPUT);
        GpF.ClearBit(Pout);
        GpIo_Driver.UnmapIoRegisrty(GpF);
    end;
     
    procedure TForm1.GPIO23switchChange(Sender: TObject);
    
    Begin
      Timer1.Enabled := False;
      if GPIO23switch.Checked then
      begin
        GpF.SetBit(Pout); //Turn LED on
      end
      else
      begin
        GpF.ClearBit(Pout); //Turn LED off
      end;
      Timer1.Enabled := True;
    end;
    
    procedure TForm1.SpeedButtonClick(Sender: TObject);
    var
      i,p,k,v: longint;
      ido:TDateTime;
    begin
      ido:= Time;
      k:= TimeStampToMSecs(DateTimeToTimeStamp(ido));
      LogMemo.Lines.Add('Start: '+TimeToStr(ido));
      p:=10000000 ;
      For i:=1 to p  do Begin
        GpF.SetBit(P16);
        GpF.ClearBit(P16);
      end;
      ido:= Time;
      v:= TimeStampToMSecs(DateTimeToTimeStamp(ido));
      LogMemo.Lines.Add('Stop: '+TimeToStr(ido)+' Frequency: '+
                                    IntToStr(p div (v-k))+' kHz');
    end;
    
    procedure TForm1.Timer1Timer(Sender: TObject);
    begin
      If PWM then Begin
        If (d > 0) and (i+d < 1024) then begin
              i:=i+d;
              GpF.PwmWrite(Ppwm,i);
        end ;
        If (d < 0) and (i+d > -1) then begin
              i:=i+d;
              GpF.PwmWrite(Ppwm,i);
        end;
      end;
    end;
    
     
    end.
    

### PXL (Platform eXtended Library) for low level native access to GPIO, I²C, SPI, PWM, UART, V4L2, displays and sensors

  * Easy and fast access to GPIO, I²C, SPI, PWM, UART and high precision CPU timer.
  * V4L2 video/image capture using onboard and USB cameras. Serial cameras such as VC0706 and LSY201.
  * OpenGL ES hardware rendering with and without X server. Software rendering.
  * I²C and SPI displays such as SSD1306, SSD1351, PCB8544 (Nokia), HX8357 and ILI9340.
  * Character LCDs with direct pin wiring.
  * Support for Software SPI and UART (bit-banging).
  * Support for SC16IS7x0 (UART controller connected via I²C or SPI), including extra GPIO pins.
  * Support for sensors such as BMP180, DHT22, L3GD20, LSM303 and SHT10.
  * Other features like networking, math, image transformation, effects, drawing primitives...



_More info and download of[PXL library](<http://asphyre.net/products/pxl>)._

_The PinDrive property is not implemented in the latest download from Asphyre so I am posting the code for my implementation in case anyone needs it. I found that the definition for TPinDrive is incorrect for the BCM2837 chip so I added some code to change it. This could be accomplished more easily by changing the definition but that file could conceivably be used for multiple chips (and I don't control the code) so I made all changes in the code to set the drive mode._
    
    
    procedure TFastGPIO.SetPinDrive(const Pin: TPinIdentifier; const Value: TPinDrive);
    const
      GPPUDadr = $94;
      GPPclkadr = $98;
    type
      TBCM28PinDrive = (drvNone, drvPullDown, drvPullUp);
    var
      dValue: TBCM28PinDrive;
      PinBCM: TPinIdentifier;
      GPPUD, GPPclk: Pointer;
    begin
      case Value of
        TPinDrive.PullUp: dValue := TBCM28PinDrive.drvPullDown;
        TPinDrive.PullDown: dValue := TBCM28PinDrive.drvPullUp;
      else
        dValue := TBCM28PinDrive.drvNone;
    
      PinBCM := ProcessPinNumber(Pin);
    
      GPPUD := GetOffsetPointer(GPPUDadr);
      WriteMemSafe(GPPUD, Cardinal(dValue));
      FSystemCore.MicroDelay(1);
    
      GPPclk := GetOffsetPointer(GPPclkadr + (Cardinal(PinBCM) div 32) * 4);
      WriteMemSafe(GPPclk, 1 shl (PinBCM mod 32));
      FSystemCore.MicroDelay(1);
    
      WriteMemSafe(GPPUD, 0);
      WriteMemSafe(GPPclk, 0);
    end;
    

## References

  * Eben Upton and Gareth Halfacree. Raspberry Pi User Guide. John Wiley and Sons, Chichester 2012, ISBN 111846446X
  * B. J. Rao (2013) The Raspberry Pi, Pi Vision and Lazarus/FPC. [Blaise Pascal Magazine](<http://www.blaisepascal.eu>). 29: 14-21.



## External and Other Links

  * If you want to install the latest stable release of fpc and, additional and isolated, the trunk fpc compiler: you can read the following guide.



It was written using gentoo but this guide will be useful with any distro: [Install fpc on Raspberry with Gentoo](<Install_fpc_on_Raspberry_with_Gentoo.md> "Install fpc on Raspberry with Gentoo") (very out of date) 

  * [Raspberry Pi Foundation](<http://www.raspberrypi.org>)
  * [Raspberry Pi Spy: Raspberry Pi tutorials, scripts, help and downloads](<http://www.raspberrypi-spy.co.uk>)
  * [Lazarus wrapper unit for Gordon Henderson's wiringPi C library](<http://www.lazarus.freepascal.org/index.php/topic,17404.0.html>)
  * [Another wiringPi wrapper with waitForInterrupt() and wiringPiISR()](<https://github.com/laz2wiringpi/laz2wiringpi>)
  * [Yet another wiringPi wrapper with waitForInterrupt(), wiringPiISR(), i2c, spi and serial](<https://forum.lazarus.freepascal.org/index.php/topic,17404.msg405237.html#msg405237>)
  * [Pin layout of the wiringPi library](<https://projects.drogon.net/raspberry-pi/wiringpi/pins/>)
  * [wiringPi fork updated for latest hardware](<https://github.com/WiringPi/WiringPi>)
  * [Additional information on Lazarus and Raspberry Pi at eLinux.org](<http://www.elinux.org/Lazarus_on_RPi>)
  * [Getting Started with Pascal on the Pi](<http://www.pp4s.co.uk/main/gs-pi-intro.html>) (noted dead link in late 2023)
  * [Getting Started with Led, Switch and DC motor on Pi](<http://www.pp4s.co.uk/main/gs-pi-gpio-intro.html>) (noted dead link in late 2023)
  * [lazi2cdev - General i2c with drivers for ADS1015 ADC, MCP23017 IO, MCP4725 DAC, PCA9685 PWM and HD44780 LCD](<https://github.com/laz2wiringpi/lazI2cdev>)
  * [Lazberry Pi: Comprehensive information on Lazarus on the Raspberry Pi computer](<http://superbitysoft.co.uk/lazberrypi/>). (noted dead link in late 2023)
  * Stefan Fischer's [rpi_hal library](<https://github.com/rudiratlos/rpi-hal>) and [rpi_hal discussion topic](<http://www.lazarus.freepascal.org/index.php/topic,20991.0.html>) on Lazarus forum.
  * [Improved Lazarus Unit for fast access to GPIO](<http://www.meltonisl.com/pigpio.pas>).
  * [PXL - Platform eXtended Library](<http://asphyre.net/products/pxl>).
  * [RPIIO - Raspberry Pi GPIO and I2C library](<https://github.com/zipplet/rpiio>).

---

_Source: [https://wiki.freepascal.org/Lazarus_on_Raspberry_Pi](https://web.archive.org/web/20250216065141/https://wiki.freepascal.org/Lazarus_on_Raspberry_Pi)_
