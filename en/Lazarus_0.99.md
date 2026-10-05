# Lazarus 1.0 release notes

Statistics: 

  1. commits: 7872
  2. resolved bug tracker issues: 487



## Contents

  * 1 LCL Interfaces Changes
  * 2 LCL Changes
    * 2.1 Known Issues
  * 3 LazUtils Changes
  * 4 IDE Changes
  * 5 Components
    * 5.1 Codetools
    * 5.2 TAChart
    * 5.3 SQLDB
    * 5.4 ChmHelpPkg
  * 6 Changes affecting compatibility
    * 6.1 Installer / Upgrading from 0.9.26 or older on MS Windows
    * 6.2 TCustomForm.Create raises an exception if resource is not found
    * 6.3 LM_DEACTIVATE message is removed
    * 6.4 TCompileReason or TLazCompilerOptions not found
    * 6.5 Two packages which share their unit output directory give "unit not found", because the IDE deletes the ppu files
    * 6.6 SourceEditorIntf
    * 6.7 SynEdit
      * 6.7.1 SynEdit: Changes to WantTabs
      * 6.7.2 SynEdit: Changes to Options, MouseOptions
      * 6.7.3 SynEdit: Changes to MouseActions
      * 6.7.4 SynEdit: BeginUpdate adds UndoBlock
      * 6.7.5 SynEdit: Several deprecated methods were removed
      * 6.7.6 SynEdit: MouseLinkColor and the underline
      * 6.7.7 SynEdit: TSynCustomFoldHighlighter
      * 6.7.8 SynEdit: TSynCompletion, TSynAutoCompletion and TSynEditAutoComplete
    * 6.8 TCustomNotebook was moved from ExtCtrls to ComCtrls
    * 6.9 TCustomNotebook was renamed to TCustomTabControl
    * 6.10 TCustomPageControl/TPageControl.OnPageChanged were removed
    * 6.11 Support for the shift states ssCaps, ssNum and ssScroll was removed from OnMouseWheel.
    * 6.12 TDBEdit use EditMask property of the linked Field.
    * 6.13 ENTER Key behavior on String Editor
  * 7 Other release notes



## LCL Interfaces Changes

  * Gtk2: Implemented LCLIntf.CreateEllipticRgn and CreateRoundRectRgn and implemented SetWindowRgn for TForm (previously it worked only for child controls).
  * WinCE: Implemented TSelectDirectoryDialog.
  * A new interface was added called LCL-CustomDrawn which implements all native controls via the Lazarus Custom Drawn Controls. This interface has a minimal number of native elements and implements TCanvas drawing through the non-native TLazCanvas, TBitmap through TLazIntfImage, etc. This is the first Lazarus interface to support Android. It also supports Windows, Mac OS X (Cocoa) and X11.
  * Qt: implemented full MDI support.



