# RichView

│ **[Deutsch (de)](</RichView/de> "RichView/de")** │  **English (en)** │  **[español (es)](</RichView/es> "RichView/es")** │    
****

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Screenshots
  * 6 Change Log
  * 7 Installation



### About

"RichView is a suite of native Delphi/C++Builder components for displaying, editing (not demonstrated by demo/not in freeware version*) and printing hypertext documents. Components support various character attributes (fonts, subscripts/superscripts, colored text background, custom drawn)." This package contains a port of the freeware version. 
    
    
     * Shareware version (as stated in lazrichview-0.5.2.2 demo project)
     The development of freeware version was stopped in 1999.
     This update was made for Delphi 5 and C++Builder 5 compatibility.
     Newer (shareware) version includes editor, data-aware versions, component for print preview, works with Unicode, supports HTML-style tables.
     Contents can have much more complicated formatting - left, center, right and justify alignments, subscripts/superscripts, paragraph backgrounds and more.
     Literally all actions of free version are performed in much faster and convenient way with shareware version.
     Please visit www.trichview.com for additional information.
    

### Author

  * Author: Sergey Tkachenko, <http://www.trichview.com>
  * LCL Port: [Paul Burton](</User:Burty89> "User:Burty89")
  * LCL Additional Portability and Fixes: [Jesus Reyes A.](</User:Jesusrmx> "User:Jesusrmx")
  * Improvements and fixes: [Sergey Bodrov](</index.php?title=User:Serbod&action=edit&redlink=1> "User:Serbod \(page does not exist\)")



### License

Freeware (see originalreadme.txt). 

### Download

Version 0.5.3: [GitHub](<https://github.com/serbod/lazrichview>)

Version 0.5.2.2: [SourceForge](<http://sourceforge.net/projects/lazarus-ccr/files/LazRichView/>)

### Screenshots

A couple of screenshots of the demo application running under Windows. 

[![Initial screen](https://wiki.freepascal.org/images/c/ca/Lazrichview-sc1.png)](</File:Lazrichview-sc1.png> "Initial screen")

Controls can be embedded in Richview 

[![Clicking an embedded button](https://wiki.freepascal.org/images/3/33/Lazrichview-sc2.png)](</File:Lazrichview-sc2.png> "Clicking an embedded button")

### Change Log

  * 30.12.05 (0.5.2.0) First Release
  * 19.01.06 (0.5.2.1) Improved compilation with lazarus and packaging
  * 08.09.06 (0.5.2.2) Fixes and Portability (now it works in Windows and Linux) plus demo, for details see readme.txt
  * 01.06.17 (0.5.3) Fixes and performance improvements



### Installation

Installation is the same as for other components: 

  * Create a directory to store the files, e.g. lazarus\components\richview
  * Unzip the files from the zip file in this directory
  * Open Lazarus
  * Open lazrichview.lpk with Component/Open package file (.lpk)
  * (Click on Compile only if you don't want to install the component into the IDE)
  * Click on Install



If you have trouble with installing, before compiling: in Compiler Options->Paths->Include Files, put only "$(LazarusDir)/lcl/" and delete everything else.

---

_Source: [https://wiki.freepascal.org/RichView](https://web.archive.org/web/20250417103317/https://wiki.freepascal.org/RichView)_
