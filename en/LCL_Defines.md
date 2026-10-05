# LCL Defines

│ **English (en)** │  **[français (fr)](</LCL_Defines/fr> "LCL Defines/fr")** │    
****

This page documents the defines that can be utilized to recompile the different widgetsets of LCL. It isn´t a comprehensive list, but may become one as more defines are added. It also mentions defines that are automatically set when you select various options, such as widgetset, in Lazarus. See Also [Platform_defines](<Platform_defines.md> "Platform defines")

## Contents

  * 1 Why use defines?
  * 2 Recompiling LCL with a define
  * 3 General LCL defines
    * 3.1 LCL Version
    * 3.2 PLATFORM
    * 3.3 Widget Set
  * 4 Win32 defines
  * 5 WinCE defines
  * 6 Gtk defines
  * 7 Qt defines
    * 7.1 Legacy
  * 8 Carbon defines
  * 9 FPC Optimization Defines
  * 10 Other Interfaces
    * 10.1 Platform specific Tips
    * 10.2 Interface Development Articles
  * 11 See Also



## Why use defines?

Defines allow to add non-standard functionality to LCL, test experimental features or to add debug output that helps testing the widgetsets. 

The defines are also useful as 'read only' for you to determine, for example, to include particular code on particular platforms. For example - 
    
    
    {$ifdef WINDOWS}
        // do some WINDOWS specific stuff
    {$else}
        // do whatever but only on non-windows platforms
    {$endif}
    

## Recompiling LCL with a define

To recompile LCL with a special define go to the Lazarus menu Tools -> "Configure Build Lazarus". Add there in the Options field: 
    
    
    -d<DEFINE_NAME>
    

So if you wish to add the PassWin32MessagesToLCL define, you will add 
    
    
    -dPassWin32MessagesToLCL
    

to this box. 

## General LCL defines

  * **DEBUG_SHELLCTRLS** \- Turns on the use of WriteLn to output debug information for the ShellCtrls unit.



#### LCL Version

Sometimes its necessary to do things differently depending on the LCL (ie Lazarus) version its going to be compiled with. The LCL_FULLVERSION define can be read like this - 
    
    
    {$if (lcl_fullversion >= 2000600)}
    DoSomething(); // only do this for Lazarus 2.0.6 or later
    {$endif}
    

LCL thinks of its versions in that seven digit number: 

  * 2010000 is 2.1.0 or 2.01.00.00
  * 2000600 is 2.0.6 or 2.00.06.00



#### PLATFORM

You would not normally set one of these, they are set for you when you choose compile options in Lazarus. Useful to read to make platform specific decisions at compile time. 

  * UNIX - eg, Linux, macOS, Ultrix
  * WINDOWS - all MS Windows platforms
  * DARWIN - macOS



There are many more .... 

#### Widget Set

You would not normally set one of these, they are set for you when you choose compile options in Lazarus. Useful to read to make widget set specific decisions at compile time. 

  * LCLCOCOA
  * LCLCARBON
  * LCLGTK2
  * LCLGTK3
  * LCLQt (for Qt4)
  * LCLQt5
  * LCLQt6



