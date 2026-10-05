# Release 2.2.0

## Contents

  * 1 Release organization
  * 2 Release procedure (Release Template)
    * 2.1 Release preparations
    * 2.2 2.1.2
    * 2.3 2.1.4 (2.2.0 beta)
    * 2.4 During the beta
    * 2.5 2.2.0
    * 2.6 Going public of the beta



## Release organization

  1. Before releasing 2.2.0 there will be a public beta available for about two months. The version number of the beta will be 2.1.4 (aka 2.2.0-beta)
  2. Before the beta there will be an 'internal' release, 2.1.2 to test the build-proces on all platforms. The release of 2.1.2 is the 'feature freeze' for version 2.2.0. In principle between version 2.1.2 and 2.1.4 only build-related patches are allowed.
  3. Version 2.1.4 will be public and announced. From the release of 2.1.4 on, no new features will be added and no functionality will be removed. Only bug fixes are allowed.
  4. About two months after version 2.1.4, there will be (at least one) release candidate for version 2.2.0.
  5. Is the release canidates are satisfactory, 2.2.0 will be released



Merging patches should be done by the developers who are 'responsible' for that part of the code. 

  
Timeline so far: 

  1. Friday the 16th version 2.1.2 will be tagged from the fixes_2_2 branch.
  2. If things go well, 2.1.4 will be tagged friday the 30th. If we are lucky, the release of 2.1.4 can be in the week thereafter.



## Release procedure ([Release Template](<Release_Template.md> "Release Template"))

Note that detailed information for some tasks can be found in [Release engineering](<Release_engineering.md> "Release engineering"). These two pages should be eventually merged (or probably rather better interlinked) in the future. 

### Release preparations

These things has to be done only once for each release-cycle. 

  1. ~~Agree on deadline for changes~~
  2. ~~New page in Wiki for release procedure with steps needed and their status (this page), at the beginning consisting of the beta (2.1.) release.~~ Later the final release sections has to be added.
  3. Check tools 
     1. Check version of the above mentioned tools (GNU tools, helper DLLs, UPX, etc.), and decide whether it isn't time to update some of these tools
     2. Repackage and upload additional tools where needed
  4. Update whatsnew.txt (/install/doc/whatsnew.txt)
  5. Update readme.txt (/install/doc/readme.txt)
  6. Update /html/faq.fp and convert it to /install/doc/faq.htm and /install/doc/faq.txt



### 2.1.2

  1. Create new page in Wiki with issue log for documentation of issues encountered in release candidates and their status
  2. Update whatsnew.txt (/install/doc/whatsnew.txt)
  3. Update readme.txt (/install/doc/readme.txt)
  4. Update /html/faq.fp and convert it to /install/doc/faq.htm and /install/doc/faq.txt
  5. ~~New[directories](<Release_engineering.md> "Release engineering")~~
     1. ~~Create new directories on ftp~~
     2. ~~Copy the extra files (asld*.zip, gdb*.zip, make*.zip) from previous release (unless updated with new versions)~~
  6. ~~Finish all source file updates~~
  7. Convert /html/faq.fp to /install/doc/faq.htm and /install/doc/faq.txt
  8. ~~[Update version number](<Release_engineering.md> "Release engineering") to new release number~~
     1. fpcbuild: All Makefile.fpc files containing version=... (plus regenerate all corresponding Makefiles)
     2. fpc: All Makefile.fpc files containing version=... (plus regenerate all corresponding Makefiles)
     3. ~~/install/doc/readme.txt~~
     4. ~~/installer/install.dat (header)~~
     5. ~~/installer/install.pas (installer version)~~
     6. ~~/install/fpc.spec~~
     7. /install/debian/changelog
     8. /docs/fpc.sty (macro fpcversion)
     9. ~~/compiler/version.pas~~
  9. ~~[Tag version](<Release_engineering.md> "Release engineering") 2.1.2~~
  10. ~~Create and upload exported[ fpcbuild.zip ](<Release_engineering.md> "Release engineering")~~
  11. ~~Create and upload the documentation - Michael~~
     1. ~~doc-pdf.zip~~
     2. ~~doc-html.zip~~
     3. ~~doc-htm.zip~~
     4. ~~doc-txt.zip~~
     5. ~~doc-ps.zip~~
     6. ~~doc-pdf.tar.gz~~
     7. ~~doc-html.tar.gz~~
     8. ~~doc-ps.tar.gz~~
  12. Create and upload source zips 
     1. short name version for binary packages
     2. short name version for docs source
     3. long name version for binary packages
     4. long name version for docs source
  13. Create and upload source tars
  14. [ Build](<Release_engineering.md> "Release engineering") and upload the following releases: 
     1. native i386-go32v2 (basic zip, full zip)
     2. ~~native i386-win32 (installer) (Florian)~~
     3. ~~native i386-linux (tar, deb:mazen, rpm)~~
     4. native i386-freebsd (tgz)
     5. native i386-netware (zip)
     6. native i386-netware libc (zip)
     7. ~~native i386-os2 (basic zip, full zip)~~ (import libraries still broken, release not possible now)
     8. native i386-mac os x + cross powerpc-mac os x (dmg, ~~.info for fink~~) (jonas~~/mischi~~)
     9. native x86_64-linux (tar, deb, rpm)
     10. ~~cross i386-win32 - > arm-wince (Florian)~~
     11. ~~cross i386-win32 - > x86_64-win64 (Florian)~~
     12. ~~native arm-linux (tar) (Florian)~~
     13. native powerpc-mac os x (dmg, ~~.info for fink~~) (jonas~~/mischi~~)
     14. ~~native powerpc-linux (tar) (tom_at_work)~~
     15. ~~native powerpc64-linux (tar) (tom_at_work)~~
     16. native powerpc-morphos (?)
     17. ~~native sparc-linux (deb:mazen)~~
  15. Run makereleasezips (combine small ZIP files together)
  16. Consider announcing availability of the new RC in fpc-devel list and/or ask there for dedicated testers for individual platforms
  17. [ Test](<Release_engineering.md> "Release engineering") the following releases: 
     1. native i386-go32v2
     2. native i386-win32
     3. native i386-linux
     4. native i386-freebsd
     5. native i386-netware
     6. native i386-netware libc
     7. native i386-os2
     8. native i386-mac os x
     9. native x86_64-linux
     10. cross i386-win32 -> arm-wince
     11. cross i386-win32 -> x86_64-win64
     12. native arm-linux
     13. native powerpc-mac os x
     14. native powerpc-morphos (?)
     15. native powerpc-linux
     16. native powerpc64-linux
     17. native sparc-linux (mazen)



