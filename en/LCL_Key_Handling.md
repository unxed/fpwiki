# LCL Key Handling

│ **English (en)** │  **[日本語 (ja)](</LCL_Key_Handling/ja> "LCL Key Handling/ja")** │  **[português (pt)](</LCL_Key_Handling/pt> "LCL Key Handling/pt")** │    
****

## Contents

  * 1 Key codes
  * 2 Pressing a key (KeyDown)
  * 3 Key pressed, characters sent (KeyPress)
  * 4 Releasing the key (KeyUp)
  * 5 What should be sent via KeyPress
    * 5.1 Keys that generate KeyDown/Char/KeyUp
    * 5.2 Keys that generate KeyDown/KeyUp only
    * 5.3 Notes on numeric keypad



## Key codes

Key codes are defined in the LCLType unit. They are in the form of VK_DELETE and although these are old Windows symbols they work on all platforms. Other useful examples are VK_RETURN, VK_ESCAPE and VK_TAB. Normal letters and numbers are in the form VK_S or VK_5 the number pad keys are formed like this VK_NUMPAD5. 

  * [VK_ definitions reference](<http://lazarus-ccr.sourceforge.net/docs/lcl/lcltype/index-2.html>)


  * VK Table, slightly more useful, even for non German speakers



## Pressing a key (KeyDown)

When the widgetset receives a key press it must act as follows. Before letting the "native" widget handle the key, send CN_KEYDOWN/CN_SYSKEYDOWN to the LCL (CN_SYSKEYDOWN when alt key is down), which calls DoKeyDownBeforeInterface consisting of: 

  1. call Application.NotifyKeyDownBeforeHandler (invokes before handlers)
  2. get parent form, if it has keypreview, call its DoKeyDownBeforeInterface
  3. let the associated dragobject handle the key
  4. call KeyDownBeforeInterface (if control does not have csNoStdEvents in ControlStyle): 
     1. call KeyDown: 
        1. call [OnKeyDown](<key_down.md> "key down") handler if any



When unhandled, that is, Message.Result = 0, it lets the widget handle the key, and if the widget has not done anything useful, then send a LM_KEYDOWN/LM_SYSKEYDOWN message to the LCL, which calls DoRemainingKeyDown consisting of: 

  1. if a popupmenu is assigned, check it for shortcuts (whether it is popped up or not)
  2. get parent form, let it handle possible shortcut (TCustomForm.IsShortcut): 
     1. if form has OnShortcut event assigned, call it
     2. if form has Menu assigned, check it for shortcuts: 
        * iterate through all menu items for this menu, check shortcuts, ".Click" the item
        * set the menu's ShortcutHandled property to false in the menu item's OnClick handler, if you did not handle the shortcut and want to let the key processing continue
     3. check the action lists for shortcuts, .Execute matching actions
  3. let the application handle a shortcut (Application.IsShortcut): 
     1. if application has OnShortcut assigned, call it
     2. if there is a modal form active, let it handle a shortcut (see above, TCustomForm.IsShortcut)
     3. else if there is a focused form (Screen.ActiveCustomForm), let it handle a shortcut
     4. else if there is a main form (Application.MainForm), let it handle a shortcut
  4. if there is a Parent, iteratively call Parent.ChildKey to handle key message 
     * use it to handle key shortcuts in a certain "area", for example a panel
     * TWinControl will call its Parent's ChildKey; that's the "iteratively"
  5. call ControlKeyDown, which in TWinControl, calls Application.ControlKeyDown: 
     1. handle tab navigation for controls
     * if your custom control does something special with tab, override and inhibit, for example TCustomMemo
  6. call KeyDownAfterInterface; TWinControl does nothing here
  7. let Application call KeyDownAfter handlers



Note that result (returning 1 or 0 in Message.Result) matters sometimes: windows for example will only send a WM_CHAR message for a key when a WM_KEYDOWN message that key has returned 0. 

See [key down](<key_down.md> "key down") for details how to handle key down events in your application. 

## Key pressed, characters sent (KeyPress)

The widgetset can either send CN_CHAR message, or call the IntfUtf8KeyPress method. CN_CHAR does not support UTF-8 characters, so that is why a IntfUtf8KeyPress exists. CN_CHAR handling consists of: 

  1. if widgetset (interface) does not send utf8 key presses, then call IntfUtf8KeyPress
  2. call DoKeyPress: 
     1. get parent form, and if it has KeyPreview call its DoKeyPress
     2. call KeyPress (if control does not have csNoStdEvents in ControlStyle): 
        1. call [OnKeyPress](<OnKeyPress.md> "OnKeyPress") handler if any



IntfUtf8KeyPress (DoUtf8KeyPress) handling consists of: 

  1. get parent form, and if it has KeyPreview call its DoUtf8KeyPress
  2. call Utf8KeyPress (if control does not have csNoStdEvents in ControlStyle): 
     1. call OnUtf8KeyPress handler if any



When unhandled, that is, Message.Result = 0, it lets the widget handle the key, and if the widget has not done anything useful, then send a LM_CHAR/LM_SYSCHAR message to the LCL, which calls SendDialogChar consisting of: 

  1. get parent form and if found call its .DialogChar: 
     1. TWinControl broadcasts DialogChar to all its children 
        * use it to implement accelerator shortcuts for controls "near" (on the same form) as the focused control
        * example: TCustomLabel uses it to Focus the assigned .FocusControl when alt+accelerator is pressed



See [OnKeyPress](<OnKeyPress.md> "OnKeyPress") for details how to handle key press events in your application. 

## Releasing the key (KeyUp)

Releasing a key is very much, in the order of events happening, alike pressing a key, but some functions are missing. Again the widgetset will send a CN_KEYUP/CN_SYSKEYUP to the LCL before letting the widget handle the key, which will call DoKeyUpBeforeInterface consisting of: 

  1. get parent form, if it has keypreview, call its DoKeyUpBeforeInterface
  2. let the associated dragobject handle the key
  3. call KeyUpBeforeInterface (if control does not have csNoStdEvents in ControlStyle): 
     1. call KeyUp: 
        1. call OnKeyUp handler if any



When unhandled, that is, Message.Result = 0, it lets the widget handle the key, and if the widget has not done anything useful, then send a LM_KEYUP/LM_SYSKEYUP message to the LCL, which calls DoRemainingKeyUp consisting of: 

  1. call ControlKeyUp, which in TWinControl, calls Application.ControlKeyDown: 
     1. handle Enter and Escape keys for forms
     * if your custom control does something special with enter and escape, override and inhibit
  2. call KeyUpAfterInterface; TWinControl does nothing here



## What should be sent via KeyPress

While KeyDown and KeyUp messages should always be sent, determining when something should be sent via CN_(SYS)CHAR/LM_(SYS)CHAR and IntfUTF8KeyPress is not so obvious. 

The general rule is: _if a key represents a character (something visual), that character must be sent_. 

However, what are the keys that represent a character? Since LCL should be Delphi compatible, and since Delphi is based on Microsoft Windows, KeyDown/Char/KeyUp messages should be sent according to the way that WM_KEYDOWN, WM_CHAR, WM_KEYUP messages are sent by Windows. 

This behaviour is sometimes a bit strange (e.g: TAB only generates WM_KEYDOWN/WM_KEYUP, while BACKSPACE generates WM_KEYDOWN/WM_CHAR/WM_KEYUP) but that's how it works. 

**Note** : In KeyDown/KeyUp messages, the virtual key code must be passed (that is, a VK_ constant), while in xx_(SYS)CHAR messages the ASCII character (or the UTF8 character for IntfUTF8KeyPress) must be sent. E.g.: for the ESC key, you put the VK_ESCAPE key code in KeyDown/KeyUp messages, and $1B in xx_(SYS)CHAR messages and in IntfUTF8KeyPress. 

### Keys that generate KeyDown/Char/KeyUp

  * 'Simple' letters (a..z)
  * Numbers
  * Esc
  * Backspace
  * Space
  * Return
  * Return on numeric keypad
  * Numeric keypad operators (Add, Subtract, Multiply, Divide)
  * Numbers on numeric keypad
  * Decimal separator on numeric keypad



_(See laterNotes on numeric keypad)_

### Keys that generate KeyDown/KeyUp only

  * Function keys (F1-F12)
  * Print Screen
  * Scroll Lock
  * Pause
  * Shift
  * Caps Lock
  * Tab
  * Control
  * Windows Logo Key
  * Alt and AltGr
  * Windows 'Application Key'
  * Insert
  * Delete
  * Home
  * End
  * Page Up
  * Page Down
  * Arrow Keys (Up, Down, Left, Right)
  * Num Lock



### Notes on numeric keypad

Usually, numbers and decimal separator on the numeric keypad can only be detected if Num Lock is on. Otherwise, they send keycodes as if the user pressed arrow keys (or keys like Ins, Del, Home and so on). Return and operators aren't affected by Num Lock. 

Usually when Num Lock is off, pressing numbers on numeric keypad is the same as pressing arrow keys (so 8 generates VK_UP, 7 generates VK_HOME and so on) and you can't determine if the user pressed the "real" arrow key or a key on the keypad. In general, you shouldn't care about it.

---

_Source: [https://wiki.freepascal.org/LCL_Key_Handling](https://web.archive.org/web/20250322214944/https://wiki.freepascal.org/LCL_Key_Handling)_
