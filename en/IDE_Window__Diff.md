# IDE Window: Diff

│ **[Deutsch (de)](</IDE_Window:_Diff/de> "IDE Window: Diff/de")** │  **English (en)** │  **[español (es)](</IDE_Window:_Diff/es> "IDE Window: Diff/es")** │  **[suomi (fi)](</IDE_Window:_Diff/fi> "IDE Window: Diff/fi")** │  **[français (fr)](</IDE_Window:_Diff/fr> "IDE Window: Diff/fr")** │    
****  
****

## Contents

  * 1 Purpose
  * 2 Limitations
  * 3 Text1
  * 4 Text2
  * 5 Options
  * 6 Close
  * 7 Open Diff in Editor
  * 8 Save diff



## Purpose

The diff tool lets you compare two files (or selected parts from two files). It can create a **diff** in Context format. See: 
    
    
    <http://en.wikipedia.org/wiki/Diff#Context_format>.
    

## Limitations

It is not suitable for creating patches for systems like Subversion and Git that need the Unified format. See: 
    
    
    <http://en.wikipedia.org/wiki/Diff#Unified_format>
    

It performs a line-based comparison (not a word- or paragraph-based) comparison, so it only works for text files (including source code), not binary files. 

## Text1

Select the first file in the comparison. If any portion of text is selected within the file, then only this selected part of the text will be compared. 

## Text2

Select the second file in the comparison. If any portion of text is selected within the file, then only this selected part of the text will be compared. 

## Options

  * **Case sensitive** : Distinguishes between lowercase and upper-case (capital letter) characters.
  * **Ignore spaces at start of line** : Ignore any indent when comparing two lines (whether space ' ' or tab characters).
  * **Ignore differences in line ends** : A unix line ending #10 is treated as identical to a Windows line ending #13#10.
  * **Ignore spaces (newline chars not included)** : Ignore all spaces. 'A B' is the same as 'AB'.
  * **Ignore if empty lines were added or removed** : Skip empty lines.
  * **Ignore spaces at end of line** : Ignore trailing spaces when comparing two lines.
  * **Ignore amount of space characters** : Any number of consecutive spaces is treated as one space.



## Close

Close this dialog. 

## Open Diff in Editor

Close the dialog and open a new page in the source editor with the diff. 

## Save diff

Opens a file dialog to save the diff to a file.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Diff](https://web.archive.org/web/20240809002801/https://wiki.freepascal.org/IDE_Window%3A_Diff)_
