# Lazarus 1.2.0 release notes

Lazarus 1.2.0 was released at March 2, 2014. 

Statistics: 

  1. commits: 5855
  2. log: svn log -r 36507:44302
  3. resolved bug tracker issues: 552



## Contents

  * 1 Minor releases
  * 2 LCL Interfaces Changes
  * 3 LCL Changes
  * 4 IDE Changes
  * 5 lazbuild
  * 6 Components
    * 6.1 TAChart
    * 6.2 TActiveX/LazActivex
    * 6.3 TDBImage
    * 6.4 TFBAdmin
    * 6.5 TFBEventMonitor
    * 6.6 TLvlGraphControl
    * 6.7 TPQTEventMonitor
    * 6.8 TSQLDBLibraryLoader
    * 6.9 TTIPropertyGrid,TTICustomPropertyGrid
    * 6.10 Command line utility to create closed source packages
  * 7 Changes affecting compatibility
    * 7.1 LCL incompatibilities
      * 7.1.1 LCLType INVALID_HANDLE_VALUE now Handle(-1)
      * 7.1.2 LCL WidgetSet Classes (wslclclasses unit)
      * 7.1.3 LCL TCustomForm.AddHandlerFirstShow,AddHandlerClose,AddHandlerCreate parameter AsFirst default value changed to false
      * 7.1.4 LCL TScreen.AddHandlerFormAdded, AddHandlerRemoveForm, AddHandlerActiveControlChanged, AddHandlerActiveFormChanged, AddHandlerFormVisibleChanged parameter AsFirst default value changed to false
      * 7.1.5 LCL TAction.OnExecute no longer sets TControl/TMenuItem.OnClick
      * 7.1.6 LCL TTreeView with Images no longer indents nodes without image
      * 7.1.7 TBitBtn.Spacing default changed
      * 7.1.8 LazUtils CreateRelativePath behaviour changed
      * 7.1.9 LazUtils TrimFilename behaviour changed
      * 7.1.10 LazFileUtils / FileUtil handling of pathdelimiters
      * 7.1.11 LCLType: fixed typo in constant declaration
      * 7.1.12 Since 1.2.4: TControlScrollBar.AutoCalcRange was removed
    * 7.2 IDE incompatibilities
      * 7.2.1 AllowDirectorySeparators no longer contains backslash under Unix
      * 7.2.2 IDEIntf TOICustomPropertyGrid.Favourites was renamed to Favorites
      * 7.2.3 Global SrcPath was removed
    * 7.3 Other release notes



# Minor releases

List of fixes since 1.2, including Lazarus 1.2.2: [Lazarus 1.2 fixes branch](<Lazarus_1.md> "Lazarus 1.2 fixes branch")

# LCL Interfaces Changes

  * TCMMouseWheel.WheelData is renamed to WheelDelta to better describe its purpose and for Delphi compatibility.
  * Qt: Implemented scrollable forms (TCustomForm.AutoScroll / TCustomForm.HorzScrollbar,TCustomForm.VertScrollBar).IDE must be rebuild with -dQTSCROLLABLEFORMS, or enable define QTSCROLLABLEFORMS in lcl/interfaces/qt/qtdefines.inc to use scrollable forms.



# LCL Changes

  * TMemo.WantReturns is implemented
  * TListView.OwnerDraw and OnDrawItem is implemented
  * TCustomListView.SelectAll, TCustomListView.ClearSelection and TCustomListView.AddItem are implemented for Delphi compatibility.



