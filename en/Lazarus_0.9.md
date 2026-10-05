# Lazarus 0.9.30 fixes branch

This page contains the revisions to be merged from trunk to the [Lazarus 0.9.30 fixes](<http://svn.freepascal.org/svn/lazarus/branches/fixes_0_9_30/>) branch. 

## Contents

  * 1 Fixes for 0.9.30.1
    * 1.1 Submitted by developer / committer
      * 1.1.1 TAChart
  * 2 Fixes for 0.9.30.3
    * 2.1 Merged
      * 2.1.1 TAChart
  * 3 Other branches



## Fixes for 0.9.30.1

(to be released as 0.9.30.2) 

#### Submitted by developer / committer

  * ~~r29551 - Examples, listview: fixed to use TPageControl. While at it, also got rid of .lrs resource.~~
    * Is this example broken in 0.9.29? Running the example seems to work. TNotebook wasn't removed in 0.9.29, but only in 0.9.31. [Vincent](</User:Vincent> "User:Vincent") 21:27, 15 February 2011 (CET) 
      * You are right, it is not broken in 0.9.29, so feel free not to merge it if you decide so. The changes are pretty simple and straightforward, though. [Maxim](</index.php?title=User:Maxim&action=edit&redlink=1> "User:Maxim \(page does not exist\)") 21:44, 15 February 2011 (CET) 
  * ~~r29569 - Qt: TToggleBox fixed messages delivery to LCL. issue #18757. @Vincent - if it's too late just remove this revision (but would be nice to have in 0.9.30 :) )~~
  * r29573 fix <http://bugs.freepascal.org/view.php?id=18759>
  * r29662: IDE: do not auto save layouts of csdesigntime forms 
  * r29735 Qt: Prevent assigning a negative value to a Word, cought by range-check. issue #18876. 
  * r29737,29738 Qt: proper paint of TQtCustomControl viewport, fixed wheel events passing for qt < 4.7. issue #18880. 
  * r29773: IDE, procedure list: added check for the case when we don't have any procedures in current file 
  * r29836 Qt: fixed bug in TQtCalendar, where mouse move,enter and leave weren't propagated to LCL. 
  * r29864: win32: fix a regression which make TStaticText draw only using default text color if Transparent = True 
  * r29877: lcl: set combobox DropDownCount on handle initialization 
  * r29897: win32: fix FindMenuItemAccelerator function - use Visible Index of menu item instead of real index 
  * r29909: IDE: code observer: fixed endless loop 
  * r29917 fix cell clicking on unexpected mouseup after closing opendialog (windows) as requested by Maxim, --[jesusrmx](</User:Jesusrmx> "User:Jesusrmx") 20:49, 19 March 2011 (CET) 
  * r30004: (bug-fix) SynEdit: highlighter fix 
  * r30008, *r30009: (refactor) (both revisions are one change) SynEdit: refactor default beautifier: this may be skipped for fixes branch, this introduces a very minor compatibility change 
  * r30018: gtk2: fix crash in calendar 
  * r30020: (bug-fix) Converter: Disabling a function replacement category was not respected. 
  * r30068,r30072 Qt: fixed wrong positioning of controls inside TScrollBox when scrollbox is scrolled. issue #18993 
  * r30069: IDE: project inspector: fixed crash 
  * r30070: (bug-fix) IDE-Editor-Options: fix disable/enable comboboxes for highlight frame type 
  * r30071: (feature) SynEdit: prevent selection from accidental mouse-jitter on click 
  * r30073: (optimization) SynEdit, Syncro-Edit: skip events, if not active 
  * r30101: (bug-fix) Set Popupmenu.PopupComponent [[|issue 19033](<http://bugs.freepascal.org/view.php?id=19033>)] 
  * r30134: (bug-fix) Clear pixmap contents of empty mask. The contents of a new pixmaps is undefined 
  * r30135: (bug-fix) When a maskhandle of a bitmap is set, update the masked property too 
  * r30096: (bug-fix) SynEdit: fix dangling pchar to de-allocated strings in some Highlighters 
  * r30128: Converter: add the user selected path also to project's include path. 
  * r30138: Qt: fixed bug when control doesn't have csCaptureMouse, but mouse coord is higher than width or height of control. 
  * r30150: (bug-fix_ Debugger: fixed crash, when deleting watches while they got updated 
  * r30152: Gtk2: bugfix where csCaptureMouse cannot be disabled. 
  * r30153,r30172: Qt: fixed tabcontrol behavior to respect AllowChange param from TPageControl.OnChanging event. 
  * r30169: Qt: fixed mouse wheel behaviour in TQtCustomControl for qt < 4.7, now it's same as with qt-4.7 
  * r30074, r30087, r30190, r30191 (optimization/bug-fix) SynEdit: "cache char width info", "fix caret auto adjust with tabs", "assert", "fix 30074, replace assert". Fix caret positioning for syncro edit (or multi win) if tabs are present on caret line 
  * r30185: Gtk2: fixed bug where combobox returned caret position for selection length. 
  * r30192: (bug-fix) SynEdit: fix home/end key if tabs are present at start/end of line 
  * r30198: Qt: fixed strange behaviour of QTreeWidget selection when multiselection is enabled (TListView.Style = vsReport). 
  * r30221: Qt: fixed assertions from Qt when drawing win panels with negative width or height. 
  * r30222: Gtk2: fixed crashes inside TGtk2WidgetSet.ShowWindow() when form is reparented. 
  * r30223: Qt: fixed wrong TQtWidgetSet.SetParent() behaviour. Flags shouldn't be setted up if form is reparented to other control. 
  * r30231: win32: fix drawing of bitbtn under windows vista/7 
  * r30247: (bug-fix) Debugger: Fixed assertion 
  * r30424: (bug-fix) LCL, TButtonPanel: don't change Enabled property when setting up buttons visibility, bug 19192 
  * r30162, 30265: Update for the dbg-testcase, to keep up with changes in ide code 
  * r30164, r30181: (feature): Improved handling of exceptions, and detection of target (32 vs 64 bit) 
  * r30246: Qt: stop signals from qt when deleting an item from list. 
  * r30301: win32: fix win32dialogs compilation with recent fpc 2.5.1 on win64 platform 
  * r30332: ideintf: fix compilation with new tkHelper element 
  * r30348: lcl: fix treeview plus/minus sign drawing with no themes 
  * r30369: LCL: Make GTK2 menu items call OnClick even if they have sub-items. 
  * r30377: win32: don't call LB_GETITEMRECT during ListBox initialization 
  * r30407: Qt: fixed bug where TPanel wasn't painted with defined color in case when BevelInner and BevelOuter are bvNone. 
  * r30423: CodeTools: Remove also { TForm1 } comment together with empty method when needed 
  * r30424 - applied 
  * r30426: LCL: Make menu items call OnClick correctly when moving mouse sideways, on GTK2 and QT. (On Windows works). 
  * r30443,r30448: IDE: find in files: check if click on node, 
  * r30456: IDE: Fix a range error when two ActionListEditors have an action with same name. 
  * r30460: (bugfix) DBG correct type for 64 bit (by Paul) 
  * r30472: LCL: Change button color when enabling/disabling TUpDown. 
  * r30474: LCL: Set Result of TQtTextEdit.viewportEventFilter when local popup is opened. 
  * r30478: LCL: Stop handling click events after popup in gtkMouseBtnPress. 
  * r30479: SourceEditor: Fix Search result, if found-text was already selected 
  * r30480: (bug-fix) SynEdit: word left right on lines with tab. (fixes [[|19232](<http://bugs.freepascal.org/view.php?id=19232>)]) 
  * r30492: (bug-fix) Stop updating gdb for breakpoints, after debug session is stopped 
  * r30493 (changed from r30943 [Juha]): (bug-fix) Support for Win 64 register ABI 



  * r30494: DBG: tests 
  * r30508, r30520: Improved timeout detection in DBG (required for Mac 64 bit) 
  * ~~r30513: Fixed mem leak (in watch eval)~~
  * r30514: Fixed DebulnEnter/Exit (potential invalid memory access) 



  * r30529: Limit search for exception frame 
  * ~~r30539: DBG: fix break condition~~
  * r30540: Qt: fixed crash in TQtWidgetSet.RawImage_FromDevice(), also fixed memleak with created TQtPixmap. 
  * r30543: Qt: use text/plain instead of text/uri-list in SlotDropFiles() as workaround for QUrl bug in qt. 
  * r30537,r30548: LCL, fix grids scrollbars issue 19257 
  * r30560: fix reset of DBG, after error-state 
  * r30555: Qt: TToolBar should have QtTabFocus focuspolicy.Removed WSToolbar (native qt toolbar implementation) define. 
  * r30558: IDE: enclose selection: fixed if line contains only one character, 
  * r30563: LCL: Fix "Read error" when copy-pasting TPairSplitter. 
  * r30564: LCL GTK2: fire OnExit event when leaving Combobox. 
  * r30569: Qt: pass key events for TScrollBar. 
  * r30575: Win32 fix: Closing file with CTRL-F4 on form designer caused access violation. 
  * r30596: Qt: fixed TQtWidgetSet.GetDeviceCaps() to return correct result for hdc. 
  * r30605: Qt: fixed wrong result of TQtWidgetSet.GetClipBox(), fixed TQtRegion translation in TQtWidgetSet.CreateRectRGN() 
  * r30608 
  * r30648: Keymap selection: SchemeRadiogroup.ItemIndex:=...; did not work on Windows if BeginUpdate for items was used. 
  * r30692: Qt: fixed crash in TQtWidgetSet.SendMessage() if TQtWidget Qt handle (QWidgetH) is nil. 
  * r30777: ideintf: patch for fix error on create persistent fields by Alexey Lagunov 
  * r30792: LCL: fix custom draw Combobox. Patch from August Klein 
  * r30897: fixes compilation on win64 with fpc 2.4.4 
  * r31003: LCL, use UTF8 functions on loading and saving grid files 
  * r31018: LCL: Fix PaintTo for GTK2. 
  * r31019: LCL: fix range error when reading an ImageList made with Delphi. Issue #19454 
  * r31038 LCL: Fix the cursor pointer for SpinEdit buttons and ListView column resize. 
  * r31040 LCL: LRSObjectTextToBinary: do not write widestring, it is not needed and makes trouble by the widestringmanager under windows 
  * r31101: Fix issue 19486 Editor focus on auto continue breakpoint 
  * r31177: Pascal-Source lines in disassembler window. 
  * r31159 
  * r31162 
  * r31234: DBG, potential double execution of keys pressed in dbg window Issue 
  * r31277: LCL Win: Call Windows FillRgn and SetStretchBltMode from cross-platform code. 
  * r31348: SynMemo Borderstyle (not saved to lfm) 
  * r31369: fix dbgrid scrollbar jumping issue 
  * r31370: fix position of scrollbar in case user clicks vertical scrollbar button arrows 
  * r31379: fix an error when TActionList has no ImageList assigned. 
  * r31382: LCL: in ShellTreeview fix an eternal loop when expanding a node an Autoexpand is true. 
  * r31384: LCL: fixed Close button glyph transparency (proper transparency mask should be assigned to HBitmap too), bug 19295 
  * r31420: LCL: check grid's topleft range before calc scrollbar position. 
  * r31577 
  * r31578 
  * r31583 
  * r31584 
  * r31797 SynEdit: Highlighter: fix ending var/const fold-sections, if procedure starts with "asm" 
  * r31798 SynEdit: fix mouse-select scrolling (scroll-timer) when past end-of-file. Issue 0019811 
  * r31852 SynEdit: Fixed/Workaround an issue with missing refresh (fold-line-mapping not updated), if no highlighter is assigned 
  * r31876 LazControls.DividerBevel: Fix saving font style 
  * r31777 MAKE DEB: Sync Debian change log file with official Debian repository (request by Mazen) 
  * r32003 SynEdit: highlight "static" 
  * r32039 fix: longest line vs maxleftchar, with eoScrollPastEOL 
  * r32047 DBG: Disassembler: follow font settings of EditorOpts 
  * r32048 DBG: Watches, fix loading watches keep enabled/disabled state 
  * r32049 SynEdit: (Un)indent current line 
  * r32056, r32058 SynEdit: ScreenCaret.ClipRect calculation / sufficient space for caret at line-end. 
  * r32156 SynEdit fix negative ExtraCharSpacing 
  * r32236 IDE, DBG: Obey Options for debug hint eval 
  * r32200 DBG: Fixed a race condition between hitting breakpoint and hint-value-evaluation ( partly merged (only very minor bits missing)) 
  * r32234 LCL, grids, made TPicklistCellEditor OnEditingDone public 
  * r32235 LCL, grids, made TStringCellEditor OnEditingDone public 
  * r32242 LCL, grids, do not AutoSize TStringCellEditor by default 
  * r32452 LCL, grids, make GetDefaultEditor virtual 
  * r32409 SrcEdit: utf8 issue incremental search 
  * r32424 / r32436 SynEdit: fixed crash, with ecDeleteLine on last line. / Stop at end of text 
  * r32453 DBG: Disassembler, when having to guess, use an even address. 



##### TAChart

  * ~~r29618,29619: Reorder components on the "Chart" palette page so that TChart comes first~~
  * r29674: Reset current extent after the change to TChart.Extent property 
  * r29749: Fix division by zero in TZoomClickTool 
  * r29855: Do not draw marks with Visible = false 
  * r29938: Fix drawing with AxisVisible = false 
  * r29962,29963,29964: Do not reserve space for invisible axis marks, minor axis demo fixes 
  * 29989,29990: Fix "unchanged" checks in event assignments: issues #18418, #17373 
  * r30186: Redraw chart after axis list change 
  * r30187: Fix AV while drawing chart with zero axises 
  * r30114,30145: Add workaround for issue #18549 
  * r30379: Always reset the extent after EndUpdate in TUserDefinedChartSource 
  * r30444: Fix List-out-of-bounds when TDbChartSource.FieldY is empty 
  * r30568: Do not draw label links with LinkPen.Visible = false 
  * r31151: Process all fields in TChartAxis.Assign 
  * r31242: Fix freeze for very long line series drawn with wide pen 
  * r31341: Do not draw legend frame with Visible = false 
  * r31431: Do not draw pointers with Visible = false 
  * r31508: Fix AV when deleting toolset which is assigned to a chart 
  * r31853: Fix axis ordering 
  * r31864: Add workaround for issue #19887 
  * r32024: Fix MouseUp event dispatch for tools 
  * r32308: Fix out-of-range array access in TIntervalList.AddRange 
  * r32454: Fix List-out-of-bounds when moving the series up in the editor 
  * r32796: Reset zooming after changing axis transformations to avoid SIGFPE in some cases 



## Fixes for 0.9.30.3

#### Merged

  * 32892 SynEdit: fix trim spaces, edit insert text into trailing spaces, could loose some spaces 
  * 33158 SynEdit: Syncro-Edit, fixed interference with trim space 
  * 33221 DBG: Fix mem leak in expression-eval (hint/inspect), except the assertions 
  * 33512 SynEdit: Fix MultySyn Highlighter (uninitialized field) 
  * 33589 Codetemplate fix for tabs. 
  * 33641,33642 SynEdit HTML highlighter: added 



##### TAChart

  * 32933: Silence "unused parameter" hint 
  * 33023: Correctly invalidate extent in TListChartSource.Delete 
  * 33132: Fix typo in the method name: CopyForm -> CopyFrom. Update demos 
  * 34169: Fix range check error while drawing the stepped line which frequently goes out of the extent, issue #20887 
  * 34981: Refresh chart after Series.Clear call 
  * 35829: Fix auto-scaling transformation with empty or infinite data 



## Other branches

Lazarus - Release Notes and Subversion Branch with Release Fixes

Release notes for Version:

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [1.10](<Lazarus_1.10.md> "Lazarus 1.10.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

**0.9** | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0](<FPC_New_Features_3.md> "FPC New Features 3.0") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/Lazarus_0.9.30_fixes_branch](https://web.archive.org/web/20171010035246/https://wiki.freepascal.org/Lazarus_0.9.30_fixes_branch)_
