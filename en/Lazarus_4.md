# Lazarus 4.0 release notes

**Lazarus 4.0 is not yet released. This page is under construction!**

## Contents

  * 1 Release
  * 2 LCL Interfaces Changes
    * 2.1 Cocoa
  * 3 LCL Changes
    * 3.1 TTaskDialog
      * 3.1.1 Support for Queries
      * 3.1.2 New events
      * 3.1.3 New properties
      * 3.1.4 Support for messages
    * 3.2 TCalendarDialog
    * 3.3 TColorDialog
    * 3.4 TOpenDialog
    * 3.5 TShape
    * 3.6 TLabeledEdit
    * 3.7 TTimeEdit
    * 3.8 TFilenameEdit
    * 3.9 Control.ChildSizing
    * 3.10 AutoLineReduction property for TMenu and TMenuItem
  * 4 IDE Changes
    * 4.1 IDE
    * 4.2 Compiler Options
    * 4.3 Editor
    * 4.4 Debugger
    * 4.5 Sample EditMask files in property editor for TMaskEdit.EditMask
    * 4.6 Code Templates
    * 4.7 Pas2js
    * 4.8 Icon Finder
    * 4.9 Incompatible IDE changes
      * 4.9.1 Editor Settings
  * 5 IDE Interface Changes
  * 6 Components
    * 6.1 TAChart
    * 6.2 TTIPropertyGrid aka TCustomPropertiesGrid
    * 6.3 Test Insight
    * 6.4 Minimap
  * 7 Changes affecting compatibility
    * 7.1 LCL incompatibility
      * 7.1.1 TTaskDialog
      * 7.1.2 TShellTreeView, TShellListView
      * 7.1.3 TFilenameEdit.InitialDir
      * 7.1.4 TSpeedButton.AutoSize
      * 7.1.5 Debugln/Dbgs in LCLProc (deprecated)
    * 7.2 Components incompatibility
      * 7.2.1 LazUtils
      * 7.2.2 TAChart
      * 7.2.3 SynEdit (in user apps)
      * 7.2.4 Debugger
  * 8 Other release notes



## Release

  * There are now Debian packages for **Raspi4+**.


  * The rpm packages were renamed to **fpc-laz** , **fpc-src-laz** and **lazarus-project** in order to avoid conflicts with packages of distributions.


  * The macOS release now uses the fpc 3.2.2 universal binary installer and a zip file containing the fpc sources and the prebuilt Lazarus. There is one zip file for **Intel Macs** and one for **Apple M1+ Macs**.



## LCL Interfaces Changes

### Cocoa

  * TListView completely reimplemented and significantly improved. currently it supports all ViewStyles(vsReport/vsIcon/vsSmallIcon/vsList), OwnerDraw/CustomDraw, MultiSelection, CheckBoxes.


  * Scrollers for custom control like SynEdit completely reimplemented and significantly improved. currently it supports Legacy and Overlay style with the visual effects highly similar to native Scrollers.


  * Modern Form Style implemented by NSToolBar. it supports more native Form appearance and functionality on macOS 11+, see also #41118


  * Lookup Word (Control+Command+D) supported in FullEditControl like SynEdit.


  * ShowBalloonHint reimplemented by UserNotifications to conform the conventions on macOS.


  * Cocoa becomes more configurable through unit CocoaConfig.



## LCL Changes

### TTaskDialog

#### Support for Queries

  * Because the unit LCLTaskDialog has been deprecated, the query capabilities of LCLTaskDialog.TTaskDialog have been added to Dialogs.TTaskDialog, albeit with a slightly different implementation. 
    * New flag tfQuery: adds a query (in the form of a combobox) to the dialog. 
      * New property QueryChoices (TStrings): sets the predefined answers.
      * New property QueryItemIndex: sets the ItemIndex of the combobox and holds said ItemIndex when the dialog is closed.
      * New flag tfQueryFixedChoices: control whether or not the user can add his/hers own text to the combobox.
      * New property QueryResult: holds the text of the choice made when the dialog closes.
    * New flag tfSimpleQuery: adds a single line Edit to the dialog. Note: this flag is mutually exclusive with the tfQuery flag. 
      * New property SimpleQuery: sets the initial text of the Edit.
      * New property SimpleQueryPassWordChar: sets PassWordChar of the single line query editor.
      * Property QueryResult holds the text of the Edit when the dialog closes.



#### New events

