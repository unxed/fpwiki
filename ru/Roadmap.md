# Roadmap

│ **[English (en)](<../en/Roadmap.md>)** │  **русский (ru)** │

Этот документ дает представление о текущем состоянии различных частей **Lazarus** , а также помогает новым участникам найти подходящее для себя направление, в котором они могут помочь. Здесь также отображаются имена людей, реализующих некоторые части для целевых платформ. 

## Contents

  * 1 Общее состояние наборов виджетов
  * 2 Текущее состояние различных частей Lazarus
  * 3 Статус возможностей в интерфейсе LCL для каждой платформы
  * 4 Статус Graphics в интерфейсе LCL для каждой платформы
  * 5 Статус встроенных элементов управления в интерфейсе LCL для каждой платформы
  * 6 Статус диалогов в интерфейсе LCL для каждой платформы
  * 7 Статус элементов управления на основе TCustomControl в интерфейсе LCL для каждой платформы
  * 8 Статус элементов управления на основе TGraphicControl в интерфейсе LCL для каждой платформы
  * 9 Статус LazDeviceAPIs на каждом widgetset
  * 10 Смотри также



**Легенда:**

Работает \- Стабильная версия, реализованы все или большинство частей. 

Частично реализовано \- Работает, но часть возможностей отсутствует. 

В процессе \- Кто-то уже работает над реализацией. 

Не реализовано \- Ничего не реализовано, требуется ваша помощь. 

Устаревшее \- Устаревшая реализация, не рекомендуется для использования в новых проектах. 

Не известно - Пожалуйста, проверьте работает ли данный компонент и укажите здесь статус его работы. 

## Общее состояние наборов виджетов

Unit | Item | State | Target | Backend | Responsible | Comments   
---|---|---|---|---|---|---  
[GTK1](<../en/GTK1_Interface.md> "GTK1 Interface") | Deprecated interface | working | 1.0 | Gtk | - | \-   
[GTK2](<../en/GTK2_Interface.md> "GTK2 Interface") | Main Linux (and similar UNIXes) interface | working | 1.0 | Gtk2 | [Zeljan](</User:Zeljan> "User:Zeljan") | \-   
[GTK3](<../en/GTK3_Interface.md> "GTK3 Interface") | Linux (and similar UNIXes) interface | progress | 1.4 | Gtk3 | [Zeljan](</User:Zeljan> "User:Zeljan") | Alpha state   
[Win32](<../en/Win32/64_Interface.md> "Win32/64 Interface") | Desktop Windows for both 32 and 64 bits | working | 1.0 | WinAPI | Paul Ishenin and [Vincent](</User:Vincent> "User:Vincent") | \-   
[Qt](<../en/Qt_Interface.md> "Qt Interface") | The Qt4 interface | working | 1.0 | Qt and LCL | [Zeljan](</User:Zeljan> "User:Zeljan") | Depends on qt4 bindings   
[Qt5](<../en/Qt5_Interface.md> "Qt5 Interface") | The Qt5 interface | working | 1.8 | Qt5 and LCL | [Zeljan](</User:Zeljan> "User:Zeljan") | Depends on qt5 bindings   
[Qt6](<../en/Qt6_Interface.md> "Qt6 Interface") | The Qt6 interface | working | 2.4 | Qt6 and LCL | [Zeljan](</User:Zeljan> "User:Zeljan") | Depends on qt6 bindings   
[WinCE](<../en/Windows_CE_Interface.md> "Windows CE Interface") | The Windows CE interface | working | 1.0 | Windows API and LCL | - | Depends on volunteers   
[fpGUI](<../en/fpGUI_Interface.md> "fpGUI Interface") | The fpGUI interface | in progress | no target | fpGUI and LCL | - | Depends on volunteers   
[Carbon](<../en/Carbon_Interface.md> "Carbon Interface") | The Carbon interface | stalled (deprecated) | 1.0 | Carbon and LCL | - | \-   
[Cocoa](<../en/Cocoa_Interface.md> "Cocoa Interface") | The Cocoa interface | working | 2.6 - 2.8? | Cocoa and LCL | [Dmitry](</index.php?title=Dmitry&action=edit&redlink=1> "Dmitry \(page does not exist\)") | Depends on volunteers   
[CustomDrawn](<../en/Custom_Drawn_Interface.md> "Custom Drawn Interface") | The CustomDrawn interface | in progress | no target | LCL, X11, Android NDK and SDK | - | Depends on volunteers   
  
