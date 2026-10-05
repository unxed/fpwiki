# Win64 for AMD64

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Obsolete
  * 2 Status
  * 3 Compiling and building
    * 3.1 Compiling a program
  * 4 Debugging and Profiling
  * 5 Todo
  * 6 Technical information



## Obsolete

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** November 2013: this page is obsolete; Windows x64 support has been implemented. This page is kept for historical reference only...

## Status

  * 2.2.x (and above) compiler support
  * compiler can build itself
  * snapshot is available <ftp://ftp.freepascal.org/pub/fpc/dist/i386-win32-2.2.0/fpc-2.2.0.x86_64-win64.exe>
  * test suite results for 2.3.1 version are at 30 failures (near to Win32, 27 failures) [http://www.freepascal.org/cgi-bin/testsuite.cgi?os=16&cpu=8&version=0&date=](<http://www.freepascal.org/cgi-bin/testsuite.cgi?os=16&cpu=8&version=0&date=>)
  * Lazarus/FreePascal daily snapshot is available <http://www.hu.freepascal.org/lazarus/>



## Compiling and building

### Compiling a program

Win64 requires the usage of the internal linker of FPC so all programs must be compiled with -Xi. 

## Debugging and Profiling

There is currently no full debugger support available, we're working on improving this situation. Basic debugging can be done so far with WinDBG <http://www.microsoft.com/whdc/devtools/debugging/install64bit.mspx>, the missing dwarf support of WinDBG makes things a little bit difficult though. 

GDB for Win64 (beta version) is already available at [http://sourceforge.net/project/showfiles.php?group_id=202880&package_id=259447](<http://sourceforge.net/project/showfiles.php?group_id=202880&package_id=259447>)

## Todo

  * ~~add compiler target~~
  * ~~adapt rtl units~~
  * ~~adapt api units~~
  * ~~adapt fcl~~
  * ~~get lazarus working~~
  * get integrated debugger working



## Technical information

We're using our own assembler and linker to overcome the problem of the missing GNU Tools. 

Information about the API etc. can be found at [Win64/AMD64 API](<Win64/AMD64_API.md> "Win64/AMD64 API")

---

_Source: [https://wiki.freepascal.org/Win64_for_AMD64](https://web.archive.org/web/20220606231911/https://wiki.freepascal.org/Win64_for_AMD64)_