# IDE Changes

  * **Editor**
    * [ Macro Recorder/Player](<IDE_Window__Editor_Macros.md> "IDE Window: Editor Macros")
      * Multiple macros. Ability to select/change macro from list.
      * Individual key-shortcuts for each Macro
      * Macros can be saved per project or with the global IDE config
      * Package EditorMacroScripting: Allow use of [ PascalScript in macros](<Editor_Macros_PascalScript.md> "Editor Macros PascalScript").
    * Sticky selection mode
    * Select text word-wise
    * Highlight different (user specified) words in different colors [ See here](<IDE_Window__Editor_User_Defined_Words.md> "IDE Window: Editor User Defined Words")
    * Auto-continue for comments and strings
    * Inactive code in $IFDEF can be "low-lighted" (greyed out)
    * Markup/Highlight colors can be merged (alpha channel) and prioritised.
    * Added Popup menu / Source / Find references of used unit: Position cursor on a uses clause will find all references of this used unit within the active unit.
  * **Debugger**
    * Watches window: added detail view, for selected watch
    * Debug Inspector: Ability to directly input expression to monitor
    * Attach and Detach
    * Support for [remote debug](<http://wiki.lazarus.freepascal.org/Remote_Debugging#Using_gdbserver>) with gdbserver
  * When option "open last project" is enabled (default) and the IDE was closed with no project, no project will open at start. When a virtual (not saved) project was open a new Application project is created.
  * The [Package Graph](<IDE_Window__Package_Graph.md> "IDE Window: Package Graph") now uses the new TLvlGraphControl, showing all package dependencies including project and IDE.
  * Package Editor: new menu item to find and remove unneeded dependencies
  * **IDEIntf**
    * Added boolean parameter BringToFront (defaults to false) to TLazIDEInterface.DoShowSearchResultsView. It specifies whether Search Results dialog should be shown on top or under Source Editor. Behavior of existing code calling this method is not affected.
    * Added TPropertyEditor.GetVerbCount, GetVerb, PrepareItem, ExecuteVerb to let TPropertyEditors add menu items to the Object Inspector.
  * **Compiler options**
    * Build modes 
      * Build modes are not visible by default. They can be enabled with a checkbox.
      * The GUI is visually above the options GUI pages, thus indicating it has effect on them.
      * There is a separate window for managing the modes. Two useful modes, "Debug" and "Release", can be created with a single click.
    * GUI for all available options 
      * Options are read and parsed directly from FPC.
      * The "Other" page has a button which opens the generated GUI. The options are synchronized and saved with the custom options memo.
      * There is another button for selecting DEFINEs. All added DEFINEs are stored in project options, the selected ones are put to custom options.
      * Options in other pages have been rearranged
    * "IDE Macro Values" was replaced with a more powerful page "[Additions and Overrides](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options")". Here are some example that you can do now, which were not possible before: 
      * Append FPC options to packages without touching the lpk
      * Changing the output directory of packages without touching the lpk
      * Define IDE macros only for some packages or only for the project
      * Append compiler options for all projects with build mode "debug"
      * Change the package(s) output directory of all projects and packages with build mode "release"
      * Append compiler options of third party projects and packages without altering their sources
    * The page [Inherited](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options") was moved to the _Show options_ dialog.
  * The window [Unit Dependencies](<IDE_Window__Unit_dependencies.md> "IDE Window: Unit dependencies") was rewritten: 
    * It shows what units a unit uses
    * It shows what units uses that unit
    * Search and filter
    * Showing packages and projects as graphs
  * **Designer**
    * New designer popup menu item **Reset** for setting control bounds to their ancestor bounds
    * Add Frame onto a form: The dialog to select the frame ancestor now scans all sources.
  * The default test directory of the IDE under Linux was changed from /tmp to ~/tmp to avoid clashes with other users
  * Delphi converter is more robust and does not abort any more for parsing or other errors. The conversions are still primitive but they are done better.



# lazbuild

  * Added options --verbose, --quiet and -q to increase/decrease verbosity. You can pass them multiple times.
  * When compiling a project (.lpi) lazbuild now checks if the project needs compilation and if not will not call the compiler. It also now writes why it recompiles.



# Components

## TAChart

  * Series: 
    * Transparency
    * `AddArray`, `AddNull`, `AddX` and `AddY` functions, `SetText` procedure
    * `TFitSeries.IEquationText` interface
    * `TPolarSeries.CloseCircle` property
    * `TFuncSeries.ExtentAutoY` property
    * `TBarSeries.GetBarWidth` function
  * Tools: 
    * New tool: `TDataPointDistanceTool`
    * `EscapeCancels` property for dragging tools
    * `TDataPointHintTool`: `UseApplicationHint` property, `OnHintPosition` event
    * `TDataPointFragTool`: `Origin` property, `OnDrag`, `OnStartDrag` events
    * `TZoomDragTool.RestoreExtentOn` property
    * `TChartToolset.DispatchedShiftState` property
  * Sources: 
    * `TDbChartSource`: `Options=[dcsoDateTimeX, dcsoDateTimeY]` property, `OnGetItem` event
    * `TListChartSource`: `SetText`, `SetColor`, `AddXYList` procedures
    * New sources: `TCustomAnimatedChartSource`, `TCustomAxisChartSource`
  * Axes and marks: 
    * Axis index property editor
    * `TChartAxis`: `Position`, `Value`, `ValueCount` properties, `IsPointInside` function
    * Marks: `Arrow`, `Margins`, `Shape`, `CalloutAngle` properties, `OnGetShape` event
  * Misc 
    * `TAChartTeeChart` unit with some helpers for migration from TeeChart
    * `TChart`: `IsZoomed`, `PrevLogicalExtent`, `AutoFocus` properties
    * `TChartExtentHistory` utility class
    * 3 new demos: `lazreport`, `animate`, `distance`



## TActiveX/LazActivex

New package. 

The LazActiveX package contains the TActiveXContainer component and the IDE integration of the necessary tools to create ActiveX components from a type library or directly from the object (exe or dll). ActiveX is a Microsoft Windows technology and can only be used on that platform. Cross compilation to windows is possible but the typelib importer used to create the COM bindings works only on Windows (wine not tested). 

See [LazActiveX](<LazActiveX.md> "LazActiveX")

## TDBImage

Delphi compatible behaviour can now be set. 

Lazarus TDBImage by default saves information about the file type before the image data. This allows seamless use of various file formats (.jpg, .bmp,...). Delphi does not save information about the file type. 

To allow programmers to use TDBImage more flexibly, including supporting Delphi compatibility, two events to TDBImage were added: 
    
    
    TOnDBImageRead = procedure(Sender: TObject; S: TStream; var GraphExt : string) of object;
    TOnDBImageWrite = procedure(Sender: TObject; S: TStream; GraphExt : string) of object;
    

If the first event is defined then it is called instead of TDBImage reading from the stream and must return the file extension as GraphExt. For example: 
    
    
    procedure TMyForm.TDBImageDBImageRead(Sender: TObject; S: TStream;
      var GraphExt: string);
    begin
       GraphExt := 'jpg';
    end;
    

can be used when the type of the image is always JPEG. 

Similarly, if OnDBImageWrite is present then this is called instead of TDBImage writing the file extension to the start of the BLOB in the database. For example: 
    
    
    procedure TMyForm.PhotoDBImageWrite(Sender: TObject;
      S: TStream; GraphExt: string);
    begin
      if GraphExt <> 'jpg' then
        raise Exception.Create('Can only save a JPEG Image');
    end;
    

also works for an implicit JPEG. 

## TFBAdmin

This database component allows you to perform administrative actions such as: 

  * add, modify or delete database users
  * backup and restore databases using single or multiple (split) files
  * get general database information
  * get the database log file listing important database events and errors



for your Firebird (and probably Interbase) database. It works independently from **TIBConnection**. 

Please see [Firebird#Database_Administration](<Firebird.md> "Firebird") and the example program included with Lazarus for more information. 

As this is a thin wrapper over the FPC **TFBAdmin** component, please also see FPC documentation and examples for further details. 

## TFBEventMonitor

This database component allows you to get notified of Firebird (Interbase) events - generated from within the database by triggers. This is useful to reduce the frequency of polling/getting information from the database via your code. 

Please see [Firebird#Monitoring_Events](<Firebird.md> "Firebird") and the example program included with Lazarus for more information. 

As this is a thin wrapper over the FPC **TFBEventMonitor** component, please also see FPC documentation and examples for further details. 

## TLvlGraphControl

This new control is part of the package LazControls and used by the IDE itself as well. The control shows a graph consisting of nodes and edges as a level graph (this type of graph is also called a layered graph). 

[![packagegraph1.png](https://wiki.freepascal.org/images/7/78/packagegraph1.png)](</File:packagegraph1.png>)

TLvlGraphControl Features 

  * Offers automatic topological sorting, with an option to put nodes leftmost or rightmost
  * You can automatically combine edges. Long edges over multiple levels are combined into single lines with forks.
  * Provides a heuristic to minimize line crossings
  * Automatically prevents overlapping of display elements
  * Nodes can be displayed as rectangles, ellipses or icons stored in a TImageList
  * You can select nodes using the mouse
  * You can select multiple nodes at once
  * You can highlight nodes or edges under the mouse
  * The control displays back edges in special colours, which facilitates quick visual identification of cycles (e.g. unit circles).
  * Edges can be drawn straight or curved (bezier)
  * There is a header caption
  * Node captions can be shown to the left, to the right, above or below nodes
  * All colors can be set via published properties
  * There are events provided to custom-draw and/or skip each part of the default drawing



There is a simple example in examples/levelgraph/lvlgraph_dependencies.lpi. 

[![unitdependencies1.png](https://wiki.freepascal.org/images/8/8f/unitdependencies1.png)](</File:unitdependencies1.png>)

## TPQTEventMonitor

This database component allows you to get notified of PostgreSQL events - generated from within the database. This is useful to reduce the frequency of polling/getting information from the database via your code. 

As this is a thin wrapper over the FPC **TFBEventMonitor** component, please also see FPC documentation and example programs (e.g. pqeventstest.pp) for details. 

## TSQLDBLibraryLoader

This database component allows you to specify the exact client library filename to load for your SQLDB connection in the **LibraryName** property. This component is useful if your environment has non-standard client library names. 

You should set the library filename before enabling the TSQLConnection. 

As this is a thin wripper over the FPC **TSQLDBLibraryLoader** component, please see FPC documentation for further details. For now, see e.g. [TSQLDBLibraryLoader](<TSQLDBLibraryLoader.md> "TSQLDBLibraryLoader")

## TTIPropertyGrid,TTICustomPropertyGrid

Added event **OnEditorFilter** to let user define what properties to show. 

## Command line utility to create closed source packages

The command line utility can be compiled with: 
    
    
    components/codetools/examples/sourcecloser.lpi
    

It can remove implementations from Pascal units and can change lpk files to compile only manually and adds -Ur to the compiler options. Start it with -h to see all options. 

# Changes affecting compatibility

## LCL incompatibilities

### LCLType INVALID_HANDLE_VALUE now Handle(-1)

  * Old value: LCLType.Handle(0)
  * New value: LCLType.Handle(-1)
  * Effect: ported code from Delphi now works right, code that misused the constant now works wrong
  * Reason: This constant is a WinAPI constant and used for Windows functions like FileOpen and FileCreate as return value on failure. The cross-plaform FCL functions with the same name return the same value on failure. LCL didn't use this constant itself. It is defined in LCL to make porting Delphi code easier.
  * Remedy: Check your code if the INVALID_HANDLE_VALUE are from unit LCLType or from unit Windows. If the former, then check the code logic.



### LCL WidgetSet Classes (wslclclasses unit)

  * Old behaviour: VIRTUAL_VMT_COUNT const value was 100
  * New behaviour: VIRTUAL_VMT_COUNT const value increased to 128
  * Reason: Ability to add more methods to ws classes (eg. selectAll in TWSCustomListView), without increasing VIRTUAL_VMT_COUNT lcl crashes.
  * Remedy: unknown



### LCL TCustomForm.AddHandlerFirstShow,AddHandlerClose,AddHandlerCreate parameter AsFirst default value changed to false

  * Effect: old code might now be called after other handlers.
  * Reason: Consistency with other LCL AddHandler.
  * Remedy: If your event needs to be called earlier add parameter **true** to call.



### LCL TScreen.AddHandlerFormAdded, AddHandlerRemoveForm, AddHandlerActiveControlChanged, AddHandlerActiveFormChanged, AddHandlerFormVisibleChanged parameter AsFirst default value changed to false

  * Effect: old code might now be called after other handlers.
  * Reason: Consistency with other LCL AddHandler.
  * Remedy: If your event needs to be called earlier add parameter **true** to call.



### LCL TAction.OnExecute no longer sets TControl/TMenuItem.OnClick

  * Old behavior: When setting Action.OnExecute all associated OnClick events of TControl and TMenuItem were set as well. The OnClicks were not saved to lfm.
  * New behavior: When setting Action.OnExecute no other event is set. On click first the OnClick and then the Action.OnExecute is called.
  * Effect: Code that checks if OnClick is assigned instead of the Action.OnExecute fails.
  * Reason: Allow to set separate OnClick and OnExecute events, consistency between lfm and Object Inspector.
  * Remedy: Check logic of all lines containing "Assigned(OnClick)".



Double clicking on a component with an Action will now automatically use the Action.OnExecute event. That means the IDE will create the event if needed and jump to implementation in the Source Editor. 

### LCL TTreeView with Images no longer indents nodes without image

  * Old behaviour: When the Images of a TTreeView was set, all nodes without ImageIndex were still painted indented as if they had images
  * New behavior: Nodes without a valid ImageIndex are not indented
  * Effect: Custom drawn TreeView may now paint at wrong place
  * Reason: The empty space looked like a bug
  * Remedy: Adapt your custom drawn code. Or set an ImageIndex and set the PaintImages parameter to false. Then the nodes are indented, but the icons are not painted.



### TBitBtn.Spacing default changed

  * Effect: default Spacing is changed from 3 to 4 px
  * Reason: Delphi compatibility
  * Remedy: TBitBtn look



### LazUtils CreateRelativePath behaviour changed

  * Old behaviour: if a relative path could be constructed, this was sometimes appended with a pathdelimiter, and sometimes not.
  * New behaviour: no pathdelimiter is appended.



### LazUtils TrimFilename behaviour changed

  * Old behaviour: TrimFilename on Windows changed all '/' into '\' and removed double PathDelimiters.
  * New behaviour: If a filename starts with '\\\?\' (Extended Length naming scheme to allow for filenames > MAXPATH characters), then the filename will not be altered. (Since the code that does this has been moved to the ResolveDots function it affects o.a. ExpandFilenameUTF8)
  * Reason: [MS specifications on filenames](<http://msdn.microsoft.com/en-us/library/windows/desktop/aa365247%28v=vs.85%29.aspx>)



### LazFileUtils / FileUtil handling of pathdelimiters

  * Old behaviour: several functions only handled PathDelim.
  * New behaviour: these functions now use AllowDirectorySeparators. This affects a.o. FindDiskFilename, FindDiskFileNaecaseInsensitive, ExtractFileNameWithoutExt, TryCreateRelativePath, ForceDirectory, FilenameIsTrimmed, AppendPathDelim, ChompPathDelim, MinimizeSearchPath, FindPathInSearchPath, ForceDirectoriesUTF8, ExtractFileRoot, IsUNCPath, ExtractUNCVolume, FilenameIsWinAbsolute, ResolveDots, TrimFileName, ExpandFilenameUtf8.
  * Reason: compatibility with FreePascal RTL handling of pathdelimiters.
  * Remedy: two new functions were added to LazFileUtils (ForcePathDelims and GetForcedPathDelims) that change all '\' into '/' (or vice versa, depending on OS) regardless of AllowDirectorySeparators.



### LCLType: fixed typo in constant declaration

  * Old behaviour: one of the MB_XXXX constants was wrongly named MB_ICONASTERICK
  * New behaviour: the constant now is named MB_ICONASTERISK
  * Reason: Compliance with MS standards, Delphi compatibility
  * Remedy: Use MB_ICONASTERISK instead of MB_ICONASTERICK in calling code (it is used as a parameter in Windows.MessageBox function)



### Since 1.2.4: TControlScrollBar.AutoCalcRange was removed

  * This incompatibility was introduced in 1.2.4 to fix a design flaw.
  * Effect: compiler error: There is no method in an ancestor class to be overridden: "TYourControlScrollBar.AutoCalcRange;"
  * Reason: Code for calculating the _AutoScroll_ ranges was moved from the two scrollbars to the new proteced method TScrollingWinControl.CalculateAutoRanges. This prevents an endless loop when the two scrollbars depend on each other.
  * Remedy: Override TYourControl.CalculateAutoRanges instead.



## IDE incompatibilities

### AllowDirectorySeparators no longer contains backslash under Unix

  * Effect: Functions ExtractFileName, ExtractFilePath, ExtractFileDir no longer treats backslashes as separator. Functions DoDirSeparators and SetDirSeparators no longer changes backslashes in file names to slashes. This affects only IDE plugins, **not** LCL applications in general.
  * Reason: Backslash is a valid character in Unix file names.
  * Remedy: If you need to change slashes and backslashes to the default path delimiter, use the functions ForcePathDelims and GetForcedPathDelims from unit LazFileUtils.



### IDEIntf TOICustomPropertyGrid.Favourites was renamed to Favorites

  * Effect: old code might fail to compile
  * Reason: Consistent American spelling
  * Remedy: Rename used identifier



### Global SrcPath was removed

The Option _Tools / Options / CodeTools / General / Additional sources_ was removed. 

  * Effect: IDE can not find an unit, while compiler can
  * Reason: This option was added as workaround for various shortcomings of the IDE and was often confused. For example the IDE was not able to find pas/pp files in the unit paths defined by the fpc.cfg. The IDE can now find such files and has now some options to setup source paths globally.
  * Remedy: Add the path to _Project Options / Paths / Other Sources_ or to the _Additions and Overrides_.



## Other release notes

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | 1.2 | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/Lazarus_1.2.0_release_notes](https://web.archive.org/web/20250219022319/https://wiki.freepascal.org/Lazarus_1.2.0_release_notes)_
