# WinCE Programming Tips

[![WinCE Logo.png](https://wiki.freepascal.org/images/8/86/WinCE_Logo.png)](</File:WinCE_Logo.png>)

Эта статья относится только к [WinCE](</Category:WinCE> "Category:WinCE").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[English (en)](<../en/WinCE_Programming_Tips.md>)** │  **русский (ru)** │

На этой странице описано несколько приёмов работы с платформой WinCE. 

## Contents

  * 1 Other Interfaces
    * 1.1 Platform specific Tips
    * 1.2 Interface Development Articles
  * 2 Если приложение не запускается?..
    * 2.1 Настройки компилятора
    * 2.2 Не тот целевой процессор
    * 2.3 Отсутствие некоторых DLL в целевой ОС
  * 3 Советы по программированию / примеры
    * 3.1 Получить ID устройства
    * 3.2 Получить имя устройства
    * 3.3 Показать/спрятать SIP Panel
    * 3.4 Включить/разбудить устройство
    * 3.5 Подсветка/вибрация
    * 3.6 Получение состояния батареи
      * 3.6.1 GetSystemPowerStatusEx
      * 3.6.2 GetSystemPowerStatusEx2
    * 3.7 Создание полноэкранного приложения
    * 3.8 Препятствие входу в режим ожидания
    * 3.9 Установка приложения на WinCe через app build(cab файл)
    * 3.10 Debugging with a log file
    * 3.11 Current Directory and Placement of DLLs
  * 4 Database Tips
    * 4.1 Pre-compiled sqlite dll
  * 5 Links



## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<../en/Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<../en/Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<../en/Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<../en/Carbon_Interface.md> "Carbon Interface") \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
  * [Cocoa Interface](<../en/Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa 64 bit interface for macOS
  * [Qt Interface](<../en/Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [Qt5 Interface](<../en/Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [GTK1 Interface](<../en/GTK1_Interface.md> "GTK1 Interface") \- The gtk1 interface for Unixes, macOS (X11), Windows
  * [GTK2 Interface](<../en/GTK2_Interface.md> "GTK2 Interface") \- The gtk2 interface for Unixes, macOS (X11), Windows
  * [GTK3 Interface](<../en/GTK3_Interface.md> "GTK3 Interface") \- The gtk3 interface for Unixes, macOS (X11), Windows
  * [fpGUI Interface](<../en/fpGUI_Interface.md> "fpGUI Interface") \- Based on the fpGUI library, which is a cross-platform toolkit completely written in Object Pascal
  * [Custom Drawn Interface](<../en/Custom_Drawn_Interface.md> "Custom Drawn Interface") \- A cross-platform LCL backend written completely in Object Pascal inside Lazarus. The Lazarus interface to Android.



### Platform specific Tips

  * [Android Programming](<../en/Android_Programming.md> "Android Programming") \- For Android smartphones and tablets
  * [iPhone/iPod development](<../en/iPhone/iPod_development.md> "iPhone/iPod development") \- About using Objective Pascal to develop iOS applications
  * [FreeBSD Programming Tips](<../en/FreeBSD_Programming_Tips.md> "FreeBSD Programming Tips") \- FreeBSD programming tips
  * [Linux Programming Tips](<../en/Linux_Programming_Tips.md> "Linux Programming Tips") \- How to execute particular programming tasks in Linux
  * [macOS Programming Tips](<../en/macOS_Programming_Tips.md> "macOS Programming Tips") \- Lazarus tips, useful tools, Unix commands, and more...
  * [WinCE Programming Tips](<../en/WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Using the telephone API, sending SMSes, and more...
  * [Windows Programming Tips](<../en/Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips



### Interface Development Articles

  * [Carbon interface internals](<../en/Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<../en/Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<../en/Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<../en/LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<../en/LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<../en/Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset



## Если приложение не запускается?..

Если вы написали и скомпилировали ваше приложение под ОС Windows CE и оно прекрасно запускается на эмуляторе, а на реальном устройстве нет (при попытке запуска возникает системная ошибка), возможны следующие варианты: 

#### Настройки компилятора

Убедитесь, что в среде Lazarus установлены правильные настройки компиляции приложения. Они должны быть такими: 
    
    
    "Меню" -> "Проект" -> "Параметры компилятора"
    

здесь : 

  * на первой владке "Пути", в самом низу в выпадающем меню "Библиотека виджетов LCL" необходимо установить "wince"


  * на вкладке "Код" в блоке "Целевая платформа", необходимо установить название операционной системы "WinCE" и целевое семейство процессоров "arm" (если вы точно знаете, что на реальном устройстве стоит x86 процессор, соответственно надо выбрать "i386" из предложенного списка). В выпадающем меню "Целевой процессор" оставляем "По умолчанию"



#### Не тот целевой процессор

Если при попытке запуска программы на целевом устройстве возникает системная ошибка 
    
    
    _'Project1' is not a valid WindowsCE application_
    

возможно, на устройстве установлен не ARM, наиболее распространённый, а MIPS-процессор, который, к сожалению, не поддерживается компилятором Lazarus. Тут уж ничего не поделаешь. 

#### Отсутствие некоторых DLL в целевой ОС

Ошибки типа : 
    
    
    _Cannot find 'project1' (or one of its components)_
    
    
    
    _'project1' (или один из компонентов) не найден_
    

а так же, вышеупомянутая 
    
    
    _'Project1' is not a valid WindowsCE application_
    

очень часто могут возникать из-за отсутствия системной библиотеки aygshell.dll в составе ОС на целевом устройстве. 

В 90% случаев такая проблема возникает на устройствах, на которых установлена "голая" версия Windows CE "core" (навигаторы, читалки книг, считыватели штрих-кодов), на устройствах под управлением Windows Mobile эта проблема не возникает. 

Вот так выглядит ошибка на навигаторе Acer p610 : 

[![Winceerr.gif](https://wiki.freepascal.org/images/9/93/Winceerr.gif)](</File:Winceerr.gif>)

Проблема решается распространением aygshell.dll вместе с программой (таким образом поступают разработчики GPS-программы Navitel) или записью этой библиотеки в системную директорию //Windows. 

Вот здесь можно скачать архив с несколькими версиями aygshell.dll для разных версий ОС WinCE: 

  * <http://hpcmonex.net/izemize.htm>



Если "прикручивание" aygshell.dll к программе не дало результатов и ошибка все равно возникает, возможно версия целевой ОС ниже требуемой и она просто не имеет той функциональности, которую требует разработанная программа. Часто такая ситуация провоцируется отсутствием требуемых функций в более ранней версии системной библиотеки coredll.dll. 

## Советы по программированию / примеры

### Получить ID устройства

Идентификация устройства полезна для защиты вашей программы. Работает только на windows mobile 5.1 и 5.2 
    
    
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
      AppData := Utf8Decode('MY_SIG');
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
    

Ссылки: <http://msdn2.microsoft.com/en-us/library/ms893522.aspx> <http://peterfoot.net/RetrieveIMEIThroughTAPI.aspx> <http://blogs.msdn.com/jehance/archive/2004/07/12/181067.aspx>

### Получить имя устройства

Это легко сделать через реестр: 
    
    
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
    

### Показать/спрятать SIP Panel

Виртуальная клавиатура для сенсорных экранов(SIP: Software Input Panel button). 
    
    
    const
      //Некоторые константы
      SIPF_OFF    =	$00000000;
      SIPF_ON     =	$00000001;
      SIPF_DOCKED =	$00000002;
      SIPF_LOCKED =	$00000004;
    
    function SipShowIM(IPStatus:DWORD):Integer; stdcall; external 'coredll.dll' name 'SipShowIM';
    
    begin
      SipShowIM(SIPF_ON)
    end;
    

Документация microsoft об этом: [[1]](<http://msdn.microsoft.com/en-us/library/ms941818.aspx>)

### Включить/разбудить устройство

Понадобится любителям будильников. 
    
    
    function SetSystemPowerState(psState: PWideChar; StateFlags: DWORD; Options : DWORD):DWORD;
     stdcall; external 'coredll.dll' name 'SetSystemPowerState';
    
    
      SetSystemPowerState(nil, POWER_STATE_ON, POWER_FORCE);
      Application.BringToFront;
      ShowWindow(Handle, SW_SHOW);
    

### Подсветка/вибрация

Вы можете включать/выключать подсветку и вибратор. У меня работало нормально (а вот автор статьи говорит, что работает, но не так, как хотелось бы, и что, видимо будут доработки). 
    
    
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
        OffOnBlink: Integer;           // 0 = выкл, 1 = вкл, 2 = мигание
        TotalCycleTime: DWORD;         // Общее время мигания в микросекундах.
        OnTime: DWORD;                 // Время вкл. в цикле в микросекундах.
        OffTime: DWORD;                // Время выкл. в цикле в микросекундах.
        MetaCycleOn: Integer;          // Число вкл. циклов миганий
        MetaCycleOff: Integer;         // Число выкл. циклов миганий
       end;
    
      function NLedGetDeviceInfo(nID:Integer; var pOutput): WordBool;
       stdcall; external 'coredll.dll' name 'NLedGetDeviceInfo';
      function NLedSetDevice(nID: Integer; var pOutput): WordBool;
       stdcall; external 'coredll.dll' name 'NLedSetDevice';
    

Примеры: 
    
    
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
    

Вибратор - это последний индикатор устройства. 
    
    
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
    

  


### Получение состояния батареи

#### GetSystemPowerStatusEx

Больше об этом(опять-таки на английском) : [MSDN GetSystemPowerStatusEx](<http://msdn.microsoft.com/en-us/library/aa453172.aspx>)
    
    
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
    

#### GetSystemPowerStatusEx2

Больше об этом(Угадайте, на каком языке?) : [MSDN GetSystemPowerStatusEx2](<http://msdn.microsoft.com/en-us/library/aa931066.aspx>)
    
    
    //добавил romlo
     type
      TBAT_INFO2 = record
      ACLineStatus: byte; // состояние внешнего питания 
      BatteryFlag: byte; // состояние основной батареи
      BatteryLifePercent: byte; // процент зарядки основной батареи
      BatteryLifeTime: DWord; // время, на которое хватит заряда
      BatteryFullLifeTime: DWord; // время, на которое хватит полной батареи
      BackupBatteryFlag: byte; // состояние резервной батареи
      BackupBatteryLifePercent: byte; // процент заряда резервной батареи
      BackupBatteryLifeTime: DWord; // время остатка заряда резервной батареи
      BackupBatteryFullLifeTime: DWord; // полное время резервной батареи
     //Физические характеристики батареи:
      BatteryVoltage: Dword; // напряжение батреи (mV)
      BatteryCurrent: DWord; // сила тока батареи (mA)
      BatteryAverageCurrent: DWord; // средний ток
      BatteryAverageInterval: DWord; // интервал изменения тока
      BatterymAHourConsumed: DWord; // емкость батареи в mA/h
      BatteryTemperature: DWord; // температура батареи
      BackupBatteryVoltage: DWord; // емкость резервной батареи
      BatteryChemistry: Byte; // тип батареи (LION, NiCD, NIMN… )
    
      function GetSystemPowerStatusEx2(var pOutput; DwLen: DWord; fUpdate: boolean ): WordBool;
       stdcall; external 'coredll.dll' name 'GetSystemPowerStatusEx2'; 
    
    //Пример:
    
    function getbat2 :  byte;
    var
      batinfo: TBAT_INFO2;
    begin
    if GetSystemPowerStatusEx2(batinfo,SizeOf(batinfo), True) then getbat:=batinfo.BatteryLifePercent
    else getbat:=255; 
    end;
    

### Создание полноэкранного приложения

Добавьте в раздел uses модуль windows и добавьте следующий код в раздел **interface** вашей программы: 
    
    
    const
      //ShFullScreen
      SHFS_SHOWTASKBAR   = $01;
      SHFS_HIDETASKBAR   = $02;
      SHFS_SHOWSIPBUTTON = $04;
      SHFS_HIDESIPBUTTON = $08;
      SHFS_SHOWSTARTICON = $10;
      SHFS_HIDESTARTICON = $20;  
    
      function SHFullScreen(hwndRequester: hWnd; dwState: DWord): WINBOOL; stdcall; external 'aygshell.dll' name 'SHFullScreen';
    

Затем вставьте в события OnCreate и OnShow ваших форм (лучше OnShow) следующее: 
    
    
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
    

### Препятствие входу в режим ожидания

Добавьте на форму таймер, в событии OnTimer пропишите следующую строку: 
    
    
    keybd_event(VK_F24, 0, KEYEVENTF_KEYUP or KEYEVENTF_SILENT, 0);
    

### Установка приложения на WinCe через app build(cab файл)

//Original de Hinnack (from Lazarus forum) 

У меня ушло много времени на то, что-бы выяснить, как установить моё приложение на WinCE-устройство(Да, я знаю, что можно скопировать и вставить), поэтому я решил поделиться этим здесь. 

1) Во-первых, нужен ini-файл (может, когда-нибудь, Лазарус сможет генерировать их сам...). Хороший рассказ о том, как это можно сделать, есть здесь(на англ.): <http://web.archive.org/web/20080205125046/> <http://www.sundialsoft.freeserve.co.uk/cabinfo.htm> (Оригинальный сайт мёртв) Я использовал следующее (только для ARM процессоров): 
    
    
    [Version] ; Обязательная секция
    Signature = "$Windows NT$"
    Provider = "bilettiX"
    CESignature = "$Windows CE$"
    
    [CEDevice.ARM]
    ProcessorType = 2577 ; значение для ARM 
    
    [DefaultInstall.ARM]
    CopyFiles = Files.ARM
    
    [Files.ARM]
    bilettixscan.exe,,,0
    sqlite3.dll,,,0
    
    [DefaultInstall] ; Обязательная секция
    CEShortcuts = Shortcuts.All
    AddReg = RegData
    
    [SourceDisksNames] ; Обязательная секция
    1 = ,"common files",,C:\Dokumente und Einstellungen\xyz\Dev;A existing folder on your HD, where the common files to be copied are found (not processor specific)
    
    [SourceDisksNames.ARM] 
    2 = ,"ARM files",,arm;Папка со специальными ARM файлами
    [SourceDisksFiles] ; Обязательная секция, бинарник приложения, файлы для копирования
    bilettixscan.EXE = 2
    sqlite3.DLL = 2
    bilettix.db = 2
    
    [DestinationDirs] ; Обязательная секция
    Shortcuts.All = 0,%CE11%
    DefaultDestDir = 0,%InstallDir%
    
    [CEStrings] ; Обязательная секция
    AppName = Ticket Validator
    InstallDir = %CE1%\%AppName%
    
    [Shortcuts.All]
    %AppName%,0,bilettixscan.exe
    
    [RegData]
    HKLM,Software\bilettix\%AppName%,MajorVersion,0x00010001,1
    HKLM,Software\bilettix\%AppName%,MinorVersion,0x00010001,0
    HKLM,Software\bilettix\%AppName%,Installpath,0x00000000,%InstallDir%
    

2) теперь создаём cab файл из наших файлов, используя утилиту cabWiz из SDK от MS и ini-файл. 3) генерируем install.ini - мой выглядел так(на самом деле icon-section не нужна...): 
    
    
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
    

4) Затем берём EzSetup for free отсюда: <http://www.spbsoftwarehouse.com/products/ezsetup/index.html>

5) Генерируем "установщик", используя ini, readme и лицензионное соглашение в txt файлах. 

6) Готово! :-) 

Удостоверьтесь, что у вас стоит ActiveSync. Двойной клик по полученному файлу, и всё, наслаждайтесь :-) 

\--[Blueicaro](</index.php?title=User:Blueicaro&action=edit&redlink=1> "User:Blueicaro \(page does not exist\)") 21:49, 29 April 2009 (CEST) 

### Debugging with a log file

Windows CE doesn't ship with a command line, so people used to command-line log debugging may have trouble. In some versions of Windows CE it is possible to install a command-line, but another solution for this is using the logging routines from the LCLProc unit in Lazarus to write log information to a file in the same directory as the executable and then read it, as in the code bellow. 
    
    
    uses LCLProc;
    
    DbgAppendToFile(ExtractFilePath(ParamStr(0)) + '1.log', 'Some text');
    

The unit LCLProc also contains other cool routines for debugging, like GetStackTrace, which returns a string with the stack trace. 

### Current Directory and Placement of DLLs

Windows CE does not have the concept of current directory, so there is no point in placing DLLs in the same folder of the executable, as they will not be loaded from there. They need to be on the \Windows directory or another suitable location. 

## Database Tips

### Pre-compiled sqlite dll

You can find sqlite3.dll pre-compiled for Windows CE [here](<ftp://ftp.freepascal.org/pub/fpc/contrib/arm-wince-sqlite322.zip>). 

## Links

Here are some links that might be useful for creating Windows CE interfaces. 

  * [Windows CE Development Notes](<../en/Windows_CE_Development_Notes.md> "Windows CE Development Notes")
  * [WinCE port of KOL GUI library](<../en/KOL-CE.md> "KOL-CE")

---

_Source: [https://wiki.freepascal.org/WinCE_Programming_Tips/ru](https://web.archive.org/web/20250122172726/https://wiki.freepascal.org/WinCE_Programming_Tips/ru)_