## LCL Changes

  * The LCL is now a normal package. It is now automatically recompiled if needed. For example when switching the target platform and you have not yet compiled for this platform. This has no impact on existing projects.
  * The platform independent parts of the LCL are now in the package LCLBase. This has no impact on existing projects.
  * The linker options needed by the LCL are now **only** added to projects that use the package LCL. Formerly all projects got them. See [here](<The_LCL_in_various_platforms.md> "The LCL in various platforms").
  * TCustomForm.Create raises an exception if a form resource is not found.
  * TNotebook and TPage: The new implementations of these classes have been added.
  * TDBNavigator: It is now possible to have focusable buttons by setting Options = [navFocusableButtons] and TabStop = True. This is useful for accessibility and for devices with neither mouse nor touch screen.
  * The names of TControlBorderSpacing.GetSideSpace and GetSpace were swapped and are now consistent. GetSideSpace = Around + GetSpace.
  * TForm.WindowState=wsFullscreen was added
  * TCanvas.TextFitInfo was added to calculate how many characters will fit into a specified Width. Useful for word-wrapping calculations.
  * TControl.GetColorResolvingParent and TControl.GetRGBColorResolvingParent were added. They simplify the work of obtaining the final color of the control while resolving clDefault and the ParentColor.
  * LCLIntf.GetTextExtentExPoint now has a good default implementation which can work in any platform which does not have a specific implementation for it. The specific implementations are still better, if they can be supported by the widgetset.
  * TTabControl was reorganized. Now it has the correct class hierarchy and inherits from TCustomTabControl as it should.
  * New unit in the LCL: lazdialogs.pas. This unit has non-native versions of various native dialogs, for example TLazOpenDialog, TLazSaveDialog, TLazSelectDirectoryDialog. It is utilized by widgetsets which either do not have a native dialog, or do not wish to use it because it is limited. These dialogs can also be utilized by user applications directly.
  * New unit in the LCL: lazdeviceapis.pas. This unit offers an interface to more hardware devices such as the accelerometer, GPS, etc. See [LazDeviceAPIs](<LazDeviceAPIs.md> "LazDeviceAPIs")
  * New unit in the LCL: lazcanvas.pas. LazCanvas is a TFPImageCanvas descendent which implements drawing in a LCL-compatible way, but 100% in Pascal, doing all the drawing itself.
  * New unit in the LCL: lazregions.pas. LazRegions is a wholly Pascal implementation of regions for canvas clipping, event clipping, finding in which control of a region tree one an event should reach, for drawing polygons, etc.
  * New LCL units: customdrawncontrols.pas, customdrawndrawers.pas, customdrawn_common.pas, customdrawn_android.pas and customdrawn_winxp.pas. These are the [Lazarus Custom Drawn Controls](<Lazarus_Custom_Drawn_Controls.md> "Lazarus Custom Drawn Controls") -controls which imitate the standard LCL ones, but with the difference that they are non-native and skinnable.
  * New APIs were added to the LCL to improve our support for accessibility software such as screen readers. Read more in [LCL Accessibility](<LCL_Accessibility.md> "LCL Accessibility")



### Known Issues

  * Accessibility support in TCustomTreeView might make this control slower if there is a very large number of items, for example, 10.000+ items. If the performance impact is unacceptable it is possible to turn accessibility off for treeview items by setting TCustomTreeView.AccessibilityOn to false. The default value is true.
  * Ubuntu's Unity liboverlay library does not work well with the LCL. See [here](<GTK2_Interface.md> "GTK2 Interface")



## LazUtils Changes

  * LazUtils and the LCL now contain a 100% Pascal implementation of text rendering which can read TrueType fonts and draw crisp texts with ClearType anti-aliasing. This library is called [LazFreeType](<LazFreeType.md> "LazFreeType") and is almost entirely in LazUtils except for one unit, the drawer unit of the LCL.



