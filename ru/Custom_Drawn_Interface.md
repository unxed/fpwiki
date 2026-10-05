# Custom Drawn Interface

│ **[English (en)](<../en/Custom_Drawn_Interface.md>)** │  **русский (ru)** │

## Contents

  * 1 Other Interfaces
    * 1.1 Platform specific Tips
    * 1.2 Interface Development Articles
  * 2 Введение
  * 3 FAQ
    * 3.1 Comparison of LCL-CustomDrawn and LCL-fpGUI
  * 4 Движки для LCL-CustomDrawn
    * 4.1 LCL-CustomDrawn-Android
    * 4.2 LCL-CustomDrawn-X11
    * 4.3 LCL-CustomDrawn-Cocoa
    * 4.4 LCL-CustomDrawn-Windows
    * 4.5 LCL-CustomDrawn-iPhone
  * 5 Canvas
  * 6 Отрисовка шрифтов
  * 7 Message and Common Dialogs
  * 8 Оконные визуальные элементы управления
  * 9 Conditional defines accepted by LCL-CustomDrawn



## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<../en/Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<../en/Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<../en/Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<../en/Carbon_Interface.md> "Carbon Interface") \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
  * [Cocoa Interface](<../en/Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa 64 bit interface for macOS
  * [Qt Interface](<../en/Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [Qt5 Interface](<../en/Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [GTK1 Interface](<../en/GTK1_Interface.md> "GTK1 Interface") \- The gtk1 interface for Unixes, macOS (X11), Windows
  * [GTK2 Interface](<../en/GTK2_Interface.md> "GTK2 Interface") \- The gtk2 interface for Unixes, macOS (X11), Windows
  * [GTK3 Interface](<../en/GTK3_Interface.md> "GTK3 Interface") \- The gtk3 interface for Unixes, macOS (X11), Windows
  * [fpGUI Interface](<../en/fpGUI_Interface.md> "fpGUI Interface") \- Based on the fpGUI library, which is a cross-platform toolkit completely written in Object Pascal
  * [Custom Drawn Interface](<../en/Custom_Drawn_Interface.md> "Custom Drawn Interface") \- A cross-platform LCL backend written completely in Object Pascal inside Lazarus. The Lazarus interface to Android.



### Platform specific Tips

  * [Android Programming](<../en/Android_Programming.md> "Android Programming") \- For Android smartphones and tablets
  * [iPhone/iPod development](<../en/iPhone/iPod_development.md> "iPhone/iPod development") \- About using Objective Pascal to develop iOS applications
  * [FreeBSD Programming Tips](<../en/FreeBSD_Programming_Tips.md> "FreeBSD Programming Tips") \- FreeBSD programming tips
  * [Linux Programming Tips](<../en/Linux_Programming_Tips.md> "Linux Programming Tips") \- How to execute particular programming tasks in Linux
  * [macOS Programming Tips](<../en/macOS_Programming_Tips.md> "macOS Programming Tips") \- Lazarus tips, useful tools, Unix commands, and more...
  * [WinCE Programming Tips](<../en/WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Using the telephone API, sending SMSes, and more...
  * [Windows Programming Tips](<../en/Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips



### Interface Development Articles

  * [Carbon interface internals](<../en/Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<../en/Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<../en/Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<../en/LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<../en/LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<../en/Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset



## Введение

LCL-CustomDrawn-Android имеет следующие оссобенности: 

  * Движки для X11, Android и Windows и частично рабочий для Cocoa, другие могут быть добавлены в будущем
  * Отрисовка сделана полностью внутри Lazarus, не используя родных(native) библиотек, за исключением отрисовки текста. Это гарантирует одинаковое изображение на всех платформах и единый уровень поддерживаемых функций
  * Только одно родное(native) окно используется для каждой формы, в backend для Андроид на данный момент это одно native-окно для всего приложения
  * Используются the Lazarus Custom Drawn Controls для реализации стандартных элементов управления LCL
  * Испольуются его графический движок для частей LCL: TLazIntfImage, TRawImage, lazcanvas и lazregions



## FAQ

### Comparison of LCL-CustomDrawn and LCL-fpGUI

**Item** | **LCL-fpGUI** | **LCL-CustomDrawn**  
---|---|---  
Native handles | fpGUI uses 1 native window for each control | LCL-CustomDrawn uses 1 native window for each form in X11, Cocoa and Windows and 1 native surface for all forms in Android   
Canvas implementation | fpGUI uses native Canvas routines from the platform | LCL-CustomDrawn has a define to choose between native and non-native (via LazFreeType) text metrics/rendering and everything else from canvas drawing is implemented by TRawImage+TLazIntfImage+TLazCanvas. Being non-native means a faster and more reliable result.   
Supported platforms | Windows, Windows CE and Linux (via X11) | Windows, Cocoa (Mac OS X), X11 (Linux) and Android   
Level of indirection | fpGUI acts as an intermediary between the LCL and the platform | There are no intermediaries, the LCL talks directly to each platform   
Adaptation for platforms without native sub-windows | fpGUI would need extensive changes to work in platforms which do not have native sub-controls, for example Android, linux framebuffer, OpenGL | LCL-CustomDrawn is designed from the start to work on very limited platforms, for example the Linux Framebuffer, an Android SurfaceView or OpenGL   
LCL Adaptation | fpGUI is a separate unrelated framework. It's controls and Canvas APIs do not necessary match what is required by the LCL | LCL-CustomDrawn is designed to perfectly implement all features from the LCL and perfectly paint it's Canvas drawing routines like LCL-Win32, LCL-Gtk2 and LCL-Qt do   
How to port the LCL to a new platform using this widgetset? | First you need to port fpGUI and then verify if LCL-fpgui keeps working | You can port LCL-CustomDrawn directly to new platforms by implementing a new backend which contains support for form, application and and other parts   
  
More comments on why developing LCL-CustomDrawn instead of LCL-fpGUI: Just to start with, it is the correct architecture. There is no need for an intermediary API which complicates the architecture and makes debugging and porting harder. We can do everything directly without it and greatly simplify our code. Also, by eating our own dog food we ensure that our platform is more strongly tested and has a higher quality. Each feature in LCL-CustomDrawn is implemented using other more basic features. For example TButton, TPageControl and all other visual controls are implemented with TCanvas, so it guarantees that our Canvas drawing is in excellent shape. Drawing in LCL-CustomDrawn is executed via LCL classes: TRawImage, TLazIntfImage and TLazCanvas. If there is an intermediary API then porting the LCL means first porting the intermediary API and debugging it and then debug the combined package. LCL porters should not need to learn unrelated frameworks to port the LCL. 

On top of that, LCL-CustomDrawn is designed from the start to be portable to even the most spartan and problematic of platforms. It requires from the platform only a raster surface in any pixel format and also input events. From that onwards we can implement everything else non-natively. There are non-native implementations of Forms, WinControls, standard controls, TCanvas drawing, dialogs and even text can be provided if necessary. Each backend can select its mix of non-native / native elements, although they should never attempt to implement TWinControl natively. fpGUI on the other hand would need a large rewrite to be able to support this level of portability. 

## Движки для LCL-CustomDrawn

LCL-CustomDrawn нуждается в движках для реализации основной части виджетов. Каждый backend должен реализовать следующие минимальные части: 

  * TWidgetSet.Run, ProcessMessages, etc
  * TForm
  * TWinControl with all events
  * TTrayIcon



### LCL-CustomDrawn-Android

Этот движок работает. Смотрите эту страницу [Custom Drawn Interface/Android](<../en/Custom_Drawn_Interface/Android.md> "Custom Drawn Interface/Android")

Также смотрите [Android Programming/ru](<Android_Programming.md> "Android Programming/ru")

### LCL-CustomDrawn-X11

Этот движок работает. Смотрите [Custom Drawn Interface/X11](<../en/Custom_Drawn_Interface/X11.md> "Custom Drawn Interface/X11")

### LCL-CustomDrawn-Cocoa

Этот движок работает. 

### LCL-CustomDrawn-Windows

Этот движок работает. 

### LCL-CustomDrawn-iPhone

Этот движок планируется, но еще не начат. 

## Canvas

TCanvas will be fully non-native in this widgetset and based on [LazCanvas](<../en/Developing_with_Graphics.md> "Developing with Graphics"). All drawings to visual controls and on the OnPaint event of controls of the form are naturally double-buffered because the entire drawing of the form is first performed on an off-screen buffer and then copied in 1 operation to the form native canvas. In reality there is only 1 TLazCanvas for the entire form, but sub-controls will think they are on a separate canvas because the property BaseWindowOrg sets an internal start position of the canvas and also because all drawings will be clipped to reflect the size and shape of the sub-control. 

## Отрисовка шрифтов

Когда определение CD_UseNativeText установленно, LCL-CustomDrawn будет использовать родной(native) для платформы способ отрисовки текста, как это предусмотренно движком. Если оно не активно, тогда будет использоватся [LazFreeType](<../en/LazFreeType.md> "LazFreeType") для отрисовки текста. Определение автоматически активно для некоторых движков, если это удобно для их использования. 

## Message and Common Dialogs

Message and Common Dialogs might be native if this is considered very convenient for the backend. If not, they will be non-native. At the moment the Android backend has native message boxes. 

## Оконные визуальные элементы управления

Все оконные визуальные элементы управления (TButton, TPageControl, и др.) будут основыватся на [Lazarus Custom Drawn Controls](<../en/Lazarus_Custom_Drawn_Controls.md> "Lazarus Custom Drawn Controls")

## Conditional defines accepted by LCL-CustomDrawn

Here are defines which affect the functionality offered by this interface: 

  * CD_UseNativeText - Activates using native text instead of LazFreeType. This define will be automatically set if convenient for a particular backend



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

---

_Source: [https://wiki.freepascal.org/Custom_Drawn_Interface/ru](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/Custom_Drawn_Interface/ru)_
