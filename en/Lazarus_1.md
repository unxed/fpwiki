# Lazarus 1.8 fixes branch

This page contains the revisions to be merged from trunk to the [Lazarus 1.8 fixes](<http://svn.freepascal.org/svn/lazarus/branches/fixes_1_8/>) branch. 

These are only fixes made after the branch was created. For other fixes made since the previous release (1.6) see SVN and [Lazarus 1.6 fixes branch](<Lazarus_1.md> "Lazarus 1.6 fixes branch")

Release notes can be found [here](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes")

## Contents

  * 1 Fixes for 1.8.6
    * 1.1 Merge requests
      * 1.1.1 Submitted by developer / committer and waiting for testing (do not commit yet)
      * 1.1.2 Merge conflicts
      * 1.1.3 Submitted by developer / committer, tested, waiting to be merged
    * 1.2 Merged revisions for 1.8.6
  * 2 Fixes for 1.8.4 (merged)
  * 3 Fixes for 1.8.2 (merged)
  * 4 Fixes for 1.8.0 (merged)
  * 5 Fixes for 1.8 RC5 (merged)
  * 6 Fixes for 1.8 RC4 (merged)
  * 7 Fixes for 1.8 RC3 (merged)
  * 8 Fixes for 1.8 RC2 (merged)
  * 9 Fixes for 1.8 RC1 (merged)
  * 10 Roadmap to 1.8.0
    * 10.1 Tests
    * 10.2 Tagging release
  * 11 How to merge
  * 12 Other branches



## Fixes for 1.8.6

### Merge requests

#### Submitted by developer / committer and waiting for testing (do not commit yet)

  * r58312 LCL: TSpeedButton does not respond to pressing, after closing the modal form with a double click. [Issue #33874](<https://bugs.freepascal.org/view.php?id=33874>)
  * r58314 LCL: Remove CaptureControl when its handle is destroyed. [Issue #33874](<https://bugs.freepascal.org/view.php?id=33874>).
  * r58325 win32: listview: fix TWin32WSCustomListView.LVItemAssign if the item was not empty. [Issue #33870](<https://bugs.freepascal.org/view.php?id=33870>)
  * r58510 LCL: TTreeView: Make TOldTreeNodeInfo.Data 32/64Bit aware to prevent crash of TreeView.Items.Assign on 64bit.
  * r58797 lcl: workaround for fpc [Issue #0034185](<https://bugs.freepascal.org/view.php?id=0034185>) (dangling FPalette pointer in TFPReaderPNG.InternalRead causes AV)
  * r58815 IDE: Allow to replace a path in PathEditor if a new path differs only by character case. [Issue #33902](<https://bugs.freepascal.org/view.php?id=33902>)
  * r58838 CustomForm: Register new properties also for designer so they remain in a form. [Issue #30241](<https://bugs.freepascal.org/view.php?id=30241>)
  * r59297 SynEdit: Pas HL, fix folding/markup for code containing "case", if "case" is not enabled for fold/markup. Part 1 of [Issue #0034410](<https://bugs.freepascal.org/view.php?id=0034410>)
  * r59298 SynEdit: Pas HL, fix markup if "procedure" is outlined, but top level "begin" is not. Part 2 of [Issue #0034410](<https://bugs.freepascal.org/view.php?id=0034410>)
  * r59299 SynEdit: Pas HL, fixes for outline, if only some elements are outlined
  * r59300 SynEdit: Pas HL, fixes for fold, detecting "else" in "case", when previous if is nested in other commands



#### Merge conflicts

  * r55865 IDE: set ToolStatus to itBuilder before compiling packages, [Issue #32421](<https://bugs.freepascal.org/view.php?id=32421>)



#### Submitted by developer / committer, tested, waiting to be merged

### Merged revisions for 1.8.6

  * r56872 LCL: Supporting TCanvas.Draw a TBitmap in pf8bit TBitmap.PixelFormat. [Issue #32878](<https://bugs.freepascal.org/view.php?id=32878>). Patch by fantangshan
  * r57983 IDEIntf: fixed registering option frames with same default index
  * r58007 Translations: fixed typo in Brazilian Portuguese translation (atuak -> atual). Hint by Jean Patrick, bug 33773.
  * r58022 SynEdit: Fix scrollbars, if new handle is created (SynEdit shown a 2nd time). Issue 33784
  * r58149 LCL: Fix compilation of TControlBar.OnBandPaint event handler in objfpc mode. (<https://forum.lazarus.freepascal.org/index.php/topic,41504.msg288037>)
  * r58181 SynEdit: fix position of multi-caret (after EOL), when column selection is deleted. [Issue #0032161](<https://bugs.freepascal.org/view.php?id=0032161>)
  * r58189 SynEdit: Fix toggle multi caret, patch by Pascal, [Issue #0029456](<https://bugs.freepascal.org/view.php?id=0029456>)
  * r58209 SynEdit, AutoComplete: prevent timer from accidental trigger. part of [Issue #0033665](<https://bugs.freepascal.org/view.php?id=0033665>)
  * r58200 LazDebuggerFP: fix (stackframe) register for 64bit app / enable watches,locals on selected stackframe
  * r58221 IDE, options: Improve/Correct sizing of color palette (e.g. in used defined markup). [Issue #0033435](<https://bugs.freepascal.org/view.php?id=0033435>)
  * r58521 IDE: Update component tree(OI) when changing the Z-order of a control. Issue #33954.
  * r58428 Examples: Fix uninitialized variable issue in gridexamples/merged_cells (<https://forum.lazarus.freepascal.org/index.php/topic,41757.msg290284>)
  * r58432 LCL/WinCE: Fix for loop in TForm.Show [Issue #0032366](<https://bugs.freepascal.org/view.php?id=0032366>)
  * r58492 LCL: Win32: Groupbox disabled painted when parent is disabled.
  * r58638 Debugger: Fix attach dialog. [Issue #0034028](<https://bugs.freepascal.org/view.php?id=0034028>) Patch by Cyrax
  * r58710 PoChecker: fixed off-by-one errors when showing updating/drawing progress in Graphical Statistics window
  * r58639 Win32 and CustomDrawn: fix compilation with fpc trunk after r39480. [Issue #0034022](<https://bugs.freepascal.org/view.php?id=0034022>).
  * r58773 TValueListEditor: implement sort. Based on a patch by Jesus Reyes. [Issue #0034141](<https://bugs.freepascal.org/view.php?id=0034141>).
  * r59030 TValueListEditor: fix DeleteRow [Issue #0034208](<https://bugs.freepascal.org/view.php?id=0034208>).
  * r59031 TFindDialog: fix setting Top/Left. [Issue #0033528](<https://bugs.freepascal.org/view.php?id=0033528>).
  * r59048 TFindDialog: slight improvement on r59031.
  * r59049 TFindDialog: use Position := poMainFormCenter in CalcPosition instead of trying to calculate that by ourselves. Patch by Serge Anvarov.
  * r59050 TFindDialog: save FFindForm position upon close.
  * r59083 TFindDialog/TReplaceDialog: show dialog poScreenCenter in IDE; fix erratic behaviour of TReplaceDialog in IDE (wrong parent). [Issue #0034297](<https://bugs.freepascal.org/view.php?id=0034297>).



## Fixes for 1.8.4 (merged)

  * r56048 LCL: GTK2: Don't grab focus for TPanel on mouse messages. [Issue #32515](<https://bugs.freepascal.org/view.php?id=32515>)
  * r56709 Designer: Prevent an AV after deleting a control with csDesignInteractive in ControlStyle. [Issue #32813](<https://bugs.freepascal.org/view.php?id=32813>).
  * r56716 LCL: Remove csDesignInteractive from ControlStyle of TControlBar. [Issue #32813](<https://bugs.freepascal.org/view.php?id=32813>).
  * r57405 LCL: Make popupnotifier dpi-aware.
  * r57429 lcl: fixed UCS2LEToUTF8 for 1byte niput, issue 33296
  * r57475 IDE, EditorMacro: fix parsing of arguments. Fix numbers and #123
  * r57483 LCL: Fix keyboard navigation in RadioGroup with disabled items ([Issue #33386](<https://bugs.freepascal.org/view.php?id=33386>) patch by Serge Anvarov).
  * r57497 IDE: ObjectInspector: Fixed SHIFT click to select a range doesn't work. [Issue #33383](<https://bugs.freepascal.org/view.php?id=33383>). Patch by Rik van Kekem
  * r57511 SynEdit: TLazSynEditNestedFoldsList fix crash with cached data
  * r57569 MenuEditor: Fixed wrong item height when in ObjectInspector caption is set from text to "-" and vice versa. [Issue #33415](<https://bugs.freepascal.org/view.php?id=33415>)
  * r57571 LCL: TLabel: Fixed TLabel.FocusControl select control without pressing ALT. [Issue #33526](<https://bugs.freepascal.org/view.php?id=33526>)
  * r57572 LCL: GTK2: TLabel: Accelerating keys will work correct again after revision 57571.
  * r57614 SynEdit, PasHL: fix "helper for" should not generate "foldnode-info" for "for". issue 33551
  * r57623 IDE, EditorOpts: Hide unimplemented "Undo after Save" option
  * r57653 LCL: Fix out-of-range access to Colors array in LazIntfImages.CopyPixels. [Issue #27144](<https://bugs.freepascal.org/view.php?id=27144>)
  * r57796 TAChart: Fix memory leak related to minor axes (see <https://forum.lazarus.freepascal.org/index.php/topic,41151.0.html>).
  * r57804 codetools: parsing external const
  * r57674 LCL: toolbar: fix endless align/autosize loop. [Issue #33575](<https://bugs.freepascal.org/view.php?id=33575>)
  * r57736 LazLogger: Thread safety
  * r57743 GDBMiDebugger: fix range check / used wrong variable
  * r57957 IDE: ObjectInspector: Fixed property filter input: caret jumps to begin. [Issue #33585](<https://bugs.freepascal.org/view.php?id=33585>)



## Fixes for 1.8.2 (merged)

  * r56596 MenuEditor: Fixed access violation on adding from menu templates. [Issue #32766](<https://bugs.freepascal.org/view.php?id=32766>). Patch by Serge Anvarov
  * r56600, r56628 Translations: Brazilian Portuguese translation updates by Marcelo B Paula
  * r56605 Translations: Hungarian translation update by Péter Gábor, bug 32764
  * r56609 rpm: fixed showing fpc-src rpm path
  * r56611 rpm: fixed rpm path
  * r56614 rpm: fpc-src: disable debug check
  * r56616 deb: uninstall fpc even if some files are missing
  * r56619 rpm: append linker option --build-id
  * r56621 rpm: omit debug_package
  * r56660 Converter: Fix a crash when converting a single unit. [Issue #32785](<https://bugs.freepascal.org/view.php?id=32785>)
  * r56661 IDE: Prevent crash in CodeExplorer. AddCodeNode can return Nil. [Issue #32786](<https://bugs.freepascal.org/view.php?id=32786>)
  * r56684 LCL: GTK2: TPanel DefaultColor should be clBackground, not clBtnFace. [Issue #32779](<https://bugs.freepascal.org/view.php?id=32779>)
  * r56686 LCL-Win32: Support compilation with FPC 2.6.4.
  * r56685 Contributors: Add "component icons" to Roland Hahn's entry.
  * r56688 IDE: update images/copyright.txt
  * r56689 IDE: Replace some palette icons for better visibility on dark desktop themes (provided by Roland Hahn aka Ally).
  * r56695 Revert incorrect r56398 "SynEdit: Enable operations for an identifier also when caret is at its end."
  * r56703 IDE: run file: fixed running command in working directory, added checkbox to show run output
  * r56705 RTTIControls: Add HighRes palette icons by Roland Hahn.
  * r56726 lazutils: fixed ResolveDots under Windows .\\.
  * r56729 codetools: debug FindFileAtCursor
  * r56731 IDE: open file at cursor: adjust cursor if cursor is behind word
  * r56746 IDE: view_unit dialog: do not check virtual directory via DirectoryCachePool
  * r56221 LazControls: More consistent component palette icons (by Roland Hahn)
  * r56748 synedit: HiRes component palette icons by Roland Hahn
  * r56753, r56802 IDE: regenerated translations and updated Russian translation
  * r56785 TAChart: New HiRes icons by Roland Hahn
  * r56786 OpenGL: Add new HiRes component palette icon by Roland Hahn
  * r56795 IDE: improved Select Frame dialog i18n, patch from Marcelo B Paula, bug 32841
  * r56811 LCL: TNoteBook: Fixed TNotebook fires OnBeforeShow for destroying pages. [Issue #32863](<https://bugs.freepascal.org/view.php?id=32863>). Patch by Rik van Kekem
  * r56836 LazReport: New HiRes palette icons (by Roland Hahn)
  * r55756 SynEdit: PAS highlighter: support NESTEDCOMMENTS mode switch. [Issue #32356](<https://bugs.freepascal.org/view.php?id=32356>)
  * r56325 IDE: Find/Replace: save options depending on selection availability. [Issue #32619](<https://bugs.freepascal.org/view.php?id=32619>)
  * r56857: win32: fix double CM_MOUSELEAVE messages. [Issue #32844](<https://bugs.freepascal.org/view.php?id=32844>)
  * r56880 IDE: Delete CollectionItems correctly, update also OI. [Issue #32820](<https://bugs.freepascal.org/view.php?id=32820>).
  * r56886 IDE: Fixed IDE crash after putting a activex control on form. [Issue #32375](<https://bugs.freepascal.org/view.php?id=32375>)
  * r56900 CustomDrawn: New Hi-DPI palette icons (by Roland Hahn)
  * r56901 dbexport: New Hi-DPI palette icons (by Roland Hahn)
  * r56902 mrumenu: New Hi-DPI component palette icon (by Roland Hahn)
  * r56904 Designer: Do not free component's PopupMenuComponentEditor when deleting a CollectionItem. [Issue #32820](<https://bugs.freepascal.org/view.php?id=32820>).
  * r55861 LazReport: Implements flBandKeepChild, from Lutz Mändle, [Issue #30466](<https://bugs.freepascal.org/view.php?id=30466>)
  * r55871 LazReport: regenerated translations and updated Russian translation
  * r55920 LCL: GTK2: Fixed tab cannot go from combobox to combobox. [Issue #32458](<https://bugs.freepascal.org/view.php?id=32458>)
  * r55979 LCL: TComboBox: GTK2: Fixed KeyUp and KeyDown react like tab key after revision 55920. [Issue #32458](<https://bugs.freepascal.org/view.php?id=32458>)
  * r56148 LCL: GTK2: Fixed Cannot hide TBitBtn, TFloatSpinEdit, TListBox, TCheckListBox, TMemo, TListView from TForm.OnCreate after HandleNeeded. [Issue #32527](<https://bugs.freepascal.org/view.php?id=32527>)
  * r55934 LCL: fixed incosistency in pen when using TLogPen and TExtLogPen. [Issue #32465](<https://bugs.freepascal.org/view.php?id=32465>)
  * r56942 IDE: PropertyEditor: Fixed components with published properties of type "interface" does not work. [Issue #32919](<https://bugs.freepascal.org/view.php?id=32919>). Patch by Andi Friess
  * r56987 Translations: updated all LazReport translations from trunk
  * r56884, 56948 LCL: Alternative fix of [Issue #32383](<https://bugs.freepascal.org/view.php?id=32383>) avoiding crash of DBCombobox and regression ([Issue #32463](<https://bugs.freepascal.org/view.php?id=32463>)) in DBCombobox and DBLookupCombobox
  * 56052, 56160-56161, 56200, 56206, 56210, 56220 Pascalscript: up to date with git
  * r56958 Debugger, GDBMI: fix detecting watchpoint errors
  * r56992 IDE: New Hi-Dpi palette icons for OpenGLControl and TParadox (by Roland Hahn)
  * r57005 Grids: factor out IsEmptyRow function and correctly use the ARow paramter inside the function. [Issue #0032965](<https://bugs.freepascal.org/view.php?id=0032965>).
  * r56989 SynEdit: allow pasting text in any form known by widgetset. Issue 32581
  * r56581 IDE: HiRes icons for palette PascalScript (by Roland Hahn).
  * r57030 TAChart: Fix TDateTimeIntervalChartSource supressing minutes in hour steps
  * r57042 IdeIntf: Prevent crash in DBGridColumnsPropertyEditorForm with "Delete All". [Issue #33000](<https://bugs.freepascal.org/view.php?id=33000>).
  * r57053 TAChart: Fix incorrect clipping of area series segments.
  * r57058 SynEdit: fixed pas HL for uses block. Issue 0032976
  * r57060 LCL: Fix unbound use of DBLookupCombobox (broken by r55911)
  * r57066 SynEdit: Fix crash in FoldNodeInfoList, due to Pas-HL hack attempting to alter fold info for "class of" [Issue #32940](<https://bugs.freepascal.org/view.php?id=32940>)
  * r57067 SynEdit: Fix fold gutter, after r 57066 / [Issue #32940](<https://bugs.freepascal.org/view.php?id=32940>)
  * r57078 SynEdit: fix ecDeleteEOL with multi codepoint utf8. [Issue #32838](<https://bugs.freepascal.org/view.php?id=32838>).
  * r57079,57081 SynEdit: fix issue with longest line info (cached line num was not updated). Horiz scrollbar sometimes disappeared after line was deleted
  * r57089 Debugger: Do not always auto adjust the stackframe when stopping. (temp fix) [Issue #32978](<https://bugs.freepascal.org/view.php?id=32978>).
  * r57101 IDE, Debugger: fixed name for "step to cursor". Two different names where in use for this.
  * r57103 Debugger: Allow pause button to abort auto-continue of breakpoint
  * r57198 Debugger, GDBMI: fix range check, when comparing qword for min() [Issue #33106](<https://bugs.freepascal.org/view.php?id=33106>).
  * r57113 TAChart: Fix crash if multi-valued lineseries is displayed in legend using styles.
  * r57141 Examples: In translation_demo provide ukrainian selection in language combo in correct language.
  * r57147 LCL-Win32: Fix hints in mainmenu's submenus. [Issue #33070](<https://bugs.freepascal.org/view.php?id=33070>).
  * r57200 LCL-Win32: Prevent a range error when assigning a menu handle. [Issue #33102](<https://bugs.freepascal.org/view.php?id=33102>).
  * r57152 IDE, EditorOpts: Fix stringcompare for color-names
  * r57181 IDE: updating EditorOptions.res for new lazarus_dci_file from rev 53111
  * r57182 IDE: Config for SynEdit outline colors. [Issue #32930](<https://bugs.freepascal.org/view.php?id=32930>).
  * r56909 LCL: TListView: fix setting column width. [Issue #32920](<https://bugs.freepascal.org/view.php?id=32920>)
  * r56910 LCL: TListView: store column widths on handle destruction. [Issue #32922](<https://bugs.freepascal.org/view.php?id=32922>)
  * r56946 LCL: revert r50942. [Issue #28654](<https://bugs.freepascal.org/view.php?id=28654>)
  * r56947 LCL: fix autosizing for not-aligned controls. [Issue #28654](<https://bugs.freepascal.org/view.php?id=28654>)
  * r56962 LCL: High-DPI: fix font scaling and design-time scaling. [Issue #32882](<https://bugs.freepascal.org/view.php?id=32882>) and [Issue #32944](<https://bugs.freepascal.org/view.php?id=32944>)
  * r57082 LCL: fix regression in autosize after r56947. [Issue #28654](<https://bugs.freepascal.org/view.php?id=28654>)
  * r57156 LCL: form: scale form with parent. [Issue #33002](<https://bugs.freepascal.org/view.php?id=33002>)
  * r56974 IDE: IDEImagesIntf: support transparent bitmaps. [Issue #32947](<https://bugs.freepascal.org/view.php?id=32947>)
  * r57238 TAChart: Fix brush style bsClear not working in TClearBrush
  * r57239 TAChart: Fix ZoomDragTool ignoring its Frame.Visible setting.
  * r57252 Opkman: Bug fix: Updating package from the maintainers webpage in some special cases failed.
  * r57265 LCL:fix scaling of font size set to <>0 in design time. [Issue #33132](<https://bugs.freepascal.org/view.php?id=33132>), regression after r56962
  * r57272 TAChart: Fix DatapointCrosshairTool ignoring its CrosshairPen.Visible setting.
  * r57318 lcl: lcltranslator: FindLocaleFileName: fixed checking if dir is absolute
  * r57321 lcl: translations: fixed getting duplicate msg from .mo file, issue 33173
  * r57339 codetools: parse built-in function concat in constant, issue 33201
  * r57351, 57362 Translations: LCL Polish translation update by Jacek Kruszniewski, bug 33200
  * r57355 LCL/ValuelistEditor: Change column titles from "Key"/"Name" to "Key"/"Value"
  * r57361 SynEdit: Completion box, respect space for taskbar. Use screen.WorkArea. Issue 033180



## Fixes for 1.8.0 (merged)

  * r54792 Carbon: implemented LCLIntf.ShowWindow(SW_RESTORE). Patch by Craig Peterson. [Issue #0021816](<https://bugs.freepascal.org/view.php?id=0021816>)
  * r55630 GTK2: an attempt to reduce the number of superfluous OnChanges of TFloatSpinEdit. [Issue #0031618](<https://bugs.freepascal.org/view.php?id=0031618>).
  * r55709 LCL-GTK2: Fix UnityTrayIcon crash on gdk_pixbuf_save. [Issue #32258](<https://bugs.freepascal.org/view.php?id=32258>).
  * r56015 Codetools: Do not chomp a procedure name with empty brackets (). [Issue #31960](<https://bugs.freepascal.org/view.php?id=31960>).
  * r56028 Codetools: Add dotted unit names to uses section correctly. Optimize. [Issue #32247](<https://bugs.freepascal.org/view.php?id=32247>).
  * r56029 Fix TMask.InitMaskString for charsets when CaseSensitive is False. [Issue #0032491](<https://bugs.freepascal.org/view.php?id=0032491>). Patch by delfion.
  * r56031 LCL: Fixed TDateEdit/TTimeEdit controls break the tab order. [Issue #32533](<https://bugs.freepascal.org/view.php?id=32533>)
  * r56036 Codetools: updated Russian translation
  * r56041 codetools: fixed CompleteCode TryAssignment loop
  * r56044 codetools: fixed complete forward proc
  * r56046 IDE: High-DPI new icons. [Issue #32508](<https://bugs.freepascal.org/view.php?id=32508>). Patch by FTurtle
  * r56054 CodeTools: Don't throw an error with "(" inside a comment in front of unit. [Issue #32235](<https://bugs.freepascal.org/view.php?id=32235>).
  * r56061 IDE: High-DPI refactoring of some images. [Issue #32529](<https://bugs.freepascal.org/view.php?id=32529>). Patch by FTurtle
  * r56063 Translations: Ukrainian translation update by Olexandr Pylypchuk, bug 32544
  * r56071 codetools: fixed example
  * r56073 codetools: AddUnitWarnDirective: fixed startnode
  * r56077 codetools: fixed AddUnitWarnDirective insert position
  * r56083 IDE: open file at cursor: show error message on fail
  * r56058 Package editor: Fix moving dependencies up/down. Use the display name for searching. [Issue #32546](<https://bugs.freepascal.org/view.php?id=32546>).
  * r56068,r56069,r56082 Improve Package Editor for dependent packages. [Issue #32546](<https://bugs.freepascal.org/view.php?id=32546>).
  * r56085 Codetools: Fix code completion for methods with empty parentheses. [Issue #31960](<https://bugs.freepascal.org/view.php?id=31960>).
  * r56086 LCL: Win32: Fixed TComboBox tab key isn't working with style csSimple. [Issue #32559](<https://bugs.freepascal.org/view.php?id=32559>)
  * r56087 IDE: regenerated translations and updated Russian translation
  * r56098 IDE: fixed layout
  * r56100 IDE: set watch property dialog to autosize, bug 32567
  * r56102 IDE: updated lazarus.lpi and fixed editor macro layout, bug 32568
  * r56106 IDE: unit info dlg: improved layout
  * r56108 projecttemplates: layout: anchors
  * r56110 leakview: fixed layout
  * r56119 lcl: lcltranslator: fixed translating only TTranslateString properties
  * r56120 lcl: lcltranslator: skip components without name
  * r56121 lcl: lcltranslator: fixed using property path, instead of subcomponent names, subcomponents names can be empty, same as IDE
  * r56124 lcl: lcltranslator: use typeinfo instead of type name
  * r55710 IDE: Improve CompilerOtherOptions frame layout. [Issue #32272](<https://bugs.freepascal.org/view.php?id=32272>).
  * r56134 IDE: Update components_images.res (forgotten in r55966)
  * r56136 IDE: Improve the layout of Project Options -> FPDocEditor. [Issue #32574](<https://bugs.freepascal.org/view.php?id=32574>).
  * r56144 IDE: High-DPI: Refreshed open icons. [Issue #32595](<https://bugs.freepascal.org/view.php?id=32595>). Patch by FTurtle
  * r56035 IdeIntf, ComponentTree: Don't try to update Collection name. Affects TSQLQuery Params. [Issue #31965](<https://bugs.freepascal.org/view.php?id=31965>).
  * r56146 IdeIntf: Fix TVariantPropertyEditor. [Issue #19324](<https://bugs.freepascal.org/view.php?id=19324>).
  * r56130 LCL: GTK2: Using nboKeyboardTabSwitch in TabControl options and <Ctrl> \+ <Tab> now working.
  * r56131 LCL: GTK2: TTabControl: Cannot use keyboard to switch tab. [Issue #31986](<https://bugs.freepascal.org/view.php?id=31986>)
  * r56112,r56122 LCL-GTK2: Support 5 button mice. [Issue #32562](<https://bugs.freepascal.org/view.php?id=32562>).
  * r56140 LCL: GTK2: Fixed set TPanel visible to false on non visual TabSheet doesn't work. [Issue #32593](<https://bugs.freepascal.org/view.php?id=32593>)
  * r56149 Translations: French translation update by Gilles Vasseur, bug 32587
  * r56150 Translations: Ukrainian translation update by Olexandr Pylypchuk, bug 32590
  * r56155 IDE: Focus FilterEdit in options initially. ActiveControl was cleared again for some reason.
  * r56143,r56156 AnchorDocking: Use async call for SimplifyPendingLayouts always. [Issue #32342](<https://bugs.freepascal.org/view.php?id=32342>),[Issue #32591](<https://bugs.freepascal.org/view.php?id=32591>),[Issue #31895](<https://bugs.freepascal.org/view.php?id=31895>).
  * r56193 lcl: gtk2: fixed error handling in GtkDragDataReceived, bug 32611, patch from C Western
  * r56197 codetools: fixed GetIdentStartEndAtPosition if pos behind source
  * r56232 codetools: fixed adding uses section to program
  * r56162 LCL: Fixed TSpinEditEx Focus on Tab Click doesn't work. [Issue #32601](<https://bugs.freepascal.org/view.php?id=32601>)
  * r56219 LazControls: New HiRes component palette icons (kindly provided by Roland Hahn).
  * r56180 win32: fix GetDpiForMonitor SIGSEGV if Shcore is not available (e.g. on MS Windows Server 2012 R2)
  * r56181 Fix firing an OnChange and setting Modified when setting Text inside OnChange and new Text = old Text. [Issue #0032602](<https://bugs.freepascal.org/view.php?id=0032602>).
  * r56185 lazutils: fixed ResolveDots: ./ -> .
  * r56187 LazFileUtils: rename a local variable to improve readability. No functional changes. (Merge request to avoid possible future merge conflicts)
  * r56234 SynEdit: fix mark gutter, calculating width of mark-image [Issue #32622](<https://bugs.freepascal.org/view.php?id=32622>)
  * r56223 examples/treeview: Use default font (instead of MS SansSerif)
  * r56225 examples/ResizeImageDemo: remove unit Windows from "uses".
  * r56226 examples/dbeditmask: Fix crash due to incorrect dateseparator in some countries. Use default font instead of MS SansSerif.
  * r56227 examples/componentstreaming: Prevent explaining labels reaching beyond form borders.
  * r56228 Examples/synedit1: Fix compilation.
  * r56235 LCL: Prevent a crash when getting editor for an empty Grid. [Issue #32623](<https://bugs.freepascal.org/view.php?id=32623>).
  * r56242 LCL: Make procedure RunDialog public in TFileNameEdit, TDirectoryEdit and CalcEdit. Helps with shortcut keys.
  * r56244 AnchorDocking: Show also inactive tabed windows in Window-menu. [Issue #32618](<https://bugs.freepascal.org/view.php?id=32618>).
  * r56248 Move package SemaphorGridLPK and its example project to Lazarus-CCR.
  * r56249 Delete example "SpeedTest". Image penguin.xpm is missing and it makes no sense.
  * r56250 Delete example "FpDocManager". Does not work.
  * r56251 IDE: HiRes icons for component palette "Misc" (by Roland Hahn)
  * r56252 Debugger: Ensure correct thread id in history entries
  * r56253 IDE: High-DPI new icons. Issue [Issue #32628](<https://bugs.freepascal.org/view.php?id=32628>). Patch by FTurtle.
  * r56256 IDE: Remember the "Origin" setting in Search/Find dialog. [Issue #32619](<https://bugs.freepascal.org/view.php?id=32619>).
  * r56261 codetools: cleaned up (removed program changeparamlist).
  * r56262 CodeTools example AddFpprofCalls: show help if no filename is given. Fix memory leak.
  * r56263 codetools: example addmethod with fpc sources
  * r56266 codetools: examples: fixed completeblock
  * r56268 codetools: examples: fixed fixfilenames
  * r56269 codetools: fixed FindMissingUnits
  * r56275 codetools: examples: fixed getexpandedoperand
  * r56276 codetools: formatted, fixed GetExpandedOperand
  * r56279 codetools: ReduceCompilerDirectives fixed crash
  * r56281 IDE: designer: fixed store selection bounds for mediator, patch from Anton
  * r56283 IDE: fixed memleak in example designnonlcl
  * r56285 sqldb: New HiDpi-icons by Roland Hahn
  * r56286 IDE: HiRes palette icons for default and unregistered component (by Roland Hahn)
  * r56291 IDE: File / Open : fixed browsing linked folders on Windows, bug 32642
  * r56293 codetools: parse specialize in param type
  * r56319 codetools: fixed copying proc param specialize
  * r56323 codetools: fixed class completion for method with specialize in mdoe delphi
  * r56310 plotfunction: Fix vertical alignment of y axis caption ([Issue #26312](<https://bugs.freepascal.org/view.php?id=26312>))
  * r56313 plotfunction: Fix memory leak.
  * r56314 plotfunction: Make axes and fonts better respond to changes in object inspector ([Issue #26311](<https://bugs.freepascal.org/view.php?id=26311>))
  * r56315 plotfunction: Fix scaling of plots.
  * r56350 LCL: revert r55892
  * r56344 LCL: revert r56162, r56031, r55890 (TabStop handling)
  * r56349 Anchordocking: Fix loading the same layout twice. [Issue #32660](<https://bugs.freepascal.org/view.php?id=32660>)
  * r56353 Revert r56181 (Fix firing an OnChange and setting Modified when setting Text inside OnChange and new Text = old Text. [Issue #0032602](<https://bugs.freepascal.org/view.php?id=0032602>).) It causes a regression.
  * r56373 lcl: Minor revision of HiRes images of lcl controls (by Roland Hahn)
  * r56379 IDE: Slightly improved versions of some component palette icons (by Roland Hahn)
  * r56357 SynEdit: Select UTF8 text correctly when Persistent Block is active. [Issue #32667](<https://bugs.freepascal.org/view.php?id=32667>).
  * r56360 IDE: Show dotted unit names in Code Explorer. [Issue #32663](<https://bugs.freepascal.org/view.php?id=32663>).
  * r56366 Codetools: Find declaration for a used unit also when caret is at its end. [Issue #32606](<https://bugs.freepascal.org/view.php?id=32606>).
  * r56398 SynEdit: Enable operations for an identifier also when caret is at its end.
  * r56413 LCL,Gtk2: fix Dialog modal windows go behind regular windows and won't come on top. [Issue #32635](<https://bugs.freepascal.org/view.php?id=32635>)
  * r56428 IDE: Auto-columnwidths in charactermap [Issue #32701](<https://bugs.freepascal.org/view.php?id=32701>).
  * r56429 LCL: Win32: Prevent AV at closing form.
  * r56431 LCL: Fix setting TTreeNode.DropTarget. [Issue #28978](<https://bugs.freepascal.org/view.php?id=28978>).
  * r56432 Printers: New HiRes component palette icons of the printer dialogs (by Roland Hahn)
  * r56433 IDE: New HiRes icons for the Dialogs component palette (by Roland Hahn)
  * r56442 gdbmi debugger tests
  * r56443 TAChart: Fix empty TPolarSeries crashing if ShowPoints is true.
  * r56444 Opkman: Spanish translation update by Gastón, bug 32706
  * r56457 gdebugger, gdbmi: fix stepping over exceptions
  * r56466 lcl: gtk2: trayicon fixed missing nil parameter
  * r56473 LCL: Prevent rotated font from being detected as default font (patch by Michl)
  * r56475 TAChart: Introduce default value of property TOpenHighLowCloseSeries.Mode
  * r56480 TAChart: Fix incorrect extent calculation of TOpenHighLowCloseSeries.
  * r56484 Turbopower_ipro: New HiRes component palette icons by Roland Hahn
  * r56486 codetools: fixed parsing deprecated
  * r56490 TAChart: Fix RandomChartSource crashing if PointsNumber is 1.
  * r56499 IDE: New HiRes palette icons by Roland Hahn for database-related components
  * r56517 IDE: fixed parameter hint auto completion when closing bracket is missing, bug 32736
  * r56526 codetools: complete property: set defaults, complex type
  * r56533 LCL: InputQuery: High-DPI. By AlexeyT. Issue 32717
  * r56534 Translations: huge Chinese translation update by robsean, bug 32750
  * r56535 Translations: Brazilian Portuguese translation update by Marcelo B Paula, bug 32754
  * r56539 Docs: added Olexandr Pylypchuk and Zheng Jian Ping aka "robsean" to contributors list
  * r56540 Lazlogger: Allow setting to nil. Issue 32548
  * r56541 lclbase: added missing include/comboex.inc
  * r56543 lcl: TCustomCheckCombo: changed TCheckComboItemState to class, added checks if Items.Objects is TCheckComboItemState, async raise error to avoid freezing X
  * r56545 IDE: updated lazarus.lpi
  * r56458 gdebugger, gdbmi: fix stepping over exceptions - TESTS
  * r56578 codetools: fixed clearing errormsg, issue 32761, from Pascal



## Fixes for 1.8 RC5 (merged)

  * r54902 IdeIntf: API function for uninstalling a package. From Balázs. (Needed for OPM)
  * r55635 gtk2: fix ScreenInfo.PixelsPerInch* values. [Issue #31199](<https://bugs.freepascal.org/view.php?id=31199>), by Michl
  * r55646, r55763 Translations: Ukrainian translation update by Olexandr Pylypchuk
  * r55653 LCL: task dialog: fix radio button result. [Issue #32231](<https://bugs.freepascal.org/view.php?id=32231>), patch by Howard Page-Clark
  * r55658 Opkman: VST support for arm-linux. Patch from @Thaddy.
  * r55659 lcl: forms: don't change Font PPI if Application.Scaled=False. [Issue #32260](<https://bugs.freepascal.org/view.php?id=32260>)
  * r55660 LCL: font: IsEqual should ignore PixelsPerInch value - Assign ignores it as well.
  * r55662 IDE: High-DPI: scale open file drop down menu icons.
  * r55663 IDE: restore desktop with correct name after debugging
  * r55665 LCL: GTK2: Fixed font issues at high-DPI on Gtk2. [Issue #32268](<https://bugs.freepascal.org/view.php?id=32268>).
  * r55666 IDE: Fixed initial mainbar height for Windows. [Issue #32277](<https://bugs.freepascal.org/view.php?id=32277>)
  * r55668 ideintf: Fix "new action" button dropdown arrow.
  * r55669 lcl: dbgrids: high dpi: scale indicator column width
  * r55670 LCL: High-DPI cursors. [Issue #32280](<https://bugs.freepascal.org/view.php?id=32280>)
  * r55672 IdeIntf: OI: center current row arrow vertically. [Issue #32280](<https://bugs.freepascal.org/view.php?id=32280>)
  * r55673 LazControls: In FilterEdits check everywhere if the associated control exists. [Issue #32282](<https://bugs.freepascal.org/view.php?id=32282>).
  * r55674 IdeIntf: High-DPI: support scale factors above 200%. [Issue #32280](<https://bugs.freepascal.org/view.php?id=32280>)
  * r55675 synedit: high-DPI: implement DoAutoAdjustLayout. [Issue #31753](<https://bugs.freepascal.org/view.php?id=31753>)
  * r55676 IDE: scale sourceeditor. [Issue #31753](<https://bugs.freepascal.org/view.php?id=31753>)
  * r55677 synedit: high-DPI fixes. [Issue #31753](<https://bugs.freepascal.org/view.php?id=31753>)
  * r55681 codetools: fpc sources: packegs/rtl-extra add include path src/SrcOS and src/SrcOS2, bug 32263
  * r55699 IDE: quickfixes: auto open changed units, bug 32284
  * r55704 LCL: ComboBox: Win32: Don't loose ItemIndex with AutoComplete set. [Issue #32301](<https://bugs.freepascal.org/view.php?id=32301>)
  * r55716 lcl: edit: force show texthint in InitializeWnd. [Issue #32053](<https://bugs.freepascal.org/view.php?id=32053>)
  * r55717 ide: options dialog: filter edit: use parent font
  * r55718 qt: edit: force show texthint in ShowHide. [Issue #32053](<https://bugs.freepascal.org/view.php?id=32053>)
  * r55734 IDE: high-DPI: scale created components
  * r55754 IDE: Fix logic for switching windows from "Window" menu. [Issue #32350](<https://bugs.freepascal.org/view.php?id=32350>)
  * r55755 LCL: Missed assigning of default font value Style.
  * r55757 LCL: Fixed Canvas.TextExtend if system font is used for drawing. [Issue #32319](<https://bugs.freepascal.org/view.php?id=32319>)
  * r55758 LCL: Win32: Fixed themed drawing for system font usage. [Issue #32319](<https://bugs.freepascal.org/view.php?id=32319>)
  * r55761 LCL: Disable scaling for default fonts, as they are correctly sized taken from OS. [Issue #32359](<https://bugs.freepascal.org/view.php?id=32359>)
  * r55775 IDE: SourceEditor: IDESynGutterMarks High-DPI aware.
  * r55779 CodeTools: remove inline, private, public and published from keyword list - they can be used as identifiers as well. [Issue #32336](<https://bugs.freepascal.org/view.php?id=32336>)
  * r55780 TAChart: Add overloaded Clone method to set a new parent and owner [Issue #32365](<https://bugs.freepascal.org/view.php?id=32365>)
  * r55781 IDE: MessageWindow: Win32: Messages painting: Prevent flickering.
  * r55785 IDE: New HiDPI component icons, kindly provided by Roland Hahn.
  * r55786 IDE: Components_images.res forgotten in previous commit
  * r55787 IDE: Some more HiDPI icons by Roland Hahn
  * r55789 LCL-GTK2: Partly revert r53653, check for gtk_minor_version again. [Issue #32196](<https://bugs.freepascal.org/view.php?id=32196>)
  * r55795 LCL: GTK2: Fixed cursor size. [Issue #32385](<https://bugs.freepascal.org/view.php?id=32385>)
  * r55796 LCL: GTK2: High-DPI cursor crNo.
  * r55797 IDE: High-DPI BookMark icons. Patch by FTurtle. [Issue #32374](<https://bugs.freepascal.org/view.php?id=32374>)
  * r55798 IDE: Forgotten copyright info from revision 55797
  * r55820 LCL: In TCustomcalendar, always SetProps in InitializeWnd ([Issue #0032379](<https://bugs.freepascal.org/view.php?id=0032379>)). Patch by soner.
  * r55842 lcl: fix docs about TControl.SetBounds
  * r55851 codetools: find declaration of dotted program name
  * r55827 IDE: More component palette icons by Roland Hahn completing the Standard palette.
  * r55829 IDE: Remove printer-related icons from folder images/components (they are in components/printers/images)
  * r55830 IDE: Update components_images_list.txt (adding of scale TFrame and TPanel icons forgotten in prev commits).
  * r55831 IdeIntf: Exit from TEnumPropertyEditor.SetValue when invalid value is given. Prevents an AV later. Reverts r185 from 2001-02-19.
  * r55840 IDE: SourceEditor: High-DPI menu bookmark icons. [Issue #32396](<https://bugs.freepascal.org/view.php?id=32396>). Patch by FTurtle
  * r55860 IDE: SourceEditor: High-DPI menu bookmark icons. [Issue #32416](<https://bugs.freepascal.org/view.php?id=32416>). Patch by FTurtle
  * r55862 IDE: New icons for palette "Additional" (provided by Roland Hahn).
  * r55863 IDE: Update components_images_list.txt and components_images.res missing in previous commit.
  * r55864 IDE: Add HighRes TScrollbox icons to resource
  * r55866 IDE: Refactoring resource image lists files. [Issue #32420](<https://bugs.freepascal.org/view.php?id=32420>). Patch by FTurtle.
  * r55867 Contributors: Add Roland Hahn (component palette icons)
  * r55876 IDE: HiRes icons for the Common Controls palette (by Roland Hahn).
  * r55877 IDE, OI: Update the component ComboBox properly when it is visible. [Issue #32427](<https://bugs.freepascal.org/view.php?id=32427>)
  * r55878 IDE: High-DPI menu items. [Issue #32431](<https://bugs.freepascal.org/view.php?id=32431>). Patch by FTurtle.
  * r55879 PackageTabs: Fixed icon after revision 55878.
  * r55880 IDE: Cleanup: Delete unused images.
  * r55868 LCL: treeview: fix tooltips. Patch by AlexeyT. [Issue #32400](<https://bugs.freepascal.org/view.php?id=32400>)
  * r55848 LCL, fix message dialogs hidding behind modal dialogs [Issue #32249](<https://bugs.freepascal.org/view.php?id=32249>)
  * r55875 DateTimeCtls: HiRes icons for TDateTimePicker by Roland Hahn. Switch icons to .res format.
  * r55884 LCL: Font: Revert revision 55761 as it breaks per monitor High-DPI scaling.
  * r55881 Qt,Qt5: fixed cpu eating by gtk+ theme.
  * r55890 LCL: TCustomEditButton: Fixed TDateEdit.SelectNext. [Issue #32335](<https://bugs.freepascal.org/view.php?id=32335>)
  * r55892 LCL: TTabControl: Fixed AV on creating control. [Issue #32455](<https://bugs.freepascal.org/view.php?id=32455>)
  * r55915 codetools: property completion: fixed check if store accessor with index specififer exists
  * r55917 codetools: property completion: fixed read/write accessor for param+index specifier
  * r55904 IDE: Update copyright.txt for component palette icons created by Roland Hahn.
  * r55906 IDE: Add HiRes palette icons for TDBDateTimePicker.
  * r55919 TAChart: Fix event TDbChartSource.OnGetItem not being alterable in designer. Add demo db-barseries.
  * r55944 codetools: fixed RemoveUnitFromAllUsesSections
  * r55946 codetools: fixed skipping sourcename, added TPascalParserTool.FindUsesNode
  * r55948 codetools: fixed continuing parsing after source name
  * r55951 codetools: added ctnSrcName, fixed find declaration dotted program name
  * r55926 IDE: Replace TStaticText palette icons with fully transparent ones.
  * r55932 IDE: High-DPI icons. [Issue #32454](<https://bugs.freepascal.org/view.php?id=32454>) Patch by FTurtle
  * r55941 SynEdit: Paint Caret in paint-event (except win32) / some very few gtk may have issues
  * r55953 DBExport examples: fix compilation and layout, improved usability
  * r55954 Debugger: In BreakpointPropertyDlg, add new conditions to top of list, remove duplicates and reject empty strings.
  * r55956 IDE: Add some HiRes icons for Run menu (drawn by Roland Hahn)
  * r55957 IDE: Update copyright.txt for icons added in previous commit
  * r55554 Qt, Qt5: fixed ItemShow - scroll to item. [Issue #32078](<https://bugs.freepascal.org/view.php?id=32078>)
  * r55555 Qt5: use QScreen_grabWindow() instead of QPixmap_grabWindow(). part of [Issue #32141](<https://bugs.freepascal.org/view.php?id=32141>)
  * r55737 Qt4: fixed build under mswindows
  * r55759 Qt5: implemented InitStockFont. part of [Issue #32354](<https://bugs.freepascal.org/view.php?id=32354>)
  * r55760 Qt,Qt5: fixed painting of TQtHintWindow. [Issue #32354](<https://bugs.freepascal.org/view.php?id=32354>)
  * r55774 Qt, Qt5: check if handle is valid inside various routines, fixes some sparta ide crashes.
  * r55782 Qt5: TQtGroupBox does not need offset as in Qt4, since Qt5 provides correct coordinates. [Issue #32186](<https://bugs.freepascal.org/view.php?id=32186>)
  * r55966 IDE: HiRes icons for System component palette (provided by Roland Hahn)
  * r55969 IDE: High-DPI icons. [Issue #32493](<https://bugs.freepascal.org/view.php?id=32493>) Patch by FTurtle
  * r55970 IDE: In ToolBarConfig ListView select correct item after adding a command. Refactoring etc.
  * r55971 components/wiki: Fix compilation of wikiget
  * r55975 LCL: Do not trigger Escape key for a disabled cancel button. [Issue #32509](<https://bugs.freepascal.org/view.php?id=32509>)
  * r55984 Translations: Hungarian translation update by Péter Gábor, bug 32503
  * r55986 IDE: Remove an extra IDEDialogLayoutList.ApplyLayout call from NewDialog constructor.
  * r55992,r55994,r55996,r55997,r56004 IDE: Less questions when opening a symlinked file. [Issue #32292](<https://bugs.freepascal.org/view.php?id=32292>).
  * r55998 LCL-GTK2: Fix SelStart and SelLength in OnChange with some widgetset versions. [Issue #24371](<https://bugs.freepascal.org/view.php?id=24371>).
  * r56001 IDE: designer: High-DPI: selection grabber. [Issue #32397](<https://bugs.freepascal.org/view.php?id=32397>)
  * r56003 IDE: designer: tune up r56001, [Issue #32397](<https://bugs.freepascal.org/view.php?id=32397>)



## Fixes for 1.8 RC4 (merged)

  * r55411 Debugger: Fix anchoring in WatchPropertyDlg.
  * r55412 Finnish translations for IDE and OI.
  * r55413 DBRadiogroup: Fix crash when clicking without assigned field ([Issue #0032077](<https://bugs.freepascal.org/view.php?id=0032077>)).
  * r55415 LCL: Japanese translation update by Haruyuki Fujimaki, [Issue #32081](<https://bugs.freepascal.org/view.php?id=32081>)
  * r55462 IDE: fixed checking for file changes on non UTF-8 files
  * r55433 Sparta_DockedFormEditor: Fixed drawing of MainMenu after adding the first MenuItem. [Issue #31206](<https://bugs.freepascal.org/view.php?id=31206>)
  * r55442 Win32: TGroupBox: Fixed wrong background color for inactive groupbox.
  * r55453 QT, QT5: Initialize WideString Result in TQtTextEdit methods.
  * r55459 LCL-Win32: Process surrogate pairs correctly when virtual keyboard is used. [Issue #32101](<https://bugs.freepascal.org/view.php?id=32101>).
  * r55460 IDE: Fix left/right arrow key behavior in identifier completion. [Issue #32117](<https://bugs.freepascal.org/view.php?id=32117>)
  * r55466 Translations: Japanese translation update by takahiro and Haruyuki Fujimaki, bugs 32099, 32100
  * r55467 IDE: Turkish translation update by lazarusturkiye, bug 32104
  * r55474, r55502 Translations: Lithuanian translation update by Valdas Jankūnas, bug 32126
  * r55479 lcl: fixed typos
  * r55482 anchordocking: fixed hiding docksite when last control gets Visible=false
  * r55485 codetools: parse generic function
  * r55489 Translations: French translation update by Gilles Vasseur, bug 32133
  * r55520 LCL: skip property TDataModule.PPI on FPC < 3.0.3
  * r55526 LCL: Dutch translation update by Bart, bug 32149
  * r55492 IDE: High-DPI: ObjectInspector: Fixed drawing of active row bitmap. [Issue #32097](<https://bugs.freepascal.org/view.php?id=32097>)
  * r55494 grids: Introduce constColRowBorderTolerance for colwidth/rowheight resizing.
  * r55495 grids: High-dpi scaling of constCellPadding, constRubberSpace and constColRowBorderTolerance
  * r55496 IDE: High-DPI: ObjectInspector: Size of filter edit glyphs. [Issue #32097](<https://bugs.freepascal.org/view.php?id=32097>)
  * r55498 IDE: High-DPI: Fixed image sizes of in IDE used TTreeFilterEdits and TListFilterEdits.
  * r55499 IDE: High-DPI: ComponentPagesPopup: Size of close button glyph. [Issue #32097](<https://bugs.freepascal.org/view.php?id=32097>)
  * r55500 IDE: High-DPI: ComponentPalette: Size of component pages speedbutton.
  * r55507 IDE: SearchResultWindow: Anchor of TreeFilterEdit to always show its button.
  * r55508 IDE: High-DPI: ObjectInspector: ComponentTreeView: Fixed image sizes.
  * r55509 IDE: High-DPI: ComponentPages: Fixed tree image sizes.
  * r55510 IDE: High-DPI: ActionsEditor: Fixed image sizes.
  * r55511 IDE: High-DPI: CodeExplorer: Fixed image sizes.
  * r55512 IDE: High-DPI: Better assigning of images to imagelists.
  * r55513 IDE: High-DPI: SourceEditor: Fixed source mark image sizes.
  * r55514 IDE: High-DPI: CodeBrowser: Fixed image sizes.
  * r55515 IDE: EditorDisplayOptions: Fixed wrong anchor of DisableAntialiasingCheckBox.
  * r55516 LCL: TTreeView: High-DPI: Make spaces between images DPI-aware.
  * r55530 LCL: form: force scale bounds in WM_DPICHANGED message handler. [Issue #32162](<https://bugs.freepascal.org/view.php?id=32162>)
  * r55531 LCL: TPromptDialog: high-DPI
  * r55533 LCL: TPromptDialog: support ChangeScale
  * r55534 lcl: groupededit: revert r53759 - buddy is scaled automatically. [Issue #32154](<https://bugs.freepascal.org/view.php?id=32154>)
  * r55537 lcl: control: high-DPI: correctly scale base bounds. [Issue #31992](<https://bugs.freepascal.org/view.php?id=31992>)
  * r55539 lcl: dbgrids: hide vertical scrollbar for ssNone and ssHorizontal style. [Issue #32134](<https://bugs.freepascal.org/view.php?id=32134>)
  * r55541 lazbuild: nicer error message on lpi file not found
  * r55548 startlazarus: start IDE on Mac via open, bug 32180, patch from Anthony Walter
  * r55558 TurboPower_iPro: Fix colored table background not being painted.
  * r55561 avglvltree: fix DisposeNode for nil Node. [Issue #32189](<https://bugs.freepascal.org/view.php?id=32189>)
  * r55568 IDEIntf: TOICustomPropertyGrid: handle wheel events on grid the same as on edit cell control, issue 32193, from Anthony Walter
  * r55583 Translations: Ukrainian translation update by Olexandr Pylypchuk, bug 32184
  * r55318 DateTimePicker: Add RemoveHandlerOnChange, RemoveHandlerOnCheckBoxChange, override RemoveAllHandlersOfObjects
  * r55292 DateTimePiker: Built-in UpDown control appears flat with dtpoFlatButton option
  * r55279 Fix indentation
  * r55435 datetimepicker: update date also when handle is not yet allocated. [Issue #32106](<https://bugs.freepascal.org/view.php?id=32106>)
  * r55560 LazUtils: add Laz_AVL_Tree to the package
  * r55609, r55622 Translations: Ukrainian translation update by Olexandr Pylypchuk
  * r55610 IDE: Polish translation update by Rafał Klekowski, bug 32217
  * r55589 docs: Improved font for source code in lhelp (at least on Windows)
  * r55591 TAChart: Fix incompletely displayed series marks for style smsLabelPercentTotal. [Issue #32202](<https://bugs.freepascal.org/view.php?id=32202>)
  * r55592 Turbopower_ipro: Underline links only at mouse-over.
  * r55600 IDE: ProcedureList: Fixed height of edit and combobox. Issue [Issue #32212](<https://bugs.freepascal.org/view.php?id=32212>)
  * r55598 LCL: combobox: sanity check for ItemIndex
  * r55604 SynEdit: assert correct method types
  * r55605 IDE options: change description and limit for "match whole word" of highlight equal terms. Issue [Issue #032068](<https://bugs.freepascal.org/view.php?id=032068>) Based on Patch by AlexeyT
  * r55606 SynEdit: fix range error. Issue [Issue #0032067](<https://bugs.freepascal.org/view.php?id=0032067>) Patch by Pascal R.
  * r55613 SynEdit: fix crash in fold structure code TLazSynEditNestedFoldsList



## Fixes for 1.8 RC3 (merged)

  * r55062 Gtk2,Gtk3: gtk should always send LM_SETFOCUS before mouse messages, like Qt and Win32 does. [Issue #0031900](<https://bugs.freepascal.org/view.php?id=0031900>)
  * r55063 Gtk3: focus message is not needed under gtk3 since it is organized in different way.
  * r55098 IdeIntf: propedits: fix call stored method with index parameter if needed. [Issue #31923](<https://bugs.freepascal.org/view.php?id=31923>)
  * r55133 DateTimeCtrls: TDateTimePicker: add Options property with dtpoDoChangeOnSetDateTime (not default). [Issue #31679](<https://bugs.freepascal.org/view.php?id=31679>)
  * r55184 Gtk2: fixed sizing of form with constraints. [Issue #0031950](<https://bugs.freepascal.org/view.php?id=0031950>)
  * r55211 Fix uninitialized variables based on compiler warnings got with dfa (data flow analysis) enabled.
  * r55213 LCL: Fixed access violation with Application.BidiMode bdRightToLeft and ChildSizing.Layout cclTopToBottomThenLeftToRight. [Issue #31966](<https://bugs.freepascal.org/view.php?id=31966>)
  * r55220 IDE: ObjectInspector: Fixed OI don't show "default" in list of font names. [Issue #31958](<https://bugs.freepascal.org/view.php?id=31958>)
  * r55222 Fix more uninitialized variables and suppress compiler warnings. Merged manually.
  * r55233 QT, QT5: Initialize WideString Result in TQtLineEdit methods.
  * r55234 DateTimePicker - corrected misleading comment
  * r55238 codetools: added TFindDeclarationTool.FindClassMember
  * r55240 IDE: TMainIDE.OnPropHookCreateMethod: fixed creating JITMethod for ancestor
  * r55241 LazUtils: Fix compilation of utf8process.pp on Darwin with fpc trunk. [Issue #0031525](<https://bugs.freepascal.org/view.php?id=0031525>)
  * r55249 IDE: Fixed TControl.InvalidatePreferredSize loop detected. [Issue #30617](<https://bugs.freepascal.org/view.php?id=30617>)
  * r55252 Translations: Ukrainian IDE translation update by Olexandr Pylypchuk. [Issue #31957](<https://bugs.freepascal.org/view.php?id=31957>)
  * r55259 IDEIntf: fixed TIDEMenuSection.Visible keeping Visible of children, bug 31970
  * r55261 IDE: method property editor: fixed rename
  * r55263 IDE: Revert revision 55249 as it breaks the scrolling of TScrollBox in designer. [Issue #7114](<https://bugs.freepascal.org/view.php?id=7114>)
  * r55266 DateTimePicker - improve smart calling change when Date/Time is set from code
  * r55267 lcl: grids: fix endless loop in ScrollToCell for 0 Width/Height and non-SmoothScroll. [Issue #31942](<https://bugs.freepascal.org/view.php?id=31942>)
  * r55268 DateTimePicker - add input checking in SendExternalKey and add SendExternalKeyCode method
  * r55269 AnchorDockingDsgn: Loading for the first time, take the settings from the previous desktop. [Issue #31981](<https://bugs.freepascal.org/view.php?id=31981>)
  * r55271 components: datetimepicker: TextForNullDate type TCaption - for translations
  * r55272 components: datetimepicker: change boolean properties EnableWhenUnchecked, AutoCheck, FlatButton to entries in Options.
  * r55277 ide: loading a docked desktop for the first time, take the settings from the previous desktop. [Issue #31981](<https://bugs.freepascal.org/view.php?id=31981>)
  * r55279 Fix indentation
  * r55280 deb: lazarus: removed editoroptions.xml, the built-in defaults are better
  * r55295 rpm: fixed omitting editoroptions.xml
  * r55299 Translations: French translation update by Gilles Vasseur, bug 31978
  * r55300 Translations: Ukrainian translation update by Olexandr Pylypchuk, bug 31980
  * r55302 IDE: Give default size for NewDialog layout at first start. Design-time size has no effect. Set constraints.
  * r55307 IDE: Support HTML lists, "ul" and "li" tags in TCodeHelpManager. [Issue #31991](<https://bugs.freepascal.org/view.php?id=31991>).
  * r55321 MenuDesigner: High-DPI Icons.
  * r55324 IDE: High-DPI: some missing component icons.
  * r55331 codetools: guess type of default()
  * r55334 grids: Fix size of focus rectangle if gridlinewidth > 1\. [Issue #32002](<https://bugs.freepascal.org/view.php?id=32002>).
  * r55335 SpartaDockedFormEditor: Make resizer High-DPI aware.
  * r55337 SpartaDockedFormEditor: Less warnings and cleanup.
  * r55344 DBCtrls: Workaround for TDBRadioGroup being changable even if Dataset is readonly. [Issue #31993](<https://bugs.freepascal.org/view.php?id=31993>).
  * r55345 Updated Contributors
  * r55346 codetools: updated debugging messages
  * r55348 SpartaDockedFormEditor: Scale resizer automatically per LCL.
  * r55351 IDE: Extending IDE context help search to lazutils.chm, issue 32022 from zbyna
  * r55357, r55358, r55359 Translations: Hungarian translation update by Péter Gábor, bug 32014
  * r55361 IDE: lazutils help: fixed freeing, bug 32022
  * r55374 lcl: win32: ClipboardFormatToMimeType: fixed not raising an exception
  * r55381 lcl: dbgrids: fix default col widths. [Issue #32021](<https://bugs.freepascal.org/view.php?id=32021>)
  * r55384 images: new high-dpi images designed by FTurle. [Issue #32029](<https://bugs.freepascal.org/view.php?id=32029>), patch by FTurtle
  * r55390 MenuEditor: Better visibility of AddButtons, especially on GTK2.
  * r55391 MenuEditor: Show FakeButtons on correct place, when deleting a MenuItem. Fixed breaking revision 54863.
  * r55393 MenuEditor: Remove calculation of ShadowBox from wrong place.
  * r55394 MenuEditor: Fixed drawing of AddSubmenuFake on GTK2.
  * r55395 MenuEditor: Fixed drawing of AddSiblingFake on GTK2.
  * r55339 SynEdit: Pas HL, handle generics foo<T: class> does not fold / patch by Pascal
  * r55376 SynEdit: Pas-HL, test for ^ in PMyByte = type ^Byte;
  * r55377 SynEdit: fix pas HL, class function. more tests / some tests by Pascal R
  * r55396 Revert r54305 "LCL-GTK2: Prevent an occational freeze." Caused regressions.
  * r55398 lcl: application: fix exception causing handling enter keyup twice. [Issue #32056](<https://bugs.freepascal.org/view.php?id=32056>)



## Fixes for 1.8 RC2 (merged)

  * r54810 new Database Login dialog. Modified patch by LacaK, issue 27764
  * r55104 TAChart: Use SameValue() in floating point comparisons.
  * r55106 SynEdit: MarkupFoldColor, fixed crash due to wrong method signature.
  * r54895 lcl: grids: fix default TGridColumn width. [Issue #31806](<https://bugs.freepascal.org/view.php?id=31806>)
  * r54897 sqldb: fixed compile under darwin
  * r54898 lcl: forms: scale TFrame the same as TForm. [Issue #31807](<https://bugs.freepascal.org/view.php?id=31807>)
  * r54899 CairoCanvas: disable compilation for WinCE (there exists no cairo support on WinCE).
  * r54900 Printers4Lazarus: force fatal compilation error on WinCE platform, since the package is unsupported on this platform.
  * r54901 fpvectorial: Fix memory leak in dxf reader (<http://forum.lazarus.freepascal.org/index.php/topic,36827.msg246126>)
  * r54903 lcl: grids: always store valid column width. [Issue #31806](<https://bugs.freepascal.org/view.php?id=31806>)
  * r54904 IdeIntf: OI: High-DPI: restricted tab. Patch by AlexeyT, [Issue #31778](<https://bugs.freepascal.org/view.php?id=31778>)
  * r54905 IdeIntf: OI: High-DPI: value button. [Issue #31784](<https://bugs.freepascal.org/view.php?id=31784>)
  * r54911 sqldb: fixed win64 compile with fpc 3.0.2
  * r54913 lcl: colorbox: high-dpi: ColorRectWidth. [Issue #31768](<https://bugs.freepascal.org/view.php?id=31768>)
  * r54914 lcl: combobox, listbox: assign Font.PixelsPerInch to Canvas for custom draw events.
  * r54915 ide: codetoolsdefines dialog: high-DPI. Patch by AlexeyT, [Issue #31774](<https://bugs.freepascal.org/view.php?id=31774>)
  * r54916 ide: project options dialog: anchor top/bottom build modes button. [Issue #31770](<https://bugs.freepascal.org/view.php?id=31770>)
  * r54917 IdeIntf: OI: High-DPI: ValueComboBox.
  * r54918 grids: scale native bitmaps; ide: buildmodesmanager dialog: high-DPI check boxes. [Issue #31771](<https://bugs.freepascal.org/view.php?id=31771>)
  * r54922 ide: compiler_other_options frame: fix high-DPI. [Issue #31779](<https://bugs.freepascal.org/view.php?id=31779>)
  * r54923 Opkman: Fix high-DPI. [Issue #31782](<https://bugs.freepascal.org/view.php?id=31782>)
  * r54924 ide: about dialog: high-DPI. [Issue #31776](<https://bugs.freepascal.org/view.php?id=31776>)
  * r54925 ide editorfilemanager dialog: high-DPI. [Issue #31775](<https://bugs.freepascal.org/view.php?id=31775>)
  * r54926 lcl: groupededit: fix TabStop mechanism. [Issue #31811](<https://bugs.freepascal.org/view.php?id=31811>)
  * r54927 lcl: control: apply constraints to new size in DoAutoAdjustLayout and update top/left if bottom/right aligned. Fixes [Issue #31772](<https://bugs.freepascal.org/view.php?id=31772>)
  * r54928 ideintf: objectinspector: fix high-dpi in custom property grids. [Issue #31769](<https://bugs.freepascal.org/view.php?id=31769>)
  * r54929 lazcontrols: lvlgraphctrl: high-DPI. [Issue #31781](<https://bugs.freepascal.org/view.php?id=31781>)
  * r54930 lcl: groupededit: fix TabStop mechanism (fix regression after r54926). [Issue #31811](<https://bugs.freepascal.org/view.php?id=31811>), [Issue #31229](<https://bugs.freepascal.org/view.php?id=31229>)
  * r54932 ide, designer: high-DPI: scale non-visual components. [Issue #31812](<https://bugs.freepascal.org/view.php?id=31812>)
  * r54934 ide: images: new high-dpi designed icons by FTurtle, [Issue #31815](<https://bugs.freepascal.org/view.php?id=31815>)
  * r54938, r54944 Translations: Ukrainian translation updates by Olexandr Pylypchuk
  * r54941 Qt5: cbindings - implemented QMargins, QSurface, QSurfaceFormat, QBackingStore, QScreen and QWindow classes.
  * r54942 Qt5: cbindings - implemented QMargins, QSurface, QSurfaceFormat, QBackingStore, QScreen and QWindow classes (qt56.pas).
  * r54951 lcl: fixed TDbLookupCombobox showing blank items when used with a TSdfDat...
  * r54952 codetools: fixed compile with fpc 2.6.4
  * r54958 Gtk2: fixed shrinking designer form, bug 31832, patch Kostas Michalopoulos. Workaround for [Issue #31832](<https://bugs.freepascal.org/view.php?id=31832>)
  * r54968 updatemakefiles: check if fpcmake supports new version
  * r54972 cody: fixed typo
  * r54984 lcl: TMenu: raise exception if something else is freeing the FItems
  * r54986 projectgroups: set menuitems to nil on destroy
  * r54988 IDEIntf: TIDEMenuItem: fixed not freeing MenuItems of TMenu
  * r54990 projectgroups: fixed memleak
  * r54992 doceditor: fixed permissions
  * r54994 docs: removed sourceforge commercial footer, privacy breach
  * r54996 debian: fixed manpage-not-compressed-with-max-compression error
  * r54999 debian: fixed permissions
  * r55000 debian: fix group writable permissions
  * r55002 debian: disable debian/rules, not used by our deb
  * r55004 debian: fixed permissions and description
  * r55006 debian: fixed lintian warning about changelog
  * r55008 debian: lazarus deb now builds without lintian errors
  * r55010 debian: fpc-src no lintian errors
  * r55012 deb: fpc: no lintian errors
  * r54697 Gtk2: fixed triggering selection change in TCheckListBox. [Issue #0031697](<https://bugs.freepascal.org/view.php?id=0031697>)
  * r54946 ide: rewrite (simplify) High-DPI icon scaling + support _150 and _200 variants for component images
  * r54957 MenuEditor: Win32: If IDE is closed a AV is thrown. [Issue #0031791](<https://bugs.freepascal.org/view.php?id=0031791>)
  * r54960 MenuEditor: GTK2: AV on closing IDE with opened menu editor. [Issue #0031816](<https://bugs.freepascal.org/view.php?id=0031816>)
  * r55017 ide: images: update debug icons (by FTurtle). [Issue #31830](<https://bugs.freepascal.org/view.php?id=31830>)
  * r55027 IDE: High-DPI: Search Result Window
  * r55032 IDE: High-DPI: Move loading of glyphs to ideintf to multiple method use + cleanup.
  * r55033 IDE: High-DPI: TabOrderDialog
  * r55034 IDE: High-DPI: MenuEditor Images. [Issue #0031877](<https://bugs.freepascal.org/view.php?id=0031877>)
  * r55035 IDE: High-DPI: All IDE SpeedButtons DPI-aware
  * r54953 IdeIntf: OI: always write values to objects if explicitely wanted.
  * r54954 IdeIntf: propedits: always write property values to objects. The property setter method has to decide about the need of update, not the propedit code.
  * r54955 IdeIntf: propedits: fix highlighting default (=not stored) properties.
  * r54981 ideintf: fix regression in image scaling code.
  * r55015 IDEIntf: Restore the backwards compatible event TPropHookModified. Add a new event TPropHookModifiedWithName.
  * r55038 ExternHelp: fix uses clause after r55035
  * r55039 IdeIntf: TIDEImages: switch parameters for CreateImage, LoadImage and GetImageIndex methods.
  * r55016 win32: menus: fix menu height calculation. [Issue #31875](<https://bugs.freepascal.org/view.php?id=31875>)
  * r55018 win32: menus: fix FPC 3.0.0 compilation. [Issue #31875](<https://bugs.freepascal.org/view.php?id=31875>)
  * r55019 win32: menus: fix menu height calculation. [Issue #31875](<https://bugs.freepascal.org/view.php?id=31875>)
  * r54980 Qt5: cbindings: implemented QObject::metaObject(), now we can extract complete classinfo from any Qt class.
  * r55041 Qt5: fixed build with QT_NATIVE_DIALOGS define.
  * r55043 qt, qt5: fix uninitialized result in TQtComboBox.getText
  * r54948 lcl: grids: high-DPI: rewrite ColWidth&RowHeight scaling so that it's Delphi and Lazarus 1.6 compatible.
  * r54949 lcl: grids: add (forgotten) *IsStored functions to DefaultColWidth and DefaultRowHeight properties. (related revision: r54948)
  * r54956 lcl: grids: keep *IsStored functions to DefaultColWidth and DefaultRowHeight properties protected-only (related revision: r54949)
  * r54964 lcl: label: fix CalcFittingFontHeight. Patch by Luca Olivetti
  * r54970 lcl: grids: add Options2 = [goScrollToLastCol, goScrollToLastRow]. [Issue #31766](<https://bugs.freepascal.org/view.php?id=31766>)
  * r55020 lcl: treeview: High-DPI: fix scaling of non-themed node icons. [Issue #31829](<https://bugs.freepascal.org/view.php?id=31829>)
  * r55022 LCL: rename TControl.ScaleCoord* methods
  * r55026 LCL: TListView: Fixed access violation switching OwnerData. [Issue #0031880](<https://bugs.freepascal.org/view.php?id=0031880>)
  * r55044 IDE: improved i18n in messages window, fixed some typos
  * r55056 IDE: regenerated translations and updated Russian translation
  * r54935 IDE: clean up
  * r55021 IDE: change DataModulePPI to FPC 3.0.3 check. [Issue #31813](<https://bugs.freepascal.org/view.php?id=31813>)
  * r54791 Gtk2: implemented LCLIntf.ShowWindow(SW_RESTORE).Patch by AlexeyT. [Issue #0031740](<https://bugs.freepascal.org/view.php?id=0031740>)
  * r54975 Gtk2: removed workaround for SW_SHOWNORMAL, added checks for SW_RESTORE and SW_SHOWMAXIMIZED. [Issue #0031832](<https://bugs.freepascal.org/view.php?id=0031832>)
  * r55058 LCL: Disconnect OnIdle handler of FilterEdit when the control is destroyed. [Issue #31893](<https://bugs.freepascal.org/view.php?id=31893>).
  * r55059 TodoList: Don't try to update the list too early. Regression caused by [Issue #31006](<https://bugs.freepascal.org/view.php?id=31006>).
  * r55060 gtk3: fixed device context offset. Patch by Anton Kavalenka. [Issue #31780](<https://bugs.freepascal.org/view.php?id=31780>).
  * r55064 MenuEditor: High-DPI: ShadowMenu. [Issue #31877](<https://bugs.freepascal.org/view.php?id=31877>)
  * r55070 MenuEditor: Showing "Add Submenu" button at the right position. Fixing breaking commit r54863.
  * r55075 MenuEditor: High-DPI: Arrow and Cleanup. [Issue #31877](<https://bugs.freepascal.org/view.php?id=31877>)
  * r55081 MenuEditor: Simplify and removed optic bug for icon choose dialog.
  * r55071 IDEIntf: Fix a stupid copy/paste error in r55015.
  * r55078 IDE: Improve adding a command and separator in Toolbar Config dialog. [Issue #31890](<https://bugs.freepascal.org/view.php?id=31890>).
  * r55083 AnchorDocking: Revert revision 54525 as it breaks other reloading of AnchorDocking layouts. [Issue #31834](<https://bugs.freepascal.org/view.php?id=31834>)
  * r55084, r55148 Translations: French translation updates by Gilles Vasseur
  * r55093 codetools: fixed EnableCodeCompleteTemplates
  * r55096 IDE: package editor: publish: disable if units/includes outside pkg dir
  * r55137 Translations: Japanese translation update by Haruyuki Fujimaki, bugs 31929, 31930, 31931
  * r55113 LCL: Implement TControl.AddHandlerOnMouseWheel/RemoveHandlerOnMouseWheel. Part of [Issue #0031868](<https://bugs.freepascal.org/view.php?id=0031868>).
  * r55114 TUpDown: Handle MouseWheel of Associate. [Issue #0031868](<https://bugs.freepascal.org/view.php?id=0031868>).
  * r55118 LCL: TTreeView: Win32 Fixed warning TResourceCacheItem.IncreaseRefCount 1000 TPenHandleCache. [Issue #30661](<https://bugs.freepascal.org/view.php?id=30661>)
  * r55122 LCL: TTreeView: Fix non-standard scrolling behaviour on selecting in multiline treeview. [Issue #31681](<https://bugs.freepascal.org/view.php?id=31681>). Patch by Eric Heijnen.
  * r55109 LCL: TToolBar: remove in 1.7 introduced incompatiblity and use real values for ButtonHeight, ButtonWidth, ButtonDropWidth.
  * r55110 LCL: TTreeView: remove in 1.7 introduced incompatiblity and use real values for ExpandSignSize and Indent.
  * r55111 LCL: TTreeView: replace wrong property default for DefaultItemHeight with stored function
  * r55112 LCL: TTreeView: fix signs for r55110
  * r55126 LCL: listbox: fix default value for Options
  * r55116 IDE: Add some 200% component palette images (TDrawgrid, TEdit, TLabel, TStringGrid, TValueListEditor)
  * r55120 IDE: some more 200% component palette images (combobox, colorbox, button, bitbtn, colorbutton, labelededit, statictext, scrollbar, tabcontrol, pagecontrol).
  * r55123 IDE: Add component palette icons at 150% and 200% magnification (standard page complete)
  * r55124 IDE: Add palette icons for TButtonPanel, TSpeedButton, TShape at 150% and 200% magnification
  * r55128 IDE: Add more Mid/HiRes component palette icons (groupbox etc, updown, arrow, headercontrol, editbutton, (float)spinedit, colorlistbox)
  * r55132 IDE: Add component palette icons of all data-aware controls at 150% and 200% magnification.
  * r55134 IDE: IDE: Add component palette icons of htmlhelpdatabase and the propstorage components at 150% and 200% magnification.
  * r55135 IDE: Add palette images for TCheckListbox, TListView, TTreeView and TMaskEdit at 150% and 200% magnification
  * r55150 IDE + chmhelp: Add 150% and 200% icons for TXMLConfig, TChmHelpDatabase, TLHelpConnector.
  * r55129 TAChart: Avoid exception in IDE if Extent.Min >= Extent.Max
  * r55145 IDE: SpartaDockedFormEditor: Fixed AV when component from component palette is selected and no form unit is in the editor.
  * r55155 Debugger: Jump properly from call stack to include files. [Issue #31755](<https://bugs.freepascal.org/view.php?id=31755>).
  * r55156 Finnish translation by Seppo Suutarla and myself.
  * r55157 IDE: Add 150% palette images for the LCL dialog components
  * r55158 IDE: Add 150% dialog palette images missing from previous commit.
  * r55159 docs: build_lcl_docs: updated, simulate shows both lazutils and lcl
  * r55161 docs: build_lcl_docs: showcmd shows --fpcdocs option
  * r55163 LCL: Grids: we need VisualChange and UpdateSizes also when handle is not allocated to access correct cache values. [Issue #31955](<https://bugs.freepascal.org/view.php?id=31955>)
  * r55164 LCL: grids: initialize scrollbar visibility info with correct values
  * r55167 docs: fixed xml
  * r55169 docs: build_lcl_docs: --footer
  * r55172 fixed spelling errors, bug 31956, from Paul Gevers
  * r55173 docs: fixed xml
  * r55177 IDE: Add 200% palette images for all dialog components
  * r55178 deb: fpc: install man pages if available
  * r55180 GTK2: Remove trailing NULL only when GTK_REMOVE_CLIPBOARD_NULL is defined. [Issue #21453](<https://bugs.freepascal.org/view.php?id=21453>) and [Issue #31547](<https://bugs.freepascal.org/view.php?id=31547>).
  * r55181 IDE: Add magnified images (150% and 200%) for data access components
  * r55183 IDE: Add magnified (150% and 200%) component palette icons for packages sqlitelaz and sqlite3laz
  * r55185 Fix resource error due to r55183
  * r55201 codetools: fixed TEventsCodeTool.MethodTypeDataToStr for fpc 3.0+
  * r55202 IDE, OI: method propety editor: show inherited method without classname, treader does not support choosing the inherited
  * r55204 codetools: TEventsCodeTool.FindClassOfInstance: search unit in unitpath, the uses section is not enough



## Fixes for 1.8 RC1 (merged)

  * r54585 Qt,Qt5: do not process events after modal form is hidden, otherwise we can have av if formclose contain caFree.
  * r54586 Qt: simplified code.
  * r54588 examples: fixed testall.lpi
  * r54589 Qt,Qt5: do not eat cpu with gtk theme. [Issue #31191](<https://bugs.freepascal.org/view.php?id=31191>)
  * r54593 Qt,Qt5: do not disable DT_WORDBREAK if we are using DT_CALCRECT, otherwise we'll have wrong calculation in combination with DT_NOCLIP.
  * r54594 LazControls: Apply filter also at start in ListFilterEdit and ListviewFilterEdit. [Issue #31632](<https://bugs.freepascal.org/view.php?id=31632>).
  * r54596 translations: German: updates from Swen Heinig
  * r54597 LazControls: Apply filter also at start in TreeFilterEdit. [Issue #31632](<https://bugs.freepascal.org/view.php?id=31632>).
  * r54599 Qt,Qt5: fixed potential memleak with TQtWSDragImageList when bitmap handle is null.
  * r54600 LCL-Win32: Fix range error in ComboBoxWindowProc. Use SetWindowLong from LCLIntf. [Issue #31635](<https://bugs.freepascal.org/view.php?id=31635>).
  * r54618 Lazreport: fix column number [Issue #31544](<https://bugs.freepascal.org/view.php?id=31544>).
  * r54619 LazReport: fix export from LazReport to pdf (fclpdf), from Aleksey Lagunov [Issue #31659](<https://bugs.freepascal.org/view.php?id=31659>)
  * r54624 LCL: TGroupBox: Win32: Fix for doubled caption. [Issue #31670](<https://bugs.freepascal.org/view.php?id=31670>).
  * r54627, r54656, r54657 LCL: TSpinEdit: Win32: Fixed Z-order problem with TSpinEdit. [Issue #27862](<https://bugs.freepascal.org/view.php?id=27862>)
  * r54631 IDE: useunit dialog: fixed adding unit to program uses section
  * r54632 codetools: fixed predefined identifiers delphi, fpc, pas2js
  * r54633 GTK2, GTK3: In DrawText support prefixed UTF-8 codepoints. [Issue #31674](<https://bugs.freepascal.org/view.php?id=31674>).
  * r54638 codetools: delphi predefined identifiers: nativeint, bools
  * r54639 codetools: referenceto: fixed endpos
  * r54641 codetools: keywordfunclists: use fast property Count
  * r54643 codetools: ctnReferencTo for typeinfo, code hints and type check
  * r54648 IDE images: new images menu_view_components, menu_view_inspector, menu_view_messages, menu_view_search_results. Designed by FTurtle, [Issue #31625](<https://bugs.freepascal.org/view.php?id=31625>)
  * r54650 codetools: fixed resolving function result, simplified code
  * r54651 DateTimePicker - OnChange fires twice [Issue #0031679](<https://bugs.freepascal.org/view.php?id=0031679>)
  * r54654 Qt, Qt5: fixed av because parent can be nil. [Issue #31684](<https://bugs.freepascal.org/view.php?id=31684>)
  * r54655 Gtk3: removed gdk_pixbuf_gettext which does not exist in gdk-pixbuf. [Issue #31677](<https://bugs.freepascal.org/view.php?id=31677>)
  * r54660 codetools: added nativeint, nativeuint for pas2js
  * r54666 codetools: added unique ids for bug reports
  * r54667 codetools: fixed recognizing str
  * r54670 codetools: fixed code completion for v in constset
  * r54672 codetools: debug gather unit names
  * r54675 codetools: fixed error msg
  * r54677 IDE: do not check package source directory of a manual package
  * r54680 fpcunit: consolerunner: use lazutils instead of lcl
  * r54683 codetools: linkscanner: unique ids for errors
  * r54685 codetools: sourcechangecache and directiveparser: unique ids for errors
  * r54687 codetools: fixed id on unknown exception
  * r54689 codetools: define template directory: fixed skipping empty directory
  * r54691 cody: identifier dictionary: use identifier: fixed jumping to syntax error
  * r54693 codetools: code completion: add proc for procvar: fixed reference-to
  * r54694 Qt, Qt5: fixed selection with TListView. [Issue #0031595](<https://bugs.freepascal.org/view.php?id=0031595>)
  * r54696 Qt, Qt5: fixed triggering of selection change in TCheckListBox. [Issue #0031697](<https://bugs.freepascal.org/view.php?id=0031697>)
  * r54698 TAChart: Change default value of TFitSeries.Pointer.Visible to false for backward-compatibility
  * r54707 Qt, Qt5: trigger OnSelectionChanged for first click. Part of [Issue #0031697](<https://bugs.freepascal.org/view.php?id=0031697>)
  * r54710 Qt, Qt5: fixed wrong key release event after pressing ENTER in combobox dropdown list. [Issue #0031574](<https://bugs.freepascal.org/view.php?id=0031574>)
  * r54713 codetools: fixed modeswitch name blocks to cblocks
  * r54715 codetools: added $mode ExtPas, load hidden units MacPas, ISO7185, ExtPas, BlockRTL
  * r54718 IDE: fixed compilation, bug 31711
  * r54720 codetools: parse multi dim arrays, patch from Paul W, issue 25045
  * r54722 lcl: forms: don't check for old value in UpdateShowInTaskBar. [Issue #31248](<https://bugs.freepascal.org/view.php?id=31248>), patch by Michl
  * r54724 IDE: identifier completion: do not auto complete single possibility when invoked by timer
  * r54727 codetools: fixed compilation with fpc 2.6.4
  * r54730 codetools: fixed parsing array
  * r54732 lcl: grids: fix endless loop "paint->update scrollbars->paint". [Issue #31518](<https://bugs.freepascal.org/view.php?id=31518>).
  * r54733 lcl: grids: don't invalidate grid when changing bounds if not necessary. [Issue #31518](<https://bugs.freepascal.org/view.php?id=31518>).
  * r54738 codetools: fixed find declaration of dotted unit name identifiers, improved identifier completion for namespaces
  * r54741 codetools: identifier completion: list each namespace only once
  * r54743 Qt5: added QBuffer class, needed for saving and loading streams where QIODevice is needed. eg QImage
  * r54746 codetools: search units with namespaces
  * r54748 IDE: sourceeditor completion box: namespaces
  * r54750 IDE: do not pass -B when option -k or -T changes, added resourcestring
  * r54752 IDE: parse namespaces from custom compiler options
  * r54757 lazutils: added MergeWithDelimiter
  * r54761 IDE: identifier completion: global property types, patch from Anton, bug ...
  * r54764 codetools: fixed compilation with fpc<3.1.1
  * r54765 LCL-GTK2: A workaround to prevent a complete freeze with menu separators. [Issue #31138](<https://bugs.freepascal.org/view.php?id=31138>).
  * r54766 LCL: TTrayIcon: Win32: Fixed SIGSEGV after closing app. [Issue #0029739](<https://bugs.freepascal.org/view.php?id=0029739>)
  * r54768 codetools: basiccodetools: ReadDottedIdentifier, FindModuleNameInSource
  * r54770 codetools: use namespaces only for pas2js and fpc>30101
  * r54771 IDE: regenerated translations and updated Russian translation
  * r54772 IDE: Spanish translation update by Alonso Cárdenas Márquez, bug 31731
  * r54777 Qt, Qt5: set correct timestamp when using X11Raise(), some wm's complains about it.
  * r54778 Qt5: do not use x11 timestamp workaround as qt4 uses. Qt5 uses libxcb which handles timestamps well.
  * r54779 Qt: reverted using unixdatetime instead of x11 CurrentTime const because problem is in buggy wm not in our code.
  * r54781 LCL: TPageControl: Win32: Fixed Pagecontrol does show space for tabs, but no tabs in it. [Issue #0019278](<https://bugs.freepascal.org/view.php?id=0019278>)
  * r54785 Translations: Hungarian translation update by Péter Gábor, bug 31739
  * r54799 ide: images: anchor editor. Designed by FTurtle, [Issue #31730](<https://bugs.freepascal.org/view.php?id=31730>)
  * r54802 Qt,Qt5: fixed regression from r54696. [Issue #31697](<https://bugs.freepascal.org/view.php?id=31697>)
  * r54808 lazutils: fixed lazunicode compile under Delphi
  * r54809 MenuDesigner: Fixed adding a line separator at last position. [Issue #0031744](<https://bugs.freepascal.org/view.php?id=0031744>)
  * r54812 lcl: grids: disable VisualChange and UpdateSizes if AutoSize is disabled. Solves part of [Issue #31715](<https://bugs.freepascal.org/view.php?id=31715>)
  * r54813 lcl: support for DebugDisableAutoSizing compiler define
  * r54815 lcl: wincontrol: ignore FBoundsRealized in WM_SIZE. Solves part of [Issue #31715](<https://bugs.freepascal.org/view.php?id=31715>)
  * r54816 lcl: grids: Hi-DPI: row heights and column width. New default (system) value is -1. Solves part of [Issue #31715](<https://bugs.freepascal.org/view.php?id=31715>)
  * r54817 lcl: grids: ignore WMSIZE when updating scrollbars. Solves part of [Issue #31715](<https://bugs.freepascal.org/view.php?id=31715>)
  * r54822 codetools: fixed guesstype of copy()
  * r54828 codetools: added compiler directive switches $E, $G, $J, $O, $U
  * r54830 rtticontrols: updated example lpi
  * r54832 codetools: renamed cmsBlocks to cmsCBlocks, because less confusing
  * r54833 DbEditMask example: Fix ShortDateFormat, it raised an exception.
  * r54835 codetools: identifier completion: Delphi directives
  * r54837 codetools: parsing modeswitch on,off
  * r54839 codetools: set defines when setting modeswitches
  * r54840 lcl: grids: fix scrolling after r54816, [Issue #31715](<https://bugs.freepascal.org/view.php?id=31715>)
  * r54841 lcl: grids: scroll to highest possible value if col/row are out of valid bounds. Related to [Issue #31766](<https://bugs.freepascal.org/view.php?id=31766>)
  * r54842 lcl: dbgrids: adapt after r54816. [Issue #31765](<https://bugs.freepascal.org/view.php?id=31765>) and [Issue #31715](<https://bugs.freepascal.org/view.php?id=31715>)
  * r54843 ide: packager: adapt after r54816. [Issue #31762](<https://bugs.freepascal.org/view.php?id=31762>) and [Issue #31715](<https://bugs.freepascal.org/view.php?id=31715>)
  * r54844 lcl: grids: make GetDefaultRowHeight and *ColWidth public. [Issue #31762](<https://bugs.freepascal.org/view.php?id=31762>) and [Issue #31715](<https://bugs.freepascal.org/view.php?id=31715>)
  * r54845 LazUtf8: fix compilation for WinCE. [Issue #0031788](<https://bugs.freepascal.org/view.php?id=0031788>).
  * r54848 ide: new icons designed by FTurtle, [Issue #31789](<https://bugs.freepascal.org/view.php?id=31789>)
  * r54848 ide: new icons designed by FTurtle, [Issue #31789](<https://bugs.freepascal.org/view.php?id=31789>)
  * r54850 IDE: menu designer: replaced some free with releasecomponent, controls ma...
  * r54852 IDE: menu designer: remove items via normal means
  * r54854 Example easter: Fix ownerdrawn listbox not painting its background, center listbox items (<http://forum.lazarus.freepascal.org/index.php/topic,36825.msg245809.html#msg245809>)
  * r54856 IDE: fixed switching build mode in options dialog, bug 31787
  * r54858 IDE: menu designer: free shadowmenu via releasecomponent
  * r54860 codetools: added predefined fpc identifier get_frame
  * r54862 IDE: increased Max_Frame_Dump to 32
  * r54864 IDE: menu designer: do not call ProcessMessages
  * r54866 Translations: Ukrainian translation update by Olexandr Pylypchuk, bug 31783
  * r54872 lhelp: Fix lhelp to show popup hints. [Issue #31732](<https://bugs.freepascal.org/view.php?id=31732>).
  * r54874 Menu designer: Call GlobalDesignHook.Modified after adding an item. [Issue #31791](<https://bugs.freepascal.org/view.php?id=31791>).
  * r54875 gridexamples: in title_image_demo show images at start, adapt height of header row to image layout) (<http://forum.lazarus.freepascal.org/index.php/topic,36841.0.html>)
  * r54876 Examples: Fix demo fontenum ignoring some user input (<http://forum.lazarus.freepascal.org/index.php/topic,36839.0.html>)
  * r54877 Examples: Minor improvements of sample project motiongraphics (<http://forum.lazarus.freepascal.org/index.php/topic,36858.msg245986.html>)
  * r54878 Examples: Improved usability of demo "openurltest".
  * r54880 examples: removed obsolete openbrowser
  * r54881 Examples: Fix crash of demo "propstorage" (<http://forum.lazarus.freepascal.org/index.php/topic,36862.msg246019/topicseen.html>).
  * r54882 lcl: grids: make default value for DefRowHeight and DefColWidth 0 and not -1 (due to LCL consistency). [Issue #31715](<https://bugs.freepascal.org/view.php?id=31715>)
  * r54888 docs: update TCustomGrid.DefColWidth, TCustomGrid.DefRowHeight, TToolBar.ButtonHeight, TToolBar.ButtonWidth, TToolBar.DropDownWidth, TCustomTreeView.ExpandSignSize, TCustomTreeView.Indent
  * r54892 examples: fixed pascalstream with FPC 3.0+



## Roadmap to 1.8.0

The release process will consist of the following steps. 

  * Creation of the SVN 1.8 fixes branch. (Done 08 Apr 2017)



This is the start of the stabilization process before a new Lazarus release. 

  * Release of 1.8.0 RC 1 (Done Wed, 17 May 2017 )



This will be the first Release Candidate. We would like to urge all users to test this version. The testing period is scheduled to last 2 weeks. 

  * Release of 1.8.0 RC 2 (Done Tue, 6 Jun 2017)



The testing period is scheduled to last 2 weeks. 

  * Release of 1.8.0 RC 3 (Done Fri, 30 Jun 2017)



The testing period is scheduled to last 2 weeks. 

  * Release of 1.8.0 RC 4 (Done Sun, 13 Aug 2017)



The testing period is scheduled to last 2 weeks. 

  * Release of 1.8.0 RC 5 (Done Fri, 13 Oct 2017)



The testing period is scheduled to last 2 weeks. 

  * Release of Lazarus 1.8 (Done Wed, 06 Dec 2017) 
    * The release of Lazarus 1.8 was delayed 09 months due to the delay in releasing FPC 3.0.4.



Users can download the new stable version. 

### Tests

  * Menu item exists
  * Clear pcp directory and start IDE, a new project application with a form should be visible
  * Double click on form - a FormCreate event should be created
  * Check View / IDE internals / What needs building - no package should need building, only the project
  * Restart the IDE - there should be no warning about upgrading
  * Install the package cody, after restart the component palette should show the component TCodyTreeView



### Tagging release

  * SVN merge requests of this page
  * Set version in fixes_1_8 branch 
    * open lazarus/lazarus.lpi in the IDE and change the version numbers in the project options dialog 1.8.0.1 for RC1, 1.8.0.2 for RC2, 1.8.0.3 for RC3, 1.8.0.4 for final
    * lazarus/ide/version.inc 1.8.0RC1 for RC1, no spaces! (1.8.0 for final release)
    * lazarus/lcl/lclversion.pas 1.8.0.1 for RC1, 1.8.0.2 for RC2, ... 1.8.0.4 for final
    * lclbase.lpk
    * lcl.lpk
    * lazarus/debian/changelog 1.8.0-1 for RC1, 1.8.0-2 for RC2, ... 1.8.0-4 for final
    * lazarus/lazarus.app/Contents/Info.plist 
      * CFBundleShortVersionString "1.8.0"
      * CFBundleVersion 1 for RC1, 2 for RC2, 3 for RC3, 4 for final release
    * tools/install/linux/environmentoptions.xml same as version.inc
    * tools/install/win/environmentoptions.xml same as version.inc
    * tools/install/macos/environmentoptions.xml same as version.inc
    * check lpl files by running "./tools/lplupdate -c" and do the svn mv commands
    * Update makefiles: Make sure **Additions and Overrides** are empty and run


    
    
    FPCDIR=/path/to/fpc/src/trunk PATH=/path/trunk/fpc/utils/fpcm/bin/x86_64-linux/:$PATH ./tools/updatemakefiles
    

  * svn commit
  * Tag fixes_1_8 branch to tags/release_1_8_0_RC1 (or tags/lazarus_1_8_0 for final release)


    
    
    svn copy svn+<ssh://svn.freepascal.org/FPC/svn/lazarus/branches/fixes_1_8> svn+<ssh://svn.freepascal.org/FPC/svn/lazarus/tags/lazarus_1_8_0_RC1> -m 'tagged 1.8.0RC1 as tags/lazarus_1_8_0_RC1'
    

  * Set version to next version in svn
  * Create new section on this page and move waiting merge requests to the new section



## How to merge

See [Lazarus_1.0_fixes_branch#How_to_merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")

## Other branches

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | 1.8 | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch") | [4.0](<Lazarus_4.md> "Lazarus 4.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/Lazarus_1.8_fixes_branch](https://web.archive.org/web/20250317103345/https://wiki.freepascal.org/Lazarus_1.8_fixes_branch)_