## IDE Changes

  * The old warnings at IDE start for Lazarus directory, compiler and FPC sources were replaced with single dialog. You can force this dialog to show with the command line parameter **\--setup**.
  * A new project now saves the session information (open files, caret positions, etc) as separate lps file in the project directory. You can change this in Project / Project Options / Session.
  * The IDE no longer asks whether changes should be saved, if only the session data has changed (such as the cursor positions) and the session is saved in the separate lps file. You can enable the confirmation in Tools / Options / Desktop / Auto Save. See [[here](<http://wiki.lazarus.freepascal.org/IDE_Window:_Environment_Options#Auto_Save>)].
  * Compiler options: You can now use the project build macros in the search paths, linker and custom options of the project and all packages of the project. See the [examples](<Macros_and_Conditionals.md> "Macros and Conditionals").
  * The package editor and project inspector were improved to better support big packages/projects with hundreds of files: You can now sort, filter and show the directory structure.
  * Packages compiled via Makefile created by the IDE are now recognized by the IDE and only recompiled if something has changed.
  * Package general and package compiler options are now using the generic IDE Options interface.
  * New dialog to [clean up build files](<IDE_Window__Clean_up_build_files_dialog.md> "IDE Window: Clean up build files dialog"), including all project's packages.
  * Cleaner clean up when compiling packages: The IDE now deletes all files in the unit output directory of a package unless the directory contains sources or is shared with another package ([See packages](<Lazarus_Packages.md> "Lazarus Packages")).
  * Open File: filter list is now configurable in Tools / Options / Environment / [File Filters](<IDE_Window__Environment_Options.md> "IDE Window: Environment Options")
  * Export to html
  * Editor: 
    * Codefolding: Folded code remains folded, even if it temporarily becomes part of a comment (due to an open "(*" or "{"). Note: Such folds are not saved in the session, they are once they returned to there normal state, after the closing "*)"/"}" was typed.
    * Codefolding: Ability to hide (fold) the currently selected text. These folds are not saved to the session
    * Improved Mouse wheel support: Allows to zoom the editor (ctrl wheel) and slower/faster scrolling (shift/alt). Keys can be reconfigured.
    * Basic Macro-Recorder. (Only one Editor can record at a time, Macro can then be played to any editor)
    * Info-Hint, showing the current class/procedure on top of the editor, if it would otherwise be scrolled out
  * Debugger 
    * Added display format for CPU-Register
    * Added Pascal Source to Disassembler
    * Added breakpoints, (display and toggle) to Disassembler
    * Added Thread dialog
    * Added Watchpoints (data breakpoints)
    * History of Watches and Locals [[see blog](<http://lazarus-dev.blogspot.com/2011/05/remember-remember-history-of-debugging.html>)]
    * Added logging of most of events (Event Log)
    * Added ability for a breakpoint (breaking or none-breaking) to enable/disable other breakpoints
    * Watch/Inspect the an object as the instantiated class.
  * Main menu structure got revamped. Source menu and its Refactor sub-menu are new.
  * New menu item to insert MIT license text
  * Filters were added for many ListBoxes and TreeViews in the IDE, including IDE Options, Project Options (visible pages filtered by option name), Package and Project Inspector, Install / Uninstall packages, and many others. Next version will have a filter even in Object Inspector.
  * "Use Unit" (Alt-F11) dialog allows selecting from all available units, not only project units.
  * [Parameter Hints](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools") now have a button to paste the missing parameter names.
  * Code Explorer: Filter now deletes nodes, instead of marking.
  * Code Explorer: New category "Surrounding" allows to quickly jump to interface, implementation and nested procedures.
  * Help for keywords: The chmhelp package can now show help for keywords.
  * New window: Tools -> "Example Projects"
  * Window: View -> Components was improved and it can be opened from Component palette's local menu
  * Configure Build Lazarus window was changed. LCL, other packages and IDE don't need their own selections any more. Examples were moved to a new window.
  * startlazarus now supports the --pcp parameter allowing multiple installations of Lazarus.
  * [lazbuild](<lazbuild.md> "lazbuild")
    * can now create/update the Makefile of a package with the new option --create-makefile.
    * \--build-ide= now builds under Windows lazarus.exe, not lazarus.new.exe
    * \--build-ide= now appends the options instead of replacing.
    * \--build-mode= can now be used together with --build-ide to select the build profile of the IDE.
  * **Design time only packages** are no longer compiled into projects, unless enabled. See [here](<Lazarus_Packages.md> "Lazarus Packages").
  * New package type **Runtime only, can not be installed in IDE**. Use this to for package incompatible with the LCL or the IDE. See [here](<Lazarus_Packages.md> "Lazarus Packages").
  * When creating a new package the option to [Add to the uses section of the project](<IDE_Window__Package_Options.md> "IDE Window: Package Options") is now disabled by default.
  * New menu item to quickly create a new component: Package / New component.
  * New macros: **PrimaryConfigPath** , **SecondaryConfigPath** , **FallbackOutputRoot** , **PkgName**. See [IDE Macros](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames")
  * The IDE interface unit **projectintf** was split into projectintf and **compoptsintf**. The unit compoptsintf contains the access to the various compiler options of the IDE. The unit packageintf now grants access to the compiler options of packages.
  * New projects can now be created with your own set of build modes, for example "debug" and "release". For this a feature was extended: Project / Project Options / Use these options as default for new projects.
  * The compiler option -Wpunitname, specifying the controller unit for embedded systems, is now used when doing a "find declaration". You can give this option under compiler options / other.
  * Project title can now contain macros
  * Packages have no longer compiler setting for TargetOS, TargetCPU, LCLWidgetType. They use the project settings. Or when compiling the IDE they use the IDE settings. Or when compiled via lazbuild they use the command line parameters.
  * Building the IDE with the IDE or lazbuild: The backup of the old Lazarus executable now works the same on all platforms. Before building the old Lazarus executable is now renamed to lazarus.old.exe. If there is already a lazarus.old.exe it will be deleted. If it can not be deleted it is renamed to lazarus.old2.exe. If even this fails a lazarus.new.exe is created. If building fails the lazarus.old.exe is renamed back to lazarus.exe,
  * When putting a new frame on a form the IDE now allows to select from frames in packages.
  * A dialog was added to see which package/project needs rebuilding and why: View / IDE Internals / What needs Building
  * Pasting multiple components on a designer form now keeps references between them



## Components

### Codetools

  * Parsing, searching and code completion of **class operators** and **advanced records**.
  * Parsing **objcclass external** syntax.
  * Parsing and class completion of **nested classes**. Searching is not yet fully implemented.
  * Parsing of class sections **type** , **const** , **var** , **class var**. Searching is not yet fully implemented.
  * Parsing **delphi style generics**. Searching in spezialized types is not yet fully implemented.
  * Parsing **jvm** syntax.
  * ctnClassType, ctnClassConst, ctnClassVar were removed and replaced with ctnTypeSection, ctnConstSection, ctnVarSection.
  * The class sub sections ctnTypeSection, ctnConstSection, ctnVarSection are now children of the visibility sections.
  * New IDE package for advanced Codetools: [Cody](<Cody.md> "Cody").
  * Find declaration: "inherited;"
  * Identifier completion: 
    * for compiler directives: keywords and FPC macros.
    * IDE option to replace only the prefix instead of the whole identifier (Tools / Options / Codetools / Identifier Completion / Replace whole identifier).
    * more keywords for programs, libraries and properties
  * Code completion: 
    * now creates variables from pointer parameters, e.g. _CreateIconIndirect(@IconInfo)_ will create _var IconInfo:TIconInfo;_. See [here](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools").
    * now creates procedures/methods from parameters, e.g. _List.Sort(@MySortFunction)_ will create the new function _MySortFunction(Item1, Item2: Pointer): Integer_. See [here](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools").



### TAChart

  * Wiki [documentation](<TAChart_documentation.md> "TAChart documentation")
  * On{Before|After}Draw{BackWall|Background}, OnExtentChanged events for TChart
  * Antialiasing
  * TChartAxis: 
    * MarkPositions to implement labels-inside bars and pies
    * **Minor axis** ticks and grid
    * AxisPen and Range properties
    * Intervals property to control **interval size**
    * Margin property
  * TChartAxisMarks: 
    * Stripes property to create "striped" chart background
    * YIndex property to implement labels for stacked series
    * Range property
  * TFuncSeries now correctly work with rotation and axis transformation
  * **5 new series** : 
    * Open-High-Low-Close, B-spline, Cubic spline, Polar, Fit
  * 3D pie series, TPieSeries.EdgePen property
  * Multi-line mark labels, title and footer rotation
  * **6 new tools** : 
    * TDataPointClickTool, TDataPointHintTool, TDataPointCrosshairTool, TZoomMouseWheelTool, TPanMouseWheelTool, TUserDefinedTool
    * TBasicPanTool.LimitToExtent property
  * Drag-zoom and crosshair (former reticule) now should work on Qt and MacOS
  * KeyDown/KeyUp/MouseWheel events for chart tools
  * New transformations: TCumulNormDistrAxisTransform, TUserDefinedAxisTransform
  * New data source: TIntervalChartSource
  * TCalculatedChartSource: derivative and smooth derivative, AccumulationDirection
  * Series can now skip NaN values
  * New IChartDrawer API with various **drawing back-ends** : 
    * AggPas (separate package)
    * Printer, for printing charts with appropriate scaling (separate package)
    * OpenGL
    * FPCanvas/NoGUI, for using in web servers
    * SVG
    * BGRABitmap (separate package)
    * Windows Metafile (separate package)
    * FPVectorial (separate package)
  * Navigation components: TChartNavScrollBar, TChartNavPanel
  * Legend: 
    * Item format
    * Multiple columns, fill direction
    * Ordering and grouping
    * SymbolFrame property
    * OnCreate event
  * New components: TChartListbox, TChartExtentLink, TChartImageList
  * Enumerators
  * Arrow property for TConstantLine and TChartAxis
  * TChart.MarginsExternal and ExtentSizeLimit properties
  * Design-time editor for TListChartSource.DataPoints
  * **17 new demo projects** , many updates to existing demos
  * Backwards-incompatible changes: 
    * Adding Pie series does not automatically hide axises any longer
    * TZoomDragTool.Proportional property is replaced by RatioLimit, allowing single-axis zooming
    * TReticuleTool, TChart.ReticuleMode, TChart.ReticulePos and TChart.OnDrawReticule are deprecated and will be removed, use TDataPointCrosshairTool instead



### SQLDB

  * Added mssql connectors (supports Microsoft SQL Server), requires fpc 2.6.1+
  * Added sybase connectors (supports Sybase ASE), requires fpc 2.6.1+
  * Added mysql 5.5 connector, requires fpc 2.6.1+



### ChmHelpPkg

  * HelpFilesPath can now contain macros and multiple directories separated by semicolon



## Changes affecting compatibility

### Installer / Upgrading from 0.9.26 or older on MS Windows

As of 0.9.28 the Windows IDE no longer saves options in the exe/installation directory. Installers for 0.9.28 and 0.9.30 asked before overwriting config in the installation directory, allowing the IDE to find such old config files and copy them to the new location. 

As of 1.0 the installer will always overwrite such files without asking. 

If you upgrade from 0.9.26 or before you are required to backup your old config, and restore it after installation. (The IDE will still copy it to the new location) 

Background: As of 0.9.28 for any new installation (non update) the installer wrote a template config to the same location used by 0.9.26. The 1.0 installer now assumes that if the file exists, it is the template. Therefore it does not ask. 

### TCustomForm.Create raises an exception if resource is not found

  * **Reason** : Each form created by the IDE should have a resource, so if resource is not found there is something wrong with either a resource or the unit which contains the faulty form. Therefore the application should inform the developer that the form can't function correctly without the resource. This change is also Delphi compatible and compatible with TFrame and TDataModule components.
  * **Remedy** : If you need a resourceless form you have 3 options: 
    * Create a TForm class (not a descendant)
    * Construct your form using the CreateNew() constructor.
    * It is also possible to disable the exception by setting the global variable RequireDerivedFormResource to False.



[See also](<http://lists.lazarus.freepascal.org/pipermail/lazarus/2011-February/060229.html>)

### LM_DEACTIVATE message is removed

  * **Reason** : Both LM_ACTIVATE and LM_DEACTIVATE had Active field which have WA_ACTIVE, WA_INACTIVE flags. Also this is more VCL compatible solution.
  * **Remedy** : Move LM_DEACTIVATE code to the LM_ACTIVATE handler inside a check of Message.Active = WA_INACTIVE, Move LM_ACTIVATE code inside a check for Message.Active <> WA_INACTIVE



### TCompileReason or TLazCompilerOptions not found

  * **Reason** : The unit projectintf was split. The declarations for the IDE compiler options are now in the unit compoptsintf. The unit packageintf needed access to the compiler options.
  * **Remedy** : Add unit **compoptsintf** to the uses section.



### Two packages which share their unit output directory give "unit not found", because the IDE deletes the ppu files

  * **Reasons:** When a package is compiled clean and the unit output directory of a package is not shared by sources nor dependent packages the IDE now deletes all files in the output directory. This solves the frequent "unit not found" / "checksum changed" problem of renamed/moved units and other left garbage created by upgrades and experiments.
  * **Remedy:** Change the unit output directory of one those packages OR create a dependency between the two packages OR set one of the two packages to update manually (Package Editor / Options / IDE Integration).



### SourceEditorIntf

SrcEditorIntf.SourceEditorWindow
    removed, see [0.9.30 release notes](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes"), use SrcEditorIntf.SourceEditorManagerIntf

### SynEdit

#### SynEdit: Changes to WantTabs

The default for WantTabs changed from False to True. If set to "False" Tab and Shift-Tab are now ignored. 

The old behaviour was that SynEdit would always take the Tab key, if it was configured in the KeyStrokes (which by default it is). Only if WantTabs was False, and Tab removed from the Keystrokes, then the Tab was passed on to the Form (change Focus) 

The new behaviour is, that if WantTabs is False then SynEdit will not take the Tab key, even if in KeyStrokes. If WantTabs is True SynEdit will always handle the Tab key (do nothing, if not in KeyStrokes) 

  


#### SynEdit: Changes to Options, MouseOptions

All mouse related flags from the property Options were moved to MouseOptions. These are: 
    
    
     eoAltSetsColumnMode, eoDragDropEditing, eoRightMouseMovesCursor, eoDoubleClickSelectsLine, eoShowCtrlMouseLinks
    

The new flags start with "em". 

The old flags still exists (but should be considered deprecated). 

  * **Changes** to the old values are forwarded to the new MouseOptions
  * Setting Options, but keeping the (all/some) mouse related flags as they currently were in Options (even if different from MouseOptions) will **not** update MouseOptions.
  * Setting a value in MouseOptions to true, will not update Options.
  * Clearing a value in MouseOptions (set to false) will update (clear) Options. But only if it was set in MouseOptions before (changed).



Old code should work as before, no changes needed. As soon as MouseOptions is used (write access), code may need to be adapted. 

#### SynEdit: Changes to MouseActions

A new flag was introduced: emUseMouseActions im SynEdit.MouseActions 

Custom MouseActions are now only used, if this is set. 

Changes required: If you use MouseActions different from the flags in MouseOptions (was Options), them enable this flag. 

#### SynEdit: BeginUpdate adds UndoBlock

  * **This changes do not affect the usage of SynEdit via the designer**



Calling SynEdit.BeginUpdate, now includes creating an undo-block. This means all changes made during the update will be one entry in the undo list. 

Changes required: If the old behaviour is required, call SynEdit.BeginUpdate(False); the EndUpdate does not require any change, it adapts automatically. 

#### SynEdit: Several deprecated methods were removed

  * **This changes do not affect the usage of SynEdit via the designer**



SynEdit.SetSelWord
    removed, use SynEdit.SelectWord
SynEdit.CtrlMouseActive
    removed, the property was a dummy property for some time with no value. Information can be retrieved via TSynEditMarkupCtrlMouseLink(SynEdit.MarkupByClass[TSynEditMarkupCtrlMouseLink]).IsMouseOverLink
SynEdit.GutterWidth
    removed, use SynEdit.Gutter.Width
SynEdit.CFDividerDrawLevel
    removed, only worked with SynPasSyn. See TSynPasSyn for configuration
SynEditHighlighter.DividerDrawLevel
    removed
SynEditLines(SynEdit.Lines).DosFileFormat
    removed, use FileLineEndType and FileWriteLineEndType
TSynEditMarkList (Bookmarks)
    First / Last / Place / Insert have been removed, use Items[] / Add. Insert has no replacements, as bookmarks are now always sorted, and inserts at arbitrary positions no longer possible
TSynEditMarkList (Bookmarks)
    GetMarksForLine, removed, use Line[n] property

#### SynEdit: MouseLinkColor and the underline

SynEdit no longer automatically underlines a link with MouseLinkColor.Foreground. 

Instead MouseLinkColor.Foreground is only used for the text foreground-color. To get the underline the following should be added/configured: 
    
    
       MouseLinkColor.FrameColor := MouseLinkColor.Foreground;
       MouseLinkColor.FrameStyle := slsSolid;
       MouseLinkColor.FrameEdges := sfeBottom;
       MouseLinkColor.FramePriority := 9999;  // make sure it is on top of all other frames
    

#### SynEdit: TSynCustomFoldHighlighter

  * **This changes do not affect the usage of SynEdit via the designer**



    This may affect users who wrote their own highlighter *fold* support (inherited from TSynCustomFoldHighlighter)
    This may affect users, who access such a highlighter from their code, to retrieve fold specific info.

FoldNodeInfo and FoldNodeInfoCount 

    Now available via Highlighter.FoldNodeInfo[LineIdx].CountEx([]) and Highlighter.FoldNodeInfo[LineIdx].NodeInfo(Index, [])

The following methods where removed: 

    function MinimumFoldLevel(Index: Integer): integer; virtual; abstract;
    function EndFoldLevel(Index: Integer): integer; virtual; abstract;

The following methods where deprecated: 

    function FoldOpenCount(ALineIndex: Integer; AType: Integer = 0): integer; deprecated;
    function FoldCloseCount(ALineIndex: Integer; AType: Integer = 0): integer; deprecated;
    function FoldNestCount(ALineIndex: Integer; AType: Integer = 0): integer; deprecated;
    They are also no longer "virtual". Inherited classes must be updated

All functionality is now available through: 

    function FoldBlockOpeningCount(ALineIndex: TLineIdx; const AFilter: TSynFoldBlockFilter): integer; virtual;
    function FoldBlockClosingCount(ALineIndex: TLineIdx; const AFilter: TSynFoldBlockFilter): integer; virtual; overload;
    function FoldBlockEndLevel(ALineIndex: TLineIdx; const AFilter: TSynFoldBlockFilter): integer; virtual; overload;
    function FoldBlockMinLevel(ALineIndex: TLineIdx; const AFilter: TSynFoldBlockFilter): integer; virtual; overload;

Please see notes in unit SynEditHighlighterFoldBase 

#### SynEdit: TSynCompletion, TSynAutoCompletion and TSynEditAutoComplete

Changed base class to be TSynPlugin. 

This means TSynCompletion/TSynAutoCompletion/TSynEditAutoComplete is now freed, if it has no owner ("create(nil)") and the last added editor is destroyed. (Removing the Editor, does not free the plugin) 

In other words: An owner-less plugin, assumes ownership by it's editor. 

Solution: \- Either create the completion with an Owner (or use the designer and drop one on your form) \- or keep at least one editor added \- or remove the editor, before destroying it 

### TCustomNotebook was moved from ExtCtrls to ComCtrls

  * **Effects** : TCustomNoteBook was moved
  * **Reason** : TCustomNotebook implements TPageControl, which is located in the ComCtrls unit. Logically the implementation of TPageControl should be in the ComCtrls unit, and this is also Delphi compatible. This change will also diminish the confusion that people make between TNotebook and TCustomNotebook, because both will be in different units now.
  * **Remedy** : The vast majority of projects should simply use TPageControl, so no change is required. For those that use TCustomNotebook, please change them to use TPageControl instead.



  


### TCustomNotebook was renamed to TCustomTabControl

  * **Effects** : TCustomNoteBook was renamed
  * **Reason** : The previous name implied an unexisting relation to TNotebook, which caused a lot of confusion. On top of that, it was also Delphi-incompatible. The new name is Delphi compatible and fixes the previous problems.
  * **Remedy** : Rename any occurance of TCustomNotebook to TCustomTabControl.



### TCustomPageControl/TPageControl.OnPageChanged were removed

  * **Effects** : TPageControl.OnPageChanged was removed
  * **Reason** : This event should never have existed because it is just a duplicate of OnChange which is the correct Delphi-compatible event. The way it was implemented, this event caused OnChange to be called twice, as per bug <http://bugs.freepascal.org/view.php?id=19903>
  * **Remedy** : Rename any occurance of TPageControl.OnPageChanged to TPageControl.OnChange. If you are using an old IDE but a new LCL you might error messages. To fix this, just rebuild your IDE to have it on the same version as your LCL.



### Support for the shift states ssCaps, ssNum and ssScroll was removed from OnMouseWheel.

  * **Previous behaviour** : LCL-Win32 would send ssCaps, ssNum and ssScroll based on the key lock state, which is totally wrong. Windows does not support those keys as shift states. LCL-Carbon had ssCaps implemented and LCL-Gtk1/2 had unreliable implementations for them. All of those implementations were only for mouse wheel events, which is inconsistent with other mouse events.
  * **Effects** : ssCaps, ssNum and ssScroll are now never sent in mouse wheel events.
  * **Reason** : These shift states were implemented very unreliably and only for mouse wheel events and they cannot be implemented reliably.
  * **Remedy** : If you were previously depending on receiving the caps lock, num lock, etc, lock state in the win32 interface, use this code instead:


    
    
    uses LCLIntf;
    
    begin
      CapsLock := LCLIntf.GetKeyState(VK_CAPITAL);
      NumLock := LCLIntf.GetKeyState(VK_NUMLOCK);
      ScrollLock := LCLIntf.GetKeyState(VK_SCROLL);
    

### TDBEdit use EditMask property of the linked Field.

  * **Previous behaviour** : TDBEdit controls used the value stored in EditMask property (inherited from TCustomMaskEdit).
  * **Effects** : TDBEdit.EditMask property will be ignored.
  * **Reason** : Delphi compatibility.
  * **Remedy** : To keep old behavior set CustomEditMask to True



### ENTER Key behavior on String Editor

The String Editor Dialog (e.g. TMemo.Lines OI's editor) now closes itself when the user hit <ENTER> and adds a new line when user hit <SHIFT> \+ <ENTER>. That regression is already fixed in fixes branch but the patch did not make it to release. 

# Other release notes

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | 1.0 | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch") | [4.0](<Lazarus_4.md> "Lazarus 4.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/Lazarus_0.99.0_release_notes](https://web.archive.org/web/20250318202549/https://wiki.freepascal.org/Lazarus_0.99.0_release_notes)_
