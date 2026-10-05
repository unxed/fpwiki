# Lazarus 1.6.0 release notes

│ **English (en)** │ 

Lazarus 1.6 was released at February 18, 2016. 

Statistics: 

  1. commits: 2683
  2. log: svn log -r 47337:50707



## Contents

  * 1 LCL Interfaces Changes
  * 2 LCL Changes
  * 3 IDE Interfaces Changes
  * 4 IDE Changes
  * 5 Components
    * 5.1 LazControls
    * 5.2 Codetools
    * 5.3 Project Groups
    * 5.4 Docked Form Editor
    * 5.5 Package Tabs
    * 5.6 Favorites
    * 5.7 Debugger / win 32
  * 6 Changes affecting compatibility
    * 6.1 LazUtils
      * 6.1.1 SysToUTF8 does not convert
      * 6.1.2 UTF8ToSys does not convert
      * 6.1.3 CopyFile behaviour changed
      * 6.1.4 Wrappers for string functions are deprecated
    * 6.2 LCL incompatibilities
      * 6.2.1 QuestionDlg modal result on close by ESC key and system close button changed
    * 6.3 Other Packages and Components
      * 6.3.1 AnchorDocking improved
      * 6.3.2 SynMemo is now Deprecated
      * 6.3.3 TSynEditExporterHtml behaviour has changed
      * 6.3.4 TSynCustomHighlighter.GetIdentChars implementation has changed
      * 6.3.5 EasyDocking isn't supported in the IDE anymore
    * 6.4 IDE incompatibilities
  * 7 Previous release notes



# LCL Interfaces Changes

# LCL Changes

  * Added TFlowPanel in ExtCtrls.pp
  * The TreeNodes of TShellTreeView are now of type TShellTreeNode. In order to access the nodes as such, you need to explicitely cast the node to TShellTreeNode. The TShellTreeNode class provides the following functions and properties: 
    * ShortFilename: returns the filename without path.
    * FullFilename: returns the fully qualified filename.
    * IsDirectory: returns True if the node represents a directory.
    * BasePath: returns the basepath of the node.
  * Added 'iscancel' parameter to QuestionDlg. If no cancel button is explicitely set, mrCancel is returned in case the dialog is closed with ESC or system close button.
  * Added support for TCustomEdit.TextHint.
  * Added TTimeEdit in editbtn.pas.



