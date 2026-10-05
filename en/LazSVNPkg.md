# LazSVNPkg

## Contents

  * 1 About
  * 2 Original author
  * 3 License
  * 4 Installation
  * 5 Usage
    * 5.1 Show Log
    * 5.2 Commit
    * 5.3 Update
    * 5.4 Diff
  * 6 Roadmap



### About

This IDE plug-in wraps the svn executable which is found on a path defined in the path environment variables. It provides a GUI interface between Lazarus and the project subversion repository. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This package is still under development, this means that functionality or work flow can heavily differ in the Lazarus repository. Contact the Lazarus mailing-list for more up-to-date information.

### Original author

Darius Blaszyk 

### License

GPL 

### Installation

  * Open the package ./components/lazsvnpkg/lazsvnpkg.lpk with Component/Open package file (.lpk)
  * Click on Install and answer 'Yes' when you are asked about Lazarus rebuilding. A new menu item named SVN will be created under the Tools main menu.



Alternatively, if Lazarus is being built using make bigide it is possible to add lazsvnpkg to the sources by patching ide/lazarus.pp, ide/Makefile, components/Makefile and possibly the corresponding Makefile.fpc files. This is unsupported but might be useful on systems which by reason of slow speed or constrained resources take an unreasonable time to rebuild the IDE. 

### Usage

Usage of LazarusSVN is very straightforward. In the next sections a brief overview of each functionality is described. 

#### Show Log

Todo 

#### Commit

Todo 

#### Update

Todo 

#### Diff

Todo 

### Roadmap

  * Stress testing
  * Adding more functionality 
    * Repository browser
    * Revert
    * Merge
    * Update
    * Clean
    * Add
    * Delete
    * etc

---

_Source: [https://wiki.freepascal.org/LazSVNPkg](https://web.archive.org/web/20250417145400/https://wiki.freepascal.org/LazSVNPkg)_
