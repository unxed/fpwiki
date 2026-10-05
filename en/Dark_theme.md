# Dark theme

## Contents

  * 1 Detect Dark Theme
    * 1.1 Universal method
      * 1.1.1 Example 1
      * 1.1.2 Example 2
    * 1.2 Windows method
      * 1.2.1 Example 1
    * 1.3 macOS method
      * 1.3.1 Example 1
        * 1.3.1.1 Demo
      * 1.3.2 Example 2
        * 1.3.2.1 Demo
      * 1.3.3 Opting out of dark mode
      * 1.3.4 Opting in to dark mode
  * 2 Dark Windows title bar
  * 3 Activate Dark Theme
    * 3.1 Windows IDE
    * 3.2 MacOS
  * 4 See Also
    * 4.1 External links



# Detect Dark Theme

How to detect whether the operating system is using a dark theme? 

## Universal method

The following method is based on the GUI colors. It works on all platforms but those colors are not updated when the theme is changed while the application is running. Thus it cannot allow to detect if the dark mode is toggled. 

### Example 1
    
    
    // by "Alextp" from Lazarus forum
    function IsDarkTheme: boolean;
    const
      cMax = $A0;
    var
      N: TColor;
    begin
      N:= ColorToRGB(clWindow);
      Result:= (Red(N)<cMax) and (Green(N)<cMax) and (Blue(N)<cMax);
    end;
    

### Example 2
    
    
    // by "Hansaplast" & "Alextp" from Lazarus forum
    function IsDarkTheme: boolean;
      function _Level(C: TColor): double;
      begin
        Result:= Red(C)*0.3 + Green(C)*0.59 + Blue(C)*0.11;
      end;
    begin
      Result:= _Level(ColorToRGB(clWindow)) < _Level(ColorToRGB(clWindowText));
    end;
    

## Windows method

### Example 1
    
    
    // by "jwdietrich" from Lazarus forum
    uses
      Windows, Win32Proc, Registry;
         
    // IsDarkTheme: Detects if the Dark Theme (true) has been enabled or not (false)
    function IsDarkTheme: boolean;
    const
      KEYPATH = '\Software\Microsoft\Windows\CurrentVersion\Themes\Personalize';
      KEYNAME = 'AppsUseLightTheme';
    var
      LightKey: boolean;
      Registry: TRegistry;
    begin
      Result := false;
      Registry := TRegistry.Create;
      try
        Registry.RootKey := HKEY_CURRENT_USER;
        if Registry.OpenKeyReadOnly(KEYPATH) then
          begin
            if Registry.ValueExists(KEYNAME) then
              LightKey := Registry.ReadBool(KEYNAME)
            else
              LightKey := true;
          end
        else
          LightKey := true;
        Result := not LightKey
      finally
        Registry.Free;
      end;
    end;
    

## macOS method

