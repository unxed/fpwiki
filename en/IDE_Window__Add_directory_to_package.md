# IDE Window: Add directory to package

│ **English (en)** │

In this dialog you can choose a directory and some filters and all fitting files will be added to the list in [Add to Package](<IDE_Window__Add_to_Package.md> "IDE Window: Add to Package"). 

## Contents

  * 1 Directory
    * 1.1 Include sub directory
  * 2 Filter
    * 2.1 Regular Expression
    * 2.2 Only text files
  * 3 Exclude filter
    * 3.1 Regular Expression



## Directory

The directory where the files are located. Use the browse button to the right to open a directory dialog. 

### Include sub directory

Check this if all sub directories should be searched too. 

## Filter

Leave blank, if all files should be added (including hidden files). Otherwise only those files will be added, that match the filter. The filter is applied to the filename without any directory. For the syntax and filter see [IDE regular expressions](<IDE_regular_expressions.md> "IDE regular expressions"). 

### Regular Expression

Normally a simple syntax for the filter is used. When this is checked the filter must be a regular expression. See [IDE regular expressions](<IDE_regular_expressions.md> "IDE regular expressions"). 

### Only text files

When checked only files that looks like text are added. Binary files like images will not be added. For details see the FileIsText function in unit FileUtil. 

## Exclude filter

Leave blank to disable this. Otherwise those files will be removed from the list, that match the exclude filter. The filter is applied to the filename without any directory. For the syntax and filter see [IDE regular expressions](<IDE_regular_expressions.md> "IDE regular expressions"). 

### Regular Expression

Normally a simple syntax for the filter is used. When this is checked the filter must be a regular expression. See [IDE regular expressions](<IDE_regular_expressions.md> "IDE regular expressions").

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Add_directory_to_package](https://web.archive.org/web/20190916203414/https://wiki.freepascal.org/IDE_Window%3A_Add_directory_to_package)_
