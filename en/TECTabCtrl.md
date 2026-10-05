# TECTabCtrl

**TECTabCtrl** [![tectabctrl.png](https://wiki.freepascal.org/images/0/05/tectabctrl.png)](</File:tectabctrl.png>) is advanced alternative [TTabControl](<TTabControl.md> "TTabControl"). It allows tab stacking, tabs in multiple rows, top/bottom/left/right orientation, reversed and right-to-left bi-directional mode, dragging tabs to move or fold them and many others. 

TECTabCtrl is installed to the (non-default) [EC tab](<Eye-Candy_Controls.md> "Eye-Candy Controls") on the Lazarus [Component Palette](<Component_Palette.md> "Component Palette"). 

Image shows tab with close button and arrow-down area. 

[![tab close dd.png](https://wiki.freepascal.org/images/1/1f/tab_close_dd.png)](</File:tab_close_dd.png>)

TECTabCtrl is part of [Eye-Candy Controls](<Eye-Candy_Controls.md> "Eye-Candy Controls") (shortly ECControls or EC-Controls), see [Eye-Candy Controls](<Eye-Candy_Controls.md> "Eye-Candy Controls"), set of visual controls written for Lazarus. Its design is based on Themes, therefore its look is very native everywhere, no matter what widgetset you use. 

Each release is announced on Lazarus Forum in section Third Party Announcements. 

There are always attached files `README.txt` (list of all known issues) and `CHANGELOG.txt` (list of all changes from previous release). 

License

GNU Lesser General Public License 2.0 with linking exception (a.k.a. Modified LGPL). File ectabctrl.pas contains license header. Also, files `COPYING.modifiedLGPL.txt` and `COPYING.LGPL.txt` are bundled to each archive. 

Author

This component is written by Blaazen. Copyright notice and real name is mentioned in the header of the unit. You can contact author on Lazarus Forum (nickname: Blaazen) in any thread about EC-Controls. If you are logged in to forum, you can get author's e-mail or send him private message. 

Download and Install

See [Eye-Candy Controls#Install](<Eye-Candy_Controls.md> "Eye-Candy Controls")

## Contents

  * 1 Options
  * 2 Using the component
  * 3 Code
  * 4 Examples
  * 5 Issues



## Options

Options (TECTabOption and TECTabCtrlOption) are shortly described (commented) in interface section of the unit `ectabctrl.pas`. 

## Using the component

Mouse

  * Left-click on a tab activates it.
  * Left-click on close-icon area closes tab.
  * Left-click on arrow-down area or right-click anywhere on the tab opens drop-down menu.
  * Middle-click closes tab (if option etcoMiddleBtnClose is set and tab is closeable).



Dragging

Dragging is realized by left mouse button or middle mouse button (if option etcoMiddleBtnClose is NOT set). 

When etcoFoldingPriority is NOT set: 

  * Dragging moves a tab.
  * Dragging + CTRL key (exactly ssModifier) folds a tab.



Setting etcoFoldingPriority causes opposite behaviour. 

Notes: 

  * etcoFixedPosition: tabs cannot be moved
  * etoCanBeFolded: tab can be folded into another tab
  * etoCanFold: tab can fold another tabs



Drop down menu

Allows: 

  * switching folded tabs
  * unfolding any or all folded tabs
  * closing any or all folded tabs
  * fold tab to any folder tab (includig its own folded tabs)
  * moving tab
  * closing respective tab



Keyboard

  * Arrow keys change tab (only when focused).
  * Arrow keys + CTRL key (exactly ssModifier) move tab (only when focused).
  * Enter or Space opens drop-down menu of selected tab (only when focused).
  * Acceleration keys (Alt + Key) change tab (doesn't need to be focused).



Note to multi-row tabs: the row with active tab is always placed on the baseline. Therefore, arrow keys Up and Down may sometime look like they work oppositely (assume horizontal position and active tab is NOT at the first row). It is by design. 

## Code

Each tab has two identificators: Index and ID. 

  * ID is unique and never changes during the life of tab
  * Index is unique but it changes as tabs are moved, folded, unfolded, added or deleted.


    
    
    function IDToIndex(AID: Integer): Integer;
    

Returns index of tab AID. 
    
    
    procedure ActivateTab(AIndex: Integer);
    

Activates tab AIndex. If tab is folded, folder and folded tab are switched. 
    
    
    function AddTab(APosition: TECTabAdding; AActivate: Boolean): TECTab;
    

Adds a new tab. Must be specified where the tab will be positioned and whether it will be activated. 
    
    
    procedure CloseAllFoldedTabs(AFolder: Integer);
    

Closes all tabs folded to AFolder. 
    
    
    procedure DeleteTab(AIndex: Integer);
    

Deletes tab AIndex. 
    
    
    function FindFolderOfTab(AIndex: Integer): Integer;
    

Returns index of folder which contains tab AIndex. Result is -1 when tab is not folded anywhere. 
    
    
    procedure FoldTab(AIndex, AFolder: Integer);
    

Folds tab AIndex to folder AFolder. 
    
    
    function IndexOfTabAt(X, Y: Integer): Integer;
    

Returns index of tab at coordinates X, Y. Result is -1 when there is no tab at X, Y. 
    
    
    function IsTabVisible(AIndex: Integer): Boolean;
    

Returns True when tab AIndex is visible and is in visible area (i.e. is not scrolled out). 
    
    
    procedure MakeTabAvailable(AIndex: Integer; AActivate: Boolean);
    

Makes tab AIndex available, i.e. moves tab to visible area (if necessary), switches it with its folder (if necessary) and activates it if AActivate is True. 
    
    
    procedure MoveTab(AFrom, ATo: Integer);
    

Moves tab to a new position. 
    
    
    procedure UnfoldTab(AID: Integer; AFolder: Integer=-1);
    

Unfolds tab AID. If its folder is known, caller can specify it. Otherwise, method attempts to find folder itself. 

## Examples

Screenshots are taken from Plasma5. Desktop theme for Examples 1-7 is Plastique, Example 8 is Breeze. 

Example 1

Basic layout. 

[![top 1row.png](https://wiki.freepascal.org/images/1/13/top_1row.png)](</File:top_1row.png>)

Example 2

Tabs are in one row, their size exceeds width of component. Left-right buttons and optional Add button appear. 

[![top 1row btns.png](https://wiki.freepascal.org/images/b/be/top_1row_btns.png)](</File:top_1row_btns.png>)

Example 3

Tabs are in two rows, their size exceeds width of component. Left-right buttons and optional Add button appear. 

[![top 2row btns.png](https://wiki.freepascal.org/images/0/08/top_2row_btns.png)](</File:top_2row_btns.png>)

Example 4

Component is in reversed mode. Tabs are in three rows, their size exceeds width of component. Left-right buttons and optional Add button appear. Note that buttons are aligned differently when component has one/two/three or more rows. 

[![top 3row btns rev.png](https://wiki.freepascal.org/images/4/40/top_3row_btns_rev.png)](</File:top_3row_btns_rev.png>)

Example 5

Tab position is tpLeft. Tabs are in two rows. Close button on each tab is displayed. 

[![left 2row close.png](https://wiki.freepascal.org/images/f/fb/left_2row_close.png)](</File:left_2row_close.png>)

Example 6

Tab position is tpBottom. Tabs are in two rows. Close button on each tab is displayed. Drop-down menu is opened (Tab5, Tab12 and Tab13 are folded to Tab1). 

Close buttons cannot be enabled for the **entire** TECTabCtrl. They can be enabled per each separate tab by adding _etoClosabel_ and _etoCloseBtn_ to the _Options_ property. 

[![bttm 2row close fold.png](https://wiki.freepascal.org/images/8/84/bttm_2row_close_fold.png)](</File:bttm_2row_close_fold.png>)

Example 7

Tab position is tpRight. Tabs are in two rows. Close button on each tab is displayed. Drop-down menu is opened (Tab5 is folded to Tab1). 

[![right 2row close fold.png](https://wiki.freepascal.org/images/e/e1/right_2row_close_fold.png)](</File:right_2row_close_fold.png>)

Example 8

Tabs are in two rows. Close button tab is displayed on the active tab only. Tabs are enlarges so they fulfil the width of component. Desktop theme is Breeze. 

[![top 2row r2l breeze enlarge.png](https://wiki.freepascal.org/images/b/b0/top_2row_r2l_breeze_enlarge.png)](</File:top_2row_r2l_breeze_enlarge.png>)

Example 9

Tab position is tpLeft but tabs are horizontal and reversed, i.e. from the bottom to the top (options etcoVertTabsHorizontal and etcoReversed). Close button on each tab is displayed. 

[![left verthor rev close.png](https://wiki.freepascal.org/images/3/31/left_verthor_rev_close.png)](</File:left_verthor_rev_close.png>)

Example 10

Add a new tab and set its caption 
    
    
       ECTabCtrl1.AddTab(etaLast, True);
       ECTabCtrl1.Tabs.Items[ECTabCtrl1.Tabs.Count-1].Text:='TabName';
    

## Issues

Component can be visually "ugly" or "incorrect" with some desktop themes or some Style setting. (example: GTk2 and Style = eosThemedPanel). That's because unit Themes does not implement images for tabs and images for button are used instead. Some themes (Oxygen, Breeze) draws button smaller than its rectangle and the remaining area around is used for shiny effect. It may cause design issue.

---

_Source: [https://wiki.freepascal.org/TECTabCtrl](https://web.archive.org/web/20250513173806/https://wiki.freepascal.org/TECTabCtrl)_
