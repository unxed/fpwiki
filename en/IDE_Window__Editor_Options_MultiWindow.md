# IDE Window: Editor Options MultiWindow

│ **English (en)** │

_**This is part of the online help for the IDE.**_  
It describes the section: "Editor" / "Pages and Windows". You can open the described dialog in your IDE via:  


  * The menu: "Tools" => "Options" / Editor options ...
  * The source-editor pop-up menu: "Editor properties ...



_**Navigation**_  


  * [Editor Options](<IDE_Window__Editor_Options.md> "IDE Window: Editor Options") Overview of all Editor Options

|  | 

  * [IDE Options](<IDE_Window__IDE_Options_Dialog.md> "IDE Window: IDE Options Dialog") Overview of all options for the IDE

  
---|---|---  
  
* * *

## Contents

  * 1 Tabs
    * 1.1 Hide tab in single page window
  * 2 Find jump target editor
    * 2.1 Order to use for editors matching the same criteria
      * 2.1.1 Most recent focused editor for this file
      * 2.1.2 Editor (for file) in most recent focused window
    * 2.2 Priority list of criteria to choose an editor
      * 2.2.1 Locked, if text in view
      * 2.2.2 Unlocked, if text in centered view
      * 2.2.3 Unlocked
      * 2.2.4 New tab in existing window / New tab in new window
      * 2.2.5 Ignore Locks, longest unused editor
      * 2.2.6 Ignore Locks, if editor is current
      * 2.2.7 Ignore Locks, if editor in current window
      * 2.2.8 New tab, existing or new window
  * 3 About Page-Locking



# Tabs

## Hide tab in single page window

If a window contains only one editor, the tab will be hidden. The Name and modified status ("*") of the editor is shown in the window's title. 

# Find jump target editor

If you have more than one editor for a file open and you want to jump to a new location in the file, then there are several possibilities how to choose which editor to use. 

  * This does **not** affect normal editor navigation, such as page up/down, simple find/replace, next occurrence of word, ...
  * It does affect how: 
    * clicking on a find-in-files result
    * clicking a compiler message
    * jumping to a bookmark or history point works.
    * jumping to a declaration (in another file, if the other file has more than one editor)
    * ...
  * In some cases it can also affect codetool jumps (jumps to declaration or between interface and implementation) within the same file.



    (e.g. if you jump to the declaration in the interface, and the declaration is already shown in another editor with the same file, you may want to switch to the other editor)

  
By default Lazarus will use the current editor. Or if you jump to a different file, it will use the most recent used editor for that file. 

Lazarus offers you to change how the target editor for any of the above actions is to be found: 

  * When you jump to a code location that is within the visible screen area of one of the editors.



    

    If one of the editors showing the target file has the target location already on it's visible screen, but this editor wasn't used for longer, then the editor chosen by the above default would be a different editor. The result would be that the editor chosen by the above default would have to scroll to the same location than the other editor. You may instead want Lazarus to use the editor that is already at the correct location.

  * If you have placed an editor to an important location, and you do not want Lazarus to move this editor to another location.



    

    You may have set an editor to show the definition of a data structure which you need to see. Now if Lazarus needs to show any other location in this file, you do not want that editor to be used, because then you would no longer see the selected structure.
    In this case you can "lock" the page that you do not want to scroll, and force Lazarus to use another editor.

  


## Order to use for editors matching the same criteria

If after evaluation the prioritized list of criteria more than one editor can be used to display whatever code you requested to jump to Lazarus will choose the editor according to the selected order. 

### Most recent focused editor for this file

The default setting. Lazarus remembers for each editor, when it last was focused. It will find the editor that was focused most recently. 

This can be the currently active editor, if the current editor displays the requested file and matches the criteria. 

### Editor (for file) in most recent focused window

With this setting Lazarus looks at the window, that holds each editor. It selects the editor in the most recently focused/active Window (including the current active window). 

In many cases the setting are leading to the same result. But consider the following example
    
    The current active window has an editor for file "A". But it currently shows file "B". File "A" is also shown in another window. And the editor for "A" in the other window was more recently used, than the one in the current window. Now if you select let's say a find-in-files-result for file "A". 

Last editor focus
    will select the editor in the other window
Last window focus
    will select the editor in the current window

## Priority list of criteria to choose an editor

If Lazarus needs to find an editor in order to jump to the code you requested, then it will go through this list and use the editor from the first matching criteria. If the criteria matches more than one editor see Order for editors matching one criteria

### Locked, if text in view

Will check if any locked editor already has the requested target in it's visible screen area. 

### Unlocked, if text in centered view

Similar to "Locked, if text in view". 

The centered view, is the area of the text where a jump does not scroll the editor. Depending on the size of your window, if you jump to the first or last 2 to 5 lines on the screen the editor will be scrolled. 

However currently some jumps force a reposition of the editor, and will scroll anyway. 

This is a separate option, because a locked editor is always assumed to be intentionally located to it's position. An unlocked editor may just to be at some location. Choosing this option is therefore more likely to sometimes unexpectedly change the window/editor. 

### Unlocked

Any unlocked editor. 

Note that many of the other criteria (except those that explicitly say "locked only"), will also check for unlocked editors them self. Disabling this option often makes no difference. 

### New tab in existing window / New tab in new window

If no unlocked editor is found, then open a new tab. 

in existing window
    a new tab will be opened in an existing window. If there is no more window, that does not already have an (locked) editor for this file, then no tab will be opened and the next criteria evaluated

in a new window
    a new tab will be opened in a new window, even if there are other windows in which this editor could also have been opened. This will always work, so further criteria are never evaluated.

### Ignore Locks, longest unused editor

If no unlocked tab is found, then locks will be ignored. The editor which has been unused (unfocused) for the longest time will be used. This will always work, so further criteria are never evaluated. 

The "unfocused time" is always evaluated by the editors focus order. Never by when the windows where focused. 

### Ignore Locks, if editor is current

If no unlocked tab is found, and the current active editor is an editor of the requested file, then the lock of the current active editor is ignored and it is used. 

If the current active editor displays another file, the next criteria will be used. 

### Ignore Locks, if editor in current window

If no unlocked tab is found, and there is an editor of the requested file in the current active window, then the lock of this editor is ignored and it is used. 

If there is no editor for the file in the current active window, the next criteria will be used. 

### New tab, existing or new window

This is the default fall-back action. It can not be disabled. 

If no editor was found, a new editor in a new tab will be opened. If possible in an existing window, otherwise in a new window. 

# About Page-Locking

Page locking can be found in the context menu of each editor. It tells Lazarus that you don't want this editor to be moved away from it's current location. If a page is locked and you try to jump to a location in your code (bookmark, declaration, find in file results) then Lazarus will use another editor (of the same file) to display the target location. (incremental and simple search/replace are not considered jumps, and therefore relocate the editor) 

Read more: [Lazarus_blog](<http://lazarus-dev.blogspot.com/2010/04/page-locking-or-multiply-editor-windows.html>)

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Editor_Options_MultiWindow](https://web.archive.org/web/20230530055342/https://wiki.freepascal.org/IDE_Window%3A_Editor_Options_MultiWindow)_
