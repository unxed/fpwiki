# IDE Window: Editor Options Mouse

│ **English (en)** │

  
_**This is part of the online help for the IDE.**_  
It describes the section: "Editor" / "Mouse". You can open the described dialog in your IDE via:  


  * The menu: "Tools" => "Options" / Editor options ...
  * The source-editor pop-up menu: "Editor properties ...



_**Navigation**_  


  * [Editor Options](<IDE_Window__Editor_Options.md> "IDE Window: Editor Options") Overview of all Editor Options

|  | 

  * [IDE Options](<IDE_Window__IDE_Options_Dialog.md> "IDE Window: IDE Options Dialog") Overview of all options for the IDE

  
---|---|---  
  
* * *

## Contents

  * 1 General
    * 1.1 Hide mouse when typing
  * 2 Gutter
    * 2.1 Standard
    * 2.2 Extended
    * 2.3 Extended, right only (only in Lazarus 1.5)
    * 2.4 Use line numbers to select lines (only in Lazarus 1.5)
  * 3 Text-area
    * 3.1 Right mouse includes caret move
    * 3.2 Drag Selection
    * 3.3 Options no longer present (moved) in Lazarus 1.0
      * 3.3.1 Alt-Key set column mode
      * 3.3.2 Double click selects line
    * 3.4 Actions for each mouse button
      * 3.4.1 Actions for mouse wheel
      * 3.4.2 More about: Jumps to implementation/other block end
  * 4 Advanced Options



# General

## Hide mouse when typing

If the mouse is positioned over the editor, it will be hidden while you type. Any mouse-move, click, or menu action will make it visible again. 

# Gutter

You can currently choose between two predefined modes for the gutter area 

## Standard

All left-clicks on the gutter act when you press the mouse button down. 

## Extended

All left-clicks on the gutter act when you release the mouse button. This mode allows the mouse to select text while being over the gutter. 

  * Mouse pressed down and moved (either to other line, or into the text area), will make a selection. No action will be taken on when the mouse is released
  * Mouse pressed and released without moving it, will execute the normal gutter click action (breakpoint/fold)



## Extended, right only (only in Lazarus 1.5)

The Gutter-parts close to the text (line-numbers, change-indicator, and fold) act as describe in extended. The others act as described in standard. 

## Use line numbers to select lines (only in Lazarus 1.5)

Independent of the above setting. the gutter part showing the line numbers will always select the current line (mouse down), and additional full lines on mouse move. 

This selects in line-mode and can not be used to select part of a line by moving the mouse over the text area. To be able to start a normal selection (select any part of the line), use the "extended" setting above. 

# Text-area

## Right mouse includes caret move

Right mouse always triggers the context Menu. Setting the caret to the click location is optional. 

## Drag Selection

You can use the left mouse button to drag an existing selection to a new location in the text. Hold ctrl while dragging to make a copy. 

## Options no longer present (moved) in Lazarus 1.0

### Alt-Key set column mode

Use the entry **Select text (Columns)** from the drop down (for alt-button) 

If you use the left mouse button, then the dropdown for shift-alt-button will offer **Continue Select text (Columns) (Bound to Alt)**

### Double click selects line

Use the drop downs in the "Left2" page 

## Actions for each mouse button

