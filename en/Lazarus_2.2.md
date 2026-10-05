# Lazarus 2.2.0 release notes

## Contents

  * 1 LazUtils Changes
    * 1.1 FileExistsUTF8 uses SysUtils.FileExists
    * 1.2 Masks unit
    * 1.3 LazFreeType
    * 1.4 PasWStr unit removed
  * 2 LCL Interfaces Changes
    * 2.1 Internal component registration and TLCLComponent.NewInstance was optimized
    * 2.2 Widgetsets
  * 3 LCL Changes
    * 3.1 TLazIntfImage.Create with size and description flags need no CreateData anymore
    * 3.2 TValueListEditor
    * 3.3 TShellTreeView
    * 3.4 TShellListView
    * 3.5 New Screen temporary cursor
    * 3.6 TMaskEdit
    * 3.7 TGroupBox, TRadioGroup, TCheckGroup
    * 3.8 TFrame
    * 3.9 T(Float)SpinEdit
    * 3.10 TListBox
  * 4 IDE Changes
    * 4.1 Debugger
  * 5 Components
    * 5.1 AnchorDocking
    * 5.2 Spotter
    * 5.3 TAChart
    * 5.4 TSpinEditEx
    * 5.5 TFloatSpinEditEx
    * 5.6 DockedFormEditor
    * 5.7 Jedi Code Format
    * 5.8 Delphi Compiler Tool
  * 6 Changes affecting compatibility
    * 6.1 LazUtils
      * 6.1.1 PO files
      * 6.1.2 LConvEncoding
        * 6.1.2.1 Global variable ConvertEncodingErrorRaisesException replaced with ConvertEncodingErrorMode
      * 6.1.3 UITypes unit is deprecated
    * 6.2 LCL incompatibilities
      * 6.2.1 TMaskEdit: hexadecimal and binary characters
      * 6.2.2 TMaskEdit: 'C' in EditMask now actually means a character (other than blank) is needed
      * 6.2.3 TMaskEdit: Validate takes in account Upper- and LowerCase for 'c' and 'C' in EditMask
      * 6.2.4 TCanvas.PolyBezier default behavior
      * 6.2.5 TImageList stores compressed images in LFM
      * 6.2.6 TCalendar: option dsStartMonday of DisplaySettings replaced by Delphi-compatible FirstDayOfWeek property
      * 6.2.7 TRadioGroup's OnEnter and OnExit events
      * 6.2.8 ShortcutToText / TextToShortcut
      * 6.2.9 TTabControl / x,y in mouse-events (move,down,up,drag) and IndexOfTabAt
      * 6.2.10 Conversion of '\n' sequences in LCLTaskDialog.TTaskDialog
      * 6.2.11 TScrollingWinControl.ScrollInView and successors (e.g. TScrollBox, TForm)
      * 6.2.12 TColorDialog.Title
      * 6.2.13 GTK2 widgetset version requirement
    * 6.3 Components incompatibilities
      * 6.3.1 TAChart
        * 6.3.1.1 Chart.AllowZoom changed
        * 6.3.1.2 DataPointDrawTool.OnDraw event deprecated
        * 6.3.1.3 TChartAxis.OnMarkToText event deprecated
        * 6.3.1.4 OpenGL-related units moved to separate packge TAChartOpenGL
        * 6.3.1.5 Chart colors changed
      * 6.3.2 VirtualTreeViews: Renamed to "laz." prefix
      * 6.3.3 SynEdit
        * 6.3.3.1 SynEdit.TextView / TSynEditFoldedView
        * 6.3.3.2 senrTextBufferChanging / senrTextBufferChanged
        * 6.3.3.3 ScreenRowToRow/RowToScreenRow => ScreenXYToTextXY/TextXYToScreenXY
        * 6.3.3.4 RowColumnToPixels => ScreenXYToPixels
        * 6.3.3.5 SynEditExport
      * 6.3.4 T(Float)SpinEditEx.ReadOnly now also disables the use of arrowkeys
      * 6.3.5 T(Float)SpinEdit(Ex) default value for MaxValue changed
      * 6.3.6 T(Float)SpinEditEx MaxValue is no longer enforced to be >= MinValue
      * 6.3.7 TSpinEditExBase: call to abstract method SameValue in descendent classes (FPC 3.2.2 only)
      * 6.3.8 TLabel: DoMeasureTextPosition removed, added DoDrawText instead
    * 6.4 IDE incompatibilities
      * 6.4.1 LPI&LPS project files: XML structure changed
      * 6.4.2 LPK package files: XML structure changed
    * 6.5 Packages incompatibilities
      * 6.5.1 ProjectGroups: LPG XML structure changed
  * 7 Other release notes



## LazUtils Changes

### FileExistsUTF8 uses SysUtils.FileExists

  * Old behavior: FileExistsUTF8 supported Unicode in fpc 2.6.x under Windows
  * New behavior: FileExistsUTF8 now calls SysUtils.FileExists, which supports Unicode under Windows since fpc 3.0
  * Reason: Simplified



