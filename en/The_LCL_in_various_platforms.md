# The LCL in various platforms

## Contents

  * 1 Introduction
  * 2 Linker options
  * 3 TForm.BorderStyle
  * 4 Shortcut Keys
    * 4.1 macOS
    * 4.2 Windows
  * 5 Popup Menus
  * 6 Command line parameters



## Introduction

Many [LCL](<LCL.md> "LCL") properties, methods and other functionality represent a concept, which is then mapped to a specific native behavior in each platform in order to provide the user with a [platform-sensitive environment](<Introduction_to_platform-sensitive_development.md> "Introduction to platform-sensitive development"). This sections aims at clarifying LCL behavior which changes across platforms by clarifying what is to be expected in each platform so that more reliable software can be built. 

## Linker options

The LCL passes various linker options depending on platform. Here is the conditionals script from the LCL: 
    
    
    // linker options
    if TargetOS='darwin' then begin
      if LCLWidgetType='gtk' then
        UsageLibraryPath := '/usr/X11R6/lib;/sw/lib'
      else if LCLWidgetType='gtk2' then
        UsageLibraryPath := '/usr/X11R6/lib;/sw/lib;/sw/lib/pango-ft219/lib'
      else if LCLWidgetType='carbon' then begin
        UsageLinkerOptions := '-framework Carbon'
          +' -framework OpenGL'
          +' ''-dylib_file'' ''/System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libGL.dylib:/System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libGL.dylib''';
      end else if LCLWidgetType='cocoa' then
        UsageLinkerOptions := '-framework Cocoa';
    end else if TargetOS='solaris' then begin
      UsageLibraryPath:='/usr/X11R6/lib';
    end;
    

## TForm.BorderStyle

The BorderStyle of a Form represents a kind of form, and different platforms have different standards about how should the border of each kind of form be, and the LCL respects that. For example, message dialogs are usually not resizable under Windows, but resizable under X11-based Unixes. By using bsDialog it is possible to create a form which will have the adequate dialog appearance in every platform. Below is a full list of expected behaviors: 

**Windows**

BorderStyle | Has Border? | Resizable? | Decoration   
---|---|---|---  
bsNone | No | No | None   
bsDialog | Yes | No | Only X (Close)   
bsSingle | Yes | No | All   
bsSizable | Yes | Yes | All   
bsSizeToolWin | Yes | Yes | Only X and small title   
bsToolWin | Yes | No | Only X and small title   
  
**Windows CE**

For Windows CE see here: [Positioning and size of Dialogs and Forms](<Windows_CE_Development_Notes.md> "Windows CE Development Notes")

**Gtk**

Under Gtk on X11 the best one can do is ask the window manager to set a certain style, and it may ignore our request, so nothing, except sizable border vs. no border at all is guaranteed. i.e. only bsSizable and bsNone have a guaranteed behavior and the other ones will be either somewhere in between or equal to bsSizable. 

| Gtk 1 | Gtk 2****  
---|---|---  
BorderStyle | Has Border? | Resizable? | Decoration | Has Border? | Resizable? | Decoration   
bsNone | No | No | None | No | No | None   
bsDialog | Yes | Yes* | All* | Yes | Yes* | All*   
bsSingle | Yes | No (WM) | All | Yes | No (WM) | All   
bsSizable | Yes | Yes | All | Yes | Yes | All   
bsSizeToolWin | Yes | Yes | All* | Yes | Yes | All*   
bsToolWin | Yes | No (WM) | All* | Yes | No (WM) | All*   
  
Values with * differ from the Windows Implementation. 

Values with (WM) indicate that the feature is Window Manager specific. 

Associated bug track items: [[1]](<http://bugs.freepascal.org/view.php?id=1540>)

**macOS**

| Carbon | Cocoa1****  
---|---|---  
BorderStyle | Has Border? | Resizable? | Decoration | Has Border? | Resizable? | Decoration   
bsNone | No | No | None | Yes | Yes | All   
bsDialog | Yes | No | All | Yes | Yes | All   
bsSingle | Yes | No | All | Yes | Yes | All   
bsSizable | Yes | Yes | All | Yes | Yes | All   
bsSizeToolWin | Yes | Yes | All | Yes | Yes | All   
bsToolWin | Yes | No | All | Yes | Yes | All   
  
1Note: The Cocoa widgetset is still in development. Details of implementation may change, as well as adherence to GUI guidelines. 

## Shortcut Keys

Some Key sequences cannot be used by the Application in some platforms because the Operating System or Windows Manager is already using them. Below is a list for each platform. 

#### macOS

You should refer to Apple [_Human Interface Guidelines_](<https://developer.apple.com/design/human-interface-guidelines/macos/user-interaction/keyboard/>) for system reserved shortcuts. 

Key Sequence | Alternative | Description   
---|---|---  
`F8` / `⇧ Shift`+`F8` | ? | F8 activates Spaces   
`F9` / `⇧ Shift`+`F9` | ? | F9 shows all windows in Exposé   
`F10` / `⇧ Shift`+`F10` | ? | F10 shows all windows of the current program   
`F11` / `⇧ Shift`+`F11` | ? | F11 shows the Desktop   
`F12` / `⇧ Shift`+`F12` | ? | F12 activates the Dashboard   
  
#### Windows

Key Sequence | Alternative | Description   
---|---|---  
`F1` | ? | F1 invokes the program-specific help system   
  
## Popup Menus

Normally the popup menus are the items in the TPopupMenu assigned to the PopupMenu property. 

**GTK** (1+2) 

The GTK automatically adds to the TNoteBook and TPageControl headers for each page a menu item to quick jump to the page. 

**GTK2**

The GTK2 automatically adds to all edit controls like TEdit and TComboBox unicode and accessibility items. 

## Command line parameters

Some widgetsets parse the [command line parameters](<Command_line_parameters_and_environment_variables.md> "Command line parameters and environment variables") and react to some parameters. 

**GTK**

  * \--gtk-module
  * \--g-fatal-warnings
  * \--gtk-debug
  * \--gtk-no-debug
  * \--gdk-debug
  * \--gdk-no-debug
  * \--display
  * \--sync
  * \--no-xshm
  * \--name
  * \--class

---

_Source: [https://wiki.freepascal.org/The_LCL_in_various_platforms](https://web.archive.org/web/20250121214258/https://wiki.freepascal.org/The_LCL_in_various_platforms)_
