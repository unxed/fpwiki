# Lazarus 1.4.0 release notes

Lazarus 1.4 was released at April 22, 2015. 

Statistics: 

  1. commits: 3245
  2. log: svn log -r 44302:48774



## Contents

  * 1 LCL Interfaces Changes
  * 2 LCL Changes
  * 3 IDE Interfaces Changes
  * 4 IDE Changes
  * 5 Components
  * 6 Changes affecting compatibility
    * 6.1 LCL incompatibilities
      * 6.1.1 TEditButton was rewritten
      * 6.1.2 TControl.GetChildsRect renamed to TControl.GetChildrenRect
      * 6.1.3 TControlScrollBar.AutoCalcRange was removed
      * 6.1.4 TDateEdit.DialogTitle property was removed
      * 6.1.5 TMemo, TTextStrings and TCustomStringGrid: changed behaviour of LoadFromFile/SaveToFile
    * 6.2 IDE incompatibilities
      * 6.2.1 Changed parameters
      * 6.2.2 IDE Macro CompPath
      * 6.2.3 Old IDE does not reopen first file when opening a project
      * 6.2.4 IDE does not show Compile Dialog
      * 6.2.5 IDE/lazbuild cross compiles for different target OS/CPU
  * 7 Previous release notes



# LCL Interfaces Changes

# LCL Changes

  * Added methods and utilities to load objects from FPC resources: TGraphic.LoadFromResourceName(), TPicture.LoadFromResourceName(), TCustomImageList.AddResourceName(), LoadGlyphFromResourceName().
  * Changed all LCL resources from LRS to RES. As a result they can be edited in executables using resource editors on Windows platform.
  * DBImage changes versus 1.2: DbImage implements loading stream directly if it doesn't have a known header, WriteHeader property. This makes writing image header optional improving compatibility with Delphi controls - see revision 43779.
  * Translations unit: SetFuzzy (boolean with default value false) parameter was added to TPOFile.Add method. It allows to mark PO entry as fuzzy (when true). As this parameter has default value (false), the current code which uses this method is not affected.
  * TDateTimePicker and TDBDateTimePicker components were added. They are Delphi compatible and are installed by default, but have their own package instead of being part of LCL.
  * TComboBoxEx and TCheckComboBox components were added. They are Delphi compatible and are part of LCL.



# IDE Interfaces Changes

  * Easier hint windows in IdeIntf 
    * Unit IdeHelpIntf has a new THintWindowManager class. It was collected from similar code from many places and refactored into a class. It is also used by Object Inspector which already is part of IdeIntf. The class uses THintWindow (Forms unit) internally and support the traditional text hints and also external hint renderers.
    * In Lazarus IDE, the external renderer now is the TurboPowerIProDsgn package. This refactoring was related to fixing a long time nasty crash bug which prevented including TurboPowerIProDsgn by default in BigIde. Now it is included.