### Example 1
    
    
    // by "trev" from Lazarus forum
       
    { returns true, if this app runs on macOS 10.14.0 Mojave or newer }
    function MojaveOrNewer: boolean;
    var
      minOsVer: NSOperatingSystemVersion;
    begin
      //Setup minimum version (Mojave)
      minOsVer.majorVersion:= 10;
      minOsVer.minorVersion:= 14;
      minOsVer.patchVersion:= 0;
    
      // Check minimum version
      if(NSProcessInfo.ProcessInfo.isOperatingSystemAtLeastVersion(minOSVer)) then
        Result := True
      else
        Result := False;
    end;
         
    { The following two functions were suggested by Hansaplast at https://forum.lazarus.freepascal.org/index.php/topic,43111.msg304366.html }
         
    // Retrieve key's string value from user preferences. Result is encoded using NSStrToStr's default encoding.
    function GetPrefString(const KeyName : string) : string;
    begin
      Result := NSStringToString(NSUserDefaults.standardUserDefaults.stringForKey(NSStr(@KeyName[1])));
    end;
         
    // IsDarkTheme: Detects if the Dark Theme (true) has been enabled or not (false)
    function IsDarkTheme: boolean;
    begin
      Result := false;
      if MojaveOrNewer then
        Result := pos('DARK',UpperCase(GetPrefString('AppleInterfaceStyle'))) > 0;
    end;
    

#### Demo
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Classes, Forms, Dialogs, StdCtrls, SysUtils,
      CocoaAll, CocoaUtils, MacOSAll;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    { returns true, if this app runs on macOS 10.14 Mojave or newer }
    function MojaveOrNewer: boolean;
    var
      minOsVer: NSOperatingSystemVersion;
    begin
      // Setup minimum version (Mojave)
      minOsVer.majorVersion:= 10;
      minOsVer.minorVersion:= 14;
      minOsVer.patchVersion:= 0;
    
      // Check minimum version
      if(NSProcessInfo.ProcessInfo.isOperatingSystemAtLeastVersion(minOSVer)) then
        Result := True
      else
        Result := False;
    end;
    
    function GetPrefString(const KeyName : string) : string;
    begin
      Result := NSStringToString(NSUserDefaults.standardUserDefaults.stringForKey(NSStr(@KeyName[1])));
    end;
    
    function IsDarkTheme: boolean;
    begin
      Result := false;
      if MojaveOrNewer then
        Result := pos('DARK',UpperCase(GetPrefString('AppleInterfaceStyle'))) > 0;
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      if(IsDarkTheme) then
        ShowMessage('Dark mode')
      else
        ShowMessage('Light mode');
    end;
    
    end.
    

### Example 2

Catalina added an "Auto" option where the computer switches between Light and Dark modes depending on time of day. The 'AppleInterfaceStyle' method apparently doesn't work if auto is enabled. 

The solution is to get the NSApp.effectiveAppearance string which will be one of a number of values including 'NSAppearanceNameAqua' (standard light mode) and 'NSAppearanceNameDarkAqua' (standard dark mode). Google these to see the whole list which includes other light and dark modes with added contrast. The effectiveAppearance is correct in all modes including when auto mode kicks in. 

Tested on Mojave, Catalina, Big Sur. 
    
    
    // by "Clover" from Lazarus forum
    function IsMacDarkMode: Boolean;
    var
      sMode: string;
    begin
      //sMode := CFStringToStr( CFStringRef( NSUserDefaults.StandardUserDefaults.stringForKey( NSSTR('AppleInterfaceStyle') ))); // Doesn't work in auto mode
      sMode  := CFStringToStr( CFStringRef( NSApp.effectiveAppearance.name ));
      Result := Pos('Dark', sMode) > 0;
    end;
    

#### Demo
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Classes, Forms, Dialogs, StdCtrls,
      CocoaAll, CocoaUtils, MacOSAll;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    function IsMacDarkMode: Boolean;
    var
      sMode: string;
    begin
      sMode  := CFStringToStr( CFStringRef( NSApp.effectiveAppearance.name ));
      Result := Pos('Dark', sMode) > 0;
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      if(IsMacDarkMode) then
        ShowMessage('Dark mode')
      else
        ShowMessage('Light mode');
    end;
    
    end.
    

### Opting out of dark mode

Mac systems automatically opt in any application linked against the macOS 10.14 or later SDK to both light and dark appearances. You can opt out of dark mode by including the NSRequiresAquaSystemAppearance key (with a value of YES) in your application’s [Info.plist](<macOS_property_list_files.md> "macOS property list files") property list file. Setting this key to YES causes the system to ignore the user's preference and always apply a light appearance to your application. 

### Opting in to dark mode

If you build your application against an earlier SDK but still want to support Dark Mode, include the NSRequiresAquaSystemAppearance key (with a value of NO) in your application's [Info.plist](<macOS_property_list_files.md> "macOS property list files") property list file. Do so only if your application's appearance looks correct when running in macOS 10.14 and later with Dark Mode enabled. 

# Dark Windows title bar

For Windows 10 and higher versions it is possible to activate dark title bar of the application window. 
    
    
    uses LCLType;
     
    function IsWindows10OrGreater(BuildNumber: Integer): Boolean;
    begin
      Result := (Win32MajorVersion >= 10) and (Win32BuildNumber >= BuildNumber);
    end;
    
    procedure SetDarkModeTitleBar(AForm: TForm; Active: Bool);
    type
      TDwmSetWindowAttribute = function(hwnd: HWND; dwAttribute: DWORD; pvAttribute: Pointer; cbAttribute: DWORD): HRESULT; stdcall;
    const
      DwmapiLibName = 'dwmapi.dll';
      DWMWA_USE_IMMERSIVE_DARK_MODE_BEFORE_20H1 = 19;
      DWMWA_USE_IMMERSIVE_DARK_MODE = 20;
    var
      DwmapiLib: TLibHandle;
      DwmSetWindowAttribute: TDwmSetWindowAttribute;
      Attr: DWord;
    begin
      DwmapiLib := LoadLibrary(DwmapiLibName);
      if DwmapiLib <> 0 then DwmSetWindowAttribute := GetProcAddress(DwmapiLib, 'DwmSetWindowAttribute')
        else DwmSetWindowAttribute := nil;
    
      if Assigned(DwmSetWindowAttribute) and IsWindows10OrGreater(17763) then begin
        Attr := DWMWA_USE_IMMERSIVE_DARK_MODE_BEFORE_20H1;
        if IsWindows10OrGreater(18985) then Attr := DWMWA_USE_IMMERSIVE_DARK_MODE;
    
        DwmSetWindowAttribute(AForm.Handle, Attr, @Active, SizeOf(Active));
      end;
      if DwmapiLib <> 0 then FreeLibrary(DwmapiLib);
    end;
    

# Activate Dark Theme

## Windows IDE

To make the Lazarus IDE in dark themed, you can use the package **metadarkstyledsgn.lpk**. At the time of writing 17th Oct 2024 the version in the _online package manager_ is broken. Use the git version [metadarkstyle](<https://github.com/zamtmn/metadarkstyle>). 

## MacOS

  * On Intel Macs you need to compile with _-WM10.14_ to support dark mode. Better use **-WM11** to also support modern style form. 
    * For the IDE: add it to the Tools / Configure "Build Lazarus" / Options
    * For a project: add it to Project / Project Options / Compiler Options / Custom Options



# See Also

  * [macOS extensions](<macOS_extensions.md> "macOS extensions")
  * [User provided syntax highlighting - color schemes](<UserSuppliedSchemeSettings.md> "UserSuppliedSchemeSettings")



## External links

  * [Apple: Mojave Dark Mode](<https://developer.apple.com/design/human-interface-guidelines/macos/visual-design/dark-mode/>).

---

_Source: [https://wiki.freepascal.org/Dark_theme](https://web.archive.org/web/20250120201131/https://wiki.freepascal.org/Dark_theme)_
