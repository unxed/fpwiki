# Cocoa Interface

│ **English (en)** │    
****

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Cocoa bindings
  * 2 Compiling
    * 2.1 FPC 3.2.0 for macOS 10.10 and earlier
    * 2.2 Prepare your Lazarus project for using Cocoa
    * 2.3 Error ATSUFindFontFromName
    * 2.4 Cocoa Debugging
  * 3 Cocoa FAQ
    * 3.1 DPI and scaling issues
    * 3.2 TButton looks too small!
    * 3.3 Overlapping Widgets
  * 4 Roadmap
  * 5 See also
  * 6 External links
  * 7 Other Interfaces
    * 7.1 Platform specific Tips
    * 7.2 Interface Development Articles



## Cocoa bindings

The Cocoa interface uses the native support in Free Pascal for direct communication with Objective-C which was added through the [Objective Pascal](<FPC_PasCocoa.md> "FPC PasCocoa") dialect. 

## Compiling

For macOS 10.14 Mojave and later you should be installing FPC 3.2.0 available from the official [Lazarus IDE file area](<https://sourceforge.net/projects/lazarus/files/>). 

### FPC 3.2.0 for macOS 10.10 and earlier

The precompiled CocoaAll header comes with an instruction to link the CoreImage framework. The framework was introduced in macOS 10.11 and later. (The CoreImage APIs were available since 10.4, BUT there was a stand-alone CoreImage framework. They were all part of QuartzCore framework). Compiling any unit using CocoaAll will likely cause the linking stage to fail on macOS 10.10 and earlier, as the framework doesn't exist. 

In order to fix that one might want to update headers and recompile them from the sources. 

1\. Get the FPC sources (you might have them already installed with Lazarus, as it's a prerequisite for the IDE). 

2\. Go to %sourcesdir%/packages/cocoaint/src 

3\. Edit CocoaAll.pas and comment out or remove The CoreImage linking line: 
    
    
     unit CocoaAll;
     interface
     
     {$linkframework Cocoa}
     {$linkframework Foundation}
     //{$linkframework CoreImage} comment out this line
     {$linkframework QuartzCore}
     {$linkframework CoreData}
     {$linkframework AppKit}
    

4\. Headers now needs to be recompiled. 

5\. Go back to the root directory of the sources. 

6\. Compile the RTL packages. 
    
    
    make rtl
    

This should compile a 64-bit version of the RTL. The RTL is needed to compile packages in the next step. 

7\. Compile the packages: 
    
    
    make packages
    

This should compile A 64-bit version of the FPC packages. 

8\. Go to %sourcesdir%/packages/cocoaint 

9\. Run the command to install the package (to the default system location). Do not forget to add OPT=" if you're on 10.7 or earlier: 
    
    
    sudo make install
    

### Prepare your Lazarus project for using Cocoa

You may need to set the Target to the 64bit processor and select the Cocoa Widget set: 

  * Open your project with Lazarus and click Project/Project Options
  * In the "Config and Target" panel set the "Target CPU family" to be "x86_64"
  * In the "Additions and Overrides" panel click on "Set LCLWidgetType" pulldown and set the value to "Cocoa"
  * In the past, for some reason Lazarus kept setting the compiler to "/usr/local/bin/ppc386" - which results in 32 bit apps. Make sure under Tools->Options that "Compiler Executable" is set to "/usr/local/bin/fpc" to get 64 bit apps.
  * Now compile your project - with any luck it will work OK.



### Error ATSUFindFontFromName

If you're getting the error: 
    
    
    carbonproc.pp(563,13) Error: Identifier not found "ATSUFindFontFromName"
    

when compiling a project for macOS using FPC, you either: 

  * set the CPU target explicitly to i386. (FPC 3.2.0 compiles to x86_64 for the Darwin target by default. This is done due Apple having dropped support for 32-bit target in macOS 10.15 Catalina released in October 2019.)



    

  * by setting the target in Project options (switching it from default to i386)
  * by setting the _CPU_TARGET=i386_ parameter for the make command if compiling from the command line.



  * or set the LCL target (widgetset) to "Cocoa".



### Cocoa Debugging

See the [Cocoa Debugging](<Cocoa_Debugging.md> "Cocoa Debugging") article. 

## Cocoa FAQ

### DPI and scaling issues

See the [Cocoa DPI](<Cocoa_DPI.md> "Cocoa DPI") article. 

### TButton looks too small!

If you design a button in another widgetset with Autosize=False it might happen that the button looks too small in Cocoa, and a number of people have complained about this, such as in [this bug report](<https://bugs.freepascal.org/view.php?id=31185>). 

If you don't care about the button size, just set AutoSize=True. If you want to have a custom width for the button, but want to allow the LCL to choose the right Height so that the button will look good in Cocoa, then the solution in this case is to set the following properties in the Object Inspector: 

  * AutoSize=True
  * Constrains.MinWidth = Constrains.MaxWidth = your desired width.



### Overlapping Widgets

Lazarus allows you to set the depth of different widgets, such that when two widgets overlap, the "closer" object blocks the view of the more "distant" object. You can do this at design time (right-click on object and click "Z-order") or at run time with functions like "BringToFront" and "SendToBack". Be aware that this may not always work with Cocoa. This is a 'feature' of Cocoa, as clipping is optimized for performance. Therefore, if you plan to compile your projects for Cocoa it is a good strategy to avoid overlapping widgets or to place them on different panels to provide explicit control of Z-order. For more details [see this bug report](<https://bugs.freepascal.org/view.php?id=32991>). 

## Roadmap

[Roadmap: Status of Features in each Widget set](<Roadmap.md> "Roadmap")

## See also

  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals")



## External links

  * [Apple: Cocoa Event Handling](<https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/EventOverview/Introduction/Introduction.html>)
  * [Apple: Button Programming Topics](<https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Button/Button.html>)
  * [Apple: Cocoa Bindings Programming Topics](<https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html>)
  * [Apple: Cocoa Text Architecture Guide](<https://developer.apple.com/library/archive/documentation/TextFonts/Conceptual/CocoaTextArchitecture/Introduction/Introduction.html>)



## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<Carbon_Interface.md> "Carbon Interface") \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
  * Cocoa Interface \- The Cocoa 64 bit interface for macOS
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
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset

---

_Source: [https://wiki.freepascal.org/Cocoa_Interface](https://web.archive.org/web/20241213023749/https://wiki.freepascal.org/Cocoa_Interface)_
