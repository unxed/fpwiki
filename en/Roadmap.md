# Roadmap

│ **English (en)** │  **[русский (ru)](<../ru/Roadmap.md> "Roadmap/ru")** │    
****

This document gives an idea of the current status of the various parts of Lazarus and also helps new contributors to find a suitable place where they can help. It also shows the people implementing the various parts and the targets. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This page refers to the LCL Interface portion (that talks to the backend toolkits) of the Lazarus Component Library. It does NOT reflect on the actual features of the individual GUI toolkits (eg: GTK2, GTK3, Qt, Qt5, Qt6, fpGUI etc)

## Contents

  * 1 General status of LCL interfaces
  * 2 Current status of the various parts of Lazarus
  * 3 Status of features on each LCL Interface
  * 4 Status of Graphics on each LCL Interface
  * 5 Status of native controls on each LCL Interface
  * 6 Status of dialogs on each LCL Interface
  * 7 Status of TCustomControl based controls on each LCL Interface
  * 8 Status of TGraphicControl based controls on each LCL Interface
  * 9 Status of LazDeviceAPIs on each widgetset
  * 10 See Also



**Legend:**

Working \- Stable, most or all parts implemented. 

Partially Implemented \- Works, but has some features missing 

In progress \- Someone is working on this 

Not Implemented \- Nothing done, needs your help 

Deprecated \- Outdated, obsolete, usage not recommended for new projects 

Unknown - Please review whether this component is working, and set its status here 

## General status of LCL interfaces

Unit | Item | State | Target | Backend | Responsible | Comments   
---|---|---|---|---|---|---  
[GTK1](<GTK1_Interface.md> "GTK1 Interface") | Deprecated interface | working | 1.0 | Gtk | - | \-   
[GTK2](<GTK2_Interface.md> "GTK2 Interface") | Main Linux (and similar UNIXes) interface | working | 1.0 | Gtk2 | [Zeljan](</User:Zeljan> "User:Zeljan") | \-   
[GTK3](<GTK3_Interface.md> "GTK3 Interface") | Linux (and similar UNIXes) interface | progress | 1.4 | Gtk3 | [Zeljan](</User:Zeljan> "User:Zeljan") | Alpha state   
[Win32](<Win32/64_Interface.md> "Win32/64 Interface") | Desktop Windows for both 32 and 64 bits | working | 1.0 | WinAPI | Paul Ishenin and [Vincent](</User:Vincent> "User:Vincent") | \-   
[Qt](<Qt_Interface.md> "Qt Interface") | The Qt4 interface | working | 1.0 | Qt and LCL | [Zeljan](</User:Zeljan> "User:Zeljan") | Depends on qt4 bindings   
[Qt5](<Qt5_Interface.md> "Qt5 Interface") | The Qt5 interface | working | 1.8 | Qt5 and LCL | [Zeljan](</User:Zeljan> "User:Zeljan") | Depends on qt5 bindings   
[Qt6](<Qt6_Interface.md> "Qt6 Interface") | The Qt6 interface | working | 2.4 | Qt6 and LCL | [Zeljan](</User:Zeljan> "User:Zeljan") | Depends on qt6 bindings   
[WinCE](<Windows_CE_Interface.md> "Windows CE Interface") | The Windows CE interface | working | 1.0 | Windows API and LCL | - | Depends on volunteers   
[fpGUI](<fpGUI_Interface.md> "fpGUI Interface") | The fpGUI interface | in progress | no target | fpGUI and LCL | - | Depends on volunteers   
[Carbon](<Carbon_Interface.md> "Carbon Interface") | The Carbon interface | stalled (deprecated) | 1.0 | Carbon and LCL | - | \-   
[Cocoa](<Cocoa_Interface.md> "Cocoa Interface") | The Cocoa interface | working | 2.6 - 2.8? | Cocoa and LCL | [Dmitry](</index.php?title=Dmitry&action=edit&redlink=1> "Dmitry \(page does not exist\)") | Depends on volunteers   
[CustomDrawn](<Custom_Drawn_Interface.md> "Custom Drawn Interface") | The CustomDrawn interface | in progress | no target | LCL, X11, Android NDK and SDK | - | Depends on volunteers   
  
