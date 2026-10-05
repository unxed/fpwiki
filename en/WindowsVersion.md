# WindowsVersion

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[Deutsch (de)](</WindowsVersion/de> "WindowsVersion/de")** │  **English (en)** │  **[русский (ru)](<../ru/WindowsVersion.md> "WindowsVersion/ru")** │    
****

  
This article is about Windows programming. Obtaining information on the version of the running Windows instance is important for many purposes. 

## Contents

  * 1 Traditional approach
  * 2 Using version helpers
  * 3 Using predefined constants provided by the FPC compiler
  * 4 References



## Traditional approach

The function below is intended for older versions. It determines the version of the current Windows installation. Please note that the GetVersion function is deprecated in Windows 8.1 and newer versions. For a more universal approach using version helpers, as described below, is recommended. 
    
    
    uses
      Windows, SysUtils, ...;
      ...
    {
    Meaning of Windows version numbers:
    5.0 => Windows 2000
    5.1 => Windows XP
    5.2 => Windows XP64 or Windows 2003 Server
    6.0 => Windows Vista or Windows 2008 Server
    6.1 => Windows 7 or Windows 2008 Server R2
    6.2 => Windows 8 or Windows Server 2012
    6.3 => Windows 8.1 or Windows Server 2012 RS
    }
    
    function funGetWinVer: string;
    // Returns the Windows version number as a string
    begin
      Result := IntToStr(LOBYTE(LOWORD(GetVersion)));
      Result := Result + '.';
      Result := Result + IntToStr(HIBYTE(LOWORD(GetVersion)));
    end;
    

## Using version helpers

Alternatively, the [SysUtils](</index.php?title=SysUtils&action=edit&redlink=1> "SysUtils \(page does not exist\)") unit provides the Windows version information in variables: 
    
    
    program EchoWinVersion;
    uses sysutils;
    ...
    begin
      Writeln('Win32 Platform    : ', Win32Platform    );
      Writeln('Win32 Major Version: ', Win32MajorVersion);
      Writeln('Win32 Minor Version: ', Win32MinorVersion);
      Writeln('Win32 Build Number : ', Win32BuildNumber );
      Writeln('Win32 CSD Version  : ', Win32CSDVersion  );
      readln;
    end.
    

The output looks like this for Windows 7 Service Pack 1: 
    
    
    Win32 Platform    : 2
    Win32 Major Version: 6
    Win32 Minor Version: 1
    Win32 Build Number : 7601
    Win32 CSD Version  : Service Pack 1
    

The output looks like this for Windows 10 Pro Build 18362 (64 bit): 
    
    
    Win32 Platform    : 2
    Win32 Major Version: 6
    Win32 Minor Version: 2
    Win32 Build Number : 9200
    Win32 CSD Version  : 
    

which is obviously wrong :( 

## Using predefined constants provided by the FPC compiler

However, the following works for identifying Windows 10 (and before): 
    
    
    ...
    Uses ..., Win32Proc;
    ...
    
    Procedure GetWinVer;
    Begin
    if WindowsVersion = wv95 then Memo.Lines.Add('Windows 95')
      else if WindowsVersion = wvNT4 then Memo.Lines.Add('Windows NT v.4')
      else if WindowsVersion = wv98 then Memo.Lines.Add('Windows 98')
      else if WindowsVersion = wvMe then Memo.Lines.Add('Windows ME')
      else if WindowsVersion = wv2000 then Memo.Lines.Add('Windows 2000')
      else if WindowsVersion = wvXP then Memo.Lines.Add('Windows XP')
      else if WindowsVersion = wvServer2003 then Memo.Lines.Add('Windows Server 2003/Windows XP64')
      else if WindowsVersion = wvVista then Memo.Lines.Add('Windows Vista')
      else if WindowsVersion = wv7 then Memo.Lines.Add('Windows 7')
      else if WindowsVersion = wv10 then Memo.Lines.Add('Windows 10')
      else Memo.Lines.Add('Windows Unknown Version!');         
    End.
    

For Windows 10 64 bit this outputs "Windows 10". 

## References

  1. Windows Developer: [Operating system version changes in Windows 8.1 and Windows Server 2012 R2](<https://docs.microsoft.com/en-us/windows/win32/w8cookbook/operating-system-version-changes-in-windows-8-1?redirectedfrom=MSDN>). 05/31/2018

---

_Source: [https://wiki.freepascal.org/WindowsVersion](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/WindowsVersion)_