[![IDE Options mouse choose button.png](https://wiki.freepascal.org/images/a/a8/IDE_Options_mouse_choose_button.png)](</File:IDE_Options_mouse_choose_button.png>)

The dialog provides a setup page for each mouse button. 

Left, Left2
    The main button of a mouse.
Right
    The 2nd button of a mouse. (also known as context menu button)
Middle
    Usually triggered by pressing (instead of turning) the wheel
Extra1, Extra2
    Only for 5 button mouses
Wheel
    Actions for turning the wheel

Each page provides a list of actions for the indicated mouse button, with or without modifier keys. 

  * The left-button without any modifier is fixed as "select text" and can not be modified.
  * Double and triple click is only supported for the left button (page "Left2").



The modifier Keys must be an exact match. If "Ctrl-Button" is configured, then other combinations like ctrl-alt-button or ctrl-shift-button will not trigger this configuration. 

  * However there is an exception, if **all** combinations containing alt for this button are set to "Nothing/Default" then ctrl-alt-button will be treated as ctrl-button.
  * If all combinations of ctrl, alt and shift are set to "Nothing/Default" then they modifier keys will be ignored for this button, and the button always triggers the action set on the no-modifier-key dropdown.
  * On some OS certain combinations of modifier key with specific buttons, may be handled by the OS, and are not available.



  


Nothing/Default
    No action, or action without modifier key
Select text
    Selects text. An existing selection will be lost.
Select text (Columns)
    Selects text in column mode. An existing selection will be lost.
Select text (Lines)
    Selects text, full lines only. An existing selection will be lost.
Select current Word
    Selects one word.
Select current Line (Text)
    Selects one line, without leading/trailing spaces.
Select current Line (Full)
    Selects one line, with leading/trailing spaces
Select current Paragraph
    Selects one paragraph. Bound by empty lines.
Paste
    Inserts the current selection. If there is no selection, then it depends on the OS, may insert (secondary) clipboard, or do nothing.
Jumps to implementation
    Navigates pascal source via codetools. (Find implementation of identifier)
Jumps to implementation/other block end
    Navigates pascal source... Also jumps between matching begin/end pairs
Add history point
    Add your own History point (normally done by implementation jumps.
History back
    Navigate to previous source location.
History forward
    If you navigated back in history, go forward again.
Set free bookmark
    Sets an available bookmark
Reset zoom
    See mouse wheel, sets editor font size to default
Context Menu
    normal editor context menu.
Context Menu (debug)
    debug section from editor context menu.
Context Menu (tab)
    Context menu for editor tabs (trigger anywhere in the editor area)

The following are only available for the shift-left-button with or without alt or ctrl. If the left button (with or without alt or ctrl) is set to "select text" (or "select text (Columns/Line)") then one extra entry is listed as first entry in the dropdown. The entry allows to continue (extend) an existing selection. The entry will be one of the following: 

Continue Select text
    
Continue Select text (Columns/Line)
    
Continue Select text (...) (Bound to Alt,Ctrl Button)
    Continue/Extend an existing selection, using the same selection mode as the button without shift

### Actions for mouse wheel

The wheel can be set to scroll, at different speeds, or change the editor font size (zoom) 

Normal scroll is vertical,but automatically changes to horizontal, if the editor has no horizontal scroll bar (to few line). Horizontal can be enforced. 

### More about: Jumps to implementation/other block end

by default, if you click on any identifier such as a function, variable or type name the IDE will jump to the declaration of this identifier (even into another unit). 

This is the same feature that is available as key-shortcut (usually `Alt`+`↑`). See "Key Mapping" / "Codetools" / "Find declaration" 

"other block end" means that if you are not over an identifier, but instead over a "begin", "end" or matching brackets or similar then the jump will go to the matching word/bracket. On the Keyboard this is `Ctrl`+`Q`+`K`. See also: "Key Mapping" / "Codetools" / "Find block other end") 

# Advanced Options

For advanced options please see here: [Advanced Mouse Options](<IDE_Window__EditorMouseOptionsAdvanced.md> "IDE Window: EditorMouseOptionsAdvanced")

Once you have made changes in the advanced settings, this page will show a warning that it does not represent your settings any more. 

In this case it also will no longer just follow the changes you make. Instead it will display a button which you must use to override the settings you made in the advanced page. A warning will be shown, if you have unsaved changes.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Editor_Options_Mouse](https://web.archive.org/web/20240221114741/https://wiki.freepascal.org/IDE_Window%3A_Editor_Options_Mouse)_
