# FreeBSD Programming Tips

│ **English (en)** │

## Contents

  * 1 Commonly used Unix commands
  * 2 File system layout
  * 3 Accurate timer
  * 4 Configuration files
  * 5 Accessing System Information
  * 6 Free Pascal Resource Compiler
  * 7 Hardware Access
  * 8 Making a Beep
  * 9 Networking
  * 10 Using HTTPS
  * 11 Other Interfaces
    * 11.1 Platform specific Tips
    * 11.2 Interfaces Development Articles
  * 12 See also



## Commonly used Unix commands

If you’re coming to FreeBSD from Windows, you may find some of its Unix terminal commands confusing. Here are some equivalents. For more information about a command, enter man _command_ in a console or xterm. 

Action | Windows command prompt window | FreeBSD console or X11 xterm   
---|---|---  
Change to a different directory | cd | cd   
Make a new directory | mkdir | mkdir   
Delete a directory | rmdir | rmdir   
List file directory in chronological order with detail | dir /od | ls -ltr   
Copy a file, preserving its date-time stamp | copy | cp -p   
Display contents of a text file | type | cat   
Delete a file | erase | rm   
Move a file | move | mv   
Rename a file | ren | mv   
Find a file | dir /s | find   
Grep a file | findstr | grep   
Display differences between two text files | fc | diff   
Change file attributes | attrib | chmod   
“Super-user” root authorization | N/A | sudo   
Create symbolic link to a file or directory | mklink | ln   
Shrink executable file size | strip (included w/ Free Pascal) | strip   
Download source from subversion repo | TortoiseSVN | svnlite   
  
## File system layout

In a console or X11 xterm, type _man hier_ to display a detailed description of the FreeBSD file system hierarchy. Note that the FreeBSD file system hierarchy is different to other Unixes (eg Solaris) and Unix-like operating systems (eg Linux). 

## Accurate timer

[EpikTimer](<EpikTimer.md> "EpikTimer") is a programmer's stopwatch that is capable of measuring very short events with traceable high precision over long periods of time. It's simple to use, consumes virtually no CPU and requires only 25 bytes of ram to implement a timer instance. The component provides a single internal timer... but unlimited numbers can be declared externally and linked to a single EpikTimer component on the form. 

## Configuration files

Refer to the Multiplatform Programming Guide [Configuration files](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide") section. 

## Accessing System Information

[Sysctl](<https://www.freebsd.org/cgi/man.cgi?query=sysctl&sektion=3>) provides an interface that allows you to retrieve many kernel parameters in FreeBSD (and macOS and the BSDs). This provides a wealth of information detailing system hardware which can be useful when debugging user issues or simply optimising your application (eg to take advantage of threads on multi-core CPU systems). For full details and example code, see the article [Accessing FreeBSD System Information](<Accessing_FreeBSD_System_Information.md> "Accessing FreeBSD System Information"). 

## Free Pascal Resource Compiler

Starting with Free Pascal v2.4 you can use standard .rc (resource script) and .res (compiled resource) files in your project to include resources. To turn a .rc script used in the sources into a binary resource (.res file), Free Pascal runs the appropriate external resource compiler (windres or GoRC). Therefore that resource compiler needs to be installed and present in the PATH environment variable. 

For details of how to install the _windres_ resource compiler on FreeBSD, see [Lazarus Resources - FreeBSD](<Lazarus_Resources.md> "Lazarus Resources")

For more details on the resource compiler, see: [_Free Pascal Programmer's guide_ , chapter 13 "Using Windows resources"](<https://www.freepascal.org/docs-html/prog/progch13.html>). 

## Hardware Access

For details on accessing hardware, see [General UNIX Hardware Access](<Hardware_Access.md> "Hardware Access")

## Making a Beep

As `SysUtils.Beep` has no effect on FreeBSD, I looked around for a way to make a beep and found nothing useful. So, I reinvented the wheel as follows: 
    
    
        {$IFDEF FREEBSD}
        procedure FBSDbeep;
        var
          BProcess: TProcess;
          begin
            BProcess:= TProcess.Create(nil);
            BProcess.CommandLine := 'sh -c "echo xxxxxxx > /dev/dsp"';
            BProcess.Execute;
            BProcess.Free;
          end;
        {$ENDIF}
    

Note: _/dev/dsp_ will automatically reroute to the sound device's correct device node (eg _/dev/dsp0.0_) using the dynamic [devfs(5)](<https://www.freebsd.org/cgi/man.cgi?query=devfs&sektion=5>) clone handler. 

**Qualifications**

  * Ok, it's more of a burp than a beep, but it serves the purpose. If you're really fussy, you could send a real .wav file to _/dev/dsp_ and play any musical note(s) you desire.


  * If FreeBSD is using a GENERIC kernel, it will work.


  * If FreeBSD is using a custom kernel and it's a desktop system, then it will most probably work because who doesn't want sound on their desktop system.


  * If FreeBSD is using a custom kernel and is running on a server, it may not work. I don't see this as a significant defect in user applications which is its use case for my purposes.



## Networking

See this article: [Networking](<Networking.md> "Networking"). 

## Using HTTPS

Placeholder. Content to come. 

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
  * FreeBSD Programming Tips \- FreeBSD programming tips
  * [Linux Programming Tips](<Linux_Programming_Tips.md> "Linux Programming Tips") \- How to execute particular programming tasks in Linux
  * [macOS Programming Tips](<macOS_Programming_Tips.md> "macOS Programming Tips") \- Lazarus tips, useful tools, Unix commands, and more...
  * [WinCE Programming Tips](<WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Using the telephone API, sending SMSes, and more...
  * [Windows Programming Tips](<Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips



### Interfaces Development Articles

  * [Carbon interface internals](<Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset



## See also

  * [FreeBSD supported versions](<FreeBSD.md> "FreeBSD")
  * [Installing Free Pascal on FreeBSD](<Installing_Lazarus.md> "Installing Lazarus")
  * [Installing Lazarus on FreeBSD](<Installing_Lazarus_on_FreeBSD.md> "Installing Lazarus on FreeBSD")
  * [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

---

_Source: [https://wiki.freepascal.org/FreeBSD_Programming_Tips](https://web.archive.org/web/20250318093952/https://wiki.freepascal.org/FreeBSD_Programming_Tips)_
