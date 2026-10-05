# Lazarus for education

## Contents

  * 1 Overview
  * 2 Installing the package EducationLaz
  * 3 Configuring the IDE
    * 3.1 Component names
    * 3.2 Component palette
    * 3.3 New files with name
    * 3.4 Hiding menu items
    * 3.5 Single file programs
    * 3.6 Code formatting
  * 4 Helpful IDE features
  * 5 Done
  * 6 ToDos
  * 7 Further ideas from non lazarus developers



## Overview

The standard Lazarus IDE is setup for professional developers. This page describes how to setup the IDE for courses and beginners. 

Since 0.9.29 lazarus comes with a package **EducationLaz** which adds some extra option pages to the IDE helping teachers to setup Lazarus for students and pupils. 

## Installing the package EducationLaz

  * Go to _Packages - > Configure installed packages_. A dialog opens.
  * Select **educationlaz** from the right list and click below on _Install selection_ to add it to the left list.
  * You may want to uninstall some packages that are too complex for your students.
  * Click on _Save and rebuild_. The IDE will ask for confirmation.
  * Restart the IDE.
  * There are now some new option pages under Tools -> Options -> Education.



## Configuring the IDE

On the _Environment / Options ... / Education / General_ page you can turn the education mode on and off. 

### Component names

When dropping components on a form, the IDE gives default names (Button1, Button2, ...). This is very useful for rapid design and testing but it does not help beginners to create readable code. There is an option to show a naming dialog under _Environment / Options .. / Environment / Form Editor / Misc / Focus name on create_. 

### Component palette

If you teach basic programming, then the component palette is only distracting for the students. You can hide any or all components on _Environment / Options ... / Education / Component palette_. 

### New files with name

When creating a new unit or a new form the IDE gives a standard name and the file is virtual. This is very useful for experts working with several projects and package at the same time, but it is confusing for beginners. The IDE can show immediately a save dialog. The option for this is _Environment / Options ... / Environment / Naming / Ask for file name on new file_. 

### Hiding menu items

The package EducationLaz provides an option page where every IDE menu item can be hidden. The IDE menus are registered in a tree like structure. You can hide single items or whole categories. There is one safety exception: You can not hide the Environment menu. 

### Single file programs

The concept of projects is too complex for beginners. Beginners starts with simple programs, that consists of only one file. The package EducationLaz adds a new menu item and a new speed button to create _single file programs_. These mini programs are added to the current project, so the students can instantly start to write programs without handling projects. The programs are built and run with the normal functions (and normal shortcuts Ctrl-F9 and F9) when they are active in the source editor. Internally these mini programs use the IDE feature **Run / Configure Build+Run File**. You can setup the source in _Environment / Options ... / Education / New program_. 

### Code formatting

Beginners have to learn how to write readable code. The IDE auto indenter (auto inserting spaces at the beginning of lines) normally mimics the indentation from the surrounding code. You can setup the indenter to copy the indentation only from the example file and you can edit the example file to whatever indentation rules you prefer. Setup the options under _Environment / Options ... / Codetools / General / Indentation_. 

## Helpful IDE features

Some features are especially useful for beginners. 

  * The [Code Explorer](<IDE_Window__Code_Explorer.md> "IDE Window: Code Explorer") shows the identifiers of the active unit. Double clicking on an identifier will jump to the source declaration.
  * Since 0.9.27 the Code Explorer contains the [Code Observer](<IDE_Window__Code_Explorer_Options.md> "IDE Window: Code Explorer Options") which lists unusual code like empty blocks, long procedures, wrong indentation, unnamed constants and more.
  * For many compiler messages the IDE provides _Quick Fixes_. Just right click on a message. This popup menu is context sensitive and shows different tools for different messages. For example since 0.9.29 there is a _Quick Fix_ tool for the message _Can't find unit_. When the compiler can not find a unit, right click on the message and it will show the _Search unit_ tool.



## Done

  * A designtime package was created 'educationlaz', that allows to easily setup the IDE for various levels.
  * Less files: put all .lrs files of .lfm files into output directory
  * Rename a unit renames the references in other units
  * 'unit not found' quick fix item: search unit
  * 'identifier not found' quick fix item: search identifier
  * When a component is created the IDE can ask for a new name immediately, because beginners have no workflow
  * When a unit is created the IDE can ask for the new name immediately (optional: save immediately), because beginners have problems with the concept of virtual files
  * Dialog to hide components of the component palette
  * Dialog to hide menu items
  * Most beginners find the concept of 'projects' confusing. Instead they need to create a lot of small test programs which contain only one file. And all these programs are stored in one directory. The education package adds a button to create single file programs with one click.
  * auto indentation on enter
  * auto indentation on paste
  * hide object inspector pages. For example _Favorites_ and _Restricted_ are bad for beginners.
  * hide some or all of the speedbuttons on the IDE bar.
  * less files: the lrs files are no longer needed
  * less windows via docking
  * All foreign units (not belonging to a project/package) now gets the unit search paths of the project.
  * Main bar speedbutton new unit and new form can be set to create other file types.



## ToDos

  * Save unit - do not ask to rename lowercase, the IDE should check regularly the uses sections and fix them
  * Possibility to hide properties in the object inspector. At the moment you can only hide properties globally - in all property grids.
  * A dialog to setup the education level and/or hide topics, for example to hide the form editing options or to hide frames or to hide the package system. For beginner courses a minimal IDE is needed.
  * F1 on an identifier in the source editor should be extended to search out of scope
  * off line help for RTL, FCL
  * less files: put auto created files into a separate directory: rc, res
  * Use package editortoolbar to add some useful buttons like jump back/forward, find, search, cut, copy, paste, find in files view messages, view debugger output, view debugger callstack, view project inspector
  * A terminal window for readln/writeln programs. Resizable, configurable font, copy to clipboard, search.
  * save a new project: ask to create a new folder
  * Configure main bar speedbuttons



## Further ideas from non lazarus developers

---

_Source: [https://wiki.freepascal.org/Lazarus_for_education](https://web.archive.org/web/20230605153133/https://wiki.freepascal.org/Lazarus_for_education)_
