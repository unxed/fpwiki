# WinCE Programming Tips

[![WinCE Logo.png](https://wiki.freepascal.org/images/8/86/WinCE_Logo.png)](</File:WinCE_Logo.png>)

This article applies to [WinCE](</Category:WinCE> "Category:WinCE") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[русский (ru)](<../ru/WinCE_Programming_Tips.md> "WinCE Programming Tips/ru")** │    
****

## Contents

  * 1 TIPS / FAQ
    * 1.1 The error message: SomeProject is not a valid Windows CE application
    * 1.2 Application runs on Windows Device Emulator, but not on physical device
      * 1.2.1 Missing aygshell.dll
      * 1.2.2 Wrong architecture
      * 1.2.3 Missing functions due to Operating System version
    * 1.3 Get Device ID
    * 1.4 Get Device Name
    * 1.5 Show/Hide SIP Panel
    * 1.6 Wakeup Device/ Power On
    * 1.7 LED / Vibrator
    * 1.8 Getting Battery Status
    * 1.9 Prevent Phone from Entering in Standby Mode
    * 1.10 Installation of an app build with Lazarus on a WinCE device
    * 1.11 Debugging with a log file
    * 1.12 Current Directory and Placement of DLLs
    * 1.13 Going Full Screen
      * 1.13.1 Removing the top taskbar
      * 1.13.2 Removing the Virtual Keyboard
      * 1.13.3 Alternative way with SHFullScreen
    * 1.14 Rotating the Screen
      * 1.14.1 Getting the Current Display Settings
      * 1.14.2 Enumerating All Supported Display Settings
      * 1.14.3 Changing Display Settings
  * 2 Pre-compiled Windows CE Libraries/Utils
    * 2.1 Pre-compiled sqlite dll
    * 2.2 OpenSSL for Windows CE
  * 3 Other Interfaces
    * 3.1 Platform specific Tips
    * 3.2 Interface Development Articles
  * 4 See also



## TIPS / FAQ

### The error message: SomeProject is not a valid Windows CE application

This is a generic error message from Windows CE which can be caused by one of the following reasons: 

  * The executable was compiled for a different Operating System, for example desktop Windows
  * The executable was compiled for a different architecture then the device has
  * One of the DLLs which the device requires could not be found. Usually this is either a database dll or aygshell.dll. See further in this FAQ about those
  * The binary is too big and doesn't fit the available RAM



Unfortunately Windows CE does not give specific error messages in this case, so one should simply test all hypotheses. 

### Application runs on Windows Device Emulator, but not on physical device

When running a compiled application on the Windows Device emulator, it works fine, but running it on the physical device you get the error: 

_Cannot find 'project1' (or one of its components)._

Possible causes are: 

#### Missing aygshell.dll

This error is usually indicative of missing DLLs on the target device, especially if you have a very simple "Hello World" type of application. In many instances, it is related to the aygshell.dll file, which is not present on many industrial-type devices running a bare Windows CE version - normally devices running "Windows Mobile" versions of Windows CE will not have this problem. 

This problem has been reported on Motorolla/Symbol MC9000 and MC1000 barcode scanners running both Windows CE 4.2 and 5. 

To resolve, do a search for "aygshells.zip" in a search engine - there are some "Dummy" aygshell.dll files available that can be copied to the device to overcome this problem. 

Here is a list of forum topics where people had trouble with aygshell.dll and found a solution: 

  * <http://forum.lazarus.freepascal.org/index.php/topic,7161.0.html>
  * <http://forum.lazarus.freepascal.org/index.php/topic,5524.0.html>



And here a number of DLLs to substitute missing ones and make applications work: 

  * <http://hpcmonex.net/izemize.htm>



#### Wrong architecture

Maybe your device runs a x86 or mips processor instead of the more common ARM. 

#### Missing functions due to Operating System version

Possibly your operating system is very old and doesn't have some functions required by Lazarus. 

### Get Device ID

Get and ID of your device useful for protect your application. This work only on Windows Mobile 5.0 and Windows CE 5.1 
    
    
    function GetDeviceUniqueID(AppData:LPCWSTR; cbApplictionData:Integer; dwDeviceIDVersion:Integer;
     var deviceIDOuput; var pcbDeviceIDOutput:DWORD):Integer; external 'coredll.dll' name 'GetDeviceUniqueID';
    
    function GetDeviceID: string;
    var
      AppData: array[0..19] of WideChar;
      DeviceID : array[0..19] of Byte;
      Count: DWORD;
      s: string;
      Res, i:Integer;
    begin
      //not sure about Unicode
      AppData := Utf8Decode('MY_SIG');//any string you like
      Count := SizeOf(DeviceID);
      FillChar(DeviceID, Count, #0);
      Res := GetDeviceUniqueID(AppData, SizeOF(AppData), 1, DeviceID, Count);
      if Res = 0 then
      begin
        Result := '';
        for i := 0 to Count -1 do
        begin
          if (i > 0) and ((i mod 2) = 0) then
            Result := Result + '-'; //add space make the string wrap in label
          Result := Result + IntToHex(DeviceID[i], 2);
        end;
      end
      else
        Result := '';//error accord
    //  you can MD5 it with your string
    //  Result := MD5Print(MD5Buffer(DeviceID, Count));
    end;
    

Reference pages: <http://msdn2.microsoft.com/en-us/library/ms893522.aspx> <http://peterfoot.net/RetrieveIMEIThroughTAPI.aspx> <http://blogs.msdn.com/jehance/archive/2004/07/12/181067.aspx>

### Get Device Name

Easy to get it from registry 
    
    
    function GetDeviceName: string;
    var
      aReg:TRegistry;
    begin
      aReg := TRegistry.Create(KEY_READ);
      try
        aReg.RootKey := HKEY_LOCAL_MACHINE;
        aReg.OpenKey('Ident', False);
        if aReg.ValueExists('Name') then
          Result := aReg.ReadString('Name')
        else
          Result := 'GUEST';
      finally
        aReg.Free;
      end;
    end;
    

### Show/Hide SIP Panel

SIP: Software Input Panel button, it is a keyboard come with WinCE for touch screen devices. 
    
    
    const
      //some of consts already found in Windows
      SIPF_OFF    =	$00000000;
      SIPF_ON     =	$00000001;
      SIPF_DOCKED =	$00000002;
      SIPF_LOCKED =	$00000004;
    
    function SipShowIM(IPStatus:DWORD):Integer; stdcall; external 'coredll.dll' name 'SipShowIM';
    
    begin
      SipShowIM(SIPF_ON)
    end;
    

Microsoft documentation for the SipShowIM routine: [[1]](<http://msdn.microsoft.com/en-us/library/ms941818.aspx>)

### Wakeup Device/ Power On

If you like to make alarm application this function make your device power on, you need also make some sounds with it. 
    
    
    function SetSystemPowerState(psState: PWideChar; StateFlags: DWORD; Options : DWORD):DWORD;
     stdcall; external 'coredll.dll' name 'SetSystemPowerState';
    
    
      SetSystemPowerState(nil, POWER_STATE_ON, POWER_FORCE);
      Application.BringToFront;
      ShowWindow(Handle, SW_SHOW);
    

### LED / Vibrator

You can turn on/off then LED/vibrator in, your device, it worked for me but not as like as i want, may be it need some improvements. 
    
    
    const
      NLED_COUNT_INFO_ID	= 0;
      NLED_SUPPORTS_INFO_ID	= 1;
      NLED_SETTINGS_INFO_ID	= 2;
    
    type
      TNLED_COUNT_INFO = record
        cLeds: DWORD;
      end;
      
      TNLED_SETTINGS_INFO = record
        LedNum: DWORD;                 // LED number, 0 is first LED
        OffOnBlink: Integer;           // 0 = off, 1 = on, 2 = blink
        TotalCycleTime: DWORD;         // total cycle time of a blink in microseconds
        OnTime: DWORD;                 // on time of a cycle in microseconds
        OffTime: DWORD;                // off time of a cycle in microseconds
        MetaCycleOn: Integer;          // number of on blink cycles
        MetaCycleOff: Integer;         // number of off blink cycles
       end;
    
      function NLedGetDeviceInfo(nID:Integer; var pOutput): WordBool;
       stdcall; external 'coredll.dll' name 'NLedGetDeviceInfo';
      function NLedSetDevice(nID: Integer; var pOutput): WordBool;
       stdcall; external 'coredll.dll' name 'NLedSetDevice';
    

Examples 
    
    
    function TForm1.MakeLEDOn;
    var
      Countnfo: TNLED_COUNT_INFO;
      Info:TNLED_SETTINGS_INFO;
    begin
      NLedGetDeviceInfo(NLED_COUNT_INFO_ID, Countnfo);
      //with Countnfo.cLeds you can check if your device support LEDs;
      Info.LedNum := 0; //<--- First LED
      Info.OffOnBlink := 1;
      Info.OffTime := 0;
      Info.MetaCycleOff:= 50;
      Info.MetaCycleOn:= 50;
      Info.TotalCycleTime := 100;
      NLedSetDevice(NLED_SETTINGS_INFO_ID, Info);
    end;
    
    procedure TForm1.MakeLedOff;
    var
      Info:TNLED_SETTINGS_INFO;
    begin
      Info.LedNum := 0;
      Info.OffOnBlink := 0;
      NLedSetDevice(NLED_SETTINGS_INFO_ID, Info);
    end;
    

Vibrator it is the last LED in your device, if you can write some music you can now make your phone dance. 
    
    
    function TForm1.MakeVibratorOn;
    var
      Countnfo: TNLED_COUNT_INFO;
      Info:TNLED_SETTINGS_INFO;
    begin
      NLedGetDeviceInfo(NLED_COUNT_INFO_ID, Countnfo);
      Info.LedNum := Countnfo.cLeds -1;
      Info.OffOnBlink := 1;
      NLedSetDevice(NLED_SETTINGS_INFO_ID, Info);
    end;  
    
    function TForm1.MakeVibratorOff;
    var
      Countnfo: TNLED_COUNT_INFO;
      Info:TNLED_SETTINGS_INFO;
    begin
      NLedGetDeviceInfo(NLED_COUNT_INFO_ID, Countnfo);
      Info.LedNum := Countnfo.cLeds -1;
      Info.OffOnBlink := 0;
      NLedSetDevice(NLED_SETTINGS_INFO_ID, Info);
    end;
    

### Getting Battery Status

For more information : [MSDN GetSystemPowerStatusEx](<http://msdn.microsoft.com/en-us/library/aa453172.aspx>)
    
    
    //by Philip Heinisch
     type
      TBAT_INFO = record
      ACLineStatus:byte; //0=Offline, 1=Online, 2=Backup Power,3= Unknown status 
      BatteryFlag:byte; //0=High, 1=Low, 2=Critical, 3=Charging, 4=No Battery, 5=Unknown
      BatteryLifePercent:byte; //0..100 Battery Life in Percent
      Reserved1:byte; //always 0
      BatteryLifeTime: DWORD; //remaining time in seconds
      BatteryFullLifeTime: DWORD; //max usage time in seconds
      Reserved2:byte; //always 0
      BackupBatteryFlag:byte; //0=High, 1=Low, 2=Critical, 3=Charging, 4=No Battery, 5=Unknown
      BackupBatteryLifePercent:byte; //0..100 Backup Battery Life in Percent
      Reserved3:byte; //always 0
      BackupBatteryLifeTime: DWORD; //remaining time in seconds
      BackupBatteryFullLifeTime: DWORD; //max usage time in seconds
     end;        
     
      function GetSystemPowerStatusEx(var pOutput;fUpdate:boolean ): WordBool;
       stdcall; external 'coredll.dll' name 'GetSystemPowerStatusEx'; 
    
    //Usage Example:
    
    function getbat :  byte;
    var
      batinfo: TBAT_INFO;
    begin
    if GetSystemPowerStatusEx(batinfo,True) then getbat:=batinfo.BatteryLifePercent
    else getbat:=255; //255=Function Call Failed 
    end;
    

### Prevent Phone from Entering in Standby Mode

Inside a timer event add the following line of code: 
    
    
    keybd_event(VK_F24, 0, KEYEVENTF_KEYUP or KEYEVENTF_SILENT, 0);
    

  


### Installation of an app build with Lazarus on a WinCE device

The simplest way to install an app is to simply copy the executable and other files it needs, but a proper installer can be made with the instructions bellow: 1) one needs a inf file (maybe Lazarus could generate one on his own some time...). A good brief on how to create one can be found here: <http://web.archive.org/web/20080205125046/http://www.sundialsoft.freeserve.co.uk/cabinfo.htm> (original website is dead) I used the following (only for ARM prozessors): 
    
    
    [Version] ; Required section
    Signature = "$Windows NT$"
    Provider = "bilettiX"
    CESignature = "$Windows CE$"
    
    [CEDevice.ARM]
    ProcessorType = 2577 ; processor value for ARM 
    
    [DefaultInstall.ARM]
    CopyFiles = Files.ARM
    
    [Files.ARM]
    bilettixscan.exe,,,0
    sqlite3.dll,,,0
    
    [DefaultInstall] ; Required section
    CEShortcuts = Shortcuts.All
    AddReg = RegData
    
    [SourceDisksNames] ; Required section
    1 = ,"common files",,C:\Dokumente und Einstellungen\xyz\Dev;A existing folder on your HD, where the common files to be copied are found (not processor specific)
    
    [SourceDisksNames.ARM] 
    2 = ,"ARM files",,arm;folder below the common files folder above for ARM specific files
    
    [SourceDisksFiles] ; Required section, application binary, files to copy
    bilettixscan.EXE = 2
    sqlite3.DLL = 2
    bilettix.db = 2
    
    [DestinationDirs] ; Required section
    Shortcuts.All = 0,%CE11%
    DefaultDestDir = 0,%InstallDir%
    
    [CEStrings] ; Required section
    AppName = Ticket Validator
    InstallDir = %CE1%\%AppName%
    
    [Shortcuts.All]
    %AppName%,0,bilettixscan.exe
    
    [RegData]
    HKLM,Software\bilettix\%AppName%,MajorVersion,0x00010001,1
    HKLM,Software\bilettix\%AppName%,MinorVersion,0x00010001,0
    HKLM,Software\bilettix\%AppName%,Installpath,0x00000000,%InstallDir%
    

2) now create a cab file out of your files using this inf file by using cabwiz.exe from the platform SDK of MS 3) generate a install.ini - my looks like this (icon-section is nor really needed...): 
    
    
    [CEAppManager]
    Version = 1.0
    Component = Ticket Validator
    
    [Ticket Validator]
    Description = bilettiX Ticket Validator
    Uninstall = bilettiXscan
    DeviceFile = bilettiXscan.exe
    IconFile = bilettix.ico
    IconIndex = 0
    CabFiles = bilettiXscan.ARM.CAB
    

4) download EzSetup for free from here: <http://www.spbsoftwarehouse.com/products/ezsetup/index.html>

5) generate the installer using the ini-file, a readme and a eula file you generated as txt files 

