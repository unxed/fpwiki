# Lazarus 3.0 release notes

## Contents

  * 1 LCL Interfaces Changes
    * 1.1 Cocoa
    * 1.2 Qt
    * 1.3 Qt5
    * 1.4 Qt6
    * 1.5 Gtk3
  * 2 LCL Changes
    * 2.1 TCustomImageList
    * 2.2 TTaskDialog
    * 2.3 TSpeedButton
    * 2.4 TLabel.Transparent, .Color and .ParentColor changes
    * 2.5 TPanel.VerticalAlignment
    * 2.6 TCalendar
    * 2.7 TCheckbox, TRadioButton
    * 2.8 Grids
    * 2.9 TShellTreeView
    * 2.10 TShellListView
    * 2.11 TTreeView
    * 2.12 TTrayIcon gtk2 Only
  * 3 IDE Changes
    * 3.1 Character map
    * 3.2 Debugger
      * 3.2.1 Project Options
      * 3.2.2 IDE Dialogs
      * 3.2.3 FpDebug / LazDebuggerFp
      * 3.2.4 LazDebuggerFpLldb (default on MacOS)
    * 3.3 Floating point properties in the Object Inspector
    * 3.4 Reading unit names in lfm
    * 3.5 Ambiguous Classtypes
    * 3.6 Editor
    * 3.7 IDE Options
    * 3.8 Lazarus Examples
  * 4 IDE Interface Changes
  * 5 Components
    * 5.1 TAChart
    * 5.2 TDateTimePicker
    * 5.3 TDBDateTimePicker
    * 5.4 T(Float)SpinEditEx
    * 5.5 TCheckListBox
    * 5.6 Pas2js
    * 5.7 Lazarus Icon Collection
    * 5.8 gir2pascal
    * 5.9 lazdelphi
    * 5.10 Jedi Code Format
    * 5.11 Sparta_DockedFormEditor
  * 6 Changes affecting compatibility
    * 6.1 IDE Settings
      * 6.1.1 Run > Run Parameters (UPDATED FOR 3.2)
    * 6.2 LCL incompatibility
      * 6.2.1 TLabel: autosized and right-aligned
      * 6.2.2 TDateEdit/TTimeEdit
      * 6.2.3 Cocoa
      * 6.2.4 GTK3
      * 6.2.5 TControl: ChildSizing
    * 6.3 LazUtils
      * 6.3.1 Masks unit
        * 6.3.1.1 Ranges and Sets
        * 6.3.1.2 Constructors do not fail anymore on an invalid mask
      * 6.3.2 Translations unit
      * 6.3.3 LazUTF8 unit
      * 6.3.4 LazUTF8Classes and LazUTF8SysUtils units
    * 6.4 Components incompatibility
      * 6.4.1 LazControls
        * 6.4.1.1 TSpinEditExBase derived classes
        * 6.4.1.2 TFloatSpinEditEx
      * 6.4.2 FpVectorial
      * 6.4.3 TurboPower_ipro
  * 7 Other release notes



## LCL Interfaces Changes

### Cocoa

  * IME fully supported, such as Chinese/Japanese/Korean, DeadKeys, Emoji & Symbols.
  * Multi Displays/Monitors fully supported.
  * Docking fully supported, including the IDE.
  * Cursor completely refactored and significantly improved, compatible with macOS Ventura.
  * TPageControl significantly improved.
  * Many controls improved (TComboxBox/TListBox/TDateTimePicker/TStaticText/TSpeedButton/TPanel etc.)
  * Many memory leaks fixed.



### Qt

  * Implemented TCheckBox.Alignment and TRadioButton.Alignment.
  * Implemented TCustomComboBox.AdjustDropDown and TCustomComboBox.ItemWidth.



### Qt5

  * Qt5 uses native event loop on all platforms. C bindings are updated. Minimum C bindings version for lazarus 3.0 is 1.2.15.
  * Note that most Linux Distributions will not have an appropriate libqt5pas library until their next release after the formal release of Lazarus 3.0. Build your own from the Lazarus source tree or download from <https://github.com/davidbannon/libqt5pas/releases/latest>
  * Implemented TCheckBox.Alignment and TRadioButton.Alignment.
  * Implemented TCustomComboBox.AdjustDropDown and TCustomComboBox.ItemWidth.



