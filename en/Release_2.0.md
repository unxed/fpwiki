# Release 2.0.4

## Contents

  * 1 Release organization
  * 2 Release procedure (Release Template)
    * 2.1 Release preparations
    * 2.2 RC1
    * 2.3 RC2
    * 2.4 RC3
    * 2.5 Final release
    * 2.6 Going public



## Release organization

  1. All patches important for 2.0.4 should be submitted by the end of June.
  2. New branch for the release will be created from the fixes branch on the 2nd July (Sunday) before lunch. Once the branch is ready, I'll announce it here. Everybody can start to build release candidate of his platforms (version number in file names should be appended with "-rcN") at that moment (I'll try to keep track of who is doing what in Wiki). URL of SVN branch for release candidate 1 is <http://svn.freepascal.org/svn/fpcbuild/tags/release_2_0_4_rc1/> (note that with svn+ssh: protocol this becomes svn+<ssh://svn.freepascal.org/FPC/svn/fcpbuild/tags/release_2_0_4_rc1/>).
  3. As much as possible should be tested and fixed by July 15 (Saturday). However, fixes are _not_ applied to the release branch, but rather just marked with "2.0.4 fix:" in SVN comment (and merged to the fixes branch as appropriate). Please, make sure that only real fixes are marked this way, no last moment "features" this time (once again, all required features should be committed before end of June).
  4. Encountered issues (and their status) should be documented [here](<Issues_2.0.md> "Issues 2.0.4").
  5. I'll merge the marked patches to the release branch on July 16 (Sunday), announce it and then the new testing cycle can start. As soon as no major issues are encountered, real release will be published.



## Release procedure ([Release Template](<Release_Template.md> "Release Template"))

Note that detailed information for some tasks can be found in [Release engineering](<Release_engineering.md> "Release engineering"). These two pages should be eventually merged (or probably rather better interlinked) in the future. 

### Release preparations

  1. ~~Agree on deadline for changes~~ (done)
  2. ~~New page in Wiki for release procedure with steps needed and their status (this page), at the beginning consisting of RC1 and final release sections~~ (done)
  3. Check and update all .msg files - running, coordinated by Tomas 
     1. errore.msg - John?
     2. ~~errord.msg - Karl-Michael Schindler~~ (done)
     3. errorf.msg - Remi Dorlet and "Oro06"/"Olivier"
     4. ~~errorn.msg - Matthijs Willemstein~~ (done)
     5. errorr.msg - Michail Baikov
     6. errorrw.msg - Michail Baikov
     7. errorues.msg
     8. errores.msg
     9. errorct.msg
     10. ~~errorhe.msg - Ido Kanner~~ (done)
     11. ~~errorptd.msg - Ari Ricardo Ody~~ (done)
     12. ~~errorptw.msg - Ari Ricardo Ody~~ (done)
     13. errorpli.msg - Wojciech Malinowski
     14. errorpl.msg - Wojciech Malinowski
     15. ~~errorid.msg - Zaenal Mutaqin~~ (done)
  4. ~~Check tools~~ (no updates this time) 
     1. Check version of the above mentioned tools (GNU tools, helper DLLs, UPX, etc.), and decide whether it isn't time to update some of these tools
     2. Repackage and upload additional tools where needed
  5. ~~Update whatsnew.txt (/install/doc/whatsnew.txt) - Florian~~ (done)
  6. ~~Finish all source file updates~~ \- **July 1**
  7. ~~Update version number to new release number - Tomas~~ (done) 
     1. ~~/install/doc/readme.txt~~ (done)
     2. ~~/installer/install.dat (header)~~ (done)
     3. ~~/installer/install.pas (installer version)~~ (done)
     4. /install/fpc.spec
     5. /install/install.sh _Done by makepack_
     6. ~~convert /html/faq.fp to /install/doc/faq.htm and /install/doc/faq.txt~~ (done)
     7. ~~/docs/fpc.sty (macro fpcversion)~~ (done)
     8. ~~All Makefile.fpc files containing version=... (plus regenerate all corresponding Makefiles)~~ (done)



