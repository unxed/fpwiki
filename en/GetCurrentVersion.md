# GetCurrentVersion

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[Deutsch (de)](</GetCurrentVersion/de> "GetCurrentVersion/de")** │  **English (en)** │    
****

Return to the [Code Examples page](<Code_Examples.md> "Code Examples"). 

This article deals with Windows programming. 

The function determines the current version of your own program. 
    
    
      uses
       Windows, SysUtils, ...;
    
       ...
    
     function funGetCurrentVersion: string;
     var
       lwdVerInfoSize: longword;
       lwdVerValueSize: longword;
       lwdDummy: longword;
       ptrVerInfo: pointer;
       VersionsInformation: PVSFixedFileInfo;
    
     begin
    
       lwdVerInfoSize: = GetFileVersionInfoSize(PChar(ParamStr(0)), lwdDummy);
       GetMem(ptrVerInfo, lwdVerInfoSize);
       GetFileVersionInfo(PChar(ParamStr(0)), 0, lwdVerInfoSize, ptrVerInfo);
       VerQueryValue (ptrVerInfo, '\', Pointer(VersionsInformation), lwdVerValueSize);
    
       with VersionsInformation^ do
       begin
         Result: = IntToStr (dwFileVersionMS shr 16);
         Result: = Result + '.'  + IntToStr(dwFileVersionMS and $ FFFF);
         Result: = Result + '.'  + IntToStr(dwFileVersionLS shr 16);
         Result: = Result + '.'  + IntToStr(dwFileVersionLS and $ FFFF);
       end;
    
       FreeMem(ptrVerInfo, lwdVerInfoSize);
    
     end;
    
     ...

---

_Source: [https://wiki.freepascal.org/GetCurrentVersion](https://web.archive.org/web/20240914011350/https://wiki.freepascal.org/GetCurrentVersion)_