# IDE Changes

  * **Resources**
    * All IDE resources are stored in RES files now.
    * Component images can be loaded from resources stored in RES files now.
    * LazRes and LrsToLfm have been improved to assist in resource migration process.
    * Project options has a 'Resource' section which allows to add any user resources which are stored in the project .RES file together with project icon, manifest and version information.
    * Project version information was extended to allow saving build-related attribute information.


  * **Editor**
    * Auto-Indent now supports "tab only". If a new line is started, the indent of the previous line is recreated using tabs. And then either spaces or cut off.
    * Keyword-pair/triplet highlight (matching begin,end) can be turned on/off for each pair/triplet
    * Keyword-pair/triplet supports if/then/else
    * Compiler message marks. Each compiler message shows an icon in the left gutter, a wavy underline in the text and a mark on the right gutter. You can right click on the left icon to get context actions, e.g. Quick Fixes.
    * Refactoring tool _Show Abstract Methods_ now supports class **interfaces**.
    * Identifier completion box (Ctrl+Space) has now options to disable sorting for scope and to disable putting the recently used identifiers top. If you disable both the list is sorted alphabetically.


  * **Debugger**
    * Attach: List available processes (Windows/Linux/OSX only)
    * Debug-Inspector: Select member values (class/record/(dyn)array/pointer-deref) by double click on row
    * Allow single step from exception to except/finally block (includes stepping through implicit except blocks, at then end of some methods). 
      * Also fix single step now works, if it steps over an ignored exception.
    * BETA: Alternative debugger fpdebug [[blog](<http://lazarus-dev.blogspot.co.uk/2014/05/de-bug-wars-new-hope.html>)]


  * **Designer**
    * New Undo feature supports component moving, resizing and deleting. Also property changed by Object Inspector can be undone.


  * **Component palette**
    * Palette is fully configurable. Pages can be added, pages and components can be reordered and components can be moved between pages freely. The difference between default layout and user layout is stored in environment options. Layouts can be exported/imported, too.


  * **Help**
    * The Help/Help menu starts the lhelp help viewer and shows all CHM files (if present and configured, as done by e.g. the Windows installer)
    * lhelp supports diagnostic output to a log file (using e.g. --debug-log=)


  * **Project**
    * The target processor option (don't confuse this with the target CPU) now uses the -Cp option instead of the -Op option. This is required by ARM processors.
    * The compiler in the project's compiler options is used to setup codetools. Formerly the IDE asked the default compiler from the Tools / Options using the project's target OS and CPU. That means you can now use build modes to switch between different compiler versions and code navigation will use the right settings.
    * When target OS/CPU are not given (aka empty or default) the IDE/lazbuild now queries the compiler for its default target platform. In other words: For cross compiling it is now enough to select the compiler. Formerly the defaults were taken from the IDE's OS/CPU.
    * The IDE now parses the -Xp and -V options in the custom compiler options to find the right compiler.
    * Project Option "Main unit has uses section containing all units of project": Formerly the main source was only updated when it was enabled. Now: Only additions are affected by this option. Renames and removes always updates the main source if it is Pascal.
    * Under Unix/Linux the IDE now opens always a project (lpi) in its physical path (i.e. all symlinks are resolved). This is compatible with FPC, which always uses the physical directory as working directory.
    * Project Inspector: 
      * The Inspector now supports multi selection. For example delete multiple files or set properties.
      * You can drag files from other applications and drop them on Inspector to add files to the project.
      * You can drag files to other directories (drag the files onto a file or directory in the package editor).
      * New menu item: Move/copy selected files to a directory. This makes it easy to split a big package into several sub directories.


  * **Packages**
    * Packages now use by default the project compiler. This comes from a change to the macro [$(CompPath)](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames"), which now resolves to the project compiler.
    * The target processor option now uses the -Cp option instead of the -Op option. This is required by ARM processors.
    * Under Unix the IDE now opens always a package (lpk) in its physical path (i.e. all symlinks resolved). This is compatible with fpc, which always uses the physical directory as working directory.
    * Package Editor: 
      * The package editor now supports multi selection. For example delete multiple files or set properties.
      * You can now copy or move files via drag and drop between package editors and to/from project inspector.
      * You can drag files from other applications and drop them on package editors to add files to the package.
      * You can drag files to other directories (drag the files onto a file or directory in the package editor).
      * New menu item: Move/copy selected files to a directory. This makes it easy to split a big package into several sub directories.
    * Packages are now compiled in [parallel](<Lazarus_Packages.md> "Lazarus Packages").


  * **Messages**
    * This window was completely rewritten. Every tool output is now clearly separated by a header line.
    * Each external tool (e.g. the compiler) now runs in a thread. The same for the biggest part of the parser.
    * The IDE now passes -vbq (message ids and full file names) to the compiler.
    * The window can handle much bigger outputs, so you can compile with more verbosity
    * The IDE now knows where each message came from and what tool has created them. For example you can see the reason why a package was compiled and the parameters via the "About" menu item.
    * Switch between English and translation at any time
    * The fpc message parser uses the message ids, the errore.msg file and the translated error*.msg file. The -Fr compiler option (pass custom fpc message file) on Project/Package Options / Compiler Options / Messages was removed.
    * Search in _Messages_ window
    * Filter for message types and urgency (hint, note, warn ...)
    * Choose file names styles (short, relative, full)
    * Change the colors in Tools / Options / Messages
    * when you insert/delete text in the source editor the messages are now updated in line *and* column number.
    * The _Compile Info_ window was removed because it had no maintainer. This includes the options "Show compile dialog" and "Autoclose compile dialog".
    * [Quick fix](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools") for fpc message "No implementation for interface method found", shows abstract methods dialog


  * **Miscellaneous**
    * new IDE macros $(LazVer) and $(FPC_FULLVERSION), see [here](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames")
    * Compiling for x86-64 Linux, *BSD and Solaris now passes -Cg (Generate PIC) code to the compiler. Use -Cg- if you don't want that.


  * **Plugins**
    * _Leakview_ now supports GDB stack traces without source file names and only mangled identifiers.
    * _TurboPowerIProDsgn_ is installed by default (is part of BigIde). It means all hints and info panels have a nice HTML formatting with colors and links for sources. The package could be installed also earlier for the same effect but there was a crash bug which prevented it from being included by default.
    * _EditorToolbar_ is installed by default (is part of BigIde). It can be hidden from View menu, thus uninstall is not necessarily needed for anyone. Lots of bugs in EditorToolbar were fixed.



# Components

  * TOpenGLControl: works now under Linux/QT/X



# Changes affecting compatibility

## LCL incompatibilities

### TEditButton was rewritten

TEditButton has been redesigned as a "grouped control". It now inherits from a different ancestor (it is no longer derived from TCustomMaskEdit). The new design now properly aligns and anchors.  
Effects: 

  * Derived components can no longer override (or use) all of the (protected) methods of TCustomMaskEdit. Most of the commonly used methods have been propagated with new names reflecting the new design e.g.: 
    * Change -> EditChange
    * DblClick-> EditDblClick
  * When the component has focus, then checking if (ActiveControl is TButtonEdit) will be False, since ActiveControl in this case is the internal edit control (of type TEbEdit). (Code that checks for (ActiveControl is TCustom(Mask)Edit) will still evaluate to True.)



Remedy: 

  * Use EditChange, EditDblClick etc. in derived controls.
  * Use (ActiveControl.Parent is TEditButton), or alternatively (ActiveControl is TEbEdit)



### TControl.GetChildsRect renamed to TControl.GetChildrenRect

  * Effect: compile error: There is no method in an ancestor class to be overridden: "TYourControl.GetChildsRect(Boolean):<record type>;"
  * Reason: Incorrect English
  * Remedy: use GetChildrenRect instead.



### TControlScrollBar.AutoCalcRange was removed

  * Effect: compiler error: There is no method in an ancestor class to be overridden: "TYourControlScrollBar.AutoCalcRange;"
  * Reason: Code for calculating the _AutoScroll_ ranges was moved from the two scrollbars to the new proteced method TScrollingWinControl.CalculateAutoRanges. This prevents an endless loop when the two scrollbars depend on each other, it simplifies the code and reduces some overhead.
  * Remedy: Override TYourControl.CalculateAutoRanges instead.



### TDateEdit.DialogTitle property was removed

  * Effect: Compile error if the property was set in code. LFM loader removes it from form files.
  * Reason: Unused and thus misleading and confusing.
  * Remedy: Remove it from your code.



### TMemo, TTextStrings and TCustomStringGrid: changed behaviour of LoadFromFile/SaveToFile

  * Effect: LoadFromFile/SaveToFile respectively LoadFromCSVFile/SaveToCSVFile now take strings in UTF8-encoding (was: system encoding) as their parameter.
  * Reason: LCL uses UTF8 internally; consistency with e.g. TSynEdit.
  * Remedy: do not use Utf8ToSys() anymore in the calls to these procedures.



## IDE incompatibilities

### Changed parameters

  * LazarusIDE.DoJumpToCompilerMessage: changed Line _integer_ to _TMessageLine_
  * CompilerOptions.ShowAllProcsOnError: was removed, option -vb is now always passed
  * CompilerOptions.ShowNothing: was removed, not needed
  * LazarusHelp.ShowHelpForMessage: removed parameter Line. The IDE now always shows help for the currently selected line.



### IDE Macro CompPath

  * Effect: IDE uses the project compiler instead of the default compiler set in the IDE options.
  * Reason: The macro **$(CompPath)** now resolves to the project compiler. Packages are now compiled with the project compiler.
  * Remedy: Use **$CompPath(IDE)**



### Old IDE does not reopen first file when opening a project

  * Effect: When a project saved with a new IDE (1.3+) is opened with an old IDE (e.g. 1.2), the first file in the source editor is not reopened automatically.
  * Reason: The default value for Editor position is now "0", which is not stored in the lpi, creating smaller lpi files.
  * Remedy: Open the file manually.



### IDE does not show Compile Dialog

  * Effect: **Compile Info** window does not appear when compiling. Options **Show compile dialog** and **Autoclose compile dialog** are missing.
  * Reason: The code that runs the compiler was completely rewritten. The dialog needed a big rewrite too, but it had no maintainer.
  * Remedy: You can abort a compile via Run / Abort Build or its shortcut Ctrl+Alt+Shift+G. You can change the shortcut in Tools / Options / Editor / Key Mappings. The number of errors, warnings and hints are shown in the Messages window.



### IDE/lazbuild cross compiles for different target OS/CPU

  * Effect: When the target OS/CPU are not set (i.e. empty or default) the 1.4 IDE/lazbuild compile for a different target than Lazarus 1.2.
  * Reason: Formerly the IDE took as default its own OS/CPU. Now it queries the project's compiler and uses its default target OS/CPU.
  * Remedy: Specify target OS/CPU in _Project options / Compiler options_ or for compiling the IDE set _Tools / Configure build Lazarus_.



# Previous release notes

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | 1.4 | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch") | [4.0](<Lazarus_4.md> "Lazarus 4.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/Lazarus_1.4.0_release_notes](https://web.archive.org/web/20250519145507/https://wiki.freepascal.org/Lazarus_1.4.0_release_notes)_