### RC1

  1. ~~Create new page in Wiki with issue log for documentation of issues encountered in release candidates and their status~~ (done)
  2. ~~New directories~~ (done) 
     1. ~~Create new directories on ftp~~ (done)
     2. ~~Copy the extra files (asld*.zip, gdb*.zip, make*.zip) from previous release (unless updated with new versions)~~ (done)
  3. ~~Create new rc1 branch in SVN -**July 2** , Tomas~~ (done)
  4. ~~Update version number in rc1 branch to #.#.#-rc# -**July 2** , Tomas~~ (done) 
     1. ~~/compiler/version.pas -**July 2** , Tomas~~ (done)
  5. ~~Create and upload exported fpcbuild.zip -**July 2** , Tomas~~ (done)
  6. Create and upload the documentation - Michael 
     1. ~~doc-pdf.zip~~ (done)
     2. ~~doc-html.zip~~ (done)
     3. ~~doc-htm.zip~~ (not included)
     4. ~~doc-txt.zip~~ (done)
     5. ~~doc-ps.zip~~ (done)
     6. ~~doc-pdf.tar.gz~~ (done)
     7. ~~doc-html.tar.gz~~ (done)
     8. ~~doc-ps.tar.gz~~ (done)
  7. Create and upload source zips 
     1. ~~short name version for binary packages - Tomas~~ (done)
     2. short name version for docs source
     3. long name version for binary packages
     4. long name version for docs source
  8. Create and upload source tars
  9. ~~Create and upload zip files for i386-go32v2 - Tomas~~ (done) 
     1. ~~basic zip (just binaries)~~ (done)
     2. ~~full zip (including docs and sources)~~ (done)
  10. ~~Create and upload zip files for i386-os2 - Tomas~~ (done) 
     1. ~~basic zip (just binaries)~~ (done)
     2. ~~full zip (including docs and sources)~~ (done)
  11. ~~Create and upload installer exe for i386-win32 - Florian~~ (done)
  12. Create and upload tgz file for i386-freebsd - Marco
  13. Create and upload release for i386-linux 
     1. ~~tar - Michael~~ (done)
     2. deb - Peter
     3. rpm - Michael
  14. Create and upload release for powerpc-linux 
     1. ~~tar - Thomas~~ (done)
  15. Create and upload release for powerpc-macosx - Jonas 
     1. ~~.dmg package~~ (done)
     2. ~~.info files for fink - Mischi~~ (done)
  16. Create and upload release for powerpc-macos
  17. Create and upload release for powerpc-morphos - Charlie
  18. Create and upload zip files for i386-netware
  19. Create and upload zip files for i386-netwlibc
  20. Create and upload release for sparc-linux 
     1. tar
  21. Create and upload release for arm-linux (cross-release) 
     1. tar - Daniel
  22. Create and upload release for x86_64-linux 
     1. tar - Michael
  23. Run makereleasezips (combine small ZIP files together)
  24. ~~Consider announcing availability of the new RC in fpc-devel list and/or ask there for dedicated testers for individual platforms~~ (done)
  25. Test the GO32v2 release 
     1. dos???.zip installation
     2. dos???full.zip installation (over the previously installed dos???.zip to simulate updates)
     3. make sure readme.txt & whatsnew.txt are for the current version
     4. run all executables in /bin/go32v2 (no params)
     5. make cycle with newly installed binaries and sources
     6. run testsuite
     7. open the installed hello.pp in IDE
     8. make a minor change in the demo in IDE & save it
     9. view documentation in IDE, traverse 2-3 pages (at least one with screenshots)
     10. compile the demo file in IDE
     11. run the demo within the IDE (debugger)
  26. Test the OS/2 release 
     1. os2???.zip installation
     2. os2???full.zip installation (over the previously installed os2???.zip to simulate updates)
     3. make sure readme.txt & whatsnew.txt are for the current version
     4. run all executables in /bin/os2 (no params)
     5. make cycle with newly installed binaries and sources
     6. run testsuite
     7. open the installed hello.pp in IDE
     8. make a minor change in the demo in IDE & save it
     9. view documentation in IDE, traverse 2-3 pages (at least one with screenshots)
     10. compile the demo file in IDE
     11. ~~run the demo within the IDE (debugger)~~ (not supported)
  27. Test the Win32 release 
     1. ~~Run installer~~ Luiz (Done)
     2. ~~make sure readme.txt & whatsnew.txt are for the current version~~ Luiz (Done)
     3. ~~run all executables in /bin/win32 (without parameters)~~ Luiz (Done)
     4. make cycle with newly installed binaries and sources
     5. run testsuite
     6. ~~open the installed hello.pp in IDE~~ Luiz (Done)
     7. ~~make a minor change in the demo in IDE & save it~~ Luiz (Done)
     8. view documentation in IDE, traverse 2-3 pages (at least one with screenshots)
     9. ~~compile the demo file in IDE~~ Luiz (Done)
     10. ~~run the demo within the IDE (debugger)~~ Luiz (Done)
  28. Test release for i386-freebsd
  29. Test release for i386-linux 
     1. tar
     2. deb
     3. rpm
  30. Test release for powerpc-linux 
     1. tar
  31. Test release for powerpc-macosx 
     1. .dmg package
     2. .info files for fink - Mischi
  32. Test release for powerpc-macos
  33. Test release for powerpc-morphos
  34. Test release for i386-netware
  35. Test release for i386-netwlibc
  36. Test release for sparc-linux 
     1. tar
  37. Test release for arm-linux 
     1. tar
  38. Test release for x86_64-linux
  39. Check PDF documentation (open all files)
  40. Check HTML documentation
  41. Check TXT documentation
  42. Read updated text files as distributed in release zip files 
     1. readme.txt
     2. faq.txt
     3. whatsnew.txt



