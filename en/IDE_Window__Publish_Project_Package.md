# IDE Window: Publish Project Package

│ **English (en)** │

****[![Project - Publish Project.png](https://wiki.freepascal.org/images/2/22/Project_-_Publish_Project.png)](</File:Project_-_Publish_Project.png>)

Publishing a project or package means here: create a copy of the project/package directory and sub directories. 

Note: It does not copy files outside the project/package directory. Developers are welcome to improve this. 

## Contents

  * 1 Destination directory
  * 2 Files
  * 3 Include filter
  * 4 Exclude filter
  * 5 Project information



## Destination directory

  * Directory - This directory will be created and/or cleaned. Default: $(TestDir)/publishedproject/
  * Command after - After copying all files, run this command. For example compressing the directory into a tgz archive.



## Files

  * Ignore binaries - do not copy binary files.



## Include filter

  * Use include filter - enable this filter. All files matching the filter will be copied. Otherwise all files will be copied.
  * Simple syntax - don't use [IDE regular expressions](<IDE_regular_expressions.md> "IDE regular expressions") in filter field.



## Exclude filter

  * Use exclude filter - enable this filter. All files matching this filter will not be copied. Otherwise all files will be copied.
  * Simple syntax - don't use [IDE regular expressions](<IDE_regular_expressions.md> "IDE regular expressions") in filter.



## Project information

  * Save info of closed editor files - store information of all files to the .lpi, that were once opened in the editor.
  * Save editor info of non project files - store information of all files to the .lpi, even those, that are not part of the project.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Publish_Project_Package](https://web.archive.org/web/20250114071446/https://wiki.freepascal.org/IDE_Window%3A_Publish_Project_Package)_