## Current status of the various parts of Lazarus

Unit | Item | State | Target | Skills | Responsible | Comments   
---|---|---|---|---|---|---  
IDE | TCollection Editor | working | 0.9.x | FCL, RTTI, IDE | - | A generic TCollection editor for the various TCollections in the LCL/FCL.   
IDE | [TActionList](<TActionList.md> "TActionList") | working | 0.9.x | - | - | \-   
IDE | Doc Editor | working | - | fpdoc | - | The doc editor will be an integrated fpDoc editor similar to fpde. It will be a process of its own, so that it can show help for dialogs as well. It should also be able to write help for packages.   
IDE | Export LFM as xml | working | - | - | - | Load and save LFM files to XML.   
IDE | [Icon Editor Roadmap](<Icon_Editor_Roadmap.md> "Icon Editor Roadmap") | in progress | post 1.0 | - | - | A simple icon editor with the ability to create lrs files. It will be a good example and can help newbies to create icons for their components.   
LCL | Borderspacing | working | 0.9.x | - | - | for aligned controls   
LCL | Drag&Drop | working |  | - | - |   
LCL | Port to Darwin Power PC, macOS | working | 0.9.x | - | - | depends on FPC 1.9.5   
LCL | Port to macOS (x86) | working | - | - | - | depends on FPC 2.1.1   
LCL | [TSplitter](<TSplitter.md> "TSplitter") | working | 0.9.x | easy | - | \-   
LCL | [TFindDialog](<TFindDialog.md> "TFindDialog") | working | - | - | - | Implemented in 0.9.16   
LCL | [TReplaceDialog](<TReplaceDialog.md> "TReplaceDialog") | working | - | - | - | Implemented in 0.9.16   
LCL | TControl.Font | in progress | 0.9.x | - | - | \-   
LCL | [TTabControl](<TTabControl.md> "TTabControl") | in progress | 0.9.x | - | - | \-   
LCL | Docking (= the combination of forms) | partially working, in progress | post 1.0 | deep LCL and interfaces | Mattias | \-   
LCL | [TFrame](<TFrame.md> "TFrame") (= forms as children) | working | 0.9.28 | deep knowledge of LCL | Mattias, Paul | \-   
IDE | Visual Form Inheritence | working | post 1.0 | IDE | Mattias | Properties are not yet propagated to open descendants   
LCL | MDI - Multiple Documents Interfaces Putting fo ... | in progress | 1.2 | deep LCL and interfaces | [Zeljan](</User:Zeljan> "User:Zeljan") | An MDI LCL emulator for widgetsets which does not support MDI, also native implementation of MDI for qt and win32/64. Currently only qt has full MDI support, others are in progress.   
LCL | Palette support | not implemented | - | - | - | Required to correctly show colors on a 256 colors display   
LCL | [TCoolBar](<TCoolBar.md> "TCoolBar") | partially working, in progress | post 1.0 | LCL and anchoring | [Juha](</User:JuhaManninen> "User:JuhaManninen") | \-   
LCL | [TControlBar](<TControlBar.md> "TControlBar") | skeleton implementation to prevent errors in Delphi conversion, in progress | post 1.0 | LCL and anchoring | [Juha](</User:JuhaManninen> "User:JuhaManninen") | \-   
LCL | [TMaskEdit](<TMaskEdit.md> "TMaskEdit") | working | - | - | [Bart](</User:Bart> "User:Bart") | \-   
LCL | [TDirectoryTreeView](</index.php?title=TDirectoryTreeView&action=edit&redlink=1> "TDirectoryTreeView \(page does not exist\)") | not implemented | - | - | - | \-   
LCL | Constrain maximization to specific area | not implemented | - | winapi, gtk | - | When maximizing a window, the left, top, width and height can all be constrained to a specific rectangular area on the screen/desktop. After this is done, constrain the source editor and maybe other windows   
Components | [TIcon](</index.php?title=TIcon&action=edit&redlink=1> "TIcon \(page does not exist\)") | working | 0.9.26 | - | Marc | \-   
Components | CUPS Package | working | 0.9.x | easy | - |   
  
