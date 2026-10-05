# htmlhelp compiler

## Contents

  * 1 Overview
  * 2 History
  * 3 Missing features
  * 4 Performance
  * 5 Download



## Overview

The htmlhelp compiler chmcmd is a GPL licensed htmlhelp1 (CHM) helpfile compiler. 

The help file compiler's main program only performs command line handling. All other functionality (including .hhp loading functionality and HTML scanning) is done by FPC's [chm](<chm.md> "chm") package, which is licensed under a more permissive license (LGPL with static linking exception) 

Two formats are supported: 

  * HTML help .hhp help projects (similar to those from the old Microsoft HTML Help workshop)
  * a superset that stores all information (including context data) in one .XML.



## History

While the primary target of the [chm](<chm.md> "chm") package is the documentation tool [fpdoc](<FPDoc_Editor.md> "FPDoc Editor"), a small commandline utility chmcmd was added for testing purposes. This utility simply created a TCHMProject instance, would load the .xml, and called the "writechm" method. The XML format had to list all files to be included (including .css and image files). 

In 2010, basic .hhp support was added, and the project was expanded to scan input files for extra files. 

## Missing features

See [chm](<chm.md> "chm")

## Performance

The performance probably won't be great. Mostly because the program is written in a portable manner. 

The chm package has some support for doing the compression using multiple cores, but this has not been enabled by default for two reasons: 

  1. on Linux/FreeBSD this pulls in libpthread and libc which makes the binary distribution dependent (on FreeBSD and Linux, basic FPC binaries are fully static and use syscalls)
  2. the savings are not that great (think 25% faster on a dual core)



A Win32 binary is currently about 420k (uncompressed, but stripped). 

## Download

The HTML compiler is not available as a separate download, but is included in most FPC releases and snapshots.

---

_Source: [https://wiki.freepascal.org/chmcmd](https://web.archive.org/web/20250514114533/https://wiki.freepascal.org/chmcmd)_
