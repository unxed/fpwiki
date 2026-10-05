# JvXPBar

## Contents

  * 1 About
  * 2 Screenshot
  * 3 Author
  * 4 License
  * 5 Download
  * 6 Change Log
  * 7 Dependencies / System Requirements
  * 8 Installation
  * 9 The JvXPBar Example Application



### About

A port of [JVCL's](<http://jvcl.sourceforge.net>) TJvXPBar control that can display an icon, a header and zero or more clickable items in its client area. Use a TJvXPBar control on a form to display an XP style group box with items that can be clicked. 

### Screenshot

  * Designing:



[![JvXPBarLaz design.JPG](https://wiki.freepascal.org/images/a/af/JvXPBarLaz_design.JPG)](</File:JvXPBarLaz_design.JPG>)

  * Running:



[![JvXPBarLaz run.JPG](https://wiki.freepascal.org/images/b/b1/JvXPBarLaz_run.JPG)](</File:JvXPBarLaz_run.JPG>)

  * Running on openSuse 10.2 inside VirtualPC 2007 compiled with GTK 2.x (at this time doesn't work with GTK 1.x):



[![JvXPBarLaz run lin.JPG](https://wiki.freepascal.org/images/7/79/JvXPBarLaz_run_lin.JPG)](</File:JvXPBarLaz_run_lin.JPG>)

* * *

Note: AFAIK VirtualPC can only emulate 16 bits per pixel resolution, thats why gradient has low quality, it should see better in real monitor. 

### Author

Sergio Samayoa 

### License

I guess the same as JVCL + Lazarus + FPC. 

### Download

From [Lazarus CCR](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=246940>) source forge. 

### Change Log

  * Version 1.0 (23.09.2007) - Initial version.
  * Still version 1.0 (24.09.2007) - Minor improvements and linux test.



### Dependencies / System Requirements

  * LCL 1.0.
  * Tested with Lazarus 0.9.23 svn revision 12082 for Windows XP SP2.
  * Tested with Lazarus 0.9.23 svn revision 12076 for Linux with GTK 2.x IDE.
  * Linux: Threads required (add -dUseCThreads in "other" tab of project's compiler options).



Status: 

  * Stable.



Issues in Windows: 

  * None as far as I known.



Issues in Linux: 

  * Minor paint error of the header: parent color lost.
  * DrawText() ignores DT_VCENTER then text are at top of the canvas instead of vertically centered.



### Installation

Should be as simple as: 

  * Open JvXPBarLaz.lpk.
  * Compile.
  * Install.



### The JvXPBar Example Application

Very simple in demo/ directory of zip file.

---

_Source: [https://wiki.freepascal.org/JvXPBar](https://web.archive.org/web/20250327135821/https://wiki.freepascal.org/JvXPBar)_
