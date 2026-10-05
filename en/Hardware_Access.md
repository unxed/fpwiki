# Hardware Access

│ **[Deutsch (de)](</Hardware_Access/de> "Hardware Access/de")** │  **English (en)** │  **[español (es)](</Hardware_Access/es> "Hardware Access/es")** │  **[français (fr)](</Hardware_Access/fr> "Hardware Access/fr")** │  **[magyar (hu)](</Hardware_Access/hu> "Hardware Access/hu")** │  **[日本語 (ja)](</Hardware_Access/ja> "Hardware Access/ja")** │  **[한국어 (ko)](</Hardware_Access/ko> "Hardware Access/ko")** │  **[polski (pl)](</Hardware_Access/pl> "Hardware Access/pl")** │  **[português (pt)](</Hardware_Access/pt> "Hardware Access/pt")** │  **[русский (ru)](<../ru/Hardware_Access.md> "Hardware Access/ru")** │  **[slovenčina (sk)](</Hardware_Access/sk> "Hardware Access/sk")** │  **[中文（中国大陆） (zh_CN)](</Hardware_Access/zh_CN> "Hardware Access/zh CN")** │    
****

## Contents

  * 1 Overview
  * 2 Parallel and Serial Comparison
  * 3 Parallel Communication
    * 3.1 Using inpout32.dll for Windows
    * 3.2 Using assembler on Windows 9x
    * 3.3 Troubleshooting on Windows
    * 3.4 Using 'ioperm' to access ports on Linux
    * 3.5 General UNIX hardware access
    * 3.6 Status and control
  * 4 Serial Communication
    * 4.1 Device Setup
    * 4.2 Synaser
    * 4.3 Visual Synapse
    * 4.4 5dpo
    * 4.5 TLazSerial
    * 4.6 FPC built in Serial unit
    * 4.7 TDataPort
    * 4.8 PXL
    * 4.9 Serial port names on Windows
  * 5 USB
    * 5.1 libusb
    * 5.2 FTDI
  * 6 Devices in general
  * 7 See also
  * 8 External Links



## Overview

This page describes various ways of accessing hardware devices on Lazarus. These devices include, but are not limited to: ISA, PCI, USB, parallel port, serial port. 

Uniform multi-platform access to hardware devices is not implemented by the Free Pascal Runtime Library (RTL) or by the LCL - the underlying operating systems are often different enough to make that very difficult. Therefore, this article will basically cover hardware access methods on different platforms. The code can be compiled on different environments using conditional compiles, like this: 
    
    
     uses
      Classes, SysUtils, LResources, Forms, Controls, Graphics, Dialogs, ExtCtrls,
     {$IFDEF WIN32}
       Windows;
     {$ENDIF}
     {$IFDEF Unix}
       ports;
     {$ENDIF}
    

## Parallel and Serial Comparison

ISA cards, PCI cards and the Parallel Port communicate with the computer using a **parallel** protocol. The Serial Port and USB devices work with a **serial** protocol. Because the processor and thus programming languages all work on a parallel approach to data, access to these kinds of protocols is easier to be implemented on the software side. When an Integer variable is accessed for example, its value can be accessed with a single command. With a serial protocol however, only one bit at a time can be accessed and the pieces need to be "glued" together to understand the data. 

Serial communication is difficult to be implemented directly, but it can be pretty easy if pre-made component are used. It is also harder on the hardware side, so many devices use specialised integrated circuits or microcontrolers to implement it. 

Now a brief comparison of hardware access protocols will be given: 

| Speed | Hardware implementation difficulty   
---|---|---  
Serial Port  | Very slow (< E5 bit/s)  | Medium   
Parallel Port  | Slow (~ E6 bit/s)  | Easy   
ISA Card  | Medium (~ E7 bit/s)  | Medium   
USB  | Medium (~ E7 bit/s)  | Hard   
PCI Card  | Very Fast (> E9 bit/s)  | Very Hard   
  
## Parallel Communication

### Using inpout32.dll for Windows

Windows has different ways to access hardware devices on the 9x and NT series. On the 9x series (95, 98, Me) programs can access the hardware directly, just like they did on DOS. The NT series (Windows NT and XP), however, do not allow this approach. On this architecture, all communication with hardware ports must be through a device driver. This is a security mechanism, but developing a driver for small projects can cost too much in terms of time and money. 

