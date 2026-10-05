# How to use menu controls

│ **English (en)** │  **[suomi (fi)](</How_to_use_menu_controls/fi> "How to use menu controls/fi")** │    
****

## Contents

  * 1 TMainMenu
  * 2 TPopupMenu
  * 3 Menu editor
    * 3.1 Setting Shortcuts Programmatically
  * 4 ActionList use



### TMainMenu

[TMainMenu](<TMainMenu.md> "TMainMenu") is the Main Menu that appears at the top of most forms; form designers can customise by choosing various menu items. 

[TMainMenu](<TMainMenu.md> "TMainMenu") is a non-visual component: that is, if the icon is selected from the [Component Palette](<Component_Palette.md> "Component Palette") and placed on the form, it will not appear at [run time](<runtime.md> "runtime"). Instead, a menu bar with a structure defined by the [Menu Editor](<IDE_Window__Menu_Editor.md> "IDE Window: Menu Editor") will appear. 

### TPopupMenu

[TPopupMenu](<TPopupMenu.md> "TPopupMenu") is a menu window that pops up with pertinent, usually context-sensitive, details and choices when the right mouse button is clicked on a control if the popupmenu is linked to the PopupMenu [property](</Property> "Property") of that component, thus providing a context sensitive menu for that component. 

### Menu editor

To see the [Menu Editor](<IDE_Window__Menu_Editor.md> "IDE Window: Menu Editor"), right-click on the Main Menu or Popup Menu icon on your Form. A pop-up box appears that invites you to enter items into the Menu bar. 

An edit box is displayed, containing a button labeled "New Item1". If you right-click on that box, a pop-up menu is displayed that allows you to add a new item before or after (along the same level) or create a sub-menu with the opportunity to add further items below (or above) the new item in a downward column. 

Any of the [TMenuItems](</index.php?title=TMenuItem&action=edit&redlink=1> "TMenuItem \(page does not exist\)") that you add can be configured using the [ Object Inspector](<IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector"). 

At the least you should give each item a _Caption_ which will appear on the Menu Bar. The caption should indicate the activity to be selected, such as "File Open" or "Close", "Run" or "Quit". You may also wish to give it a more meaningful _Name_. 

If you want a particular letter in the Caption to be associated with a shortcut key, that letter should be preceded by an [ ampersand (&)](<&.md> "&"). The Menu item at run-time will appear with the shortcut letter underlined, and hitting that letter key will have the same effect as selecting the menu item. Alternatively you can choose a shortcut key sequence (such as `Ctrl`+`C` for Copy or `Ctrl`+`V` for Paste - the standard keyboard shortcuts) with the _ShortCut_ property of the [TMenuItem](</index.php?title=TMenuItem&action=edit&redlink=1> "TMenuItem \(page does not exist\)"). 

#### Setting Shortcuts Programmatically

Two functions are provided that convert virtual key to shortcuts and visa versa, KeyToShortCut() and ShortCutToKey(). eg 
    
    
    MenuBold.ShortCut:= KeyToShortCut(VK_B, [ssMeta]);
    

### ActionList use

It is often helpful to use the Menu controls in conjunction with a [TActionList](<TActionList.md> "TActionList") which contains a series of standard or customised [TActions](<TAction.md> "TAction"). Menu items can be linked in the Object Inspector to _actions_ on the list, and the same actions can be linked to [TButtons](<TButton.md> "TButton"), [TToolButtons](<TToolButton.md> "TToolButton"), [TSpeedButtons](<TSpeedButton.md> "TSpeedButton") etc. It is obviously more efficient to re-use the same code to respond to the various events, rather than writing separate _OnClick_ event handlers for each individual control. 

By default, a number of standard actions are pre-loaded from _StdActns_ or, if DataAware controls are used, from _DBActns_. These actions can be chosen using the [ActionList editor](</index.php?title=ActionList_editor&action=edit&redlink=1> "ActionList editor \(page does not exist\)") which appears when you right-click on the [TActionList](<TActionList.md> "TActionList") icon on the Form Designer.

---

_Source: [https://wiki.freepascal.org/How_to_use_menu_controls](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/How_to_use_menu_controls)_
