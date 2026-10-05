# IDE Window: Find

│ **English (en)** │

If this dialog is invoked by Ctrl+F or by menu item "Search > Find" then the replace buttons and options are disabled. 

If this dialog is invoked by Ctrl+R or by "Search > Replace" then the replace options will be enabled. 

  * Text to Find
  * Replace With



The above two comboboxes contain the last searched texts and replace patterns. Down-key will go back in history and fetch the last text to find. 

**Hint** : You can choose, whether the IDE starts the dialog with the word at the cursor in the source editor or with a blank field in Environment -> Editor Options -> General -> Find text at cursor 

## Contents

  * 1 Options
  * 2 Origin
  * 3 Scope
  * 4 Direction



## Options

  * Case sensitive - distinguish lower case and upper case ('a' and 'A')
  * Whole words only - the found text must start at a word start and end at a word end
  * Regular expressions - treat texts as [IDE regular expressions](<IDE_regular_expressions.md> "IDE regular expressions")
  * Multi line - the search does not stop at line ends or in other words: the found text can contain multiple lines. You can paste/copy multiple lines from the source editor to the Find field and you will see the line endings. These line endings can not be created by normal keys. When searching with regular expressions you can use _\n_ (backslash plus n) as marker for line end characters. This also activates "multi-line" reg-expression option: for example, reg-expression "begin.*Application" will find such text:


    
    
    begin
      Something;
      Application
    

  * Prompt on replace - ask before replacing (only enabled if replacing)



## Origin

  * From Cursor - start search at cursor. If direction is forward, it searches till the end of the file.
  * Entire Scope - starts search at the text edge (at the beginning for forward search, at the very end for backward search)



## Scope

  * Global - search in whole file
  * Selected Text - limit the search to the selected text



**Hint** : To search multiple files use Search -> [Find in files](<IDE_Window__Find_in_files.md> "IDE Window: Find in files"). 

## Direction

  * Forward search - search top to bottom. Each line is searched from left to right.
  * Backward search - search bottom to top. Each line is searched forward - left to right.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Find](https://web.archive.org/web/20200814031254/https://wiki.freepascal.org/IDE_Window%3A_Find)_
