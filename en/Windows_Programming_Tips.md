# Windows Programming Tips

│ **English (en)** │  **[français (fr)](</Windows_Programming_Tips/fr> "Windows Programming Tips/fr")** │    
****

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

This page is dedicated to desktop Windows - including server - **Windows programming tips**. 

## Contents

  * 1 Articles about Windows Programming
  * 2 Windows specific compiler options
  * 3 Writing cross-platform code that works on Windows
  * 4 COM Programming
    * 4.1 Importing and using a COM library
    * 4.2 Creating a library which exports a COM object
    * 4.3 Windows Sensor/Location API
  * 5 ActiveX controls
  * 6 Services
  * 7 Using sleep(n) and Application.ProcessMessages
  * 8 Code signing Windows executables
  * 9 Code snippets
    * 9.1 File Association
    * 9.2 Ensure only a single instance can run
    * 9.3 Showing Memory, Disk Space Information
    * 9.4 Using Windows native wininet for web retrieval
    * 9.5 Listing all available drives
    * 9.6 Creating a shortcut (.lnk) file
    * 9.7 Opening / Closing a CD or DVD
    * 9.8 Recently used programs list
    * 9.9 Getting special folders (My documents, Desktop, local application data, etc)
    * 9.10 Enabling and disabling devices
    * 9.11 Downloading a file using urlmon
    * 9.12 Showing/finding processes
    * 9.13 Identify Windows Version
    * 9.14 Detecting Light or Dark theme
    * 9.15 Task bar icon not showing on secondary monitor
    * 9.16 Retrieving hard disk serial number etc
    * 9.17 Retrieving system information
  * 10 Troubleshooting
    * 10.1 A referral was returned from the server
    * 10.2 Lazarus 32 bit: External SIGSEGV exception when running with debugging
    * 10.3 FPC 2.6.x/Lazarus warning (Missing support for SEH)
  * 11 Other Interfaces
    * 11.1 Platform specific Tips
    * 11.2 Interface Development Articles



## Articles about Windows Programming

  * [High DPI](<High_DPI.md> "High DPI") \- How to make your application DPI-aware on Windows 7.
  * [Aero Glass](<Aero_Glass.md> "Aero Glass") \- How to apply Aero Glass effect in a Lazarus Form on Windows 7.
  * [Windows Icon](<Windows_Icon.md> "Windows Icon") \- How to design your icon with the right sizes.
  * [Inno Setup Usage](<Inno_Setup_Usage.md> "Inno Setup Usage") \- How to create setup files with File Association support.



## Windows specific compiler options

The most prominent options are the **-W** flags. A GUI application requires the **-WG** flag. See _Project Options / Compiler Options / Linking / Target OS Specific options / Win32 GUI application_. No console is shown, **writeln** and **readln** are not possible, you will get **File not open** errors. Omitting this option creates a console application (same as passing -WC). 

## Writing cross-platform code that works on Windows

While you can use Windows-only code (such as the **windows** unit), with a little care you can often prepare for cross-platform use (e.g. use the [lclintf](<lclintf.md> "lclintf") unit). 

