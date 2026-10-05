# Windows API units

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

_Note: this article probably will need to be updated to reflect the current (FPC 2.7.1) situation. Also, it apparently was called RTL - does another article called RTL need to be added/substituted? BigChimp, October 2012_

Under windows (win32/win64/wince) one can access the Windows API via several Windows API units. This article will try to sketch an overview of what is available in FPC. Lazarus of course also supports Windows APIs (e.g. to show dialogs) which is not covered here. 

In general this page will describe the status as of FPC 2.2.1+, and only detail the earlier situation when it is needed for the understanding of the current structure. 

## Contents

  * 1 Organization
  * 2 Reducing the Windows unit
  * 3 Expansion
    * 3.1 3.2.x series



## Organization

The Windows API units have been redivided over directories/packages several times, currently the situation is as follows: 

  * The _windows_ and some other units (_winsock_ ,_winsock2_) reside in the RTL. This is because these units contain functions used by the RTL.
  * The other units originating from FPC reside in a package _winunits-base_ and some might be in rtl-extra
  * The current version of the (independent) JEDI windows API translation resides in a package _winunits-jedi_.



In older versions, both winunits packages were merged in one package (base/winunits or extra/winunits, depending on FPC version). In older versions more windows units were present in the RTL directory. This has now been reduced to the ones that the RTL actually has dependencies on. 

## Reducing the Windows unit

In the past, before _winunits-base_ existed, most missing API symbols were added to unit windows. Currently efforts are underway to remove these symbols and put them in the unit where Delphi puts them (_comdlg_ , _commctrl_ , _activex_). To avoid scoping problems (e.g. dangerous situations where the order of _commctrl_ and _windows_ in the uses clause matter) they are already removed from the Windows unit. This poses a barrier for Lazarus to use the winunits-base units, which was unacceptable. On top of this, the Windows unit versions weren't translated properly. 

Specifically, listview_* functions were removed (already in [commctrl](</index.php?title=commctrl&action=edit&redlink=1> "commctrl \(page does not exist\)")) and openfilename* and some other precooked dialogue related functions were moved to _commdlg_. 

## Expansion

Since 2.0.4, the winunits-base units have been vastly expanded. Most notably retranslations of many headers were done to take advantage of better 2.0 support. (unicode versions now useful due to widestrings, interfaces, winbools/bytebools), and numerous small fixes that improve the Delphi compability of the declarations. 

This process will probably continue for a while. 

### 3.2.x series

The 3.2.x series has mostly some organic fixes and enhancements to the windows header units. 

It should also enable structured exception handling which should improve handling COM objects that use exceptions internally

---

_Source: [https://wiki.freepascal.org/Windows_API_units](https://web.archive.org/web/20241209231300/https://wiki.freepascal.org/Windows_API_units)_
