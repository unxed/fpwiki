# Lazarus IDE Shortcuts

│ **English (en)** │  **[한국어 (ko)](</Lazarus_IDE_Shortcuts/ko> "Lazarus IDE Shortcuts/ko")** │    
****

## Contents

  * 1 Key mappings
    * 1.1 Gnome
    * 1.2 KDE
    * 1.3 macOS, Apple style
    * 1.4 macOS, Lazarus style
      * 1.4.1 Overidden "Apple style" keys



## Key mappings

All shortcuts can be edited in the [Tools > Options > Editor > Key Mappings](<IDE_Window__Editor_Options_Keymap.md> "IDE Window: Editor Options Keymap") and the preset schemes can be loaded using the [Choose Key Mapping Scheme](<IDE_Window__Choose_Key_Mapping_Scheme.md> "IDE Window: Choose Key Mapping Scheme") dialog. 

For what does each shortcut does, see the [Lazarus tutorial](<Lazarus_Tutorial.md> "Lazarus Tutorial"). 

### Gnome

The default key mapping for Windows fits quite well under Gnome. 

### KDE

The default key mapping for Windows fits quite well under KDE. Some keys like F9 are used by KDE. 

### macOS, Apple style

macOS has quite different key policies. For example most F keys are used by the OS, there are no menu Alt shortcuts and there are substantial guidelines (contrary to Linux and Windows where only a few keys are standardised to work the same under all programs). 

