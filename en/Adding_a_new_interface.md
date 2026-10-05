# Adding a new interface

This article describes the steps to add a new widgetset interface. This article is intended for developers who want to make an existing widget set available for use with the LCL. It describes the first steps to set up the directories and adapt the IDE for developing the new interface. 

## Contents

  * 1 Other Interfaces
    * 1.1 Platform specific Tips
    * 1.2 Interface Development Articles
  * 2 Steps
    * 2.1 Register the new widgetset in the IDE
    * 2.2 Create a directory for the interface
    * 2.3 Update Makefiles
    * 2.4 Add widgetset directory to update_allunits
    * 2.5 Compiler options / search paths



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
  * [Windows Programming Tips](<Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips



### Interface Development Articles

  * [Carbon interface internals](<Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * Adding a new interface \- How to add a new widget set interface
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset



## Steps

As example we write the steps for adding a new interface for the _pogo_ widget set. 

### Register the new widgetset in the IDE

In the file lcl/interfacebase.pp add _lppogo_ to TLCLPlatform add 'pogo' to LCLPlatformDirNames. Add 'pogo' to ide/lazconf.pp (LCLPlatformDisplayNames). 

Then rebuild the IDE with the LCL and restart it. You should now see the new wigdetset in the 'Configure build lazarus' dialog. Now the codetools know about it and basic code functions will work. 

### Create a directory for the interface

Create a subdirectory pogo in lazarus\lcl\interface. 

Then copy the Makefile.fpc from one of the existing widgetsets and adapt it. For example change the _unittargetdir_. Then create the Makefile with [Fpcmake](<Fpcmake.md> "Fpcmake"): 

For *nix: 
    
    
     cd lazarus/lcl/interfaces/pogo
     export FPCDIR=/home/username/fpc_sources_trunk/
     fpcmake -Tall
     export FPCDIR=
    

For windows: 
    
    
     cd lazarus\lcl\interfaces\pogo
     set FPCDIR=lazarus\fpc\source
     lazarus\fpc\2.3.1\bin\i386-win32\fpcmake -Tall
     set FPCDIR=
    

You **must** use fpcmake from the latest and greatest and unstable FPC version, so that all FPC platforms are known in the Makefile. For the source it is less critical. The last line sets the variable FPCDIR to none. Otherwise other Makefiles will be auto regenerated too, which can accidently break things. So better make sure with the last line that FPCDIR is not set. 

Then copy the interfaces.pp from one of the other widgetsets and adapt it. For example rename the widgetset class to TPogoWidgetSet. 

Then create a first unit pogoint.pp for the new class TPogoWidgetSet. Every widgetset needs to override some abstract methods. Position the blinking cursor on class and use the refactoring tool Source Editor / Popup menu / Refactoring / Show abstract methods to automatically add the method stubs. Don't forget to use pogoint in the new interfaces.pp. 

### Update Makefiles

Edit lazarus\lcl\interfaces\Makefile.fpc and append _pogo_ to the Target directories 
    
    
    [target]
    dirs=gtk gtk2 win32 wince qt carbon fpgui pogo
    

Generate the Makefile in lazarus\lcl\interfaces using [Fpcmake](<Fpcmake.md> "Fpcmake"). 

### Add widgetset directory to update_allunits

Add a line in lcl/update_allunits.lpr for the _pogo_ widgetset. You will need to run this program as you implement the PogoWSxxxx units. I used 
    
    
     instantfpc ./update_allunits.lpr
    

Or you could run 
    
    
     lazbuild ./update_allunits.lpr
     ./update_all_units
    

You can now compile your new widgetset in the IDE, by setting the widgetset in the 'Configure build lazarus' dialog. 

  


### Compiler options / search paths

If the new widgetset has subdirectories or need some extra flags, then these must be added to the Makefile (e.g. lcl/interfaces/pogo/Makefile) **and** to the LCL.lpk. Otherwise: ask Mattias via mail.

---

_Source: [https://wiki.freepascal.org/Adding_a_new_interface](https://web.archive.org/web/20250317225935/https://wiki.freepascal.org/Adding_a_new_interface)_
