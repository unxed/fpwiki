# key down

│ **[Deutsch (de)](</key_down/de> "key down/de")** │  **English (en)** │    
****

## Overview

The [OnKeyDown](<https://lazarus-ccr.sourceforge.io/docs/lcl/controls/twincontrol.onkeydown.html>) event of an object allows you to check what key the user has pressed. 

Note that the procedure keeps track of shift/alt/ctrl etc keys separately (in _Shift_) from the "regular" keys (in _Key_) - see the procedure signature in the example. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** _OnKeyDown_ doesn't support Unicode characters. If you need Unicode characters but no control characters, use [OnUTF8KeyPress](<https://lazarus-ccr.sourceforge.io/docs/lcl/controls/twincontrol.onutf8keypress.html>).

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** When a key is held down, the _OnKeyDown_ event is re-triggered. The first re-triggering event is after approx 500 ms and the next ones cycle between 30 and 50 ms.

## Example
    
    
    uses
      ...LCLType, Dialogs, ...;
      ...  
    procedure TForm1.Edit1KeyDown(Sender: TObject; var Key: Word;
      Shift: TShiftState);
    begin
      // Example: checking for simple keys:
      if (Key = VK_DOWN) or
         (Key = VK_UP) then
        ShowMessage('Pressed arrow up or down key');
      // Check for Alt-F2
      if (Key = VK_F2) and (ssAlt in Shift) then
        ShowMessage('Alt F2 was pressed')
      Key := 0; // Necessary for some widgetsets, e.g. Cocoa, in order to disable processing in subsequent elements.
    end;
    

## See also

  * [Description of keyboard events in Delphi](<http://delphi.about.com/od/objectpascalide/a/keyboard_events.htm>); should be applicable to Lazarus, as well.
  * [LCL Key Handling](<LCL_Key_Handling.md> "LCL Key Handling") Detailed background on key handling in the LCL.
  * [OnKeyPress](<OnKeyPress.md> "OnKeyPress")

---

_Source: [https://wiki.freepascal.org/key_down](https://web.archive.org/web/20240425002659/https://wiki.freepascal.org/key_down)_
