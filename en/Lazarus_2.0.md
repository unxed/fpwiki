# Lazarus 2.0.0 release notes

**Lazarus 2.0.0 was released on 2019-02-05** [[release announcement](<http://forum.lazarus.freepascal.org/index.php/topic,44161.0.html>)] 

## Contents

  * 1 LCL Interfaces Changes
  * 2 LCL Changes
    * 2.1 TScrollingWinControl (TForm, TScrollBox, TFrame)
    * 2.2 Support for mouse wheel horz scrolling
    * 2.3 Compiler defines to exclude some graphics support
    * 2.4 TCustomImageList / TImageList
    * 2.5 TSpeedButton, TBitBtn
    * 2.6 TWinControl.DoubleBuffered, .ParentDoubleBuffered and TApplication.DoubleBuffered
    * 2.7 TListView
    * 2.8 TTreeView
    * 2.9 FilterOptions in filter controls
    * 2.10 Advanced menus Assign
    * 2.11 TPageSetupDialog
  * 3 IDE Changes
    * 3.1 Editor
    * 3.2 Debugger
      * 3.2.1 LLDB based Debugger (New)
      * 3.2.2 GDB based Debugger
  * 4 IDE Interfaces Changes
  * 5 Components
    * 5.1 TOpenGLControl
    * 5.2 TAChart
    * 5.3 TCustomEdit
  * 6 Changes affecting compatibility
    * 6.1 Installer
      * 6.1.1 Debian package fpc was renamed to fpc-laz
    * 6.2 LazUtils
    * 6.3 LCL incompatibilities
      * 6.3.1 TToolBar children ignore Align
      * 6.3.2 TCustomComboBox.ReadOnly was deprecated
      * 6.3.3 Predefined clipboard format pcfDelphiBitmap was removed
      * 6.3.4 TEdit.Action visibility lowered to public
      * 6.3.5 TControl.ScaleFontsPPI, .DoScaleFontPPI parameter change
      * 6.3.6 MouseEntered deprecated/missing
      * 6.3.7 TCustomImageList.Add method
      * 6.3.8 TBitBtn.Glyph in combination with TBitBtn.Kind
      * 6.3.9 TCustomTreeView.OnChanging event: Node parameter
      * 6.3.10 TCustomDialog properties Width and Height visibility lowered to public
      * 6.3.11 No LCL Application exception dump
      * 6.3.12 No default LazLogger
      * 6.3.13 Screenshot for LCLExceptionStackTrace and LazLogger Additions and Overrides
      * 6.3.14 TCustomTabControl setting TabIndex or PageIndex by code
      * 6.3.15 TCustomGrid: The var parameter ABitmap in event OnUserCheckBoxBitmap used to be preassigned with default bitmap, but now it is nil
    * 6.4 Components incompatibilities
      * 6.4.1 LazControls: TSpinEditEx no longer inherits from TCustomFloatSpinEditEx
      * 6.4.2 TAChart: Reticule-related properties deprecated
      * 6.4.3 TAChart events On[After|Before]Draw[Background|BackwallWall] deprecated
      * 6.4.4 TAChart: The TCubicSplineOption csoDrawFewPoints is removed.
      * 6.4.5 TAChart: Other properties removed that had been marked as deprecated for a long time
    * 6.5 IDE incompatibilities
  * 7 Previous release notes



# LCL Interfaces Changes

# LCL Changes

### TScrollingWinControl ([TForm](<TForm.md> "TForm"), [TScrollBox](<TScrollBox.md> "TScrollBox"), [TFrame](<TFrame.md> "TFrame"))

  * ScreenToClient and ClientToScreen are now calculated without scrollbar offset. Done for Delphi compatibility.



### Support for mouse wheel horz scrolling

Added TControl events for horizontal mouse wheel moving (special mice with horz scroll buttons for PC, and modern mice for Mac). OnMouseWheelHorz, OnMouseWheelLeft, OnMouseWheelRight. Events are used in some non native LCL components (TreeView, UpDown, SpinEdit). Native components should handle horz scrolling already. 

### Compiler defines to exclude some graphics support

Added flags to exclude support for some graphics formats to create smaller applications: 

  * -dDisableLCLGIF
  * -dDisableLCLJPEG
  * -dDisableLCLPNM
  * -dDisableLCLTIFF



### TCustomImageList / TImageList

  * the image list now supports multiple resolutions of one image. See [Multiple-resolution TImageList in Lazarus 1.9 and newer](<TImageList.md> "TImageList") for more details. As a result all LCL controls support High-DPI glyphs on Windows+Linux and Retina on Mac without any additional code.
  * every LCL control that supports ImageList has now a new ImagesWidth property to decide what custom width at 96 PPI (100% scale) is to be used. Example: TToolBar.Images/.ImagesWidth, TListView.LargeImages/.LargeImagesWidth, .StateImages/.StateImagesWidth.
  * set the TCustomImageList.Scaled=True property to let the image list automatically pick up the right resolution for your control.



### TSpeedButton, TBitBtn

  * New properties Images, ImageIndex and ImageWidth. With them full ImageList support was added. 
    * no need to save the same Glyph in LFM all over the application
    * automatic high-DPI image handling



### TWinControl.DoubleBuffered, .ParentDoubleBuffered and TApplication.DoubleBuffered

  * Note: DoubleBuffered is a LCLWin32-only feature
  * Old behavior: DoubleBuffered wasn't properly implemented and it was forced True
  * New behavior: 
    * A Delphi-compatible DoubleBuffered/ParentDoubleBuffered concept was created (it is equal to the Font/ParentFont concept).
    * The LCL has the TApplication.DoubleBuffered extension over Delphi that allows you to set form's default DoubleBuffered value globally for the whole application (set Application.DoubleBuffered before creating the first form). The value is then applied to all controls on the form with ParentDoubleBuffered:=True.
    * DoubleBuffered is True by default unless in remote session (this is different to Delphi where DoubleBuffered is False by default).
    * If you need one specific control to be DoubleBuffered:=False even for Application.DoubleBuffered:=adbDefault, make sure you set control.DoubleBuffered:=False and .ParentDoubleBuffered:=False.



### TListView

Added CustomSort method like in Delphi. 

### TTreeView

  * Implemented HotTrack property (show blue underline for item under cursor).
  * Implemented auto-scrolling up/down during drag-drop (when mouse reaches the edge of treeview).



### FilterOptions in filter controls

Base class for filter controls TCustomControlFilterEdit has new property FilterOptions, which is set of such flags: 
    
    
     TFilterStringOption = (fsoCaseSensitive, fsoMatchOnlyAtStart);
     TFilterStringOptions = set of TFilterStringOption;
    

Two flags are implemented for 3 filter controls in LazControls: TListFilterEdit, TListViewFilterEdit, TTreeFilterEdit. 

### Advanced menus Assign

  * Supported TMenuItem.Assign(TMenuItem): this copies all props of menuitem.
  * Supported TMenu.Assign(TMenu): this copies all items with all nested subitems from one menu to another (TMainMenu, TPopupMenu).



### TPageSetupDialog

  * TPageSetupDialog: added Margin* and Units properties.
  * Big rework of Unix TPageSetupDialog, it is now complete like it's on Windows (on Windows it was native, on Unix - emulation).



# IDE Changes

  * several **High-DPI IDE improvements** and retina support on Cocoa
  * **Delphi Attributes** : Find declaration, parameter hints, $modeswitch prefixedattributes.
  * The IDE parses the custom compiler options for the **fpc switch -FN <namespaces>**, to define the default namespaces, so you can omit the namespaces in the uses sections.
  * [pas2js](<pas2js.md> "pas2js") support: 
    * New IDE package **[pas2jsdsgn](<lazarus_pas2js_integration.md> "lazarus pas2js integration")** (not installed by default): 
      * You can create a browser or nodejs webapplication
      * on '_Run_ starts the webapp in your webbrowser
    * pas2js settings are automatically fetched, same as fpc settings
    * quickfixes work with pas2js messages
  * added quickfix for fpc message "inherited method is hidden": add modifier _override_ , _overload_ , or _reintroduce_ shortcuts.
  * added designer menu item to hide icons for components like TOpenDialog. Option: **Show non visual components**
  * extended filter for the identifier completion window - include identifiers containing prefix. (Settings: IDE Options -> Codetools -> Identifier Completion -> Sorting -> Include identifiers containing prefix.) See <https://bugs.freepascal.org/view.php?id=32974>.
  * the identifier completion window includes also text (words) from the currently opened units. It can be disabled or set-up in IDE Options -> Codetools -> Identifier Completion -> Include words
  * the Publish Project/Package feature and GUI window was revamped. Now all project/package member files are included automatically. More files can be added using a filter. The directory structure is maintained even if some files are in directories "above" the main directory.
  * the "New Component" window and user interface is improved. It also now supports 3 icons for different resolutions.
  * the Run -> Compile many Modes... feature now remembers its previously selected modes.
  * the Search -> Procedure List... window now remembers the state of its filter buttons.



## Editor

  * Added new **"smart move cursor"** moving commands to editor mappings. They introduce word jumping positions both at word starts and ends. They are useful as alternative Ctrl+Left/+Right/+Shift+Left/+Shift+Right key
  * Added Goto/Toggle bookmark submenu to toolbar.



    Ctrl-b pops up "goto bookmark" list, with location for each bookmark. Ctrl-Shift-b pops up "toggle bookmark" list.

  * Sublime like handling of selection with right/left navigation keys. Remove selection and keep caret at ex-boundary of selection. To activate check both: "Caret skips selection" and "Caret left/rigt clears selection (no move)". See [mantis 26477](<https://bugs.freepascal.org/view.php?id=26477>)
  * More options to fine tune outline colors



## Debugger

  * Auto closing of the asm window, if it was opened by breaking at a none source line. <https://bugs.freepascal.org/view.php?id=27800#c109463>
  * Dragging selected identifier from source editor to watches window, to create a watch



### LLDB based Debugger (New)

The main target for this debugger is the Mac environment, which comes with a pre-installed lldb. For other platforms the use of the GDB based debugger is highly recommended. 

The LLDB debugger depends on the availability and correct function of the lldb executable. 

  * Alpha: LLDB based debugger for MacOs (no code-signing required)



    This is a raw LLDB wrapper. All watches must be lldb compatible. This may require C-style expressions entered into the watch window.
    This debugger is experimental and provided "as is".

  * Beta: LLDB + FpDebug based debugger for MacOs (no code-signing required)



    This is the recommended LLDB based debugger.

To use the lldb based debugger: 

Open the IDE and install the package "LazDebuggerFpLLdb"
    

  * It should be in the list of available packages, but if not it is in components/lazdebuggers/lazdebuggerfplldb
  * Make sure to use the one with "Fp" in the name. (There also is LazDebuggerLLdb, but it is not as good.)
  * Restart the IDE.



Go to Tools > Options > Debugger
    

  * In the "debugger type" dropdown, you should find and select "LLDB debugger (with fpdebug)"
  * The edit below this (where you normally have the path to gdb), should be changed to the path of lldb.



    (lldb comes with the tools from Apple; and as such it is already codesigned by Apple).

In case of problems please see: <http://forum.lazarus-ide.org/index.php/topic,42869.0.html>

### GDB based Debugger

GDB Debugger new options
    

  * Added option "FixStackFrameForFpcAssert" to workaround fpc wrong frame pointer (display correct line after assert failed)
  * Added option "FixIncorrectStepOver" to workaround "step over" issues experienced on some platforms (step-over acts like step-in) <http://forum.lazarus-ide.org/index.php/topic,44121>
  * Added option "AssemblerStyle": ATT vs Intel
  * Added option "DisableStartupShell": Required on MacOs.
  * Added more size limits for data evaluation (avoid errors, timeouts and extremely slow responses)



    Some users may experience a small speed gain, when setting both of the "...LengthForStaticArray" values to an equal value, and both of the "...ValueMemLimit" to an equal value (the mem and length may differ,this has no effect) 

  * MaxDisplayLengthForStaticArray: Similar to existing "MaxDisplayLengthForString". This should apply to static array, but it is up to gdb which types it applies it to.
  * MaxLocalsLengthForStaticArray: The same, but applied while getting values for the locals window, and the function parameters show in the stack window. Should be more restrictive.
  * GdbValueMemLimit: For all types. Do not evaluate values, if gdb would need more memory. Setting this to big may crash gdb. Big values may also lead to slow response times (several minutes during which the IDE would be blocked)
  * GdbLocalsValueMemLimit: For locals and stack.



  * DebugServer: Added "target-download" if remote supports it.



# IDE Interfaces Changes

  * Added **FormEditingHook.SaveComponentAsPascal** , which stores a designer form as Pascal statements using **TCompWriterPas**. You have various options to define the format and it tells you what units are needed for the Pascal code. There is an example adding a designer menu item to copy the Pascal statements to the clipboard _examples/pascalstream/CopyAsPasPkg/copyaspasdemounit1.pas_.


  * Added interface class TLazCompilationToolOptions with a Command and CompileReasons. Used in CompilerOptions.ExecuteBefore and .ExecuteAfter.



# Components

### TOpenGLControl

  * New property Options of type set, currently with ocoMacRetinaMode as the only member. If set, ocoMacRetinaMode determines that the OpenGL controls will use retina support (high resolution mode).
  * Control can use Qt5 widgeset now, Qt4 is also fixed for modern OpenGL contexts.



### TAChart

  * The new TExpressionSeries and TExpressionColorMapSeries plot mathematical functions at design-time.
  * TLineSeries has a new property "ColorEach" which can be used to color the line segments individually.
  * TCubicSplineSeries has a new property "SplineType" which allows to select between the "natural" and the "monotone Hermite" splines (the latter avoiding overshoot of the interpolation).
  * TAreaSeris has a new property "Banded" which suppresses painting of the bottom layer of stacked areas and makes the series look like a band or a stack of bands.
  * The new property "MarkDistancePercent" of TPieSeries can be used, for example, to center a label within its pie for all chart sizes.
  * TLineSeries, TCubicSplineSeries, TBSplineSeries and TFitSeries now can display error bars.
  * The TFitSeries fitting engine was extended to be able to exclude specific parameters from fitting and to do a statistical analysis of the fit results (error estimates of the determined fit parameters, confidence limits, goodness of fit).
  * TBarSeries has new property "MarkPositionCentered" which allows to center series marks within each bar. Available also for TAreaSeries where it works accordingly. NOTE: In intermediate trunk versions and some of the release candidates, this effect could be achieved partly by using option lmpInsideCenter of TMarkPositions - this option has been dropped now.



### TCustomEdit

  * Qt5 widgetset native implementation of TextHint property, Qt4 uses emulated TextHint.



# Changes affecting compatibility

## Installer

### Debian package fpc was renamed to fpc-laz

This about the packages provided by the Lazarus team on sourceforge. 

  * Old: fpc_3.0.4-3_amd64.deb, fpc_3.0.4-3_i386.deb
  * New: fpc-laz_3.0.4-1_amd64.deb, fpc-laz_3.0.4-1_i386.deb
  * Reason: avoiding name clash with Ubuntu/Debian repository package "fpc"
  * Remedy: adapt your scripts



Note, that the fpc-src package keeps its name, as the Ubuntu/Debian package is fpc-source, so there is no name clash here. 

## LazUtils

## LCL incompatibilities

### TToolBar children ignore Align

Controls placed within TToolBar cannot be aligned with the Align property. 

  * Old behavior: The Align property of TToolBar children was taken into account.
  * New behavior: The Align property is ignored.
  * Reason: It caused endless alignment loops when the control's size was changed by code in Create.
  * Remedy: use a parent container for TToolBar and align the controls within this container.



### TCustomComboBox.ReadOnly was deprecated

  * Old behavior: When True, only items from the list are accepted, by direct selection from the list or AutoComplete.
  * New behavior: it does nothing and will be removed.
  * Reason: Delphi-compatibility, confusing naming, WS compatibility (different behavior on Win32/Qt/Gtk)
  * Remedy: Use extended styles for the same feature.



### Predefined clipboard format pcfDelphiBitmap was removed

  * Old behavior: The enumeration TPredefineClipboardFormat contained an element pcfDelphiBitmap which once had to be introduced due to streaming differences of bitmaps between Delphi and LCL.
  * New behavior: pcfDelphiBitmap has been removed. (Rare) code relying on exact count and position of the elements of this enumeration will fail.
  * Reason: No longer needed and causing trouble in clipboard access.
  * Remedy: None - revisit your code.



### TEdit.Action visibility lowered to public

  * Old behavior: TEdit.Action was published.
  * New behavior: TEdit.Action is now public.
  * Reason: Delphi compatibility.
  * Remedy: set Action in code.



### [TControl](</index.php?title=TControl&action=edit&redlink=1> "TControl \(page does not exist\)").ScaleFontsPPI, .DoScaleFontPPI parameter change

  * Old behavior: No AToPPI parameter
  * New behavior: AToPPI parameter was added
  * Reason: font scaling problem [Issue #32882](<https://bugs.freepascal.org/view.php?id=32882>). This change will be merged to 1.8.1
  * Remedy: fix parameters



### MouseEntered deprecated/missing

  * Old behavior: No warning on using MouseEntered
  * New behavior: Warning: Symbol "MouseEntered" is deprecated: "use MouseInClient instead"
  * Reason: Delphi compatibility
  * Remedy: use property MouseInClient instead



### TCustomImageList.Add method

  * Old behavior: the image got sliced if too big or extended if too small.
  * New behavior: the image is scaled to all resolutions in the image list.
  * Reason: Image List now supports multiple resolutions.
  * Remedy: use AddSliced (if the image consists of several icons to be added) or AddSlice (if one image from a custom rect has to be added - also rect outside the image is supported).



### TBitBtn.Glyph in combination with TBitBtn.Kind

  * Old behavior: The Glyph property was filled with a valid bitmap when the Kind property was set to a different value from bkCustom.
  * New behavior: The Glyph property is empty.
  * Reason: TBitBtn now supports image list with multiple resolutions. The Glyph (TBitmap) doesn't support multiple resolutions and thus cannot be used anymore to define the TBitBtn image.
  * Remedy: if you need the image for a specific TBitBtn.Kind get it directly from the LCLGlyphs property defined in ImgList.



### TCustomTreeView.OnChanging event: Node parameter

  * Old behvior: The parameter Node passed to the OnChanging event points to the currently selected node. This is not compatible with Delphi which has the destination node here.
  * New behavior: The parameter Node passed to the OnChanging event is the node which will be the selected node after the node-changing operation has completed.
  * Reasons: 
    * The operation can be aborted by setting "AllowChange" to false. For this decision, knowledge of the node going to be selected is helpfull, at least more helpful than knowledge of the node to be left which still can be accessed as TreeView.Selected at this point. The old behavior does not tell the new node.
    * The new behavior is compatible with Delphi
  * Remedy: If old OnChanging handlers refere to the parameter Node replace Node by TreeView.Selected.



### TCustomDialog properties Width and Height visibility lowered to public

  * Old behaviour: Width and Height were published.
  * New behaviour: Width and Height are public.
  * Reason: Delphi compatibility.
  * Remedy: set the properties in code.



### No LCL Application exception dump

  * Old behavior: In case of an exception a dump was automatically written with [DebugLn](<DebugLn.md> "DebugLn") (e.g. to a console).
  * New behavior: There is no automatic exception dump any more.
  * Reason: The LCL should not emit debugging info by default.
  * Remedy 1: Add the LCLExceptionStackTrace unit to the uses clause of any of your units.
  * Remedy 2: To enable LCLExceptionStacktrace globally for all projects, create a new "Additions and Overrides" entry in "Stored in IDE" for "#project" target with the contents "-FaLCLExceptionStacktrace" (see screenshot below). (The drawback of this approach is that this will now also be used for console applications, so they won't compile unless you uncheck the addition in Compiler Options.)



### No default LazLogger

  * Old behavior: LazLogger was registered by default by the LCL.
  * New behavior: LazLogger is not registered any more, so there is an empty logger (LazLoggerBase) used with no output.
  * Reason: The LCL should not register any logger by default - the user has to define if he wants to use the logger and what logger he wants to use.
  * Remedy 1: Add the LazLogger unit to the uses clause of any of your units.
  * Remedy 2: To enable LazLogger globally for all projects, create a new "Additions and Overrides" entry in "Stored in IDE" for "#project" target with the contents "-FaLazLogger" (see screenshot below). (The drawback of this approach is that this will now also be used for console applications, so they won't compile unless you uncheck the addition in Compiler Options.)



### Screenshot for LCLExceptionStackTrace and LazLogger Additions and Overrides

[![AdditionsOverridesExUnits.png](https://wiki.freepascal.org/images/9/96/AdditionsOverridesExUnits.png)](</File:AdditionsOverridesExUnits.png>)

### TCustomTabControl setting TabIndex or PageIndex by code

  * Old behavior: setting TabIndex or PageIndex by code would invoke a call to CanChangePageIndex/CanChange
  * New behavior: setting TabIndex or PageIndex by code will not invoke CanChangePageIndex/CanChange. As a result OnChanging is not called in this scenario.
  * Reason: Delphi compatibility.
  * Remedy: set nboDoChangeOnSetIndex in Options (this option is not available in Delphi), call OnChanging in you own code when changing TabIndex/PageIndex, or move the code in OnChanging (or an overridden CanChangePageIndex/CanChange) to another event (or method) if applicable.



### TCustomGrid: The var parameter ABitmap in event OnUserCheckBoxBitmap used to be preassigned with default bitmap, but now it is nil

  * Old behaviour: You could draw directly to ABitmap in OnUserCheckBoxBitmap event.
  * New behaviour: You have to create a bitmap yourself and assign it to ABitmap parameter in OnUserCheckBoxBitmap event.
  * reason: this is var parameter and its proper usage has always been this way.
  * remedy: change code to provide the bitmap and assign it to ABitmap parameter in code of OnUserCheckBoxBitmap.



More information in [bugreport 35085](<https://bugs.freepascal.org/view.php?id=35085>). 

## Components incompatibilities

### LazControls: TSpinEditEx no longer inherits from TCustomFloatSpinEditEx

  * Old behavior: TSpinEditEx inherited from TFloatSpinEditEx.
  * New behavior: Both TSpinEditEx and TFloatSpinEditEx now inherit from a common (generic) base class: TSpinEditExBase. As a result testing for "is TCustomFloatSpinEdit" now returns False.
  * Reason: support for Int64 values in TSpinEdit (Double does not have enough precision to cover the Int64 range). Also makes for cleaner code.
  * Remedy: test for the appropriate (base) class instead.



### TAChart: Reticule-related properties deprecated

  * Old behavior: The reticule could be used to display a crosshair cursor in the chart.
  * New behavior: The reticule will be removed because the crosshair cursor is included in the more versatile charttools.
  * Reason: Remove duplicate features.
  * Remedy: Attach a TChartToolset to the chart, add a DatapointCrosshairTool to the toolset and adjust its properties.



### TAChart events On[After|Before]Draw[Background|BackwallWall] deprecated

  * Old behavior: These events could be used for custom-painting of the chart background or the chart data rectangle background.
  * New behavior: These events will be removed.
  * Reason: The events have a parameter Canvas to paint on. TAChart, however, supports additional drawing backends which do not have a canvas.
  * Remedy: Use the new events On[After|Before]CustomDraw[Background|Backwall] which get the currenty used drawer as a parameter. For drawing use the corresponding drawer methods.



### TAChart: The TCubicSplineOption `csoDrawFewPoints` is removed.

  * Old behavior: The CubicSplineSeries required at least 4 data points for drawing. In case of a TCubicSplineSeries with less data points a polygon was drawn as a replacement using the `BadDataPen` if the option `csoDrawFewDataPoints` was active; if this option was not set the series was not drawn at all.
  * New behavior: After fixing the cubic spline interpolation in a local version if NumLib's ipf unit it is possible to draw splines also for less than 4 data points. Old code using the `csoDrawFewDatapoints` option will not compile any more.
  * Reason: The old behavior was a workaround for a NumLib bug.
  * Remedy: Don't use `csoDrawFewPoints` any more.



### TAChart: Other properties removed that had been marked as deprecated for a long time

  * The following table lists the removed properties/procedures/functions along with their replacements:

removed  | use instead   
---|---  
property `TChartTitle.Font` (unit TAChartAxisUtils)  | `TChartTitle.LabelFont`  
property `TCustomChartSeries.ShowInLegend` (unit TACustomSeries)  | `TCustomChartSeries.Legend.Visible`  
method `TFuncSeries.GetFitEquationText` (unit TAFuncSeries)  | `TFuncSeries.EquationText`  
method `TChart.DrawOnCanvas` (unit TAGraph)  | `TChart.PaintOnCanvas`  
property `TChartLegend.Margin` (unit TALegend)  | `TChartLegend.MarginX`  
event `TLineSeries.OnDrawPointer` (unit TASeries)  | `TLineSeries.OnCustomDrawPointer`  
class `TSerie` (unit TASeries)  | `TLineSeries`  
class `TLine` (unit TASeries)  | `TConstantLine`  
property `TZoomDragTool.Proportional` (unit TATools)  | `TZoomDragTool.RatioLimit`, set to `zlrProportional`  
  
## IDE incompatibilities

# Previous release notes

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | 2.0 | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/Lazarus_2.0.0_release_notes](https://web.archive.org/web/20240602095824/https://wiki.freepascal.org/Lazarus_2.0.0_release_notes)_
