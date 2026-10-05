# IDE Window: Editor Options General

│ **English (en)** │    
****

_**This is part of the online help for the IDE.**_  
It describes the section: "Editor" / "General". You can open the described dialog in your IDE via:  


  * The menu: "Tools" => "Options" / Editor options ...
  * The source-editor pop-up menu: "Editor properties ...



_**Navigation**_  


  * [Editor Options](<IDE_Window__Editor_Options.md> "IDE Window: Editor Options") Overview of all Editor Options

|  | 

  * [IDE Options](<IDE_Window__IDE_Options_Dialog.md> "IDE Window: IDE Options Dialog") Overview of all options for the IDE

  
---|---|---  
  
* * *

## Contents

  * 1 Undo / Redo
    * 1.1 Undo after save
    * 1.2 Group Undo
    * 1.3 Undo limit
  * 2 Scrolling
    * 2.1 Scroll past end of file
    * 2.2 Scroll by one less
    * 2.3 Half page scroll
    * 2.4 Scroll Hint
  * 3 Indent and Tabs
    * 3.1 Tabs to spaces
    * 3.2 Tab widths
    * 3.3 Smart tabs
    * 3.4 Auto indent
    * 3.5 Auto Indent (mode dropdown)
    * 3.6 Tab indents blocks
    * 3.7 Block indent (spaces/tabs)
  * 4 Cursor
    * 4.1 Keep cursor X position
    * 4.2 Cursor skips selection
    * 4.3 Cursor skips tabs
    * 4.4 Persistent cursor
    * 4.5 Right mouse moves caret
    * 4.6 Always visible cursor
    * 4.7 Home key jumps to nearest start
    * 4.8 Caret past end of line
    * 4.9 End key jumps to nearest start
  * 5 Multi carets commands (Lazarus 1.5)
    * 5.1 Enable multi caret for column selection
    * 5.2 Multi caret (column select) move with cursor
    * 5.3 Multi caret move with cursor
  * 6 Selection
    * 6.1 Persistent Selection
    * 6.2 Overwrite Block



# Undo / Redo

### Undo after save

Keep the Undo list on save. 

### Group Undo

When undoing/redoing actions, handle all continuous changes of the same kind in one call instead of undoing/redoing each command separately. 

## Undo limit

Maximum number of Undo steps. 

# Scrolling

### Scroll past end of file

Allows to scroll the last line of your text to the top of the window. This will display blank lines. If this option is off, then the last line of text will be forced to the bottom line of the window. 

### Scroll by one less

Scrolling pages will scroll one line less than the amount of lines in the window. 

### Half page scroll

When scrolling with page-up and page-down commands, only scroll a half page at a time. 

### Scroll Hint

When activated and you scroll the editor page with the mouse of scrollbar thumb, a small hint window will appear in the top right indicating the line number at the top of the editor page. After scrolling, the hint window will automatically disappear after 1.5 seconds. 

# Indent and Tabs

