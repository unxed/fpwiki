# Lazarus 1.8.0 release notes

Statistics: 

  1. commits: 3980
  2. log: svn log -r 50542:56591 (<https://svn.freepascal.org/svn/lazarus/branches/fixes_1_8>)



Fixes branch: [Lazarus 1.8 fixes branch](<Lazarus_1.md> "Lazarus 1.8 fixes branch")

## Contents

  * 1 LCL Interfaces Changes
  * 2 LCL Changes
  * 3 IDE Changes
  * 4 IDE Interfaces Changes
  * 5 Components
    * 5.1 LazControls
    * 5.2 ToDoListLaz
    * 5.3 TAChart
    * 5.4 EditorToolbar
  * 6 Changes affecting compatibility
    * 6.1 LazUtils
      * 6.1.1 Wrappers for string functions are disabled
      * 6.1.2 FindDefaultExecutablePath searches on Windows in BaseDir
    * 6.2 LCL incompatibilities
      * 6.2.1 1.8.4: TToolBar children ignore Align
      * 6.2.2 TLCLPlatform definition was moved
      * 6.2.3 TToolButton.OnArrowClick added
      * 6.2.4 TCustomAbstractGroupedEdit descendants must override GetRealText/SetRealText methods
      * 6.2.5 TFontStyle enum item order is changed to match with Delphi
      * 6.2.6 THintWindow transparent for mouse messages by default
      * 6.2.7 TCustomTabControl setting TabIndex or PageIndex by code
      * 6.2.8 TCustomControlFilterEdit.UseFormActivate deprecated
      * 6.2.9 DPI value change for all Mac OSX widget sets
      * 6.2.10 TDateTimePicker.OnCheckBoxChange Sender parameter change
      * 6.2.11 PCheckComboItemState not found
    * 6.3 Components incompatibilities
      * 6.3.1 easylazfreetype: TCustomFreeTypeFontCollection.AddFile
    * 6.4 IDE incompatibilities
      * 6.4.1 Form resource strings table LRT format support is dropped, LRJ format introduced instead
    * 6.5 DateTimeCtrls
      * 6.5.1 DateTimeCtrls split into DateTimeCtrls and DateTimeCtrlsDsgn packages
    * 6.6 LazControls
      * 6.6.1 Register procedures moved into LazControlDsgn package
  * 7 Previous release notes



# LCL Interfaces Changes

  * [Qt5](<Qt5_Interface.md> "Qt5 Interface"): new widgetset based on Qt5 libraries.



# LCL Changes

  * [TApplication](<TApplication.md> "TApplication")
    * Scaled property: the LCL is DPI-aware. Please see [High DPI](<High_DPI.md> "High DPI") for more information.


  * [TCustomForm](</index.php?title=TCustomForm&action=edit&redlink=1> "TCustomForm \(page does not exist\)")
    * new position poWorkAreaCenter - center form on working area (depends on DefaultMonitor)
    * new compiler define LCLRealFormBounds. If you compile your application with LCLRealFormBounds on, the TCustomForm bounds (Width/Height) will be real outer bounds and not client bounds. Also constraints will affect real bounds. For now, it is supported only by win32.
    * Scaled property: the LCL is DPI-aware. Please see [High DPI](<High_DPI.md> "High DPI") for more information.


  * TImage 
    * new event OnPaintBackground.
    * new properties StretchInEnabled, StretchOutEnabled.
    * new properties KeepOriginXWhenClipped, KeepOriginYWhenClipped.


  * TTreeView 
    * implemented Tooltips property.
    * implemented MultiSelectStyle property.


  * [TToolButton](<TToolButton.md> "TToolButton")
    * new style tbsButtonDrop - paints a drop down arrow on the right side of the button. In contrast to tbsDropDown the button has only single clicking area that invokes the DropDownMenu if assigned.
    * new event OnArrowClick. It is called if Style=tbsDropDown when the user clicks the arrow button area of toolbutton. Breaking change: In 1.6. the OnClick event was called for this!


  * [TToolBar](<TToolBar.md> "TToolBar")
    * new DropDownWidth property to setup the width of the toolbutton arrow area for Style=tbsDropDown.


  * ClipBoard 
    * The ClipBoard now supports copying and pasting text in HTML format by means of the methods GetAsHtml and SetAsHtml.


  * Menus ([TPopupMenu](<TPopupMenu.md> "TPopupMenu"), [TMainMenu](<TMainMenu.md> "TMainMenu"), [TMenuItem](</index.php?title=TMenuItem&action=edit&redlink=1> "TMenuItem \(page does not exist\)")) 
    * Menus now support OwnerDraw with (almost) Delphi's approach. If you set TMenu.OwnerDraw to true the OnMeasureItem and OnDrawItem from TMenu and TMenuItem will be called. OnDrawItem from Lazarus has the same parameters as OnAdvancedDrawItem from Delphi. You can use the events also from TMenu, which is an LCL addition to Delphi. For now only win32 is supported.


  * Dialogs 
    * [TTaskDialog](<TTaskDialog.md> "TTaskDialog") added


  * [TDateTimePicker](<TDateTimePicker.md> "TDateTimePicker")
    * new [TDateTimePicker.Options](<DateTimeCtrls_Package.md> "DateTimeCtrls Package") property



# IDE Changes

  * [lazbuild](<lazbuild.md> "lazbuild")
    * new option --add-package-link to register given lpk files without building.


  * [Quick Fixes](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools")
    * new QuickFix for any fpc hint, note or warning: add compiler directive {$warn id off}


  * [Code creation](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools")
    * new shortcut `Ctrl`+`⇧ Shift`+`X` that shows a CodeCreation dialog with more options. `Ctrl`+`⇧ Shift`+`C` never shows a dialog now.
    * You can choose the default visibility of the methods in Tools / Options / Codetools / Class Completion / Default section of methods.


  * Word+Identifier completion: If the option Codetools / Identifier Completion / Replace whole words is enabled, pressing Return replaces the whole word at cursor, pressing Shift+Return replaces only the left side. If the option is disabled it works the other way round: Return replaces left side, Shift+Return replaces whole word.


  * "case ofall" code template indentation change 
    * Old behavior: the created case-list was always flat
    * New behavior: by default it is now indented according to source editor options.
    * Reason: the indented case-list is Delphi-standard
    * Remedy: edit the ofall template and use the WithoutExtraIndent parameter: "$OfAll(WithoutExtraIndent)"


  * Ctrl-Enter alias "Open file at cursor" was improved when cursor in a Pascal file: It now recognizes properly $I and $R directives, uses nodes, string literals and comments.


  * DPI awareness: the IDE is DPI-aware (it's ongoing work, so expect issues). Please see [High DPI](<High_DPI.md> "High DPI") for more information.


  * pas2js support: 
    * parsing the external class declaration of pas2js, code completion, find declaration, etc.
    * namespaces: The IDE supports the -NS option and project default name spaces of Delphi/pas2js. As soon as FPC supports namespaces Lazarus supports it too.


  * Editor 
    * New [colored outline of source structure](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools"). (like cnpack, if then block outline)


  * Support for Win98/WinNT has been dropped.



# IDE Interfaces Changes

  * New function LazarusIDE.FindUnitsOfOwner listing all units of a project/package. [[1]](<http://wiki.lazarus.freepascal.org/Extending_the_IDE#All_units_of_current_project>)



# Components

## LazControls

  * Added [TSpinEditEx](<TSpinEditEx.md> "TSpinEditEx") and [TFloatSpinEditEx](<TFloatSpinEditEx.md> "TFloatSpinEditEx") a widgetset independant implementation of [TSpinEdit](<TSpinEdit.md> "TSpinEdit") resp. [TFloatSpinEdit](<TFloatSpinEdit.md> "TFloatSpinEdit") with some extended capabilities: 
    * NullValue property.
    * Configurable behaviour when the text inside the control is not a number.



## ToDoListLaz

  * Now searches all used units of projects. Excluding used packages, including all units listed in the project inspector.



## [TAChart](<TAChart.md> "TAChart")

  * New component [TChartCombobox](<TAChart_documentation.md> "TAChart documentation")
  * Improved data point tools (property Target)
  * New demos (runtime demos, datapointtools)



## EditorToolbar

  * The package editortoolbar.lpk is removed and its functionality is integrated in IDE. 
    * The settings can be found in Tools -> Options -> Environment -> Editor Toolbar



# Changes affecting compatibility

## LazUtils

### Wrappers for string functions are disabled

Various units in LCL, LazUtils and Codetools have wrappers for string functions in LazUTF8 and LazFileUtils. They are now disabled. They can still be enabled by defining "EnableWrapperFunctions". 

The wrapper functions were already deprecated in Lazarus 1.6 with a proper hint about where to find the actual function. This allowed a transition period for everybody who updates to every major Lazarus version. 

### FindDefaultExecutablePath searches on Windows in BaseDir

  * Applies: Non Unix systems.
  * Old behavior: BaseDir was only searched if '.' was in %PATH% - same as Unix shells.
  * New behavior: BaseDir is always searched (if set) on non Unix.
  * Reason: Windows console searches in current directory so Windows users expect to find executable in base directory.
  * Remedy: Use instead SearchFileInPath(Executable,BaseDir,GetEnvironmentVariableUTF8('PATH'),PathSeparator,[sffDontSearchInBasePath]);



## LCL incompatibilities

### 1.8.4: TToolBar children ignore Align

Controls placed within TToolBar cannot be aligned with the Align property. 

  * Old behavior: The Align property of TToolBar children was taken into account.
  * New behavior: The Align property is ignored.
  * Reason: It caused endless alignment loops when the control's size was changed by code in Create.
  * Remedy: use a parent container for TToolBar and align the controls within this container.



### TLCLPlatform definition was moved

TLCLPlatform was moved from InterfaceBase to a new LCLPlatformDef unit in r53711. Also a related LCLPlatformDisplayNames was moved from LazConf to the same unit. 

  * Old behavior: Unit InterfaceBase was used for TLCLPlatform type.
  * New behavior: Unit LCLPlatformDef must be used for it. InterfaceBase may still be needed for other types.
  * Reason: The GUI independent parts of LCL definitions will be needed for command line programs (LazBuild).
  * Remedy: use the right units in your uses-sections.



### TToolButton.OnArrowClick added

  * Applies: event called if Style=tbsDropDown when the user clicks the arrow button area of toolbutton.
  * Old behavior: OnClick was called.
  * New behavior: OnArrowClick is called.
  * Reason: there was no easy way to detect click on the arrow sub-button.
  * Remedy: use the same event handler for OnArrowClick and OnClick.



### TCustomAbstractGroupedEdit descendants must override GetRealText/SetRealText methods

  * Old behavior: TCustomAbstractGroupedEdit reintroduced the Text property
  * New behavior: TCustomAbstractGroupedEdit uses TControl.Text
  * Reason: unnecessary 2 different properties with the same name and function
  * Remedy: override GetRealText/SetRealText instead of GetText/SetText.



### TFontStyle enum item order is changed to match with Delphi

  * Old behavior: the last item was fsUnderline
  * New behavior: the last item is fsStrikeOut
  * Reason: Delphi compatibility.
  * Remedy: check code if it depends on Ord(TFontStyle) - like "StyleStrings: Array[TFontStyle] of string = ('Bold', 'Italic','StrikeOut','Underline');" or saving/retrieving font style as ordinal value.
  * More info: It does not affect LFM streaming. It can make a difference when iterating items.



### THintWindow transparent for mouse messages by default

  * Old behavior: [THintWindow](<THintWindow.md> "THintWindow") received mouse messages
  * New behavior: [THintWindow](<THintWindow.md> "THintWindow") passes mouse message through itself to the underlying form/control.
  * Reason: Delphi compatibility.
  * Remedy: to make [THintWindow](<THintWindow.md> "THintWindow") clickable again, add a LM_NCHITTEST message handler to your hint window descendant and return HTCLIENT as message result:


    
    
      procedure WMNCHitTest(var Message: TLMessage); message LM_NCHITTEST;
      // ...
      procedure TMyHintWindow.WMNCHitTest(var Message: TLMessage);
      begin
        Message.Result := HTCLIENT;
      end;
    

### TCustomTabControl setting TabIndex or PageIndex by code

  * Old behavior: setting TabIndex or PageIndex by code caused an OnChange event to be fired
  * New behavior: setting TabIndex or PageIndex by code no longer causes an OnChange to be fired
  * Reason: Delphi compatibility.
  * Remedy: set nboDoChangeOnSetIndex in Options (this option is not available in Delphi), call OnChange in you own code when changing TabIndex/PageIndex, or move the code in OnChange to another event if applicable.



### TCustomControlFilterEdit.UseFormActivate deprecated

  * Old behavior: setting UseFormActivate to true, the filter edit got focus on Form.Activate
  * New behavior: UseFormActivate does nothing and will be removed after 1.8
  * Reason: it globally misused the TForm.OnActivate event.
  * Remedy: set ActiveControl in Form.DoCreate or use correct TabOrder or similar.



### DPI value change for all Mac OSX widget sets

  * Old behavior: all DPI/PPI properties had value of 72 on all Mac OSX widget sets.
  * New behavior: Screen.PixelsPerInch and TMonitor.PixelsPerInch change values from 72 to 96 on Mac. The value of GetDeviceCaps(MyDC, LOGPIXELSX) still returns 72.
  * Reason: 100% scaling on Windows and Linux system is 96 DPI. If Mac uses 72 DPI for 100% the following points are a problem: 
    * the Font.Height / Font.Size ratio is different from Win/Linux. (Setting Font.Size := 10 results in different font height on Mac and Win/Linux.)
    * automatic LCL High-DPI methods scale controls down on Mac.
  * Remedy: If you develop applications for the Mac, check your code for any "Font.Size := ValueGreaterThanZero" assignments. You have to adjust them in ratio 72/96 to receive the same font height as for LCL 1.6. E.g. "Font.Size := 12" has to be changed to "Font.Size := 9". Font.Height assignments don't need to be adjusted.



### TDateTimePicker.OnCheckBoxChange Sender parameter change

  * Old behavior: Sender was the TCheckBox object within TDateTimePicker
  * New behavior: Sender is the TDateTimePicker object
  * Reason: Do not expose private field TCheckBox to public methods.
  * Remedy: If you need the TCheckBox object in OnCheckBoxChange handler, you can get it from the Controls array property.



### PCheckComboItemState not found

  * Old behavior: TCustomCheckCombo Items.Objects[] were PCheckComboItemState
  * New behavior: TCustomCheckCombo Items.Objects[] are now TCheckComboItemState
  * Reason: when Items.Objects[] was accidentally overwritten or an item was added wrong, there was a crash, even possible X freeze. Now it checks for such a mistake and creates a normal exception.
  * Remedy: Use TCustomCheckCombo instead.



## Components incompatibilities

### easylazfreetype: TCustomFreeTypeFontCollection.AddFile

  * Old behavior: return Boolean
  * New behavior: return TCustomFontCollectionItem
  * Reason: there was no easy possiblity to work with the added item.
  * Remedy: don't check the result for True, but for "<>nil".



## IDE incompatibilities

### Form resource strings table LRT format support is dropped, LRJ format introduced instead

  * Old behavior: Form resource strings were extracted into LRT file when saving form.
  * New behavior: Form resource strings are extracted into LRJ file with format unified with RSJ files which are emitted by FPC 3.0.0 and up. LRT support is dropped.
  * Reason: LRT file format was broken by design, e.g. it didn't handle multiline captions properly (see bug 26553 for details).
  * Remedy: Open your project and click "Project" -> "Resave forms with enabled i18n" menu item, then rebuild your project. After this you can safely remove LRT files.



## DateTimeCtrls

### DateTimeCtrls split into DateTimeCtrls and DateTimeCtrlsDsgn packages

  * Old: one run/design time package DateTimeCtrls
  * New: runtime package=DateTimeCtrls, designtime package=DateTimeCtrlsDsgn
  * Reason: Consistency, separate run and design time code
  * Remedy: DateTimeCtrls components get unregistered. You have to install DateTimeCtrlsDsgn into the IDE.
  * Info: Don't link DateTimeCtrlsDsgn into your application.



## LazControls

### Register procedures moved into LazControlDsgn package

  * Old: register procedures were in the LazControls package although there was a LazControlDsgn package
  * New: moved register procedures into LazControlDsgn package
  * Reason: Consistency, separate run and design time code
  * Remedy: the components get unregistered. You have to install LazControlDsgn into the IDE.
  * Info: LazControlDsgn is a [designtime](<Lazarus_Packages.md> "Lazarus Packages") package.



# Previous release notes

  * [Lazarus 1.6.0 release notes](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes")
  * [Lazarus 1.4.0 release notes](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes")
  * [Lazarus 1.2.0 release notes](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes")
  * [Lazarus 1.0 release notes](<Lazarus_1.md> "Lazarus 1.0 release notes")



Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | 1.8 | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/Lazarus_1.8.0_release_notes](https://web.archive.org/web/20241226155225/https://wiki.freepascal.org/Lazarus_1.8.0_release_notes)_
