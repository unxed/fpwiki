# CmdLine

## Contents

  * 1 About
  * 2 Screenshots
  * 3 Authors
  * 4 License
  * 5 Download
  * 6 Change Log
  * 7 Dependencies / System Requirements
  * 8 Installation



### About

_CmdLine_ is a prompt component, which has a few extra features to fit better into a VCL environment. 

Its main features over those of an ordinary prompt are : 

  * Dynamic line length: lines which are too long are wrapped (Word wrapping or Char wrapping) 
  * Independent input buffer so you can still write during input 
  * Input history 
  * Copy/paste/cut for input 
  * Multispace font support 
  * ANSI escape codes or CmdBox special escape codes (or none if you switch them off) 



**If you find bugs, don't work around them please,** [contact me](</User:Alexandrus> "User:Alexandrus"), I probably can fix it in less than a day and you don't have to work around again if the next version is released. 

It should work on Win32, GTK and GTK2. 

### Screenshots

Example Application on Vista 

[![ucmdline2.png](https://wiki.freepascal.org/images/9/94/ucmdline2.png)](</File:ucmdline2.png>)

### Authors

Julian Schutsch 

Dmitry Kolomiets aka B4rr4cuda (UTF8, testing) 

### License

[LGPL](<http://www.opensource.org/licenses/lgpl-license.php>) (please contact the author if the LGPL doesn't work with your project licensing) 

Example Program:[GPL](<http://www.opensource.org/licenses/gpl-license.php>)

### Download

The latest stable release can be found on the [Lazarus CCR download page](<http://sourceforge.net/projects/lazarus-ccr/files/?source=navbar>). 

It is also available in the lazarus SVN, [Using Lazarus-ccr SVN](<Using_the_Lazarus-ccr_SVN_repository.md>)

An Example Application is included. 

### Change Log

  * Version 0.5.4 2014.12.01 
    * Fixed arrow key behaviour 
    * Fixed component height calculation 
  * Version 0.5.2 2009.08.04 
    * Published generic controls 
    * Seperated front- and background drawing, should solve kerning issues 
    * Added WriteInput property 
    * Scrolling simplified 
  * Version 0.5.1 2009.03.25 
    * Fixed StartRead 
    * Wrap Types Char/Word 
    * Buffered Linecounts (slight speed improvement) 
  * Version 0.5 2008.06.29 
    * TColorString rewritten 
    * Multispace support, lots of changes for that 
    * Added Tab/Escape stuff 
    * Speed improvements 
    * UTF8 Support 
    * Multithreading (write/writeln) checked, could work without synchronize (with TTimer instead) 
  * Version 0.3 2007.10.9 
    * Removed fixed line width support 
    * Added Paste/Copy/Cut 
    * Threadsafe writeln/write 
    * Fixed GTK 1/2 Support (tested on Kubuntu) 
    * Workaround for font height, now gets +2 added (TTextMetric not equal on Win32/GTK) 
    * Fixed Scrolling with Pos1/End suppressed on GTK 
  * Version 0.2 2007.10.2 
    * LGPL Licence 
    * Added history 
    * Added password input mode 
    * Added fixed promt description infront of input 
    * Minor bugs (Resizing, other) 
  * Version 0.1 2007.9.27 
    * Published, component works on Win32, hopefully everywhere else too 



### Dependencies / System Requirements

  * LCL >=1.0 



Status: _Beta_

### Installation

  * Open cmdline.lpk 
  * Press Install

---

_Source: [https://wiki.freepascal.org/CmdLine](https://web.archive.org/web/20170720112053/https://wiki.freepascal.org/CmdLine)_
