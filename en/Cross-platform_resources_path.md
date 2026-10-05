# Cross-platform resources path

A solution how to detect "resources path": 

  * on Windows the data files are stored on the same directory as the executable (or any other directory based on it, like ResourcesPath + 'data' + PathDelim + 'myfile.dat') 
  * on Unixes it will be on a directory read from a configuration file. If no configuration file exists or it contains no info, then a constant ('/usr/share/myapp/') is utilized as the default directory. 



[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The behaviour of this code under Windows SEEMS WRONG given the above explanation - unless it is meant for read-only access!

The configuration file path is located with the GetAppConfigFile function from the Free Pascal Runtime Library. 

Below is a full unit which you can use at your applications. 
    
    
    unit appsettings;
     
    interface
     
    {$ifdef fpc}
      {$mode delphi}{$H+}
    {$endif}
     
    uses
      Classes, SysUtils, Forms, IniFiles, constants;
     
    type
     
     { TConfigurations }
     
     TConfigurations = class(TObject)
     private
       function GetResourcesPath: string;
     public
       {other settings as fields here}
       ConfigFilePath: string;
       ResourcesPath: string;
       constructor Create;
       destructor Destroy; override;
       procedure ReadFromFile(Sender: TObject);
       procedure Save(Sender: TObject);
     end;
     
     
    var
     vConfigurations: TConfigurations;
     
    implementation
     
    {$IFDEF Win32}
    uses
      Windows;
    {$ENDIF}
    {$ifdef Darwin}
    uses
      MacOSAll;
    {$endif}
     
    const
      DefaultDirectory = '/usr/share/myapp/';
      BundleResourcesDirectory = '/Contents/Resources/';
     
      SectionGeneral = 'General';
      SectionUnix = 'UNIX';
     
      IdentResourcesPath = 'ResourcesPath';
     
    { TConfigurations }
     
    constructor TConfigurations.Create;
    begin
    {$ifdef win32}
     ConfigFilePath := ExtractFilePath(Application.EXEName) + 'myapp.ini';
    {$endif}
    {$ifdef Unix}
     ConfigFilePath := GetAppConfigFile(False) + '.conf';
    {$endif}
     
      ResourcesPath := GetResourcesPath();
     
      ReadFromFile(nil);
    end;
     
    destructor TConfigurations.Destroy;
    begin
      Save(nil);
     
      inherited Destroy;
    end;
     
    procedure TConfigurations.Save(Sender: TObject);
    var
      MyFile: TIniFile;
    begin
      MyFile := TIniFile.Create(ConfigFilePath);
      try
        MyFile.WriteString(SectionUnix, IdentResourcesPath, ResourcesPath);
      finally
        MyFile.Free;
      end;
    end;
     
    procedure TConfigurations.ReadFromFile(Sender: TObject);
    var
     MyFile: TIniFile;
    begin
     MyFile := TIniFile.Create(ConfigFilePath);
     try
      // Here you can read other information from the config file
     
    {$ifdef Win32}
       ResourcesPath := MyFile.ReadString(SectionUnix, IdentResourcesPath,
    ExtractFilePath(Application.EXEName));
    {$else}
      {$ifndef darwin}
       ResourcesPath := MyFile.ReadString(SectionUnix, IdentResourcesPath,
    DefaultDirectory);
      {$endif}
    {$endif}
     finally
       MyFile.Free;
     end;
    end;
     
    function TConfigurations.GetResourcesPath(): string;
    begin
    {$ifdef Darwin}
    var
      pathRef: CFURLRef;
      pathCFStr: CFStringRef;
      pathStr: shortstring;
    {$endif}
    begin
    {$ifdef UNIX}
    {$ifdef Darwin}
      pathRef := CFBundleCopyBundleURL(CFBundleGetMainBundle());
      pathCFStr := CFURLCopyFileSystemPath(pathRef, kCFURLPOSIXPathStyle);
      CFStringGetPascalString(pathCFStr, @pathStr, 255, CFStringGetSystemEncoding());
      CFRelease(pathRef);
      CFRelease(pathCFStr);
     
      Result := pathStr + BundleResourcesDirectory;
    {$else}
      Result := DefaultDirectory;
    {$endif}
    {$endif}
     
    {$ifdef Windows}
      Result := ExtractFilePath(Application.EXEName);
    {$endif}
    end;
     
    initialization
     
     vConfigurations := TConfigurations.Create;
     
    finalization
     
     FreeAndNil(vConfigurations);
     
    end.

and here is an example code of how to use that unit to get a resource file from it's correct location: 
    
    
    bmp := TBitmap.Create
    try
      bmp.LoadFromFile(vConfigurations.ResourcesPath + 'MyBitmap.bmp');
    finally
      bmp.Free;
    end;

---

_Source: [https://wiki.freepascal.org/Cross-platform_resources_path](https://web.archive.org/web/20180329145736/https://wiki.freepascal.org/Cross-platform_resources_path)_
