# IDE Window: Environment Options

│ **English (en)** │

[![IDE-options-Environment.JPG](https://wiki.freepascal.org/images/6/69/IDE-options-Environment.JPG)](</File:IDE-options-Environment.JPG>)

## Contents

  * 1 Files
  * 2 General
  * 3 Window
  * 4 IDE CoolBar
  * 5 Editor ToolBar
  * 6 Component Palette
  * 7 Form Editor
  * 8 Object Inspector
  * 9 FPDoc Editor
  * 10 Backup
  * 11 Naming
  * 12 File Filters



## Files

Max recent files

The maximum number of items in the _open recent file_ menu. 

Max recent project files

The maximum number of items in the _open recent project_ menu. 

Open last project on start

Enable to automatically reopen the last edited project on IDE start 

Show compile dialog

If enabled, you will see a compile 'progress' dialog appear while your project is being compiled. It will also show a summary of the total lines compiled, number of hints, warnings etc. 

Lazarus directory

Set here the directory of the Lazarus source directory. The Lazarus source directory contains directories like 'ide', 'debugger', 'converter', 'components' and 'lcl'. It is used to find sources (.pas) of the LCL and components. 

Compiler path

This must be the full filename of the used compiler. For example /usr/bin/ppc386 under Linux or /usr/local/bin/ppc386 (32 bit) or /usr/local/bin/ppcx64 (64 bit) under macOS or C:\pp\ppc386.exe under Windows. The filename depends on how the compiler was installed. 

FPC Source directory

Set here the directory of the Free Pascal source directory. The source directory contains directories like 'compiler', 'packages', 'rtl' and 'fcl'. Typically under Linux/*BSD/MacOSX this is found at /usr/share/fpcsrc/. 

**Note** : Do not confuse this with the FPC unit/library directory (.../units/i386-win32/), that only contains the compiled .ppu files and has a totally different structure. 

Make path

Set here the filename of the **make** tool. Leave blank to let the IDE use the default. The **make** tool is a utility used by many projects to help compiling big sources and is therefore installed on almost all Linux and macOS systems by default. The Lazarus Windows installer installs a version too. Under BSD use gmake. See here for more information [make](<http://www.gnu.org/software/make/>). 

Delphi installs a make executable too, which is incompatible with gnu make and which cannot be used for Lazarus. Don't leave this option blank if the make from Delphi os on the path. 

Directory for building test projects

When a newly created project is not yet saved and you build it, the IDE saves the whole project into this directory and builds it there.   


## General

[![IDE-options-Environment-General.JPG](https://wiki.freepascal.org/images/c/c1/IDE-options-Environment-General.JPG)](</File:IDE-options-Environment-General.JPG>)

Language

The language of all text of the IDE. You should restart the IDE to make sure all text is reloaded. 

Mouse Action

  * **Prefer double-click over single-click** (was "Double click on message jumps"). When enabled you must double click on a line in the message window (e.g. compiler messages). When disabled a single click is enough.



Auto Save

  * **Ask before saving project's session** \- When the project is closed the IDE asks to save changes. If only the session data (i.e. cursor positions and what files are open in the source editor) is changed and the session is saved to a separate lps file (not in the lpi file), then the IDE will not ask.



Desktop files

Load or save the environment options to/from a file. 

Check changes on disk with loading

When the IDE checks if some editor files changed on disk it normally only compares the file date. To also load the file and compare the content, enable this option. 

Hints for component palette

Show hints, when mouse is over the component palette. 

Hints for main speed buttons

Show hints, when mouse is over the buttons of the main IDE bar window. (e.g. buttons for open, save) 

  


## Window

[![IDE-options-Environment-Window.JPG](https://wiki.freepascal.org/images/6/67/IDE-options-Environment-Window.JPG)](</File:IDE-options-Environment-Window.JPG>)

Hide IDE windows on run

When running the project, all IDE windows are hidden and restored when program stops. 

Hide message icons

Normally the **Messages** window prepends an icon to each message so you can quickly see what type it has or if the IDE can _quick fix_ the message. If you don't want the icons, enable this option. 

IDE title starts with project name

Normally the IDE uses as its caption in the taskbar (the panel at top or the bottom of your screen) _Lazarus IDE vX.Y.Z - project1_. Most taskbars are too short and you can only see _Lazarus ..._. Enable this to show the project name first. This can be useful when starting multiple instances of the IDE to work on several different projects at the same time. Since 0.9.29. 

Window positions

Fine tune the positions of the IDE windows.   


## IDE CoolBar

[![IDE-options-Environment-IDE CoolBar.JPG](https://wiki.freepascal.org/images/2/2d/IDE-options-Environment-IDE_CoolBar.JPG)](</File:IDE-options-Environment-IDE_CoolBar.JPG>)

Coolbar is visible

If this checkbox is ticked, then the coolbar will be displayed in the IDE. 

Coolbar width

Sets the width of the coolbar. If you add more buttons, you will need to increase this value. 

Toolbar grab style

Determines the style of the left-hand coolbar grabbers. 

Grab width

Sets the width of the collbar grabbers. 

Toolbar border style

Determines whether there is a line between toolbars (default). 

  
The **Restore defaults** button restores the above settings to their defaults. 

* * *

  


The **Add** button inserts a new toolbar. 

The [**Configure**](<IDE_Window__Toolbar_Config.md> "IDE Window: Toolbar Config") button allows you to add and remove items from a toolbar. 

The **Delete** button deletes the selected toolbar. 

  
The **Restore defaults** button restores the above settings to their defaults.   


## Editor ToolBar

[![IDE-options-Environment-Editor ToolBar.JPG](https://wiki.freepascal.org/images/d/d7/IDE-options-Environment-Editor_ToolBar.JPG)](</File:IDE-options-Environment-Editor_ToolBar.JPG>)

  


## Component Palette

[![IDE-options-Environment-Component Palette.JPG](https://wiki.freepascal.org/images/7/7f/IDE-options-Environment-Component_Palette.JPG)](</File:IDE-options-Environment-Component_Palette.JPG>)

  


## Form Editor

[![IDE-options-Environment-Form Editor.JPG](https://wiki.freepascal.org/images/5/5e/IDE-options-Environment-Form_Editor.JPG)](</File:IDE-options-Environment-Form_Editor.JPG>)

Colors

  * **Grid color** \- the color of the grid points
  * **Grabber color** \- the color of the 8 small boxes used to resize a selection of controls.
  * **Guide lines Left, Top** \- the color for left, top aligned guide lines.
  * **Guide lines Right, Bottom** \- the color for right, bottom aligned guide lines.
  * **Marker color** \- the color of the 4 small boxes on each control in a multi selection.
  * **Rubberband selection** \- the color of a selection rubber band: Left click on the designer form client area, then drag the mouse keeping pressed. Release the left button and all intersecting controls are selected. Pressing shift to add/remove from/to the current selection.
  * **Rubberband creation** \- the color of a creation rubber band. Select a control in the component palette. Then drag a rectangle on a designer form. The new control will be created with this size.



Grid

  * **Show grid** \- show a grid of points on each designer window. All client areas, which accept controls have this grid.
  * **Show border spacing** \- Show rectangles around each control showing the borderspacing. This is quite slow.
  * **Snap to grid** \- When moving or resizing components on the designer round the movement to the nearest grid point.
  * **Grid Size X** \- The horizontal distance of the grid points
  * **Grid Size Y** \- The vertical distance of the grid points.



Guide lines

The guide lines are the lines, showing to which controls the current selection is currently aligned. 

  * **Show Guide lLines** \- paint guide lines
  * **Snap to Guide Lines** \- snap to nearest guide line during moving/resizing of controls on the designer form.



Miscellaneous

  * **Select grandchildren** \- when multiple controls are selected by the “rubber band method” (dragging a rectangle) and the rubber band touches children of a control, i.e. grand children of the control where the drag started, then these controls will be included in the selection if this option is checked.
  * **Show captions of non-visual components** \- Shows the component name of non-visual components along below their palette icon.
  * **Show designer hints** \- Show hints of the component position and size, when mouse is over a control on a designer form.
  * **Open designer on open unit** \- Automatically open the designer form, when opening a pascal unit file. Useful for newbies, so enabled on default.
  * **Right click selects** \- right click selects and then shows the popup menu.
  * **Reduce designer painting** \- the IDE will use smart InvaldidateRect calls instead of full redraws. This is faster, but does not work with all widgets of all widgetsets. Use this to reduce artefacts on the designer forms.
  * **Ask name on create** \- Prompts for the component name and caption (if available) when a new component is dropped in the designer form.
  * **Switch to Object inspector Favorites tab** \- Automatically switches to the object inspector’s Favorites tab when a new component is dropped on the form. This way the user sees only the most-often-used properties. Can only be changed when the option “Ask name on create” is checked.
  * **Check packages on form create** -



  


## Object Inspector

[![IDE-options-Environment-Object Inspector.JPG](https://wiki.freepascal.org/images/9/9e/IDE-options-Environment-Object_Inspector.JPG)](</File:IDE-options-Environment-Object_Inspector.JPG>)

Colors

Colors for various property items. 

Miscellaneous

  * Item Height - the default height of each property row. Not all widgetsets support resizing the edit and comboboxes. For example the win32 interface does not allow setting the combobox heights.



Options

  * Auto show
  * Show component tree
  * Show information box
  * Show status bar
  * Show Hints - show hints in object Inspector when moving the mouse over a property row.
  * Bold non default values
  * Show gutter
  * Draw grid lines



  


## FPDoc Editor

[![IDE-options-Environment-FPDoc Editor.JPG](https://wiki.freepascal.org/images/2/2f/IDE-options-Environment-FPDoc_Editor.JPG)](</File:IDE-options-Environment-FPDoc_Editor.JPG>)

FPDoc files path

Contains paths to FPDoc files with documentation for the units not documented in a project. You can also add paths to the RTL's (*.xml) documentations files, so that tooltip help works for both RTL code.   


## Backup

[![IDE-options-Environment-Backup.JPG](https://wiki.freepascal.org/images/b/bb/IDE-options-Environment-Backup.JPG)](</File:IDE-options-Environment-Backup.JPG>)

The IDE can create backup files each time it saves a file and an old file exists already. 

The options allow to setup the extension and place of backup files. 

**Hint** : Normally it is better to use a version control system like svn or cvs for backups and to disable backups here.   


## Naming

[![IDE-options-Environment-Naming.JPG](https://wiki.freepascal.org/images/5/53/IDE-options-Environment-Naming.JPG)](</File:IDE-options-Environment-Naming.JPG>)

Default pascal extension

Whenever the IDE creates a pascal unit, it uses this extension as default. The .pas is typical for Delphi users, while .pp is often used for FPC sources and .p is known on Mac computers. 

**Note** : Custom projects can create pascal sources with defined extension. The IDE will not override this. 

Ambiguous file action

Before compile the IDE checks if there are ambiguous files. For example it checks if a unit exists twice in the unit path. The compiler will use the first it finds, which might not the intended thing and this gives strange errors. Here you can set the action, what should happen, if the IDE finds such a file. 

  * Ask
  * Auto delete file
  * Auto rename lowercase
  * Warn on compile
  * Ignore



Save as - auto rename pascal files lowercase

Source code, that compiles on platforms with case sensitive filenames must make sure, that units are written correct. FPC searches first for the unit name with the uses case, then lowercase and finally uppercase. You can either make sure, that the case is written correct in all units, which is hard for windows users, as they don't need to. Or you can let the IDE save all new units lowercase. The source editor will show the unit name as written in the source code, so you see the right case in the IDE anyway. 

  * Ask
  * Auto rename lowercase
  * no automatic renaming



When a unit is renamed, update references

  * Always
  * Ask
  * Never



  


## File Filters

[![IDE-options-Environment-File Filters.JPG](https://wiki.freepascal.org/images/1/10/IDE-options-Environment-File_Filters.JPG)](</File:IDE-options-Environment-File_Filters.JPG>)

The **File / Open File** dialog has a list of filters, that can be configured here. Right click on the grid to add and remove items. (Since 0.9.31) 

The IDE always adds a few filters: 

  * prepends a filter that sums up all filters
  * appends a filter for all file types of the open files in the source editor, that are not yet in the list. For example if you have a text.txt file open, this filter will contain a *.txt.
  * appends an any files filter (*.*)

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Environment_Options](https://web.archive.org/web/20241115090200/https://wiki.freepascal.org/IDE_Window%3A_Environment_Options)_
