# Cocoa Internals/Theme

There are several types of "Themes" in macOS. 

## Contents

  * 1 System Wide Themes
    * 1.1 Tint
    * 1.2 Accent aka Highlight Color
    * 1.3 Aqua Theme vs Dark Mode
  * 2 Themed Controls Drawing API
    * 2.1 Toolbar Buttons
  * 3 See Also



## System Wide Themes

### Tint

The tint is a small change of highlighting colors. Historically it has been only 2 colors available: 

  * Blue
  * Graphite



As of today the functionality is still available in macOS, yet deprecated, as it has been suppressed by "Accent Color" 

### Accent aka Highlight Color

Introduced in macOS 10.13 - the system-wide highlight color change 

### Aqua Theme vs Dark Mode

The initial functionality is available in 10.13. The system preferences are allowing to change the main bar to the dark mode. However APIs are available to change window and/or view 

The full potential has been released in 10.14 Mojave. 

## Themed Controls Drawing API

The Cocoa doesn't have an explicit API to draw part of controls. (Carbon did provide HITheme interface. It still seems to be available for 64-bit ? really??, yet goes undocumented). 

In order to draw a part of a control on a view, a "hidden" control could be created and then drawn to the target context. The similar approach is used for Printing controls to a printer. 

The same might work with NSCells, yet Apple deprecates NSCells. 

### Toolbar Buttons

Toolbars buttons are used to draw "FLAT" buttons style. "FLAT" buttons are expected to be TRANSPARENT to the hosting control. They CAN draw background, if pressed. In order to achieve that, NSButtonCell is used, with the following attributes: 

  * buttonType = NSToggleButton (using NSOnOffButton type is causing to draw the background)
  * bezelStyle = NSSmallSquareBezelStyle



## See Also

  * <https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/DrawColor/Tasks/SystemTintAware.html>

---

_Source: [https://wiki.freepascal.org/Cocoa_Internals/Theme](https://web.archive.org/web/20240909184120/https://wiki.freepascal.org/Cocoa_Internals/Theme)_
