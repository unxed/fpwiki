# Windows version

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

  
This article is about Windows programming. Obtaining information on the version of the running Windows instance is important for many purposes. 

## Contents

  * 1 Using Windows unit
  * 2 Using SysUtils variables
  * 3 Using Win32Proc Lazarus unit
  * 4 References



## Using Windows unit

The function below is intended for older versions. It determines the version of the current Windows installation. Please note that the GetVersion function is deprecated in Windows 8.1 and newer versions. More universal approach is using SysUtils unit variables, as shown below. 
    
    
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
    
    function GetWinVersion: string;
    begin
      Result := 
        IntToStr(LOBYTE(LOWORD(GetVersion))) + '.' +
        IntToStr(HIBYTE(LOWORD(GetVersion)));
    end;
    

## Using SysUtils variables

The SysUtils unit provides the Windows version information in several variables: 
    
    
    uses SysUtils;
    begin
      Writeln('Win32 Platform    : ', Win32Platform    );
      Writeln('Win32 Major Version: ', Win32MajorVersion);
      Writeln('Win32 Minor Version: ', Win32MinorVersion);
      Writeln('Win32 Build Number : ', Win32BuildNumber );
      Writeln('Win32 CSD Version  : ', Win32CSDVersion  );
      Readln;
    end.
    

The output looks like this for Windows 7 Service Pack 1: 
    
    
    Win32 Platform    : 2
    Win32 Major Version: 6
    Win32 Minor Version: 1
    Win32 Build Number : 7601
    Win32 CSD Version  : Service Pack 1
    

The output looks like this for Windows 10 Pro Build 18362 (64 bit): 
    
    
    Win32 Platform    : 2
    Win32 Major Version: 6
    Win32 Minor Version: 2
    Win32 Build Number : 9200
    Win32 CSD Version  : 
    

## Using Win32Proc Lazarus unit

The following works for identifying Windows 11 and all older versions: 
    
    
    uses ..., Win32Proc;
    
    function GetWinVersion: string;
    begin
      case WindowsVersion of
        wv95: Result:= 'Windows 95';
        wvNT4: Result:= 'Windows NT v.4';
        //etc. 
        //see possible values in the unit "win32proc" in "lcl/interfaces/win32/win32proc.pp"
      end;
    end.
    

## References

  1. Windows Developer: [Operating system version changes in Windows 8.1 and Windows Server 2012 R2](<https://docs.microsoft.com/en-us/windows/win32/w8cookbook/operating-system-version-changes-in-windows-8-1?redirectedfrom=MSDN>). 05/31/2018

---

_Source: [https://wiki.freepascal.org/Windows_version](https://web.archive.org/web/20241201000000/https://wiki.freepascal.org/Windows_version)_