## Текущее состояние различных частей Lazarus

Unit | Item | Состояние | Target | Навыки | Ответственный | Комментарии   
---|---|---|---|---|---|---  
[IDE](<../en/IDE.md> "IDE") | TCollection Editor | Работает | 0.9.x | FCL, RTTI, IDE | - | A generic TCollection editor for the various TCollections in the LCL/FCL.   
[IDE](<../en/IDE.md> "IDE") | TActionList | Работает | 0.9.x | - | - | \-   
[IDE](<../en/IDE.md> "IDE") | Doc Editor | Работает | - | fpdoc | - | The doc editor will be an integrated fpDoc editor similar to fpde. It will be a process of its own, so that it can show help for dialogs as well. It should also be able to write help for packages.   
[IDE](<../en/IDE.md> "IDE") | Export LFM as xml | Работает | - | - | - | Load and save LFM files to XML.   
[IDE](<../en/IDE.md> "IDE") | [Icon Editor Roadmap](<../en/Icon_Editor_Roadmap.md> "Icon Editor Roadmap") | в процессе | после 1.0 | - | - | A simple icon editor with the ability to create lrs files. It will be a good example and can help newbies to create icons for their components.   
[LCL](<LCL.md> "LCL/ru") | Borderspacing | Работает | 0.9.x | - | - | for aligned controls   
[LCL](<LCL.md> "LCL/ru") | Drag&Drop | Работает |  | - | - |   
[LCL](<LCL.md> "LCL/ru") | Port to Darwin Power PC, macOS | Работает | 0.9.x | - | - | depends on FPC 1.9.5   
[LCL](<LCL.md> "LCL/ru") | Port to macOS 86 | Работает | - | - | - | depends on FPC 2.1.1   
[LCL](<LCL.md> "LCL/ru") | TSplitter | Работает | 0.9.x | easy | - | \-   
[LCL](<LCL.md> "LCL/ru") | TFindDialog | Работает | - | - | - | Реализовано в 0.9.16   
[LCL](<LCL.md> "LCL/ru") | TReplaceDialog | Работает | - | - | - | Реализовано в 0.9.16   
[LCL](<LCL.md> "LCL/ru") | TControl.Font | в процессе | 0.9.x | - | - | \-   
[LCL](<LCL.md> "LCL/ru") | TTabControl | в процессе | 0.9.x | - | - | \-   
[LCL](<LCL.md> "LCL/ru") | Docking (= комбинация форм) | частично работает, в процессе | после 1.0 | глубокое знание LCL и интерфейсов | Mattias | \-   
[LCL](<LCL.md> "LCL/ru") | Frames (= forms as children) | Работает | 0.9.28 | глубокое знание LCL | Mattias, Paul | \-   
[IDE](<../en/IDE.md> "IDE") | Visual Form Inheritence | Работает | после 1.0 | IDE | Mattias | Properties are not yet propagated to open descendants   
[LCL](<LCL.md> "LCL/ru") | MDI - Multiple Documents Interfaces Putting fo ... | в процессе | 1.2 | глубокое знание LCL и интерфейсов | [Zeljan](</User:Zeljan> "User:Zeljan") | An MDI LCL emulator for widgetsets which does not support MDI, also native implementation of MDI for qt and win32/64. Currently only qt has full MDI support, others are in progress.   
[LCL](<LCL.md> "LCL/ru") | Palette support | не реализовано | - | - | - | Required to correctly show colors on a 256 colors display   
[LCL](<LCL.md> "LCL/ru") | TCoolBar | частично работает, в процессе | после 1.0 | LCL and anchoring | [Juha](</User:JuhaManninen> "User:JuhaManninen") | \-   
[LCL](<LCL.md> "LCL/ru") | TControlBar | скелетная реализация для предотвращения ошибок в преобразовании из Delphi, в процессе | после 1.0 | LCL and anchoring | [Juha](</User:JuhaManninen> "User:JuhaManninen") | \-   
[LCL](<LCL.md> "LCL/ru") | TMaskEdit | Работает | - | - | [Bart](</User:Bart> "User:Bart") | \-   
[LCL](<LCL.md> "LCL/ru") | TDirectoryTreeView | не реализовано | - | - | - | \-   
[LCL](<LCL.md> "LCL/ru") | Constrain maximization to specific area | не реализовано | - | winapi, gtk | - | When maximizing a window, the left, top, width and height can all be constrained to a specific rectangular area on the screen/desktop. After this is done, constrain the source editor and maybe other windows   
Components | TIcon | Работает | 0.9.26 | - | Marc | \-   
Components | CUPS Package | Работает | 0.9.x | easy | - |   
  
