# Custom Drawn Interface

│ **English (en)** │  **[русский (ru)](<../ru/Custom_Drawn_Interface.md> "Custom Drawn Interface/ru")** │    
****

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** THIS INFORMATION IS EXTREMELY DATED AND NEEDS UPDATING as of 2015

## Contents

  * 1 Other Interfaces
    * 1.1 Platform specific Tips
    * 1.2 Interface Development Articles
  * 2 Introduction
  * 3 FAQ
    * 3.1 Diagram of the Custom Drawn Interface
  * 4 The LCL-CustomDrawn Backends
    * 4.1 LCL-CustomDrawn-Android
    * 4.2 LCL-CustomDrawn-X11
    * 4.3 LCL-CustomDrawn-Cocoa
    * 4.4 LCL-CustomDrawn-Windows
    * 4.5 LCL-CustomDrawn-iPhone
  * 5 Canvas
    * 5.1 Getting the TLazCanvas object inside TCanvas
    * 5.2 Drawing optimization roadmap
      * 5.2.1 Optimizations already done
      * 5.2.2 Optimizations to be done
  * 6 Font rendering
  * 7 Message and Common Dialogs
  * 8 Windowed visual controls
  * 9 Conditional defines accepted by LCL-CustomDrawn
  * 10 Screenshots
  * 11 See Also



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
  * Custom Drawn Interface \- A cross-platform LCL backend written completely in Object Pascal inside Lazarus. The Lazarus interface to Android.



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



## Introduction

LCL-CustomDrawn-Android has the following features: 

  * Backends for X11, Android and Windows and a partially working one for Cocoa, more can be added in the future
  * Painting is done completely inside Lazarus without any interference from the native libraries except for text drawing. This assures a complete perfection of the executed drawings in all platforms and a uniform level of supported features
  * Only 1 native window is utilized for each form, in the Android backend at the moment this is 1 native window for the entire application
  * Utilizes the Lazarus Custom Drawn Controls for implementing the LCL standard controls
  * Utilizes as it's painting engine the lcl parts: TLazIntfImage, TRawImage, lazcanvas and lazregions



## FAQ

### Diagram of the Custom Drawn Interface