### 2.1.4 (2.2.0 beta)

  1. Section for new beta on release pages in Wiki
  2. ~~Look at unmerged changes in fpc and fpcbuild since the last RC and consider/ask for necessity of their inclusion in the release~~
  3. ~~Create new page in Wiki with issue log for documentation of[issues](<Issues_2.1.md> "Issues 2.1.4") encountered in release candidates and their status~~
  4. ~~Update whatsnew.txt (/install/doc/whatsnew.txt)~~
  5. ~~Update readme.txt (/install/doc/readme.txt)~~
  6. ~~New[directories](<Release_engineering.md> "Release engineering")~~
     1. ~~Create new directories on ftp~~
     2. ~~Copy the extra files (asld*.zip, gdb*.zip, make*.zip) from previous release (unless updated with new versions)_only done for go32v2 since there is no OS/2 release_~~
  7. ~~Finish all source file updates~~
  8. Convert /html/faq.fp to /install/doc/faq.htm and /install/doc/faq.txt
  9. ~~[Update version number](<Release_engineering.md> "Release engineering") to new release number~~
     1. ~~fpcbuild: All Makefile.fpc files containing version=... (plus regenerate all corresponding Makefiles)~~
     2. ~~fpc: All Makefile.fpc files containing version=... (plus regenerate all corresponding Makefiles)~~
     3. ~~/install/doc/readme.txt~~
     4. ~~/installer/install.dat (header)~~
     5. ~~/installer/install.pas (installer version)~~
     6. ~~/install/fpc.spec _not needed anymore, done by make rpm_~~
     7. ~~/install/debian/changelog~~
     8. ~~/docs/fpc.sty (macro fpcversion)_The documentation version number is already set to 2.2.0_~~
     9. ~~/compiler/version.pas~~
  10. ~~[Tag version](<Release_engineering.md> "Release engineering") 2.1.4~~
  11. ~~Create and upload exported[ fpcbuild.zip ](<Release_engineering.md> "Release engineering")~~
  12. ~~Create and upload the documentation - Michael~~
     1. ~~doc-pdf.zip~~
     2. ~~doc-html.zip~~
     3. doc-htm.zip
     4. ~~doc-txt.zip~~
     5. ~~doc-ps.zip~~
     6. ~~doc-pdf.tar.gz~~
     7. ~~doc-html.tar.gz~~
     8. ~~doc-ps.tar.gz~~
     9. ~~doc-dvi.zip~~
  13. Create and upload source zips 
     1. short name version for binary packages
     2. short name version for docs source
     3. long name version for binary packages
     4. long name version for docs source
  14. Create and upload source tars
  15. [ Build](<Release_engineering.md> "Release engineering") and upload the following releases: 
     1. native i386-go32v2 (basic zip, full zip)
     2. ~~native i386-win32 (installer) (Florian)~~
     3. ~~native i386-linux (tar, deb:mazen, rpm)~~
     4. ~~native i386-freebsd (tgz)~~
     5. ~~native i386-netware (zip)_a netware version in not released_~~
     6. ~~native i386-netware libc (zip)_a netware version in not released_~~
     7. ~~native i386-os2 (basic zip, full zip) (import libraries still broken, release not possible now)~~
     8. ~~native i386-mac os x + cross powerpc-mac os x (dmg, .info for fink) (jonas/mischi)~~
     9. ~~native x86_64-linux (tar, deb, rpm~~)
     10. ~~cross i386-win32 - > arm-wince (Florian)~~
     11. ~~cross i386-win32 - > x86_64-win64 (Florian)~~
     12. ~~native arm-linux (tar) (Florian)~~
     13. ~~native powerpc-mac os x (dmg, .info for fink) (jonas/mischi)~~
     14. ~~native powerpc-linux (tar) (tom_at_work)~~
     15. ~~native powerpc64-linux (tar) (tom_at_work)~~
     16. ~~native powerpc-morphos (?)_a morphos version in not released_~~
     17. ~~native sparc-linux (deb:mazen)~~
  16. Run makereleasezips (combine small ZIP files together)
  17. Consider announcing availability of the new RC in fpc-devel list and/or ask there for dedicated testers for individual platforms
  18. [ Test](<Release_engineering.md> "Release engineering") the following releases: 
     1. native i386-go32v2
     2. native i386-win32
     3. native i386-linux
     4. native i386-freebsd
     5. native i386-netware
     6. native i386-netware libc
     7. native i386-os2
     8. native i386-mac os x
     9. native x86_64-linux
     10. cross i386-win32 -> arm-wince
     11. cross i386-win32 -> x86_64-win64
     12. native arm-linux
     13. native powerpc-mac os x
     14. native powerpc-morphos (?)
     15. native powerpc-linux
     16. native powerpc64-linux
     17. native sparc-linux (mazen)



