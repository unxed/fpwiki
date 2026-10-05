# New IDE features since

This page contains a list of major IDE features that made Lazarus even more impressive. All features are sorted by the Lazarus release version that they became available in. 

If you would like to contribute to this page, please read the following page ([Creating animated gifs for wiki](<Creating_animated_gifs_for_wiki.md> "Creating animated gifs for wiki")) giving hints on creating animated gifs for this wiki. 

## Contents

  * 1 Overview
  * 2 prior to v0.9.4 (2004 and earlier)
    * 2.1 form designer guidelines (June 2002)
  * 3 (2005-01-09)
  * 4 v0.9.10 (2005-10-06)
    * 4.1 Integrated Documentation Editor
  * 5 v0.9.13 (2006-02-03)
    * 5.1 Quick Fixes
  * 6 v0.9.17 (2006-05-27)
  * 7 v0.9.18 (2006-09-18)
    * 7.1 procedure list
  * 8 v0.9.20 (2006-11-05)
    * 8.1 divider lines in editor
  * 9 v0.9.24 (2007-11-05)
    * 9.1 editor toolbar
  * 10 v0.9.26 (2008-10-25)
    * 10.1 fpdoc help inside editor tooltips
  * 11 v0.9.28 (2009-10-12)
    * 11.1 block matching
    * 11.2 code macro OfAll
    * 11.3 Code Templates with macro $Param
    * 11.4 Syncron-Edit
  * 12 v0.9.30 (2011-03-28)
    * 12.1 Multi Source Editor
    * 12.2 Docked IDE
    * 12.3 Code folding with hiding option for Comments
  * 13 v1.0 (2012-08-29)
    * 13.1 Folding (Hiding) text by selection
    * 13.2 Watches/Locals History in debugger
    * 13.3 Macro Recorder for Code Editor
    * 13.4 Procedure/Class-Name Hint for long declarations
  * 14 v1.2 (2014-03-04)
    * 14.1 Command Macros can be saved
    * 14.2 PascalScript macros
    * 14.3 Multiple user defined word highlight/markup
    * 14.4 Package Graph Control
    * 14.5 Dialog to find and remove unneeded package dependencies
    * 14.6 Auto-continue for comments and strings
    * 14.7 Project Additions and Overrides
    * 14.8 Low-lighting inactive $IFDEF code
    * 14.9 Unit Dependencies
    * 14.10 Quick Fix for circular unit not found
    * 14.11 Find references of used unit
  * 15 v1.4 (2015-04-22)
    * 15.1 Designer supports Undo
    * 15.2 Drag and Drop refactoring of files in project and packages
    * 15.3 Parallel building packages
    * 15.4 Message Marks
    * 15.5 Messages windows shows translated messages
    * 15.6 Messages windows allows to search with highlighting
    * 15.7 Hint windows and Object Inspector info box have HTML rendering by default
    * 15.8 Editor Toolbar is visible by default
    * 15.9 Checkbox can be used as a Boolean property editor in OI
    * 15.10 Component palette is configurable
  * 16 v1.6
    * 16.1 Multi Caret
    * 16.2 Project Groups
    * 16.3 Desktops
    * 16.4 IDE CoolBar
    * 16.5 Package Tabs
    * 16.6 Favorites
  * 17 v1.8
    * 17.1 Outline for Pascal Sources
  * 18 Unsorted
    * 18.1 built-in diff viewer
    * 18.2 refactoring
    * 18.3 favorites in Object Inspector
    * 18.4 integrated unit testing with FPCUnit
    * 18.5 better anchoring and margins in form designer
    * 18.6 file browser
    * 18.7 CodeTools
    * 18.8 Include files
    * 18.9 Lazarus Packages
    * 18.10 Enclose Selection
    * 18.11 Quick Fix: Remove unit
    * 18.12 Code Observer
    * 18.13 Line Change indicators



# Overview

