# fpGUI Interface

│ **[Deutsch (de)](</fpGUI_Interface/de> "fpGUI Interface/de")** │  **English (en)** │  **[español (es)](</fpGUI_Interface/es> "fpGUI Interface/es")** │  **[français (fr)](</fpGUI_Interface/fr> "fpGUI Interface/fr")** │    
****

## Contents

  * 1 Introduction
  * 2 Other Interfaces
    * 2.1 Platform specific Tips
    * 2.2 Interface Development Articles
  * 3 Creating your first LCL application with fpGUI
  * 4 Implementation details
    * 4.1 Component mapping
  * 5 Road map for the fpGUI interface
  * 6 See also



## Introduction

[fpGUI](<fpGUI.md> "fpGUI") is a widgetset completely written in Object Pascal. It links directly with the underlying windowing system, and thus avoids the need for many external libraries. More information on its website: <http://fpgui.sourceforge.net>

This page covers the fpGUI widgetset - a Lazarus LCL widget wrapping the fpGUI code. It is different from [pure fpGUI](<fpGUI.md> "fpGUI") code. 

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
  * fpGUI Interface \- Based on the fpGUI library, which is a cross-platform toolkit completely written in Object Pascal
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
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset



## Creating your first LCL application with fpGUI

This is a step-by-step guide to build your first application with the fpGUI widgetset 

  1. Download the latest Lazarus from subversion and build it (not necessary if you downloaded a pre-compiled snapshot)
  2. Download fpGUI source code from the SourceForge.net Git repository as follows: 

    
         
         git clone git://git.code.sf.net/p/fpgui/code

  3. Make sure you switch the the **maint** branch of fpGUI. 

    
         
         git checkout maint

  4. Now we need to make the Lazarus LCL find the fpGUI code. 
     1. **Using Lazarus (trunk)**
        * Option 1) 

    Copy the 'src' directory from the fpGUI sources into your Lazarus directory as follows:  
  

    <fpgui>/src/ ⇒ <lazarus>/lcl/interfaces/fpgui/src/
      

        * Option 2) 

    Use your file system's symbolic linking function to create a link the fpGUI **src** directory, to the correct location in Lazarus.
    For example:  
  

    cd /opt/lazarus/lcl/interfaces/fpgui
    ln -s /path/to/fpgui/src src
      

     2. **Using Lazarus 1.0.8 and earlier**
        * Option 1)

    Supported on all platforms. Copy two directories and one file from the downloaded **fpgui** to **lazarus**  
  

    <fpGUI dir>/src/gui ⇒ lazarus/lcl/interfaces/fpgui/gui
    <fpGUI dir>/src/corelib ⇒ lazarus/lcl/interfaces/fpgui/corelib
    <fpGUI dir>/src/VERSION_FILE.inc ⇒ lazarus/lcl/interfaces/fpgui/VERSION_FILE.inc
      

        * Option 2)

    Linux or any other Unix type platforms. Create symbolic links from the fpGUI's **corelib** , **gui** directories and VERSION_FILE.inc to the Lazarus LCL directory as follows:  
  

    ln -s <fpGUI dir>/src/gui lazarus/lcl/interfaces/fpgui/gui
    ln -s <fpGUI dir>/src/corelib lazarus/lcl/interfaces/fpgui/corelib
    ln -s <fpGUI dir>/src/VERSION_FILE.inc lazarus/lcl/interfaces/fpgui/VERSION_FILE.inc
      

  5. Now you need to recompile LCL for the LCL-fpGUI widgetset 
     1. **Using Lazarus (trunk)**
        1. Start a new LCL application
        2. Tell it to use the LCL-fpGUI widgeset by using the **Additions and Overrides** dialog. This can be found by selecting "Project Options -> Compiler Options -> Additions and Overrides". You can use the drop-down button menus in that dialog to define the following IDE Macros, or type them in by hand. For the **fpGUIPlatform** macro, only define one, dependent on the platform you are compiling for. 
           * define IDE Macro: **LCLWidgetType := fpgui**
           * define IDE Macro: **fpGUIPlatform := gdi**(for Windows)
           * define IDE Macro: **fpGUIPlatform := x11**(for Linux/FreeBSD)
        3. Compile your project!
     2. **Using Lazarus 1.0.8 and earlier**
        1. Start a new LCL application
        2. Open Lazarus, open the menu "Tools -> Configure Build Lazarus", select all options to None and LCL to Build+Clean, select fpGUI widgetset and rebuild it.
        3. Go to the menu Compiler Options, select **fpGUI** as the widgetset
        4. Compile your project!



## Implementation details

### Component mapping

This is a table indication which FPGUI controls are used to build each LCL control. 

**NOTE:** This table is incomplete and out of date. Many other controls have already been implemented in LCL-fpGUI, but not listed in this table. 

  


LCL Control | FPGUI Control | Comments   
---|---|---  
TPageControl | TfpgPageControl | \-   
TButton | TfpgButton | \-   
TBitBtn | TfpgButton | fpGUI's standard button class supports images.   
TSpeedButton | TfpgButton | fpGUI's standard button class supports images and flat look etc.   
  
## Road map for the fpGUI interface

Here: [Roadmap#Widgetset dependent components](<Roadmap.md> "Roadmap")

## See also

  * [fpGUI](<fpGUI.md> "fpGUI") Toolkit
  * [**Easy fpGUI**](<http://www.turbocontrol.com/easyfpgui.htm>) The easy way to try fpGUI and Free Pascal! Simply unpack the archive and you have a fully working FPC and fpGUI environment.

---

_Source: [https://wiki.freepascal.org/fpGUI_Interface](https://web.archive.org/web/20240909212652/https://wiki.freepascal.org/fpGUI_Interface)_
