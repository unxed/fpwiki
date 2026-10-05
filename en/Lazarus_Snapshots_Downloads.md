# Lazarus Snapshots Downloads

│ **English (en)** │  **[suomi (fi)](</Lazarus_Snapshots_Downloads/fi> "Lazarus Snapshots Downloads/fi")** │  **[français (fr)](</Lazarus_Snapshots_Downloads/fr> "Lazarus Snapshots Downloads/fr")** │  **[magyar (hu)](</Lazarus_Snapshots_Downloads/hu> "Lazarus Snapshots Downloads/hu")** │  **[italiano (it)](</Lazarus_Snapshots_Downloads/it> "Lazarus Snapshots Downloads/it")** │  **[português (pt)](</Lazarus_Snapshots_Downloads/pt> "Lazarus Snapshots Downloads/pt")** │  **[русский (ru)](<../ru/Lazarus_Snapshots_Downloads.md> "Lazarus Snapshots Downloads/ru")** │  **[中文（中国大陆） (zh_CN)](</Lazarus_Snapshots_Downloads/zh_CN> "Lazarus Snapshots Downloads/zh CN")** │  **[中文（臺灣） (zh_TW)](</Lazarus_Snapshots_Downloads/zh_TW> "Lazarus Snapshots Downloads/zh TW")** │    
****

## Contents

  * 1 Unofficial macOS Lazarus and FPC Snapshots
  * 2 Official Snapshots
    * 2.1 Download locations
    * 2.2 Change log for snapshot building
  * 3 Download (Source ZIP) from a GitHub mirror
  * 4 See also



## Unofficial macOS Lazarus and FPC Snapshots

I am currently making daily, automated, untested builds of the Lazarus development version (trunk in svn, main in git) for macOS -- Cocoa, 64 bit, Intel + aarch64 (M1) processors -- available on [SourceForge](<https://sourceforge.net/projects/macos-lazarus-snapshots/>). 

There is also a daily, automated, untested build of the development version (trunk in svn, main in git) of the Free Pascal Compiler for macOS -- aarch64 (M1) processor only. 

The only guarantee is that the software compiled successfully using a recent development version of FPC. THIS IS COMPLETELY UNTESTED SOFTWARE. Caveat Utilitor! 

## Official Snapshots

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Snapshots are currently not provided. Links in this section are outdated, any Snapshots still found may be well older than current releases.  
This page is kept in the hope that at some time it may be restored.

Snapshots are nightly builds of lazarus using the latest available sources and with the latest fixes and new bugs. There are several lazarus snapshots available: 

  * Source: Lazarus fixes in zip and bz2
  * Source: Lazarus in zip and bz2
  * Installer win32: Lazarus fixes + fpc 2.4.4 win32
  * Installer win32: Lazarus + fpc 2.4.4 win32 / win32-arm-wince
  * Installer win64: Lazarus + fpc 2.4.4 win64
  * Linux source rpms: fpc (to create fpc and fpc-src rpms) and lazarus
  * Linux i386 rpms: fpc, fpc-src, lazarus
  * Linux i386 debs tar: fpc, lazarus
  * Linux x86_64 rpms: fpc, fpc-src, lazarus
  * Linux x86_64 debs tar: fpc, lazarus
  * Mac OS X powerpc dmgs: fpc, fpcsrc and lazarus
  * Mac OS X i386 dmgs: fpc, fpcsrc and lazarus



Some mirrors only have a few snapshot types. Look in the table below for details. FPC 2.4.5 available only in Germany mirror. 

### Download locations

Location | Url | Status   
---|---|---  
Main | <https://sourceforge.net/projects/lazarus-snapshots> | Currently not updated   
Main (old) | <http://www.hu.freepascal.org/lazarus/> | No longer exists   
France | <http://mirrors.iwi.me/lazarus/snapshots/> | No longer exists   
Germany | <http://michael-ep3.physik.uni-halle.de/Lazarus/> | Currently not updated   
USA | <http://www.lazarus.maro.net/> | Currently not updated   
  
~~I have written a program with fpcunit and[lNet](<lNet.md> "lNet") to check, if the daily snapshots are refreshed. You can view the [report](<http://www.hu.freepascal.org/lazarus/testresults/monitor.xml>), which is refreshed every 15 minutes.~~ stopped working. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** These snapshots are generated automatically and are untested. The only thing we can say is, that the compiler found the source good enough to compile. These snapshots are provided as a courtesy only. If they don't work, too bad! If they destroy your project files, crash your machine, and eat your disk: tough luck! (just to say that YOU ARE USING COMPLETELY UNTESTED SOFTWARE)__

###  Change log for snapshot building

  * 2011.06.28 Snapshots with new fpc release.
  * 2006.08.01: Snapshots for i386-win32, i386-linux and x86_64-linux are built with fpc 2.0.4-rc2, to give more people the opportunity to test this release candidate.
  * 2006.08.04: powerpc-darwin snapshot based on fpc 2.0.4-rc2 uploaded.
  * 2006.08.13: i386-win32, i386-linux, x86_64-linux and powerpc-darwin snapshots have been built with fpc 2.0.4-rc3
  * 2006.10.02: win32 installer comes in two varieties: one with fpc 2.0.4 and one with fpc 2.1.1
  * 2007.06.24: win32 and win64 use a patched version of fpc 2.1.5 named 2.1.5-L, see [here](</index.php?title=Useful_changes_not_in_the_fixes_branch&action=edit&redlink=1> "Useful changes not in the fixes branch \(page does not exist\)") for details.
  * 2007.07: no more patched builds, because all pending patches were merged.
  * 2007.09.20: all fpc 2.0.4 snapshots have been replaced by fpc 2.2.0 snapshots.



## Download (Source ZIP) from a GitHub mirror

There are several unofficial GitHub mirrors. They allow you to download sources as a zip (including any chosen older commit). To download a zip, open the menu from the "Code" button or use the link below. 

To check if a mirror is still updating look at the date of the last commit. 

Project | Browse repro | Latest source Zip | Status   
---|---|---|---  
Lazarus | <https://github.com/fpc/Lazarus> | <https://github.com/fpc/Lazarus/archive/refs/heads/main.zip> | Up to date   
FPC | <https://github.com/fpc/FPCSource/> | <https://github.com/fpc/FPCSource/archive/refs/heads/main.zip> | Up to date   
  
## See also

  * [Installing Lazarus](<Installing_Lazarus.md> "Installing Lazarus") \- Official binary installations and installation from source.
  * [Installing the Free Pascal Compiler](<Installing_the_Free_Pascal_Compiler.md> "Installing the Free Pascal Compiler") Official binary installations and installation from source.
  * [FpcUpDeluxe](<https://github.com/newpascal/fpcupdeluxe/releases/latest>) can be used to acquire and build a required version of FPC and Lazarus.

---

_Source: [https://wiki.freepascal.org/Lazarus_Snapshots_Downloads](https://web.archive.org/web/20241207112013/https://wiki.freepascal.org/Lazarus_Snapshots_Downloads)_
