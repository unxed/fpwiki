# IDE Window: Find in files

│ **English (en)** │

This dialog allows to setup search and replace patterns for searching in multiple files. 

Setup the options and click the Find button at the bottom to start the search. A window will open showing the progress. It contains a button to abort the search, if it takes too long. When all files are searched the [Search Results](<IDE_Window__Search_Results.md> "IDE Window: Search Results") window will open. 

  * Text to find
  * Replace - check this to enable the replace text field.



## Options

  * Case sensitive - distinguish lower and uppercase (for example 'a' and 'A')
  * Whole words only - found text must start and end at a word boundary (position between word-chars and non-word-chars)
  * Regular expression - treat texts as [IDE regular expressions](<IDE_regular_expressions.md> "IDE regular expressions")
  * Multiline pattern - for reg-expressions, activates option "dot means also newlines"



## Where

  * search all files in project - search in all files, that belong to the current project
  * search all open files - search in all files opened in the source editor
  * search in directories - search in folder(s) specified in option below
  * search in the current file



## Directory options

  * Directory - folder paths, where the search starts. Allowed several paths separated by ";", trailing slashes ignored. Use the "..." button to the right to open a dialog to choose a directory.
  * File mask - defines mask(s) for the files, that are searched. Only text files will be searched. Examples for masks are: "*.pas" or "*.pas;*.pp;*.inc". The combobox contains last entered masks.
  * Include sub directories - allows to search in all subfolders.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Find_in_files](https://web.archive.org/web/20220930035945/https://wiki.freepascal.org/IDE_Window%3A_Find_in_files)_
