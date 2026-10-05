# Detect Windows x32-x64 example

│ **English (en)** │  **[français (fr)](</Detect_Windows_x32-x64_example/fr> "Detect Windows x32-x64 example/fr")** │    
****

Sometimes you want to know the OS bitness. For Windows, works for me on Vista x64 with FPC x86 compiler - thanks to [German Lazarus forum](<http://www.lazarusforum.de/viewtopic.php?f=55&t=5287%7Cthe>): 
    
    
    program bitness;
    
    {$mode objfpc}{$H+}
    {$APPTYPE CONSOLE}
    uses {$IFDEF UNIX} {$IFDEF UseCThreads}
      cthreads, {$ENDIF} {$ENDIF}
      Classes,
      SysUtils,
      Windows;
    
      function IsWindows64: boolean;
      {
      Detect if we are running on 64 bit Windows or 32 bit Windows,
      independently of bitness of this program.
      Original source:
      http://www.delphipraxis.net/118485-ermitteln-ob-32-bit-oder-64-bit-betriebssystem.html
      modified for FreePascal in German Lazarus forum:
      http://www.lazarusforum.de/viewtopic.php?f=55&t=5287
      }
    {$ifdef WIN32} //Modified KpjComp for 64bit compile mode
      type
        TIsWow64Process = function( // Type of IsWow64Process API fn
            Handle: Windows.THandle; var Res: Windows.BOOL): Windows.BOOL; stdcall;
      var
        IsWow64Result: Windows.BOOL; // Result from IsWow64Process
        IsWow64Process: TIsWow64Process; // IsWow64Process fn reference
      begin
        // Try to load required function from kernel32
        IsWow64Process := TIsWow64Process(Windows.GetProcAddress(
          Windows.GetModuleHandle('kernel32'), 'IsWow64Process'));
        if Assigned(IsWow64Process) then
        begin
          // Function is implemented: call it
          if not IsWow64Process(Windows.GetCurrentProcess, IsWow64Result) then
            raise SysUtils.Exception.Create('IsWindows64: bad process handle');
          // Return result of function
          Result := IsWow64Result;
        end
        else
          // Function not implemented: can't be running on Wow64
          Result := False;
    {$else} //if were running 64bit code, OS must be 64bit :)
      begin
       Result := True;
    {$endif}
      end;
    
    begin
      try
        if IsWindows64 then
        begin
          writeln('This operating system is 64 bit.');
        end
        else
        begin
          writeln('This operating system is 32 bit.');
        end;
      except
        on E: exception do
        begin
          writeln('Could not determine bitness of operating system. Error details: ' + E.ClassName+'/'+E.Message);
        end;
      end;
    end.

---

_Source: [https://wiki.freepascal.org/Detect_Windows_x32-x64_example](https://web.archive.org/web/20250325082354/https://wiki.freepascal.org/Detect_Windows_x32-x64_example)_
