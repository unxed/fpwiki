# IDE Window: Project Inspector

│ **English (en)** │  **[русский (ru)](<../ru/IDE_Window__Project_Inspector.md>)** │

This is a floating window showing all files and dependencies of the project. 

## Contents

  * 1 Open
  * 2 Add
  * 3 Remove
  * 4 Options
  * 5 Popup menu
    * 5.1 Disable I18N for lfm
    * 5.2 Open loaded package
    * 5.3 Remove dependency
    * 5.4 Move dependency up
    * 5.5 Move dependency down
    * 5.6 Store file name as default for this dependency
    * 5.7 Store file name as preferred for this dependency
    * 5.8 Clear dependency filename
    * 5.9 Remove non existing files



## Open

Open the selected file or package. 

## Add

Add a file or dependency. 

## Remove

Remove a file or dependency. 

## Options

Opens the [Project Options](<IDE_Window__Project_Options.md> "IDE Window: Project Options"). 

## Popup menu

Note that some items only appear when right clicking on a file or dependency. 

### Disable I18N for lfm

Check this to disable the automatic update of the .po file for the strings of the lfm of the unit. For the general options see [project options](<IDE_Window__Project_Options.md> "IDE Window: Project Options"). 

### Open loaded package

Open the package editor of this dependency. 

### Remove dependency

### Move dependency up

Change order of dependencies. The order is used by the IDE, but you should not rely on it. This feature exists mostly for aesthetic reasons. 

### Move dependency down

Same as above, but one position down. 

### Store file name as default for this dependency

Store the relative filename to the .lpk file in the project (.lpi) file to use as default. When the project is copied to another computer and the project is opened, the IDE searches for all needed packages. As last default it uses this filename. 

[![SetDefaultPkDependency1.png](https://wiki.freepascal.org/images/c/c8/SetDefaultPkDependency1.png)](</File:SetDefaultPkDependency1.png>)

### Store file name as preferred for this dependency

Store the relative filename to the .lpk file in the project (.lpi) file to use as preferred. When the project is opened the IDE opens the given package file. If a package with the same name was already open, it will be replaced by the preferred one. This function exists since 0.9.29. 

### Clear dependency filename

Clears the default filename for this dependency set by the above 'Store dependency filename'. 

### Remove non existing files

Remove all files from the project that do not exist in the file system. This function exists since 0.9.29.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Project_Inspector](https://web.archive.org/web/20250406084044/https://wiki.freepascal.org/IDE_Window%3A_Project_Inspector)_
