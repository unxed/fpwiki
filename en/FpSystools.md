# FpSystools

│ **English (en)** │    
****

## Contents

  * 1 Free Pascal SysTools Package
    * 1.1 About
    * 1.2 Status: Alpha
    * 1.3 Author
    * 1.4 License
    * 1.5 Download
    * 1.6 Change Log
    * 1.7 Getting the latest source from CVS
    * 1.8 Dependencies / System Requirements
    * 1.9 Installation



## Free Pascal SysTools Package

### About

This package is a conversion of TurboPower SysTools for Kylix Version 1.01 to FPC/Lazarus. The conversion process was started on June 12, 2003 and initially completed on June 17,2003 

### Status: Alpha

With this release, all of the units compile and most are probably working. I've flagged any changes I've made with a //TL tag. If there were issues, I added one or more ! to TL. You can grep through the code and selectively list these areas using the tags. Serious, functionality breaking problems have two or more !!. 

Out of over the 50,000+ lines of code in the library there are about 130 instances of serious errors that need to be fixed... and these probably comprise a set of about a dozen types. Once these are solved, the solution can be applied to others in its class. 

All of the units were converted in the 1.01 release except 2... which were the barcode or StSystem units. The barcode I/O seemed to have printing and QT dependencies that I didn't think were resolvable with a reasonable amount of effort. The system unit had a LOT of problems compiling and didn't seem to offer anything that wasn't already done better in the existing FPC Linux unit. 

### Author

This package was created and generously made available to the open source community by [TurboPower Software](<http://sourceforge.net/projects/tpsystoolskylix/>)

Initial conversion to Lazarus and Free Pascal by [ Tom Lisjac](</User:VlxAdmin> "User:VlxAdmin"). 

### License

Site says "GNU Library or Lesser General Public License (LGPL), Mozilla Public License 1.1 (MPL 1.1)". Code headers mostly show MPL 1.1 

### Download

The latest release of FpSystools can be found on the [Lazarus CCR Files page](<http://sourceforge.net/project/showfiles.php?group_id=92177&release_id=193574>). 

### Change Log

Most of the common changes that were needed for FPC conversion were: 

  * Prefixing function parameters with an "@"
  * Commenting out the critical section code... there were ifdefs around them but I didn't try to use them. This needs to be revisited and updated in the next pass.
  * FPC claimed duplicate ID's in a lot of method declarations that Delphi/Kylix obviously didn't worry about. I approached this, for better or worse, by adding a 1 suffix to the identifier in the declarations... and then fixing up the implementations. All are flagged for and should be reviewed.
  * The Libc unit was changed to Linux in all cases



### Getting the latest source from CVS
    
    
    cvs -d:pserver:anonymous@cvs.sourceforge.net:/cvsroot/lazarus-ccr login
    

(press the Enter key when prompted for a password) 
    
    
    cvs -z3 -d:pserver:anonymous@cvs.sourceforge.net:/cvsroot/lazarus-ccr co fpsystools
    

This package is also available from the Free Pascal Compiler CVS in projects/contrib/fpsystools 

### Dependencies / System Requirements

There's a _lot_ of in-line assembler... so most of the code is currently bound to the i386 platform. (Some of the assembler constructs are broken under FPC and need to be fixed.) 

### Installation

Un-tar the file, cd to the directory and make.

---

_Source: [https://wiki.freepascal.org/FpSystools](https://web.archive.org/web/20250326011728/https://wiki.freepascal.org/FpSystools)_