The following (most of them Delphi compatible) events were implemented: 

  * OnDialogConstructed
  * OnDialogCreated
  * OnDialogDestroyed
  * OnVerificationClicked. This also updates the Flags property to reflect the current state of the verification checkbox.
  * OnExpanded. The associated new property Expanded only has meaning in the context of this event.
  * OnTimer
  * OnRadioButtonClicked
  * OnHyperlinkClicked (Windows Vista+ native dialog only). The associated new property URL only has meaning in the context of this event.
  * OnNavigated (Windows Vista+ native dialog only). This event only fires if the dialog has pages (see: [MS docs](<https://learn.microsoft.com/en-us/dotnet/api/system.windows.forms.taskdialogpage.navigate?view=windowsdesktop-7.0>)). Pages are not implemented in TTaskDialog, but they might be implemented in a derived class. The event handler must however be in the base class for Delphi compatibility.
  * OnHelp. Does not exist in Delphi (even though Delphi does have a protected method DoOnHelp). Triggers when user presses F1 when the dialog is open.



#### New properties

  * CollapseButtonCaption: controls the caption of the Expand/Collapse button when the dialog is in expanded state.
  * CustomMainIcon and CustomFooterIcon: icons to be displayed in the dialog (instead of the standard icons) when the flags tfUseHIconMain and tfUseHIconFooter are used.
  * ProgressBar: the progressbar on the dialog. You can set it's properties Position, Min, Max, MarqueeSpeed (native Vista+ only) and State (native Vista+ only). When the dialog is shown, you can send messages (e.g. in OnDialogXXX events) to alter these properties.



#### Support for messages

  * Support for sending messages (via SendMessage) to the dialog has been added to the emulated dialog. This allows a.o. to enable/disable (radio)buttons, click (radio)buttons and update captions (Title, Text, FooterText and ExpandedText)



### TCalendarDialog

  * Added `FirstDayOfWeek` as a new publishced property since it has been present in `TCalendar` but could not be accessed easily.



### TColorDialog

  * Added Delphi compatible property Options (ATM only implemented in Win32/Win64 WS)



### TOpenDialog

  * New enums for TOpenOptionEx, mainly for Windows Vista+ specific options (but may be extended for other widgesets in the future)



### TShape

  * New public method `function PtInShape(const P: TPoint): Boolean` to determine whether the specified point P lies within the shape boundaries.
  * New event `OnShapeClick` firing when a click on the control occurs within the shape boundaries
  * The control can be selected in the form designer only by clicking somewhere within the shape. Clicking on controls covered by the TShape can still be selected when the click is outside the shape boundary.
  * New event `OnShapePoints`, together with the new `TShapeType` element `tsPolygon`, gives the user the opportunity to define an arbitrary polygon for the component.



### TLabeledEdit

  * The published `EditLabel.Layout` property could not be changed due to anchoring. The anchor was removed, and `EditLabel.Layout` now can be adjusted when the label is at the left or right side of the edit.
  * New property `EditLabel.Alignment` to adjust horizontal position of the label when it is above or below the edit.



### TTimeEdit

  * New properties `TimeFormat` and `TimeSeparator` for more flexible formatting of the time output.



### TFilenameEdit

  * Old behaviour: InitialDir was unchanged after user executed the dialog.
  * New behaviour: if the dialog is executed, InitialDir is set to the directory of the dialog (even if the dialog is cancelled).
  * Reason: consistency with the behaviour of TOpenDialog.
  * Commit: 4c4c9631



### Control.ChildSizing

  * Added crsSameSize for sizing all columns/row to have each an equal share of the available space.



### AutoLineReduction property for TMenu and TMenuItem

  * A Delphi compatible property AutoLineReduction has been added to TMenu(Item). It controls wether leading, trailing or consecutive menu-spearators are shown.
  * If you change the property programatically, you should call the RethinkLines method for the TMenuItem in question. RethinkLines returns True if the visibility of any separator was changed, otherwise it returns False.



## IDE Changes

### IDE

  * The IDE now comes with built-in docking and docked-form editor (they can be toggled as option, rather than rebuilding IDE to install/remove packages). When run for the first time the user will be prompted with a choice to enable them.



### Compiler Options

  * Added project compiler option for [SubTarget](<FPC_Subtarget_Support.md> "FPC Subtarget Support").
  * Added IDE macro [$(FPCTarget)](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames"), a short form of _$(TargetCPU)-$(TargetOS)-$(SubTarget)_.
  * Added compiler option **Write config instead of command line parameters (@)**. [See here](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options"). Useful for passing large amount of paths to the compiler or an easier way to use the Lazarus options in your build scripts.
  * Added predefined variable **Laz_FullVersion** to conditionals. [See here](<Macros_and_Conditionals.md> "Macros and Conditionals"). Useful to define Lazarus version dependent command line parameters.
  * Wildcards * and ** in search paths. [See here](<IDE_Window__Project_Options.md> "IDE Window: Project Options").



### Editor

  * Added elastic tabs
  * Pas-Highlighter: added color for Proc/Var modifiers
  * Gutter 
    * Configurable padding between parts for improved readability
    * Configurable Width (e.g. space for more marks breakpoint, bookmark and error/warning)
    * Highlight of current line in gutter
    * Highlight of inner-most fold section in fold-tree gutter
  * Syncro-Edit 
    * Added key-combos for matching words case-sensitive or insensitive. Added key-combos for matching words according to their "context" (e.g. don't group code with strings or comments)
    * Added ability to add custom cells from selection
    * Added ability to resize cells
  * File -> "Close All" is gone. 
    * It was misleading and confusing because it closed only the source editor windows, not the whole project nor the package editors. People thought it does the same thing as "Close All" in Delphi but it didn't. You can close all editor windows with Window -> "Manage Source Editors ...". There "Check All" and "Close All Checked". You can also right click an editor tab and select "Close All Other Pages". It leaves one editor tab which usually is what you want. That one can be closed separately if really needed.



### Debugger

  * Alt + Left-Mouse Gutter click / Ctrl-Alt-F5: Add breakpoint (if not yet set) and open the properties of that breakpoint.
  * Color config for assembler window (in the drop-down list with languages ​​of the IDE Options category _Editor > Display > Colors_)
  * Breakpoint window can group breakpoints by "group". Change group by drag and drop
  * Breakpoint "HitCount" now acts like "Condition". If the HitCount is not yet reached the breakpoint is skipped (all actions are skipped, previously only "break/pause" was skipped, but logging and enable/disable others was executed). See <https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40676>
  * Run Param (Windows only): Allow to specify the default pos/size for console/gui window. [IDE Window: Run parameters#Default_Window/Console_position_and_size](<IDE_Window__Run_parameters.md> "IDE Window: Run parameters")
  * Run Param: Redirect StdIn/Out/Err (FpDebug and "Run without debug" on Linux/Windows)
  * "Debug visualizers" / Value-Formatter [IDE Window: IDE Options - Debugger Value-Formatter](<IDE_Window__IDE_Options_-_Debugger_Value-Formatter.md> "IDE Window: IDE Options - Debugger Value-Formatter")
    * TDateTime
    * TColor / TAlphaColor
    * Currency
    * Ordinal to named constant
  * New "DisplayFormats" for watches 
    * Values in arrays and structures are now following the DisplayFormat
    * Global (and Project) Defaults for Watch-DisplayFormats. (Including global settings for Hints, and Locals)
  * New intrinsic: [FpDebug-Watches-Intrinsic-Functions](<FpDebug-Watches-Intrinsic-Functions.md> "FpDebug-Watches-Intrinsic-Functions")
    * Flatten: for linked-list, trees, ..
    * Try/TryN: find (and use) first expression that does not cause an error during evaluation)
    * i2o: Interface2Object, show the object that implements the current instance of the interface (Only Windows/Linux / Only Intel/AMD)
    * Ord: ordinal value
    * Log, Pi, Ln, Sqrt, Sin,Cos,Tan: -
    * A ? B : C => inline if operator
  * Registers Window 
    * FPU (on Win-32/64 and Linux-64)
    * Xmm (on Win-32/64 and Linux-64)
    * Ymm (on Win-32/64)
  * New Memory Viewer Window



### Sample EditMask files in property editor for TMaskEdit.EditMask

  * If you load a sample EditMask file in the property editor, the IDE now reloads this file when you invoke the property editor again
  * You can unload a sample file if you don't want to have it loaded anymore.
  * The property editor supports Delphi sample files (*.dem) as well as Lazarus sample files (*.lem), the latter having a more relaxed syntax.
  * Lines in a sample file which do not properly represent a sample editmask are ignored.
  * For the time being, Lazarus does **not** distribute sample editmask files, mostly because of possible copyright issues.



### Code Templates

These are the templates you get by a shortcut `Ctrl`+`J` in the editor. 

  * File containing default templates `lazarus_dci_file.dci` was renamed to `codetemplates.dci`. This is an internal lazarus file and does not affect custom templates, which are still located in the configuration folder.
  * The class `TSynAutoComplete` (actually `TCustomSynAutoComplete`) interface was changed to support ordering the list of templates properly. This class is probably used only for the `Ctrl`+`J` templates and not for any user code. If it is used somewhere, then the code must be adjusted.



### Pas2js

  * The Pas2jsDsgn package now adds a new menu item under **Tools / Install/Update pas2js** , which allows to download the latest release, unpacks it and sets the paths for pas2js and the simple web server. So with a few mouse clicks Lazarus is ready for web development.



### Icon Finder

  * [Icon Finder](<Icon_Finder.md> "Icon Finder") is a new IDE plug-in for easy searching for icons by means of keywords.
  * Used by the TImageList and TPicture component editors.



### Incompatible IDE changes

#### Editor Settings

  * Mouse Settings: "Right button click includes caret move" now also affects the "Context menu (debug)". If you wish to have different settings for the "full context menu" and the "Context menu (debug)" then you can make those changes in the advanced mouse options
  * Color schemes may need updates for "modifiers" => See SynEdit compatibility changes.



## IDE Interface Changes

## Components

### TAChart

  * **TBubbleSeries** : New `BubbleSizeUnits` options `bruPercentageRadius` and `bruPercentageArea` \- the bubble size values are interpreted as bubble radius or bubble area. The bubble radius then is drawn as a percentage of the plot area size where the largest bubble corresponds to `BubbleRadiusPercentage`.
  * **TChartSeries** : New event **OnGetMarkText** , similar to **OnGetMark** , but with additional parameters (series, y index and x index of the data point) for specifying individual data point labels in multi-values series. **OnGetMark** has been deprecated and will be removed when v5.0 is released
  * **TLineSeries** : New `LineType`s `ltStepCenterXY` and `ltStepCenterYX`, available in LibreOffice Calc.
  * **TPieSeries** , **TBubbleSeries** , **TBarSeries** allow controlling all brush and pen parameters of individual data points by means of the **Styles** property.
  * Chart sources can provide **multi-level data-point labels** separated by the **LabelSeparator**.
  * **Chart.AxisPosition** now can be given in axis units (**cuAxis**). For performing the correct axis transformation, the index of the orthogonal axis containing the `Position` value must be specified, **OrthogonalAxisIndex**.
  * **Chart styles** were extended to support individual series pointers per y level.
  * The **Chart.Legend** allows the value `ColumnCount = 0` now which automatically determines orientation and fill order of the legend, depending on the `Alignment` property.



### TTIPropertyGrid aka TCustomPropertiesGrid

  * Added **ExtVisibility** property on fpc 3.3.1: Adds properties from extended RTTI. For example when set to `[vcPublic]` it adds the public properties.
  * **TRecordPropertyEditor** : a property editor for record types. 
    * At the moment it supports only field accessors, aka no _getter_ or _setter_ method, for example


    
    
    type
      {$RTTI EXPLICIT FIELDS([vcPublic])}
      TWing = record
        Size: integer;
        Feathered: boolean;
        Color: TColor;
      end;
    
      {$RTTI INHERIT PROPERTIES([vcPublic, vcPublished])}
      TBird = class(TComponent)
      private
        FWing: TWing;
      public
        property Wing: TWing read FWing write FWing;
      end;
    

### Test Insight

The FPCUnit package now also contains support for the [Test Insight](<fpcunit.md> "fpcunit") extension to FPCunit, allowing the test results to be displayed directly in the IDE, and clicking on a test and/or an exception in the displayed results will take you directly to the method. 

### Minimap

New IDE addon [LazMinimap](<LazMinimap.md> "LazMinimap") shows the current source right or left zoomed out, allowing to quickly visually scroll in the source. 

## Changes affecting compatibility

### LCL incompatibility

#### TTaskDialog

  * The implementation of TTaskDialog has been refactored. 
    * Widgetset specific implementation has been moved to the widgetset.
    * The emulated taskdialog has been moved to a new unit: TaskDlgEmulation.
    * The unit LCLTaskDialog has been deprecated.
    * Remedy: if you use TTaskDialog from (now deprecated) unit LCLTaskDialog, use TTaskDialog from Dialogs unit instead.
  * Execute() now passes the handle of the active form instead of 0. 
    * Reason: it makes Execute() honour the tfRelativeToWindow flag.
    * Remedy: if your code relied on passing 0 as the ParentWind parameter use Execute(0) instead.
  * The occurrence of '\n' in e.g. captions is no longer converted to a LineFeed. 
    * Reasons: 
      * Wrong results if the capion of a (radio)button is set to e.g. 'Save in "c:\new_folder\new.work"'
      * You can have a LineFeed (#10) inside a Pascal string.
    * See also [Issue #38676](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/38676>).
    * Remedy: use LineFeed character in captions where applicable.



#### TShellTreeView, TShellListView

  * The `ReadOnly` property of these controls has been changed to be `true` by default. 
    * Reason: These controls, by default, do not propagate changes of their item/node captions to the file system. The old behaviour was confusing because the user was able to rename an item/node but had to notice that the change was not persistent without additional code.
    * Remedy: Applications using the shell controls only for selecting files and folders are not affected. In "file-manager" application types, however, the `ReadOnly` property must be changed explicitely to `false` so that the user can edit items/nodes again.



#### TFilenameEdit.InitialDir

  * Previous behaviour: InitialDir was not updated after the dialog was executed.
  * New behaviour: InitialDir will be updated after the dialog was executed. This also happens if the user cancels the dialog.
  * Reason: consistency with the behaviour of TOpenDialog.
  * Remedy: include ofNoChangeDir in DialogOptions
  * Commit: 4c4c9631



#### TSpeedButton.AutoSize

  * Previous behaviour: Active AutoSize of a themed TSpeedButton could truncate the speedbutton's Caption.
  * New behaviour: The autosized speedbutton now is larger by a few pixels. In some cases, this can lead to misaligned speedbuttons in existing components/applications.
  * Reason: Fixing the AutoSize behaviour by adding 6 pixels to the PreferredWidth/Height.
  * Remedy: in case of TSpeedButton descendants: override the CalculatePreferredSize method and reduce the preferred size by 6 pixels; in case of applications, use anchoring to align the speedbutton borders.



#### Debugln/Dbgs in LCLProc (deprecated)

  * Debugln/Dbgs functions in LclProc are now deprecated.
  * Remedy: Use variants from LazLogger / LazLoggerBase



### Components incompatibility

#### LazUtils

  * Function **IsNumber** was poorly named because it suggests to check the provided string for being a valid number, but only checks for numeric characters omitting e.g. negative and hex or binary values. Renamed too **IsNumeric** and deprecated the old function (to be removed in v4.99).



#### TAChart

  * Utility functions **PointDist** and **PointLineDist** have been marked as being **deprecated** and replaced by **PointDistSq** and **PointLineDistSq** since they return the square of the distance rather than the distance itself.



#### SynEdit (in user apps)

  * The default Keystrokes for indent/unindent changed from **Shift-Ctrl I/U** to **Ctrl I/U**   
This affects only TSynEdit newly added to forms. Existing TSynEdit will load the old keystrokes. Action is only needed if TSynEdits are added to apps with other existing TSynEdit, and should be in sync. <https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40810>
  * **TSynPasSyn** The Pascal Highlighter now has a separate attribute for "modifiers" (such as stdcall, overload, virtual, deprecated, ...). 
    * Those were previously highlighted with the attribute for keywords **property TSynPasSyn.KeyAttri**
    * The now have there own attribute **property TSynPasSyn.ModifierAttri** (defaults to _bold_)



    As a result, if custom color setting were made for KeyAttri, and those should be continued to be applied to modifiers too, then the ModifierAttri must be set accordingly

#### Debugger

SHA-1: 665bae1dd1afd290b53df5ae903ce3f2f85587e0, Branch RevNo: 67414 

  * The behaviour of "HitCount" for breakpoints changed. (It now acts more like "Condition") 
    * Old behaviour: If HitCount was not reached, the the breakpoint would not enter pause. However the breakpoint would do all other configured actions: Enable/Disable other breakpoint-groups, log to eventlog, take snapshots.
    * New behaviour: If HitCount is not reached, the breakpoint will not perform any of those actions.



  
<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40676>

## Other release notes

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | 4.0

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch") | [4.0](<Lazarus_4.md> "Lazarus 4.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/Lazarus_4.0_release_notes](https://web.archive.org/web/20250512065207/https://wiki.freepascal.org/Lazarus_4.0_release_notes)_
