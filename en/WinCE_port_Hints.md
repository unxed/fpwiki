# WinCE port Hints

[![WinCE Logo.png](https://wiki.freepascal.org/images/8/86/WinCE_Logo.png)](</File:WinCE_Logo.png>)

This article applies to [Windows CE](</Category:WinCE> "Category:WinCE") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

This page collects platform specific programming hints for [arm-wince](<arm-wince.md> "arm-wince") and [i386-wince](<i386-wince.md> "i386-wince") Targets. (Where a hint is specific to a CPU, this is noted) 

### WinCE port notes

  * If you are getting "Uknown error! Error code : 0xc1" and using dlls then the error might be becouse of using some export entries not found in dll file.Check the dll file export section and name of them and how you used them.
  * `**chdir**` procedure always produces an error (WinCE does not support setting of current directory).
  * All file/dir paths must be absolute (started with \\).
  * WinCE is unicode OS. All string parameters to API calls must be PWideChar.
  * WinCE does not have support for environment strings.
  * WinCE does not have support for console applications by default. But you can install console support by yourself. Please note that FPC creates GUI applications for WinCE target by default. To create console application you should use `-WC` compiler switch or put `{$APPTYPE CONSOLE}` directive to source code.  
To enable console in WinCE install one of the following programs: 
    * **PocketCMD** by SymbolicTools. It is recommended solution.  
Get it [here](<http://www.symbolictools.de/public/pocketconsole/applications/PocketCMD>)  
Seems to be ARM/PPC-only
    * **PPC Command Shell** from Microsoft Windows Mobile Developer Power Toys. Get it [here](<http://www.microsoft.com/downloads/details.aspx?FamilyID=74473fd6-1dcc-47aa-ab28-6a2b006edfe9&displaylang=en>)  
PPC Command Shell have less features than PocketCMD. Also it have some issues. One of them - a new console window is opened even if an application is started from a console command prompt.
    * To enable console output in Windows Mobile 5 and 6 set "HKEY_LOCAL_MACHINE\Drivers\Console\OutputTo" to 0.



### Project settings

<to be extended>

---

_Source: [https://wiki.freepascal.org/WinCE_port_Hints](https://web.archive.org/web/20240920213339/https://wiki.freepascal.org/WinCE_port_Hints)_
