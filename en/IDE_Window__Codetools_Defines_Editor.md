# IDE Window: Codetools Defines Editor

│ **English (en)** │

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** These values are only used by the codetools, not by the compiler. To setup search paths use the [Environment options](<IDE_Window__Environment_Options.md> "IDE Window: Environment Options"), [Compiler Options](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options"), [Package Editor](<IDE_Window__Package_Editor.md> "IDE Window: Package Editor") and [Project Inspector](<IDE_Window__Project_Inspector.md> "IDE Window: Project Inspector"). The IDE automatically creates codetools nodes for those values. 

This dialog is mainly for debugging, when codetools parse different than the compiler. Or to setup the Delphi sources in the Lazarus IDE. 

[![Codetools Defines Editor.png](https://wiki.freepascal.org/images/e/e3/Codetools_Defines_Editor.png)](</File:Codetools_Defines_Editor.png>)

## Contents

  * 1 Introduction
  * 2 Tree - predefined nodes
    * 2.1 Free Pascal Compiler
    * 2.2 Free Pascal Sources
    * 2.3 Lazarus Sources
    * 2.4 Packages
  * 3 Node types
    * 3.1 Block
    * 3.2 Directory
    * 3.3 Define
    * 3.4 Define Recurse
    * 3.5 Undefine
    * 3.6 Undefine Recurse
    * 3.7 Undefine All
    * 3.8 IfDef
    * 3.9 IfNDef
    * 3.10 If
    * 3.11 ElseIf
    * 3.12 Else
  * 4 Menu
    * 4.1 Exit - Save and Exit
    * 4.2 Exit - Exit without Save
    * 4.3 Edit - Move node up
    * 4.4 Edit - Move node down
    * 4.5 Edit - Move node one level up
    * 4.6 Edit - Move node one level down
    * 4.7 Edit - Insert node below
    * 4.8 Edit - Insert node as child
    * 4.9 Edit - Delete node
    * 4.10 Edit - Convert node
    * 4.11 Tools - Open Preview
    * 4.12 Insert Template
  * 5 Treeview
  * 6 Selected node
    * 6.1 Name
    * 6.2 Description
    * 6.3 Value
    * 6.4 Value as Text
    * 6.5 Values as File Paths



## Introduction

The codetools provide functions to parse pascal sources - FreePascal, Delphi, Kylix. To find the pascal sources they need the search paths and to parse the IFDEFs they need the compiler flags. These values are defined by rules - the **define templates**. 

For every directory the codetools manage a set of values. That means every unit in a directory share the same values (paths and flags). That's why you should **only** put two projects/packages into one directory if they use the same settings. On the other hand this system allows to handle an arbitrary number of distinct sources at the same time. For example you can have Delphi and FreePascal sources. The Delphi sources will use the Delphi system.pas, while the FPC sources will use the FPC system.pp. Or you can have two different FPC versions. Or several FPC cross sources (For example for linux and windows). You do not need to restart the IDE or recompile anything for that. Note: The compiler of course can only handle one type of sources. 

This dialog allows to explore and edit the define templates. 

All values and variables can contain macros. A variable that was assigned an empty value _is still defined. That means 'IFDEF variable' will still result in 'true'. To purge a variable use 'Undefine'._

## Tree - predefined nodes

### Free Pascal Compiler

This node provides global definitions, provided by the currently used compiler set in the project's compiler options. They are updated if you change the compiler in the compiler options (e.g. IDE loads a project) or 'Rescan FPC source directory'. they are not automatically updated when you install another FPC while the IDE is running. They are auto generated and can not be changed. 

### Free Pascal Sources

This node provides definitions for the Free Pascal source directory, defined in the environment options. The nodes are auto created by some fix wired heuristics in the codetools (components/codetools/definetemplates.pas function CreateFPCSrcTemplate) and can not be changed in the dialog. They are updated when the FPC source directory is changed (e.g. in options or on IDE start) or the Target CPU or OS changes or by 'Rescan FPC source directory'. They are not updated, when files are changed on disk while the IDE is running. 

### Lazarus Sources

This node provides definitions for the Lazarus source directory, defined in the environment options. The nodes are auto created by some fix wired heuristics in the codetools (components/codetools/definetemplates.pas function CreateLazarusSrcTemplate) and can not be changed in the dialog. They are updated when the Lazarus source directory is changed (e.g. in options or on IDE start). They are not updated, when files are changed on disk while the IDE is running. 

### Packages

The IDE creates on node for each package. At the moment the nodes of the packages LCL, IDEIntf, SynEdit and Codetools are special packages and defined by the 'Lazarus Sources'. 

## Node types

The codetools parse the nodes from top to bottom. Some nodes like _Directory_ , _IF_ , _IFDEF_ , ... control if their child nodes are parsed or not. 

The tree shows two icons for each node. The first is the node type (Block, IF, IFDEF, ...). The second icon shows a 'computer', if the node was auto created and can not be altered. 

### Block

This node type exists only to group nodes. 

### Directory

To create nodes only valid for a specific directory use this node type. The child nodes are only parsed for the directory specified in **Value**. **Define** and **Undefine** nodes will set/clear variables only for this directory, not for the sub directories. To set/clear variables for sub directories, create a child node of type **Directory** or use the **Define Recurse** , **Undefine Recurse**. 

### Define

Sets a flag/value for the current directory. Examples: 

  * Set Name to 'Def1'. Now IFDEF Def1 will be true and in source a {$IFDEF Def1} will be parsed by the codetools.
  * Set Name to 'Def1' and Value to '123'. Same as above, but addionally IF Def1=123 gives true.
  * Set Name to 'Def1' and Value to '$(Def1);bogus'. This will append ';bogus' to the value of Def1.



### Define Recurse

Same as **Define** , but for the current directory and all sub directories. 

### Undefine

Purges a variable form the current set of definitions. 

### Undefine Recurse

Same as **Undefine** , but for the current directory and all sub directories. 

### Undefine All

Same as **Undefine Recurse** , but purges all variables. Useful for directories that build their own world. For example the FPC source. 

### IfDef

If the Variable is defined the child nodes are executed. 

### IfNDef

If the Variable is **not** defined the child nodes are executed. 

### If

Executes the expression in Value and if the result is '1' then the child nodes are executed. The syntax is the same as the compiler. For example 'defined(avariable)' returns '1' if the avariable is defined, otherwise '0'. Examples: 

  * Set 'Value' to 'defined(linux) and defined(i386)'
  * To check for a specific FPC version: 'VER2_0 or VER2_2'. The variables will be replaced by their values. Empty or not defined values result in '0'.
  * To compare the values of two variables use: '$(#TargetOS)'='$(#SrcOS)'. IMPORTANT: If you forget the apostrophes, the following will happen: First the macros will be replaced, which results in linux=unix, which is further replaced by the values of the variables: 1=1, which always results to true.



### ElseIf

If all former If resulted in false, then the ElseIf or Else node is executed. The ElseIf is the same as an Else If. 

### Else

If all former If or ElseIf resulted false, then this node is executed. 

## Menu

### Exit - Save and Exit

Save changes and close dialog. 

### Exit - Exit without Save

Discard changes and close dialog. 

### Edit - Move node up

Move selected node in front of next upper sibling. 

### Edit - Move node down

Move selected node below next lower sibling. 

### Edit - Move node one level up

Move selected node to parent node. 

### Edit - Move node one level down

Make selected node a child of the previous sibling. 

### Edit - Insert node below

Create a new node and insert it below selected node. 

### Edit - Insert node as child

Create a new node and insert it as child of selected node. 

### Edit - Delete node

Delete selected node. 

### Edit - Convert node

Change type of selected node. 

### Tools - Open Preview

Open the [Codetools Directory Values](<IDE_Window__Codetools_Directory_Values.md> "IDE Window: Codetools Directory Values"). 

### Insert Template

Create a new node from the templates and insert it below selected node. 

## Treeview

Select a node and view the details below. 

## Selected node

### Name

### Description

### Value

### Value as Text

Shows the value. 

### Values as File Paths

Splits the value for semicolons and shows each path on a separate line.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Codetools_Defines_Editor](https://web.archive.org/web/20240121085830/https://wiki.freepascal.org/IDE_Window%3A_Codetools_Defines_Editor)_