6) done :-) 

be sure to have ActiveSync installed. double-click the installer and have fun :-) 

### Debugging with a log file

Windows CE doesn't ship with a command line, so people used to command-line log debugging may have trouble. In some versions of Windows CE it is possible to install a command-line, but another solution for this is using the logging routines from the LCLProc unit in Lazarus to write log information to a file in the same directory as the executable and then read it, as in the code bellow. 
    
    
    uses LCLProc;
    
    DbgAppendToFile(ExtractFilePath(ParamStr(0)) + '1.log', 'Some text');
    

The unit LCLProc also contains other cool routines for debugging, like GetStackTrace, which returns a string with the stack trace. 

### Current Directory and Placement of DLLs

Windows CE does not have the concept of current directory, so paths to files should always be complete and never relative to the executable path. 

As one exception to this rule, DLLs can be placed in the same folder as the executable and they will be loaded from there. Another suitable location for DLLs is the \Windows directory. 

### Going Full Screen

Making an application Fullscreen requires removing both the bottom menu bar and the top taskbar 

#### Removing the top taskbar

This code should remove the taskbar by passing False to the parameter of this function: 

Note that if you don't provide a quit button it will be then impossible to close the application and that if the application crashes, the taskbar will not be restored automatically. Rebooting the device will restore it. 
    
    
    uses Windows, aygshell;
    
    procedure TForm1.RemoveTaskbar(AFullScreen: Boolean);
    const
      MENU_HEIGHT = 26;
    var
      rc: TRect;
    begin
      if AFullScreen then
      begin
        GetWindowRect(Handle, @rc);
    
        SHFullScreen(Handle, SHFS_HIDETASKBAR);
    
        MoveWindow(Handle,
      	  rc.left,
      	  rc.top-MENU_HEIGHT,
      	  rc.right,
      	  rc.bottom+MENU_HEIGHT,
      	  TRUE);
      end
      else
      begin
        GetWindowRect(Handle, @rc);
        SHFullScreen(Handle, SHFS_SHOWTASKBAR);
        MoveWindow(Handle,
             rc.left,
             rc.top+MENU_HEIGHT,
             rc.right,
             rc.bottom-MENU_HEIGHT,
             TRUE);
      end;
    end;
    

