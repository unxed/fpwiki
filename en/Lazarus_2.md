# Lazarus 2.2 fixes branch

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This page is used internally by the Lazarus team.   
The data in this page does not represent the full extent of what is included in the fixes branch/versions 

For a list of what was merged to the **Fixes** branch please visit: 

  * [[List of fixes merged to the fixes branch](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/commits/fixes_2_2>)]



    

    

  * [[List of issues fixed in Version 2.2](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/milestones/84>)]
  * [[List of issues fixed in Version 2.0](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/milestones/2>)]



Open issues listed on the linked milestones are loosely targeted for the indicated version. Their final inclusion depends on the available man-power and/or contributions. Release may or may not be deferred for any non resolved issue. 

## Contents

  * 1 Internal Merge requests for 2.2.8
    * 1.1 Open Merge requests
      * 1.1.1 Submitted by others / waiting for sign off by team member
      * 1.1.2 Submitted by developer / committer and waiting for testing (do not commit yet)
      * 1.1.3 Merge conflicts
      * 1.1.4 Submitted by developer / committer, tested, waiting to be merged
    * 1.2 Processed Merge requests
  * 2 Fixes for 2.2.6 (merged)
  * 3 Fixes for 2.2.4 (merged)
  * 4 Fixes for 2.2.2 (merged)
  * 5 Fixes for 2.2.0 (merged)
  * 6 Fixes for 2.2.0RC2 (merged)
  * 7 Fixes for 2.2.0RC1 (merged)
  * 8 Roadmap to 2.2
    * 8.1 Tests
    * 8.2 Tagging release
  * 9 Other branches



  