## Status of features on each LCL Interface

Component | win32 | gtk | gtk2 | carbon | qt/qt5/qt6 | wince | fpgui | cocoa | customdrawn   
---|---|---|---|---|---|---|---|---|---  
Accelerator Keys  | Working | Working | Partially Implemented  | Partially Implemented | Working | Not Applicable  | Not Implemented | Working | Not Implemented   
Caret  | Working | Working | Working  | Working | Working | Unknown  | Not Implemented | Working | Not Implemented   
[Clipboard](<Clipboard.md> "Clipboard") | Working | Working | Working  | Working | Working | Working  | Not Implemented | [Working](<Cocoa_Internals/Clipboard.md> "Cocoa Internals/Clipboard") | Implemented in Android   
Cursors  | Working | Working | Working  | Working | Working | Working  | Partially Implemented | Working | Not Implemented   
Drag & Drop  | Working | Working | Working  | Partially Implemented | Working | Not Applicable  | Not Implemented | Working | Not Implemented   
[Drop files event](<Drop_files_event.md> "Drop files event") | Working | Working | Working  | Partially Implemented | Working | Not Applicable  | Not Implemented | Working | Not Implemented   
MDI Support  | Not Implemented | Not Implemented | Not Implemented  | Not Implemented | Working | Not Implemented  | Not Implemented | Not Implemented | Not Implemented   
Printing  | Working | Working | Working  | Partially Implemented | Working | Unknown  | Not Implemented | Not Implemented | Not Implemented   
Regions  | Working | Working | Working  | Working | Working | Working  | Partially Implemented | Partially Implemented | Working   
TCustomControl descendents  | Working | Working | Working  | Partially Implemented | Working | Working  | Working | Working | Working   
Unicode Support  | Working | Impossible to Implement | Working  | Working | Working | Working  | Working | Working | Working   
[BidiMode](<BidiMode.md> "BidiMode") | Working | Not Implemented | Partially Implemented  | Not Implemented | Working | Not Implemented  | Not Implemented | Not Implemented | Not Implemented   
Application | Working | Working | Working  | Working | Working | Partially Implemented  | Partially Implemented | [Working](<Cocoa_Internals/Application.md> "Cocoa Internals/Application") | Working   
[TTimer](<TTimer.md> "TTimer") | Working | Working | Working  | Working | Working | Partially Implemented  | Working | Working | Working   
TApplication.QueueAsyncCall | Working | Unknown | Working  | Unknown | Working | Unknown  | Working | [Working](<Cocoa_Internals/Application.md> "Cocoa Internals/Application") | Not Implemented   
TThread.Synchronize | Working | Unknown | Working  | Unknown | Working | Unknown  | Working | [Working](<Cocoa_Internals/Application.md> "Cocoa Internals/Application") | Not Implemented   
PostMessage | Working | Unknown | Working  | Working | Working | Unknown  | Working | [Working](<Cocoa_Internals/Application.md> "Cocoa Internals/Application") | Not Implemented   
PostThreadMessage | Working | Unknown | Unknown  | Unknown | Working | Unknown  | Not Implemented | Unknown | Not Implemented   
  
## Status of Graphics on each LCL Interface