[![customdrawn diagram.png](https://wiki.freepascal.org/images/1/10/customdrawn_diagram.png)](</File:customdrawn_diagram.png>)

Legend: 

  * Blue - New elements implemented as part of the LCL-CustomDrawn project
  * Yellow - Pre-existing LCL elements
  * Gray - System APIs



## The LCL-CustomDrawn Backends

LCL-CustomDrawn needs backends to implement the most basic parts of the widgetset. Each backend should implement the following minimal parts: 

  * TWidgetSet.Run, ProcessMessages, etc
  * TForm
  * TWinControl with all events
  * TTrayIcon



### LCL-CustomDrawn-Android

This backend is working. See this page [Custom Drawn Interface/Android](<Custom_Drawn_Interface/Android.md> "Custom Drawn Interface/Android")

And also [Android Programming](<Android_Programming.md> "Android Programming")

### LCL-CustomDrawn-X11

This backend is working. See [Custom Drawn Interface/X11](<Custom_Drawn_Interface/X11.md> "Custom Drawn Interface/X11")

### LCL-CustomDrawn-Cocoa

This backend is working. 

### LCL-CustomDrawn-Windows

This backend is working. 

### LCL-CustomDrawn-iPhone

This is planned, but not yet started. 

## Canvas

TCanvas will be fully non-native in this widgetset and based on [LazCanvas](<Developing_with_Graphics.md> "Developing with Graphics"). All drawings to visual controls and on the OnPaint event of controls of the form are naturally double-buffered because the entire drawing of the form is first performed on an off-screen buffer and then copied in 1 operation to the form native canvas. In reality there is only 1 TLazCanvas for the entire form, but sub-controls will think they are on a separate canvas because the property BaseWindowOrg sets an internal start position of the canvas and also because all drawings will be clipped to reflect the size and shape of the sub-control. 

### Getting the TLazCanvas object inside TCanvas

This works only in the LCL-CustomDrawn interface: 
    
    
    uses lazcanvas, lclintf;
    
    var
      MyLazCanvas: TLazCanvas:
    begin
      if nctLazCanvas in LCLIntf.GetAvailableNativeCanvasTypes(MyCanvas.Handle) then
      begin
        MyLazCanvas := TLazCanvas(LCLIntf.GetNativeCanvas(MyCanvas.Handle, nctLazCanvas));
        // do something here with TLazCanvas
      end;
    

### Drawing optimization roadmap

#### Optimizations already done

  * Give 1 bitmap to each control and buffer the control images and only redraw them if invalidated. This greatly helps in large forms when invalidating only 1 control.
  * Don't draw controls which are completely covered by other ones. Commits: 
    * [http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=36437](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=36437>)
    * [http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=36455](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=36455>)
  * Change the control bitmaps to use the native format (with ifdefs for ARGB32 for alpha blending support). This was implemented together with the next optimization:
  * Optimize the case of drawing a bitmap to a canvas, when the pixel formats match and the clip rect is inexistent or rectangular. The magnifier full painting went from 630ms to 477ms for me. 
    * [http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=36576](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=36576>)
    * [http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=36578](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=36578>)
  * Optimize rectangle area filling if the clip rect is inexistent or rectangular. Decreased the drawing of a fullscreen form in X11 with 4 buttons from 95ms to 33ms for me 
    * [http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=36580](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=36580>)



#### Optimizations to be done

  * Buffer the native bitmap of the form instead of generating it on each operation
  * Reuse the buffered native bitmap contents if they don't change
  * Support invalidating only a rectangle instead of the entire control



## Font rendering

When the define CD_UseNativeText is activated, LCL-CustomDrawn will use the native text rendering of the platform as provided by the backend. If it isn't activated, then it will use [LazFreeType](<LazFreeType.md> "LazFreeType") to render the text. The define is automatically activated for some backends if convenient when using them. 

## Message and Common Dialogs

Message and Common Dialogs might be native if this is considered very convenient for the backend. If not, they will be non-native. At the moment the Android backend has native message boxes. 

## Windowed visual controls

All Windowed visual controls (TButton, TPageControl, etc) will be based in the [Lazarus Custom Drawn Controls](<Lazarus_Custom_Drawn_Controls.md> "Lazarus Custom Drawn Controls")

## Conditional defines accepted by LCL-CustomDrawn

One easy way to set a conditional define for the LCL-CustomDrawn is to define it in the file lcl/interfaces/customdrawn/customdrawndefines.inc 

Here are defines which affect the functionality offered by this interface: 

  * CD_UseNativeText - Activates using native text instead of PasFreeType. This define will be automatically set if convenient for a particular backend, don't set it manually unless you know what you are doing.



And here debug information defines: 

  * VerboseCDPaintProfiler - Adds profiling information to indicate how fast the paint event is processed
  * VerboseCDWinAPI - Extended verbose information for LCLIntf calls, except those which are covered by one of these defines instead: 
    * VerboseCDText - Verbose info for text winapi calls
    * VerboseCDDrawing - Verbose info for Canvas and drawing operations
    * VerboseCDBitmap - Verbose info for Bitmap and rawimage creation and handling
  * VerboseCDForms - Extended verbose information for TWSCustomForm methods and about the non-native form from customdrawnproc (when utilized)
  * VerboseCDEvents - Extended verbose information for native events (for example mouse click, key input, etc). This excludes the paint event
  * VerboseCDPaintEvent - Debug info for the paint event
  * VerboseCDApplication - Verbose info for App routines from the Widgetset object
  * VerboseCDMessages - Verbose info for messages from the operating system (Paint, keyboard, mouse)



## Screenshots

[![Lazclock customdrawn.png](https://wiki.freepascal.org/images/0/05/Lazclock_customdrawn.png)](</File:Lazclock_customdrawn.png>)

[![lcl android 30 mar.png](https://wiki.freepascal.org/images/3/3e/lcl_android_30_mar.png)](</File:lcl_android_30_mar.png>) [![vpr-android.png](https://wiki.freepascal.org/images/4/43/vpr-android.png)](</File:vpr-android.png>)

## See Also

  * [Lazarus Custom Drawn Controls](<Lazarus_Custom_Drawn_Controls.md> "Lazarus Custom Drawn Controls")

---

_Source: [https://wiki.freepascal.org/Custom_Drawn_Interface](https://web.archive.org/web/20240920204109/https://wiki.freepascal.org/Custom_Drawn_Interface)_