### Qt6

  * Qt6 widgetset implemented. C bindings are based on Qt6 6.2.0 LTS. Minimum C bindings version for lazarus 3.0 is 6.2.7.
  * Note that most Linux Distributions will not have an appropriate libqt6pas library until their next release after the formal release of Lazarus 3.0. Build your own from the Lazarus source tree or download from <https://github.com/davidbannon/libqt6pas/releases/latest>
  * Implemented TCheckBox.Alignment and TRadioButton.Alignment.
  * Implemented TCustomComboBox.AdjustDropDown and TCustomComboBox.ItemWidth.



### Gtk3

  * Gtk3 pascal bindings are completely reworked.
  * A number of stability improvements.
  * Now requires GTK >= 3.24.24 and Glib2.0 >= 2.66



## LCL Changes

### TCustomImageList

TCustomImageList made more extensible: 

  1. Protected MarkAsChanged method is added, which sets FChanged to true (this allows to make custom triggers for OnChange event).
  2. Virtual protected DoAfterUpdateStarted and DoBeforeUpdateEnded methods are added. They are called in first BeginUpdate and last EndUpdate respectively.



### TTaskDialog

  * Old behaviour Win32: A placeholder icon was used for FooterIcon = tdiNone and MainIcon = tdiNone.
  * New behaviour Win32: No icon is used for FooterIcon = tdiNone and MainIcon = tdiNone.
  * Reason: Removing drawing glitch. The text move over to allow more content and better alignment. See [Issue #39172](<https://bugs.freepascal.org/view.php?id=39172>)



### TSpeedButton

  * Old behaviour: Multi-line captions could only be entered by code. And multi-line captions were always left-aligned.
  * New behaviour: Multi-line captions can also be entered in the object inspector. New property Alignment to specify whether the caption should be left-/right-aligned or centered. Default: centered, like in Delphi.
  * Reason: Better usability.



### TLabel.Transparent, .Color and .ParentColor changes

  * Old behaviour: Transparent property was bound to Color=clNone
  * New behaviour: Transparent is a standalone property
  * Reason: Delphi compatibility and to fix ParentColor issues.
  * Remedy: If you are setting the Color property, Transparent is not automatically switched from True to False now, you have to do it yourself. This is in compliance with Delphi and also solves problems with Color/ParentColor changes.



### TPanel.VerticalAlignment

  * Old behaviour: The panel caption was always centered vertically.
  * New behaviour: The new property VerticalAlignment (taAlignTop, taAlignBottom, taVerticalCenter) allows to place the caption also at the top or bottom of the panel interior.
  * Reason: Delphi compatibility and better usability



### TCalendar

  * Properties MinDate and MaxDate are implemented. These limits are only imposed if MaxDate > MinDate. Unfortunately GTK2/3 widgetsets do not support this, so selecting a date outside the MinDate/MaxDate range will still be possible there.



### TCheckbox, TRadioButton

  * Different calculation of checkbox/radiobutton size in order to correctly take care of Win-10 "ease of access" feature. See [issue #39398](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39398>)
  * Consequence: lfm files will contain different sizes of these controls (if auto-sized) compared with earlier versions.



### Grids

  * You can now set the cell editors properties ParentColor and ParentFont by including goEditorParentColor resp. goEditorParentFont in the grid's Options2 property.



### TShellTreeView

  * New property `ExpandCollapseMode` defining options whether a collapsing node should clear its child nodes.
  * Implements custom sorting of the treeview items by setting `FileSortType` to `fstCustom` and providing a custom compare function in the event `OnSortCompare`.



### TShellListView

  * TShellListView now subclasses TListItem, so it can store file info in it.
  * If you use OnCreateItemClass to create your own descendant of TListItem, it may be advisable to base your own class on TShellListItem instead of on TListItem. This way you will also have access to the TShellListItem's FileInfo property.



### TTreeView

  * Adds ShowSeparators as a published property like ShowLines and ShowRoot.



### TTrayIcon gtk2 Only

If using the gtk2 tray icon, a new global variable is available that will allow you to predetermine which of the two TrayIcon models you use. See the Wiki for details. This is, and will only ever be, gtk2 only. 

## IDE Changes

### Character map

  * Resizable characters to improve readability.
  * The character map was split off from the IDE and moved to separate packages. The designtime package is installed by default so that there is no difference in the IDE. Now users can access it in their own applications after adding the runtime package "charactermappkg.lpk" to the project requirements.



### Debugger

#### Project Options

  * "Run(F9)" can either be "Debug" or "Run without debug".



    In newly created Debug/Release modes, the Release mode will no longer invoke the debugger by default.
    Existing Debug/Release modes must be edited, to enable this.

  * Per project "Debugger backend" settings. In addition to choosing a specific backend from the global IDE settings, a backend can be configured just for the project (i.e. special gdb-server settings)



#### IDE Dialogs

  * Improved _Watches window_
    * Expand/Unfold for classes, records, etc
    * Expand/Unfold with paged browser for arrays
    * Drag and Drop to reorder watches
    * Drag and Drop to create new "top level" watches from nested entries (in expand/unfolded lists)
    * Address column for types with internal pointer (classes, long-string, dyn-array, (real) pointer)
  * Improved _Locals window_
    * Expand/Unfold for classes, records, etc
    * Expand/Unfold with paged browser for arrays
    * Address column for types with internal pointer (classes, long-string, dyn-array, (real) pointer)
    * Power button
  * Improved _Inspect window_
    * Fixed: Updating value when context changes
    * Added options for Function calling / and "Converter" (See FpDebug: SysVarToLstr)
    * Added filter to search for text in name or value.
    * Added ctrl-Up/Down/Page-Up/Page-Down to navigate the grid.  
Alt-Left/Right for history. And ctrl-Enter to select.
  * Improved _Evaluate/Modify window_
    * New Layout
    * Added DisplayFormat
    * Added options for Function calling / and "Converter" (See FpDebug: SysVarToLstr)
  * Improved _Assembler window_
    * Added history navigation (forward/backward)
    * For FpDebug: added annotations to jump/call targets, and allow to ctrl-click to disassemble target address



#### FpDebug / LazDebuggerFp

  * Improved "function calling" in watch eval. See [FpDebug-Watches-FunctionEval](<FpDebug-Watches-FunctionEval.md> "FpDebug-Watches-FunctionEval")
  * %RAX Accessing cpu registers in watch expression (only full registers, not yet AH or AL or EAX on 64bit)
  * Intrinsic functions: [FpDebug-Watches-Intrinsic-Functions](<FpDebug-Watches-Intrinsic-Functions.md> "FpDebug-Watches-Intrinsic-Functions")
  * Intrinsic/extended operators: MyArray[1..3] array slice with operator mapping. [FpDebug-Watches-Intrinsic-Functions#Intrinsic_Operators](<FpDebug-Watches-Intrinsic-Functions.md> "FpDebug-Watches-Intrinsic-Functions")
  * Option to detect "variant" and call "SysVarToLStr" in the target app.
  * Suspend/Resume individual threads (must be done while app is paused, and will be applied for subsequent step/run)
  * Partial improvements to debug in DLL: [https://wiki.freepascal.org/Debugger_Status#Other](<Debugger_Status.md>)
  * Disassembler now annotates lines for call/jmp/jne/... with info on the target address (function name, file, line)
  * F7/F8 Step-Into/Over can now be used to start the debugger and run to the first line of the main program begin/end.



#### LazDebuggerFpLldb (default on MacOS)

  * Added Mem-Limits (and String,Pchar,Array) to the debugger config



    See [https://wiki.freepascal.org/LazDebuggerFp](<LazDebuggerFp.md>) (MaxMemReadSize, MaxStringLen, MaxArrayLen, MaxNullStringSearchLen)
    Limiting the default results for watches/locals/stack-params,... can prevent slow evaluation.
    Arrays can then be browsed in the watches window, using the new "paged browser for arrays" (expand via [+])

### Floating point properties in the Object Inspector

  * The Object Inspector now explicitely disallows to set a floating point property's value to +/-Inf or NaN.
  * Reason: whilst +/-Inf and NaN are valid values for a floating point property, they cannot be streamed (so, the form could not be loaded) and setting it to NaN caused havoc in the IDE.
  * Remedy: set the value at runtime (in code)



### Reading unit names in lfm

FPC 3.3.1 component writer supports optionally writing types with their unit names as _unitname/type_. Lazarus can now read this. 

### Ambiguous Classtypes

You can now register two component classes with the same name, e.g. _fresnel.TButton_ and _StdCtrls.TButton_. You can even put both on the same form. 

### Editor

\- Highlight for PasDoc 

### IDE Options

In Tools -> Options -> Environment -> Window page these three settings are now **ON** by default : 

  * IDE title starts with project name
  * IDE title shows project directory
  * IDE title shows selected build mode



It has been requested by many users. If the IDE's title bar has no info about the active project, a user must open Project Inspector or Project Options to see it, which is inconvenient. 

### Lazarus Examples

A new approach to Examples that copies an Example to a fresh working area (avoiding the Linux Read Only Problem) and, possibly an easier to use search model. 

## IDE Interface Changes

## Components

### TAChart

  * The **TLegendClickTools** now is able to detect clicks on series legend items and reports the clicked series in the new **OnSeriesClick** event.
  * New **TDatapointMarksClickTool** which becomes active when the user clicked on the marks of a series.
  * New property **TickWidth** for the chart axes.
  * New property **FullWidth** for the chart title and footer to run their background across the entire chart width.
  * New property **RandomColors** for `TRandomChartSource`.
  * New properties **YIndexWhiskerMin** , **YIndexBarMin** , **YIndexCenter** , **YIndexBarMax** , **YIndexWhiskerMax** , and **YDataLayout** in TBoxAndWhiskerSeries for more flexible assignment of y values to the parts of the box/whisher shape.
  * New option **aipInteger** in the set TAxisIntervalParamOptions which sets axis labels only at integer values and thus supresses the unwanted intermediate labels in bar charts and helps to enforce labels in logarithmic plots at the powers of the logarithmic base (usually 10).
  * New event **OnAddStyleToLegend** for TChartStyles. It has a boolean parameter `AddToLegend` with which you can determine whether the series level using this style is displayed in the legend.



### TDateTimePicker

  * New properties MonthDisplay and CustomMonthNames. They are meant to replace the MonthNames property, which has been deprecated.
  * New property DecimalSeparator. Allows a user-specified value to be used instead of a hard-coded Colon character.



### TDBDateTimePicker

  * Adds Options as a published property.
  * Publishes the DecimalSeparator property.
  * Publishes the missing Alignment property. Consistent with TDateTimePicker,



### T(Float)SpinEditEx

  * New property **Orientation** which allows to arrange the spin buttons horizontally.



### TCheckListBox

  * Adds HeaderColor and HeaderBackgroundColor properties. Used on list items where the Header property is enabled. Implemented for the Win32 widget set.



### Pas2js

  * lazbuild now can compile pas2js projects by passing the environment variable PAS2JS with the path of the pas2js executable.
  * Project groups with pas2js projects now can compile without being opened.
  * New project type [Progressive Web Application](<lazarus_pas2js_integration.md> "lazarus pas2js integration")
  * New project type [Electron Web Application](<lazarus_pas2js_integration.md> "lazarus pas2js integration")
  * pas2jsdsgn now uses the SimpleWebServerGUI package, replacing its own http server controller.
  * F9, Run now builds, starts a HTTP server and a browser



### Lazarus Icon Collection

  * Not a component, but the Lazarus installation now contains a folder with general-purpose icons for usage in toolbars, menus, buttons etc. of any GUI applications (folder _images/general_purpose_).
  * The images come in various sizes and thus are compatible with the scaled image list of Lazarus v2.0+.
  * Author: Roland Hahn (<https://www.rhsoft.de/>).
  * License: Creative Commons CC0 (no restrictions in usage).



### gir2pascal

  * Sources of [gir2pascal](<gir2pascal.md> "gir2pascal") (a tool to convert [GObject Introspection](<https://gi.readthedocs.io/>) descriptions to Pascal files) are now included in Lazarus source tree ([tools/gir2pascal](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/tree/main/tools/gir2pascal>) directory) and maintained there.



### lazdelphi

An IDE addon adding a parser for the Delphi compiler errors and hints. You can run dcc32.exe as external tool or **execute before** command in the compiler options and use the _Delphi Compiler_ parser for the output, so that the errors/hints in the Messages window can open the source. See [Lazarus Delphi Compiler Tool](<Lazarus_Delphi_Compiler_Tool.md> "Lazarus Delphi Compiler Tool")

### Jedi Code Format

  * Some improvements in code formatting.
  * The jcf command line tool is now a text-mode application and no longer requires XWindow on linux to run.



### Sparta_DockedFormEditor

In case you are still using the deprecated **sparta_dockedformeditor** , use the **dockedformeditor** package instead. 

## Changes affecting compatibility

### IDE Settings

#### Run > Run Parameters (UPDATED FOR 3.2)

The changes originally described introduced a regression. \- They do apply to Lazarus 3.0 \- As off Lazarus 3.2 (fixes release) the behaviour has been updated 
    
    
      1. If a non-empty string is set in "run params" \> "working directory", then this will be used as working directory.  
    This should be "as before"
         1. IDE-Macros are supported in this path.
         2. If that path is relative, then this was not previously supported. It will be resolved against the "Project directory", not the "Output path"  
    This is, because the "Output path" is only used as default Woring dir, if there is on "Host App". And resolving a relative directory against different bases can more easily lead to mistakes.  
    Also the "Project dir" is used as base to resolve a relative "Host app" too.
      2. If there is no user-specified "working directory" then the path of the "target exe" is used.
         1. If a host app is given, the path of the host app is used.
            1. Macros in the host app are allowed
            2. A relative host app is resolved against the "Project dir"
         2. If there is no host app, the path of the compiled project executable is used. (This should be the "output patch")
            1. Macros in the "output path" are allowed
            2. A relative path is resolved against the "Project dir"
      3. If that does not return a path (if there is neither a project exe, nor a ho
    

Also see <https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40693#note_1727379327>

~~If your program uses relative paths but the executable is not in the same directory as the main project file (usually with .lpr extension), the IDE will set different current directory for the program as you expected. The current directory set will point to the directory with main project file instead of the directory where the executable is generated, so all relative paths used in the program will be invalid (ParamStr(0) and all other current-directory-specific functions will return wrong path). This happens only if you run your program using IDE (for example, using F9 key). This behavior was modified in the Lazarus 3.0 and breaks the backward compatibility.~~

~~

The Working-dir is now determined as follows
    
~~~~

  1. BuildMode.RunParams "working directory" set by user (New, this can now be relative to project dir)
  2. Project Dir, if not virtual
  3. Directory from Host-App (RunParams), or if (and only if) Host-App is empty from Project.exe (If host app is in %PATH, then there is no "working directory")

~~~~~~

The first 2 steps are the same as before the change. The 3rd step was previously only used for "debugging", but "run without debug" did use: "Launch-App", "Host-App", Project.exe.

~~

Change
    The path of the "Launch App" is no longer considered.
~~~~

The Launch-/Host-App location are now determined as follows
    
~~~~

  1. An app with absolute path is used as given.
  2. A relative path (including no path at all) is resolved as relative to the Project-dir.
  3. An app without any path at all (if not found in step 2) is searched in the %PATH environment.

~~~~

Change
    Search in %PATH was only done by "run without debug", but not by debug. It is now done by both.
Change
    Checking for an exe relative to the project-dir was added.
~~~~

Remedy
    So now, if you need to use relative paths in the program and be able to run/debug your program using IDE, just go to the "Run Parameters" window and set the
~~~~
    
    
    $Path($(OutputFile))

~~~~~~

~~string in the "Working directory" field.~~

### LCL incompatibility

#### TLabel: autosized and right-aligned

  * Old behaviour: Autosized label with Alignment=taRightJustify but Anchors=[akLeft,...] grew to left.
  * New behaviour: The label grows to right now.
  * Reason: It wasn't possible to implement the behavior also for hidden labels without significant extensions in the LCL. The LCL has a different and more generic feature of control-based anchoring that delivers the same effect (see Remedy down), so it is not needed and wanted to double this feature and make the LCL code more complex and prone to bugs.
  * Remedy: Use the LCL anchoring to a secondary control. Anchor the right side of the label to another control. Then the autosized label will grow to the left but won't move to the right when the parent is resized like it is done with a simple akRight anchor without a reference control.



#### TDateEdit/TTimeEdit

The value of NullDate has changed.  
Reason: 

  * It was impossible to actually select the date corresponding to NullDate (30 dec 1899 by default) in the control.
  * Remedy (1): if your code depended on NullDate actually being 0.0, you have to adjust your code.
  * Remedy (2): if your code used NullDate for a TTimeEdit, change that to the new constant NullTime instead.
  * Note: NullDate is actually a writeable constant. This was kept for compatibility reasons. It is however a bad idea to change it's value to anything that is an actual date that is within the range of the control.



#### Cocoa

Some global configuration variables were moved from CocoaInt to CocoaConfig. such as CocoaBasePPI, CocoaIconUse, CocoaToggleBezel, CocoaToggleType etc. 

#### GTK3

GTK3 is no longer supported on earlier Linuxes, such as Ubuntu 20.04. It requires GTK >= 3.24.24 and Glib2.0 >= 2.66 

#### TControl: ChildSizing

  * Old behavior: When a control overrides `AdjustClientRect` and children are aligned via `ChildSizing` the adjusted clientrect was ignored. A typical example is a `TPanel` with a wide `BevelWidth` where the children were moved into the bevel.
  * New behavior: The children now are aligned with respect to the adjusted clientrect. In the example of the `TPanel`, the child controls are now positioned such that the bevel is not covered.
  * Reason: unexpected behavior which also contradicts the behavior of `Align` or `AnchorSides` which do respect the adjusted clientrect.
  * Remedy: In the rare case that users have met this situation and have compensated the incorrect `ChildSizing` layout by additional border spacings, these additional corrections must be reverted.



### LazUtils

#### Masks unit

The masks unit has been completely rewritten.  
Reasons: 

  * speed: the old Matches() method had O(n^2) or even O(n^3) characteristics.
  * improved control over how the mask is interpreted.



New types (for parameters) and a dedicated TMaskWindows class have been added.  
TMask.MatchesWindowsMask and the old TMaskOptions type have been deprecated and will be removed in the next release.  


##### Ranges and Sets

  * The old masks implementation supported sets, but not ranges. The new implementation supports both sets ([abc]) and ranges ([a-c]). As a consequence a '-' inside such a construct is now interpreted as part of the range definition, not as a literal '-'.
  * Reason: ranges are a good thing to have by default (the old implementation simply lacked this). We decided it's a small price to pay.
  * Remedy: either escape the '-' with EscapeChar (which defaults to '\') or exclude mocRange from the TMaskOpcodes parameter.



##### Constructors do not fail anymore on an invalid mask

  * When providing an invalid mask to the old T(Windows)Mask(List) constructors an exception was raised.
  * The new constructors do not raise an exception in this case. Instead an exception is raised in when Matches() is called.
  * Reason: it's not very nice to have a constructor fail.



#### Translations unit

Added GetLanguageID function. It returns a record with language code (in ISO 639-1 or ISO 639-2) and country code (in ISO 3166) for current system locale.  
Added GetLanguageIDFromLocaleName function. It parses Unix locale name and returns a record with language code and country code. It is useful to parse language identifiers passed e. g. via command-line parameters. 

Implementation is based on GetLanguageIDs procedure from GetText unit, but is rewritten to have the following properties: 

  * Language and country codes are always returned in ISO formats on Windows.
  * Unix locale identifier is properly parsed and language/country codes are properly extracted.
  * Don't assume that language code is always two-letter (ISO 639-1), it can have bigger length (e. g. three letters, like in ISO 639-2).
  * Locale ID is returned in a record type. This will allow to return additional fields in backwards-compatible manner in future. Currently it contains language code, country code and language ID (combination of language code and country code).



These functions are used now throughout the Lazarus codebase. This greatly improves automatic language detection and loading of correct translations by Lazarus: 

  * Three-letter (ISO 639-2) language identifiers are no more truncated to two letters. Thus, translations for such languages will be correctly loaded when available.
  * Previously on Windows some language and country codes were obtained in non-ISO format, which prevented correct loading of some translations, e. g. Chinese (zh_CN).
  * On Unix translations with country codes, like Brazilian Portuguese (pt_BR) or Chinese (zh_CN) are correctly loaded now.
  * macOS is now handled as any other Unix. This removes dependency on language list in Lazarus bundle (which had to be maintained manually) and thus fixes loading of Czech, Hungarian, Brazilian Portuguese, Ukrainian translations.



#### LazUTF8 unit

  * Deprecated LazGetLanguageIDs (returns combination of language and country codes) and LazGetShortLanguageID (returns only language code) procedures.
  * Reason: this functionality belongs to Translations unit (calls of these procedures are almost always followed by calls to procedures from Translations unit), and these procedures are now thin wrappers of GetLanguageID function from Translations unit.
  * Remedy: use GetLanguageID function from Translations unit.



#### LazUTF8Classes and LazUTF8SysUtils units

Everything in these units was deprecated for a long time and now they were removed. LazUTF8SysUtils was earlier renamed to LazSysUtils and this deprecated version was left for a transit period. 

  * Class TStringListUTF8 can be replaced with TStringList.
  * Class TMemoryStreamUTF8 can be replaced with TMemoryStream.
  * Global procedure LoadStringsFromFileUTF8 can be replaced with TStrings.LoadFromFile.
  * Global procedure SaveStringsToFileUTF8 can be replaced with TStrings.SaveToFile.
  * Functions NowUTC and GetTickCount64 can be found in LazSysUtils.



### Components incompatibility

#### LazControls

###### TSpinEditExBase derived classes

  * All derived classes form TSpinEditExBase must implement a SameValue method. This method is defined as an **abstract** method in TSpinEditExBase.
  * Reason: All derived classes used Math.SameValue. This is wrong for comparing integer types (even if is is safe when comparing relative small values).
  * Remedy: unfortunately you'll have to adjust your code.



###### TFloatSpinEditEx

  * The property NumbersOnly is no longer published.
  * Reason: the property makes no sense for this control and only confuses users.
  * Remedy: if you really need NumbersOnly to be True, you must set it in code.



#### FpVectorial

  * The `Size` element in the FPVectorial `TvFont` record is a floating point value now (type `double`).
  * Reason: Avoid rounding errors because the drawing coordinates are `double` already.
  * Remedy: There is rarely a chance that this change will have an effect on user code. Only when the font size is stored in a variable it must be declared as `double` rather than as `integer`.



#### TurboPower_ipro

  * Type declarations and the html nodes were moved from unit IpHtml to separate units, IpHtmlTypes, IpHtmlClasses and IpHtmlNodes. This may break compilation of existing projects.
  * Reason: Improve maintainability of the extremely long unit IpHtml
  * Remedy: Add IpHtmlTypes, IpHtmlClasses and/or IpHtmlNodes to the uses clause of the project unit(s) when an "identifier not found" error referring to this package is reported by the compiler.



## Other release notes

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | 3.0 | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/Lazarus_2.4.0_release_notes](https://web.archive.org/web/20250122103205/https://wiki.freepascal.org/Lazarus_2.4.0_release_notes)_
