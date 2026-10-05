# Issues 3.2.2

## Contents

  * 1 Issues with 3.2.2 release candidates
  * 2 Issues with 3.2.2-rc1
    * 2.1 General
    * 2.2 macOS
      * 2.2.1 Getting Started (Intel and ARM64).rtf
    * 2.3 Linux
    * 2.4 Windows any
    * 2.5 Windows 7 32-bit
    * 2.6 Windows 10 64-bit



## Issues with 3.2.2 release candidates

This page is supposed to contain all issues discovered while testing release candidates and their status. Make sure to include information about the platform where the problem appears (if applicable). Once the issue gets resolved, you should strike it over and add number of SVN revision fixing it. If the revision number refers to a SVN repository different from "fpc" (i.e. "fpcbuild" or "fpcdocs"), include information about the SVN repository too. 

## Issues with 3.2.2-rc1

### General

  * Readme, news, and FAQ need to be updated for 3.2.2 **mvdv: readme is already ok, faq needs update, news is webpage? That is usually done post release** **K: Sorry, I mean the whatsnew.txt that's along the other text docs.**
  * Compiler's copyright date should be updated to 2021 **fixed: mvdv r49309**
  * IDE > Help > Using help: just shows an empty box, with or without CHM files set up. (Same behavior on 3.2.0 so not a regression.) **MvdV: I think this is back from the day we still released with html docs so they were generally present. No solution other than distributing CHM? Might be worth a mantis item**
  * <ftp://ftp.freepascal.org/pub/fpc/beta/3.2.2-rc1/docs/doc-chm.zip> version numbers should be updated for 3.2.2 **MvdV: Michael says that doc version numbers have been meanwhile updated in SVN**
  * The language reference, user guide, and programmer's guide CHM's aren't rendering correctly in the IDE; codepage problem? **Mvdv: YES, chms were switched to utf8 recently, as the IDE is not the only consumer. Not fixable on release timescales? Also worth a mantis item to track. Most notable in the accents in Michael and Florian's names. Bugreport worthy too since windows terminal/console is in flux in this regard**
  * Most PDF document files also need to be updated for 3.2.2 **docs will be regenerated from svn**



### macOS

#### Getting Started (Intel and ARM64).rtf

"Requirements: 

  * Intel: Mac OS X 10.6.0 or later with Xcode and the Xcode command line tools installed.
  * ARM64: macOS 11.0 or later with Xcode and the Xcode command line tools installed."



Is it really necessary for every user to instal the 16GB of Xcode as well as the Xcode Command Line Tools package? From my experiments, it seems the full Xcode is only needed to: 

  * access the non-macOS SDKs for iOS, iPadOS, watchOS and tvOS; or
  * validate and upload applications to the Mac App Store; or
  * notarize applications for distribution outside of the Mac App Store.



Note: xcode-select (for installing the CLT package) comes bundled with macOS. 

  * **Indeed, this was a copy/paste from older Xcode versions, where installing the command line tools had to be done from inside the Xcode application (or System Preferences are installing Xcode)** — [Jonas](</User:Jonas> "User:Jonas") ([talk](</User_talk:Jonas> "User talk:Jonas"))



Suggestion: Add link to the FPC + Lazarus Wiki. 

  * **Done** — [Jonas](</User:Jonas> "User:Jonas") ([talk](</User_talk:Jonas> "User talk:Jonas"))



### Linux

  * On a 64-bit archlinux laptop, running "fppkg update -v" errored with "Warning: Host name resolution for "www.freepascal.org" failed." I'm not sure why; I can ping the URL on the same machine without problems, and use elinks to fetch the exact xml file fppkg was trying to get. Could be specific to this system.
  * On a 64-bit archlinux laptop, two notable test failures: webtbs/tw9089d failed with exit code 1, and webtbf/tw22665b passed unexpectedly. I don't see these failures in the automatic test suite results for 3.2.0 or 3.3.1, but I'm getting them both when testing 3.2.0 and 3.2.2rc1, so it'll be due to something system-specific and not a regression.



### Windows any

  * The x32+x64 installer shows an introduction text when installation is done; afterward, you can view the included readme.txt which is similar. Both say version 3.2.2rc1 at the top, but the text body describes 3.2.0 and older versions only. **MvdV: I will pay attention to this when building new installers**
  * Running the x86_64-win64-windres.exe errors out with "'x86_64-win64-gcc' is not recognized as an internal or external command, operable program or batch file." There is a gcc.exe among the provided binaries, but no x86_64 gcc. (The 64-bit crosscompiler appears to work correctly on the 32-bit system regardless.) **mvdv: nice catch! The gcc is a dummy file, but indeed the cross-windres will prefix target string to the gcc.exe search. Copying normal gcc.exe to x86_64-win64-gcc.exe helps**
  * "fppkg update" failed with "Could not initialize OpenSSL library".



### Windows 7 32-bit

  * Compared to automatic testsuite 3.2.0 run 503531, there is one new failure: webtbs/tw37926.pp: Floating point overflow RTE 217. This is also appearing in 3.3.1 testruns, so it's a known failure.



### Windows 10 64-bit

  * The IDE couldn't compile hello.pp due to not finding system.ppu. Worked fine compiling on commandline with both ppc386 and ppcrossx64. Had to add unit path to IDE manually. Could be something on my system only. **MvdV: Which installer is this? Only the installer does any preconfiguring of e.g. fpc.cfg and ide**



**K: This was fpc-3.2.2rc1.win32.and.win64.exe. However, after fully uninstalling and deleting FPC, then reinstalling to a different directory, the IDE compiled hello.pp fine. I must have selected a wrong option somewhere when first starting the IDE.**

  * test/packages/fcl-registry/tw35060a.pp: OpenKey('\Software\FPCBug0035060\aei') failed: "The operation completed successfully."
  * test/packages/fcl-registry/tw35060c.pp: OpenKey('\Software\FPCBug0035060\aei') failed: "The operation completed successfully."
  * test/packages/zlib/tzlib1.pp: Exitcode -1073741515 (expected 0)
  * test/units/sysutils/tffirst.pp: exit code 2
  * webtbs/tw11039a.pp: Error while linking
  * webtbs/tw11039b.pp: Error while linking
  * webtbs/tw29010b.pp: Runtime error 216

---

_Source: [https://wiki.freepascal.org/Issues_3.2.2](https://web.archive.org/web/20241202161300/https://wiki.freepascal.org/Issues_3.2.2)_
