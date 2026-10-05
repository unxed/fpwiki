# IDE Window: Find or Rename identifier

│ **English (en)** │

This dialog is reached by placing the cursor on an [identifier](<Identifier.md> "Identifier") in the source editor, right-clicking and choosing either "Find > Find Identifier References..." or choosing "Refactoring > Rename Identifier...". Alternatively, this dialog can be reached by choosing from the Main Menu, "Search > Find Identifier References...". 

When the dialog is opened, the source editor will first jump to the declaration of the identifier then show the dialog. Hint: You can jump back via Search / Jump Back. 

Setup the search options and start the search. A progress window will popup up with a button to abort. When the search finished the _Search Results window_ will open presenting the result. If the identifier was renamed the result will be empty. 

## Contents

  * 1 Identifier
  * 2 Rename to
  * 3 Search where
  * 4 Additional files to search
  * 5 Search in comments too



## Identifier

In the caption of the groupbox the searched identifier is shown. In the listbox below the unit and include files of the declaration is presented, so you can make sure, the right identifier is replaced. 

## Rename to

Set here the name of the new identifier. 

## Search where

Note: This will be disabled if the identifier is only visible in the current unit. For example when it is a private or local variable. 

  * in current unit - search only in the current source editor file
  * in main project - search in all files of the current project (i.e. all files listed in the project inspector)
  * in project/package owning current unit - first search the project/package to which this file belong, then replace in all files of this project/package.
  * in all open projects and packages - as above, but search also in all depending projects/packages.



In any case the IDE will always skip binary files (using the FileIsText function). 

## Additional files to search

Specify here additional files to search. You can give multiple files and directories separated by semicolon. Macros are allowed and wild masks * and ? are allowed in the last part of the file name. Relative paths are expanded with the project directory. Examples: 

  * ***.pas;*.pp** : search in all files pas and pp files in the project directory.
  * **$(LazarusDir)/ide** : search in all pascal sources in the directory /your/path/to/lazarus/sources/ide
  * **folder** : search in all pascal sources in the projects sub directory _folder_



## Search in comments too

The codetools will replace the identifier in all comments as well.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Find_or_Rename_identifier](https://web.archive.org/web/20230603080550/https://wiki.freepascal.org/IDE_Window%3A_Find_or_Rename_identifier)_