The Apple [_Human Interface Guidelines_ \- keyboard section](<https://developer.apple.com/design/human-interface-guidelines/macos/user-interaction/keyboard/>) deals with key bindings for macOS. 

Xcode is Apple's own integrated development environment. Here are the Xcode shortcuts: [![Xcode Shortcuts B W.png](https://wiki.freepascal.org/images/4/4c/Xcode_Shortcuts_B_W.png)](</File:Xcode_Shortcuts_B_W.png>)

([Image Source](<http://www.1729.us/xcode/Xcode%20Shortcuts%20B_W.png>)). 

### macOS, Lazarus style

A macOS key map is available for Lazarus and can be set via [Tools > Options > Editor > Key Mappings / Load a scheme](<IDE_Window__Editor_Options_Keymap.md> "IDE Window: Editor Options Keymap") which opens the [Choose Key Mapping Scheme](<IDE_Window__Choose_Key_Mapping_Scheme.md> "IDE Window: Choose Key Mapping Scheme") dialog. This preset scheme is based on the "macOS, Apple style" with some keys overridden (see below). 

Keys | Function defined by Apple | Conflict with lazarus default | Solution   
---|---|---|---  
F1 | misc | help | Apple: Cmd-[Shift]-?   
Ctrl-Shift-F1 | misc | help editor | Mattias: Cmd-Alt-[Shift]-?   
Ctrl-F2 | misc | stop program | Xcode: Shift-Cmd-Enter   
F3 | misc | find next | Apple: Cmd-G   
Shift-F3 | misc | find previous | Apple: Cmd-Shift-G   
F4 | misc | run to cursor | Apple: misc, Mattias: no shortcut for 'run to cursor'   
Ctrl-F4 | misc | close file | Apple: Cmd-Shift-W   
F5 | misc | add breakpoint | Xcode: Ctrl-P   
Ctrl-F5 | misc | add watch | Apple: misc, Mattias: no shortcut for add watch   
Ctrl-F8 | misc | jump next error | Xcode: Cmd-+   
Ctrl-Shift-F8 | misc | jump previous error | Xcode: Cmd-Shift-+   
F7 | misc | step into | Mattias: Cmd-Alt-R   
Ctrl-F7 | misc | evaluate | Ctrl-Shift-E   
F8 | misc | step over | Mattias: Cmd-Shift-R   
F9 | misc | run | Xcode: Cmd-R   
Ctrl-F9 | misc | build | Xcode: Cmd-B   
F11 | misc | toggle OI | Mattias: Alt-Cmd-I   
Shift-F11 | misc | add unit to project | Xcode: Alt-Cmd-A   
Ctrl-F11 | misc | open project | Apple: misc, Mattias: no key for 'open project'   
Ctrl-Shift-F11 | misc | project options | Mattias: no key for 'project options'   
F12 | misc | toggle unit/form | Apple: misc, Mattias: Cmd-Alt-F   
Ctrl-F12 | misc | view units | Mattias: Ctrl-Alt-U   
Shift-F12 | misc | view forms | Mattias: Ctrl-Shift-U   
[Shift]-Up,Down,Right,Left | move cursor |  |   
Alt-[Shift]-Left/Right | jump to next word |  |   
Alt-Up/Down | jump to start/end of paragraph (laz: Ctrl Q,B) | find declaration | Mattias: find declaration   
Cmd-[Shift]-Left/Right | jump to start/end of line (laz:Home/End) | jump next word | Apple: jump to start/end of line   
Cmd-Alt-Left/Right |  |  | Xcode: switch file   
Cmd-Alt-Up/Down |  |  | Xcode: jump source/header   
Cmd-Up/Down | jump to start/end of file (laz:Ctrl-Home) | scroll one up/down | Apple: jump to start/end of file   
Ctrl-Up/Down |  |  | Mattias: scroll one up/down   
Cmd-Shift-Up/Down | select to start/end of file (laz:Shift-Ctrl-Home) | method jump | Apple: select to start/end of file   
Ctrl-Shift-Up/Down |  |  | Mattias: method jump   
Cmd-[Shift]-? | show context help |  |   
Delete/Backspace | delete char |  |   
[Shift]-PageUp/Down | jump page up/down |  |   
[Shift]-Home/End | jump to start/end of file (laz:Ctrl-Home) | jump to start/end of line | Apple: jump to start/end of file   
Cmd-[Shift]-Tab | switch application | jump prev/next file | Apple: switch app   
Ctrl-[Shift]-Tab | jump to next control |  |   
Cmd-*-Space | spotlight, apple reserved | identifier completion | Apple: misc   
Ctrl-Space | Xcode: set mark |  | Mattias: identifier completion   
Shift-Ctrl-Space |  |  | Mattias: code context   
Alt-Ctrl-Space |  |  | Mattias: word completion   
Cmd-Alt-Escape | force quit |  |   
Cmd-A | select all |  |   
Cmd-C | copy |  |   
Cmd-Alt-D | show/hide dock |  |   
Cmd-Ctrl-D | dictionary |  |   
Cmd-E | find word at cursor | incremental find | find is smart to auto use word at cursor. incremental find.   
Cmd-F | find |  |   
Cmd-Alt-F | Xcode: view details |  | Mattias: Toggle Unit/Form   
Cmd-G | find next (laz:F3) | goto line | Apple: find next   
Cmd-Shift-G | find previous (laz:Shift-F3) | insert guid |   
Ctrl-Shift-G |  | insert guid | Mattias: insert guid   
Cmd-[Alt]-H | hide app | jump back | Apple: hide app   
Ctrl-[Shift]-H |  |  | Mattias: jump history backward/forward   
Ctrl-I |  | indent selection | Xcode: indent selection   
Ctrl-U |  | unindent selection | Xcode: unindent selection   
Cmd-L |  |  | Xcode: goto line   
Cmd-M | minimize window |  |   
Cmd-Alt-M | minimize app |  |   
Cmd-N | new file |  |   
Cmd-Shift-N |  |  | Tom: Insert line   
Cmd-O | open file |  |   
Cmd-Q | quit (laz: none) | goto line | Apple: quit   
Cmd-Shift-Q | log out |  |   
Cmd-R | Xcode: build and run | search and replace | Xcode: run   
Cmd-Shift-R |  |  | Mattias: step over   
Cmd-Alt-R |  |  | Mattias: step into   
Cmd-S | save |  |   
Cmd-Shift-S | Save As (laz: none) | save all | Apple: Cmd-Shift-S   
Cmd-Alt-S | Save All |  |   
Cmd-V | paste |  |   
Cmd-W | close window (laz: none) | word completion | Apple: close window   
Cmd-Shift-W | Xcode: close file |  |   
Cmd-Alt-W | close all files |  |   
Cmd-X | cut |  |   
Cmd-Y |  |  | Tom: delete to end of line   
Cmd-Z | undo |  |   
Control | The control modifier is used by cocoa in text fields. So it is free for designer and source editor functions |  |   
Alt-1-9 | create special chars on some layouts |  |   
Ctrl-1-9 |  | jump to bookmark |   
Ctrl-Shift-1-9 |  | set bookmark |   
Cmd-*-1-9 | apple functions like capture screen |  |   
  
#### Overidden "Apple style" keys

The "Apple style" scheme of keys can be hard on Lazarus users who work with Lazarus on multiple platforms because many of them are very different from the FreeBSD, Linux and Windows schemes. To minimize this problem the "macOS, Lazarus style" scheme was created (based on the default "macOS, Apple style" scheme, but with some changes to have keys similar to those on other platforms. Below are the keys which differ from the standard "macOS, Apple style" scheme: 

Keys | Function in Apple style | Function in Lazarus style   
---|---|---  
Home/End | jump to start/end of file | jump to start/end of line

---

_Source: [https://wiki.freepascal.org/Lazarus_IDE_Shortcuts](https://web.archive.org/web/20240121085726/https://wiki.freepascal.org/Lazarus_IDE_Shortcuts)_
