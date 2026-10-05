# wndInspector FF8S

window Inspector **F** ind **F** ile **&** **S** elect 

repository  | [Github](<https://github.com/in0k-LazarusIDE-plugins/in0k_LazIdeEXT_wndInspector_FF8S>) |   
---|---|---  
last  | [v0.5.1_fullSRC.zip](<https://github.com/in0k-LazarusIDE-plugins/in0k_LazIdeEXT_wndInspector_FF8S/releases/download/v0.5.1/in0k_LazIdeEXT_wndInspector_FF8S-v0.5.1_fullSRC.zip>)  
  
  


## Contents

  * 1 Adds features to the IDE
    * 1.1 How it works
  * 2 Features
    * 2.1 IDE Command
    * 2.2 Auto mode
  * 3 Installation and Configuration



# Adds features to the IDE

Searching file from "Source Editor", in open "Inspectors" windows ("[Project Inspector](<IDE_Window__Project_Inspector.md> "IDE Window: Project Inspector")", "[Package Editor](<IDE_Window__Package_Editor.md> "IDE Window: Package Editor")"). 

##### How it works

Setting focus to a node "Dependency Tree" of window "Inspector", according to the current active file in the "Source Editor". 

For a visual understanding, see GIF demonstration [IDE Command](<https://github.com/in0k-LazarusIDE-plugins/in0k_LazIdeEXT_wndInspector_FF8S/wiki/Animation-'IDE-command'>) and [Auto MODE](<https://github.com/in0k-LazarusIDE-plugins/in0k_LazIdeEXT_wndInspector_FF8S/wiki/Animation-'Auto-MODE'>). 

# Features

###### IDE Command

  * shortcut: `Ctrl`+`Shift`+`Alt`+`F` (to change see Shortcuts)
  * menu item: `IDE menu`->`Search`->`Find File in "Inspector"`
  * menu item: `Source editor`->`Рopup menu`->`Find File in "Inspector"`
  * additionally: 
    * if file found in "Inspector", then brings window to "foreground"
    * message, if the file is not found in any of the open windows "Inspectors"



###### Auto mode

  * the search starts when you change "Active source editor"
  * additionally: 
    * if file found in "Inspector", then brings window to "[Second Plan](<https://github.com/in0k-src/in0k-bringToSecondPlane>)" (this works well on Windows systems. For other systems this option is by default NOT included, since it leads to "blink" interface)
    * visual highlighting of the active node in the "Dependency Tree"
    * save state of collapsed nodes in the "Dependency Tree"
    * "minimap" for Selected and Active node in the "Dependency Tree"
    * "Dependency Tree" in "Inspector" 
      * additional items in Рopup menu (Collapse All ...)



# Installation and Configuration

  * **Sources** : clone the [repository](<https://github.com/in0k-LazarusIDE-plugins/in0k_LazIdeEXT_wndInspector_FF8S>) with ALL subprojects OR download "FULL source code" archive (`.._fullSRC.zip`) from last [release](<https://github.com/in0k-LazarusIDE-plugins/in0k_LazIdeEXT_wndInspector_FF8S/releases>).
  * **Installation** : It uses a [standard](<Install_Packages.md> "Install Packages") installation package scheme.
  * **Configuration** : if you want, before the "build" package, edit the file `in0k_lazExt_SETTINGs.inc`.

---

_Source: [https://wiki.freepascal.org/wndInspector_FF8S](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/wndInspector_FF8S)_
