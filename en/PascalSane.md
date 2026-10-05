# PascalSane

│ **English (en)** │  **[español (es)](</PascalSane/es> "PascalSane/es")** │    
****

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Change Log
  * 6 Dependencies / System Requirements
  * 7 Documentation
  * 8 How to include PascalSane in a Lazarus application
  * 9 The _PascalSane_ Example Application



### About

PascalSane provides pascal bindings for the [libsane](<http://www.sane-project.org/html/>) library, enabling Lazarus and FreePascal applications to access scanners under Linux. 

Principal operations : 

  * list available scanners
  * list options for a specified scanner
  * set options for a scanner
  * capture scanner input in PNM format



The download contains the libsane bindings and a unit _saneutils.pas_ which provides some simple functions for manipulating scanner data. It also contains a demonstration Lazarus application, which contains examples of operations that can be performed using libsane. 

### Author

**Malcolm Poole** : mgpoole at users.sourceforge.net 

### License

The libsane headers are in the public domain. The demo application is licensed under the GPL 

### Download

The latest stable release can be found at <https://code.google.com/archive/p/ocrivist/downloads>. 

### Change Log

  * Version 0.2 _8 May 2011_



    \- Added missing functions and bitwise enumerations
    \- corrected translation of constraint union in SANE_Option_Descriptor
    \- fixed a number of memory management issues in demo project
    \- added libsane test backend to demo project

  * Version 0.1 _19 November 2008_



### Dependencies / System Requirements

  * Linux
  * libsane (libsane-dev for Ubuntu)



### Documentation

[Documentation](<http://www.sane-project.org/html/>) for the Sane API covers all the functions provided by the bindings. The C source code for [scanimage](<http://www.sane-project.org/man/scanimage.1.html>) and other simple scanning applications are recommended for guidance. 

### How to include PascalSane in a Lazarus application

  * add 'sane' to the **uses** statement
  * in the Project Options dialog, add the path to _sane.pas_ in the **Other Unit Files** section.



### The _PascalSane_ Example Application

  * Open pascalsanedemo.lpi
  * set path to sane.pas in Project Options dialog
  * compile
  * run

---

_Source: [https://wiki.freepascal.org/PascalSane](https://web.archive.org/web/20250324163640/https://wiki.freepascal.org/PascalSane)_