#### Removing the Virtual Keyboard

One can remove the keyboard with SHFS_HIDESIPBUTTON, like this: 
    
    
    uses Windows, aygshell;
    
    procedure TForm1.RemoveVirtualKeyboard(ARemove: Boolean);
    begin
      if ARemove then SHFullScreen(Handle, SHFS_HIDESIPBUTTON)
      else SHFullScreen(Handle, SHFS_SHOWSIPBUTTON);
    end;
    

#### Alternative way with SHFullScreen

This is a different solution for going Fullscreen. Make sure that you have included the windows unit to your program (uses windows;) In the INTERFACE section of your program paste the following code: 
    
    
    const
      //ShFullScreen
          SHFS_SHOWTASKBAR   = $01;
          SHFS_HIDETASKBAR   = $02;
          SHFS_SHOWSIPBUTTON = $04;
          SHFS_HIDESIPBUTTON = $08;
          SHFS_SHOWSTARTICON = $10;
          SHFS_HIDESTARTICON = $20;  
    
      function SHFullScreen(hwndRequester: hWnd; dwState: DWord): WINBOOL;
       stdcall; external 'aygshell.dll' name 'SHFullScreen';
    

Then in your Forms, OnCreate or OnShow (better on the OnShow) event add the following code: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      Rect:TRect;
      hTaskBar:THandle;
      menuh:Integer;
    begin
      hTaskBar := FindWindow('HHTaskBar','');
      GetWindowRect(hTaskBar,rect);
      menuh:=Rect.Bottom-Rect.Top;
      GetWindowRect(Form1.Handle,Rect);
      SHFullScreen(Form1.Handle,SHFS_HIDETASKBAR or SHFS_HIDESTARTICON or SHFS_HIDESIPBUTTON);
      movewindow(Form1.Handle,Rect.Left,Rect.Top-menuh,Rect.Right,Rect.Bottom+menuh,True);
    end;
    