### RC2

  1. ~~Section for new RC on release pages in Wiki~~ (done) 
     1. ~~Release procedure~~ (done)
     2. ~~Issue log~~ (done)
  2. ~~Look at unmerged changes in fpc and fpcbuild since the last RC and consider/ask for necessity of their inclusion in the release~~ (done)
  3. ~~New directories~~ (done) 
     1. ~~Create new directories on ftp~~ (done)
     2. ~~Copy the extra files (asld*.zip, gdb*.zip, make*.zip) from previous RC (unless updated with new versions)~~ (done)
  4. ~~Create new rc2 branch in SVN - July 16, Tomas~~ (done)
  5. ~~Update version number in rc2 branch to #.#.#-rc# - Tomas~~ (done) 
     1. ~~/compiler/version.pas - Tomas~~ (done)
  6. ~~Create and upload exported fpcbuild.zip - Tomas~~ (done)
  7. ~~Create and upload the documentation - Michael~~ (done) 
     1. ~~doc-pdf.zip~~ (done)
     2. ~~doc-html.zip~~ (done)
     3. _doc-htm.zip_ (not included)
     4. ~~doc-txt.zip~~ (done)
     5. ~~doc-ps.zip~~ (done)
     6. ~~doc-pdf.tar.gz~~ (done)
     7. ~~doc-html.tar.gz~~ (done)
     8. ~~doc-ps.tar.gz~~ (done)
  8. Create and upload source zips 
     1. ~~short name version for binary packages - Tomas~~ (done)
     2. ~~docs source (including the link for short name version) - Tomas~~ (done)
     3. ~~long name version for binary packages - Tomas~~ (done)
  9. Create and upload source tars
  10. ~~Create and upload zip files for i386-go32v2 - Tomas~~ (done) 
     1. ~~basic zip (just binaries)~~ (done)
     2. ~~full zip (including docs and sources)~~ (done)
  11. ~~Create and upload zip files for i386-os2 - Tomas~~ (done) 
     1. ~~basic zip (just binaries)~~ (done)
     2. ~~full zip (including docs and sources)~~ (done)
  12. ~~Create and upload installer exe for i386-win32 - Florian~~ (done)
  13. ~~Create and upload tgz file for i386-freebsd - Marco~~ (done)
  14. Create and upload release for i386-linux 
     1. ~~tar - Michael~~ (done)
     2. ~~deb - Mazen~~ (done)
     3. ~~rpm - Michael~~ (done)
  15. Create and upload release for powerpc-linux 
     1. ~~tar - Thomas/Daniel~~ (done - no smartlinked version with GDB available)
  16. ~~Create and upload release for powerpc-macosx - Jonas, July 19~~
     1. ~~.dmg package~~ (done)
     2. ~~.info files for fink - Mischi~~
  17. Create and upload release for powerpc-macos
  18. Create and upload release for powerpc-morphos - Charlie
  19. Create and upload zip files for i386-netware
  20. Create and upload zip files for i386-netwlibc
  21. Create and upload release for sparc-linux 
     1. tar
  22. ~~Create and upload release for arm-linux (cross-release)~~ (done) 
     1. ~~tar - Daniel~~ (done)
  23. ~~Create and upload release for x86_64-linux~~ (done) 
     1. ~~tar - Michael~~ (done)
  24. Consider announcing availability of the new RC in fpc-devel list and/or ask there for dedicated testers for individual platforms
  25. Test the GO32v2 release 
     1. dos???.zip installation
     2. dos???full.zip installation (over the previously installed dos???.zip to simulate updates)
     3. make sure readme.txt & whatsnew.txt are for the current version
     4. run all executables in /bin/go32v2 (no params)
     5. make cycle with newly installed binaries and sources
     6. run testsuite
     7. open the installed hello.pp in IDE
     8. make a minor change in the demo in IDE & save it
     9. view documentation in IDE, traverse 2-3 pages (at least one with screenshots)
     10. compile the demo file in IDE
     11. run the demo within the IDE (debugger)
  26. Test the OS/2 release 
     1. os2???.zip installation
     2. os2???full.zip installation (over the previously installed os2???.zip to simulate updates)
     3. make sure readme.txt & whatsnew.txt are for the current version
     4. run all executables in /bin/os2 (no params)
     5. make cycle with newly installed binaries and sources
     6. run testsuite
     7. open the installed hello.pp in IDE
     8. make a minor change in the demo in IDE & save it
     9. view documentation in IDE, traverse 2-3 pages (at least one with screenshots)
     10. compile the demo file in IDE
     11. _run the demo within the IDE (debugger)_ (not supported)
  27. Test the Win32 release 
     1. ~~Run installer - Luiz~~ Done
     2. ~~make sure readme.txt & whatsnew.txt are for the current version - Luiz~~ Done
     3. ~~run all executables in /bin/win32 (without parameters) - Luiz~~ Done
     4. ~~make cycle with newly installed binaries and sources - Luiz~~ Done
     5. ~~run testsuite - Luiz~~ Done
     6. ~~open the installed hello.pp in IDE - Luiz~~ Done
     7. ~~make a minor change in the demo in IDE & save it - Luiz~~ Done
     8. ~~view documentation in IDE, traverse 2-3 pages (at least one with screenshots) - Luiz~~ Done
     9. ~~compile the demo file in IDE - Luiz~~ Done
     10. ~~run the demo within the IDE (debugger) - Luiz~~ Done
  28. Test release for i386-freebsd
  29. Test release for i386-linux 
     1. tar
     2. deb
     3. rpm
  30. Test release for powerpc-linux 
     1. tar
  31. ~~Test release for powerpc-macosx~~
     1. ~~.dmg package~~
     2. ~~.info fink installation~~
  32. Test release for powerpc-macos
  33. Test release for powerpc-morphos
  34. Test release for i386-netware
  35. Test release for i386-netwlibc
  36. Test release for sparc-linux 
     1. tar
  37. Test release for arm-linux 
     1. tar
  38. Test release for x86_64-linux
  39. Check PDF documentation (open all files)
  40. Check HTML documentation
  41. Check TXT documentation
  42. Read updated text files as distributed in release zip files 
     1. readme.txt
     2. faq.txt
     3. whatsnew.txt



