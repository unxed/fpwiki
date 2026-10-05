# Project Groups

## Contents

  * 1 Install
  * 2 Features
    * 2.1 Menu items
    * 2.2 Editor
      * 2.2.1 Editor toolbar
      * 2.2.2 Popup menu
  * 3 Concepts
    * 3.1 Projects
    * 3.2 Package
    * 3.3 Pascal file
    * 3.4 Nested Project Group
  * 4 Other features
  * 5 Bugs / ToDos



## Install

The package is available since Lazarus 1.5. 

  * Go to Package > Install/Uninstall Packages.
  * Search in **Available for installation _for_** _lazprojectgroups'_.
  * Select it.
  * Click on **Install selection**.
  * Save and rebuild IDE. The IDE will ask for confirmation, then it build the package, links it in and restarts itself.



## Features

### Menu items

  * New project group - creates a new project group with the active project as target. Same function as the editor **More / New**.
  * Open project group - opens a dialog to select an .lpg file
  * Open recent project group - list of recently opened groups
  * Save project group - same function as the editor **Save** button.
  * Saves project group as - same function as the editor **More / Save As** button.



### Editor

[![ProjectGroupsEditor2.png](https://wiki.freepascal.org/images/d/da/ProjectGroupsEditor2.png)](</File:ProjectGroupsEditor2.png>)

#### Editor toolbar

  * **Save** \- saves the .lpg including the .lpg files of nested project groups.
  * **Add**
    * **Add from file** \- Add projects (*.lpi), packages (*.lpk), Groups (*.lpg), single Pascal mfile (*.pas, *.pp) or scripts (e.g. *.sh, *.bat)
    * **Add current project** \- Add active project to project group
  * **Remove** \- removes target from the list and puts it into **removed targets**.
  * **Compile** \- compiles the selected target: 
    * Project/Build Mode: if the project and build mode is active it calls the IDE function, otherwise it uses lazbuild to compile the project. lazbuild automatically compiles outdated packages used by the project - same as the IDE.
    * Package: ompile using lazbuild. It uses lazbuild to compile a package in order to build independent of any project. It uses the target OS and CPU of lazbuild. lazbuild automatically compiles outdated packages used by the package.
    * Pascal module: builds the file using the IDE directives.
    * Project Group: builds all targets of the project group
    * Script: runs the script
  * **Compile Clean** \- same as **Compile** , except it passes -B to lazbuild when compiling, telling FPC to rebuild every unit it finds sources
  * **Earlier** \- moves the target one position up
  * **Later** \- moves the target one position down
  * **Reload** \- reloads the current .lpg, rebuilding the whole tree.
  * **More**
    * **New** \- create a new project group. Closes the old and opens the new.
    * **Save As** \- saves the project group as a different file.
    * **Undo** \- undo the last Delete
    * **Redo** \- not yet implemented
    * **Options** \- Opens the options frame. Same as Tools / Options / Environment / Project Groups
    * **Info** \- Opens a dialog showing some information, e.g. all source paths of the project group



#### Popup menu

Right clicking on a node gives the following menu items depending on the selected node: 

  * **Copy filename** \- copies the filename of the selected target to the clipboard
  * **Run** \- Open the target in the IDEthis loads the project in the IDE
  * **Compile**
  * **Compile clean**
  * **Compile from here** \- compiles this target and all targets below.
  * **Remove**
  * **Earlier**
  * **Later**
  * **Open** \- same as double click 
    * Project: this opens the project in the IDE (closing the currently active).
    * Package: opens the package editor of the package.
    * Pascal file: opens it in the source editor
  * **Properties**
    * Project: as open, except it also opens the Project Options.
    * Package: opens the package editor of the package.
    * Pascal file: opens the dialog to setup the IDE directives for Build and Run.



## Concepts

### Projects

  * The IDE has only one active project. The active project defines the active target OS, CPU and processor, the loaded packages, search paths and other compiler flags. And the active project has exactly one active build mode.
  * When compiling the active project build mode the normal IDE function _DoBuildProject_ is called.
  * When compiling another build mode of the active project the active build mode is switched and _DoBuildProject_ is called.
  * When compiling a non active project the external tool **lazbuild** is called and output is shown in the Messages window. Hints/Warnings/Errors of files outside the active project or its packages might be interpreted wrong by the IDE.
  * Note: You can add the same project multiple times. For example add _ProjectA_ with build mode _Debug_ active, then _ProjectA_ again, except with build mode _Release_ active.



### Package

  * A package is affected by the active project, for example the project defines the target OS and CPU.
  * When compiling a package the tool _lazbuild_ is called to make sure the active project has no effect. _lazbuild_ uses the target OS/CPU it was compiled with.



### Pascal file

  * If you want to see output of the **Run** command, add the following directive to the file:


    
    
    {%RunFlags Messages+}
    

  * Hint: You can use instantfpc scripts to write Pascal scripts to do arbitrary things, like creating a zip, update documentation, install, uploading files. See [InstantFPC](<InstantFPC.md> "InstantFPC")



Example for an instantfpc script: 
    
    
    #!/usr/bin/instantfpc
    {%BuildCommand instantfpc --skip-run -B -gl myscript.pas}
    {%RunCommand instantfpc -gl myscript.pas}
    {%RunFlags Messages+}
    begin
      writeln('Running MyScript ...');
    end.
    

### Nested Project Group

You can add a project group as a sub target of the project group. 

## Other features

  * By default if the project group editor was open when closing the IDE, the editor will reopen on next start with the last open group. You can disable this in the options.
  * You can redirect Compile/Build to compile the selected target in the project group editor. See Tools / Options / Project Groups / "Compile" and "Build" compile target
  * Option "Show target paths": Disable: show file names without path, Enable: show file names with paths relative to directory of lpg
  * Find in files now has the option to search in all files in the currently loaded project group



## Bugs / ToDos

  * Not yet updating menu items enabled state.
  * Changes in the IDE are not yet updating the project group editor. After altering you need to click **Reload**.
  * Changes on disk are not yet recognized. Use **Reload**.
  * Show active build mode, active project
  * "New" button to create a package/project/file and add to project groups
  * clean function, like the Run / Clean up and build dialog
  * drag and drop within the editor 
    * reorder targets
    * move targets between sub groups
    * move file to another project/package
  * save session in project group, allowing to quickly switch the active project
  * load sub projects in IDE to use code navigation for files not in the active project
  * find references in files

---

_Source: [https://wiki.freepascal.org/Project_Groups](https://web.archive.org/web/20250512124710/https://wiki.freepascal.org/Project_Groups)_