### Rotating the Screen

There is a Microsoft tutorial with code in C here about this: <http://msdn.microsoft.com/en-us/library/ms812499.aspx> Bellow is code equivalent to that tutorial, but in Pascal. 

#### Getting the Current Display Settings

To obtain the current display settings, pass the ENUM_CURRENT_SETTINGS constant in the iModeNum parameter to the EnumDisplaySettings API, as illustrated by the following Pascal code. 
    
    
    var
      dm: DEVMODE;
    begin
      FillChar(dm, #0, SizeOf(dm));
      dm.dmSize := sizeof(dm);
    
      if 0 <> EnumDisplaySettings(0, ENUM_CURRENT_SETTINGS, @dm) then
      begin
          // inspect the DEVMODE structure to obtain details
          // about the display settings such as
          //  - Orientation
          //  - Width and Height
          //  - Frequency
          //  - etc.
       end;
    

#### Enumerating All Supported Display Settings

To enumerate all display settings supported by the current display device pass zero in the iModeNum parameter to the EnumDisplaySettings API and then continue calling it with incremented iModeNum values until the function returns zero, as shown in the following Pascal code. 
    
    
    var
      index: Integer = 0;
      dm: DEVMODE;
    begin
      FillChar(dm, #0, SizeOf(dm));
      dm.dmSize := sizeof(dm);
    
      while 0 <> EnumDisplaySettings(NULL, index, @dm) do
      begin
          // inspect the DEVMODE structure to obtain details
          // about the display settings such as
          //  - Orientation
          //  - Width and Height
          //  - Frequency
          //  - etc.
        Inc(Index)
      end;
    

#### Changing Display Settings

To change the display settings pass in a pointer to a valid DEVMODE structure to the ChangeDisplaySettings API. The following Pascal code demonstrates how to rotate the screen orientation clockwise by 90 degrees. Note that this code will only work with devices that support the respective display settings. It is important to obey the return value of the ChangeDisplaySettings API as some operations may require the computer to be restarted in order for the graphics mode to work. 
    
    
    var
      dm: DEVMODE;
      dwTemp: Cardinal;
      lRet: Integer;
    begin
      FillChar(dm, #0, SizeOf(dm));
      dm.dmSize := sizeof(dm);
    
       if (0 <> EnumDisplaySettings(NULL, ENUM_CURRENT_SETTINGS, @dm)) do
       begin
          // swap height and width
          dwTemp := dm.dmPelsHeight;
          dm.dmPelsHeight:= dm.dmPelsWidth;
          dm.dmPelsWidth := dwTemp;
    
          // determine new orientaion
          case (dm.dmDisplayOrientation) of
          DMDO_DEFAULT:    dm.dmDisplayOrientation := DMDO_270;
          DMDO_270:   dm.dmDisplayOrientation := DMDO_180;
          DMDO_180:   dm.dmDisplayOrientation := DMDO_90;
          DMDO_90:    dm.dmDisplayOrientation := DMDO_DEFAULT;
          else
             // unknown orientation value
             // add exception handling here
          end;
          lRet := ChangeDisplaySettings(@dm, 0);
          if (DISP_CHANGE_SUCCESSFUL <> lRet) then
          begin
             // add exception handling here
          end;
       end;
    

## Pre-compiled Windows CE Libraries/Utils

### Pre-compiled sqlite dll

You can find sqlite3.dll pre-compiled for Windows CE [here](<ftp://ftp.freepascal.org/pub/fpc/contrib/arm-wince-sqlite322.zip>). 

And newer ones can be found here: <http://www.parmaja.com/downloads/sqlite3/>

### OpenSSL for Windows CE

It is pretty hard to find a good binary OpenSSL library for Windows CE these days, so good versions which dont depend on any wierd, unusual library, are hosted here: 

<https://sourceforge.net/projects/p-tools/files/OpenSSL%20for%20Windows%20CE/>

They were obtained from here: <http://q3.snak.org/en/old.html> and hosted on the link above in case this page goes offline 

## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<Carbon_Interface.md> "Carbon Interface") \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
  * [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa 64 bit interface for macOS
  * [Qt Interface](<Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [Qt5 Interface](<Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [GTK1 Interface](<GTK1_Interface.md> "GTK1 Interface") \- The gtk1 interface for Unixes, macOS (X11), Windows
  * [GTK2 Interface](<GTK2_Interface.md> "GTK2 Interface") \- The gtk2 interface for Unixes, macOS (X11), Windows
  * [GTK3 Interface](<GTK3_Interface.md> "GTK3 Interface") \- The gtk3 interface for Unixes, macOS (X11), Windows
  * [fpGUI Interface](<fpGUI_Interface.md> "fpGUI Interface") \- Based on the fpGUI library, which is a cross-platform toolkit completely written in Object Pascal
  * [Custom Drawn Interface](<Custom_Drawn_Interface.md> "Custom Drawn Interface") \- A cross-platform LCL backend written completely in Object Pascal inside Lazarus. The Lazarus interface to Android.



### Platform specific Tips

  * [Android Programming](<Android_Programming.md> "Android Programming") \- For Android smartphones and tablets
  * [iPhone/iPod development](<iPhone/iPod_development.md> "iPhone/iPod development") \- About using Objective Pascal to develop iOS applications
  * [FreeBSD Programming Tips](<FreeBSD_Programming_Tips.md> "FreeBSD Programming Tips") \- FreeBSD programming tips
  * [Linux Programming Tips](<Linux_Programming_Tips.md> "Linux Programming Tips") \- How to execute particular programming tasks in Linux
  * [macOS Programming Tips](<macOS_Programming_Tips.md> "macOS Programming Tips") \- Lazarus tips, useful tools, Unix commands, and more...
  * WinCE Programming Tips \- Using the telephone API, sending SMSes, and more...
  * [Windows Programming Tips](<Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips



### Interface Development Articles

  * [Carbon interface internals](<Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset



## See also

Here are some links that might be useful for creating Windows CE interfaces. 

  * [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes")
  * [WinCE port of KOL GUI library](<KOL-CE.md> "KOL-CE")

---

_Source: [https://wiki.freepascal.org/WinCE_Programming_Tips](https://web.archive.org/web/20250321015149/https://wiki.freepascal.org/WinCE_Programming_Tips)_
