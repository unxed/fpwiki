# IDE Window: Code Explorer

│ **[Deutsch (de)](</IDE_Window:_Code_Explorer/de> "IDE Window: Code Explorer/de")** │  **English (en)** │  **[français (fr)](</IDE_Window:_Code_Explorer/fr> "IDE Window: Code Explorer/fr")** │  **[português (pt)](</IDE_Window:_Code_Explorer/pt> "IDE Window: Code Explorer/pt")** │    
****  
****

## Navigation

[![Code Explorer Code tab.png](https://wiki.freepascal.org/images/1/13/Code_Explorer_Code_tab.png)](</File:Code_Explorer_Code_tab.png>)

[![Code Explorer Directives tab.png](https://wiki.freepascal.org/images/7/78/Code_Explorer_Directives_tab.png)](</File:Code_Explorer_Directives_tab.png>)

  
The Code Explorer dialog is reached from the Lazarus IDE [Main Menu](<Main_menu.md> "Main menu") > [View](<Main_menu.md> "Main menu") > Code Explorer menu item. 

  


## Overview

The Code Explorer dialog has two tabs: **Code** and **Directives**. The _Code_ tab shows the Pascal structure - types, variables, constants, classes, etc and the [Code Observer](<IDE_Window__Code_Explorer_Options.md> "IDE Window: Code Explorer Options"). The _Directives_ tab shows the compiler directives structure - $MODEs, $IFDEFs, $DEFINEs, $INCLUDEs, etc. 

The Code Explorer dialog usually opens with the '_Code_ tab displaying the Unit name and branches for Interface and Implementation sections, but clicking on the + box to the left of any branch will open up its sub-branches, in more and more detail until individual constants, types and variables are displayed as well as procedure and function declarations. If you change the file displayed in the main Source Editor window, you need to click on the Refresh button of the Code Explorer to display the structure of the new file. 

Double click on the nodes to jump to the corresponding position in the source editor. 

_HINT:_ The Code Explorer is a floating window. That means you can leave it open and switch freely between other floating windows like the source editor or the [Object Inspector](<IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector"). 

  
**Toolbar Buttons**

  * **Filter** (text field above treeview): For example 'to' will show all identifiers containing 'to'. Like 'Button', 'IntToStr'.


  * **Refresh** : Click on this toolbar button to rebuild the Code Explorer treeview, forcing the unit currently shown in the source editor to be reparsed. This is useful if you have made changes in the Editor since first opening the Code Explorer window, and you don't have the option to refresh _On Idle_ set.


  * **Show Source Nodes** (code tab only): Click on this toolbar button to...


  * **Options** : This toolbar button opens the dialog for the setup options for configuring the Code Explorer. See the [Code Explorer Options](<IDE_Window__Code_Explorer_Options.md> "IDE Window: Code Explorer Options") dialog for more details.



  


**Context menu - Code tab**

[![Code Explorer Contetx Menu.png](https://wiki.freepascal.org/images/b/b3/Code_Explorer_Contetx_Menu.png)](</File:Code_Explorer_Contetx_Menu.png>)

  * **Jump to** : The same effect as double-clicking on a node in the treeview (see above).


  * **Show position of source editor** : Shows the current position in the source editor.


  * **Refresh** : The same effect as clicking on the toolbar Refresh button (see above).


  * **Rename** : Opens the [Find or Rename Identifier](<IDE_Window__Find_or_Rename_identifier.md> "IDE Window: Find or Rename identifier") dialog.



  
**Context menu - Directives tab**

  * **Jump to** : The same effect as double-clicking on a node in the treeview (see above).


  * **Show position of source editor** : Shows the current position in the source editor.


  * **Refresh** : The same effect as clicking on the toolbar Refresh button (see above).

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Code_Explorer](https://web.archive.org/web/20241004151150/https://wiki.freepascal.org/IDE_Window%3A_Code_Explorer)_
