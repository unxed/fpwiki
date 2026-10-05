# AppIsRunning

│ **English (en)** │

## Test whether an Application is already running

Here's a unit that works under both Windows and Linux 

  * There's no need to pass the full application path to the function - the ExeName will usually do. Below code cannot find out its own exename though.


    
    
    unit uappisrunning;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils
      {$IFDEF WINDOWS}, Windows, JwaTlHelp32{$ENDIF}
      {$IFDEF LINUX},process{$ENDIF};
    // JwaTlHelp32 is in fpc\packages\winunits-jedi\src\jwatlhelp32.pas
    
    // Returns TRUE if EXEName is running under Windows or Linux
    // Don't pass an .exe extension to Linux!
    function AppIsRunning(const ExeName: string):Boolean;
    
    implementation
    
    // These functions return Zero if app is NOT running
    // Override them if you have a better implementation
    
    {$IFDEF WINDOWS}
    function WindowsAppIsRunning(const ExeName: string): integer;
    var
      ContinueLoop: BOOL;
      FSnapshotHandle: THandle;
      FProcessEntry32: TProcessEntry32;
    
    begin
      FSnapshotHandle := CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0);
      FProcessEntry32.dwSize := SizeOf(FProcessEntry32);
      ContinueLoop := Process32First(FSnapshotHandle, FProcessEntry32);
      Result := 0;
    
      while integer(ContinueLoop) <> 0 do
        begin
        if ((UpperCase(ExtractFileName(FProcessEntry32.szExeFile)) =
          UpperCase(ExeName)) or (UpperCase(FProcessEntry32.szExeFile) =
          UpperCase(ExeName))) then
          begin
          Inc(Result);
          // SendMessage(Exit-Message) possible?
          end;
        ContinueLoop := Process32Next(FSnapshotHandle, FProcessEntry32);
        end;
    
      CloseHandle(FSnapshotHandle);
    end;
    {$ENDIF}
    
    {$IFDEF LINUX}
    function LinuxAppIsRunning(const ExeName: string): integer;
    var
      t: TProcess;
      s: TStringList;
    
    begin
      Result := 0;
      t := tprocess.Create(nil);
      t.CommandLine := 'ps -C ' + ExeName;
      t.Options := [poUsePipes, poWaitonexit];
    
        try
        t.Execute;
        s := TStringList.Create;
          try
          s.LoadFromStream(t.Output);
          Result := Pos(ExeName, s.Text);
          finally
          s.Free;
          end;
        finally
        t.Free;
        end;
    
    end;
    {$ENDIF}
    
    function AppIsRunning(const ExeName: string):Boolean;
    begin
    
    {$IFDEF WINDOWS}
      Result:=(WindowsAppIsRunning(ExeName) > 0);
    {$ENDIF}
    
    {$IFDEF LINUX}
      Result:=(LinuxAppIsRunning(ExeName) > 0);
    {$ENDIF}
    
    end;
    end.
    

## See also

  * [UniqueInstance](<UniqueInstance.md> "UniqueInstance")

---

_Source: [https://wiki.freepascal.org/AppIsRunning](https://web.archive.org/web/20241212115900/https://wiki.freepascal.org/AppIsRunning)_