Fortunately there is a library that solves this problem. If Windows NT is detected, it decompresses the HWInterface.sys kernel device driver and installs it. If Windows 9x is detected, it simply uses assembler opcodes to access the hardware. 

The library has only two functions, Inp32 and Out32, and their use is quite intuitive. 

The library will be loaded dynamically, so define both functions first: 
    
    
     type
       TInp32 = function(Address: SmallInt): SmallInt; stdcall;
       TOut32 = procedure(Address: SmallInt; Data: SmallInt); stdcall;
    

  * 'Address' represents the address of the port to be accessed
  * 'Out32' sends data to the port specified by 'Address'
  * 'Inp32' returns a byte from the port specified by 'Address'



Now the library can be loaded. This may be implemented in the 'OnCreate' method of the program's main form: 
    
    
     uses
     ....dynlibs...
    
     type
       TMyForm = class(TForm)
       .........
       private
         { private declarations }
         Inpout32: THandle;
         Inp32: TInp32;
         Out32: TOut32;
       .........
     implementation
       .........
     procedure TMyForm.FormCreate(Sender: TObject);
     begin
     {$IFDEF WIN32}
       Inpout32 := LoadLibrary('inpout32.dll');
       if (Inpout32 <> 0) then
       begin
         // needs overtyping, plain Delphi's @Inp32 = GetProc... leads to compile errors
         Inp32 := TInp32(GetProcAddress(Inpout32, 'Inp32'));
         if (@Inp32 = nil) then Caption := 'Error';
         Out32 := TOut32(GetProcAddress(Inpout32, 'Out32'));
         if (@Out32 = nil) then Caption := 'Error';
       end
       else Caption := 'Error';
     {$ENDIF}
     end;
    

If the library is loaded on 'OnCreate', it must be unloaded in 'OnDestroy': 
    
    
     procedure TMyForm.FormDestroy(Sender: TObject);
     begin
     {$IFDEF WIN32}
       FreeLibrary(Inpout32);
     {$ENDIF}
     end;
    

Here is a simple example of how to use the 'Inp32' function: 
    
    
     {$IFDEF WIN32}
       myLabel.Caption := IntToStr(Inp32($0220));
     {$ENDIF}
    

This code was tested with a custom ISA card on port $0220, using Lazarus 0.9.10 on Windows XP. Of course 'Windows' must be in the uses clause in order for this code to run. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** For deployment "inpout32.dll" must be in the same directory of the application. Also the library has to be registered in 'system' by the 'administrator' user on Windows NT/XP/2000 or elevated privileges on Windows Vista/7. This can be done by installation of a program such as InnoSetup: 
    
    
    Filename: {sys}\rundll32.exe; Parameters: "inpout32.dll,IsInpOutDriverOpen"; WorkingDir: {app}; Flags: 32bit;

### Using assembler on Windows 9x

On Windows 9x assembler code can be used. Suppose one wants to write $CC to the $320 port. The code to do that is: 
    
    
     {$ASMMODE ATT}
     ...
        asm
            movl $0x320, %edx
            movb $0xCC, %al
            outb %al, %dx
        end ['EAX','EDX'];
    

### Troubleshooting on Windows

One possible source of trouble using parallel hardware that does not support Plug And Play on Windows is that Windows may assign the port utilized by the hardware to another device. 

### Using 'ioperm' to access ports on Linux

The best way to access the hardware on Linux is through device drivers, but, due to the complexity of the task of creating a driver, sometimes a quick method is very useful. 

In order to use the "[ports](<http://lazarus-ccr.sourceforge.net/docs/rtl/ports> "doc:rtl/ports")" unit under Linux the program must be run as root, and IOPerm must be called to set appropriate permissions on the port access. Documentation about the "[ports](<http://lazarus-ccr.sourceforge.net/docs/rtl/ports> "doc:rtl/ports")" unit can be found here: <http://www.freepascal.org/docs-html/rtl/ports/index.html>