## Win32 defines

  * **DisableWindowsUnicodeSupport** \- Turns off Unicode support on Windows. The win32 interface gives string in ansi encoding to the LCL (instead of UTF8).
  * **DisableVistaDialogs** \- Turns off Windows Vista (and higher) file dialogs ([IFileDialog](<https://learn.microsoft.com/en-us/windows/win32/api/shobjidl_core/nn-shobjidl_core-ifiledialog>)) and uses the older style dialogs.



## WinCE defines

  * **VerboseWinCE** \- Turns on the use of writelns to output debug information. Useful in desktop windows to trace bugs which are reproducible in the desktop too.



## Gtk defines

  * **LCLGTK2** \- Set when you choose the GTK2 widget set. Treat as read only.


  * **LCLGTK3** \- Set when you choose the GTK3 widget set. Treat as read only.


  * **DebugLCLComponents** \- This activates saving information on every creation or destruction of LCL components, device contexts, gdi objects (pen, brush, font, ...) and main widgets. It will spot mem leaks and double frees with stacktraces.


  * **TraceGDICalls** \- This activates saving information on every creation or destruction of gdi objects (pen, brush, font, ...). It will spot mem leaks and double frees with stacktraces.


  * **GTK2_USE_OLD_CAPTURE** \- As of lazarus 0.9.29 r27638 we use new way of capturing (TGtk2WidgetSet.SetCapture). Old way can be reached with this define.



It's used only for debugging purposes (eg. diff in capturing before old & new code) if some bug is filled against gtk2 capturing. 

  * **USE_GTK_MAIN_OLD_ITERATION** \- As of lazarus 0.9.29 r27896 we use g_main_context* instead of g_main* which is deprecated from gtk2-2.2.



This define fixes sending/receiving messages via PostMessage() from other threads than main.Also, we hook g_main_context's poll function to make g_main_context_wakeup accurate.From glib2 >= 2.24 g_main_context_wakeup is enough, but for smaller versions we need to check if new message arrived (from another thread), and interrupt main_context_iteration. For those who want to use deprecated gtk_main_* there's USE_GTK_MAIN_OLD_ITERATION. 

  * **GTK2OLDENUMFONTFAMILIES** \- As of lazarus 0.9.31 r35400 TGtk2WidgetSet.EnumFontFamiliesEx() does not use raw X11 fonts naming, but pango via fontconfig.



Old fashioned font names can be used with this define. 

  * **GTK2USEDUMMYNOTEBOOKPAGE** \- As of lazarus 1.1 svn revision 39882 dummy notebook page isn't used anymore for gtk2 notebook. If some old gtk2 version



needs it just use this define and pls. report issue about gtk2 version and needed dummy notebook page. 

## Qt defines

You can use the following defines while building LCL to configure your Qt Interface: 

  * **LCLQT** \- Set when you choose the Qt4 widget set. Treat as read only.


  * **LCLQT5** \- Set when you choose the Qt5 widget set. Treat as read only.


  * **VerboseQt** \- Writes extensive debug information about the creation and deletion of Qt objects and LCL Objects to the StdOut.


  * **VerboseQtWinAPI** \- Writes extensive debug information about the Qt implementation of the functions on the LCLIntf unit to StdOut.


  * **VerboseQtEvents** \- Writes extensive debug information about qt events passed via event filter to the StdOut. If defined VerboseQt then this define isn't needed.


  * **VerboseQtWinAPI_MISSING_IMPLEMENTATION** \- Writes extensive debug information about the MISSING Qt implementation of the functions on the LCLIntf unit to StdOut.


  * **QTOPIA** \- Build for qtopia/embedded Qt Interface. See also [Supported Platforms of FreePascal Qt4 binding](<Qt4_binding.md> "Qt4 binding").Do not use QTOPIA define (comment it) if you build qt application for eg. Nokia N9 phone (arm) - it's a pure X11 app, so QTOPIA must be off.


  * **QT_NATIVE_DIALOGS** \- Build with native dialogs (TOpenDialog, TSaveDialog ...). In that case there's no eg. OnFilterChanged() events since Qt uses static functions for native dialogs.


  * **QTDIALOGS_USES_QT_LOOP** \- As of Lazarus svn r23536 qtlcl uses LCL loop for QDialog and QFileDialog classes (TQtDialog, TQtMessageBox, TQtFileDialog). If you want to use "old" (original Qt way) then you can use this define, and QDialogs should run in it's own loop managed by Qt. This define doesn't do anything in case we use QT_NATIVE_DIALOGS (for TQtFileDialog only).


  * **QT_ENABLE_LCL_PAINT_TABS** \- as of r25354 used to allow LCL to paint over tabwidget tabs. PaintEvent from tab provides rect in tab coordinates.


  * **QT_DRAWTEXT_MAP_TO_CLIPRECT** \- there's an issue (fixed already) [[1]](<http://bugs.freepascal.org/view.php?id=17678>) about TCanvas.TextRect() with qtlcl. If you enable QT_DRAWTEXT_MAP_TO_CLIPRECT then Qt will paint your text with respect to DT_WORDBREAK (testpaintrect_2.png), otherwise it works like other widgetsets (testpaintrect_1.png).


  * **VerboseQtCustomControlResizeDeadlock** \- Writes debug information about intercepted deadlocks inside TQtCustomControl when resized.


  * **DebugQtGDIObjects** \- writes debug info about TQtGdiObjects class (checks validity of HGDIObj in IsValidGDIObject())


  * **QTSCROLLABLEFORMS** \- Central widgets of forms are QAbstractScrollArea class, this is enabled by default except if the QTCOCOA define is enabled. If you want less CPU/memory intrusive TCustomForms then uncomment this define. In that case we use QWidget as central widget of TCustomForm, but in that case there's no scrollbars in TCustomForm (eg TCustomForm.AutoScroll means nothing, same applies to TCustomForm. Vertical & Horizontal scrollbars.


  * **QTCOCOA** \- Automatically enabled if we are on DARWIN and CPU=64 bit.If this define is enabled QTSCROLLABLEFORMS is disabled. When QTSCROLLABLEFORMS is enabled with QTCOCOA there are various crashes which aren't fixed yet.


  * **QtUseNativeEventLoop** \- On non X11 targets we use the Qt native loop instead of the LCL one. Valid only for Qt5 widgetset.



### Legacy

Many Qt defines have become unnecessary, entries moved here for clarity. 

  * ~~USE_QT_4_3 - The default Qt version utilized by the interface is 4.2 or superior. By setting this define you allow the interface to use 4.3+ features, like MDI Forms.~~ \- Made default and removed as of 0.9.25
  * ~~QT_HIDDEN_BUTTON_WORKAROUND - This activates a work around for a LCL miss-behavior which makes it create a hidden button to overcome some Gtk problem. This should be removed after LCL is fixed. Link for the bug:<http://www.freepascal.org/mantis/view.php?id=9152> ~~
  * ~~QT_LAZARUS_IDE_WORKAROUND - Activates workaround for invisible mainmenu inside lazarus ide, also moving offsets of TGraphicControl descendants , because geometry of form doesn't take TMainMenu height and TToolbar height into account. This should be removed, as soon as we fix offsets, and accounting of TMainMenu & TToolBar height.~~
  * ~~VerboseQtWinAPI_DEBUG_CLIPBOARD - Writes extensive debug information about LCL clipboard operations.~~
  * ~~QT_NO_MENU_EFFECTS - Turns off menu effects, fading and animating, which sometimes produces problems (eg. stays visible even they are closed). It is also possible that this is an Qt bug, but have to check it first.~~
  * ~~USE_QT_44 - The default Qt version utilized by the interface is 4.3 or superior. By setting this define you allow the interface to use qt 4.4 features like better QPrinter,QPainter support etc.Also, if you use this define, you must download qt bindings for 4.4 (1.66 or superior).~~ as of lazarus svn r 21913 qt-4.5.X is default (no more USE_QT_XX defines).
  * ~~USE_QT_45 - The default Qt version utilized by the interface is 4.3 or superior. By setting this define you allow the interface to use qt 4.5 features like better QPrinter,QPainter support etc.Also, if you use this define, you must download qt bindings for 4.5 (1.69 or superior).~~ as of lazarus svn r 21913 qt-4.5.X is default (no more USE_QT_XX defines).
  * ~~QTSizeFix - fixes resizing overhead by reducing InvalidateClientRectCache() calls to minimum.Now only TQtPage calls it from resize event. Works ok with -dOldAutoSize and especially with new autosize algorithms.Will be set as default soon (and removed define).~~ as of lazarus svn revision 25065 this is default behaviour, define removed.



## Carbon defines

List of Carbon defines used to show debug info when tracing: 

  * DebugEventLoop
  * VerboseObject - Carbon interface application
  * VerboseTimer
  * VerboseWinAPI
  * VerboseLCLIntf
  * VerboseMouse
  * VerboseCursor
  * VerboseKeyboard
  * VerbosePaint
  * VerboseCanvas
  * VerboseMenu
  * VerboseScroll
  * VerboseWidget - TCarbonWidget class
  * VerboseAppEvent - Carbon application event handlers
  * VerboseControlEvent - Carbon control event handlers
  * VerboseWindowEvent - Carbon window event handlers
  * VerboseCommonEvent - Carbon common event handlers
  * VerboseWSClass - TCarbonWS classes
  * VerboseCarbonTrayIcon - TCarbonWSTrayIcon verbose mode



Other defines: 

  * CarbonUseCocoa - Activates using Cocoa controls. Removed in Lazarus 0.9.29
  * CarbonDontUseCocoa - Deactivates using Cocoa controls. Added in Lazarus 0.9.29
  * CarbonUsePrivateAPIs - Activates using _NSGetCarbonMenu in TTrayIcon. Not very useful at the moment.



## FPC Optimization Defines

There are also a number of defines possible that apply to the FP Compiler - see [Optimization in Code](<Optimization.md> "Optimization")

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
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * LCL Defines \- Choosing the right options to recompile LCL
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset



## See Also

[Free Pascal Compiler Documentation - Compiler defines during compilation](<https://www.freepascal.org/docs-html/prog/progap7.html>) such as CPU, Platform, compiler ability etc.

---

_Source: [https://wiki.freepascal.org/LCL_Defines](https://web.archive.org/web/20241206075216/https://wiki.freepascal.org/LCL_Defines)_