Component | win32 | gtk | gtk2 | carbon | qt/qt5/qt6 | wince | fpgui | cocoa | customdrawn   
---|---|---|---|---|---|---|---|---|---  
TBitmap/TPixmap/TIcon/etc | Working | Working | Working  | Working | Working | Working  | Partially Implemented | Working | Working   
TBrush | Working | Working | Working  | Partially Implemented | Working | Working  | Partially Implemented | Working | Working   
TFont | Working | Working | Partially Implemented  | Working | Working | Working  | Partially Implemented | Working | Working   
TPen | Working | Working | Working  | Working | Working | Working  | Partially Implemented | Working | Working   
ExtTextOut | Working | Working | Working  | Working | Working | Working  | Working | Working | Working   
  
## Status of native controls on each LCL Interface

Native controls are TWinControl descendants which do not descend from TCustomControl. 

Component | win32 | gtk | gtk2 | carbon | qt/qt5/qt6 | wince | fpgui | cocoa | customdrawn   
---|---|---|---|---|---|---|---|---|---  
[TBitBtn](<TBitBtn.md> "TBitBtn") | Working | Working | Working  | Working | Working | Working  | Working | Working | Partially Implemented   
[TButton](<TButton.md> "TButton") | Working | Working | Working  | Working | Working | Working  | Working | Working | Working   
[TCalendar](<TCalendar.md> "TCalendar") | Working | Working | Working  | Partially Implemented | Working | Working  | Not Implemented | Working | Not Implemented   
[TCheckBox](<TCheckBox.md> "TCheckBox") | Working | Working | Working  | Working | Working | Working  | Working | Working | Working   
[TCheckGroup](<TCheckGroup.md> "TCheckGroup") | Working | Working | Working  | Working | Working | Working  | Working | Working | Not Implemented   
[TCheckListBox](<TCheckListBox.md> "TCheckListBox") | Working | Working | Working  | Working | Working | Working  | Working | Working | Not Implemented   
[TComboBox](<TComboBox.md> "TComboBox") | Working | Working | Working  | Partially Implemented | Working | Working  | Partially Implemented | [Working](<Cocoa_Internals/Text_Controls.md> "Cocoa Internals/Text Controls") | Implemented in Android   
[TEdit](<TEdit.md> "TEdit") | Working | Working | Working  | Working | Working | Working  | Working | Working | Working   
[TForm](<TForm.md> "TForm") | Working | Working | Working  | Working | Working | Working  | Working | Working | Working   
[TGroupBox](<TGroupBox.md> "TGroupBox") | Working | Working | Working  | Working | Working | Working  | Working | Working | Working   
[TIdleTimer](<TIdleTimer.md> "TIdleTimer") | Working | Working | Working  | Working | Working | Working  | Working | Working | Not Implemented   
[TImageList](<TImageList.md> "TImageList") | Working | Working | Working  | Partially Implemented | Working | Working  | Not Implemented | Working | Not Implemented   
[TListBox](<TListBox.md> "TListBox") | Working | Working | Working  | Working | Working | Working  | Working | Working | Not Implemented   
[TListView](<TListView.md> "TListView") | Working | Working | Partially Implemented  | Partially Implemented | Working | Working  | Not Implemented | Working | Not Implemented   
[TMainMenu](<TMainMenu.md> "TMainMenu") | Working | Working | Working  | Working | Working | Working  | Working | Working | Implemented in Android   
[TMemo](<TMemo.md> "TMemo") | Working | Working | Working  | Working | Working | Working  | Working | [Working](<Cocoa_Internals/Text_Controls.md> "Cocoa Internals/Text Controls") | Working   
[TMenuItem](</index.php?title=TMenuItem&action=edit&redlink=1> "TMenuItem \(page does not exist\)") | Working | Working | Working  | Working | Working | Working  | Working | Working | Implemented in Android   
[TPageControl](<TPageControl.md> "TPageControl") and [TTabSheet](</index.php?title=TTabSheet&action=edit&redlink=1> "TTabSheet \(page does not exist\)") | Working | Working | Working  | Working | Working | Working  | Not Implemented | Working | Not Implemented   
[TPairSplitter](<TPairSplitter.md> "TPairSplitter") | Working | Working | Working  | Working | Working | Not Implemented  | Working | Working | Not Implemented   
[TPanel](<TPanel.md> "TPanel") | Working | Working | Working  | Working | Working | Working  | Working | Working | Working   
[TPopupMenu](<TPopupMenu.md> "TPopupMenu") | Working | Working | Working  | Working | Working | Working  | Working | Working | Not Implemented   
[TProgressBar](<TProgressBar.md> "TProgressBar") | Working | Working | Working  | Working | Working | Working  | Working | Working | Working   
[TRadioButton](<TRadioButton.md> "TRadioButton") | Working | Working | Working  | Working | Working | Working  | Working | Working | Partially Implemented   
[TRadioGroup](<TRadioGroup.md> "TRadioGroup") | Working | Working | Working  | Working | Working | Working  | Working | Working | Not Implemented   
[TScrollBar](<TScrollBar.md> "TScrollBar") | Working | Working | Working  | Working | Working | Working  | Partially Implemented | Working | Partially Implemented   
[TScrollBox](<TScrollBox.md> "TScrollBox") | Working | Working | Working  | Partially Implemented | Working | Unknown  | Partially Implemented | Working | Not Implemented   
[TSpinEdit](<TSpinEdit.md> "TSpinEdit") | Working | Working | Working  | Working | Working | Unknown  | Not Implemented | Working | Partially Implemented   
[TSplitter](<TSplitter.md> "TSplitter") | Working | Working | Working  | Working | Working | Unknown  | Partially Implemented | Working | Not Implemented   
[TStaticText](<TStaticText.md> "TStaticText") | Working | Working | Working  | Working | Working | Working  | Working | Working | Working   
[TStatusBar](<TStatusBar.md> "TStatusBar") | Working | Working | Working  | Working | Working | Working  | Not Implemented | Working | Not Implemented   
[TToggleBox](<TToggleBox.md> "TToggleBox") | Working | Working | Working  | Working | Working | Partially Implemented  | Not Implemented | Working | Not Implemented   
[TTrackBar](<TTrackBar.md> "TTrackBar") | Working | Working | Working  | Working | Working | Working  | Not Implemented | Working | Working   
[TTrayIcon](<TTrayIcon.md> "TTrayIcon") | Working | Working | Working  | Partially Implemented | Working | Not Implemented  | Not Implemented | Working | Not Implemented   
  