### RC3

  1. ~~Section for new RC on release pages in Wiki~~ (done) 
     1. ~~Release procedure~~ (done)
     2. ~~Issue log~~ (done)
  2. ~~Look at unmerged changes in fpc and fpcbuild since the last RC and consider/ask for necessity of their inclusion in the release~~ (done)
  3. ~~New directories~~ (done) 
     1. ~~Create new directories on ftp~~ (done)
     2. ~~Copy the extra files (asld*.zip, gdb*.zip, make*.zip) from previous RC (unless updated with new versions)~~ (done)
  4. ~~Update path and file names in /install/macosx/*.info for the new RC~~ (done)
  5. ~~Create new RC branch in SVN - Tomas~~ (done)
  6. ~~Update version number in RC branch to #.#.#-rc# - Tomas~~ (done) 
     1. ~~/compiler/version.pas - Tomas~~ (done)
  7. ~~Create and upload exported fpcbuild - Tomas~~ (done) 
     1. ~~fpcbuild-%{version}.zip~~ (done)
     2. ~~fpcbuild-%{version}.tar.gz~~ (done)
  8. ~~Create and upload the documentation - Michael~~ (done) 
     1. ~~doc-pdf.zip~~ (done)
     2. ~~doc-html.zip~~ (done)
     3. _doc-htm.zip_ (not included)
     4. ~~doc-txt.zip~~ (done)
     5. ~~doc-ps.zip~~ (done)
     6. ~~doc-pdf.tar.gz~~ (done)
     7. ~~doc-html.tar.gz~~ (done)
     8. ~~doc-ps.tar.gz~~ (done)
  9. ~~Create and upload source zips~~ (done) 
     1. ~~short name version for binary packages - Tomas~~ (done)
     2. ~~docs source (including link for short name version) - Tomas~~ (done)
     3. ~~long name version for binary packages - Tomas~~ (done)
  10. ~~Create and upload zip files for i386-go32v2 - Tomas~~ (done) 
     1. ~~basic zip (just binaries)~~ (done)
     2. ~~full zip (including docs and sources)~~ (done)
  11. Create and upload zip files for i386-os2 - Tomas 
     1. basic zip (just binaries)
     2. full zip (including docs and sources)
  12. ~~Create and upload installer exe for i386-win32 - Peter~~ (done)
  13. ~~Create and upload tgz file for i386-freebsd - Marco~~ (done)
  14. ~~Create and upload release for i386-linux~~ (done) 
     1. ~~tar - Michael~~ (done)
     2. ~~deb - Mazen~~ (done)
     3. ~~rpm - Michael~~ (done)
  15. ~~Create and upload release for powerpc-linux~~ (done) 
     1. ~~tar - Daniel~~ (done)
  16. ~~Create and upload release for powerpc-macosx - Jonas~~ (done) 
     1. ~~.dmg package~~ (done)
     2. .info files for fink - Mischi
  17. Create and upload release for powerpc-macos
  18. Create and upload release for powerpc-morphos - Charlie
  19. Create and upload zip files for i386-netware
  20. Create and upload zip files for i386-netwlibc
  21. Create and upload release for sparc-linux 
     1. tar
  22. ~~Create and upload release for arm-linux (cross-release)~~ (done) 
     1. ~~tar - Daniel~~ (done)
  23. ~~Create and upload release for x86_64-linux~~ (done) 
     1. ~~tar - Michael~~ (done)
  24. ~~Consider announcing availability of the new RC in fpc-devel list and/or ask there for dedicated testers for individual platforms~~ (done)
  25. ~~Test the GO32v2 release - Laaca~~ (done) 
     1. dos???.zip installation
     2. dos???full.zip installation (over the previously installed dos???.zip to simulate updates)
     3. make sure readme.txt & whatsnew.txt are for the current version
     4. run all executables in /bin/go32v2 (no params)
     5. make cycle with newly installed binaries and sources
     6. run testsuite
     7. open the installed hello.pp in IDE
     8. make a minor change in the demo in IDE & save it
     9. view documentation in IDE, traverse 2-3 pages (at least one with screenshots)
     10. compile the demo file in IDE
     11. run the demo within the IDE (debugger)
  26. Test the OS/2 release 
     1. os2???.zip installation
     2. os2???full.zip installation (over the previously installed os2???.zip to simulate updates)
     3. make sure readme.txt & whatsnew.txt are for the current version
     4. run all executables in /bin/os2 (no params)
     5. make cycle with newly installed binaries and sources
     6. run testsuite
     7. open the installed hello.pp in IDE
     8. make a minor change in the demo in IDE & save it
     9. view documentation in IDE, traverse 2-3 pages (at least one with screenshots)
     10. compile the demo file in IDE
     11. _run the demo within the IDE (debugger)_ (not supported)
  27. Test the Win32 release 
     1. ~~Run installer - Luiz~~
     2. ~~make sure readme.txt & whatsnew.txt are for the current version - Luiz~~
     3. run all executables in /bin/win32 (without parameters) - Luiz
     4. make cycle with newly installed binaries and sources
     5. run testsuite
     6. ~~open the installed hello.pp in IDE - Luiz~~
     7. ~~make a minor change in the demo in IDE & save it - Luiz~~
     8. ~~view documentation in IDE, traverse 2-3 pages (at least one with screenshots)~~
     9. ~~compile the demo file in IDE - Luiz~~
     10. ~~run the demo within the IDE (debugger) - Luiz~~
  28. Test release for i386-freebsd
  29. Test release for i386-linux 
     1. tar
     2. deb
     3. rpm
  30. Test release for powerpc-linux 
     1. tar
  31. Test release for powerpc-macosx 
     1. .dmg package
  32. Test release for powerpc-macos
  33. Test release for powerpc-morphos
  34. Test release for i386-netware
  35. Test release for i386-netwlibc
  36. Test release for sparc-linux 
     1. tar
  37. Test release for arm-linux 
     1. tar
  38. Test release for x86_64-linux
  39. Check PDF documentation (open all files)
  40. Check HTML documentation
  41. Check TXT documentation
  42. Read updated text files as distributed in release zip files 
     1. readme.txt
     2. faq.txt
     3. whatsnew.txt



### Final release

  1. ~~Look at unmerged changes in fpc and fpcbuild since the last RC and consider/ask for necessity of their inclusion in the release (only cosmetic changes should be included, otherwise a new RC is needed instead of final release)~~ (done)
  2. ~~New directories~~ (done) 
     1. ~~Create new directories on ftp~~ (done)
     2. ~~Copy the extra files (asld*.zip, gdb*.zip, make*.zip) from previous RC (unless updated with new versions)~~ (done)
  3. ~~Update path and file names in /install/macosx/*.info for the new release~~ (done)
  4. ~~Add new section for the upcoming build in /install/debian/changelog~~ (done)
  5. ~~Create new release branch in SVN - Tomas~~ (done)
  6. ~~Update version number in release branch to the #.#.# - Tomas~~ (done) 
     1. ~~/compiler/version.pas - Tomas~~ (done)
  7. ~~Create and upload exported fpcbuild - Tomas~~ (done) 
     1. ~~fpcbuild-%{version}.zip~~ (done)
     2. ~~fpcbuild-%{version}.tar.gz~~ (done)
  8. ~~Create and upload the documentation - Michael~~ (done) 
     1. ~~doc-pdf.zip~~ (done)
     2. ~~doc-html.zip~~ (done)
     3. _doc-htm.zip_ (not included)
     4. ~~doc-txt.zip~~ (done)
     5. ~~doc-ps.zip~~ (done)
     6. ~~doc-pdf.tar.gz~~ (done)
     7. ~~doc-html.tar.gz~~ (done)
     8. ~~doc-ps.tar.gz~~ (done)
  9. ~~Create and upload source zips~~ (done) 
     1. ~~short name version for binary packages - Tomas~~ (done)
     2. ~~docs source (including link for short name version) - Tomas~~ (done)
     3. ~~long name version for binary packages - Tomas~~ (done)
  10. ~~Create and upload zip files for i386-go32v2 - Tomas~~ (done) 
     1. ~~basic zip (just binaries)~~ (done)
     2. ~~full zip (including docs and sources)~~ (done)
  11. ~~Create and upload zip files for i386-os2 - Tomas~~ (done) 
     1. ~~basic zip (just binaries)~~ (done)
     2. ~~full zip (including docs and sources)~~ (done)
  12. ~~Create and upload installer exe for i386-win32 - Florian~~ (done)
  13. ~~Create and upload tgz file for i386-freebsd - Marco~~ (done)
  14. ~~Create and upload release for i386-linux~~ (done) 
     1. ~~tar - Michael~~ (done)
     2. ~~deb - Mazen~~ (done)
     3. ~~rpm - Michael~~ (done)
  15. ~~Create and upload release for powerpc-linux~~ (done) 
     1. ~~tar - Daniel~~ (done)
  16. ~~Create and upload release for powerpc-macosx - Jonas~~ (done) 
     1. ~~.dmg package~~ (done)
  17. Create and upload release for powerpc-macos
  18. ~~Create and upload release for powerpc-morphos - Charlie~~ (done)
  19. Create and upload zip files for i386-netware
  20. Create and upload zip files for i386-netwlibc
  21. Create and upload release for sparc-linux 
     1. tar
  22. ~~Create and upload release for arm-linux (cross-release)~~ (done) 
     1. ~~tar - Daniel~~ (done)
  23. ~~Create and upload release for x86_64-linux~~ (done) 
     1. ~~tar - Michael~~ (done)
  24. Test the GO32v2 release 
     1. dos???.zip installation
     2. dos???full.zip installation (over the previously installed dos???.zip to simulate updates)
     3. make sure readme.txt & whatsnew.txt are for the current version
     4. run all executables in /bin/go32v2 (no params)
     5. make cycle with newly installed binaries and sources
     6. run testsuite
     7. open the installed hello.pp in IDE
     8. make a minor change in the demo in IDE & save it
     9. view documentation in IDE, traverse 2-3 pages (at least one with screenshots)
     10. compile the demo file in IDE
     11. _run the demo within the IDE (debugger)_ (debugging in IDE currently not available/working)
  25. Test the OS/2 release 
     1. os2???.zip installation
     2. os2???full.zip installation (over the previously installed os2???.zip to simulate updates)
     3. make sure readme.txt & whatsnew.txt are for the current version
     4. run all executables in /bin/os2 (no params)
     5. make cycle with newly installed binaries and sources
     6. run testsuite
     7. _open the installed hello.pp in IDE_ (IDE not included this time)
     8. _make a minor change in the demo in IDE & save it_ (IDE not included this time)
     9. _view documentation in IDE, traverse 2-3 pages (at least one with screenshots)_ (IDE not included this time)
     10. _compile the demo file in IDE_ (IDE not included this time)
     11. _run the demo within the IDE (debugger)_ (not supported)
  26. Test the Win32 release 
     1. Run installer - Luiz
     2. make sure readme.txt & whatsnew.txt are for the current version - Luiz
     3. run all executables in /bin/win32 (without parameters) - Luiz
     4. make cycle with newly installed binaries and sources
     5. run testsuite
     6. open the installed hello.pp in IDE - Luiz
     7. make a minor change in the demo in IDE & save it - Luiz
     8. view documentation in IDE, traverse 2-3 pages (at least one with screenshots)
     9. compile the demo file in IDE - Luiz
     10. run the demo within the IDE (debugger) - Luiz
  27. Test release for i386-freebsd
  28. Test release for i386-linux 
     1. tar
     2. deb
     3. rpm
  29. Test release for powerpc-linux 
     1. tar
  30. Test release for powerpc-macosx 
     1. .dmg package
  31. Test release for powerpc-macos
  32. ~~Test release for powerpc-morphos~~ (done)
  33. Test release for i386-netware
  34. Test release for i386-netwlibc
  35. Test release for sparc-linux 
     1. tar
  36. Test release for arm-linux 
     1. tar
  37. Test release for x86_64-linux
  38. Check PDF documentation (open all files)
  39. Check HTML documentation
  40. Check TXT documentation
  41. Read updated text files as distributed in release zip files 
     1. readme.txt
     2. faq.txt
     3. whatsnew.txt



### Going public

  1. ~~Make new version numbers (release plus next odd patch number for continuing fixes) available in bug tracker~~ (done)
  2. ~~Make new version numbers (release plus next odd patch number for continuing fixes) available in testsuite db - Florian~~ (done)
  3. Make new files on FTP available to wide public 
     1. _update symlinks_ (no symlinks any more?)
     2. move the old version to the olddist/<version>
     3. ~~upload files to SourceForge.net and add them to new "releases" for individual platforms~~ (done)
     4. ~~make new "releases" on SourceForge.net accessible for users (change status to "active")~~ (done)
     5. ~~Allow automated notifications on individual SourceForge.net file release pages to be sent~~ (done)
  4. ~~Submit darwin packages to fink~~ (done) - Mischi
  5. ~~Update WWW pages~~ (done) 
     1. ~~/html/news.fp~~ (done)
     2. ~~/html/down/* (links to all individual files & file sizes)~~ (done)
     3. ~~/html/download.fp (version number and list of platforms)~~ (done)
     4. ~~/html/fpc.fp~~ (done)
     5. ~~/html/faq.fp (things like "the latest version is ...")~~ (done)
  6. _Create new fixes branch (only after a major release - ?.?.0)_ (not relevant this time) 
     1. _Update version number in the trunk branch (only after a major release - ?.?.0)_ (not relevant this time) 
        1. _/compiler/version.pas_ (not relevant this time)
        2. _All Makefile.fpc files containing "version=..." (plus regenerate the corresponding Makefiles)_ (not relevant this time)
  7. ~~Update version number in the fixes branch (increase the patch to next odd number) - Tomas~~ (done) 
     1. ~~/compiler/version.pas~~ (done)
     2. ~~All Makefile.fpc files containing "version=..." (plus regenerate the corresponding Makefiles)~~ (done)
  8. ~~Check the WWW pages - Tomas~~ (done) 
     1. ~~make sure<http://www.freepascal.org> contains the new version already~~ (done)
     2. ~~read news.html - Tomas~~ (done)
     3. ~~read fpc.html - Tomas~~ (done)
     4. ~~read download.html and check links to individual files - Tomas~~ (done)
  9. ~~Send announcement to our mailing lists - Tomas~~ (done)
  10. ~~Post announcement on the community site - Tomas~~ (done)
  11. ~~Post announcement on Sourceforge.net (only "Project Administrators" may do it) - Jonas~~ (done)
  12. Make sure that all unfixed issues encountered during RC testing and listed on dedicated page in Wiki are documented in bug tracker too
  13. Revise / update /html/future.fp after major versions (?.?.0)

---

_Source: [https://wiki.freepascal.org/Release_2.0.4](https://web.archive.org/web/20250124215916/https://wiki.freepascal.org/Release_2.0.4)_