### During the beta

  1. Check and update all .msg files 
     1. errore.msg - John?
     2. errorct.msg
     3. errord.msg, errordu.msg - Karl-Michael Schindler aka mischi
     4. errorda.msg - Christian Iversen?
     5. errores.msg
     6. errorf.msg - Remi Dorlet and "Oro06"/"Olivier"?
     7. errorfi.msg
     8. errorhe.msg, errorheu.msg - Ido Kanner?
     9. errorid.msg - Zaenal Mutaqin?
     10. errorn.msg - Matthijs Willemstein?
     11. errorpl.msg - Wojciech Malinowski?
     12. errorpli.msg - Wojciech Malinowski?
     13. errorptd.msg - Ari Ricardo Ody?
     14. errorptw.msg - Ari Ricardo Ody?
     15. errorr.msg - Michail Baikov?
     16. errorrw.msg - Michail Baikov?
     17. errorues.msg



### 2.2.0

  1. ~~Section for the final build on release page in Wiki~~
  2. ~~Look at unmerged changes in fpc and fpcbuild since the last RC and consider/ask for necessity of their inclusion in the release~~
  3. New [directories](<Release_engineering.md> "Release engineering")
     1. Create new directories on ftp
     2. Copy the extra files (asld*.zip, gdb*.zip, make*.zip) from previous release (unless updated with new versions) _only done for go32v2 since there is no OS/2 release_
  4. ~~Finish all source file updates~~
  5. ~~Convert /html/faq.adp to /install/doc/faq.htm and /install/doc/faq.txt~~
  6. ~~[Update version number](<Release_engineering.md> "Release engineering") to new release number~~
     1. ~~fpcbuild: All Makefile.fpc files containing version=... (plus regenerate all corresponding Makefiles)~~
     2. ~~fpc: All Makefile.fpc files containing version=... (plus regenerate all corresponding Makefiles)~~
     3. ~~/install/doc/readme.txt~~
     4. ~~/installer/install.dat (header)~~
     5. ~~/installer/install.pas (installer version)~~
     6. ~~/install/fpc.spec _not needed anymore, done by make rpm_~~
     7. ~~/install/debian/changelog~~
     8. ~~/docs/fpc.sty (macro fpcversion)_The documentation version number is already set to 2.2.0_~~
     9. ~~/compiler/version.pas~~
  7. ~~[Tag version](<Release_engineering.md> "Release engineering") 2.2.0~~
  8. ~~Create and upload exported[ fpcbuild.zip ](<Release_engineering.md> "Release engineering")~~
  9. Create and upload the documentation - Michael 
     1. ~~doc-pdf.zip~~
     2. ~~doc-html.zip~~
     3. doc-htm.zip
     4. ~~doc-txt.zip~~
     5. ~~doc-ps.zip~~
     6. ~~doc-pdf.tar.gz~~
     7. ~~doc-html.tar.gz~~
     8. ~~doc-ps.tar.gz~~
     9. ~~doc-dvi.zip~~
  10. ~~Create and upload source zips~~
     1. short name version for binary packages
     2. short name version for docs source
     3. long name version for binary packages
     4. long name version for docs source
  11. ~~Create and upload source tars~~
  12. [ Build](<Release_engineering.md> "Release engineering") and upload the following releases: 
     1. native i386-go32v2 (basic zip, full zip)
     2. ~~native i386-win32 (installer)~~
     3. ~~native i386-linux (tar, deb:mazen, rpm)~~
     4. ~~native i386-freebsd (tar in Marco's homedir on Idefix)~~
     5. ~~cross i386-netware (zip)_a netware version in not released_~~
     6. ~~cross i386-netware libc (zip)_a netwlibc version in not released_~~
     7. ~~native i386-os2 (basic zip, full zip)_(import libraries still broken, release not possible now)_~~
     8. ~~native i386-mac os x + cross powerpc-mac os x (dmg, .info for fink)~~
     9. ~~native x86_64-linux (tar,~~ deb~~, rpm)~~
     10. cross i386-win32 -> arm-wince
     11. cross i386-win32 -> x86_64-win64
     12. native arm-linux (tar)
     13. native powerpc-mac os x (~~dmg~~ , .info for fink)
     14. ~~native powerpc-linux (tar)~~
     15. ~~native powerpc64-linux (tar)~~
     16. ~~native powerpc-morphos (?)_a morphos version in not released_~~
     17. ~~native sparc-linux (deb:mazen)~~
  13. Run makereleasezips (combine small ZIP files together)
  14. Consider announcing availability of the new RC in fpc-devel list and/or ask there for dedicated testers for individual platforms
  15. [ Test](<Release_engineering.md> "Release engineering") the following releases: 
     1. native i386-go32v2
     2. native i386-win32
     3. native i386-linux
     4. native i386-freebsd
     5. native i386-netware
     6. native i386-netware libc
     7. native i386-os2
     8. native i386-mac os x
     9. native x86_64-linux
     10. cross i386-win32 -> arm-wince
     11. cross i386-win32 -> x86_64-win64
     12. native arm-linux
     13. ~~native powerpc-mac os x~~
     14. native powerpc-morphos (?)
     15. native powerpc-linux
     16. native powerpc64-linux
     17. native sparc-linux



### Going public of the beta

  1. Make new version numbers (release plus next odd patch number for continuing fixes) available in bug tracker
  2. Make new version numbers (release plus next odd patch number for continuing fixes) available in testsuite db
  3. Make new files on FTP available to wide public 
     1. _update symlinks_ (no symlinks any more?)
     2. move the old version to the olddist/<version>
     3. upload files to SourceForge.net and add them to new "releases" for individual platforms
     4. make new "releases" on SourceForge.net accessible for users (change status to "active")
     5. Allow automated notifications on individual SourceForge.net file release pages to be sent
  4. Submit darwin packages to fink
  5. Update WWW pages 
     1. /html/news.fp
     2. /html/down/* (links to all individual files & file sizes)
     3. /html/download.fp (version number and list of platforms)
     4. /html/fpc.fp
     5. /html/faq.fp (things like "the latest version is ...")
  6. ~~Create new fixes branch (only after a major release - ?.?.0)~~ (already done at the beginning of the release cycle) 
     1. ~~Update version number in the trunk branch (only after a major release - ?.?.0)~~ (already done at the beginning of the release cycle) 
        1. ~~/compiler/version.pas~~ (already done at the beginning of the release cycle)
        2. ~~All Makefile.fpc files containing "version=..." (plus regenerate the corresponding Makefiles)~~ (already done at the beginning of the release cycle)
  7. Update version number in the fixes branch (increase the patch to next odd number) 
     1. /compiler/version.pas
     2. All Makefile.fpc files containing "version=..." (plus regenerate the corresponding Makefiles)
  8. Check the WWW pages 
     1. make sure <http://www.freepascal.org> contains the new version already
     2. read news.html
     3. read fpc.html
     4. read download.html and check links to individual files
  9. Send announcement to our mailing lists
  10. Post announcement on the community site
  11. Post announcement on Sourceforge.net (only "Project Administrators" may do it)
  12. Make sure that all unfixed issues encountered during RC testing and listed on dedicated page in Wiki are documented in bug tracker too
  13. Revise / update /html/future.fp after major versions (?.?.0)

---

_Source: [https://wiki.freepascal.org/Release_2.2.0](https://web.archive.org/web/20250124213410/https://wiki.freepascal.org/Release_2.2.0)_
