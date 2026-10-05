# IDE Window: Menu Editor

│ **English (en)** │  **[suomi (fi)](</IDE_Window:_Menu_Editor/fi> "IDE Window: Menu Editor/fi")** │    
****

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Description of old version Menu Editor you can see (and edit) at [discussion page](</Talk:IDE_Window:_Menu_Editor> "Talk:IDE Window: Menu Editor")

## About

"Menu editor" is dialog which allows to customize contents of menus: [TMainMenu](<TMainMenu.md> "TMainMenu"), [TPopupMenu](<TPopupMenu.md> "TPopupMenu"). 

[![menueditor-popupmenu.png](https://wiki.freepascal.org/images/b/b3/menueditor-popupmenu.png)](</File:menueditor-popupmenu.png>)

## Preview area

This area shows preview of menu designed. For TMainMenu it shows horizontal band with top-level items, for TPopupMenu it shows vertical band (more vertical bands shown for each submenu). Clickable labels: 

  * "Add menu item" to add item. This label is shown when the last item of menu is selected (in TPopupMenu - bottom item).
  * "Add submenu" to add child submenu.



To edit properties of an item, click it on preview, then call Object Inspector. Or you can skip using OI, instead use right-click on each item. 

## Action buttons

Buttons with icons peform these actions: 

  * icons "long arrows": Move selected item up/down (or left/right in TMainMenu top level).
  * icons "plus with line": Add separator item above/below selected item. Disabled for TMainMenu top level.
  * icon "minus": Delete selected item.
  * icons "plus with 1 arrow": Add item before/after selected item (and select it).
  * icon "plus with 2 arrows": Add submenu for selected item (and select its first item).



Icons are different for TMainMenu top level (since items here are arranged horizontally).

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Menu_Editor](https://web.archive.org/web/20240308084610/https://wiki.freepascal.org/IDE_Window%3A_Menu_Editor)_