See [Windows specific issues](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide") for more details. 

## COM Programming

### Importing and using a COM library

The first step to import and use a COM library is generating the interface definitions from it. Use the program importtl which is located in Free Pascal in fpc/utils/importtl. A pre-compiled binary of this program can be found here: <http://sourceforge.net/projects/p-tools/files/ImportTL/>

You can call it, for example for MSAA like this: 

importtl.exe C:\Windows\system32\oleacc.dll 

And it will generate the type library pascal unit Accessibility_1_1_TLB.pas in the folder where it is. 

### Creating a library which exports a COM object

ToDo: write me 

### Windows Sensor/Location API

Available since Windows 7. See [possible Windows implementation](<LazDeviceAPIs.md> "LazDeviceAPIs")

## ActiveX controls

You can use ActiveX controls in recent Lazarus versions. See [LazActiveX](<LazActiveX.md> "LazActiveX")

## Services

Lazarus and FPC make writing Windows services easy. See [Daemons and Services](<Daemons_and_Services.md> "Daemons and Services")

## Using sleep(n) and Application.ProcessMessages

Windows has a messaging system (see <https://docs.microsoft.com/en-us/windows/desktop/winmsg/messages-and-message-queues>) and uses messages to "talk" to all running applications. You obstruct that when using sleep(n). In such a case use Application.ProcessMessages instead of sleep(n) which is blocking. You have to give the OS the time to process its message queue. Application.ProcessMessages handles all waiting system messages in an application's message queue. 

There is one exception: sleep(0). This has a special meaning and means relinquish time slice. Any other sleep is almost always bad programming. 

Note that Application.ProcessMessages is called by the LCL automatically after every message (eg after every event like OnClick). So you would only need to call it in your event handler where there is a significant delay doing some processing. Of course, event handlers should probably not be doing any significant processing. Instead, you should be calling another function or procedure which may require the use of Application.ProcessMessages to ensure the application continues to respond to events and does not appear to have locked up. However, even in that case, it might be better to simply alert the user to the extended processing time (eg when compressing a file) by using the appropriate "busy" cursor or a progress bar. 

Beware of unintended consequences... sprinkling Application.ProcessMessages through your code like fairy dust may sound like a good idea, but consider what happens if your application is still busy with the task and an impatient user clicks that button a second time. That second click will be put into the Windows message queue and a second identical task will now be triggered. The result may be, for example, interleaved output to a file which is probably not what you intended. 

## Code signing Windows executables

For details of code signing Windows executables to avoid your end users receiving warnings that the publisher of the software is "unknown" when attempting to run an executable, see [Code Signing for Windows](<Code_Signing_for_Windows.md> "Code Signing for Windows"). 

## Code snippets

### File Association

To add icons to file associations, and register them for use with a program, use the [FileAssociation](<FileAssociation.md> "FileAssociation") component. If the FileAssociation component for some reason does not work for you, or you need to run it as administrator for all users without elevating your application privileges, an Inno Setup Script may fit your needs best. See [Inno Setup](<Inno_Setup_Usage.md> "Inno Setup Usage"). 

### Ensure only a single instance can run

To ensure only a single instance of your program can run, you can use the [UniqueInstance](<UniqueInstance.md> "UniqueInstance") component which supports Windows and Linux (note that macOS prevents the same application from running more than once by default). 

Alternatively, you can use the mutex unit below which also supports Windows and Linux: 
    
    
    {Author: Serguei Tarassov (from  https://arbinada.com/en/node/1426)}
    
    unit mutex;
    
    {$mode objfpc}{$H+}
    
    interface
    
    type
      TMutex = class
      private
        FFileHandle: integer;
      public
        constructor Create(const AName: string; const WaitForMSec: integer = 10000);
        destructor Destroy; override;
      end;
    
    implementation
    
    uses
      Classes, SysUtils, DateUtils,
      {$IFDEF WINDOWS}
      Windows
      {$ENDIF};
    
    function GetTempDir: string;
    begin
    {$IFDEF WINDOWS}
      SetLength(Result, 255);
      SetLength(Result, GetTempPath(255, (PChar(Result))));
    {$ENDIF}
    {$IFDEF LINUX}
      Result := GetEnv('TMPDIR');
      if Result = '' then
        Result := '/tmp/'
      else if Result[Length(Result)] <> PathDelim then
        Result := Result + PathDelim;
    {$ENDIF}
    end;
    
    constructor TMutex.Create(const AName: string; const WaitForMSec: integer);
      function NextAttempt(const MaxTime: TDateTime): boolean;
      begin
        Sleep(1);
        Result := Now < MaxTime;
      end;
    
    var
      MaxTime: TDateTime;
      LockFileName: string;
    begin
      inherited Create;
      LockFileName := IncludeTrailingPathDelimiter(GetTempDir) + AName + '.tmp';
      MaxTime := IncMillisecond(Now, WaitForMSec);
      repeat
        if FileExists(LockFileName) then
          FFileHandle := FileOpen(LockFileName, fmShareExclusive)
        else
          FFileHandle := FileCreate(LockFileName, fmShareExclusive);
      until (FFileHandle <> -1) or not NextAttempt(MaxTime);
      if FFileHandle = -1 then
        raise Exception.CreateFmt('Unable to lock mutex (File: %s; waiting: %d msec)', [LockFileName, WaitForMSec]);
    end;
    
    destructor TMutex.Destroy;
    begin
      if FFileHandle <> -1 then
        FileClose(FFileHandle);
      inherited;
    end;
    
    end.
    

To use it, add in your project file: 
    
    
    {$IFDEF WINDOWS}
    var
      MyMutex: TMutex;
    {$ENDIF}
    begin
      Application.Title := 'My App';
    
      {$IFDEF WINDOWS}
      Try
        MyMutex := TMutex.Create('MyAppMutex', 100);
      Except
        ShowMessage(Application.Title + ' is already running.');
        MyMutex.Free;
        Exit;
      End;
      {$ENDIF}
    
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    
      {$IFDEF WINDOWS}
      MyMutex.Free;
      {$ENDIF}
    end.
    

### Showing Memory, Disk Space Information
    
    
    ...
    Uses
      Windows;
    ...
    
    procedure TForm1.MemoryClick(Sender: TObject);
    Var
      Memory: TMemoryStatus;
    
    begin
      InfoMemo.Text := '';
    
      Memory.dwLength := SizeOf(Memory);
      GlobalMemoryStatus(Memory);
    
      InfoMemo.Lines.Add(Format('Memory total: %f GB', [Memory.dwTotalPhys /1024 /1024 /1024]));
      InfoMemo.Lines.Add(Format('Memory free: %f GB', [Memory.dwAvailPhys /1024 /1024 /1024]));
      InfoMemo.Lines.Add(Format('Memory in use: %d%%', [Memory.dwMemoryLoad]));
      InfoMemo.Lines.Add(Format('Pagefile size: %f GB', [Memory.dwTotalPageFile /1024 /1024 /1024]));
      InfoMemo.Lines.Add(Format('Pagefile free: %f GB', [Memory.dwAvailPageFile /1024 /1024 /1024]));
      InfoMemo.Lines.Add(Format('Virtual memory total: %f GB', [Memory.dwTotalVirtual /1024 /1024 /1024]));
      InfoMemo.Lines.Add(Format('Virtual memory free: %f GB', [Memory.dwAvailVirtual /1024 /1024 /1024]));
      InfoMemo.Lines.Add(Format('Disk space total: %f GB', [DiskSize(0) /1024 /1024 /1024]));
      InfoMemo.Lines.Add(Format('Disk space free: %f GB', [DiskFree(0) /1024 /1024 /1024])); 
    end;
    

### Using Windows native wininet for web retrieval
    
    
    {$IFDEF WINDOWS}
    // Need to use Windows WinInet to avoid issue with HTTPS
    // needing two OpenSSL DLLs to be provided with application
    // if using TFPHttpClient.
    // The WinINet API also gets any connection and proxy settings
    // set by Internet Explorer. Blessing or curse?
     
    uses
      wininet;
    
    function GetWebPage(const Url: string): string;
    var
      NetHandle: HINTERNET;
      UrlHandle: HINTERNET;
      Buffer: array[0..1023] of Byte;
      BytesRead: dWord;
      StrBuffer: UTF8String;
    begin
      Result := '';
      BytesRead := Default(dWord);
      NetHandle := InternetOpen('Mozilla/5.0(compatible; WinInet)', INTERNET_OPEN_TYPE_PRECONFIG, nil, nil, 0);
     
      // NetHandle valid?
      if Assigned(NetHandle) then
        Try
          UrlHandle := InternetOpenUrl(NetHandle, PChar(Url), nil, 0, INTERNET_FLAG_RELOAD, 0);
     
          // UrlHandle valid?
          if Assigned(UrlHandle) then
            Try
              repeat
                InternetReadFile(UrlHandle, @Buffer, SizeOf(Buffer), BytesRead);
                SetString(StrBuffer, PAnsiChar(@Buffer[0]), BytesRead);
                Result := Result + StrBuffer;
              until BytesRead = 0;
            Finally
              InternetCloseHandle(UrlHandle);
            end
          // o/w UrlHandle invalid
          else
            ShowMessage('Cannot open URL: ' + Url);
        Finally
          InternetCloseHandle(NetHandle);
        end
      // NetHandle invalid
      else
        raise Exception.Create('Unable to initialize WinInet');
    end;
    {$ENDIF}
    

### Listing all available drives
    
    
    program listdevices;
    
    {$ifdef fpc}{$mode delphi}{$endif}
    {$apptype console}
    
    uses
      Windows;
    
    var
      Drive: Char;
      DriveLetter: string;
      OldMode: Word; 
    begin
      WriteLn('The following drives were found in this computer:');
      WriteLn('');
    
      // Empty Floppy or Zip drives can generate a Windows error.
      // We disable system errors during the listing.
      // Note that another way to skip these errors would be to use DEVICE_IO_CONTROL.
      OldMode := SetErrorMode(SEM_FAILCRITICALERRORS);
      try 
    
        // Search all drive letters
        for Drive := 'A' to 'Z' do
        begin
          DriveLetter := Drive + ':\';
       
          case GetDriveType(PChar(DriveLetter)) of
           DRIVE_REMOVABLE: WriteLn(DriveLetter + ' Floppy Drive');
           DRIVE_FIXED:     WriteLn(DriveLetter + ' Fixed Drive');
           DRIVE_REMOTE:    WriteLn(DriveLetter + ' Network Drive');
           DRIVE_CDROM:     WriteLn(DriveLetter + ' CD-ROM Drive');
           DRIVE_RAMDISK:   WriteLn(DriveLetter + ' RAM Disk');
          end;
        end;
    
      finally
        // Restores previous Windows error mode.
        SetErrorMode(OldMode); 
      end;
    
      // Also add a stop to see the result under Windows
      WriteLn('');
      WriteLn('Please press <ENTER> to exit the program.');
      ReadLn(DriveLetter);
    end.
    

### Creating a shortcut (.lnk) file

Creating a shortcut on the Desktop (can be easily adapted to any location). Adapted from [post by Felipe Monteiro de Carvalho](<http://www.mail-archive.com/fpc-pascal@lists.freepascal.org/msg13325.html>) The ISLink object has more methods that you can use to modify your shortcut... 
    
    
    uses
    ...
    windows, shlobj {for special folders}, ActiveX, ComObj;
    ...
    procedure CreateDesktopShortCut(Target, TargetArguments, ShortcutName: string);
    var
      IObject: IUnknown;
      ISLink: IShellLink;
      IPFile: IPersistFile;
      PIDL: PItemIDList;
      InFolder: array[0..MAX_PATH] of Char;
      TargetName: String;
      LinkName: WideString;
    begin
      { Creates an instance of IShellLink }
      IObject := CreateComObject(CLSID_ShellLink);
      ISLink := IObject as IShellLink;
      IPFile := IObject as IPersistFile;
    
      ISLink.SetPath(pChar(Target));
      ISLink.SetArguments(pChar(TargetArguments));
      ISLink.SetWorkingDirectory(pChar(ExtractFilePath(Target)));
    
      { Get the desktop location }
      SHGetSpecialFolderLocation(0, CSIDL_DESKTOPDIRECTORY, PIDL);
      SHGetPathFromIDList(PIDL, InFolder);
      LinkName := InFolder + PathDelim + ShortcutName+'.lnk';
    
      { Create the link }
      IPFile.Save(PWChar(LinkName), false);
    end;
    

### Opening / Closing a CD or DVD

See the [CD open close](<CD_open_close.md> "CD open close") article for Windows code. 

### Recently used programs list

To make your program appear in the Recently Used Programs List on the Windows Start Menu, see [this article](<recently_used_programs_list.md> "recently used programs list"). 

### Getting special folders (My documents, Desktop, local application data, etc)

Often it is useful to get the location of a special folder such as the desktop. The example below shows how you can get the LocalAppData directory - where you can store user-specific configuration files etc for your programs. 

Use the MyDocuments folder (or a subfolder) to store documents. 

Look in the [shlobj](<http://delphi-miranda-plugins.googlecode.com/svn-history/r105/trunk/FPC/units/src/shlobj.pp>) unit (note: link may not be up to date) for more defines that let you look up the Desktop (note: use CSIDL_DESKTOPDIRECTORY, not CSIDL_DESKTOP), Recycle Bin, etc. 
    
    
    uses 
    ...
    shlobj;
    
    var
      AppDataPath: Array[0..MaxPathLen] of Char; //Allocate memory
    ...
    begin
    ...
        AppDataPath:='';
        SHGetSpecialFolderPath(0,AppDataPath,CSIDL_LOCAL_APPDATA,false);
        writeln('Your local appdata path is: ' + AppDataPath);
    

Another way to achieve the task without using the Windows API is to query a specific environment variable. This solution allows the writing of a more homogeneous system code since, depending on the OS, only the variable name may have to be changed. 

For example: 
    
    
    program UseEnv;
    
    uses sysutils;
    
    begin
      writeln(GetEnvironmentVariable('APPDATA'));
      writeln(GetEnvironmentVariable('PROGRAMFILES'));
      writeln(GetEnvironmentVariable('HOMEPATH'));
      readln;
    end.
    

However this solution is not always as accurate as **SHGetSpecialFolderPath()** , since it is sometimes necessary to check several variables. 

The available variables can be retrieved with **GetEnvironmentString()** : 
    
    
    program ListEnv;
    
    uses sysutils;
    
    var
      i: integer;
    
    begin
      for i in [0..GetEnvironmentVariableCount-1] do
        writeln(GetEnvironmentString(i));
      readln;
    end.
    

### Enabling and disabling devices

The following code can be used to enable and disable Windows devices; it is useful to e.g. reset a serial port or USB device. An example program follows below. 
    
    
    unit controlwindevice;
    
    { Enable Disable windows devices
    
      Copyright (c) 2010-2012 Ludo Brands
    
      Permission is hereby granted, free of charge, to any person obtaining a copy
      of this software and associated documentation files (the "Software"), to
      deal in the Software without restriction, including without limitation the
      rights to use, copy, modify, merge, publish, distribute, sublicense, and/or
      sell copies of the Software, and to permit persons to whom the Software is
      furnished to do so, subject to the following conditions:
    
      The above copyright notice and this permission notice shall be included in
      all copies or substantial portions of the Software.
    
      THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
      IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
      FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
      AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
      LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
      FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
      IN THE SOFTWARE.
    }
    
    
    {$mode delphi}{$H+}
    
    interface
    
    uses
      Classes, SysUtils,dynlibs,windows;
    
    const
      GUID_DEVCLASS_NET : TGUID = '{4D36E972-E325-11CE-BFC1-08002BE10318}';
      GUID_DEVCLASS_PORT : TGUID = '{4D36E978-E325-11CE-BFC1-08002BE10318}';
    
    type
      TDeviceControlResult=(DCROK,DCRErrEnumDeviceInfo,DCRErrSetClassInstallParams,
        DCRErrDIF_PROPERTYCHANGE);
    
    function LoadDevices(GUID_DevClass:TGUID):TStringList;
    function EnableDevice(SelectedItem: DWord):TDeviceControlResult;
    function DisableDevice(SelectedItem: DWord):TDeviceControlResult;
    
    implementation
    
    // Setup api, based on SetupApi.pas JEDI library
    const
        DIF_PROPERTYCHANGE                = $00000012;
        DICS_ENABLE     = $00000001;
        DICS_DISABLE    = $00000002;
        DICS_FLAG_GLOBAL         = $00000001;  // make change in all hardware profiles
        DIGCF_PRESENT         = $00000002;
        SPDRP_DEVICEDESC                  = $00000000; // DeviceDesc (R/W)
        SPDRP_CLASS                       = $00000007; // Class (R--tied to ClassGUID)
        SPDRP_CLASSGUID                   = $00000008; // ClassGUID (R/W)
        SPDRP_FRIENDLYNAME                = $0000000C; // FriendlyName (R/W)
    
    type
      HDEVINFO = Pointer;
      DI_FUNCTION = LongWord;    // Function type for device installer
    
      PSPClassInstallHeader = ^TSPClassInstallHeader;
      SP_CLASSINSTALL_HEADER = packed record
        cbSize: DWORD;
        InstallFunction: DI_FUNCTION;
      end;
      TSPClassInstallHeader = SP_CLASSINSTALL_HEADER;
    
      PSPPropChangeParams = ^TSPPropChangeParams;
      SP_PROPCHANGE_PARAMS = packed record
        ClassInstallHeader: TSPClassInstallHeader;
        StateChange: DWORD;
        Scope: DWORD;
        HwProfile: DWORD;
      end;
      TSPPropChangeParams = SP_PROPCHANGE_PARAMS;
    
      PSPDevInfoData = ^TSPDevInfoData;
      SP_DEVINFO_DATA = packed record
        cbSize: DWORD;
        ClassGuid: TGUID;
        DevInst: DWORD; // DEVINST handle
        Reserved: ULONG_PTR;
      end;
      TSPDevInfoData = SP_DEVINFO_DATA;
    
      TSetupDiEnumDeviceInfo = function(DeviceInfoSet: HDEVINFO;
        MemberIndex: DWORD; var DeviceInfoData: TSPDevInfoData): LongBool; stdcall;
      TSetupDiSetClassInstallParamsA = function(DeviceInfoSet: HDEVINFO;
        DeviceInfoData: PSPDevInfoData; ClassInstallParams: PSPClassInstallHeader;
        ClassInstallParamsSize: DWORD): LongBool; stdcall;
      TSetupDiSetClassInstallParamsW = function(DeviceInfoSet: HDEVINFO;
        DeviceInfoData: PSPDevInfoData; ClassInstallParams: PSPClassInstallHeader;
        ClassInstallParamsSize: DWORD): LongBool; stdcall;
      TSetupDiSetClassInstallParams = TSetupDiSetClassInstallParamsA;
      TSetupDiCallClassInstaller = function(InstallFunction: DI_FUNCTION;
        DeviceInfoSet: HDEVINFO; DeviceInfoData: PSPDevInfoData): LongBool; stdcall;
      TSetupDiGetClassDevs = function(ClassGuid: PGUID; const Enumerator: PAnsiChar;
        hwndParent: HWND; Flags: DWORD): HDEVINFO; stdcall;
      TSetupDiGetDeviceRegistryPropertyA = function(DeviceInfoSet: HDEVINFO;
        const DeviceInfoData: TSPDevInfoData; Property_: DWORD;
        var PropertyRegDataType: DWORD; PropertyBuffer: PBYTE; PropertyBufferSize: DWORD;
        var RequiredSize: DWORD): BOOL; stdcall;
      TSetupDiGetDeviceRegistryPropertyW = function(DeviceInfoSet: HDEVINFO;
        const DeviceInfoData: TSPDevInfoData; Property_: DWORD;
        var PropertyRegDataType: DWORD; PropertyBuffer: PBYTE; PropertyBufferSize: DWORD;
        var RequiredSize: DWORD): BOOL; stdcall;
      TSetupDiGetDeviceRegistryProperty = TSetupDiGetDeviceRegistryPropertyA;
    
    var
      DevInfo: hDevInfo;
      SetupDiEnumDeviceInfo: TSetupDiEnumDeviceInfo;
      SetupDiSetClassInstallParams: TSetupDiSetClassInstallParams;
      SetupDiCallClassInstaller: TSetupDiCallClassInstaller;
      SetupDiGetClassDevs: TSetupDiGetClassDevs;
      SetupDiGetDeviceRegistryProperty: TSetupDiGetDeviceRegistryProperty;
    
    var
      SetupApiLoadCount:integer=0;
    
    function LoadSetupApi: Boolean;
    var SetupApiLib:TLibHandle;
    begin
      Result := True;
      Inc(SetupApiLoadCount);
      if SetupApiLoadCount > 1 then
        Exit;
      SetupApiLib:=LoadLibrary('SetupApi.dll');
      Result := SetupApiLib<>0;
      if Result then
      begin
        SetupDiEnumDeviceInfo := GetProcedureAddress(SetupApiLib, 'SetupDiEnumDeviceInfo');
        SetupDiSetClassInstallParams := GetProcedureAddress(SetupApiLib, 'SetupDiSetClassInstallParamsA');
        SetupDiCallClassInstaller := GetProcedureAddress(SetupApiLib, 'SetupDiCallClassInstaller');
        SetupDiGetClassDevs := GetProcedureAddress(SetupApiLib, 'SetupDiGetClassDevsA');
        SetupDiGetDeviceRegistryProperty := GetProcedureAddress(SetupApiLib, 'SetupDiGetDeviceRegistryPropertyA');
      end;
    end;
    
    // implementation
    
    function StateChange(NewState, SelectedItem: DWord;
      hDevInfo: hDevInfo): TDeviceControlResult;
    var
      PropChangeParams: TSPPropChangeParams;
      DeviceInfoData: TSPDevInfoData;
    begin
      PropChangeParams.ClassInstallHeader.cbSize := SizeOf(TSPClassInstallHeader);
      DeviceInfoData.cbSize := SizeOf(TSPDevInfoData);
      // Get a handle to the Selected Item.
      if (not SetupDiEnumDeviceInfo(hDevInfo, SelectedItem, DeviceInfoData)) then
      begin
        Result := DCRErrEnumDeviceInfo;
        exit;
      end;
      // Set the PropChangeParams structure.
      PropChangeParams.ClassInstallHeader.InstallFunction := DIF_PROPERTYCHANGE;
      PropChangeParams.Scope := DICS_FLAG_GLOBAL;
      PropChangeParams.StateChange := NewState;
      if (not SetupDiSetClassInstallParams(hDevInfo, @DeviceInfoData,
         PSPClassInstallHeader(@PropChangeParams), SizeOf(PropChangeParams))) then
      begin
        Result := DCRErrSetClassInstallParams;
        exit;
      end;
      // Call the ClassInstaller and perform the change.
      if (not SetupDiCallClassInstaller(DIF_PROPERTYCHANGE, hDevInfo, @DeviceInfoData)) then
      begin
        Result := DCRErrDIF_PROPERTYCHANGE;
        exit;
      end;
      Result := DCROK;
    end;
    
    function GetRegistryProperty(PnPHandle: HDEVINFO;
      DevData: TSPDevInfoData; Prop: DWORD; Buffer: PChar;
      dwLength: DWord): Boolean;
    var
      aBuffer: array[0..256] of Char;
    begin
      dwLength := 0;
      aBuffer[0] := #0;
      SetupDiGetDeviceRegistryProperty(PnPHandle, DevData, Prop, Prop, PBYTE(@aBuffer[0]), SizeOf(aBuffer), dwLength);
      StrCopy(Buffer, aBuffer);
      Result := Buffer^ <> #0;
    end;
    
    function ConstructDeviceName(DeviceInfoSet: hDevInfo;
      DeviceInfoData: TSPDevInfoData; Buffer: PChar; dwLength: DWord): Boolean;
    const
      UnknownDevice = '<Unknown Device>';
    begin
      if (not GetRegistryProperty(DeviceInfoSet, DeviceInfoData, SPDRP_FRIENDLYNAME, Buffer, dwLength)) then
      begin
        if (not GetRegistryProperty(DeviceInfoSet, DeviceInfoData, SPDRP_DEVICEDESC, Buffer, dwLength)) then
        begin
          if (not GetRegistryProperty(DeviceInfoSet, DeviceInfoData, SPDRP_CLASS, Buffer, dwLength)) then
          begin
            if (not GetRegistryProperty(DeviceInfoSet, DeviceInfoData, SPDRP_CLASSGUID, Buffer, dwLength)) then
            begin
              dwLength := DWord(SizeOf(UnknownDevice));
              Buffer := Pointer(LocalAlloc(LPTR, Cardinal(dwLength)));
              StrCopy(Buffer, UnknownDevice);
            end;
          end;
        end;
      end;
      Result := true;
    end;
    
    
    function LoadDevices(GUID_DevClass:TGUID):TStringList;
    var
      DeviceInfoData: TSPDevInfoData;
      i: DWord;
      pszText: PChar;
    
    begin
      if (not LoadSetupAPI) then
        begin
        result:=nil;
        exit;
        end;
      DevInfo := nil;
      // Get a handle to all devices in all classes present on system
      DevInfo := SetupDiGetClassDevs(@GUID_DevClass, nil, 0, DIGCF_PRESENT);
      if (DevInfo = Pointer(INVALID_HANDLE_VALUE)) then
      begin
        result:=nil;
        exit;
      end;
      Result:=TStringList.Create;
      DeviceInfoData.cbSize := SizeOf(TSPDevInfoData);
      i := 0;
      // Enumerate though all the devices.
      while SetupDiEnumDeviceInfo(DevInfo, i, DeviceInfoData) do
      begin
        GetMem(pszText, 256);
        try
          // Get a friendly name for the device.
          ConstructDeviceName(DevInfo, DeviceInfoData, pszText, DWord(nil));
          Result.AddObject(pszText,Tobject(i));
        finally
          FreeMem(pszText);
          inc(i);
        end;
      end;
    end;
    
    function EnableDevice(SelectedItem: DWord):TDeviceControlResult;
    
    begin
      result:=StateChange(DICS_ENABLE, SelectedItem , DevInfo);
    end;
    
    function DisableDevice(SelectedItem: DWord):TDeviceControlResult;
    
    begin
      result:=StateChange(DICS_DISABLE, SelectedItem , DevInfo);
    end;
    
    end.
    

Example program that lists all ports preceeded by a number. 

Enter a number and the port will be disabled. Enter return again and the port will be enabled again. 
    
    
    program devicetest;
    
    {$mode delphi}{$H+}
    
    uses
      Classes, controlwindevice;
    var
      sl:tstringlist;
      i:integer;
    begin
      sl:=Loaddevices(GUID_DEVCLASS_PORT);
      for i:=0 to sl.count-1 do
        writeln(i,' : ',sl[i]);
      readln(i);
      if DisableDevice(i)=DCROK then
        writeln(sl[i],' disabled');
      readln;
      if EnableDevice(i)=DCROK then
        writeln(sl[i],' enabled');
      sl.Free;
      readln;
    end.
    

### Downloading a file using urlmon

Urlmon.dll is built into Windows and can be used to e.g. download a file from a web site. It supports SSL/TLS connections. 

Windows-only; please look into libraries like [fphttpclient](<fphttpclient.md> "fphttpclient"), [Synapse](<Synapse.md> "Synapse") and [Indy](</index.php?title=Indy&action=edit&redlink=1> "Indy \(page does not exist\)") for cross-platform solutions. 
    
    
    function URLDownloadToFile(pCaller: pointer; URL: PChar; FileName: PChar; Reserved: DWORD; lpfnCB : pointer): HResult; stdcall; external 'urlmon.dll' name 'URLDownloadToFileA';
    
    procedure TForm1.Button1Click(Sender: TObject);
    var Source, Dest: string;
    begin
     Source:='http://lazarus.freepascal.org';
     Dest:='C:\Windows\temp\data.txt';
     if URLDownloadToFile(nil, PChar(Source), PChar(Dest), 0, nil)=0 then
      showmessage('Download ok!')
     else
      showMessage('Error downloading '+Source);
    end;
    

### Showing/finding processes

Use code like this to find a process handle based on the executable name (akin to the tasklist command): 
    
    
    program ProcessFindPID;
    
    {$mode objfpc}{$H+}
    
    uses 
      Classes, Sysutils, Windows, JwaTlHelp32;
    
    function QueryFullProcessImageNameW(hProcess: HANDLE; dwFlags: DWORD; lpExeName: LPTSTR;
    var lpdwSize: DWORD): BOOL; stdcall; external 'KERNEL32';
    
    function FindInProcesses(const PName: string): DWord;
      // Looks for process with PName executable and return
    var
      i: integer;
      CPID: DWORD;
      CProcName: array[0..259] of char;
      S: HANDLE;
      PE: TProcessEntry32;
    begin
      Result := 0;
      CProcName := '';
      S := CreateToolHelp32Snapshot(TH32CS_SNAPALL, 0); // Create snapshot
      PE.DWSize := SizeOf(PE); // Set size before use
      I := 1;
      if Process32First(S, PE) then
        repeat
          CProcName := PE.szExeFile;
          CPID := PE.th32ProcessID;
          //if CProcName = '' then Writeln(IntToStr(i) + ' - (' + IntToStr(CPID) + ') Failed to get a process name')
          Inc(i);
          if UpperCase(CProcName) = UpperCase(PName) then
            // Found the name. Set Result to the PID of process found
            Result := CPID;
        until not Process32Next(S, PE);
      CloseHandle(S);
    end;
    
    begin
      writeln('Explorer.exe has process id '+inttostr(FindInProcesses('explorer.exe')));
    end.
    

### Identify Windows Version

There is a code example at [WindowsVersion](<WindowsVersion.md> "WindowsVersion")

### Detecting Light or Dark theme

A dark theme was introduced in the Windows 10 May 2019 Update. Users can enable or disable the dark theme via Settings > Personalization > Colors. Select the drop-down menu for "Choose your color" and pick Light, Dark, or Custom. Light or Dark changes the look of the Windows Start menu and the built-in applications. For details of how to detect the theme, see [Dark theme](<Dark_theme.md> "Dark theme"). 

### Task bar icon not showing on secondary monitor

Without using a screen dump, here is a description of the issue: 

  * The Lazarus IDE is on the left (primary) monitor.
  * The application uses XML to save the last window position, and appears on the right (secondary) monitor.
  * The application's icon always appears on the primary monitor taskbar, both when run from inside the IDE and when run standalone outside the IDE.



The solution is to add these lines to the application's project source file before the `Application.CreateForm()` call: 
    
    
    {$IFDEF WINDOWS}
    Application.MainFormOnTaskBar := True;
    {$ENDIF}
    

The $IFDEF/$ENDIF is needed if you are compiling the application for multiple operating systems because this is a Windows-specific code. 

### Retrieving hard disk serial number etc

Add this unit to your project: 
    
    
    unit hddinfo;
    {$ifdef fpc}{$mode delphi}{$endif}
    interface
    
    uses Windows, SysUtils, Classes;
    
    const
      IOCTL_STORAGE_QUERY_PROPERTY = $2D1400;
    
    type
    THDDInfo = class (TObject)
    private
      FDriveNumber: Byte;
      FFileHandle: Cardinal;
      FInfoAvailable: Boolean;
      FProductRevision: string;
      FProductId: string;
      FSerialNumber: string;
      FVendorId: string;
      procedure ReadInfo;
      procedure SetDriveNumber(const Value: Byte);
    public
      constructor Create;
      property DriveNumber: Byte read FDriveNumber write SetDriveNumber;
      property VendorId: string read FVendorId;
      property ProductId: string read FProductId;
      property ProductRevision: string read FProductRevision;
      property SerialNumber: string read FSerialNumber;
      function SerialNumberInt: Cardinal;
      function SerialNumberText: string;
      function IsInfoAvailable: Boolean;
    end;
    
    implementation
    
    type
    
    STORAGE_PROPERTY_QUERY = packed record
      PropertyId: DWORD;
      QueryType: DWORD;
      AdditionalParameters: array[0..3] of Byte;
    end;
    
    STORAGE_DEVICE_DESCRIPTOR = packed record
      Version: ULONG;
      Size: ULONG;
      DeviceType: Byte;
      DeviceTypeModifier: Byte;
      RemovableMedia: Boolean;
      CommandQueueing: Boolean;
      VendorIdOffset: ULONG;
      ProductIdOffset: ULONG;
      ProductRevisionOffset: ULONG;
      SerialNumberOffset: ULONG;
      STORAGE_BUS_TYPE: DWORD;
      RawPropertiesLength: ULONG;
      RawDeviceProperties: array[0..511] of Byte;
    end;
    
    function ByteToChar(const B: Byte): Char;
    begin
      Result := Chr(B + $30)
    end;
    
    function SerialNumberToCardinal (SerNum: String): Cardinal;
    begin
      HexToBin(PChar(SerNum), PChar(@Result), SizeOf(Cardinal));
    end;
    
    function SerialNumberToString(SerNum: String): String;
    var
      I, StrLen: Integer;
      Pair: string;
      B: Byte;
      Ch: Char absolute B;
    
    begin
      Result := '';
      StrLen := Length(SerNum);
    
      if Odd(StrLen) then Exit;
    
      I := 1;
    
      while I < StrLen do
      begin
        Pair := Copy (SerNum, I, 2);
        HexToBin(PChar(Pair), PChar(@B), 1);
        Result := Result + Chr(B);
        Inc(I, 2);
      end;
    
      I := 1;
    
      while I < Length(Result) do
      begin
        Ch := Result[I];
        Result[I] := Result[I + 1];
        Result[I + 1] := Ch;
        Inc(I, 2);
      end;
    end;
    
    constructor THddInfo.Create;
    begin
      inherited;
    
      SetDriveNumber(0);
    end;
    
    function THDDInfo.IsInfoAvailable: Boolean;
    begin
      Result := FInfoAvailable
    end;
    
    procedure THDDInfo.ReadInfo;
    type
      PCharArray = ^TCharArray;
      TCharArray = array[0..32767] of Char;
    
    var
      Returned: Cardinal;
      Status: LongBool;
      PropQuery: STORAGE_PROPERTY_QUERY;
      DeviceDescriptor: STORAGE_DEVICE_DESCRIPTOR;
      PCh: PChar;
    
    begin
      FInfoAvailable := False;
      FProductRevision := '';
      FProductId := '';
      FSerialNumber := '';
      FVendorId := '';
    
      try
        FFileHandle := CreateFile(
                         PChar('\\.\PhysicalDrive' + ByteToChar(FDriveNumber)),
                         0,
                         FILE_SHARE_READ or FILE_SHARE_WRITE,
                         nil,
                         OPEN_EXISTING,
                         0,
                         0
                       );
    
        if FFileHandle = INVALID_HANDLE_VALUE then RaiseLastOSError;
    
        ZeroMemory(@PropQuery, SizeOf(PropQuery));
        ZeroMemory(@DeviceDescriptor, SizeOf(DeviceDescriptor));
    
        DeviceDescriptor.Size := SizeOf(DeviceDescriptor);
    
        Status := DeviceIoControl(
                    FFileHandle,
                    IOCTL_STORAGE_QUERY_PROPERTY,
                    @PropQuery,
                    SizeOf(PropQuery),
                    @DeviceDescriptor,
                    DeviceDescriptor.Size,
                    Returned,
                    nil
                  );
    
        if not Status then RaiseLastOSError;
    
        if DeviceDescriptor.VendorIdOffset <> 0 then
        begin
          PCh := @PCharArray(@DeviceDescriptor)^[DeviceDescriptor.VendorIdOffset];
          FVendorId := PCh;
        end;
    
        if DeviceDescriptor.ProductIdOffset <> 0 then
        begin
          PCh := @PCharArray(@DeviceDescriptor)^[DeviceDescriptor.ProductIdOffset];
          FProductId := PCh;
        end;
    
        if DeviceDescriptor.ProductRevisionOffset <> 0 then
        begin
          PCh := @PCharArray(@DeviceDescriptor)^[DeviceDescriptor.ProductRevisionOffset];
          FProductRevision := PCh;
        end;
    
        if DeviceDescriptor.SerialNumberOffset <> 0 then
        begin
          PCh := @PCharArray(@DeviceDescriptor)^[DeviceDescriptor.SerialNumberOffset];
          FSerialNumber := PCh;
        end;
    
        FInfoAvailable := True;
      finally
        if FFileHandle <> INVALID_HANDLE_VALUE then CloseHandle(FFileHandle);
      end;
    end;
    
    function THDDInfo.SerialNumberInt: Cardinal;
    begin
      Result := 0;
      if ((IsInfoAvailable = True) and (FSerialNumber <> '')) then Result := SerialNumberToCardinal(FSerialNumber)
    end;
    
    function THDDInfo.SerialNumberText: string;
    begin
      Result := '';
      if ((IsInfoAvailable = True) and (FSerialNumber <> '')) then Result := SerialNumberToString(FSerialNumber)
    end;
    
    procedure THDDInfo.SetDriveNumber(const Value: Byte);
    begin
      FDriveNumber := Value;
      ReadInfo;
    end;
    
    end.
    

To use the hddinfo unit: 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls, Hddinfo;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        Memo1: TMemo;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      HDDInfo : THDDInfo;
    begin
      HDDInfo := THDDInfo.Create();
      HDDInfo.DriveNumber := 0;
      if HDDInfo.IsInfoAvailable then
        try
          begin
            Memo1.Lines.Add('DriveNum  = ' + HDDInfo.DriveNumber.ToString);
            Memo1.Lines.Add('VendorID  = ' + HDDInfo.VendorId);
            Memo1.Lines.Add('ProductID = ' + HDDInfo.ProductId);
            Memo1.Lines.Add('Revision  = ' + HDDInfo.ProductRevision);
            Memo1.Lines.Add('SerialNo  = ' + HDDInfo.SerialNumberText);
            Memo1.Lines.Add('SerialNo  = ' + HDDInfo.SerialNumber);
            Memo1.Lines.Add('SerialNo  = ' + HDDInfo.SerialNumberInt.ToString);
          end;
        finally
          HDDInfo.Free;
        end;
    end;
    
    end.
    

Note: The THDDInfo Delphi unit is Copyright Artem Parlyuk - free for commercial use but requires the copyright to be acknowledged (eg in About). Distribution of the source code is allowed as long as no alternations are made to the contents. 

### Retrieving system information

The SMBIOS (System Management BIOS) is a standard developed by the [DMTF](<https://www.dmtf.org/>). The information stored in the SMBIOS includes device manufacturers, model name, serial number, BIOS version, asset tag, processors, ports and installed device memory. The [TSMBIOS library](<https://github.com/RRUZ/tsmbios/>) allows access to the System Management BIOS (SMBIOS) using the Object Pascal language (Delphi or Free Pascal). 

## Troubleshooting

### A referral was returned from the server

From Windows 7 onwards this error can occur whereby a dialog box appears with the message "A referral was returned from the server" and your program will not execute. It turns out that the culprit is enabling "UI Access" in Project Options > Application > For Windows [x] UI Access. Uncheck that UI Access option, recompile and the blocking error disappears! 

What is UI Access? Microsoft documentation states: "Applications that are not providing accessibility should set this flag to false. Applications that are required to drive input to other windows on the desktop (on-screen keyboard, for example) should set this value to true." 

### Lazarus 32 bit: External SIGSEGV exception when running with debugging

There is a common problem with Lazarus 32 Bit: If "Automatic" is choosen as type of debugging info, Lazarus chooses the "Stabs" format which is outdated for Windows. 

Go to Project > Project options > Compiler Options > Debugging in the Lazarus Menu and choose "Dwarf with sets" as the debugger info. 

### FPC 2.6.x/Lazarus warning (Missing support for SEH)

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** If you can avoid it, do not use Win64 builds of Lazarus based on FPC 2.6.x and earlier (this includes Lazarus 1.x). See below for details.

Please be aware that all Lazarus 1.x releases use FPC 2.6.x. FPC 2.6.x (and probably earlier versions as well) does not properly support Windows 64 bit. Therefore, please use the 32 bit Lazarus IDE on Win64. If it is absolutely necessary to build 64 bit executables (eg explorer extensions), please install the 64 bit cross compiler add-on for the 32 bit IDE. 

Details of the bug: on Windows 64, exceptions in (third party) DLLs may be raised using SEH. These exceptions should be handled within the DLL itself. 

However, FPC sees an (incorrect) FPC exception which may cause your programs (or Lazarus) to crash. This applies to DLLs like printer drivers, database drivers, Windows explorer extensions. 

The issue has been fixed in the FPC development version but it is a major change and it will not be backported to FPC 2.6.x. 

Relevant bug reports include <http://bugs.freepascal.org/view.php?id=12742>

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
  * [WinCE Programming Tips](<WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Using the telephone API, sending SMSes, and more...
  * Windows Programming Tips \- Desktop Windows programming tips



### Interface Development Articles

  * [Carbon interface internals](<Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset

---

_Source: [https://wiki.freepascal.org/Windows_Programming_Tips](https://web.archive.org/web/20250317192349/https://wiki.freepascal.org/Windows_Programming_Tips)_