This page contains the revisions to be merged from trunk to the [Lazarus 2.2 fixes](<http://svn.freepascal.org/svn/lazarus/branches/fixes_2_2/>) branch. 

These are only fixes made after the branch was created. For other fixes made since the previous release (2.0) see Git and [Lazarus 2.0 fixes branch](<Lazarus_2.md> "Lazarus 2.0 fixes branch")

Release notes can be found [here](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes")

## Internal Merge requests for 2.2.8

### Open Merge requests

#### Submitted by others / waiting for sign off by team member

#### Submitted by developer / committer and waiting for testing (do not commit yet)

  * 57f8a2e341 Docs: LCL/dbgrids. Removes topic TCustomDBGrid.ResetSizes for changes in 6402a449. _— depends on not merged yet revision_
  * 46e8e195 LazUtf8: fix UTF8CompareLatinTextFast to be consistent with Utf8CompareText, when comparing lower ASCII values. [Issue #40014](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40014>)
  * b16ec679 Win32: clear cached data on themechange. Patch by Anton Kavalenka. Resolves [Issue #40085](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40085>).



#### Merge conflicts

  * 5e0cc9032a Documentation updates. _— depends on**d5fe15d448b3b84183c0cf5c399e184ac6aa7091 (Docs: LCL. Updates topics in ShellCtrls.xml.)**_
  * 06a60b60b6 Documentation updates. — depends on 840389dad3b79d180297422d7c208950f4ecdc4e (LCL/dialogs, LazUtils/lazfileutils.).
  * 1f1239bb7a Docs: LazUtils/lazmethodlist. Adds SameMethod and updates CompareMethods topics. Depends on 814e4ba4.
  * 370e42c4e4 Docs: LCL/lclproc. Removes topics deprecated since version 2.1. Depends on 814e4ba4.
  * fc0c7393bb Docs: LCL/interfacebase. Adds missing topic for MemSizeLessThan. Depends on 75aebbee.
  * bdd25fd5e7 Docs: LCL/ShellCtrls. Removes invalid See Also links.
  * 7c2368058c Docs: LazUtils/lazstringutils. Removes topic for deprecated routine in #9a816398. _— #9a816398 is not (and should not be) merged to fixes_2_2._
  * 0bbdfcf556 Docs: LCL/buttons. Adds content for ActionChange and FindDownButton topics in TCustomBitBtn. Depends on 27d3cf84.
  * 70ee915771 Docs: LCL/shellctrls. Adds and removes topics.Depends on 06701d05a7.
  * 9d1e04d4e0 Docs: LCL/shellctrls. Updates content for TShellTreeNode topics. Depends on 93124666, [Issue #39629](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39629>).
  * 593cb01295 Docs: LCL/shellctrls. Updates TCustomShellListView.AdjustColWidths topic for changes in 82b9902b.
  * d68fd2277e Docs: LCL/comctrls/extctrls. Updates topics for code reverted in b02bb356. _— depends on some not merged revision_
  * 061ba75ede Docs: LazControls. Removes footer processing in project file and scripts. _— depends on some not merged revision_
  * 5a3736ac90 Docs: LazUtils/fileutil,lazfileutils,lconvencoding. Fixes XML tagging with extra or missing space characters.
  * ac840480e1 Docs: LCL/various. Fixes XML tagging with extra or missing space characters.



#### Submitted by developer / committer, tested, waiting to be merged

  * 8f00090a IDE: disallow setting floating point properties to +/-Inf or NaN. [Issue #39793](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39793>).
  * 0c9fb431 IDE: fix disallowing to set a components name to "Owner" in IDE. [Issue #40134](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40134>).
  * afb620fe TurboPower_ipro: Add unit IpHtmlTypes to "uses" of TIpHtmlPv (print preview form).



### Processed Merge requests

## Fixes for 2.2.6 (merged)

  * ac784af6 ideintf/ImageListEditor: Less truncation of loaded large images.
  * 2a084c7a IDE: TImageIndexPropertyEditor: fix drawing images for ImageIndex properties that have no default value
  * 78225565 freetype: Fix range-check error when compiled with range-check on.
  * fbe7aa44 LCL/SpeedButton: Fix clipping of caption containing double ampersand characters, [Issue #39820](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39820>)
  * e171cf6f TAChart: Workaround for TChartListbox not detecting clicks on checkbox and series icon correctly on macOS Monterey. See <https://forum.lazarus.freepascal.org/index.php/topic,61074.msg458489.html>.
  * 58f237f0 LazControls: Fix LCLScaling of TColorButton.ButtonColorSize and .BorderWidth.
  * a1f01e19 LCL/Grids: Erase grid col/row drag indicator when no col/row swap is performed. Patch by Joeny Ang, [Issue #39995](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39995>)
  * f40f8d5c SQLDB: Fix generation of incomplete SQL statements by component editor. Modified patch by Don Siders, [Issue #39975](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39975>)
  * ecf49109 SQLDB: Enforce usage of default SynEdit font in GenerateSQLDlg form. Patch by Don Siders, see [Issue #39975](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39975>).
  * 7a73eac6 SQLDB: Fix several layout issues in GenerateSQLDlg. Based on patch by Don Siders, [Issue #40009](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40009>)
  * c67c7456 LazUtf8: honour CaseSensitive in TStringListUTF8Fast.DoCompareText.
  * 1b9e56b5 LCL: fixed TWinControl.GetClientRect wcfBoundsRealized, [Issue #40052](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40052>)
  * 5f76b3e3 LCL: additional fix to TWinControl.GetClientRect wcfBoundsRealized (DockedFormEditor issue), [Issue #40052](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40052>). Patch by rich2014.
  * 49c7830b LCL: Call OnShow event also for a maximized form. [Issue #40043](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40043>).
  * 0e8088ef Win32: fix drawing selection range if TTrackBar.Reversed is True. Resolves [Issue #40080](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40080>).
  * 45e80c9d LCL/Menu: Do not draw mouse-over menu items with effect gdeHighlighted on Windows. [Issue #40092](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40092>).
  * aa477d82 LCL: Fix TLazCanvas to respect DrawingModes (alphablending). [Issue #40100](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40100>)
  * b05d3277 IDEIntf/ImageListEditor: Allow multi-selection for "Replace all resolutions". Immediately update the preview image when images are added/replaced. Patch by Roland Hahn.
  * e8affb6d IDEIntf/ActionsEditor: Improved spacing of items in actionlistbox. Issue #39386. Patch by Roland Hahn.
  * 6cc8ef7446 LCL/FlowPanel: Fix TFlowPanel.AutoSize calculation, excludes hidden controls now. Patch by Henner Drewes, [Issue #40137](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40137>)



## Fixes for 2.2.4 (merged)

  * d3396e9fa0 LCL/Listview: Fix listview jumping to first item when a key is pressed in virtual mode. [Issue #39715](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39715>)
  * 0883c8c524 LCL/ListView: implement incremental search in virtual mode for Windows. [Issue #39748](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39748>)
  * 7ca9ba2fb6 Support more calling conventions. [Issue #39405](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39405>).
  * 59eeb39742 IDE: Support scaled font sizes in All Compiler Options window. By BrunoK.
  * bed8f0d9e9 FPVectorial: Fix missing call to parent constructor of TIndentedStringList. [Issue #39764](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39764>). Patch by ExSystem.
  * e10b896d7e LCL/FloatSpinEdit: Fix KeyPress swallowing the 'e' of exponential notation.
  * 9f508f3c7d ToDoList: Fix caption of first exported column to be "Type" rather than "Done".
  * f5321043f4 Packager: Use pure package name of a dependency when searching online packages.
  * 7568b050 Qt5: fixed crash when calling stretchBlt() on invisible form. [Issue #39780](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39780>).
  * bbcc0215 - Fixes a multiuser bug affecting TrayIcon on Linux
  * 39a13d91bb LCL/Win32: Fix opening popup menu when Application.MainForm does not exist. [Issue #39451](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39451>). Patches by DeerLab and Iret Company.
  * 4b19396ac4 Docs: LCL/comctrls. Fixes typo.
  * 358a6955a0 TAChart: Fix crash in design mode when Undo is executed after deleting a series.
  * d6828c434c Turbopower_ipro: Fix handling of a < br > tag after a table. [Issue #39759](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39759>)
  * 455dc87b839617720bf6afc8fec6a37924560fdb Don't raise an exception when date in the edit is out of bounds. [Issue #39825](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39825>).
  * e9e8f6ef32 LCL/ShellCtrls: Fix ShellTreeView crashing when a file has been deleted by the OS.
  * fd4f2ea2ec Docs: LCL/lcltranslator. Removes extra spaces in topics. Updates tagging for True and False values.
  * 7f0a33ed65 Docs: LCL/lcltranslator. Adds, updates topic content. Depends on d85e87e5.
  * 00f9467f14 Docs: LCL/lcltranslator. Fixes tagging errors.
  * 8abffea77a Grids: capitalize Sender. Patch by Alexey Torgashin. [Issue #39849](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39849>). (Requested for merging to avoid possible merge conflicts in the future)
  * 686b056fcf, d623f68c1d IdeIntf: Prevent bogus entries with a non-contiguous enum in TEnumPropertyEditor. [Issue #39832](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39832>).
  * 024e28702a LCL/Grids: Remove internal function PointIgual(), use built-in TPoint operator <> instead. [Issue #39850](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39850>)
  * b9c4368fce Designer: TScrollingWinControl paints design-time grid only on visible area. Based on work by German forum user "siro". [Issue #39866](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39866>)
  * 43b6a8bac4 LCL/Images: Higher contrast version of cycle-implementation overlay icon in unit dependencies form.
  * 7db50f95cc LCL: Increase distance between TToolButton icon and text in List mode.
  * 6d123c6ac8 LCL/Toolbar: Fix incorrect high-dpi scaling of toolbuttons due to hard-coded constant margins.
  * e21bc4cd01 LCL/DBGrid: Fix memory leak, patch by michalgw, [Issue #39904](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39904>)
  * e7e585f13d17e29f2ea075cf9a302c8f71b65b3d TDateEdit: avoid OnChange being issued in Loaded.
  * 39bf8b7abacf7d1a0be83ac4f98d4abcc3d2f072 TDateEdit: remove unused var from Loaded (leftover from a workaround).
  * 402c6a3c09 Converter: Do not comment out {$R *.res} directive even if the file does not exist. It will be generated. [Issue #39336](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39336>).
  * 8bb0530a24 IDE: Keep suggesting the old suffix of a project main source file when "Saving As". [Issue #39885](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39885>).
  * b442769bb0 LCL/TaskDialog: Fix command and radio buttons on TaskDialog being exchanged in non-Windows widget sets.
  * 95cfa51d TFloatSpinEdit(Ex): fix crash in OI when setting MaxValue/MinValue/Increment. [Issue #39792](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39792>).
  * 05531f10 TFloatSpinEditEx: revert changes made in #95cfa51d, since it refuses to compile on MacOS en Linux 64 bit. //Note: (Only reverses changes in SpinEx, not in Spin unit)



## Fixes for 2.2.2 (merged)

  * 30ba61883f LCL-GTK2: Fix TPopupMenu on 2 monitors aligned vertically. [Issue #39422](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39422>).
  * 71d44be64af4ebc7de222d9107ad3843b9388c63 LazUtf8: use unsigned variables in Utf8LengthFast.
  * c87b73cadc Docs: LazUtils/lazutf8. Updates topic content in UTF8Length, UTF8LengthFast.
  * a57cf215d3 Codetools: Update target processor types for x86_64.
  * f48623e90c Docs: LCL/controls. Updates GetDefaultColor topics for TControl, TControlCanvas.
  * 59fae3d727 Docs: LCL/spin. Corrects content in TCustomFloatSpinEdit.GetLimitedValue.
  * 3b9e6859e7 TAChart: Fix occasional flicker of datapoint crosshair tool. Issue #39548, patch by kupferstecher.
  * b8f2e92f03 TAChart: Fix missing minor ticks when an axis is inverted by means of a linear axis transform. Issue #39546.
  * 5133009b Qt/Qt5/Win32: fixed page size and name (qt) and page orientation mismatch under win32. Patch by Joeny Ang. [Issue #39449](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39449>)
  * 09a4c2fb Qt5: fixed slowness with TQtPrinter under MSWindows. [Issue #39461](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39461>)
  * 53cda894 Qt/Qt5: fixed font changing of TComboBox editor. [Issue #39549](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39549>)
  * 4fa1bfc4 TAChart: Fix TDateTimeIntervalChartSource formatting of dates in "quarter/year" format.
  * ad0cee4c TAChart: Fix position of quarter-year marks in TDateTimeIntervalChartSource
  * 8e85780b TAChart: Fix unit test of IsEquivalent().
  * c5b1c183 TAChart: Add StartOfTheQuarter and IncQuarter functions (date/time-related). Add related unit tests.
  * 334d1265 TAChart: Fix quarter-of-year label positioning in TDateTimeIntervalChartSource based on the new IncQuarter() and StartOfTheQuarter() functions.
  * 565be6fcc7 Docs: LCL/stdctrls. Updates topic content.
  * 3b0b5e67dd IDE: List different processors for i8086 target. [Issue #39565](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39565>).
  * 9eaf4e3531 IDE: Hide IDE windows on "Run without Debugging" when the setting says so. [Issue #39567](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39567>).
  * f7d2d0a690 IDE: Use '-W' parameter for MacOS bundle on "Run without Debugging". Simplify the code. [Issue #39567](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39567>).
  * 6f8172a61a Codetools: Allow keyword const in generic parameter list. [Issue #38087](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/38087>).
  * 4ac1f026c2 IDE: Create new MenuItems for bookmarks. A MenuItem cannot be used for many parent menus. [Issue #39540](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39540>).
  * 6c639074fc Docs: LazUtils/lazutf8. Updates topic content in Utf8EscapeControlChars.
  * b2b3689434 Docs: LCL/controls. Updates topic content and links.
  * bcf2fdd549 Docs: LCL/graphics. Fixes awkward wording in the TPicture topic.
  * 05cf5afbc2 Docs: LCL/extctrls. Updates topic content and links.
  * 58f8d07925 IDE: Disable 'Abort Build' menu item when not building. [Issue #39585](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39585>).
  * dfa66431c4 Codetools: Complete code correctly for "procedure xxx; forward;". [Issue #39510](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39510>).
  * c875cc8f8e Docs: LCL/forms. Fixes error in GetParentForm topic from issue #39591.
  * c111b7caf5 Docs: LazUtils/utf8process. Updates topic content.
  * ed84fe08f1 Docs: LCL/comctrls. Updates content and links for topics.
  * 3ecdcf0320 IDE: Allow multi-selection with Shift in Project Inspector and Package Editor. [Issue #39595](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39595>).
  * c12011e450 Docs: LCL/grids. Removes an extraneous character in a See Also link.
  * 4d72245552 LCL: Solve most performance issues with virtual TListView (OwnerData=True). [Issue #39324](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39324>).
  * 81ac10f738 Images: Fix new ico files to be linux-compatible. [Issue #39596](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39596>).
  * fde1f53879 Images: Replace the default lazarus.ico used anywhere within the installation folder by the new version. [Issue #39596](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39596>).
  * 1c44ec9237 Images: Replace the macOS .icns files to be like the .ico files. Provided by Roland Hahn. [Issue #39550](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39550>).
  * 082d051466 LCL: Do not InvalidateSelected in MultiSelect virtual TListView. Part of [Issue #39324](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39324>).
  * ff3970a6b3 Docs: LCL/comctrls. Updates topics for changes in 4d72245552, 082d051466.
  * 0aada2da4c Images: Replace lazarus*.ico files by versions in which the small images (16x16, 20x20) are a bit more "crispy". Created by Roland Hahn.
  * b6222f0da8 TAChart: TBoxAndWhiskerSeries does not draw box if box image left=right or top=bottom. [Issue #39615](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39615>), merge request by Frank Freeman.
  * c4a17bf926 TAChart: TBoxAndWhiskerSeries cannot use different color on each point for box border, whiskers and median. [Issue #39616](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39616>), marge request by Frank Freeman.
  * 5bade4a47a Docs: LCL/grids. Updates content for TCustomGrid topics.
  * 55c4d7d5af Docs: LazUtils/lazutf8. Fixes spelling errors in topics.
  * 2d93d8f51b LCL: Fix mouse position in unhandled mouse-wheel event to be relative to parent, [Issue #39621](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39621>).
  * a54aa9c1bb Images: Replace icons in images/windows/ by versions adapted to the new style. Provided by Roland Hahn.
  * 1f695b9d3a Tools/chmmaker: Fix compilation with FPC 3.2.x
  * fcb70e903a TAChart: Fix TBoxAndWhiskerSeries crashing when MinYValue or MaxYValue is queried and the series is empty.
  * 89bd5e8943 LCL/SpeedButton: Draw centered multi-line caption [Issue #39632](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39632>).
  * ad41c897b7 LCL/TSpeedButton: Improve text alignment when there is no glyph [Issue #39632](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39632>).
  * 603fb729459c5ca1db91e5cc69397f3c02b19067: T(Float)SpinEdit: update the control in EditingDone so that text matches Value. Reported by Jamie in <https://forum.lazarus.freepascal.org/index.php/topic,57722.msg429596.html#msg429596>
  * 5c44c3b487 Docs: LCL/spin. Adds TCustomFloatSpinEdit.EditingDone topic. Depends on 603fb729.
  * 2b04018e69 LCL/SpeedButton: Fix misaligned icons in TFilenameEdit etc. Related to [Issue #39632](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39632>).
  * fe1125219b Docs: LCL/controls. Updates topic content and links.
  * 86f222952f Docs: LCL/controls. Updates topic content and links. Mostly in TWinControl.
  * 84a85843ed Docs: LCL/popupnotifier. Updates topic content and links.
  * d47c55d93d Docs: LCL/popupnotifier. Fixes spelling errors and typos.
  * 5e581ce962 Docs: build_lcl_docs. Removes use of default footer file name.
  * 2f59545512 Docs: build_lcl_docs. Fixes an incompatibility with FPDoc 3.3.x. Depends on 5e581ce962.
  * b0d263f11d Docs: build_lcl_docs. Sets an explicit title for CHM files used in the LHelp TOC navigation tree. [Issue #28500](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/28500>). Depends on 5e581ce962, 2f59545512.
  * 7cf2b7390c Docs: build_lcl_docs. Fixes missing links from LazUtils to LCL topics in output formats. Depends on 5e581ce9, 2f595455, b0d263f1.
  * f47c1f2ea6 Docs: Fixes spelling, grammar, and file name errors in README.txt. Depends on 2f59545512.
  * 3e672ebe93e QT4: fixed uninitialized Result when getting WheelScrollLines. Merge request !78.
  * 61f292751af QT5: fixed uninitialized Result when getting WheelScrollLines. Merge request !78.
  * c805ff750d Move FreeType license file from LazUtils to FreeType directory. [Issue #39657](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39657>).
  * 7f856147a1 Docs: LazUtils. Comments platform-specific links that cause build warnings.
  * 68d06aa93d Docs: LazUtils/integerlist. Updates topic content.
  * 891501a228 Docs: Removes redundant text in remark tags.
  * 2843627279 Docs: LCL/printers. Updates topic content.
  * 7ef64b00e4 IDE: Improve treatment of a .lpr project file dropped onto the IDE. Ask to open as a file/project. [Issue #39663](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39663>).
  * 1cbba92872 Docs: LCL/comctrls. Updates topics for TListColumn, TListColumns.
  * c6e64db4d2 Docs: LazUtils/fileutil. Updates topic content and links.
  * 4d46c0b812 Docs: LazUtils/lazfileutils. Updates topic content, links, formatting in code tags.
  * fce9b1ebba Docs: LazUtils/lazsysutils/textstrings. Fixes links to external topics.
  * b63949616e Docs: Adds a link to LazUtils on the Online Help page.
  * 1f7f5418a1 Docs: LazUtils/textstrings. Fixes an XML error introduced in the previous commit. Oops.
  * b0f6b29a LCL: Label.ParentColor stored default True. Revert 53362c37. [Issue #38976](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/38976>)
  * 61ad4be039 Tools/chmmaker: Fix crash when editing table-of-contents of loaded sample project.
  * c0c92c1b57 Tools/chmmaker: Try to fix sitemap editor. Still the whole concept is rough, and probably also suffers from changes wrt projectdir, since the sitemaps are saved to a different directory
  * 28c2a3348f Docs: LCL/extctrls. Adds TPanel.OnChangeBounds topic for changes in f9e896f0.
  * c8241f1ebd Docs: LCL/stdctrls. Updates topics for code removed in b0f6b29a.
  * 8e5d00ff3a Docs: LazUtils/fileutil. Updates topic content and links.
  * 79868a41b3 Docs: LazUtils/lclproc. Updates topic content. Removes topics not needed for current source code.
  * 455af077fa Docs: LazUtils/lazloggerbase/lazlogger. Updates topics for routines from include files.
  * c334298026 Docs: RttiControls. Adds missing topics for current source revision.
  * e6f66a8da3 Docs: RttiControls. Adds project file, css, and script needed to produce help formats.
  * 89a9d84d34 Docs: Fixes typo in build script.
  * 089d619fcf Docs: RTTIControl/rttigrids. Adds, updates topic content.
  * 31bb92943f Docs: RttiControls. Updates output file names to match package name.
  * 68a0b6fd59 Docs: RttiControls/rttigrids. Fixes invalid topic link.
  * 8f440b657f Docs: RttiControls/rttictrls. Adds, updates topic content.
  * 93d6e5a16c Docs: LCL/forms. Updates topic content and see also links.
  * 9da44c1c9b Codetools: Do not crash when invoking ident completion on empty editor. [Issue #39685](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39685>).
  * 478e5101d9 LCL/StringGrid: Fix crash when Columns are exchanged and column indices are specified in reverse order.
  * bdd9145187 Docs: LCL/grids. Updated topic content for changes in 478e5101.
  * a9a1c7cceb Docs: LCL/graphics. Updates content and see also links for TCanvas topics. Issue #39701.
  * 34143c995e Docs: LCL/various. Fixes XML tagging with extra or missing space characters.
  * 417ff31e84 LCL/ImageList: Fix incorrect png format check in lfm files. [Issue #39719](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39719>), patch by Guilherme Scaglia.
  * fcf1fdcc Qt5: fixed Qt5 build missing qt5 subdirectory. [Issue #39723](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39723>), patch by PragmaticLinux.
  * 1a6aea0daa TAChart: Fix designtime crash when Chart is unlinked from ChartLiveView; add missing Notification method.
  * 69c1ebca24 TAChart: Cosmetic in TAChartListbox.pas



## Fixes for 2.2.0 (merged)

  * 34d7072ca1 Docs: LazUtils/masks. Removes deprecation warnings after #896764a7.
  * b18bf9be0c Docs: LazUtils/lazstringutils. Updates ShortDotsLine topic for deprecated routine.
  * 937b6bdfef Codetools: Ignore empty () in function description. Generate function body correctly. [Issue #39441](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39441>)
  * 46a1f547f0 IdeIntf: Rebuild the OI component tree after comp deletion. CollectionItems do not get deleted otherwise.
  * c8bfe6c6cd LazUtils/uitypes.. Comments see also links in the #rtl.system.uitypes namespace.
  * 0d20c53cd4 LCL-GTK3: Prevent crash when Open/SaveDialog is closed. Remove 'destroy' signal handler. Still no memory leaks.
  * da5bd4801c LCL-GTK3: Prevent crash in SaveDialog creation. For some reason virtual class functions confused it.
  * adafca05c0 lcl: fixed TSpeedButton with Action and changing Action.ImageIndex
  * 5aee9d5c07 lcl: fixed TBitButton with Action and changing Action.ImageIndex
  * 123fe0c949 Docs: LCL/buttons. Adds content for TCustomSpeedButton.ActionChange. Depends on adafca05.
  * d43d4618 Qt,Qt5: fixed OnCloseUp event for TDBCustomComboBox. [Issue #39455](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39455>)
  * 79e6efe7eb LCL/controls. Updates Hint-related topics in TControl.
  * bbd1e41311 Docs: LCL/forms. Adds missing topics or content.
  * f642074957 LCL/grids. Fixes spelling error.
  * f506e095e4 Docs: LCL. Removes duplicated content and links to updated original.
  * c0540bb8b0 Docs: LazUtils/lazloggerbase. Comments see also links to unit properties flagged as build warnings.
  * 9cfc817173 Docs: LCL/controls. Adds or updates topic content.
  * 6bcaef208f Docs: LCL, LazUtils. Updates topics for changes in 53fe133e.
  * 805c9e3169 IDE: Open also projects and packages when their names are drag/dropped, or passed as cmd line for a running instance. [Issue #39338](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39338>).
  * 641c3a73f0 Designer: Name Menu separators as "SeparatorX" to make name clashes less likely. INCLUDES A BUG FIX. [Issue #39471](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39471>).
  * 4b12f85ede LCL: Publish TDBEdit.TextHint. [Issue #39470](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39470>).
  * dda96a3864 ProjectTemplates: Prevent wrongly replacing "project" in .lpi file. [Issue #39467](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39467>).
  * 6708a746c8 FontEnum example: Add a missing alphabet. [Issue #39480](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39480>).
  * 3abc423cba, 163a3a2018 Docs: LCL/dialogs. Updates content for the DefaultPromptDialog routine.
  * 229162c203 ProjectGroups: Call ActiveChanged() when compiling all. Updates an indicator in the GUI. [Issue #32475](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/32475>).
  * cbc97e5a03 IDE: Update ComponentList after component visibility in Palette changed. [Issue #23541](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/23541>).
  * b1e6affa83 Education package: Fix compilation after cbc97e5a03.
  * e17301cab0 IDE: Disable Build-buttons in Configure Build Lazarus dlg when the IDE already compiles. [Issue #26884](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/26884>).
  * 182cc8b8e6 LazControls/TSpinEditExBase: Add virtual method SameValue() to fix compilation issue on Win/x64 for a currency descendant. [Issue #39490](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39490>)
  * 7f45938388 LazControls/TSpinEditExBase: Fix compilation issue with FPC 3.2.0. [Issue #39491](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39491>).
  * 5523c58bc9 LazControls/TSpinEditExBase: IFDEF the previous commit (workaround for an already solved FPC bug). [Issue #39490](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39490>), [Issue #39491](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39491>).
  * 171f18dd75 LazControls/SpinEditExBase: Clearer defines for issue #39490 workaround. [Issue #39490](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39490>), [Issue #39491](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39491>)
  * 0e5fdc884a Docs: LazControls/spinex. Adds new topic for TSpinEditExBase.SameValue. Depends on 182cc8b8.
  * c301731931 Docs: LCL/forms. Adds and updates topic content. Depends on d13c67e6, 89162e9c.
  * 05382f4ddf IDE: Show Frame's contents in designer also when its component package is installed in palette. [Issue #39488](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39488>).
  * 15e62fda4d Docs: LazControls/spinex. Adds topics for overloaded SameValue() methods in TCustomFloatSpinEditEx and TCustomSpinEditEx. Depends on 182cc8b8e6, 7f45938388, 5523c58bc9 0e5fdc884a, c301731931.
  * 28984a7cdf IDE: Revert experimental exceptions.
  * aaee8f2881 Docs: LCL/forms. Updates topic content for TCustomFrame and TFrame.
  * 2b2368cb83 IDE: Use radiobuttons in project buildmode menu (rather than checkboxes). [Issue #39497](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39497>), patch by Alexey Torgashin
  * 06d44e3815 Docs: LazControls. Updates topic content and See Also links.
  * 4020cb96fb IDE: Remove an assertion in TMessagesCtrl. Triggered rarely by timing issues. [Issue #39503](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39503>).
  * a821a43b5f IDE: In message header, move to last line of previous view with Up-key. Part of [Issue #39506](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39506>).
  * ab4e2af841 IDE: Move to actual message header when the header hint on top is clicked. [Issue #39506](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39506>).
  * 4eff151060 IDE: Improve the header hint clicked behavior. [Issue #39506](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39506>).
  * c96a14fd0d Docs: LCL/controls. Updates content for Align and Anchors topics in TControl.
  * 05765da982ab46e2486c6932648c71fd692a54d5 LazUtils: LConvEncoding: Fix not setting target codepage in various xxxToUtf8() functions. [Issue #39348](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39348>).
  * 5aa4ccaa470317595eec2e77927436c5a5e9a07a: LazUtils: LConvEncoding: remove all code that is disabled if FPC_HAS_CPSTRING is NOT defined,
  * 01e627e35da5030ccea4406cffbdf5b67b85b0b9 LazUtils: LConvEncoding: change signature of TConvertEncodingFunction analogue to all XXXToUtf8() functions. Part of [Issue #39348](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39348>).
  * 153a562187177282f98ffbb41f259525eac4ddb0 LazUtf8: fix rangecheck error due to not detecting invalid 4-byte Utf8-codepoint in ConvertUTF8ToUTF16. [Issue #39501](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39501>).
  * b12267c1e3 Docs: LazUtils/lconvencoding. Adds topic for SetTargetCodePage parameter in TConvertEncodingFunction. Depends on 01e627e3.
  * d116a8f5cc Docs: LCL/extctrls. Updates topics for TCustomTrayIcon, TTrayIcon.
  * 285ed6ea0d LCL/TColorButton: Add missing Notification method to prevent crash when assigned ColorDialog is deleted, [Issue #39518](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39518>).
  * 7114800c05 LCL/TCustomGrid, TCustomCheckBoxEx: Add missing Notification method to react on destruction of TitleImageList and Images, respectively.
  * cb9d685f79d5fd44f8e74d9075aaaa4143cb3242 LConvEncoding: reverts #05765da9 and #01e627e3 and resolves [Issue #39348](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39348>) in a different way (by always setting the target's codepage when converting to UTF8).
  * d674025f4f Docs: LCL/colorbox. Adds Notification topics in TCustomColorBox, TCustomColorListBox. Depends on 6efaae3d.
  * cbaf46012c Docs: LCL/comboex. Adds topic for TCustomComboBoxEx.Notification. Depends on 7114800c.
  * d93d5c3e08 Docs: LCL/grids. Adds topic for TCustomGrid.Notification. Depends on 7114800c.
  * 9278af1408 LCL: Allow external ColorDialog for custom operations in TColorBox and TColorListBox. [Issue #39189](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39189>), patch by jamie philbrook.
  * 6efaae3d24 LCL/TColorBox, TColorListbox: Add missing Notification procedure. Issue #39518.
  * 08ed2844 Project Templates: Improve configuration of ProjectFile and other variables. [Issue #38244](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/38244>).
  * 734cf1a136 Project Templates: Disable OK button while necessary information is not given.
  * ce4661f5ec IdeIntf/graphpropedits: Scale images in TImageIndexPropertyEditor dropdown to line height. [Issue #35807](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/35807>).
  * f5a0532427 Docs: LCL/dialogs. Adds, updates topics in TColorButton. Depends on 285ed6ea (already merged).
  * c71e7356c4 LazBuild: Default to Cocoa widgetset on 64-bit Darwin. [Issue #39293](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39293>).
  * 3e7c23c53c LazBuild: Default to Carbon widgetset on PowerPC. [Issue #39293](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39293>).
  * 8740a2f1ce Docs: LCL/lclplatformdefs. Updates BuildLCLWidgetType topic for MacOS widgetset usage. Depends on c71e7356c4, 3e7c23c53c.
  * 18bece2476 Docs: LazControls/listviewfilteredit. Removes topic for duplicate type in #e8b4862a.
  * c152361b72 Docs: LCL/controls. Updates topic content and links.
  * 70452d48d7 Docs: LCL/dialogs. Updates topics.
  * b4b11c6f2a Documentation updates.
  * 1b7848191e Docs: LCL/grids. Updates topic content.
  * 7a38231905 Docs: LazUtils/lconvencoding. Updates topics for changes in cb9d685f.
  * 05038a37b1 Docs: LazUtils/fileutil. Changes MacOSX to MacOS in topics.
  * 6719d7b54a Docs: LCL, LazUtils. Changes MacOS to macOS in topics.
  * 6226965c11 Docs: LCL/controls. Adds missing punctuation.
  * d1cd17f10d Docs: LazUtils/fileutil. Fixes capitalization for mac bundle post-fix.



## Fixes for 2.2.0RC2 (merged)

  * r65294 LazReport: Allow entering more than one digit in font combobox editor. [Issue #39026](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39026>), patch by Soner.
  * r65295 LazReport: Change an event parameter name. "Width" clashed with TControl.Width. [Issue #39025](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39025>), patch by Soner.
  * r65383 LazUtils: Fix building when directory is a Windows mount point. [Issue #39120](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39120>).
  * r65388 IdeIntf: Allow multi-line property editors in Dialogs, TTaskDialog. [Issue #39170](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39170>).
  * r65392, 65424, 65438, 65444, 65450, 65462, 65472, 65477, 65481, 65495, 65500 Documentation updates.
  * r65395 TPageControl: QT5: Fixed PageControl.GetTabRect, see project [Issue #39166](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39166>)
  * r65396 TPageControl: QT: Fixed PageControl.GetTabRect, see project [Issue #39166](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39166>)
  * r65432 IDE: add unit DB to uses when TBufDataset or TCSVDataset are added. [Issue #29733](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/29733>).
  * r65441 DockedFormEditor: Fixed wrong ShowHint value when switching from tab Code to tab Form. [Issue #39217](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39217>)
  * r65442 DockedFormEditor: Disable CM_PARENTSHOWHINTCHANGED for all WidgetSets. [Issue #39217](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39217>)
  * r65443 DockedFormEditor: Use correct WidgetSet Win32 instead of Windows
  * r65448 DockedFormEditor: Check for WidgetSet LCLWin32 and LCLWin64. [Issue #39222](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39222>)
  * r65449 LazUtils: Restore function KOI8ToUTF8, earlier renamed as KOI8RUToUTF8, and deprecate it.
  * r65314 Examples/address_book: Fix compilation. Remove hard-coded MS font.
  * r65315 Examples/bidi: Fix layout issue due to overlapping controls.
  * r65316 Examples/dblookup: Fix compilation.
  * r65336 examples: Remove a writeln from checkbox demo.
  * r65337 examples: Fix typo in TestAll demo.
  * r65338 examples/mssql: Fix compilation
  * r65355 LCL/TPageControl: Add new method "Clear" to remove all pages.
  * r65362 docs/lcl: Add documentation for the new method TPageControl.Clear.
  * r65366 docs/lcl: Merge the two TPageControl.Clear doc versions from r65362 and 65358.
  * r65375 LCL/TColorDialog: Show Title property. Issue #39155, based on code submitted by jamie in <https://forum.lazarus.freepascal.org/index.php/topic,55236.msg410713.html#msg410713>
  * r65454 LCL/ShellTreeView: Speed-up of populating a folder with many subfolders (example: c:\Windows\WinSxS)
  * r65455 LCL/ShellTreeView: Speed-up when opening a node with many subfolders a second time.
  * r65376 TAChart: Update xml help.
  * r65379 TAChart: Decode system colors in text elements.
  * r65384 TAChart: Add method GetAxisRange to determine the range of an axis covered by all series assigned to it.
  * r65385 TAChart: Use real axis limits in ChartEditor demo.
  * r65389 TAChart: Rename TChart.GetAxisRange to .GetAllSeriesAxisLimits (more precise).
  * r65391 TAChart/ChartEditor demo: Check that axis maximum is always greater than the minimum.
  * r65393 TAChart/ChartEditor demo: Fix bug in handling of axis.Inverted.
  * r65394 TAChart: Update xml doc.
  * r65420 TAChart/ChartEditor demo: Fix update of axis grid visibility.
  * r65421 TAChart/ChartEditor demo: Fix axis lines not being updated.
  * r65422 TAChart: Always draw axis arrows with solid pen.
  * r65423 TAChart: Fix usage of axistransformations in ChartLiveView having ExtentY = lveAuto.
  * r65425 TAChart: Fix initialization error in TChartLiveView when ExtentY is lveAuto.
  * r65427 TAChart: Fix axis scaling of TChartLiveView with coincident axis limits and ExtentY = lveAuto.
  * r65431 TAChart/ChartEditor demo: add forgotten lfm file.
  * r65440 TAChart/ChartLiveView: Fix autoscaling when multiple axes are used. Add new ExtentY mode, lveMultiAxisRange
  * r65453 TAChart/TChartLiveView: Add some comments. Add xml documentation.
  * r65456 TAChart/TChartLiveView: Simplify operation of ExtentY modes.
  * r65463 TAChart/TChartLiveView: Fix operation when axes have a predefined Range. Modified patch by forum user Muso (<https://forum.lazarus.freepascal.org/index.php/topic,55266.msg412075.html#msg412075>)
  * r65468 LazReport: Fix compilation of ZeosDB
  * r65469 IDE: Fix compiling with FPC trunk. The removed code was only active in fpc trunk 3.1.1 [r36768-r38658)
  * r65470 TAChart/TChartLiveView: Simplification
  * r65471 TAChart/LiveView demo: fix typo.
  * r65473 LCL: Fix TListView selection on Windows with MultiSelect=True and OwnerData=True. [Issue #34877](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/34877>).
  * r65474 LCL: Make ListView.Selected return the first selected item also when OwnerData=True.
  * r65475 FpDebug, Windows: Forward OutputDebugString to IDE.
  * r65479 GdbMiDebugger: fixed reporting OutputDebugString messages
  * r65488 FpDebug: remove double line break from formatted watches.
  * r65489 Debugger: Remove linefeeds for locals dialog. Issue #0039212
  * r65490 Add DbgS for FormStyle.
  * r65491 AnchorDocking: Don't set FormStyle to fsStayOnTop for modal shown windows to prevent showing it in front of other applications. E.G. in IDE Tools -> Desktops.
  * r65496 TAChart: Fix default value of TChartTitle.Brush.Color
  * r65499 TAChart: Add methods StoreAxisRange() and RestoreAxisRange() to LiveView component.
  * 3e9e3ecc6c, 4d4485f209, c7508cbbf3, 543b3ba2af, bb47f65547, 145bf90ea6, 9d48c0eff5, 862cedaca9, 03b41f9ef2, bc7d6df12a, 172b6d5464, ac39e408c7, 5db1922e37, 0467d7cf0f, fb27fcd12a Documentation updates.
  * 3313ccc17c Adds and updates grids, dbgrids topics for merges to fixes_2_2.
  * 10c474f730 Documentation updates.
  * ce63f619 Qt,Qt5: Fixed slowness with TListBox.[Issue #39311](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39311>)
  * ~~5466d422 Qt,Qt5: Fixed checkboxes states after sorting in TListView.[Issue #38137](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/38137>)~~ Reverted since it cannot be compiled in fixes_2_2 !
  * fcf36153 Qt,Qt5: fixed changing TMenuItem.ImageIndex inside main menu. [Issue #37608](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/37608>)
  * 31c207d9ed, 59b2adcee3, f8fae99933, 510899de02 Documentation updates.
  * 5ae19e4e88, acf31c45b6, 4ff5c00578, 64fdfecf46 Documentation updates.
  * a84e602a Qt5: Additional theme drawing fix for issue [Issue #38356](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/38356>)
  * 63ca7a9a Qt5: Fixed AV when changing TListView.ViewStyle in design time. [Issue #39353](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39353>)
  * 2473e77422 Documentation updates.
  * e1be3c5608ee03f5c IDE: Update DefineTemplates after switching build mode. Issue #39347
  * 655bd2ba0d, 6c8e9f0867, fc99cfa6ee, 19b127a2b1, 0f5ed91814 Documentation updates.
  * 515da224 Qt5: fixed doubled menu items in IDE. [Issue #39364](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39364>)
  * fc685470 Qt5: implemented TCustomMemo.setCaretPos(). Patch by Joeny Ang. [Issue #39373](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39373>)
  * a7ee5a544d, 3b9cfa9943, 34bef0cbf9, 6a29f96cec Documentation updates.
  * 6eca378f9d101199cc429c67420ee59739f72274 TListView: call OnDeletion also when destroying. [Issue #39391](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39391>).
  * 5a1abf733a7 LazUtils: Check for Pascal keywords in LazIsValidIdent. Must be prepended with '&'. [Merge Request !11](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/merge_requests/11>) by Abdullahi Usman.
  * f6b7ba26622 Designer: Prevent an illegal name for a component when "Ask name on create" is enabled. [Issue #39292](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39292>).
  * 92c69567 , 059c77a6 , 86a3ea83 Documentation updates.
  * 45a590ed0b9b6 Revert partly "Add HookGetCheckboxForBoolean handler in OI only once." [Issue #39378](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39378>)
  * b7ab9549a64 IdeIntf: Improve recursion in TComponentTreeView.FindAndChange. Call RebuildCollection only once per deleted comp. [Issue #39290](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39290>).
  * 907f8248 IDE: When completing a label, add a colon (':') after it. [Issue #38307](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/38307>).
  * a8a398e553 PascalScript: Fix event TPSOnProcessDirective signature. Active clashed with TForm.Active. [Issue #39359](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39359>).
  * 1d0d348cf6 Documentation updates.
  * de4f724924255 Revert checking for Pascal keywords in LazIsValidIdent which is used also for unit names etc. Prevent illegal name by other means.
  * 1c0f3e699c Docs: LazUtils/lazstringutils. Updates LazIsValidIdent for revert in de4f7249.
  * 824e696005 IDE: Remove Assert() that prevented adding source files with errors in them. [Issue #22921](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/22921>).
  * 7cb38d7bc6 IDE: Check Filename<>UnitName mismatch only when Codetools was able to parse the source. [Issue #22921](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/22921>).
  * ee4324bbe5 Documentation updates.
  * b379bae7 OpkMan: fix readability of hint window on dark themes, remove hardcoded colors...
  * 24ababf578 Docs: LCL. Updates topics to reflect code changes in 865013ee.
  * f120539d03 LCL-GTK3: Show applications better under KWin. [Issue #39367](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39367>).
  * 555a0bb5fc CodeTools: Support case-insensitive {$Align X} etc. directives. [Issue #39339](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39339>).
  * 736e71078e Docs: LCL/filectrl. Updates MinimizeName to specify that the width argument is pixels and not characters.
  * a669e084 TComboBox: update ItemIndex in RealSetText also if Text is empty string. [Issue #39366](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39366>).
  * 2e98d119 T(Float)SpinEditEx: set Modified to False if Value is set programmatically.
  * abd3cd5c T(Float)SpinEdit: set Modified to False if Value is set programmatically.
  * e868afe1 T(Float)SpinEditEx: set make sure Modified is correct in OnChange event as well.
  * 16c42cea T(Float)SpinEditEx: forgotten to commit file in e868afe1.
  * 2befd9dd T(Float)SpinEdit: make Modified have the correct value in OnChange as well.
  * f77ea526e4 Docs: LCL/stdctrls. Updates topics for issue #39413. Depends on a669e084.
  * 335ac8b724 Docs: LazControls/spinex. Updates topics for recent source code changes.
  * cc0c03af28 Docs: LCL/spin. Adds and updates topics for changes in abd3cd5c, 2befd9dd.
  * 0ca9443df0 Update create_lazarus_export_tgz.sh
  * d753efec99 Update create_fpc_rpm.sh
  * e8317fb5 Qt,Qt5: fixed problem with drawing primitives when antialiasing enabled. [Issue #39416](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39416>)
  * 8c248da41d IDE: Disable Project Options command while compiling many build modes. [Issue #39332](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39332>).
  * b6a4282835 IDE: Execute a shortcut command only when it is enabled.
  * 29fa1e50 Qt,Qt5: fixed maximized window with borderstyle = bsnone. [Issue #39158](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39158>)
  * 26930d85 Qt,Qt5: fixed antialias drawing of ellipse and polygon. [Issue #35568](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/35568>), part of [Issue #39416](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39416>)
  * e42dfcff Qt5: fixed typo.
  * e67c74ed7e IDE: Disable Project Options toolbuttons while compiling many build modes. [Issue #39332](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39332>).
  * 4204b2bd DockedFormEditor: Remove invalid widgetset LCLWin64
  * 6439e19d40 JEDI Code Formatter: Support a list of used registers at the end of "asm" block. [Issue #35722](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/35722>).
  * e027061cb5 Docs: LCL/controls. Adds short descriptions for various topics.
  * c822e578 Qt,Qt5: fixed radialPie paint with antialiasing. [Issue #39416](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39416>)
  * 52f2a8b8 Qt,Qt5: Fixed menu radio items. [Issue #37741](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/37741>)
  * 4076f4ef Qt,Qt5: fixed palette change at runtime. [Issue #38589](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/38589>)
  * a058e53864 Docs: LCL/menus. Adds missing topic content.
  * 7fa32a1e Qt5: optimized palette reload instead of calling recreateWnd(). [Issue #38589](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/38589>)
  * b83a9fd9 Qt5: implemented T(Float)SpinEdit.EditorEnabled. [Issue #38736](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/38736>)
  * 9233efad Qt: optimized palette reload instead of recreating handles. [Issue #38589](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/38589>)
  * e5ed5082d5 Docs: LazControls/checkboxthemed. Updates TCustomCheckBoxThemed.State to reflect actions in the property setter.
  * d21ebb1b Qt,Qt5: removed old workaround for Qt < 4.8.7, fixes TApplication activate/deactivate events. [Issue #37868](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/37868>) [Issue #29294](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/29294>)
  * f1156f55 Qt,Qt5: ReleaseCapture when app is deactivated.
  * 5081a526 Qt,Qt5: restored the way how we send application activate/deactivate to LCL, removed calling of RestoreStayOnTop windows which cause confusion.
  * fea136a894 Docs: LazControls/listviewfilteredit. Removes extra short description in module topic.
  * 99e4bd2515 LCL-GTK2: Use a new SysTray API by default. The old one fails to work in new Ubuntu versions + derivatives.
  * 14304e8180 LCL: Fix typos in virtual key constants. [Issue #39434](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/39434>)
  * 622a7619d0 Docs: LCL/lcltype. Adds missing topics. Removes topics no longer in source. Updates topic content.
  * 9057aece Qt,Qt5: fixed system mouse lock when TCustomControl is double clicked. [Issue #27384](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/27384>)
  * 7f0e0cd0 Qt,Qt5: fixed broken TTrayIcon popup menu after recreating popup menu handle. [Issue #38849](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/38849>)
  * 64ccfb3fd2 Docs: Updates module topics for LCL and LazUtils to include both name and abbreviation.
  * e6e2178c LCL/Menus: Fix owner-drawn menu having zero item size if not specified by OnMeasureItem. [Issue #35972](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/35972>), patch by dmz73.



## Fixes for 2.2.0RC1 (merged)

  * r65243, 65249, 65254-65256, 65261-65271, 65274, 65278-65279, 65281, 65290, 65293, 65297, 65300, 65305, 65317, 65325, 65328, 65335, 65339, 65358, 65367 Documentation updates
  * r65248 Qt: removed unsupported QString_toHtmlEscaped. [Issue #38982](<https://bugs.freepascal.org/view.php?id=38982>).
  * r65275 CodeTools: Allow a "var" section inside a record with {$modeswitch advancedrecords}. [Issue #38957](<https://bugs.freepascal.org/view.php?id=38957>).
  * r65303 LCL: Revert revision 63847, fixing FormShow call at wrong time. [Issue #38922](<https://bugs.freepascal.org/view.php?id=38922>)
  * r65304 IDE: Remove EnvironmentOptions.DesignerPaintLazy to prevent paintings outside OnPaint resulting in painting artefacts. Fixes issue #23741 and #33781
  * r65306 IDE: regenerated translations
  * r65311,r65312,r65318 IDE: Implement multi-line selection in Messages window. [Issue #35493](<https://bugs.freepascal.org/view.php?id=35493>)
  * r65319 IDE: Update DefineTemplates (package graph) after adding a component, in case its package was not used earlier.
  * r65320 Fix spelling errors in grids.pas. Patch by Don Siders. Issue #0039093.
  * r65329 Converter: Replace Mask with Masks, not MaskEdit.
  * r65330 IDE: Generate Dwarf3 for Debug build mode and for debug profile for the IDE itself.
  * r65341 IdeIntf: Disable copy/paste of TMenuItem in Object Inspector tree because it does not work for some reason. [Issue #19530](<https://bugs.freepascal.org/view.php?id=19530>).
  * r65342 IDE: Use Dwarf3 debug format for our projects. Needed when debugging with FPDebug.
  * r65348 SQLdb: added pqteventmonitor.pas unit to LPK, bug 39097
  * r65354 IdeIntf: Disable copy/paste of TAction in Object Inspector tree because it does not work for some reason. [Issue #19530](<https://bugs.freepascal.org/view.php?id=19530>).
  * r65372 Translations: Brazilian Portuguese translation update by Marcelo B Paula, bug 39152
  * r65374 Fix spelling errors in lclproc.pas. Patch by Don Siders. [Issue #0039161](<https://bugs.freepascal.org/view.php?id=0039161>).
  * r65378 AnchorDocking DragNDrop of pages not working on GTK2. [Issue #39166](<https://bugs.freepascal.org/view.php?id=39166>)
  * r65412 win installer: EnvironmentOpt.xml: Fix gdb location for windows (new installation)
  * r65413 make component: compile freetype before lazcontrols
  * r65414 lcl: updated Makefile.compiled
  * r65415 lcl: updated Makefile.compiled
  * r65416 lcl: updated Makefile.compiled with freetype
  * r65417 lcl: fix makefile path for freetype
  * r65418 Installer, Win: fix cross build flags
  * r65419 Installer, Win: fix cross build, include package buildintf



## Roadmap to 2.2

The release process will consist of the following steps. 

  * Creation of the SVN 2.2 fixes branch. (Done 15 Jun 2021)



This is the start of the stabilization process before a new Lazarus release. 

  * Release of 2.2.0 RC 1 (Done 12 Jul 2021)



This will be the first Release Candidate. We would like to urge all users to test this version. The testing period is scheduled to last 2 weeks. 

  * Release of 2.2.0 RC 2 (Done 4 Nov 2021)



The testing period is scheduled to last 2 weeks. 

  * Release of 2.2.0 RC 3 (No RC 3 was released)



The testing period is scheduled to last 2 weeks. 

  * Release of Lazarus 2.2 (Done 5 Jan 2022)



Users can download the new stable version. 

  


### Tests

  * Menu item exists
  * Clear pcp directory and start IDE, a new project application with a form should be visible
  * Double click on form - a FormCreate event should be created
  * Check View / IDE internals / What needs building - no package should need building, only the project
  * Restart the IDE - there should be no warning about upgrading
  * Install the package cody, after restart the component palette should show the component TCodyTreeView
  * Run Unit-Tests from menu "Tools"--> "Example Projects". Choose project "runtestsgui.lpi" and run it. It will do about 390 unittests.



### Tagging release

  * GIT merge requests of this page
  * For a RC tag in gitlab before increasing the version in the sources 'tagged 2.2.0RC2 as lazarus_2_2_0_RC2'
  * set version in fixes_2_2 branch: 
    * open lazarus/lazarus.lpi in the IDE and change the version numbers in the project options dialog 2.2.0.1 for RC1, 2.2.0.2 for RC2, 2.2.0.3 for RC3, 2.2.0.4 for final
    * lazarus/ide/version.inc 2.2.0RC1 for RC1, no spaces! (2.2.0 for final release)
    * lazarus/components/lazutils/lazversion.pas 2.2.0.1 for RC1, 2.2.0.2 for RC2, ... 2.2.0.4 for final
    * lclbase.lpk
    * lcl.lpk
    * lazarus/lazarus.app/Contents/Info.plist 
      * CFBundleShortVersionString "2.2.0"
      * CFBundleVersion 1 for RC1, 2 for RC2, 3 for RC3, 4 for final release
    * tools/install/linux/environmentoptions.xml same as version.inc
    * tools/install/win/environmentoptions.xml same as version.inc
    * tools/install/macos/environmentoptions.xml same as version.inc
    * check lpl files by running "./tools/lplupdate -c" and do the git mv commands
    * Update makefiles: Make sure **Additions and Overrides** are empty and run


    
    
    FPCDIR=/path/to/fpc/src/trunk PATH=/path/trunk/fpc/utils/fpcm/bin/x86_64-linux/:$PATH ./tools/updatemakefiles
    

  * check that ide/lazarus.lpi and ide/lazarus.res have changed:


    
    
    git status
    

  * git add .
  * git commit -m 'set fixes version to 2.2.0RC1'
  * git push origin
  * Tag fixes_2_2 branch in gitlab to lazarus_2_2_0_RC1 (or lazarus_2_2_0 for final release) 
    * <https://gitlab.com/freepascal.org/lazarus/lazarus/-/tags>
    * Use "create from" fixes_2_2
  * For a non RC release: 
    * Set version to next version in git main
  * Create new section on this page and move waiting merge requests to the new section
  * upload packages
  * update checksums: <https://gitlab.com/freepascal.org/lazarus/website> site/pages/checksums.html
  * every 5min a preview can be viewed here: <https://test.lazarus-ide.org/index.php?page=checksums>
  * Mail to Marc to update website from svn



## Other branches

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | 2.2 | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/Lazarus_2.2_fixes_branch](https://web.archive.org/web/20250219120248/https://wiki.freepascal.org/Lazarus_2.2_fixes_branch)_