Version| Category| Feature  
---|---|---  
v0.9.4 (and prior, 2004 and earlier)| Designer|  form designer guidelines (June 2002)  
v0.9.10 (2005-10-06)| Doc|  Integrated Documentation Editor  
v0.9.13 (2006-02-03)| Compilation|  Quick Fixes  
v0.9.18 (2006-09-18)| Code managment|  procedure list  
v0.9.20 (2006-11-05)| Editing|  divider lines in editor  
v0.9.24 (2007-11-05)| Editing|  editor toolbar  
v0.9.26 (2008-10-25)| Doc|  fpdoc help inside editor tooltips  
v0.9.28 (2009-10-12)| Editing|  block matching  
v0.9.28 (2009-10-12)| Editing|  code macro OfAll  
v0.9.28 (2009-10-12)| Editing|  Code Templates with macro $Param  
v0.9.28 (2009-10-12)| Editing|  Syncron-Edit  
v0.9.30 (2011-03-28)| Editing|  Multi Source Editor  
v0.9.30 (2011-03-28)| IDE|  Docked IDE  
v0.9.30 (2011-03-28)| Editing|  Code folding with hiding option for Comments  
v1.0 (2012-08-29)| Editing|  Folding (Hiding) text by selection  
v1.0 (2012-08-29)| Debugging|  Watches/Locals History in debugger  
v1.0 (2012-08-29)| Editing|  Macro Recorder for Code Editor  
v1.0 (2012-08-29)| Editing|  Procedure/Class-Name Hint for long declarations  
v1.2 (2014-03-04)| Editing|  Command Macros can be saved  
v1.2 (2014-03-04)| Editing|  PascalScript macros   
v1.2 (2014-03-04)| Editing|  Multiple user defined word highlight/markup  
v1.2 (2014-03-04)| IDE|  Package Graph Control  
v1.2 (2014-03-04)| Code managment|  Dialog to find and remove unneeded package dependencies  
v1.2 (2014-03-04)| Editing|  Auto-continue for comments and strings  
v1.2 (2014-03-04)| Compilation|  Project Additions and Overrides  
v1.2 (2014-03-04)| Editing|  Low-lighting inactive $IFDEF code  
v1.2 (2014-03-04)| Compilation|  Unit Dependencies  
v1.2 (2014-03-04)| Compilation|  Quick Fix for circular unit not found  
v1.2 (2014-03-04)| Code managment|  Find references of used unit  
v1.4 (to be announced)| Compilation|  Message Marks  
v1.4 (to be announced)| Compilation|  Messages windows shows translated messages  
v1.4 (to be announced)| Compilation|  Messages windows allows to search with highlighting  
v1.4 (to be announced)| IDE|  Hint windows and OI info box have HTML rendering by default  
v1.4 (to be announced)| Editing|  Editor Toolbar is visible by default  
v1.4 (to be announced)| Designer|  Checkbox can be used as a Boolean property editor in OI  
v1.4 (to be announced)| Designer|  Component palette is configurable  
v1.5 (to be announced)| Editing|  Multi Caret  
Unsorted| IDE|  built-in diff viewer  
Unsorted| Code managment|  refactoring  
Unsorted| Designer|  favorites in Object Inspector  
Unsorted| IDE|  integrated unit testing with FPCUnit  
Unsorted| Designer|  better anchoring and margins in form designer  
Unsorted| IDE|  file browser  
Unsorted| Code managment|  CodeTools  
Unsorted| IDE|  Include files  
Unsorted| IDE|  Lazarus Packages  
Unsorted| Editing|  Enclose Selection  
Unsorted| Compilation|  Quick Fix: Remove unit  
Unsorted| Code managment|  Code Observer  
Unsorted| Editing|  Line Change indicators  
  
# prior to v0.9.4 (2004 and earlier)

## form designer guidelines (June 2002)

The Form Designer shows green or blue guidelines when you position components. This help you align or size components correctly. When components are correctly aligned or sized, then guidelines are shown. 

