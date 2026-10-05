# file types

This page is a summary of the file types created by FPC and Lazarus. The main purpose is to make it easy to find out whether or not a particular file should be added to version control. 

## Types to add to VCS

**Extension** | **Description**  
---|---  
.pas | Pascal source file  
.pp | Pascal source file  
.lfm | Lazarus form source file  
.lpi | Lazarus project file  
.lpk | Lazarus project main source file  
.lpr | Lazarus package source file  
.rc | ?? Resource file??  
.ico | Application icon image  
.manifest | Windows manifest file for themes  
  
## Types usually not added to VCS

**Extension** | **Description**  
---|---  
.lps | Lazarus session file  
.compiled | FPC compilation state  
.o | Object file  
.or | Object file  
.ppu | Precompiled Unit file  
.res | Lazarus resource file.  
.rst | Compiled resource strings. Used for L10n. If you intend to translate an application, this should probably be version controlled.

---

_Source: [https://wiki.freepascal.org/file_types](https://web.archive.org/web/20171026085346/https://wiki.freepascal.org/file_types)_
