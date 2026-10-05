# IDE Window: Clean up build files dialog

│ **[Deutsch (de)](</IDE_Window:_Clean_up_build_files_dialog/de> "IDE Window: Clean up build files dialog/de")** │  **English (en)** │    
****

# Overview

This dialog can be found via the IDE menu _Run / Clean up build files ..._

It shows all files of all output directories of the project and all its packages. You can define filters and delete the files. You can clean up all source directories too. 

[![CleanUpProjectFiles1.png](https://wiki.freepascal.org/images/e/e3/CleanUpProjectFiles1.png)](</File:CleanUpProjectFiles1.png>)

You can define one filter for each category: 

  * the project output directory (See Project / Project Options / Compiler options / Paths / Unit output directory)
  * the project source directories (See Project / Project Options / Compiler options / Paths / Other unit files)
  * the package output directories
  * the package source directories



A filter is a semicolon separated list of file masks. The file masks support the globbing characters star ***** for anything and question mark **?** for any character. 

The project filters are stored in the project sesssion file (lps). The package filters are stored in the IDE's global options (~/.lazarus/inputhistory.xml).

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Clean_up_build_files_dialog](https://web.archive.org/web/20230330122102/https://wiki.freepascal.org/IDE_Window%3A_Clean_up_build_files_dialog)_