## Статус возможностей в интерфейсе LCL для каждой платформы

Компонент | win32 | gtk | gtk2 | carbon | qt | wince | fpgui | cocoa | customdrawn   
---|---|---|---|---|---|---|---|---|---  
Accelerator Keys  | Работает | Работает | Частично реализовано  | Частично реализовано | Работает | Не применимо  | Не реализовано | Работает | Не реализовано   
Caret  | Работает | Работает | Работает  | Работает | Работает | Не известно  | Не реализовано | Работает | Не реализовано   
[Clipboard](<../en/Clipboard.md> "Clipboard") | Работает | Работает | Работает  | Работает | Работает | Работает  | Не реализовано | Работает | Реализовано в Android   
Cursors  | Работает | Работает | Работает  | Работает | Работает | Работает  | Частично реализовано | Работает | Не реализовано   
Drag & Drop  | Работает | Работает | Работает  | Частично реализовано | Работает | Не применимо  | Не реализовано | Работает | Не реализовано   
[Drop files event](<../en/Drop_files_event.md> "Drop files event") | Работает | Работает | Работает  | Частично реализовано | Работает | Не применимо  | Не реализовано | Работает | Не реализовано   
MDI Support  | Не реализовано | Не реализовано | Не реализовано  | Не реализовано | Работает | Не реализовано  | Не реализовано | Не реализовано | Не реализовано   
Printing  | Работает | Работает | Работает  | Частично реализовано | Работает | Не известно  | Не реализовано | Не реализовано | Не реализовано   
Regions  | Работает | Работает | Работает  | Работает | Работает | Работает  | Частично реализовано | Частично реализовано | Работает   
TCustomControl descendents  | Работает | Работает | Работает  | Частично реализовано | Работает | Работает  | Работает | Работает | Работает   
Unicode Support  | Работает | Невозможно реализовать | Работает  | Работает | Работает | Работает  | Работает | Работает | Работает   
[BidiMode](<../en/BidiMode.md> "BidiMode") | Работает | Не реализовано | Частично реализовано  | Не реализовано | Работает | Не реализовано  | Не реализовано | Не реализовано | Не реализовано   
Application | Работает | Работает | Работает  | Работает | Работает | Частично реализовано  | Частично реализовано | Работает | Работает   
[TTimer](<TTimer.md> "TTimer/ru") | Работает | Работает | Работает  | Работает | Работает | Частично реализовано  | Работает | Работает | Работает   
TApplication.QueueAsyncCall | Работает | Не известно | Работает  | Не известно | Работает | Не известно  | Работает | Работает | Не реализовано   
TThread.Synchronize | Работает | Не известно | Работает  | Не известно | Работает | Не известно  | Работает | Работает | Не реализовано   
PostMessage | Работает | Не известно | Работает  | Не известно | Работает | Не известно  | Работает | Работает | Не реализовано   
PostThreadMessage | Работает | Не известно | Не известно  | Не известно | Не известно | Не известно  | Не реализовано | Не известно | Не реализовано   
  