## Status of dialogs on each LCL Interface

Component | win32 | gtk | gtk2 | carbon | qt/qt5 | wince | fpgui | cocoa | customdrawn   
---|---|---|---|---|---|---|---|---|---  
LCLIntf.MessageBox | Working | Working | Working  | Not Implemented | Working | Working  | Working | Working | Implemented for Android   
Application.MessageBox, MessageDlg, LCLIntf.PromptUser | Working | Working | Working  | Working | Working | Working  | Working | Working | Implemented for Android   
LCLIntf.AskUser | Working | Working | Working  | Working | Working | Working  | Not Implemented | Working | Not Implemented   
[TColorDialog](<TColorDialog.md> "TColorDialog") | Working | Working | Working  | Working | Working | Not Implemented  | Working | Working | Not Implemented   
[TFontDialog](<TFontDialog.md> "TFontDialog") | Working | Working | Working  | Working | Working | Not Implemented  | Working | Working | Not Implemented   
[TOpenDialog](<TOpenDialog.md> "TOpenDialog") | Working | Working | Working  | Working | Working | Working  | Working | Working | Not Implemented   
[TPrinterSetupDialog](<TPrinterSetupDialog.md> "TPrinterSetupDialog") | Working | Working | Working  | Not Implemented | Working | Not Implemented  | Not Implemented | Not Implemented | Not Implemented   
[TSaveDialog](<TSaveDialog.md> "TSaveDialog") | Working | Working | Working  | Working | Working | Working  | Working | Working | Not Implemented   
[TTaskDialog](<TTaskDialog.md> "TTaskDialog") | Working | Unknown | Working  | Working | Working | Unknown  | Working | Working | Not Implemented   
  