### Masks unit

  * An mechanism to disable sets in a specified mask is implemented. For this purpose a new parameter Options: TMaskOptions is introduced. Specifying moDisableSets in the Options parameter will disable interpreting '[' as the begin of a set in the specified mask.
  * Example: MatchesMask('[x]','[x]',[moDisableSets]) will return True.



### LazFreeType

  * Freetype font rendering units were moved to a package of its own "components/freetype/freetypelaz.lpk"
  * Reason: Units have grown to an impressive number



### PasWStr unit removed

  * The PasWStr unit has been removed.
  * Reason: it contained functional code only if the compiler was pre 3.0.
  * Remedy: remove PasWStr from your uses clause.



## LCL Interfaces Changes

### Internal component registration and TLCLComponent.NewInstance was optimized

LCL components are registered and mapped to a widgetset by TLCLComponent.NewInstance. This is an undocumented implementation detail and should not affect any components outside of LCL itself. However some external components could support widgetsets the same way. Then "class procedure WSRegisterClass; override;" is needed having a call to "RegisterWSComponent()". It was added to component OpenGlContext in trunk r63636. Please use it as an example if needed. 

### Widgetsets

  * Qt5 widgetset needs libQt5Pas v 1.2.8 since result of QWidget_winId() is changed from PtrInt to PtrUInt. 
    * Windows: Qt5Pas1.dll is already in lazarus trunk binaries directory.
    * Linux: distributions should rebuild libQt5Pas packages.
    * macOS: you should rebuild libQt5Pas against downloaded Qt5Pas libs (Qt5-5.6 is minimum).


  * Qt5 added QLCLOpenGLWidget into libQt5Pas v.1.2.8 for full OpenGL component support.


  * Qt, Qt5 and Gtk2: Form dimensions are accurate now on x11 (frame around form). 
    * Solution is provided by creating dummy widget before Application.MainForm.Handle is created. This can be disabled via command line switch "-disableaccurateframe". This feature is used inside WidgetSet.GetWindowRect() only.


  * Qt and Qt5: Form dimensions are accurate now on MS Windows and macOS. 
    * This is accomplished by creating winID if form isn't mapped yet inside WidgetSet.GetWindowRect().



## LCL Changes

