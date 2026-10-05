# Carbon interface internals

│ **[Deutsch (de)](</Carbon_interface_internals/de> "Carbon interface internals/de")** │  **English (en)** │ 

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/50px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

This article applies to [iOS](</Category:iOS> "Category:iOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Introduction
  * 2 Other Interfaces
    * 2.1 Platform specific Tips
    * 2.2 Interface Development Articles
  * 3 Documentation about Carbon
  * 4 Carbon interface development basics
  * 5 What is already working ?
  * 6 What needs to be done next?
  * 7 Carbon IDE Bugs
  * 8 Implementation Details
    * 8.1 Keyboard
    * 8.2 Application
    * 8.3 Drawing and measuring text precisely in parts
    * 8.4 Screenshot taking
    * 8.5 Focusing
    * 8.6 Canvas Clipping
  * 9 Compatibility issues
    * 9.1 Mouse.CursorPos changing does not generate mouse (move) events
    * 9.2 Release mouse capture is not supported
    * 9.3 Command line parameters
    * 9.4 Shortcuts indication is not supported
    * 9.5 Drawing on Canvas outside OnPaint event is not supported
    * 9.6 Drawing on screen device context is not supported
    * 9.7 TCustomControl.Color of clBtnFace makes its background transparent
    * 9.8 TWinControl.Font fsStrikeOut is not supported
    * 9.9 TForm
    * 9.10 TEdit.PasswordChar different then default is not supported
    * 9.11 TMemo.WordWrap when is disabled, does not allow to scroll text horizontally
    * 9.12 TListBox.Columns is not supported
    * 9.13 TComboBox
    * 9.14 TPanel.Bevelxxx: bvLowered and bvSpace are not supported
    * 9.15 TBitBtn.Spacing is not supported
    * 9.16 TTrackBar
    * 9.17 TProgressBar
    * 9.18 TColorDialog.Title is not supported
    * 9.19 macOS System (Windows' and Controls') Handles
    * 9.20 TTrayIcon
      * 9.20.1 Checking if the TTrayIcon menu is visible
  * 10 How to add a new control



## Introduction

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** With the release of macOS 10.15 Catalina in October 2019, Apple has removed all support for the 32 bit Carbon framework from the operating system in favour of the [64 bit Cocoa framework](<Cocoa_Internals.md> "Cocoa Internals"). Consequently, the Carbon widgetset is no longer being developed.

This page gives an overview of the LCL Carbon interface for macOS and will help new developers. 

For installation and creating a first Carbon application refer to [Carbon Interface](<Carbon_Interface.md> "Carbon Interface"). 

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

  * Carbon interface internals \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset



## Documentation about Carbon

  * [Apple Carbon docs](<http://developer.apple.com/documentation/Carbon>)


  * [Apple Mailing List search](<http://search.lists.apple.com>)


  * [Carbon Book for download](<http://www.mactech.com/macintosh-c/downloads.html>)


  * [Carbon Dev](<http://www.carbondev.com/site/>)


  * ["Learning Carbon" sample book chapter](<http://www.oreilly.com/catalog/learncarbon>)


  * Free Pascal's Carbon API unit is FPCMacOSAll.pas in /usr/local/share/fpcsrc/packages/extra/univint.



## Carbon interface development basics

  * Target Mac Tiger version is 10.4
  * Avoid using obsolete or deprecated APIs and functions (e. g. QuickDraw vs. _Quartz 2D_)
  * Use object approach
  * Check Carbon calls result with OSError function
  * Assume UTF-8 encoded strings between LCL and Carbon widgetset (for future Lazarus Unicode support)



## What is already working ?

  * Features - see [Roadmap#Status_of_features_on_each_widgetset](<Roadmap.md> "Roadmap")
  * Forms and controls - see [Roadmap#Status_of_components_on_each_widgetset](<Roadmap.md> "Roadmap")
  * Creating TOpenGLControl with AGL context (see components/opengl/)
  * Mouse events
  * Keyboard events



[![Working components in the Carbon interface](https://wiki.freepascal.org/images/d/d3/Carbon_interface_show_case.png)](</File:Carbon_interface_show_case.png> "Working components in the Carbon interface")

## What needs to be done next?

See [Bug Tracker Carbon open issues](<http://www.freepascal.org/mantis/view_all_set.php?type=3&source_query_id=1017>)

## Carbon IDE Bugs

Item | Note | Dependencies | Responsible   
---|---|---|---  
'...' buttons in dialogs | Now they are too large I think |  |   
OK/Cancel | Many dialogs have non-Mac check-X glyphs |  |   
Menu bar | Too wide for 1024x768 display resolution |  |   
Lazarus menu | Help About and app prefs (Environment options) should be accessible here |  |   
Menus | Many missing or non-standard menu shortcuts:  
File New should be Cmd+N  
File Save As should be Shift+Cmd+S  
File Quit should be Cmd+Q  
Search Find should be Cmd+F  
Search Find Next should be Cmd+G  
Search Find Previous should be Shift+Cmd+G  
(Goto line and Procedure List need to be reassigned)  
Option+Cmd+F does not bring up Search Results  
F11 behaves weird!  |  |   
  
See [Bug Tracker Carbon IDE open issues](<http://www.freepascal.org/mantis/view_all_set.php?type=3&source_query_id=1018>)

## Implementation Details

### Keyboard

  * Apple Command key is mapped to ssMeta
  * Apple Control key is mapped to ssCtrl
  * Apple Option key is mapped to its inscription, i.e. ssAlt
  * Virtual key codes mapping is not reliable (depends on keyboard language layout!)



### Application

  * Title: You cannot change it at runtime. You have to set it in Application Bundle.
  * OnDropFiles event is fired when file is dropped on application dock icon or opened via Finder if is associated. You have to enable this event in Application Bundle.



### Drawing and measuring text precisely in parts

If you want to draw (via TextOut, TextRect) or measure (via TextWidth, TextHeight) text divided into various parts and rely it will be displayed same each time not depending on its division, you have to disable some default typographic features (like kerning, fractional positioning, ...) of Canvas. You can choose one of the following: 

  * CarbonWidgetSet.SetTextFractional(Canvas, False); // from CarbonInt unit
  * TCarbonCustomControl(CustomControl.Handle).TextFractional := False; // from CarbonPrivate unit
  * TCarbonDeviceContext(Canvas.Handle)..TextFractional := False; // from CarbonCanvas unit



### Screenshot taking

The only possible efficient way of taking a screenshot on macOS is using OpenGL. Apple provides a demonstration function which returns a CGImageRef with the screenshot. This function was converted to Pascal and is available on the glgrab.pas unit on the carbon interface directory. This function requires the Apple-specific parts of the Apple OpenGL headers, and those aren't yet on the FPC Packages, so they were translated and are located on lazarus/lcl/interfaces/carbon/opengl.pas until they are released on a stable Free Pascal. 

After obtaining a CGImageRef, the next step to implement the RawImage_fromDevice method is obtaining a local copy of it's pixel data. It isn't possible to directly access the bytes of a CGImageRef, so the image needs to be drawn to a CGContextRef which uses a memory area allocated by us as buffer. This method has the great advantage of converting from the internal format of the screenshot bytes to the very convenient ARGB, 32-bits depth, 8-bits per channel format that is default to LCL. 

### Focusing

macOS has two possible ways of focusing: Full keyboard navigation (enabled with Accessibility options), Text field navigation (only controls that are to receive text input should be focused). Full keyboard navigation is the same as any other widgetset. 

There's no way to say if control is text-input or not. Native OS controls will handle focus themselves. The Carbon Widgetset checks if bound LCLObject is SynEdit and will switch the focus to it. 

### Canvas Clipping

Because Quartz 2D's clipping is decreasing, care must be taken for proper clipping region change. There should be no code, that changes CGContext's clip region directly. Carbon widget API's functions should be used instead. 

## Compatibility issues

These are things that will probably never be solved. 

### Mouse.CursorPos changing does not generate mouse (move) events

### Release mouse capture is not supported

### Command line parameters

Because Carbon applications are executed via Application Bundle, command line parameters are not passed. You have to use OnDropFiles event to detect openning associated files. 

### Shortcuts indication is not supported

### Drawing on Canvas outside OnPaint event is not supported

### Drawing on screen device context is not supported

### TCustomControl.Color of clBtnFace makes its background transparent

### TWinControl.Font fsStrikeOut is not supported

### TForm

  * Icon is not supported
  * ShowInTaskbar is not supported
  * TForm.top=0 is not good. Use TForm.top=23



### TEdit.PasswordChar different then default is not supported

### TMemo.WordWrap when is disabled, does not allow to scroll text horizontally

### TListBox.Columns is not supported

### TComboBox

  * DroppedDown does not show drop down list when style is csDropDownList
  * DropDownCount is not supported
  * Style: csSimple, csOwnerDrawFixed and csOwnerDrawVariable are not supported



### TPanel.Bevelxxx: bvLowered and bvSpace are not supported

### TBitBtn.Spacing is not supported

### TTrackBar

  * LineSize is not supported
  * ScalePos is not supported
  * TickMarks are not supported



### TProgressBar

  * BarShowText is not supported
  * Smooth is not supported
  * Step is not supported



### TColorDialog.Title is not supported

### macOS System (Windows' and Controls') Handles

It’s some times necessary to access system windows or control handles directly. For example, you wish to use some system features that are not available (not yet implemented?) for LCL. It’s very common for LCL Windows developers (and Delphi developers) to use TControl.Handle property, since Handle is a system window handle for Win32 widgetset. It can be used with any system function. But it’s not so for Carbon widgetset. TControl.Handle would return handle native to widgetset not macOS. TControl.Handle would return TCarbonWidget class (declared at CarbonDef unit). This’s a wrapper class, and you can use it to get system handle. 
    
    
    var
     AHiView: HiViewRef;
    begin
    ...
    //this is incorrect way of getting system handle for Carbon widgetset
    //AHiView:= HiViewRef(Button1.Handle)
    
    //this is correct way
    AHiView := TCarbonWidget(Button1.Handle).Widget;
    ...
    end;
    

You should also note, that there’s difference in getting WindowRef in LCL versions. If you’re using Lazarus 0.9.26 or earlier you should get WindowRef in the following way: 
    
    
    var
      MacWin: WindowRef;
    begin
    ...
    MacWin := TCarbonWidget(Form1.Handle).Widget;
    ...
    end;
    

If you’re using svn LCL version (latest trunk), then you should use the following way: 
    
    
    uses
        ..CarbonPrivate..
    var
      MacWin: WindowRef;
    begin
    ...
    MacWin := TCarbonWindow(Form1.Handle).Window;
    ...
    end;
    

Keep in mind, that LCL is designed to be cross-platform library, and accessing system handles to use with system functions makes you program hardly portable. The way of getting system objects handle might also be changed in future. 

### TTrayIcon

#### Checking if the TTrayIcon menu is visible

Unfortunately Apple only added a facility to check menu visibility in Snow Leopard 10.6. To overcome this limitation in a simple way in previous versions one can use the following code, which should work in previous versions using a class method in TCarbonWSCustomTrayIcon: 
    
    
    uses
      CarbonWSExtCtrls;
    
    ...
    
    begin
      if TCarbonWSCustomTrayIcon.IsTrayIconMenuVisible(TrayIcon1) then Caption := 'Visible'
      else Caption := 'Not visible';
    end;
    

## How to add a new control

For example TButton. 

TButton is defined in lcl/buttons.pp. This is the platform independent part of the LCL, which is used by the normal LCL programmer. 

Its widgetset class is in lcl/widgetset/wsbuttons.pp. This is the platform independent base for all widgetsets (carbon, gtk, win32, ...). 

Its Carbon interface class is in lcl/interfaces/carbon/carbonwsbuttons.pp: 
    
    
    TCarbonWSButton = class(TWSButton)
    private
    protected
    public
      class function  CreateHandle(const AWinControl: TWinControl; const AParams: TCreateParams): TLCLIntfHandle; override;
    end;
    

Every WS class that actually implements something must be registered. See the initialization section at the end of the carbonwsXXX.pp unit: 
    
    
    RegisterWSComponent(TCustomButton, TCarbonWSButton);
    

TCarbonWSButton overrides CreateHandle to create a Carbon button helper class called TCarbonButton in carbonprivate.pp, which really creates the button control in the Carbon and installs event handlers.

---

_Source: [https://wiki.freepascal.org/Carbon_interface_internals](https://web.archive.org/web/20240910222500/https://wiki.freepascal.org/Carbon_interface_internals)_