## Статус Graphics в интерфейсе LCL для каждой платформы

Компонент | win32 | gtk | gtk2 | carbon | qt | wince | fpgui | cocoa | customdrawn   
---|---|---|---|---|---|---|---|---|---  
TBitmap/TPixmap/TIcon/etc | Работает | Работает | Работает  | Работает | Частично реализовано | Работает  | Частично реализовано | Работает | Работает   
TBrush | Работает | Работает | Работает  | Частично реализовано | Работает | Работает  | Частично реализовано | Работает | Работает   
TFont | Работает | Работает | Частично реализовано  | Работает | Работает | Работает  | Частично реализовано | Работает | Работает   
TPen | Работает | Работает | Работает  | Работает | Работает | Работает  | Частично реализовано | Работает | Работает   
ExtTextOut | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Не известно | Работает   
  
## Статус встроенных элементов управления в интерфейсе LCL для каждой платформы

Встроенные элементы управления являются потомками TWinControl, которые не происходят от TCustomControl. 

Компонент | win32 | gtk | gtk2 | carbon | qt | wince | fpgui | cocoa | customdrawn   
---|---|---|---|---|---|---|---|---|---  
[TBitBtn](<TBitBtn.md> "TBitBtn/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Частично реализовано   
[TButton](<TButton.md> "TButton/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Работает   
[TCalendar](<TCalendar.md> "TCalendar/ru") | Работает | Работает | Работает  | Частично реализовано | Работает | Работает  | Не реализовано | Работает | Не реализовано   
[TCheckBox](<TCheckBox.md> "TCheckBox/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Работает   
[TCheckGroup](<TCheckGroup.md> "TCheckGroup/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Не реализовано   
[TCheckListBox](<TCheckListBox.md> "TCheckListBox/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Не реализовано   
[TComboBox](</index.php?title=TComboBox/ru&action=edit&redlink=1> "TComboBox/ru \(page does not exist\)") | Работает | Работает | Работает  | Частично реализовано | Работает | Работает  | Частично реализовано | Работает | Реализовано в Android   
[TEdit](<TEdit.md> "TEdit/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Работает   
[TForm](<TForm.md> "TForm/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Работает   
[TGroupBox](<TGroupBox.md> "TGroupBox/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Работает   
[TIdleTimer](<TIdleTimer.md> "TIdleTimer/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Не реализовано   
[TImageList](<TImageList.md> "TImageList/ru") | Работает | Работает | Работает  | Частично реализовано | Работает | Работает  | Не реализовано | Работает | Не реализовано   
[TListBox](</index.php?title=TListBox/ru&action=edit&redlink=1> "TListBox/ru \(page does not exist\)") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Не реализовано   
[TListView](<TListView.md> "TListView/ru") | Работает | Работает | Частично реализовано  | Частично реализовано | Работает | Работает  | Не реализовано | Работает | Не реализовано   
[TMainMenu](<TMainMenu.md> "TMainMenu/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Реализовано в Android   
[TMemo](<TMemo.md> "TMemo/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Работает   
[TMenuItem](</index.php?title=TMenuItem/ru&action=edit&redlink=1> "TMenuItem/ru \(page does not exist\)") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Реализовано в Android   
[TPageControl](<TPageControl.md> "TPageControl/ru") and [TTabSheet](</index.php?title=TTabSheet/ru&action=edit&redlink=1> "TTabSheet/ru \(page does not exist\)") | Работает | Работает | Работает  | Работает | Работает | Работает  | Не реализовано | Работает | Не реализовано   
[TPairSplitter](<TPairSplitter.md> "TPairSplitter/ru") | Работает | Работает | Работает  | Работает | Работает | Не реализовано  | Работает | Работает | Не реализовано   
[TPanel](<TPanel.md> "TPanel/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Работает   
[TPopupMenu](<TPopupMenu.md> "TPopupMenu/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Не реализовано   
[TProgressBar](<TProgressBar.md> "TProgressBar/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Работает   
[TRadioButton](</index.php?title=TRadioButton/ru&action=edit&redlink=1> "TRadioButton/ru \(page does not exist\)") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Частично реализовано   
[TRadioGroup](<TRadioGroup.md> "TRadioGroup/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Не реализовано   
[TScrollBar](</index.php?title=TScrollBar/ru&action=edit&redlink=1> "TScrollBar/ru \(page does not exist\)") | Работает | Работает | Работает  | Работает | Работает | Работает  | Частично реализовано | Работает | Частично реализовано   
[TScrollBox](<TScrollBox.md> "TScrollBox/ru") | Работает | Работает | Работает  | Частично реализовано | Работает | Не известно  | Частично реализовано | Работает | Не реализовано   
[TSpinEdit](<TSpinEdit.md> "TSpinEdit/ru") | Работает | Работает | Работает  | Работает | Работает | Не известно  | Не реализовано | Работает | Частично реализовано   
[TSplitter](<TSplitter.md> "TSplitter/ru") | Работает | Работает | Работает  | Работает | Работает | Не известно  | Частично реализовано | Работает | Не реализовано   
[TStaticText](<TStaticText.md> "TStaticText/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Работает   
[TStatusBar](<TStatusBar.md> "TStatusBar/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Не реализовано | Работает | Не реализовано   
[TToggleBox](<TToggleBox.md> "TToggleBox/ru") | Работает | Работает | Работает  | Работает | Работает | Частично реализовано  | Не реализовано | Работает | Не реализовано   
[TTrackbar](</index.php?title=TTrackbar/ru&action=edit&redlink=1> "TTrackbar/ru \(page does not exist\)") | Работает | Работает | Работает  | Работает | Работает | Работает  | Не реализовано | Работает | Работает   
[TTrayIcon](<TTrayIcon.md> "TTrayIcon/ru") | Работает | Работает | Работает  | Частично реализовано | Работает | Не реализовано  | Не реализовано | Работает | Не реализовано   
  
## Статус диалогов в интерфейсе LCL для каждой платформы

Компонент | win32 | gtk | gtk2 | carbon | qt | wince | fpgui | cocoa | customdrawn   
---|---|---|---|---|---|---|---|---|---  
LCLIntf.MessageBox | Работает | Работает | Работает  | Работает | Частично реализовано | Работает  | Работает | Работает | Реализовано для Android   
Application.MessageBox, MessageDlg, LCLIntf.PromptUser | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Реализовано для Android   
LCLIntf.AskUser | Работает | Работает | Работает  | Работает | Работает | Работает  | Не реализовано | Работает | Не реализовано   
[TColorDialog](<TColorDialog.md> "TColorDialog/ru") | Работает | Работает | Работает  | Работает | Работает | Не реализовано  | Работает | Работает | Не реализовано   
[TFontDialog](<TFontDialog.md> "TFontDialog/ru") | Работает | Работает | Работает  | Частично реализовано | Работает | Не реализовано  | Работает | Работает | Не реализовано   
[TOpenDialog](<TOpenDialog.md> "TOpenDialog/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Не реализовано   
[TPrinterSetupDialog](<TPrinterSetupDialog.md> "TPrinterSetupDialog/ru") | Работает | Работает | Работает  | Не реализовано | Работает | Не реализовано  | Не реализовано | Не реализовано | Не реализовано   
[TSaveDialog](<TSaveDialog.md> "TSaveDialog/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Не реализовано   
  
## Статус элементов управления на основе TCustomControl в интерфейсе LCL для каждой платформы

Обратите внимание, что будучи потомком TCustomControl не гарантирует, что элемент управления не имеет реализации widgetset. TArrow имеет его, хотя он имеет хорошую реализацию по умолчанию. TNotebook будет полностью реализована в LCL. 

Компонент | win32 | gtk | gtk2 | carbon | qt | wince | fpgui | cocoa | customdrawn   
---|---|---|---|---|---|---|---|---|---  
[TArrow](<TArrow.md> "TArrow/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Работает   
[TNoteBook](<TNotebook.md> "TNotebook/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Не реализовано | Работает | Работает   
[TUpDown](<TUpDown.md> "TUpDown/ru") | Работает | Работает | Работает  | Работает | Работает | Частично реализовано  | Работает | Работает | Частично реализовано   
[TStringGrid](<TStringGrid.md> "TStringGrid/ru") | Работает | Работает | Работает  | Частично реализовано | Работает | Частично реализовано  | Частично реализовано | Работает | Частично реализовано   
[TDrawGrid](<TDrawGrid.md> "TDrawGrid/ru") | Работает | Работает | Работает  | Частично реализовано | Работает | Не известно  | Частично реализовано | Работает | Частично реализовано   
[TToolBar](<TToolBar.md> "TToolBar/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Не реализовано | Работает | Не реализовано   
[TTreeView](<TTreeView.md> "TTreeView/ru") | Работает | Работает | Работает  | Частично реализовано | Работает | Работает  | Не реализовано | Работает | Не реализовано   
[TValueListEditor](<TValueListEditor.md> "TValueListEditor/ru") | Работает | Работает | Работает  | Частично реализовано | Работает | Работает  | Частично реализовано | Работает | Не реализовано   
  
## Статус элементов управления на основе TGraphicControl в интерфейсе LCL для каждой платформы

**Примечание:** Они предназначены для заворачивания в LCL компоненты, а **не** для конкретных самостоятельных функций GUI инструментария. 

Компонент | win32 | gtk | gtk2 | carbon | qt | wince | fpgui | cocoa | customdrawn   
---|---|---|---|---|---|---|---|---|---  
[TBevel](<TBevel.md> "TBevel/ru") | Работает | Работает | Работает  | Работает | Работает | Частично реализовано  | Работает | Работает | Не реализовано   
[TLabel](<TLabel.md> "TLabel/ru") | Работает | Работает | Работает  | Работает | Работает | Работает  | Работает | Работает | Реализовано для Android   
[TShape](<TShape.md> "TShape/ru") | Работает | Работает | Работает  | Частично реализовано | Работает | Частично реализовано  | Работает | Работает | Работает   
[TSpeedButton](<TSpeedButton.md> "TSpeedButton/ru") | Работает | Работает | Работает  | Работает | Работает | Не известно  | Работает | Работает | Работает   
[TPaintBox](<TPaintBox.md> "TPaintBox/ru") | Работает | Работает | Работает  | Работает | Работает | Не известно  | Работает | Работает | Работает   
[TImage](<TImage.md> "TImage/ru") | Работает | Работает | Работает  | Работает | Работает | Частично реализовано  | Работает | Работает | Работает   
  
## Статус LazDeviceAPIs на каждом widgetset

Компонент | customdrawn-android   
---|---  
Accelerometer | Работает   
Messaging (SMS, MMS and E-Mail) | Реализовано SMS   
PositionInfo | Работает   
  
Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<../en/Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<../en/Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<../en/Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<../en/Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<../en/Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<../en/Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<../en/Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<../en/Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<../en/Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<../en/Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<../en/Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<../en/Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<../en/Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<../en/Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<../en/Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<../en/Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<../en/Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<../en/Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<../en/Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<../en/Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<../en/Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<../en/Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<../en/Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<../en/Lazarus_3.md> "Lazarus 3.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<../en/User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<../en/User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<../en/User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<../en/User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<../en/User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<../en/User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<../en/User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<../en/User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<../en/User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<../en/User_Changes_3.md> "User Changes 3.0") | [3.0.2](<../en/User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<../en/User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<../en/User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<../en/User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<../en/User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<../en/FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<../en/FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<../en/FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<../en/FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<../en/FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<../en/FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<../en/FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<../en/FPC_New_Features_Trunk.md> "FPC New Features Trunk")

  


## Смотри также

  * [LCL Internals](<../en/LCL_Internals.md> "LCL Internals")
  * [TAChart Roadmap](<../en/TAChart_Roadmap.md> "TAChart Roadmap")

---

_Source: [https://wiki.freepascal.org/Roadmap/ru](https://web.archive.org/web/20241213023649/https://wiki.freepascal.org/Roadmap/ru)_
