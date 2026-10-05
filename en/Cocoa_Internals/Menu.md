# Cocoa Internals/Menu

The menu logic in macOS is different from the one in Windows and other systems. The menu bar runs along the top of the screen on the Mac. Sub-menus and icons in the menu bar let the user choose commands, perform tasks and check status. There is an option to automatically hide the menu bar so it's shown only when you move the pointer to the top of the screen. There are no "per window" menus attached (an application window could use a Toolbar instead). Only the Apple menu, system status menus (including third-party ones eg fan monitoring) and the **active** application's menu appear in the menu bar. 

## Contents

  * 1 Basic Logic
  * 2 Implementation
    * 2.1 Application Menu
    * 2.2 Application Menu Translation
    * 2.3 Menus and Menu Items Clicks
  * 3 Popup (Context) Menu
  * 4 See also



## Basic Logic

This is how the contents of the menu is defined: 

  * The TMainMenu of the active window is set as the Application menu
  * If active window doesn't have TMainMenu the last active window with TMainMenu is still used
  * The Window running as modal disables the main menu, UNLESS it has its own main menu.



## Implementation

Used classes are: 

  * NSMenu
  * NSMenuItem



### Application Menu

If there's a TMainMenu allocated for LCL form, WSCocoa internally allocates the "Application" menu. The "Application" menu is a macOS convention that any app should use. Since there's no logical concept of "Application" menu in the LCL, CocoaWS simply always creates it for you. 

### Application Menu Translation

The titles (caption) of the Application menus are stored as ["resourcestring"](<../IDE_Window__Make_ResourceString.md> "IDE Window: Make ResourceString") and could be translated by a translation module. 

### Menus and Menu Items Clicks

There's a difference between Menus and MenuItems. "Menus" is simply a "MenuItem" with a submenu (and other subitems). 

The difference is how CocoaWS is reporting "clicks" to the LCL. 

For "Menus" the clicks are reported when a mouse HOVERs over the menuitem. (this is due to the fact of the click is called at **menuNeedUpdate:**. Calling at **menuWillOpen:** is too late, as macOS directly forbids changing the submenu items at that time). 

"Clicks" for "MenuItems" are called only when the menu is already closed (the released the mouse). Yet, visually, it looks the as if the action takes place immediately. 

## Popup (Context) Menu

Cocoa provides NSView - based mechanism for handling context menus. The mechanism is not used by LCL. 

Instead, mouse events are listened. When either "right" click or "ctrl+click" is detected LM_ContextMenu event would be fired. 

Note: unlike Windows, macOS context menus are shown on "mouse-down" event. (on Windows, context menu event follows "mouse up" event). 

MacOS context menu doesn't have a corresponding keyboard short cut. "Context Menu" button (VK_APPS) is recognized as a button (and corresponding KeyDown, KeyUp events are called), but it doesn't cause the context menu to be shown up (anywhere in macOS system). 

## See also

  * [Cocoa Internals](<../Cocoa_Internals.md> "Cocoa Internals") \- Overview.
  * [Cocoa Internals/Application](<Application.md> "Cocoa Internals/Application")
  * [Cocoa Internals/Forms](<Forms.md> "Cocoa Internals/Forms")
  * [Cocoa Internals/Input](<Input.md> "Cocoa Internals/Input")

---

_Source: [https://wiki.freepascal.org/Cocoa_Internals/Menu](https://web.archive.org/web/20240914024011/https://wiki.freepascal.org/Cocoa_Internals/Menu)_