[![IDE Options Editor General Tabs.png](https://wiki.freepascal.org/images/4/4a/IDE_Options_Editor_General_Tabs.png)](</File:IDE_Options_Editor_General_Tabs.png>)

_**Some option for automatic indent can be found on the Codetools - General Settings page**_

### Tabs to spaces

When editing lines the editor adds spaces instead of tabs. The rest of the lines will not be altered and already existing tabs are kept. To convert all tabs of a file select the whole file (Ctrl-A) and use Main menu / Edit/ tabs to spaces. 

### Tab widths

The width of on tab. A tab in the source ends at the next multiply of this (e.g. 4, 8, 12, 16, 20). If "Tabs to spaces" is used, then the `Tab ⇆` key will insert the amount of spaces needed to reach the next tab stop column. 

This setting does not apply if "Smart tabs" are active. 

### Smart tabs

The tab key will move forward to position below the start-position of the next word in the preceding line. 

If below the last word of the preceding line, then you move to the position below the end of the preceding line. If behind the last word of the preceding line, it uses the next line above that is longer than your current line. 

### Auto indent

When inserting new lines (e.g. through pressing Return or Enter key), the editor will indent the caret on the new line with the same amount of leading white space as the preceding line. 

This includes "auto un-intend": If the caret is located after the last whitespace of a continuous block of whitespaces at the beginning of a line, then pressing backspace will un-intend. This is done by looking for a start of line (indent) in the line(s) above the current line. For an line containing Whitespaces only (see trim-trailing-space modes) this happens if the caret is at the end of the line. 

### Auto Indent (mode dropdown)

"Auto Indent" can use several ways to indent the new line. Despite the name, this currently refers to what happens if you press `↵ Enter` (and if you use the editors, not codetools indenter). 

  * Spaces: Fill the beginning of the new line with spaces to match the desired position
  * Space/Tab: Copy the same mix of spaces and tabs as used in the previous line
  * Position only: Do not insert anything, position the caret past the end of line (even if this option is disabled). Spaces will be filled in, if something is typed
  * Tabs, then spaces: Fill indent with tabs, and use spaces if no exact match is possible
  * Tabs, cut off: Fill indent with tabs, and reduce indent if no exact match is possible



### Tab indents blocks

When active `Tab ⇆` and `⇧ Shift`+`Tab ⇆` act as block indent, unindent when text is selected. 

### Block indent (spaces/tabs)

Specifies amount of spaces or tabs a block is indented/unindented. 

This applies to all block indent/unindent operations. They can be triggered by tab, if "Tab indents block" is on, or by any key assigned in the key setting (normally `Ctrl`+`I`) or the menu. 

See also: "Tab indent blocks". [[1]](<http://bugs.freepascal.org/view.php?id=20256>). 

# Cursor

### Keep cursor X position

When moving through lines w/o Cursor Past EOL, keeps the X position of the cursor. 

### Cursor skips selection

When there is a selection and you press the left arrow, the cursor will go to the start of the selection. 

### Cursor skips tabs

When enabled, then the edit cursor will jump over a tab characters (the width of the tab character) in the source code, if you press the `←` or `→` arrow keys. If disabled, the cursor will move one space at a time - ignoring the tab characters in the source code. 

### Persistent cursor

Keep cursor blinking, even if editor lost focus. This is needed on some systems, where the focus messages do not work properly. 

### Right mouse moves caret

This entry no longer exists. Use the mouse configuration instead. [IDE Window: Editor Options Mouse](<IDE_Window__Editor_Options_Mouse.md> "IDE Window: Editor Options Mouse")

### Always visible cursor

Move cursor to be always visible when scrolling. I.e., if cursor is at the top of editor and you scroll editor down, cursor will move into new visible area. 

### Home key jumps to nearest start

Home key jumps to first non-space char, and then to line start. 

### Caret past end of line

Allows the cursor to go past the last character into the white space at the end of a line. 

### End key jumps to nearest start

End key jumps to visual line end (last none whitespace) if caret is before this point, then to end of line 

# Multi carets commands (Lazarus 1.5)

[SynEdit MultiCaret plugin](<SynEdit_MultiCaret_plugin.md> "SynEdit MultiCaret plugin")

### Enable multi caret for column selection

When selecting in column mode (normally with Alt modifier key), a caret is created in each line of the selection. Typing will insert/edit text on each line. 

### Multi caret (column select) move with cursor

By default any caret movement with the cursor keys (up/down/left/right/end/home) exits the multi caret mode. If this option is enabled carets will persist, and all follow the action given by the key. 

### Multi caret move with cursor

For multi carets not created by column selection (mouse or special key commands), if this option is enabled carets will follow the action given by cursor keys (up/down/left/right/end/home). If the option is disabled any caret movement with the cursor keys exits the multi caret mode. 

# Selection

### Persistent Selection

A selection that does not become unselected, if the caret moves away, nor if the text is modified 

### Overwrite Block

If you start typing while a selection exists, the selected text will be overwritten. If the option is off, it will be unselected and the text be inserted behind.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Editor_Options_General](https://web.archive.org/web/20230922155942/https://wiki.freepascal.org/IDE_Window%3A_Editor_Options_General)_