[![Ide features design guidelines.gif](https://wiki.freepascal.org/images/2/2e/Ide_features_design_guidelines.gif)](</File:Ide_features_design_guidelines.gif>)

# (2005-01-09)

Invert Assignment tool 

# v0.9.10 (2005-10-06)

## Integrated Documentation Editor

Lazarus IDE is now capable of editing the external fpdoc help (xml) files while you are working with the source code. The FPDoc Editor can be found at: **View - > FPDoc Editor** menu option. As you navigate your code, the FPDoc Editor window gets updated with the related documentation. External help files prevent you from obfuscating your source code. fpdoc is similar to the JavaDoc technology, but with its own features and syntax. 

[![Ide features fpdoc editor.gif](https://wiki.freepascal.org/images/3/30/Ide_features_fpdoc_editor.gif)](</File:Ide_features_fpdoc_editor.gif>)

# v0.9.13 (2006-02-03)

## Quick Fixes

  * Quick Fix for improving message: Unit not found
  * Quick Fix for Sender not used



# v0.9.17 (2006-05-27)

  * Quick Fix for some linker errors



# v0.9.18 (2006-09-18)

## procedure list

This makes finding methods in your current unit really fast! It can be found at **Search > Procedure list** (Hotkey is Alt+G). The procedure list enables you to view a list of Free Pascal / Lazarus procedures in the current unit and quickly jump to the implementation. Include files are also supported and the method list gets filtered as you type the partial method name. 

[![Ide features procedure list.gif](https://wiki.freepascal.org/images/2/2c/Ide_features_procedure_list.gif)](</File:Ide_features_procedure_list.gif>)

# v0.9.20 (2006-11-05)

## divider lines in editor

The Lazarus editor supports drawing horizontal divider lines between classes, methods, nested procedures, begin..end blocks and many more. So now there is no need to add extra blank lines or {*******...} comments between classes or methods. In the upcoming v0.9.28 release this feature is very customizable in the "**Environment > Options > Editor > Divider Drawing**" screen. In the following screenshot it shows some divider lines using the default color and a divider line for nested procedures using a customized color. 

[![Ide features divider lines.png](https://wiki.freepascal.org/images/3/37/Ide_features_divider_lines.png)](</File:Ide_features_divider_lines.png>)

# v0.9.24 (2007-11-05)

  * Quick Fix for fpc warning constructing a class with abstract methods



## editor toolbar

The editor toolbar plug-in allows you to define toolbar buttons to any of the IDE menu items. 

[![Ide features editor toolbar.png](https://wiki.freepascal.org/images/1/16/Ide_features_editor_toolbar.png)](</File:Ide_features_editor_toolbar.png>)

# v0.9.26 (2008-10-25)

## fpdoc help inside editor tooltips

Lazarus can now display fpdoc documentation inside the tooltip help. Packages and Project can all be setup to find the related fpdoc (*.xml) help files, which then enables Lazarus to display the extended help in the tooltips. Simply hover the mouse over various parts of the code to see the tooltips in action. In the image below the mouse was hovered over the TCollection class definition. 

[![Ide features fpdoc tooltip.png](https://wiki.freepascal.org/images/7/7f/Ide_features_fpdoc_tooltip.png)](</File:Ide_features_fpdoc_tooltip.png>)

# v0.9.28 (2009-10-12)

## block matching

The IDE automatically highlights matching blocks - begin..end pairs. 

[![block matching.gif](https://wiki.freepascal.org/images/8/8d/block_matching.gif)](</File:block_matching.gif>)

## code macro OfAll

The code templates of the IDE can use code macros. Writing code macros is easy. The new code macro _$OfAll()_ creates a list of enums for case statements as demonstrated in the image below: 

[![CodeMakroOfAll.gif](https://wiki.freepascal.org/images/0/0b/CodeMakroOfAll.gif)](</File:CodeMakroOfAll.gif>)

(Hint: The separator line between text and folder marks can be enabled via [Margin and gutter](<IDE_Window__Editor_Options.md> "IDE Window: Editor Options")) 

New users will automatically get the new macro. Old users can either delete their ~/.lazarus/lazarus.dci or add the template themselves. How to create the template: 

  * IDE menu / Environment / code templates
  * Add. Token: "ofall" or whatever you like. Comment: "case variable of <list of all enums> end"
  * Click ok to add it
  * The new template is selected below.
  * check "Enable macros"
  * The code of the template:


    
    
    of
    |$OfAll()end;
    

  * click ok to save and close the templates.



## Code Templates with macro $Param

[![Image-Ide features trycf demo.gif](https://wiki.freepascal.org/images/2/2f/Image-Ide_features_trycf_demo.gif)](</File:Image-Ide_features_trycf_demo.gif>)

To use this feature add the following macro (or delete your old config): 
    
    
    $param(variable) := $param(typename).Create;
    try
      |
    finally
      $param(variable,sync=1).Free;
    end;
    

## Syncron-Edit

[![SynEditSyncro.gif](https://wiki.freepascal.org/images/a/a6/SynEditSyncro.gif)](</File:SynEditSyncro.gif>)

# v0.9.30 (2011-03-28)

  * Quick Fix for message: unit1(): fatal: can not find unit2 used by unit3
  * Quick Fix for identifier not found: add local variable
  * Quick Fix for identifier not found: search via Code Browser
  * Quick Fix for unit not found: remove unit from uses section
  * Quick Fix for note local variable not used: remove variable
  * Quick Fix for hints: add IDE directive {%H-}



## Multi Source Editor

Lazarus can now open as many Source-Editor-Windows as you want, and you can freely move files between them. 

It is also possible to view the same file in multiple Windows. 

[![Three src edit medium2.png](https://wiki.freepascal.org/images/4/47/Three_src_edit_medium2.png)](</File:Three_src_edit_medium2.png>)

[[|Multiply Source-Editor-Windows](<http://lazarus-dev.blogspot.com/2010/04/using-multiply-editor-windows.html>)] 

## Docked IDE

The IDE now supports docking. Install either the package [AnchorDockingDsgn](<Anchor_Docking.md> "Anchor Docking") or EasyDockMgrDsgn. 

[![Ide docked.png](https://wiki.freepascal.org/images/a/af/Ide_docked.png)](</File:Ide_docked.png>)

## Code folding with hiding option for Comments

Comments in the source code can be completely folded away (hidden). The context menu (on the fold gutter) provides info on the nested folds at the selected line 

[![FoldHideComments2.png](https://wiki.freepascal.org/images/e/ef/FoldHideComments2.png)](</File:FoldHideComments2.png>)

# v1.0 (2012-08-29)

  * Quick Fix: cant find unit: show ppu of installed package



## Folding (Hiding) text by selection

Lazarus can now fold any text/code segment you want. Just select the text and hide it. (These folds are not remembered in the session, restoring folds on load only works for folds at keywords) 

[![Fold selection.gif](https://wiki.freepascal.org/images/8/8f/Fold_selection.gif)](</File:Fold_selection.gif>)

## Watches/Locals History in debugger

A history of Watches and Locals 

More Info: [[blog](<http://lazarus-dev.blogspot.com/2011/05/remember-remember-history-of-debugging.html>)] [![LazDbgHistory.png](https://wiki.freepascal.org/images/7/7d/LazDbgHistory.png)](</File:LazDbgHistory.png>)

  


## Macro Recorder for Code Editor

The source-code editor can record and play macros 

## Procedure/Class-Name Hint for long declarations

The editor displays an embedded hint with the name of the current Procedure/Class if the top of the declaration is scrolled out 

# v1.2 (2014-03-04)

## Command Macros can be saved

[![Editor macros.png](https://wiki.freepascal.org/images/1/11/Editor_macros.png)](</File:Editor_macros.png>)

Support for multiple command macros, that can be saved with session or IDE config. [IDE_Window:_Editor_Macros](<IDE_Window__Editor_Macros.md> "IDE Window: Editor Macros")

## PascalScript macros

## Multiple user defined word highlight/markup

The user can define any amount of different markup settings, and assign his own choice of words,expressions (one line terms) to them. This allows to highlight all occurrences of one or more identifiers of interest. 

Words to be highlighted can be toggled via keyboard. Words can be highlighted per editor, or across all editors. A list of pre-defined words can be defined, that will be active when the IDE starts. 

[![SynUserDefinedMarkup.png](https://wiki.freepascal.org/images/4/4d/SynUserDefinedMarkup.png)](</File:SynUserDefinedMarkup.png>)

[ See documentation](<IDE_Window__Editor_User_Defined_Words.md> "IDE Window: Editor User Defined Words")

## Package Graph Control

The package graph now shows the package dependencies as graph. 

This new control is part of the package LazControls and used by the IDE itself as well. The control shows a graph consisting of nodes and edges as a level graph (this type of graph is also called a layered graph). 

[![packagegraph1.png](https://wiki.freepascal.org/images/7/78/packagegraph1.png)](</File:packagegraph1.png>)

## Dialog to find and remove unneeded package dependencies

Available via package editor / context menu of a dependency or via the package graph. 

[![cleanuppackagedependencies1.png](https://wiki.freepascal.org/images/3/32/cleanuppackagedependencies1.png)](</File:cleanuppackagedependencies1.png>)

## Auto-continue for comments and strings

## Project Additions and Overrides

Projects can now add compiler options to packages and override the output directories depending on the current build mode. You can also add compiler options to all projects on a machine depending on build mode. 

[Additions and Overrides](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options")

[![CompilerAdditionsAndOverrides1.png](https://wiki.freepascal.org/images/c/cd/CompilerAdditionsAndOverrides1.png)](</File:CompilerAdditionsAndOverrides1.png>)

## Low-lighting inactive $IFDEF code

[![Source Editor IFDEF lowlight.png](https://wiki.freepascal.org/images/7/7e/Source_Editor_IFDEF_lowlight.png)](</File:Source_Editor_IFDEF_lowlight.png>)

## Unit Dependencies

You can now find out what units uses a unit. 

[Unit Dependencies](<IDE_Window__Unit_dependencies.md> "IDE Window: Unit dependencies")

[![UnitDependencies1.png](https://wiki.freepascal.org/images/0/04/UnitDependencies1.png)](</File:UnitDependencies1.png>)

## Quick Fix for circular unit not found

When you create a cycle between units, the compiler only reports one edge of the cycle. Right click on the message / Show unit dependencies will open the unit dependencies dialog with a full path from unit A to unit B to unit A. 

## Find references of used unit

Position source editor cursor on a unit within a uses clause. Then right click / Find / Find references of used unit. This will find all references within the current unit to the used unit. 

# v1.4 (2015-04-22)

## Designer supports Undo

This is one of the coolest improvements in this release. Earlier if you worked on a complex layout of components inside a Panel and then deleted the Panel by accident, there was no way to get it back. Now there is! 

## Drag and Drop refactoring of files in project and packages

Drag and Drop support was added to both Project inspector and Package editor. 

## Parallel building packages

Compilation of packages is now threaded. The Messages window has coloured header lines with information, one header per package. So when your project depends on multiple packages that have to be compiled, the IDE starts multiple threads to compile all those packages. This speeds up the compilation especially for larger projects, like the Lazarus IDE itself. 

Downside is that sections in the Messages window are updated in random order, thus it is more difficult to follow the compilation process. Parallel compilation can be disabled by setting "Maximum parallel processes" to 1. 

## Message Marks

For each compiler message listed in the Messages window the Source Editor shows an icon in the left gutter, a line in the right gutter and the offending token is given a wavey underline. Right-clicking on the left gutter icon shows a context menu with a list of quick fixes. 

The wavey underlines and other marks are only updated after compilation. This differs from the real-time syntax check in new Delphi versions where the wavey underline comes and goes while typing. 

[![MessageMarks.png](https://wiki.freepascal.org/images/3/33/MessageMarks.png)](</File:MessageMarks.png>)

## Messages windows shows translated messages

You can use the FPC provided translation files to show messages in many languages like German and French. 

## Messages windows allows to search with highlighting

Explanation 

## Hint windows and Object Inspector info box have HTML rendering by default

Hints showing suitably formatted text give the IDE a more professional appearance, and the documentation shown usually includes clickable hyperlinks to sources and associated packages. Hint formatting was only available previously if you installed the TurboPowerIProDsgn package. It was not installed by default due to outstanding bugs which are now resolved. 

[![OI Infobox.png](https://wiki.freepascal.org/images/8/87/OI_Infobox.png)](</File:OI_Infobox.png>)

## Editor Toolbar is visible by default

The Editor Toolbar package was improved and is now installed by default. It has preconfigured buttons for "Jump Back" and "Jump Forward" features. Those buttons are often requested by new users. This toolbar makes Lazarus yet more newbie-friendly. It can be hidden from View-menu, thus there is no need to uninstall the package any more. 

[![EditorToolbar.png](https://wiki.freepascal.org/images/0/08/EditorToolbar.png)](</File:EditorToolbar.png>)

## Checkbox can be used as a Boolean property editor in OI

A Checkbox is maybe the most natural control for editing Booleans. It can be configured in Object Inspector environment settings, although the Combobox with "False" and "True" choices is still the default. 

[![OI Checkbox.png](https://wiki.freepascal.org/images/4/47/OI_Checkbox.png)](</File:OI_Checkbox.png>)

## Component palette is configurable

The order of components and pages in the component palette can be changed by user. There is a new options page for it under environment options. 

[![CompPaletteOpts.png](https://wiki.freepascal.org/images/5/5b/CompPaletteOpts.png)](</File:CompPaletteOpts.png>)

# v1.6

  * Quick fix for fpc message _Local variable "i" does not seem to be initialized_ : insert assignment
  * Added TFlowPanel in ExtCtrls.pp



## Multi Caret

The editor allows multiple carets. Those can be set from column mode selection (Shift-Alt-Select), individually (Shift-Ctrl-Left-mouse or Shift-Ctrl-Insert-Key), or from selection (see image) 

[![multicaret.gif](https://wiki.freepascal.org/images/b/b3/multicaret.gif)](</File:multicaret.gif>)

See also [Multi Caret](<IDE_Window__Editor_Options_General.md> "IDE Window: Editor Options General") and [SynEdit MultiCaret plugin](<SynEdit_MultiCaret_plugin.md> "SynEdit MultiCaret plugin")

## Project Groups

[![ProjectGroupsEditor1.png](https://wiki.freepascal.org/images/9/90/ProjectGroupsEditor1.png)](</File:ProjectGroupsEditor1.png>)

See [Project Groups](<Project_Groups.md> "Project Groups"). 

## Desktops

[![desktops.png](https://wiki.freepascal.org/images/d/dd/desktops.png)](</File:desktops.png>)

See [Desktops](<IDE_Window__Desktops.md> "IDE Window: Desktops")

## IDE CoolBar

Replaces the old static Toolbar. ToDo: upload an animation about changing Coolbar layout. 

## Package Tabs

[![packagetabs-top.png](https://wiki.freepascal.org/images/c/c9/packagetabs-top.png)](</File:packagetabs-top.png>)

See [Package Tabs](<Package_Tabs.md> "Package Tabs")

## Favorites

[![favorites.png](https://wiki.freepascal.org/images/e/e3/favorites.png)](</File:favorites.png>)

See [Favorites](<Favorites.md> "Favorites")

# v1.8

  * Quick fix for any fpc hint, note or warning: insert compiler directive {$warn id off}



## Outline for Pascal Sources

The editor can outline pascal code blocks, such as if/then, loops, try/except, begin/end and others. 

Settings can be found under Tools -> Options -> Editor on the pages "Markup and Matches" and "Colors" 

[![editor outline example.png](https://wiki.freepascal.org/images/1/15/editor_outline_example.png)](</File:editor_outline_example.png>) [![editor outline example2.png](https://wiki.freepascal.org/images/e/ec/editor_outline_example2.png)](</File:editor_outline_example2.png>)

# Unsorted

Any items below this point have not been sorted per Lazarus release yet. 

  


## built-in diff viewer

... 

## refactoring

... 

  


## favorites in Object Inspector

... 

## integrated unit testing with FPCUnit

... 

## better anchoring and margins in form designer

... 

## file browser

... 

  


## CodeTools

  * word completion
  * Rename component in Designer auto-renames all instances in unit.



... 

## Include files

... 

## Lazarus Packages

... 

## Enclose Selection

Select some text in editor. Right Click to get popup menu. Select "Refactoring > Enclose Selection". 

## Quick Fix: Remove unit

In messages window 

## Code Observer

... 

## Line Change indicators

...

---

_Source: [https://wiki.freepascal.org/New_IDE_features_since](https://web.archive.org/web/20240208194715/https://wiki.freepascal.org/New_IDE_features_since)_
