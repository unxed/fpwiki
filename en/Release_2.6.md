# Release 2.6.2

## Contents

  * 1 Release Issues
  * 2 Release preparations
  * 3 RC1
  * 4 RC2
  * 5 Final release
  * 6 Going public



### Release Issues

The issues page is [Issues 2.6.2](<Issues_2.6.md> "Issues 2.6.2") (from [Issues Template](<Issues_Template.md> "Issues Template") ) 

### Release preparations

  1. For major versions check [major versions to do](<Release_engineering.md> "Release engineering")
  2. Send a mail to fpc-devel to announce that the fixes branch is frozen for the benefit of committers that are not on core (or do not read it regularly)
  3. Agree on deadline for changes and date for release candidate building 
     1. Contact platform maintainers who are not subscribed to the core list and notify them about upcoming release and ask them for their inputs into the release schedule 
        1. Olivier Coursiere - Haiku
        2. Karl-Michael Schindler - Mac OS X - fink
     2. #FPC-2.6.2rc1 deadline:
     3. #FPC-2.6.2 deadline:
  4. Check the [Detailed 2.6.2 Todo](</index.php?title=Detailed_2.6.2_Todo&action=edit&redlink=1> "Detailed 2.6.2 Todo \(page does not exist\)") list for the particular release (should be linked from [To Do lists](<To_Do_lists.md> "To Do lists")) for status of individual todo items
  5. Check status of [bugs assigned to that particular release](<http://bugs.freepascal.org/roadmap_page.php>) in the bugtracker
  6. New page in Wiki named "Release_2.6.2" for release procedure with steps needed and their status (based on [Release Template](<Release_Template.md> "Release Template")), at the beginning consisting of (at least) RC1 and final release sections. If it is a final release add a link it to [Releasing](<Releasing.md> "Releasing")
  7. Create new page in Wiki with issue log for documentation of issues encountered in release candidates and their status ("Issues_2.6.2" based on [Issues Template](<Issues_Template.md> "Issues Template"))
  8. Move merged changes from [User Changes Trunk](<User_Changes_Trunk.md> "User Changes Trunk") to the appropriate [User Changes 2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") page, possibly adding changes that were merged but not added to the trunk page
  9. Update the version number/svn revision of all bug reports in mantis for which the patches were merged but that have not yet been updated (for <http://bugs.freepascal.org/changelog_page.php> )
  10. Ask platform maintainers and [external maintainers](</index.php?title=External_maintainers&action=edit&redlink=1> "External maintainers \(page does not exist\)") about including their platforms/builds in the new release
  11. Check and update all .msg files 
     1. errore.msg
     2. errord.msg
     3. errorf.msg
     4. errorn.msg
     5. errorr.msg
     6. errorrw.msg
     7. errorues.msg
     8. errores.msg
     9. errorct.msg
     10. errorhe.msg
     11. errorheu.msg
     12. errorptd.msg
     13. errorptw.msg
     14. errorpli.msg
     15. errorpl.msg
     16. errorid.msg
  12. Check tools 
     1. Check version of the above mentioned tools (GNU tools, helper DLLs, UPX, etc.), and decide whether it isn't time to update some of these tools. See [helper tools](<helper_tools.md> "helper tools")
     2. Repackage and upload additional tools where needed
  13. Update whatsnew.txt (/install/doc/whatsnew.txt)
  14. Find testers 
     1. Create a [testers](</index.php?title=Testers_x.x.x&action=edit&redlink=1> "Testers x.x.x \(page does not exist\)") page in Wiki (based on [Testers Template](<Testers_Template.md> "Testers Template"))
     2. Ask in fpc-devel list for volunteers interested in testing the individual platforms / builds and list them on the newly created page (they can add themselves)
  15. Update path and file names in /install/macosx/*.info for the new version
  16. Update path and file names in /install/fpc.ist for the new version
  17. Finish all source file updates
  18. convert /html/faq.fp to /install/doc/faq.htm and /install/doc/faq.txt ([more info](<Release_engineering.md> "Release engineering"))



### RC1

\-- Below is a template for every version 

  1. New directories ([more info](<Release_engineering.md> "Release engineering")) 
     1. Create new directories on ftp (/pub/fpc/beta/2.6.2-rcN or /pub/fpc/dist/2.6.2 and cpu-os under that)
     2. Copy the extra files (asld*.zip, gdb*.zip, make*.zip) from previous release (unless updated with new versions)
  2. Add new section for the upcoming build in /install/debian/changelog ([more info](<Release_engineering.md> "Release engineering"))
  3. Create new branch in SVN (release_2_6_2_rc1 or release_2_6_2) ([more info](<Release_engineering.md> "Release engineering"))
  4. Update version number in release branch (and, if necessary also in main branch (trunk or fixes)) ([more info](<Release_engineering.md> "Release engineering")) 
     1. /compiler/version.pas
     2. /install/doc/readme.txt
     3. /install/doc/whatsnew.txt
     4. /installer/install.dat (header)
     5. /installer/install.pas (installer version)
     6. /docs/fpc.sty (macro fpcversion)
     7. All Makefile.fpc files containing version=... (plus regenerate all corresponding Makefiles) ([more info](<Release_engineering.md> "Release engineering"))
     8. All version-numbers in fpmake.pp files ([more info](<Release_engineering.md> "Release engineering"))
  5. Create and upload exported fpcbuild ([more info](<Release_engineering.md> "Release engineering")) 
     1. fpcbuild-%{version}.zip
     2. fpcbuild-%{version}.tar.gz
     3. fpc-%{version}-source.zip
     4. fpc-%{version}-source.tar.gz
  6. Create and upload the documentation ([more info](<Release_engineering.md> "Release engineering")) 
     1. doc-pdf.zip
     2. doc-html.zip
     3. _doc-htm.zip_ (not included)
     4. doc-txt.zip
     5. doc-ps.zip
     6. doc-pdf.tar.gz
     7. doc-html.tar.gz
     8. doc-ps.tar.gz
  7. Create and upload source zips ([more info](<Release_engineering.md> "Release engineering")) 
     1. ~~short name version for binary packages~~
     2. ~~docs source (including link for short name version)~~
     3. long name version for binary packages
  8. Create and upload binary releases (place the name of the person who agreed to build a release behind the name of the target below) 
     * i386-go32v2/basic zip (just binaries)
     * i386-go32v2/full zip (including docs and sources)
     * ~~i386-os2/basic zip~~ (just binaries)
     * ~~i386-os2/full zip~~ (including docs and sources)
     * i386-win32/exe
     * x86_64-win64/exe
     * arm-wince/exe (cross-release)
     * arm-symbian (cross-release)
     * i386-symbian (cross-release)
     * arm-gba (cross-release)
     * arm-nds (cross-release)
     * powerpc-wii (cross-release)
     * i386-freebsd/tgz
     * i386-linux/tar ([more info](<Release_engineering.md> "Release engineering"))
     * i386-linux/deb ([more info](<Release_engineering.md> "Release engineering"))
     * i386-linux/rpm ([more info](<Release_engineering.md> "Release engineering"))
     * x86_64-linux/tar ([more info](<Release_engineering.md> "Release engineering"))
     * x86_64-linux/deb ([more info](<Release_engineering.md> "Release engineering"))
     * x86_64-linux/rpm ([more info](<Release_engineering.md> "Release engineering"))
     * powerpc-linux/tar ([more info](<Release_engineering.md> "Release engineering"))
     * powerpc64-linux/tar ([more info](<Release_engineering.md> "Release engineering"))
     * powerpc32-macosx/dmg
     * powerpc32->powerpc64-macosx/dmg
     * i386-macosx/dmg
     * i386->x86_64-macosx/dmg
     * i386->ARM-macosx/dmg
     * i386-macosx/.info for fink
     * powerpc-macos
     * sparc-linux/tar ([more info](<Release_engineering.md> "Release engineering"))
     * sparc-linux/deb ([more info](<Release_engineering.md> "Release engineering"))
     * arm-linux/tar (cross-release)
     * powerpc-morphos
     * powerpc-amiga
     * i386-netware
     * i386-netwlibc
     * i386-haiku
     * i386-sunos/tar
     * sparc-sunos/tar
  9. Get it tested 
     1. Ask dedicated testers for testing their platforms
     2. Consider announcing availability of the new RC in fpc-devel list
  10. Keep track of testing in [the wiki](</index.php?title=Testers_x.x.x&action=edit&redlink=1> "Testers x.x.x \(page does not exist\)")



\-- End of template for every version 

### RC2

  1. Section for new RC on release pages in Wiki 
     1. Release procedure
     2. Issue log
  2. Look at unmerged changes in fpc and fpcbuild since the last RC and consider/ask for necessity of their inclusion in the release



\-- Template from above 

### Final release

  1. Look at unmerged changes in fpc and fpcbuild since the last RC and consider/ask for necessity of their inclusion in the release (only cosmetic changes should be included, otherwise a new RC is needed instead of final release)
  2. Check all bugreports marked fixed with the previous fixes version, or trunk if they apply to the release. (iow if the release packages the fix). If they are fixed, set their "fixed in version" setting to the release version, and update the "fixed in revision" to the revision the fix was merged to release_ or fixes_ branch.



\-- Template from above 

### Going public

  1. Make new version numbers (release plus next odd patch number for continuing fixes) available in bug tracker (don't forget to set releases to "released", or they won't be visible to users.)
  2. Make new version numbers (release plus next odd patch number for continuing fixes) available in testsuite db
  3. Make new files on FTP available to wide public 
     1. _update symlinks_ (no symlinks any more?)
     2. Move the releases older than 18 months to the olddist/<version> dir.
     3. Publish files on SourceForge.net 
        1. Upload files to SourceForge.net and add them to new "releases" for individual platforms
        2. Mark the latest whatsnew.txt file as a "Release info" file (using properties of that file in File Manager pages)
        3. Select this release info file for the respective folders containing the new release
        4. Check that the right files are selected as default for the individual platforms recognized by SourceForge.net (use the latest readme.txt as the default download for "All other")
        5. Make new "releases" on SourceForge.net accessible for users (change status to "active")
        6. Allow automated notifications on individual SourceForge.net file release pages to be sent
  4. Submit darwin packages to fink
  5. Update WWW pages 
     1. /html/news.fp
     2. /html/down/* (links to all individual files & file sizes)
     3. /html/download.fp (version number and list of platforms)
     4. Search for the old version number (both with and without dots) in all files under /html/down/ (e.g. using grep) and fix where necessary
     5. /html/fpc.fp
     6. /html/faq.fp (things like "the latest version is ...")
  6. Create new fixes branch (only after a major release - ?.?.0) 
     1. Update version number in the trunk branch (only after a major release - ?.?.0) 
        1. /compiler/version.pas
        2. All Makefile.fpc files containing "version=..." (plus regenerate the corresponding Makefiles)
  7. Update version number in the fixes branch (increase the patch to next odd number) 
     1. /compiler/version.pas
     2. all Makefile.fpc files containing "version=..." (plus regenerate the corresponding Makefiles)
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
  14. Remove unneeded RC builds from FTP

---

_Source: [https://wiki.freepascal.org/Release_2.6.2](https://web.archive.org/web/20250124214226/https://wiki.freepascal.org/Release_2.6.2)_
