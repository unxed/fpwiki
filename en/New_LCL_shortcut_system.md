# New LCL shortcut system

This page collects ideas about improving system of shortcuts in LCL. 

## Contents

  * 1 Current state
  * 2 Requirements
  * 3 Conclusion
    * 3.1 TLCLShortCut class



## Current state
    
    
    TShortCut = Low(Word)..High(Word); // in classes.pp unit
     
    // virtual key codes VK_*
    Key := ShortCut and $FF; 
     
    // key modifiers
    Meta    := ShortCut and $1000; // scMeta
    Shift   := ShortCut and $2000; // scShift
    Control := ShortCut and $4000; // scControl
    Alt     := ShortCut and $8000; // scAlt
    

Unused bits in TShortCut: $0F00 

## Requirements

  1. backward compatibility 
     * accept current TShortCut format
     * new format can use only unused bits in current TShortCut format
  2. secondary, ternary shortcuts
  3. platform-specific shortcuts (E.g. `Ctrl`+`C` under Windows, `⌘ Cmd`+`C` under Mac OS X)
  4. shortcuts also with characters (E.g. `⌘ Cmd`+`?`)



## Conclusion

  * create list of extra shortcut data under TApplication (TLCLShortCutList)
  * the TShortCut value would be "an index" to one item in this list, unused bits can distinguish different shortcuts with same key code (0 will be reserved for old version)
  * modify shortcut handling in TApplication, TForm, TAction and TMenuItem 
    * check for character shortcuts in IntfUTF8KeyPress
  * add functions like ShortCutToText to TApplication
  * modify TShortCut component editor



### TLCLShortCut class

  * index - TShortCut with modified unused bits
  * priority list of TLCLShortCutItem 
    * key modifiers
    * platform modifiers
    * key or UTF-8 character

---

_Source: [https://wiki.freepascal.org/New_LCL_shortcut_system](https://web.archive.org/web/20240226045238/https://wiki.freepascal.org/New_LCL_shortcut_system)_