The first thing to do is link to (g)libc and call IOPerm. A unit that links to the entire (g)libc exists on free pascal, but this unit gives problems when used directly by an application and linking statically to the entire (g)libc library is not good practise as it changes often between versions in an incompatible manner. Functions like ioperm, however, are unlikely to change. 
    
    
     {$IFDEF Linux}
     function ioperm(from: Cardinal; num: Cardinal; turn_on: Integer): Integer; cdecl; external 'libc';
     {$ENDIF}
    

  * "from" represents the first port to be accessed.
  * "num" is the number of ports after the first to be accessed, so ioperm($220, 8, 1) will give access for the program for all ports between and including $220 and $227.



After linking to IOPerm, port[<Address>] will access the ports. 
    
    
     {$IFDEF Linux}
       i := ioperm($220, 8, 1);
       port[$220] := $00;
       myLabel.Caption := 'ioperm: ' + IntToStr(i);
       i := Integer(port[$220]);
       myOtherLabel.Caption := 'response: ' + IntToStr(i);
     {$ENDIF}
    

This code was tested with a custom ISA card on port $0220, using Lazarus 0.9.10 on Mandriva Linux 2005 and Damn Small Linux 1.5 

### General UNIX hardware access
    
    
    {$IFDEF Unix}
    Uses Clib;   // retrieve libc library name.
    {$ENDIF}
    
    {$IFDEF Unix}
    function ioperm(from: Cardinal; num: Cardinal; turn_on: Integer): Integer; cdecl; external clib;
    {$ENDIF}
    

  
