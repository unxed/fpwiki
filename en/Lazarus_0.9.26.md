# Lazarus 0.9.26.2 release plan

## Contents

  * 1 Release preparation
  * 2 Before tagging
    * 2.1 Bugs to be fixed
  * 3 Tagging release
  * 4 Building and uploading release
    * 4.1 Building
    * 4.2 Uploading
  * 5 Announcements
  * 6 After release
  * 7 Further



### Release preparation

  * ~~Gather list of todos from developers~~ (Vincent)
  * ~~Add LazTarget (0.9.28) to the mantis, so we can postpone issues one release.~~



### Before tagging

  * ~~Informally announce (IRC, mailing list) a pending release (+/- week before actual release), so that people can test for regressions~~ (Vincent) 
    * ~~Blog post about intented release~~ (Vincent)
    * ~~Email to lazarus list~~ Vincent



#### Bugs to be fixed

Things that need to be fixed before tagging: 

  * ~~A list of bugs with[target 0.9.26.2](<http://bugs.freepascal.org/view_all_set.php?type=3&source_query_id=1634>).~~



### Tagging release

  * ~~Set release version in fixes_0_9_26~~ (Vincent) 
    * ~~lazarus/ide/version.inc~~
    * ~~lazarus/lcl/lclversion.pas~~
    * ~~lazarus/debian/changelog~~
  * ~~Tag fixes_0_9_26 to tags/release_0_9_26_2~~ (Vincent)
  * ~~Tag binaries to tags/binaries_0_9_26_2~~ (Vincent)
  * ~~Increase version number in fixes_0_9_26 to 0_9_26_3~~(Vincent)



### Building and uploading release

#### Building

  * ~~source~~ (Vincent)
  * ~~win32~~ (Vincent)
  * ~~win32 based on qt~~ (Vincent)
  * ~~win32 for arm-wince~~ (Vincent)
  * ~~win32 for x64_86-win64~~ (Vincent)
  * ~~win64~~ (Vincent)
  * ~~linux source rpm~~ (Marc)
  * ~~linux i386 rpm~~ (Marc)
  * ~~linux x86_64 rpm~~ (Marc)
  * ~~linux i386 deb~~ (Vincent)
  * ~~linux x86_64 deb~~ (Vincent)
  * ~~Mac OS X powerpc~~ (Vincent)
  * ~~Mac OS X i386~~( Vincent)
  * ~~html docs~~ (Vincent)
  * ~~chm docs~~ (Vincent)



#### Uploading

  * ~~Ubuntu repo i386~~ (Vincent)
  * ~~Ubuntu repo x86_64~~ (Vincent)
  * ~~Set file releases from _Hidden_ to _Active_ on SourceForge and send email to people monitoring the package~~ (Vincent)



### Announcements

  * Wiki: downloading, installation, getting source hints (Mattias)
  * ~~List of changes:[Lazarus 0.9.26.2 release notes](<Lazarus_0.9.26.md> "Lazarus 0.9.26.2 release notes")~~ (Vincent)
  * ~~Mailing lists~~ (Mattias)
  * ~~News item on www.lazarus.freepascal.org~~ (Vincent)
  * ~~Sourceforge~~ (Vincent)
  * ~~Freshmeat~~ (Vincent)
  * ~~Change IRC topic~~ (Marc)
  * ~~New versions in Mantis~~ (Vincent)



### After release

  * ~~Make sure snapshots are created correctly for the new version~~ (Vincent)



### Further

  * Relax (all)

---

_Source: [https://wiki.freepascal.org/Lazarus_0.9.26.2_release_plan](https://web.archive.org/web/20250124215554/https://wiki.freepascal.org/Lazarus_0.9.26.2_release_plan)_