## Status of TCustomControl based controls on each LCL Interface

Note that being a TCustomControl descendant does not guarantee that a control has no widgetset implementation. TArrow has it, although it has a good default implementation. TNotebook is fully implemented in the LCL. 

Component | win32 | gtk | gtk2 | carbon | qt/qt5/qt6 | wince | fpgui | cocoa | customdrawn   
---|---|---|---|---|---|---|---|---|---  
[TArrow](<TArrow.md> "TArrow") | Working | Working | Working  | Working | Working | Working  | Working | Working | Working   
[TNotebook](<TNotebook.md> "TNotebook") | Working | Working | Working  | Working | Working | Working  | Not Implemented | Working | Working   
[TUpDown](<TUpDown.md> "TUpDown") | Working | Working | Working  | Working | Working | Partially Implemented  | Working | Working | Partially Implemented   
[TStringGrid](<TStringGrid.md> "TStringGrid") | Working | Working | Working  | Partially Implemented | Working | Partially Implemented  | Partially Implemented | Working | Partially Implemented   
[TDrawGrid](<TDrawGrid.md> "TDrawGrid") | Working | Working | Working  | Partially Implemented | Working | Unknown  | Partially Implemented | Working | Partially Implemented   
[TToolBar](<TToolBar.md> "TToolBar") | Working | Working | Working  | Working | Working | Working  | Not Implemented | Working | Not Implemented   
[TTreeView](<TTreeView.md> "TTreeView") | Working | Working | Working  | Partially Implemented | Working | Working  | Not Implemented | Working | Not Implemented   
[TValueListEditor](<TValueListEditor.md> "TValueListEditor") | Working | Working | Working  | Partially Implemented | Working | Working  | Partially Implemented | Working | Not Implemented   
  
## Status of TGraphicControl based controls on each LCL Interface

**Note:** These are for LCL wrapped components only, **not** for the specific GUI toolkit features itself. 

Component | win32 | gtk | gtk2 | carbon | qt/qt5/qt6 | wince | fpgui | cocoa | customdrawn   
---|---|---|---|---|---|---|---|---|---  
[TBevel](<TBevel.md> "TBevel") | Working | Working | Working  | Working | Working | Partially Implemented  | Working | Working | Not Implemented   
[TLabel](<TLabel.md> "TLabel") | Working | Working | Working  | Working | Working | Working  | Working | Working | Implemented for Android   
[TShape](<TShape.md> "TShape") | Working | Working | Working  | Partially Implemented | Working | Partially Implemented  | Working | Working | Working   
[TSpeedButton](<TSpeedButton.md> "TSpeedButton") | Working | Working | Working  | Working | Working | Unknown  | Working | Working | Working   
[TPaintBox](<TPaintBox.md> "TPaintBox") | Working | Working | Working  | Working | Working | Unknown  | Working | Working | Working   
[TImage](<TImage.md> "TImage") | Working | Working | Working  | Working | Working | Partially Implemented  | Working | Working | Working   
  
## Status of LazDeviceAPIs on each widgetset

Component | customdrawn-android   
---|---  
Accelerometer | Working   
Messaging (SMS, MMS and E-Mail) | SMS Implemented   
PositionInfo | Working   
  
Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch") | [4.0](<Lazarus_4.md> "Lazarus 4.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

  


## See Also

  * [LCL Internals](<LCL_Internals.md> "LCL Internals")
  * [TAChart Roadmap](<TAChart_Roadmap.md> "TAChart Roadmap")
  * [Debugger Status](<Debugger_Status.md> "Debugger Status")

---

_Source: [https://wiki.freepascal.org/Roadmap](https://web.archive.org/web/20250403230717/https://wiki.freepascal.org/Roadmap)_