### TLazIntfImage.Create with size and description flags need no CreateData anymore

  * Old: After creating of a TLazIntfImage with size and flags CreateData has to be called
  * New: Creating of a TLazIntfImage with size and flags internal calls CreateData
  * Reason: Simplify, see [Issue #35035](<https://bugs.freepascal.org/view.php?id=35035>)



### TValueListEditor

  * Old behaviour: having the current NameValueSeparator (by default: '=') in the Key column was allowed.
  * New behaviour: you cannot have a NameValueSeparator in the Key column. If you type the NameValueSeparator in the Key column, the focus will move to the Value Column. If you paste it into the column, it will be removed (without any feedback or warning).
  * Reason: By design a Key can never have a NameValueSeparator in it. Having it there causes havoc. See [this forum post](<https://forum.lazarus.freepascal.org/index.php?topic=51977.0;topicseen>).
  * Note: This new behaviour is not Delphi compatible (which is a conscious design decision), Delphi raises an exception if you try to type or paste a NameValueSeparator in the Key column.
  * Revision: r64089 and r64090.



### TShellTreeView

  * New event OnAddItem gives possibility for finer control over what to add to the tree.
  * Shell icons are displayed automatically in Windows when no imagelist is attached and when UseBuiltInIcons is true.



### TShellListView

  * New event OnAddItem gives possibility for finer control over what to add to the listview.
  * New property MaskCaseSensitivity to allow e.g. case-insensitive masks on *nix. The property defaults to platfom standard behaviour.
  * Shell icons are displayed automatically in Windows when no imagelist is attached and when UseBuiltInIcons is true.



### New Screen temporary cursor

Screen has new methods: 

  * BeginTempCursor / EndTempCursor
  * BeginWaitCursor / EndWaitCursor
  * BeginScreenCursor / EndScreenCursor



These should be used for temporary cursor changes instead of setting the Screen.Cursor property directly. They ensure that valid cursor values are restored. 

### TMaskEdit

  * New property ValidationErrorMode to allow not raising an exception when validation fails, but instead firing an OnValidationError event.
  * New property EnableSets to allow specifying sets in the EditMask property.



### TGroupBox, TRadioGroup, TCheckGroup

  * Old behaviour Win32: You could set property Color and it was directly displayed.
  * New behaviour Win32: If you want to change the color, you have to set ParentBackground := False;
  * Reason: TGroupBox and TRadioGroup are now transparent like in Delphi (Delphi does not have a TCheckGroup). See [Issue #37151](<https://bugs.freepascal.org/view.php?id=37151>)



### TFrame

  * Old behaviour Win32: You couldn't change the color of TFrame.
  * New behaviour Win32: If you want to change the color, you have to set ParentBackground := False;
  * Reason: Default TFrame is transparent, but you can change the color if you want. See [Issue #35229](<https://bugs.freepascal.org/view.php?id=35229>)



### T(Float)SpinEdit

  * New property EditorEnabled to disable direct user input in the edit. If set to False, users can only change the value by using the spinners or the arrowkeys.



### TListBox

  * Old: ScrollWidth always was received from OS.
  * New: If ScrollWidth is set, this value is taken. If default 0 is set, at runtime the ScrollWidth is received from OS.
  * Reason: Enabling of ScrollBar. See [Issue #19079](<https://bugs.freepascal.org/view.php?id=19079>)



## IDE Changes

  * Added IDE macro $(OutputFile) for the outputfile of the project. This usually corresponds to $(TargetFile), except when having a host application, in which case $(TargetFile) is the host application, while $(OutputFile) is the project executable (or library).
  * Codetools support for anonymous functions
  * "Close tabs to the right" in Source-Notebook
  * Buttons for closing pages on the left, right and others in Search Results window
  * An options page "Environment > IDE Startup" in the IDE Options dialog. Allows selecting a project type to be created when the IDE starts.
  * Object Inspector 
    * Keep Expanded/Collapsed states in the component tree after adding or deleting a component or changing Z-order.
    * Remember Expanded/Collapsed states in the component tree when switching between designer forms.
    * Editor window for collections, like Fields, Columns, Params etc. shows now collections of same type when switching between controls of same class.


  * Project Inspector 
    * GUI for Min / Max version of a package requirement, similar what the Package Editor has.
  * Editor 
    * Added commands (key-shortcut) for 
      * Swap line/selection up/down (selection is extended to full lines)
      * Duplicate line/selection (selection is extended to full lines)
      * Copy/Cut append to clipboard (with space separator, unless already present)
      * Copy/Cut (append) line/selection to clipboard (selection is extended to full lines, and also always includes line with caret)
    * Option to prevent Select-All/Paragraph/ToBrace from scrolling
    * Identifier Completion to trigger on any input, not just on dot. (Optional, must be enabled first)


  * Extension of master PO files (a.k.a. templates) is changed from .po to .pot. 
    * Example for IDE translation:



    

    Previously we had:

    

    Template: lazaruside.po
    German: lazaruside.de.po
    Russian: lazaruside.ru.po
    Spanish: lazaruside.es.po

    

    Now we have (note extension of the template):

    

    Template: lazaruside.pot
    German: lazaruside.de.po
    Russian: lazaruside.ru.po
    Spanish: lazaruside.es.po

    

  * Reasons: 
    * .pot is an 'industry standard' extension for PO template files. As a consequence, PO editors can now open our templates 'out of the box' and automate creation of translations.
    * It is much simpler to detect template files now that dotted unit names are allowed.
  * Remedy: You can safely remove .po template files from your project after you rebuild your project with the new IDE (at this point .pot files should be generated).



  * Project Groups: 
    * Option frame under Tools / Options
    * Load last open project group on start (option to disable)
    * More / Undo menu item to undo a delete
    * More / Info menu item to show some stats. At the moment it shows all source directories.
    * Find in Files has a new option "Search in project group".
    * Option to redirect the IDE command Compile/Build project to Compile/Build the selected target in the project group editor. For example Ctrl-F9 can compile the whole project group.
  * Designer 
    * Change background color of non form designer to clWindow. Color is now changeable per option.
    * Remove environment option "Reduced designer painting" to prevent painting artefacts. See [Issue #23741](<https://bugs.freepascal.org/view.php?id=23741>) or [Issue #33781](<https://bugs.freepascal.org/view.php?id=33781>)
  * Package Manager 
    * When creating new package or component, suggested name is not localized anymore (always 'NewPackage' for every language). This change prevents crashes on attempt to create a new package when IDE interface language has non-latin alphabet.


  * macOS: relocation of the Lazarus IDE "Lazarus > Tools > Options" menu to "Lazarus > Preferences".



#### Debugger

  * LazDebugger-FP (FpDebug) v1.0 is now the default for **new** installations on Windows and Linux. Users with existing config should change their settings.
  * GDB(mi): comes now with gdb version 9.2 modified for use with unicode. (for 32 and 64 bit)



    

  * GDB(mi) Debugger: Stepping from exception to finally/except now works under Win64 SEH. (Requires updated GDB)
  * GDB(mi) Debugger: Updated GDB for Win64 to 8.2
  * GDB(mi) Applying a condition to a breakpoint will now mark the breakpoint as invalid, if gdb does not accept the condition. The breakpoint may still break (or not), but the condition was not set.
  * Debugger Config: The IDE now provides the ability to set up different debugger-configs as named profiles.
  * "Run to Cursor" in addition to the existing "Step over to Cursor", the debugger (gdbmi/fpdebug) now provide "Run to Cursor".



## Components

  * OpenGL: full support for Qt5 widgetset



### AnchorDocking

  * New button to minimize a docked site.
  * Allow ManualDock for target AnchorDockPanel.
  * New option "Multiline Tabs" for docked pages added.
  * New option "Floating windows on top" added. If there are undocked windows shown, show these on top of main window. In a own application, there can be set DockMaster.MainDockForm, then all floating windows are shown on top of it. If MainDockForm is not set, Application.MainForm is used.
  * AnchorDocking can load layouts HighDPI aware



### Spotter

  * New IDE addon, showing a search list of all IDE commands.



### TAChart

  * New chart tools "TAxisClickTool", "TTitleFootClickTool" and "TLegendClickTool": Create OnClick events when the user clicks on a chart axis or its title, the chart title or footer, or the chart legend, respectively. Good for creating an interactive GUI for formatting charts.
  * New property "AllowPanning" to turn built-in panning on/off.
  * New property "LimitToExtent" for zooming tools, avoids zooming out of the chart's full extent.
  * New property "Wordwrap" for chart axis titles as well as chart title and footer.
  * New series type "TPolygonSeries" which draws a filled closed polygon. Can be the basis of GIS and contour plots.
  * Property editor for TSeriesPointerStyle.
  * TBarSeries: 
    * New property "BarShape" for drawing alternative shapes (cylinder, hexagonal prism, pyramid or cone) instead of rectangular bar.
  * TPieSeries: 
    * Impoved painting of 3D pie series due to new properties: "DepthBrightnessDelta" allowing to darken or brighten the sides of the pie slices, "Orientation" and "ViewAngle" for different views of the series
    * New property "InnerRadiusPercent" allowing to draw a chart (donut).
    * New property "MarkPositionCentered" for attaching data point labels to the pie center instead of the pie perimeter.
    * New property "StartAngle" to define the position of the first pie slice.
    * New property "AngleRange" to define tha total angular range between first and last slice.
  * TListChartSource can be sorted by X, Y, XList, YList, Color, Text, or in a user-specific way defined by the event OnCompare.
  * New "TSortedChartSource" which, if inserted between original chart source and series, allows sorting of TUserDefinedChartSource or TCalculatedChartSource by the same criteria as TListChartSource.
  * New property "AdjustSides" of TChartExtentLink allowing to automatically align the sides of aligned charts to each other (in case of differently sized axis labels and/or titles).
  * New component "TChartLiveView" which represents a scrolling viewport for a large amount of data arriving by and by.



### TSpinEditEx

  * New property "ThousandSeparator" to allow displaying the value with thousandseparators inserted.



### TFloatSpinEditEx

  * Entering text in scientific notation (e.g. "1.23E3") is now supported.
  * New property DisplayMode that controls in wether or not to use scientific notation. If set to dmAuto the following new properties control how the value is displayed: 
    * Property ExponentialFormatLimitPos controls from which positive exponent on scientific notation is used for displaying.
    * Property ExponentialFormatLimitNeg controls from which negative exponent on scientific notation is used for displaying.
  * New property Precision controls the precion used when value is displayed in scientific notation.



### DockedFormEditor

  * New package DockedFormEditor added. For more info see [DockedFormEditor](<DockedFormEditor.md> "DockedFormEditor"). 
    * Replacement for Sparta_DockedFormEditor. All Sparta packages are now marked as deprecated and will be removed in a future Lazarus version.
    * Optional Anchor Designer included.



### Jedi Code Format

  * Various improvement for the parser. It now supports most modern Object Pascal syntax.



### Delphi Compiler Tool

Use the Delphi command line compiler in Lazarus: [Lazarus Delphi Compiler Tool](<Lazarus_Delphi_Compiler_Tool.md> "Lazarus Delphi Compiler Tool")

## Changes affecting compatibility

### LazUtils

#### PO files

  * Trailing lineendings are not forcibly added to multiline strings in PO files anymore. Now multiline strings in PO files match exactly corresponding resource strings.



    

  * Reasons: 
    * PO format does not prohibit multiline strings without trailing lineendings (and e.g. recent versions of Poedit do not complain about them).
    * New behavior unbreaks loading multiline translations from MO files.
  * Note that all multiline strings with mismatching trailing lineendings will be marked as fuzzy during translation update.



#### LConvEncoding

##### Global variable ConvertEncodingErrorRaisesException replaced with ConvertEncodingErrorMode

The unit had global variable ConvertEncodingErrorRaisesException (boolean) which is now replaced by the enumeration: 
    
    
    type
      TConvertEncodingErrorMode = (
        ceemSkip,
        ceemException,
        ceemReplace,
        ceemReturnEmpty
        );
    var
      ConvertEncodingErrorMode: TConvertEncodingErrorMode = ceemSkip;
    

#### UITypes unit is deprecated

The UITypes unit is deprecated in favor of System.UITypes, which is available in FPC 3.2.0 and up. 

  * Reason: System.UITypes is a superset of UITypes and contains some additional constants (Delphi-compatible).
  * Remedy: Use System.UITypes unit instead of UITypes unit in your programs.



### LCL incompatibilities

#### TMaskEdit: hexadecimal and binary characters

Mask support for hexadecimal and binary characters was added (this is a Lazarus extension, Delphi does not support this). 

  * Old behavior: the characters 'h', 'H', 'b' and 'B' had no special meaning in the EditMask property.
  * New behavior: these characters now are mask characters for hexadecimal and binary input respectively.
  * Reason: nice to have feature (QT5 supports it natively).
  * Remedy: if you need one of these characters as a literal in your EditMask: escape them with a '\'.



#### TMaskEdit: 'C' in EditMask now actually means a character (other than blank) is needed

  * Old behaviour: both 'c' and 'C' in EditMaks were treated as "any Utf8 char but not necessary".
  * New behaviour: 'C' is now treated as "any Utf8 char, but NOT SpaceChar".
  * Reason: Delphi compatibility.
  * Remedy: use 'c', not 'C', if any character (including SpaceChar) should be accepted.



#### TMaskEdit: Validate takes in account Upper- and LowerCase for 'c' and 'C' in EditMask

  * Old behavior: upper- or lowercase was checked (if so specified by '>' or '<' in EditMask) for 'a', 'A', 'l', 'L', 'h' and 'H', but not for 'c' and 'C'.
  * New behaviour: upper- or lowercase is now also checked for 'c' and 'C'.
  * Reason: consistency.
  * Remedy: if you need the old behaviour, insert '<>' before 'c' or 'C'.



#### TCanvas.PolyBezier default behavior

The default value of the Continuous parameter was changed because it breaks Delphi compatibility. 

  * Old behavior: There are several overloads and optional parameters of the canvas' `PolyBezier` method. In the Delphi-compatible call having only the points array as a parameter, the `Continuous` argument has the default value `false`.
  * New behavior: The argument `Continuous` has the value `true` now.
  * Reason: The Delphi `PolyBezier` behavior is different from the Lazarus behaviour with the same call parameters; it draws the Bezier curve like Lazarus using `Continuous = true`.
  * Remedy: To obtain the old behavior expand the parameter list by the `Continuous` parameter set to `false`.



#### TImageList stores compressed images in LFM

  * Old: TCustomImageList stored binary bitmap data of the base resolution.
  * New: The binary bitmap data streamed to LFM is compressed with ZLib.
  * Reason: smaller LFM and EXE files.
  * Effects: Lazarus 1.8 cannot open LFMs with a TImageList from Lazarus 2.2. Lazarus 2.0 can open them.
  * Remedy: If you want to open a Lazarus 2.2 project in Lazarus 1.8, you first have to open it in Lazarus 2.0, open all forms containing LFMs with a TImageList and resave them. Then you can open these resaved LFMs in Lazarus 1.8.



#### TCalendar: option dsStartMonday of DisplaySettings replaced by Delphi-compatible FirstDayOfWeek property

  * Old behavior: Changing the start day of a week to Monday works only in QT and QT5 widgetsets. And even there it does not allow for other weekdays like Saturday as needed for the Arabian world. Moveover the option is not compatible with Delphi which uses a separate property FirstDayOfWeek.
  * New behavior: The option dsStartMonday has been removed, use the property FirstDayOfWeek instead. It allows switching to other weekdays as well. The default option, dowDefault, makes the weeks begin with the day defined by the widgetset/OS - which is the old behavior in those cases where dsStartMonday did not work.
  * Reason: Improved Delphi compatibility, more than two weekdays should be allowed as start of the week.
  * Remdy: In most cases dsStartMonday probaby has not been set because it did not work. If it was set, ignore the property when reading (or remove the line dsStartMondy = true from the lfm file), and set FirstDayOfWeek to dowMonday in the Object Inspector.
  * Note 1: The FirstDayOfWeek property is still not supported by all widgetsets. It is respected only for win32, qt and qt5.
  * Note 2: The name of the default option of FirstDayOfWeek, dowDefault, is not exactly Delphi-compliant (dowLocaleDefault) because a "Locale" does not exist in all widgetsets. Since the default value is not stored anyway this incompatibility may be tolerable.



#### TRadioGroup's OnEnter and OnExit events

  * Old behavior: The OnEnter and OnExit events are triggered by TRadioGroup when another item is selected. As a consequence, these events fire twice when the control receives or loses focus. This is against Delphi behavior.
  * New behavior: New events OnItemEnter and OnItemExit were added; they fire when the ItemIndex changes and carry the corresponding radiobutton in the Sender parameter.
  * Reason: No logical behavior; Delphi compatibility.
  * Remedy: Ony applications evaluating these events for the internal selection change are affected. The new events OnItemEnter and OnItemExit should be used instead.



#### ShortcutToText / TextToShortcut

  * Old behaviour: 
    * the textual representation of the VK_OEM_PLUS was '+'.
    * the result of TextToShortcut('+') was 187.
    * the result of TextToShortcut('=') was 0.
  * New behaviour: 
    * the textual representation of the VK_OEM_PLUS now is '='.
    * the result of TextToShortcut('+') is now 0.
    * the result of TextToShortcut('=') is now 187.
  * Reason: Delphi compatibility.
  * Remedy: change any shortcuts with value '+' to '='



#### TTabControl / x,y in mouse-events (move,down,up,drag) and IndexOfTabAt

  * Old behaviour 
    * When the mouse was over the tab, the x/y params to any mouse event where given relative to the embedded PageControl.

    For the remaining ClientArea x/y were relative to the TabControl
    I.e. X/Y over the tabs where: Negative values for tabs on top / 0..20 for tabs on bottom
    This meant that the same x/y in a TabControl could be received for the mouse over a header (tabs at bottom) and the mouse in the client area. (Y=0 to Y=approx-20 did exist twice)
    * IndexOfTabAt did expect the x/y as given in the event. ScreenToClient(Mouse.CursorPos) could not be used, as it always returned x/y relative to the TabControl, and never relative to the embedded PageControl
  * New behaviour 
    * All X/Y are reported relative to the TabControl. There are no longer any negative values for Y.
    * IndexOfTabAt expects coordinates relative to the TabControl
  * Reason: 
    * This fixes ambiguity for X/Y value that could occur twice
    * IndexOfTabAt can now accept the event-X/Y as well as the ScreenToClient(Mouse.CursorPos)
  * Remedy: 
    * If you called IndexOfTabAt with the event X/Y => all keeps working
    * If you did any amendments to any X/Y (other than calling ClientToScreen), that is if you added/subtracted some constant to "make them work" => remove your workaround
  * See also 
    * <https://bugs.freepascal.org/view.php?id=38403>
    * Cocoa: <https://bugs.freepascal.org/view.php?id=38553>



#### Conversion of '\n' sequences in LCLTaskDialog.TTaskDialog

  * Old behaviour: 
    * '\n' sequence was converted to linefeed in the following fields: Title, Inst, Content, Verify, Info, InfoExpanded, InfoCollapse, Footer.
  * New behaviour: 
    * '\n' sequence is no longer converted to linefeed in these fields.
  * Reason: Conversion of '\n' to linefeed mangled strings like 
        
        'Save "c:\new_folder\new.work"?'
        

  * Remedy: replace '\n' sequences which should be converted to linefeeds with LineEnding constants, i.e. instead of
        
        'Line1\nLine2'
        

use 
        
        'Line1'+LineEnding+'Line2'
        

  * See also <https://bugs.freepascal.org/view.php?id=38676>



#### TScrollingWinControl.ScrollInView and successors (e.g. TScrollBox, TForm)

  * Old behavior: Scrolling of a control in view was possible without ScrollBars visible.
  * New behavior: Scrolling of a control in view is only possible when ScrollBar is visible.
  * Reason: A scrolling feature is only with ScrollBars usefull. Also some controls are painted wrong, when no scrollbar is visible.
  * See also 
    * <https://bugs.freepascal.org/view.php?id=38838>
    * <https://forum.lazarus.freepascal.org/index.php/topic,54354>



#### TColorDialog.Title

  * Old behavior: On Windows, the Title of the TColorDialog was ignored and painted by the OS as the localized text "Color".
  * New behavior: Now the Title property of the TColorDialog is used like in other widgetsets. The default title is the unlocalized text "Select color" which can be considered to be inconsistent in localized applications.
  * Reason: An unused property is always confusing to the users.
  * Remedy: Enter the Title manually when an application requires the localized native title, or use the translation utilities to translate the LCL resource strings.



#### GTK2 widgetset version requirement

  * Old behavior: At least GTK2 2.8 was required.
  * New behavior: At least GTK2 2.24 is required.
  * Reason: All supported distribution versions already have GTK2 2.24.
  * Remedy: GTK2 2.22 may still work (untested). Older GTK2 (2.20 and earlier) versions may still work (untested, so completely at your own risk) after reverting commit 0172124a423be.



### Components incompatibilities

#### TAChart

##### Chart.AllowZoom changed

The property AllowZoom of TChart is meant to give the user the chance to turn the builtin zooming feature on or off. 

  * Old behavior: AllowZoom operates also on an external toolset.
  * New behavior: AllowZoom is effective only for the built-in toolset.
  * Reason: More consistent handling of properties
  * Remedy: Use the property Enabled of the external zoom tool to switch zooming off, or don't add a zoom tool at all.



##### DataPointDrawTool.OnDraw event deprecated

The event OnDraw of the data point draw tools (TDataPointCrosshairTool, TDataPointDistanceTool) will be removed and replaced by the new event OnCustomDraw. 

  * Old behavior: OnDraw uses the chart's Drawer for painting the crosshair or distance bars.
  * New behavior: the Drawer to be used is passed as a parameter.
  * Reason: When a chart is painted to an external file the chart's Drawer still refers to the screen; therefore, the datapoint cross hair or distance bars are missing in the external file.
  * Remedy: Copy the old OnDraw event handler code to the new OnCustomDraw handler, but replace "Chart.Drawer" by the "ADrawer" given as a event parameter.



##### TChartAxis.OnMarkToText event deprecated

The event OnMarkToText of the chart's axes will be removed and replaced by the new event OnGetMarkText. 

  * Old behavior: The old OnMarkToText event does not have a Sender parameter.
  * New behavior: The new OnGetMarkText event has the affected axis as Sender parameter.
  * Reason: When dealing with multiple charts and/or multiple axes, it is currently required to create an individual handler for each axis because it is not possible to determine the axis for which to create axis marks for. This is not required any more with the new event.
  * Remedy: Copy the old OnMarkToText handler code to the new OnGetMarkText handler.



##### OpenGL-related units moved to separate packge TAChartOpenGL

  * Old behavior: OpenGL-related units had been available in the main TAChartLazarusPackage directly. The drawer unit TADrawerOpenGL was not added to the uses list automatically and required a special treatment (see old OpenGL demo project).
  * New behavior: OpenGL-related units are now contained in the new package TAChartOpenGL.
  * Reason: Compilation issues when the system did not contain OpenGL libraries.
  * Remedy: Add package TAChartOpenGL to the requirements of the project. Only needed when project uses the OpenGL drawer.



##### Chart colors changed

  * Old behavior and reason: Some chart elements had hard-coded rgb colors, such as Chart.Title.Font.Color = clBlue, or ChartAxis.TickColor = clBlack. Therefore, these elements could hardly be discerned when the system runs in "dark mode".
  * New behaviour: Such critical colors are system colors now which have a decent appearance in both dark and bright mode
  * Remedy: If the old colors are really needed reset them in the object inspector.



#### VirtualTreeViews: Renamed to "laz." prefix

VirtualTreeViews is essentially needed by OnlinePackageManager. Having this original package within the Lazarus installation makes is impossible for users to install a different version of this package. 

  * Old behavior: Projects with VirtualTreeViews required the "virtualtreeview_package" and had the unit "virtualtrees" in the uses clause. The registered component classes were named "TVirtualStringTree", "TVirtualDrawTree", "TVTHeaderPopup".
  * New behavior: The package was renamed to "laz.virtualtreeview_package". All file names were changed to have a "laz." prefix, i.e. the uses clause will have to contain "laz.virtualtrees". The registered components are named "TLazVirtualStringTree", "TLazVirtualDrawTree", and "TLazVTHeaderPopup".
  * Reason: Avoid naming conflicts when another version of VirtualTreeViews is installed
  * Remedy: For existing projects requiring the VirtualTreeViews distributed with Lazarus, 
    * open the lfm file in an external editor and replace "TVirtualStringTree" by "TLazVirtualStringTree", as well as - if available - "TVirtualDrawTree" by "TLazVirtualDrawTree" and "TVTHeaderPopup" by "TLazVTHeaderPopup".
    * open the project in the IDE, remove package "VirtualTreeView_package" from the project requirements and replace it by "laz.virtualtreeview_package"
    * in the pas file's "uses" clause, replace "virtualtrees" by "laz.virtualtrees"
    * in the pas file replace "TVirtualStringTree" by "TLazVirtualStringTree" (and accordingly for the other VTV components).



#### SynEdit

SynEdits TextView system has undergone some changes. TextViews (TSynEditStrings) map the physical text (as saved/loaded to/from a file) to what is visible on the screen. 

This also affects the coordinate system. In addition to [Logical/Physical](<https://wiki.lazarus.freepascal.org/SynEdit#Logical.2FPhysical_caret_position>) SynEdit now also has ViewedXY. "Viewed" also maps the Y coordinate. 

##### SynEdit.TextView / TSynEditFoldedView

The protected property SynEdit.TextView is no longer of type TSynEditFoldedView. It may hold any other TextView-class instead. 

  * Old behaviour: TSynEditFoldedView was accessible via SynEdit.TextView
  * New behaviour: SynEdit.TextView can be any TextView
  * Remedy: SynEdit.TextViewsManager.SynTextViewByClass[TSynEditFoldedView]



  
Class hierarchy 

  * TSynEditFoldedView is now a TSynEditStrings.



  
TSynEditFoldedView.Lines/Count 

  * Old behaviour: Lines/Count referred to visible lines only
  * New behaviour: Lines/Count referred to al lines.
  * Remedy: Use ViewedLines / ViewedCount



  
Mapping 

  * Old behaviour: TextToViewIndex/ViewToTextIndex => ViewPos was 1 based
  * New behaviour: TextToViewIndex/ViewToTextIndex => ViewPos was 0 based
  * Remedy: Use ToPos() / ToIdx()



  


  * Old behaviour: TextPosAddLines with 1 based lines/result
  * New behaviour: AddVisibleOffsetToTextIndex with 0 base lines/result
  * Remedy: Rename and use ToPos() / ToIdx()



  
Callbacks 

  * Old behaviour: AddFoldChangedHandler was reporting changes to folding.
  * New behaviour: AddChangeHandler(senrLineMappingChanged)



##### senrTextBufferChanging / senrTextBufferChanged

Callbacks on the TextView/TextBuffer were only valid for the current Buffer. If SynEdit shared/unshared the textbuffer, then all handlers needed to be re-registered. 

This is no longer needed. 

##### ScreenRowToRow/RowToScreenRow => ScreenXYToTextXY/TextXYToScreenXY

ScreenRowToRow/RowToScreenRow were deprecated, use ScreenXYToTextXY/TextXYToScreenXY 

The new functions take a point, instead of just a line/row. This reflects the idea that a single text-line can visible occupy several lines. Or mapping can otherwise relay on the x pos, depending on whatever TextViews are used. 

##### RowColumnToPixels => ScreenXYToPixels

RowColumnToPixels is deprecated, use ScreenXYToPixels 

  


##### SynEditExport

  * Old behaviour: TSynCustomExporter.ExportRange raised an EAbort exception when ALines or Highlighter is unassigned, or when the content of the range to be exported was in fact empty.
  * New behaviour: 
    * A verbose exception (ESynExport) is raised when either ALines or Highlighter are unassigned.
    * No exception is raised if the range to be exported is empty, it will simply export nothing in that case.
  * Reason: 
    * Raising EAbort if the range to be expoterd is empty doesn't really make sense, at least it should not be forbidden.
    * Not having a Highlighter or not having Lines is a critical error, and as such must raise a verbose exception.
  * Remedy: if your code relied on this behaviour (rasing EAbort), you will have to adjust it.



#### T(Float)SpinEditEx.ReadOnly now also disables the use of arrowkeys

  * Old behaviour: if ReadOnly is True, the value of the control could still be changed using the up or down arrow keys.
  * New behaviour: if ReadOnly is True, the value of the control cannot be changed using the the up or down arrow keys.
  * Reason: internal consistency and consistency with T(Float)SpinEdit.
  * Revision: r64952.



#### T(Float)SpinEdit(Ex) default value for MaxValue changed

  * Old behaviour: the default value of MaxValue was 100
  * New behaviour: the default value for MaxValue is 0. As a consequence the control is no longer "limited" by default.
  * Reason: compatibility with Delphi's TSpinEdit.
  * Remedy: in Object Inspector set MaxValue to 100 (if so desired) and re-save the form.
  * Revision: r64969, r64972



#### T(Float)SpinEditEx MaxValue is no longer enforced to be >= MinValue

  * Old behaviour: if you set MaxValue to a value less than MinValue, MaxValue got the same value as MinValue.
  * New behaviour: MaxValue is allowed to be less than MinValue.
  * Reason: old behaviour cased bugs in loading.
  * Remedy: if your code depended on the old behaviour, you'll have to adjust is.
  * Note: since the control is only "limited" if MaxValue > MinValue, it should not really make a difference.
  * Revision: r65065.



#### TSpinEditExBase: call to abstract method SameValue in descendent classes (FPC 3.2.2 only)

  * Old behaviour: The component used Math.SameValue to compare two values. FPC 3.2.2, however, sometimes refuses compilation because it cannot determine which one of the many overloads of Math.SameValue should be used.
  * New behaviour: The component has its own method SameValue now. It had to be declared as abstract to keep compilation with FPC 3.2.0 and older. Therefore, all descendants of TSpinEditExBase must override this method.
  * Remedy: If your descendent class of TSpinEditExBase reports a runtime error "abstract method called" you must implement the method SameValue for it, based on the data type specializing the generic data variable. It is needed only for FPC 3.2.2.
  * Note: This issues only affects third-party components descending from TSpinEditExBase.
  * Introduced by revisions <https://gitlab.com/freepascal.org/lazarus/lazarus/-/commit/182cc8b8e6d6a671eb62f7177ff86ee948814de8> and <https://gitlab.com/freepascal.org/lazarus/lazarus/-/commit/7f45938388be4b3ee50b2cd1ed1d04d991f92143>



#### TLabel: DoMeasureTextPosition removed, added DoDrawText instead

  * Old behavior: TCustomLabel had the DoMeasureTextPosition virtual method that allowed to set the X/Y position of the text.
  * New behavior: TCustomLabel now has the DoDrawText virtual method that allows to override the whole text painting and automatic sizing of the label.
  * Reason: It is now possible to override the painting/sizing the label with own text drawing routine, which wasn't easily possible before. Now the TLabel is also Delphi-compatible.
  * Remedy: if your TCustomLabel descendant used to override DoMeasureTextPosition, it needs to override DoDrawText now and handle the whole paint process.
  * Note: This issue only affects third-party components descending from TCustomLabel. See more info: <https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39523#note_798080430>



### IDE incompatibilities

#### LPI&LPS project files: XML structure changed

  * Old behavior: Child elements had an index suffix.
  * New behavior: Child elements have no suffix.
  * Problems: New LPI files cannot be opened in legacy Lazarus installations.
  * Reason: VCS like the new structure better.
  * Remedy: Save your LPI files in compatibility mode. (Project options -> Miscellaneous -> Maximize compatibility of project files). Users of old Lazarus versions can convert the file format by means of the **[fixlp](<fixlp.md> "fixlp")** utility.



#### LPK package files: XML structure changed

  * Old behavior: Child elements had an index suffix.
  * New behavior: Child elements have no suffix.
  * Problems: New LPK files cannot be opened in legacy Lazarus installations.
  * Reason: VCS like the new structure better.
  * Remedy: Save your LPK files in compatibility mode. (Package options -> Usage -> Maximize compatibility of package file). Users of old Lazarus versions can convert the file format by means of the **[fixlp](<fixlp.md> "fixlp")** utility.



### Packages incompatibilities

#### ProjectGroups: LPG XML structure changed

  * Old behavior: The <Mode?> elements were not placed within the <BuildModes> element. Child elements had an index suffix.
  * New behavior: Child elements have no suffix.
  * Problems: New LPG files cannot be opened in legacy Lazarus installations.
  * Reason: VCS like the new structure better.
  * Remedy: There is no.



## Other release notes

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | 2.2 | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/Lazarus_2.2.0_release_notes](https://web.archive.org/web/20241227110548/https://wiki.freepascal.org/Lazarus_2.2.0_release_notes)_