**Note** : FPC provides an abstraction for ioperm called "fpioperm" in [unit x86](<http://lazarus-ccr.sourceforge.net/docs/rtl/x86/index.html> "doc:rtl/x86/index.html"), and also defines fpIOPL and out-/inport functions. These functions are currently implemented for Linux/x86, Linux/x86_64 and FreeBSD/x86, FreeBSD/x86_64. 

It is not recommended to link to libc unless absolutely necessary due to possible deployment and portability functions. Also manual linking to libc (by declaring ad hoc libc imports for functions that are available elsewhere) like done above is not recommended (e.g. the above libc import line will unnecessarily fail if the standard C lib is not called libc, like e.g. libroot on BeOS, or on platforms with a non standard C symbol mangling). 

**Note 2** : Using **unit** libc is not recommended under any circumstances other than Kylix compatibility. See [libc unit](<libc_unit.md> "libc unit")

### Status and control

Besides data lines, the parallel port also has status and control lines which are accessed using the status and control registers. While the base address accesses the data lines and reads or writes data bytes from/to them, the status register is accessed on the address offset by +1 and the control register is accessed on the offset +2. For example, LPT1 (first parallel port on a PC) has the base address $378, so its status register is at $379 and control register at $380. To get individual status line states, read a byte from its address and its bits represent those lines. Setting control lines is similarly done by writing a byte with accordingly set bits to the control register. 

Newer bidirectional parallel port versions have more registers on higher offsets. More details about them, together with information which bits map to which lines can be found [here](<http://www.ni.com/white-paper/3466/en>). 

Most directly accessed hardware devices other than PC parallel ports are controlled in a similar way. Depending on the device in question, it is necessary to find out what registers are available (above mentioned control and status, but also address and other registers) and which bits represent which hardware functions. 

## Serial Communication

### Device Setup

Before starting, make sure to authorize your device on Linux. An option is to create a persistent [UDEV rule](<https://wiki.archlinux.org/index.php/udev>) for each device. For example, the following approach apply mode "0064" for the device specified by 'serial', 'idVendor' and 'idProduct' attributes: 
    
    
    # create a new dev rule based on some constants
    echo 'SUBSYSTEM=="tty", ATTRS{serial}=="A1004chl", ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6001", MODE="0664"' | sudo tee /etc/udev/rules.d/50-usb-serial.rules > /dev/null 
    
    # reload rules
    sudo udevadm control --reload-rules
    
    # re-add all devices
    sudo udevadm trigger
    
    # test
    ls -al /dev/ttyUSB0
    
    # you may want to reboot your system
    sudo reboot
    

  


### Synaser

It is very easy to build a serial communication software using the [Synaser library](<http://synapse.ararat.cz/doku.php>). The example when used together with the [Synaser documentation](<http://synapse.ararat.cz/doc/help/synaser.html>) should be trivial to understand. The most important part is TBlockSerial.Config to configure the speed (in bits per second), data bits, parity bits, stop bits and handshake protocol, if any. The following code was tested with a serial mouse connected to COM1. 
    
    
    program comm;
    
    {$apptype console}
    
    uses
      Classes, SysUtils, Synaser;
    
    var
      ser: TBlockSerial;
    begin
      ser:=TBlockSerial.Create;
      try
        ser.Connect('COM1');
        ser.config(1200, 7, 'N', SB1, False, False);
        while True do
          Write(IntToHex(ser.RecvByte(10000), 2), ' ');
      finally
        ser.free;
      end;
    end.
    

The following code-example is an alternative version of the example above. The example above has a critical bug in its main concept, to be exact, it is the part with "while true do...". On the test system (Asus A6T laptop with Digitus USB to RS232 Adapter, Ubuntu 8.04.1), this part caused the following error: the application ran only one time successfully per session, when the application was started again, the application was unable to connect to the serial port. Thus, a reboot was necessary everytime the user tried to relaunch the application. 

The reason is not difficult to understand: The application is in the while true do - loop, which is, to be more precise, an endless loop. There is no abort-condition, so the only way to close the application is to close the terminal or to press CTRL-C. But if the application is aborted this way, the important part with "ser.free", which frees the serial port, will never be called. This problem is described in the following thread in the German Lazarus-Forum [http://www.lazarusforum.de/viewtopic.php?f=10&t=2082](<http://www.lazarusforum.de/viewtopic.php?f=10&t=2082>)

There is a bit of code around the main application to make it clear to the user not to press CTRL-C. /dev/ttyUSB0 is used for the com-port due to the USB to Serial Adapter (from Digitus) on the test-system. With a built-in serial port, use the 'Com0' - declaration like in the code - example above. 
    
    
    program serialtest;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,SysUtils,Synaser,Crt
      { add other units after this };
    
      var l:boolean;
    
      function check_affirmation():boolean;
      var k:string;
      begin
           Writeln('To quit the application do NOT use CTRL-C! Instead, press any key to quit the application! '+
           'Confirm this notification to continue the application! '+
           '[0]=Quit, [1]=Confirm, continue! ');
           Writeln('Your choice: ');
           Read(k);
           if StrtoInt(k) = 1 then
           begin
                check_affirmation:=true;
                Writeln('OK, application continues ...');
           end
           else
           begin
                check_affirmation:=false;
                Writeln('Abort');
           end
      end;
    
      procedure RS232_connect;
      var
        ser: TBlockSerial;
      begin
        ser:=TBlockSerial.Create;
        try
          ser.Connect('/dev/ttyUSB0'); //ComPort
          Sleep(1000);
          ser.config(1200, 7, 'N', SB1, False, False);
          Write('Device: ' + ser.Device + '   Status: ' + ser.LastErrorDesc +' '+
          Inttostr(ser.LastError));
          Sleep(1000);
          repeat
            Write(IntToHex(ser.RecvByte(10000), 2), ' ');
          until keypressed; //Important!!!
        finally
          Writeln('Serial Port will be freed...');
          ser.free;
          Writeln('Serial Port was freed successfully!');
        end;
      end;
    
      begin
        l:=check_affirmation();
        if l=true then
        RS232_connect()
        else
        Writeln('Program quit! ');
      end.
    

Also, the [ External Links](<Hardware_Access.md> "Hardware Access") section has UNIX and Windows serial port tutorials. 

Note the function of the TBlockSerial.LinuxLock parameter under linux. When set to default of True, a connect will try to create a lock file (eg. "LCK..ttyUSB0") under /var/lock and fail if a lock already exists for the requested port. The lock file will be left over if Free was not called. Setting LinuxLock to False will make Synaser ignore port locking. 

There are alternatives to Synaser; see below. 

### Visual Synapse

[Visual Synapse](<https://sourceforge.net/projects/visualsynapse/>) has lots of component wrappers for many parts of Synapse serial and networking library. TvsComPort is a visual component wrapper around Synaser. 

### 5dpo

There is also a visual component [5dpo](<http://wiki.lazarus.freepascal.org/5dpo>) based on Synaser. 

### TLazSerial

Based on 5dpo (and therefore Synapse): <http://forum.lazarus.freepascal.org/index.php/topic,20481.0.html>

### FPC built in Serial unit

Another [Serial unit](<Serial_unit.md> "Serial unit") is part of FreePascal since version 2.2.2: just put 'Serial' in the Uses list. However there does not seem to be any documentation other than the Serial.pp source file and some discussions. 

An example is [here](<Serial_unit.md> "Serial unit"). 

### TDataPort

[DataPort](<https://github.com/serbod/dataport>) is a thread-safe abstract port for data exchange. It is used for communication over networks (TCP/UDP/HTTP), serial ports (UART, COM-port, FTDI), device files (ioctl supported) and conventional file named pipes. For serial communication it has TDataPortSerial and TDataPortFtdi descendants. Small example can be found in [demo](<https://github.com/serbod/dataport/tree/master/demo>) and [here](<http://forum.lazarus.freepascal.org/index.php/topic,36523.msg245030.html#msg245030>). 

### PXL

[PXL (Platform eXtended Library)](<Lazarus_on_Raspberry_Pi.md>) for low level native access to serial ports, GPIO, I²C, SPI, PWM, V4L2, displays and sensors. 

### Serial port names on Windows

COM ports are named with a number on Windows 9x-based OSes (95, 98, ME), e.g. COM1, COM30. 

On Windows NT-based systems (NT, 2000, XP, Vista, Windows 7, Windows 8), COM ports are numbered too, but only for compatibility with DOS/Win9x. 

Use this code to get the real name: 
    
    
    // ComNr is obviously the number of the COM port
    if ComNr > 9 then
      Result := Format('\\\\.\\COM%d', [ComNr])
    else
      Result := Format('COM%d', [ComNr]);
    

## USB

### libusb

A cross platform possibility for Windows, Linux, BSDs and macOS is [libusb](<http://libusb.sourceforge.net/>). 

name | author | version | date | link | remarks   
---|---|---|---|---|---  
libusb.pp | Uwe Zimmermann | 0.1.12 | 2006-06-29 |  |   
libusb.pas | Johann Glaser |  | 2012-09-23 | <https://github.com/hansiglaser/pas-libusb> | includes OOP wrapper, see branch "libusb-1.0" for libusb 1.0   
fpcusb | Joe Jared | 0.11-14 | 2006-02-02 |  |   
libusb.pp | Marko Medic | 1.0 | 2010-12-14 | <http://www.lazarus.freepascal.org/index.php/topic,11435.0.html> |   
  
### FTDI

If one of the chips from [FTDI](<http://www.ftdichip.com/>) is used, their pascal headers for their dll interface to the chips can be used. 

## Devices in general

On Windows, devices can be managed from code, see here: [Windows Programming Tips#Enabling and disabling devices](<Windows_Programming_Tips.md> "Windows Programming Tips")

## See also

  * [Lazarus on Raspberry Pi](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi")
  * [Networking](<Networking.md> "Networking")
  * [Target NativeNT](<Target_NativeNT.md> "Target NativeNT")



  


## External Links

  * Communication Protocols speed comparison:


  1. [Wikipedia, Serial port](<http://en.wikipedia.org/wiki/Serial_port#Speed>)
  2. [Wikipedia USB](<http://en.wikipedia.org/wiki/USB#Transfer_Speed>)
  3. [Wikipedia PCI bus](<http://en.wikipedia.org/wiki/PCI#Conventional_PCI_bus_specifications>)


  * Serial Communication Links:


  1. On UNIX: [Unix serial port](<http://www.easysw.com/~mike/serial/serial.html>)
  2. On Windows: [Microsoft msdn](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnfiles/html/msdn_serial.asp>)
  3. Synaser component: [synapse. CZ](<http://synapse.ararat.cz/>)
  4. Comport Delphi package: [Sourceforge Delphi comport](<http://sourceforge.net/projects/comport/>)

---

_Source: [https://wiki.freepascal.org/Hardware_Access](https://web.archive.org/web/20250503230839/https://wiki.freepascal.org/Hardware_Access)_
