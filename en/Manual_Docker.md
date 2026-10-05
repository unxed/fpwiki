# Manual Docker

## Contents

  * 1 About
  * 2 Screenshot
  * 3 Author
  * 4 License
  * 5 Download
  * 6 Change Log
  * 7 Dependencies / System Requirements
  * 8 Installation
  * 9 How-to Use
  * 10 See Also



### About

There's been a lot of patches and wishes for Messages window to be docked at the source editor of Lazarus IDE. All patches were eventually rejected, because Messages window must be dockable by using [Docking manager](<Docking.md> "Docking"), that's not yet fully implemented for all widgetset. 

Keeping Messages window floating might be very annoying for some people, especially coming from Delphi. 

This IDE extension allows Messages window to dock to the source editor. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** **Disclaimer** : _The plugin there is to compensate on missing/incomplete functionality of a docking manager. The plugin WAS written at the time docking didn't work well. By the time you're reading this page, there might be a fully functioning docking manager and you're encouraged to use the proper solution, rather than this temporary workaround._

### Screenshot

[![ManualDock.ScreenShot.PNG](https://wiki.freepascal.org/images/2/22/ManualDock.ScreenShot.PNG)](</File:ManualDock.ScreenShot.PNG>), 

### Author

Dmitry 'skalogryz' Boyarintsev 

### License

You're free to use this IDE Plugin and its sources in any way you find useful. 

### Download

The latest stable release can be found at SVN: <https://svn.code.sf.net/p/lazarus-ccr/svn/components/manualdock>

### Change Log

  * Version 1.0.0.2 (r2776) 
    * fix the LCL error (of endless re-size loop)
    * fix the initial menu state
    * fix the controls order (that was causing the splitter and the panel to be on the different side of the "status bar"). Now the status bar is always moved to the bottom; (it's a hack though, since won't work if StatusBar is changed to someother control)
  * Revised on 12-sep-2012 with Lazarus 1.1. There's an error given on "Dock Message" action. Though it still allows to go on with Lazarus usage;
  * Version 1.0.0.1 14-feb-2009 fixed bug with unreadable/unwritable files
  * Version 1.0 10-feb-2009 Released



### Dependencies / System Requirements

  * Lazarus IDE 0.9.29 or higher



Status: Stable 

### Installation

  1. Download the extension sources.
  2. Copy the extensions source directory to Lazarus/Components directory.
  3. Start Lazarus IDE (if not started)
  4. Open installed packages manager Package->Configure installed packages...
  5. Select "manualdock 1.0" from "Available packages list"
  6. Press "Save and rebuild IDE".



### How-to Use

After Lazarus IDE is rebuilt with the extension, you should see an additional menu item at View menu, named "Dock Messages window". 

Selecting this menu item will dock/undock messages window from the source editor. 

On Linux you have to open "Package options" window and click to "Options" button, where Unit string "$(PkgOutDir)\" change to "$(PkgOutDir)/". 

## See Also

  * [Source Editor Maximizer](<Source_Editor_Maximizer.md> "Source Editor Maximizer")

---

_Source: [https://wiki.freepascal.org/Manual_Docker](https://web.archive.org/web/20250324162410/https://wiki.freepascal.org/Manual_Docker)_
