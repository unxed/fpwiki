# LHelp

## Contents

  * 1 Overview
  * 2 Operation
  * 3 Internals
  * 4 Limitations
  * 5 See also



## Overview

LHelp is the default help viewer for Lazarus. It displays CHM help files. 

The screenshot below shows Lazarus LCL context-sensitive help displayed in LHelp: [![lhelp.png](https://wiki.freepascal.org/images/9/90/lhelp.png)](</File:lhelp.png>)

## Operation

LHelp can be run stand-alone and used to open/view CHM files like a regular CHM viewer. 

It can also be invoked and controlled by the Lazarus IDE to display context-sensitive help etc. For that, it uses the [Help protocol](<Help_protocol.md> "Help protocol"). 

## Internals

LHelp uses the [chm](<chm.md> "chm") package provided by FPC to read CHM files, table of contents (TOC), and full text search index. 

## Limitations

  * LHelp cannot search on words containing the search term. This is similar to the behaviour of the Windows help viewer. Implementing this would require traversing the entire full-text search tree, parsing results and then displaying which would probably take a lot of time.
  * Known bugs: See bug reports on the bug tracker with tag LHelp, LHelp in the description etc.



## See also

  * [Add Help to Your Application](<Add_Help_to_Your_Application.md> "Add Help to Your Application")
  * [chmhelp](<chmhelp.md> "chmhelp")
  * [htmlhelp compiler](<htmlhelp_compiler.md> "htmlhelp compiler")



[Installing Help in the IDE](<Installing_Help_in_the_IDE.md> "Installing Help in the IDE") \- How to install help for the RTL, FCL and LCL in the Lazarus IDE

---

_Source: [https://wiki.freepascal.org/lhelp](https://web.archive.org/web/20250515064238/https://wiki.freepascal.org/lhelp)_
