# Widgetset

[![Lazarus App Architecture.png](https://wiki.freepascal.org/images/f/f1/Lazarus_App_Architecture.png)](</File:Lazarus_App_Architecture.png>)

[](</File:Lazarus_App_Architecture.png> "Enlarge")

**Widgetsets** are adapter libraries that provide an interface between a platform-inpedentent sourcecode and platform-specific system functions. Thus they allow for development of [platform-native](<Introduction_to_platform-sensitive_development.md> "Introduction to platform-sensitive development") software without requiring to change the source code among platforms. 

Therefore, widgetsets form the platform-sensitive "glue" between [LCL](<LCL.md> "LCL") and operating system. 

## Development status of widgetsets

Unit | Item | State | Target | Backend | Responsible | Comments   
---|---|---|---|---|---|---  
[GTK1](<GTK1_Interface.md> "GTK1 Interface") | Deprecated interface | working | 1.0 | Gtk | - | \-   
[GTK2](<GTK2_Interface.md> "GTK2 Interface") | Main Linux (and similar UNIXes) interface | working | 1.0 | Gtk2 | [Zeljan](</User:Zeljan> "User:Zeljan") | \-   
[GTK3](<GTK3_Interface.md> "GTK3 Interface") | Linux (and similar UNIXes) interface | progress | 1.4 | Gtk3 | [Zeljan](</User:Zeljan> "User:Zeljan") | Alpha state   
[Win32](<Win32/64_Interface.md> "Win32/64 Interface") | Desktop Windows for both 32 and 64 bits | working | 1.0 | WinAPI | Paul Ishenin and [Vincent](</User:Vincent> "User:Vincent") | \-   
[Qt](<Qt_Interface.md> "Qt Interface") | The Qt4 interface | working | 1.0 | Qt and LCL | [Zeljan](</User:Zeljan> "User:Zeljan") | Depends on qt4 bindings   
[Qt5](<Qt5_Interface.md> "Qt5 Interface") | The Qt5 interface | working | 1.8 | Qt5 and LCL | [Zeljan](</User:Zeljan> "User:Zeljan") | Depends on qt5 bindings   
[Qt6](<Qt6_Interface.md> "Qt6 Interface") | The Qt6 interface | working | 2.4 | Qt6 and LCL | [Zeljan](</User:Zeljan> "User:Zeljan") | Depends on qt6 bindings   
[WinCE](<Windows_CE_Interface.md> "Windows CE Interface") | The Windows CE interface | working | 1.0 | Windows API and LCL | - | Depends on volunteers   
[fpGUI](<fpGUI_Interface.md> "fpGUI Interface") | The fpGUI interface | in progress | no target | fpGUI and LCL | - | Depends on volunteers   
[Carbon](<Carbon_Interface.md> "Carbon Interface") | The Carbon interface | stalled (deprecated) | 1.0 | Carbon and LCL | - | \-   
[Cocoa](<Cocoa_Interface.md> "Cocoa Interface") | The Cocoa interface | working | 2.6 - 2.8? | Cocoa and LCL | [Dmitry](</index.php?title=Dmitry&action=edit&redlink=1> "Dmitry \(page does not exist\)") | Depends on volunteers   
[CustomDrawn](<Custom_Drawn_Interface.md> "Custom Drawn Interface") | The CustomDrawn interface | in progress | no target | LCL, X11, Android NDK and SDK | - | Depends on volunteers   
  
**Legend:**

Working \- Stable, most or all parts implemented. 

Partially Implemented \- Works, but has some features missing 

In progress \- Someone is working on this 

Not Implemented \- Nothing done, needs your help 

Deprecated \- Outdated, obsolete, usage not recommended for new projects 

Unknown - Please review whether this component is working, and set its status here 

## See also

  * [Lazarus Roadmap](<Roadmap.md> "Roadmap") \- more detailed widgets status.
  * [Accessing the Interfaces directly](<Accessing_the_Interfaces_directly.md> "Accessing the Interfaces directly")

---

_Source: [https://wiki.freepascal.org/Widgetset](https://web.archive.org/web/20240906214020/https://wiki.freepascal.org/Widgetset)_
