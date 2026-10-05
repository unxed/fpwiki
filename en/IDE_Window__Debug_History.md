# IDE Window: Debug History

│ **English (en)** │  **[русский (ru)](<../ru/IDE_Window__Debug_History.md>)** │

## Contents

  * 1 Navigation
  * 2 Important
  * 3 Debug History
  * 4 Limitations
  * 5 Data Displayed
  * 6 Automatic entries vs (User-taken-) Snapshots
  * 7 Interface
  * 8 See also



## Navigation

[Main Menu](<Main_menu.md> "Main menu") > [View](<Main_menu.md> "Main menu") > Debug Windows > History 

## Important

You must [setup the debugger](<Debugger_Setup.md> "Debugger Setup") and start the project to debug it. Only then this window will be useful. 

## Debug History

[![History.png](https://wiki.freepascal.org/images/b/bf/History.png)](</File:History.png>)

The History window shows a list of locations where the application was previously stopped or paused. (e.g. Hit Breakpoint, stopped after stepping). Entries can also be added by none-breaking [Breakpoints](<IDE_Window_Breakpoints.md> "IDE Window:Breakpoints"). 

Each time an entry is added locals, watches, stack and threads that are shown/evaluated at the time are stored. The history window allows to select each entry and view the stored values. Stored values can be viewed in there normal viewer windows (e.g. Watch window) which will follow the selection of the history window. 

## Limitations

  * The history only affects Watches, Locals, Threads, and Stack Window.



    Other Windows are _not_ affected. Not affected windows keep showing the current data while a history entry is selected

  * Only Watches and Locals in the top stackframe of the current thread are automatically evaluated (available, even if the related viewer windows were closed). Other data is only kept, if it was displayed at the time. (e.g. if the user manually selected another frame, and viewed the watches in the watch window.
  * Data evaluation can be aborted, if the user runs or steps the application before the data is ready. In this case it is not available in the history



## Data Displayed

  * The time when the entri was taken. This does not indicate how much time the app was running, as it includes paused time.
  * The location: if available method name (format depends on type of debug info), and source line. This refers to the thread that was active at the time.



## Automatic entries vs (User-taken-) Snapshots

The main list automatically has an entry added for every time the debugger pauses the app. This list will also remove older entries and keep only the last n entries. 

The 2nd list contains only entries the user selected. Those entries are kept until after the end of the debug session. Entries can be added to this list by either the snapshot button, or the breakpoint-property "take snapshot". 

## Interface

Double click
    Selects or deselects an entry. If an entry is selected, then [Watch list](<IDE_Window__Watch_list.md> "IDE Window: Watch list"), [Local variables](<IDE_Window__Local_Variables.md> "IDE Window: Local Variables"), [Stack](<IDE_Window__Call_Stack.md> "IDE Window: Call Stack") and [Thread](<IDE_Window__Threads.md> "IDE Window: Threads") show the history content.
[![debugger power.png](https://wiki.freepascal.org/images/b/ba/debugger_power.png)](</File:debugger_power.png>) Power
    If powered off, now history entries will be created.
[![debugger enable.png](https://wiki.freepascal.org/images/7/76/debugger_enable.png)](</File:debugger_enable.png>) Enable
    Indicates/Toggles, if history is shown in other windows. Uses the history entry last set by double click
[![clock.png](https://wiki.freepascal.org/images/8/8b/clock.png)](</File:clock.png>)/[![camera.png](https://wiki.freepascal.org/images/d/de/camera.png)](</File:camera.png>) Choose list
    Selects list of snapshot. [![clock.png](https://wiki.freepascal.org/images/8/8b/clock.png)](</File:clock.png>) Automatically created entries, created at each step/pause (if power=on). Contains up to the last 25 entries. [![camera.png](https://wiki.freepascal.org/images/d/de/camera.png)](</File:camera.png>) User selected snapshots or snapshots by watches with "take snapshot" option. The list is not limited.
[![camera add.png](https://wiki.freepascal.org/images/1/17/camera_add.png)](</File:camera_add.png>) Add to selected snapshot list
    Adds the current entry to the user selected list. The entry remains in the automatic list, until it is replaced by newer entries.
[![laz delete.png](https://wiki.freepascal.org/images/6/63/laz_delete.png)](</File:laz_delete.png>) Remove
    Deletes one entry from the current list.
[![menu clean.png](https://wiki.freepascal.org/images/7/74/menu_clean.png)](</File:menu_clean.png>) Delete all
    Deletes all entries from the current list.
[![laz save.png](https://wiki.freepascal.org/images/c/c1/laz_save.png)](</File:laz_save.png>)/[![laz open.png](https://wiki.freepascal.org/images/5/5f/laz_open.png)](</File:laz_open.png>) Export/Import
    Exports/Imports all entries, including the values for the watches, locals, stack and threads

## See also

  * [Watch list](<IDE_Window__Watch_list.md> "IDE Window: Watch list")
  * [Local variables](<IDE_Window__Local_Variables.md> "IDE Window: Local Variables")
  * [Call Stack](<IDE_Window__Call_Stack.md> "IDE Window: Call Stack")
  * [Thread](<IDE_Window__Threads.md> "IDE Window: Threads")


  * [Announcement on the developer Blog](<http://lazarus-dev.blogspot.co.uk/2011/05/remember-remember-history-of-debugging.html>)

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Debug_History](https://web.archive.org/web/20230209093516/https://wiki.freepascal.org/IDE_Window%3A_Debug_History)_