# IDE Interfaces Changes

  * [Quick fix](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools") for fpc message _Local variable "i" does not seem to be initialized_ inserts an assignment.
  * Local variable completion (e.g. Complete Code on "i:=3;") now groups variables of same type. You can get the old behaviour in _Tools / Options / Codetools / Code Creation / Group automatically defined local variables_.



# IDE Changes

  * Editor 
    * Basic multi-caret mode
    * Option for multi-line notebook-tabs
    * line-number part of gutter now allows to select the current line, or several lines.


  * Project 
    * Additions and Overrides are now added as last. Formerly they were added after the inherited and before the custom compiler options.


  * Messages Window 
    * The IDE now warns when there are duplicate units or include files in packages.
    * Success line. New option to show compiler message "n lines compiled. m sec". _Tools / Options / Messages Window / Show FPC message "lines compiled"_


  * Desktops 
    * See [Desktops](<IDE_Window__Desktops.md> "IDE Window: Desktops")
    * Multi-monitor screen layouts are supported by the IDE.


  * IDE CoolBar and Editor Toolbar 
    * A new configurable IDE CoolBar replaces the static Toolbar. It shares settings GUI with EditorToolbar which was moved to IDE's codebase. The command set that can be selected was refined. Earlier only IDEMenuCommands could be selected, now a limited selection of all IDECommands.
    * EditorToolbar package is empty and deprecated. It's functionality is now part of the IDE. Please uninstall it from the IDE because it will be removed from the 1.8 release.


  * Object Inspector 
    * The Boolean property editor now uses a CheckBox by default. A ComboBox with "False", "True" can still be selected in options.


  * Package editor 
    * Two separate buttons for adding files were replaced by one button with a dropdown menu. The "Add" and "Add more" buttons can be considered a failed experiment.
    * Many usability issues were fixed. The whole tree is not updated any more after each operation and focus stays in right place after them.


  * Environment 
    * new Environment setting "Multiple Lazarus instances" to set up what happens if Lazarus is started with a file in command parameter.


  * [lazbuild](<lazbuild.md> "lazbuild")
    * new option --verbose-pkgsearch to show what packages are searched and found.


  * lazarus 
    * new option --force-new-instance to force opening a new instance and ignoring the "Multiple Lazarus instances" option.


  * Code navigation and completion for class helpers, record helpers and type helpers.



# Components

## LazControls

  * Added [TListViewFilterEdit](<TListViewFilterEdit.md> "TListViewFilterEdit"), having the same idea as other FilterEdits but working on [TListView](<TListView.md> "TListView"). This is still very basic and will be improved later.
  * All FilterEdits now use TextHint property for the grey '(Filter)' text which is shown when not active.
  * Added [TCheckBoxThemed](</index.php?title=TCheckBoxThemed&action=edit&redlink=1> "TCheckBoxThemed \(page does not exist\)"), a custom drawn CheckBox. It is in development for use as OI Boolean property editor.



## Codetools

  * Support for type, record and class helpers
  * Support for operator type evaluation "I := A + B;"
  * Improved support for ObjCCategory
  * Class completion of properties can use prefixed variable name and const parameter. See Options / Codetools / Class Completion.
  * New option Options / Codetools / Code Creation / Insert new event methods to section
  * New option Options / Codetools / Identifier Completion / Miscellaneous / Jump to error
  * It now finds sources of units installed via fppkg.



## Project Groups

  * New IDE package to manage projects, packages and Pascal files, see [here](<Project_Groups.md> "Project Groups").



## Docked Form Editor

The package implementing a docked form editor can be found in components/sparta/dockedformeditor/sparta_dockedformeditor.lpk. It makes the most sense with [Anchor Docking](<Anchor_Docking.md> "Anchor Docking") but can be used also without it. 

IMPORTANT: the docked form editor is included in 1.6 but is not stable there and has known bugs that affect its usage. Please do not report issues based on your 1.6 experience but always check against the current trunk where the designer has been improved further. Docked editor bug fixes won't be merged into 1.6 since a lot of them were OS/WS dependent and the LCL code had to be changed as well. 

If you want to use the docked form editor, you have to use complete Lazarus trunk. Copying the sparta packages from trunk into your 1.6 fixes won't work. 

## Package Tabs

  * New IDE package to replace source notebook tabs/pages with buttons sorted by package and name, see [Package Tabs](<Package_Tabs.md> "Package Tabs").



## Favorites

  * New IDE package to add projects to favorites and keep them up in the "Open" button drop-down menu, see [Favorites](<Favorites.md> "Favorites").



## Debugger / win 32

The version of gdb shipped with the win32 installer was updated to 7.7.1 

PRO: 

  * solves some gdb crash issues for all current window versions (7 and up)
  * solves a bug, that environment changes were not passed on



REGRESSIONS 

  * <https://sourceware.org/bugzilla/show_bug.cgi?id=16016> this manifests for example:



    

  * records may be displayed as class and vice versa
  * pointer to record may be dereferenced without warning (hiding the fact that a value is a pointer)
  * shortstrings may not display correct (they are internally encoded as record)



# Changes affecting compatibility

## LazUtils

  * Unit LazUTF8 now sets UTF8 as default codepage when compiled with FPC 2.7+ making programming with Unicode easier, but there are some incompatibilities. Read [more](<Unicode_Support_in_Lazarus.md> "Unicode Support in Lazarus").
  * There are wrapper functions around LCL, CodeTools and LazUtils for some string functions defined in LazUtils. The wrappers are now deprecated. They can be compeletely disabled by defining "DisableWrapperFunctions". They will be removed in the next release (after 1.6).



### SysToUTF8 does not convert

Similar: AnsiToUTF8 

  * Old behaviour: SysToUTF8 converted on Windows from system codepage to UTF-8.
  * New behaviour: Does not change the string (unless you disable the new UTF-8 mode).
  * Reason: function was mainly used for converting strings from the RTL, which is now talking UTF-8.
  * Remedy: use function WinCPToUTF8 from unit LazUTF8 or CP1251ToUTF8 from unit LConvEncoding.



### UTF8ToSys does not convert

Similar: UTF8ToAnsi 

  * Old behaviour: UTF8ToSys converted on Windows UTF-8 to system codepage.
  * New behaviour: Does not change the string (unless you disable the new UTF-8 mode).
  * Reason: function was mainly used for converting strings for the RTL, which is now talking UTF-8.
  * Remedy: use function UTF8ToWinCP from unit LazUTF8 or UTF8ToCP1251 from unit LConvEncoding.



### CopyFile behaviour changed

  * Old behaviour: CopyFile raised exceptions by default when copying failed under arbitrary circumstances.
  * New behaviour: CopyFile returns False upon all failures, regardless of the reason.
  * Reason: a function that returns False upon failure should not raise an exception in such cases.
  * Remedy: an optional parameter ExceptionOnError has been added. Set it to True to get the old behaviour.



### Wrappers for string functions are deprecated

Various units in LCL, LazUtils and Codetools have wrappers for string functions in LazUTF8 and LazFileUtils. They are now deprecated and the hint after "deprecated" tells where the actual function can be found. The wrappers can be completely disabled by defining "DisableWrapperFunctions". 

## LCL incompatibilities

### QuestionDlg modal result on close by ESC key and system close button changed

  * Old behavior: the cancel modal result was picked up from the actual button list.
  * New behavior: the cancel modal result is always mrCancel.
  * Reason: the ESC or system close button are in a way a third "virtual button" that is always there. If your dialog shows only buttons "Yes" and "No", closing by ESC doesn't really mean any of them.
  * Remedy: use the new 'IsCancel' parameter to define cancel modal result and get the old behavior.
  * More information: [Dialog_Examples#QuestionDlg](<Dialog_Examples.md> "Dialog Examples")



## Other Packages and Components

### AnchorDocking improved

  * Due to recent bug fixes in AnchorDocking, your application can be started with slightly changed docked layout (docked window size growing/shrinking).
  * Remedy: Call TAnchorDockMaster.ResetSplitters after your layout and form have been initialized (the docking site has the correct height at last).
  * More information: [Issue #29475](<https://bugs.freepascal.org/view.php?id=29475>)



### SynMemo is now Deprecated

  * SynMemo was deprecated.



    Any code referring to SynMemo will now give a warning about this.

  * SynMemo was also removed from the component palette.



    However SynMemo is still a registered component, and if you open any project already containing a SynMemo it will load correctly.
    If you must have an IDE with SynMemo on the component palette, you can compile the IDE with WithSynMemo defined. This is temporary, and will be removed at some point after the release.

SynMemo was nearly identical to SynEdit (except it did publish less properties). 

It only featured 2 extra method: 
    
    
       function CharIndexToRowCol(Index: integer): TPoint;   
       function RowColToCharIndex(RowCol: TPoint): integer;  
    

Those are now available in SynEdit. However with a strong warning 
    
    
       experimental; deprecated 'SynMemo compatibility - very slow / SynEdit operates on x/y';
    

It is highly recommended not to use them. The calculation in those methods was always done without taking in account trailing spaces (if trim trailing spaces was active). This may have lead to wrong results. 

A common use case for those methods was apparently: 

    \- Get the caret position as CharIndex.
    \- Insert some text
    \- Add the text length to the CharIndex
    \- Set the caret position from the CharIndex

If setting the text changed trailing spaces (e.g. text was appended at the end of line, behind some trailing spaces, which then would not be trailing any more) then the calculation would not be correct. 

Yet if the 2 methods would include trailing spaces, it could equally go wrong. If between getting the caret position, and restoring it any trailing spaces where trimmed, then the restore is wrong. 

For this reason, and for the fact that they are extremely inefficient (slow) their use is not recommended. 

  
To archive caret adjustment in SynEdit, use InsertTextAtCaret, TextBetweenPointsEx or SetTextBetweenPoints, which all have the option of adjusting the caret. 

### TSynEditExporterHtml behaviour has changed

  * Old behaviour: both ExportAsText and CreateHTMLFragment controlled the output.
  * New behaviour:



    Output is now controlled by the property Options
    The property CreateHTMLFragment is deprecated and will be removed in a future release
    ExportAsText now only affects how the exportbuffer is put on clipboard

  * Reason:



    More options than CreateHTMLFragment were needed
    The comments in the parentclass (TSynEditExport) state that this property controls the way the export buffer is put onto the clipboard

  * Remedy:



    Use Options property to control the output.
    Remove references to CreateHTMLFragment

### TSynCustomHighlighter.GetIdentChars implementation has changed

  * Old implementation: [#33..#255]
  * New implementation: ['_', 'A'..'Z', 'a'..'z', '0'..'9']
  * Reason:



    The new character array better suits most uses.

### EasyDocking isn't supported in the IDE anymore

You cannot install easydockmgr and easydockmgrdsgn in the IDE anymore. Use AnchorDocking instead. easydockmgrdsgn package was removed from 1.7. 

## IDE incompatibilities

# Previous release notes

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | 1.6 | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/Lazarus_1.6.0_release_notes](https://web.archive.org/web/20250219023618/https://wiki.freepascal.org/Lazarus_1.6.0_release_notes)_
