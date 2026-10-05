# IDE Development

│ **English (en)** │  **[русский (ru)](<../ru/IDE_Development.md>)** │

This page contains notes for lazarus core developers about ongoing development. 

Note: This paragraph is referenced by 

  * [Lazarus 0.9.26 release notes](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes")
  * [Lazarus 0.9.28 release notes](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes")
  * [Lazarus 0.9.30 release notes](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes")
  * [Lazarus 1.0 release notes](<Lazarus_1.md> "Lazarus 1.0 release notes")
  * [Lazarus 1.2.0 release_notes](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes")
  * [Lazarus 1.4.0 release_notes](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes")



## Contents

  * 1 Multi form properties / using DataModules from other forms in the designer
    * 1.1 Short Description
    * 1.2 Working
    * 1.3 ToDo
  * 2 Translations, i18n, lrt files, po files
    * 2.1 Short Description
    * 2.2 Working
    * 2.3 ToDo
  * 3 Frames
    * 3.1 Short Description
    * 3.2 Working
    * 3.3 ToDo
  * 4 VFI - Visual Form Inheritance
    * 4.1 Short Description
    * 4.2 Working
    * 4.3 ToDo
  * 5 The jedi code formatter
    * 5.1 Short Description
    * 5.2 Working
    * 5.3 ToDo
  * 6 Debugger
    * 6.1 Short Description
    * 6.2 Working
    * 6.3 ToDo
  * 7 Release Notes



# Multi form properties / using DataModules from other forms in the designer

## Short Description

This feature allows to use components of other designer forms. A common example is to use the TDataSource on a DataModule for the DataSource property of a db control. Designer forms can only reference each other if they have a CreateForm statement in the lpr file (Project Options / Forms). 

## Working

  * Manual referencing via source code works.
  * TPersistentPropertyEditor.GetValues - List all possible values. Check for class compatibility and if the target form is listed in the CreateForm statements of the lpr file and if the target unit does not belong to a package that will conflict if used.
  * TPersistentPropertyEditor.GetValue - Show component path
  * TPersistentPropertyEditor.SetValue - search the component via the given path
  * When component is deleted, the property must be set to nil - This is not the job of the IDE, but should be achieved by the normal TComponent FreeNotification feature. Maybe eventually a check could be added, if this is implemented properly and force a nil on error.
  * When a component is renamed the property does not need to be updated, because the form is open and use the pointer not the name. But the designer must be flagged 'modified', because the lfm has changed. See below.
  * When reference form is opened, then target forms are opened too.
  * When target form is closed, then only the designer is closed. The component is kept hidden.
  * When all referring forms are closed/hidden, the hidden component will be automatically freed.
  * Circle dependencies are allowed.
  * When referenced component is renamed, the using units must be set modified.
  * When referenced component is deleted, the using units must be set modified.
  * Opening a unit now checks if designer is already created
  * Reopen/Revert a form - all connected forms are now closed



## ToDo

  * Allow to connect to forms, that are used in the uses section. Technically there is _no_ connection between the uses section and the form streaming, because form streaming uses global variables. But the IDE must somehow find the referenced form. Without the uses section the IDE must in worst case search and read every reachable lfm file on disk. So the uses section is a good compromise between speed and flexibility.



# Translations, i18n, lrt files, po files

## Short Description

This feature automatically creates and updates .po files for packages and projects. 

## Working

  * Dialog options for project/package to enable i18n
  * Collect TTranslateStrings from RTTI while designer form writing and writing them to lrt file.
  * Copying new strings from rst and lrt files to a single .po file for each project
  * load the .po files
  * Delete unused strings from the .po file of package/projects
  * Update translated .po files, similar to the updatepofiles tool.



## ToDo

  * Copying new strings from rst and lrt files to a single .po file for each package (implemented, need dialog option to change po filename or need agreedment to use same name of package)
  * Collect all .po files of project and all used packages into a directory. This directory can then be used by the project to load the strings at runtime. And installers can copy the content.
  * standardize filenames and path, so that loading the translations at runtime is possible without writing code.



# Frames

## Short Description

Frames are special child forms. The goal is to edit them like forms in the IDE designer, and to use them as components on designer forms. 

## Working

compile lazarus clean with passing **-dEnableTFrame** (not necessary starting from svn release 16909) 

  * Manually creating via source
  * gtk1, gtk2, qt, win32/win64
  * Create a new frame
  * Open a frame in the designer
  * Save a frame
  * Close a frame
  * Revert a frame
  * Design a frame (adding, selecting, moving controls, ...)
  * Opening a nested frame
  * Opening a nested frame with ancestors
  * Support for the inline keyword for lfm and lrs streams.
  * Add a frame to a form, add unit to uses section, package to project dependencies 
    * Auto opening frame if not yet loaded
  * Close a frame that is currently used by a nested frame
  * Close a form with an embedded frame
  * Save a form with an embedded frame
  * Delete a frame embedded in a form
  * Inherited nested Components (Components with the frame as Owner and not the Form) 
    * Show inherited nested components properties in OI
    * Forbid deleting nested inherited components
    * Forbid renaming nested inherited components
    * Show inherited nested components events in OI as ClassName.MethodName
    * When dblclick on inherited nested components events in OI create an event with the following code: Frame1.FrameResize(Sender);
    * When Ctrl+Click on inherited nested components events in OI jump to inherited code.
    * show nested components in OI component tree
    * Write nested component to stream
  * Forbid putting a component onto a nested frame, because TWriter does not support that.
  * Revert a form with an embedded frame



## ToDo

  * Propagate changes of ancestor to nested controls
  * Tutorial
  * Documentation
  * Update



# VFI - Visual Form Inheritance

## Short Description

Forms can inherit from other forms. The .lfm streams of ancestors are read/applied before the current lfm. The .lfm of the descendant is the difference between ancestor and current state. The IDE allows to open and edit inherited forms. VFI does not only apply to TForm, but TDataModule, TFrame and any registered base designer class. For the full features compile with -dEnableTFrame. 

## Working

  * find ancestor unit, lfm and class
  * automatically open ancestor form hidden in background
  * open descendant and read ancestor lfm first
  * show hidden ancestor form
  * hide ancestor form
  * automatically close hidden ancestor form if not needed anymore
  * create new ancestor form via 'File / New ... / Inherited Items'
  * forbid deleting inherited components
  * forbid renaming inherited components



## ToDo

  * inherited events (partially working)
  * apply changes of ancestor to descendants when both are open
  * Tutorial
  * Documentation



# The jedi code formatter

## Short Description

## Working

  * Pressing ctrl+D will format your source file
  * Settings are stored and read by IDE from JCFSettings.cfg file
  * port the jcf option dialogs



## ToDo

  * use jcf for indentation



# Debugger

Please see: [Debugger_Status](<Debugger_Status.md> "Debugger Status")

## Short Description

## Working

  * show breakpoints etc in asm window
  * implement/design interface on debugger class to show/handle threads
  * attach to process



## ToDo

  * implement/design interface on debuggerclass to handle os exceptions
  * implement/design interface on debugger class to show messages
  * implement/design interface on debugger class to show loaded modules
  * implement debugging of forked processes
  * implement launching process on xterm (mainly for console apps)
  * find include files in fpc sources



# Release Notes

  * [Lazarus 1.4.0 release notes](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes")
  * [Lazarus 1.2.0 release notes](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes")
  * [Lazarus 1.0 release notes](<Lazarus_1.md> "Lazarus 1.0 release notes")
  * [Lazarus 0.9.30 release notes](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes")
  * [Lazarus 0.9.28.2 release notes](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes")
  * [Lazarus 0.9.28 release notes](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes")
  * [Lazarus 0.9.26.2 release notes](<Lazarus_0.9.26.md> "Lazarus 0.9.26.2 release notes")
  * [Lazarus 0.9.26 release notes](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes")
  * [Lazarus 0.9.24 release notes](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes")

---

_Source: [https://wiki.freepascal.org/IDE_Development](https://web.archive.org/web/20241213023924/https://wiki.freepascal.org/IDE_Development)_
