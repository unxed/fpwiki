# IDE Window: Editor Macros

│ **English (en)** │  **[русский (ru)](<../ru/IDE_Window__Editor_Macros.md>)** │

## Contents

  * 1 Editor macros
    * 1.1 Display
    * 1.2 The Interface of the Editor macros dialog
  * 2 Invoking macros from the keyboard
  * 3 Limitations



  * _Lazarus 1.0 supports only macros entered via the keyboard_
  * The Editor Macros window is available in release versions of Lazarus from 1.2.6 onwards. It is _not_ available in the Lazarus 1.0 release.
  * See also [Editor_Macros_PascalScript](<Editor_Macros_PascalScript.md> "Editor Macros PascalScript")



# Editor macros

[![Editor macros.png](https://wiki.freepascal.org/images/1/11/Editor_macros.png)](</File:Editor_macros.png>)

Lazarus lets you record an unlimited number of macros. The Editor macros dialog allows you to choose which recorded macro is currently played by the dialog's Play button (or by using the default shortcut key combination `⇧ Shift`+`Ctrl`+`P` when editing code). Recorded macros can be saved for future reuse by moving them to the project session's saved settings, or to the IDE configuration settings. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** When editing a macro and then **saving** it, this saves it to the project (or IDE options) in **memory**. You must then save the project to preserve the macro. (Options are saved, for instance, when the IDE is closed). If the project is not saved then changes such as newly recorded or edited macros are lost.

## Display

All macros are displayed in one of 3 groupings: 

Recorded
    This lists newly recorded macros. These macros are not saved, and are lost when the IDE is closed.
Project
    This lists macros you have opted to be saved with the project session. Each project can have its own set of macros.  
Note: If the project is configured not to save a session, then macros in this group are not saved, and will be lost when the project is closed.
IDE
    This group lists all macros that will be saved with the IDE configuration settings. These macros are available to all projects.

Macros can be moved between Project and IDE using the appropriate button. 

Macros may show a status icon: 

[![Editor macros current.png](https://wiki.freepascal.org/images/8/81/Editor_macros_current.png)](</File:Editor_macros_current.png>)
    Current macro. This can be set using the "Select" button. This is the macro that will be played when you use the keyboard shortcut (the default is [Shift][Ctrl][P]).
[![Editor macros rec.png](https://wiki.freepascal.org/images/b/bc/Editor_macros_rec.png)](</File:Editor_macros_rec.png>)
    This macro is currently being recorded.
[![Editor macros play.png](https://wiki.freepascal.org/images/1/1a/Editor_macros_play.png)](</File:Editor_macros_play.png>)
    This macro is currently playing.
[![Editor macros broken.png](https://wiki.freepascal.org/images/d/d4/Editor_macros_broken.png)](</File:Editor_macros_broken.png>)
    This macro is broken. This icon indicates that the macro could not be loaded from the configuration file, and so the macro cannot be used. (It will remain untouched in its broken form in the config file).

## The Interface of the Editor macros dialog

Select
    Sets the **current** macro which will be used by the keyboard shortcut.  
The Play button is not affected by this.
Rename...
    Rename the macro (Macro1, Macro2, etc., the default names assigned by the IDE, are almost completely uninformative, and should be changed to a suitable self-descriptive name such as AlignSelectedLinesAtColon).
Edit Key
    Assign a key-combination for invoking the macro from the keyboard.
Edit
    Edit the macro as source - a new tab is opened in the Source Editor where you can view the naming used by the macro recorder and edit the macro source as needed.
Delete
    Delete the macro.
Play
    Play the selected macro. This is the macro selected by clicking the list. Not the "current" macro.
Repeat
    Sets how often the macro is played when the Play button is pressed. This setting affects only the Play button (the keyboard shortcut invokes only a single execution of the macro).
Record
    Starts the recording of a new macro.   
During recording this button doubles as a pause/continue button, allowing you to pause or continue the macro recording.  
If anything was recorded, the new macro will be set as the "current" macro in the Source Editor.
Stop
    This button ends the recording, and also allows you to interrupt and stop repeated play. In certain circumstances, it is possible to stop in the middle of a playing macro.
Move to
    Project/Ide: These two buttons move the selected macro to the indicated group when you want to save the macro for future use. The buttons are enabled or disabled according to the possible action(s).

# Invoking macros from the keyboard

The Lazarus IDE sets up the following 2 keyboard shortcuts by default: 

  * `⇧ Shift`+`Ctrl`+`P` Play the current macro
  * `⇧ Shift`+`Ctrl`+`R` Start/Stop recording a new macro



    During recording, the shortcut for Play can be used to Pause recording

# Limitations

  * A macro can only be recorded from a single editor.  
A macro can be played in any editor window afterwards, but only in one editor window at a time.
  * Macros only record commands issued by keyboard, and certain menu commands.  
Changes caused by mouse buttons or movement (such as moving the caret or selecting text) are [b]not[/b] recorded.
  * Macros are restricted to the Editor. You cannot currently record actions made in the Form Designer.
  * Sometimes you may be able to record actions apart from the editing of text. However, this is not supported, and playback may not render the desired effects correctly (or at all). E.g. It is possible to record stepping in the debugger (F7,F8,F9), but on playback you may find some of the commands are skipped at random.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Editor_Macros](https://web.archive.org/web/20250121215401/https://wiki.freepascal.org/IDE_Window%3A_Editor_Macros)_
