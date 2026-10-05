# OnKeyPress

**English (en)**   
****

## Overview

The [OnKeyPress](<https://lazarus-ccr.sourceforge.io/docs/lcl/controls/twincontrol.onkeypress.html>) event of an object allows you to check what key the user has pressed. 

Note that this procedure handles printable characters only. Non-printable characters (e.g. control sequences) are handled by the [OnKeyDown](<key_down.md> "key down") event. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** _OnKeyPress_ doesn't support Unicode characters. If you need Unicode characters but no control characters, use [OnUTF8KeyPress](<https://lazarus-ccr.sourceforge.io/docs/lcl/controls/twincontrol.onutf8keypress.html>).

## Example
    
    
    uses
      ...LCLType, Dialogs, ...;
      ...  
    
    procedure TMainForm.FormKeyPress(Sender: TObject; var Key: char);
    begin
      case key of
        '0': ShowMessage('"0" key pressed');
        '1': ShowMessage('"1" key pressed');
        '2': ShowMessage('"2" key pressed');
      end;
      Key := #0; // Necessary for some widgetsets, e.g. Cocoa, in order to disable processing in subsequent elements.
    end;
    

## See also

  * [LCL Key Handling](<LCL_Key_Handling.md> "LCL Key Handling") Detailed background on key handling in the LCL.
  * [key down](<key_down.md> "key down")

---

_Source: [https://wiki.freepascal.org/OnKeyPress](https://web.archive.org/web/20240613131455/https://wiki.freepascal.org/OnKeyPress)_
