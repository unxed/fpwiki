# Lazarus 0.9.30.2 release plan

## Contents

  * 1 Release preparation
    * 1.1 Bugs to be fixed
  * 2 Ask for testing
  * 3 Merge revisions from trunk
    * 3.1 Submitted by developer / committer
    * 3.2 Postponed merge requests
    * 3.3 User requested merges
  * 4 Tagging release
  * 5 Building release
  * 6 Announcements
  * 7 After release
  * 8 Further



### Release preparation

  * ~~Gather list of todos from developers~~
~~~~
  * ~~Add new LazTarget to the mantis for so we can postpone issue one release.~~



#### Bugs to be fixed

  * ~~Go over the list of new issues and determine if there are regressions among them~~

~~~~

Things that need to be fixed before tagging:

~~~~

~~~~
  * ~~A list of bugs with[target 0.9.30.2](<http://bugs.freepascal.org/view_all_set.php?type=3&source_query_id=????>).~~



### Ask for testing

  * ~~Informally announce (IRC, mailing list) a pending release (+/- week before actual release), so that people can test for regressions. (Vincent)~~



### Merge revisions from trunk

#### Submitted by developer / committer

The following revisions contain bug fixes and need to be merged from trunk to the fixes_0_9_30 branch. 

  * ~~31291~~



#### Postponed merge requests

#### User requested merges

### Tagging release

  * ~~Set version to 0.9.30.2 in fixes_0_9_30 branch (Vincent)~~
    * ~~lazarus/ide/version.inc~~
~~
    * lazarus/lcl/lclversion.pas
    * lazarus/debian/changelog
    * lazarus/lazarus.app/Contents/Info.plist
~~
    * ~~open lazarus/lazarus.lpi in the IDE and change the version numbers in the project options dialog~~
  * ~~Tag fixes_0_9_30 branch to tags/release_0_9_30 (Vincent)~~
~~~~
  * ~~Set version to 0.9.30.3 in fixes_0_9_30 branch (Vincent)~~



### Building release

  * ~~source (Vincent)~~
~~
  * html docs (Vincent)
  * chm docs (Vincent)
  * win32 (Vincent)
  * win32 for arm-wince (Vincent)
  * win64 (Vincent)
  * linux source rpm (Vincent)
~~
  * ~~linux i386 rpm (Vincent)~~
    * crosswin32 rpm (Mattias)
  * ~~linux x86_64 rpm (Vincent)~~
~~~~
  * ~~linux i386 deb (Vincent)~~
    * crosswin32 deb (Mattias)
  * ~~linux x86_64 deb (Vincent)~~
~~
  * Mac OS X powerpc (Vincent)
  * Mac OS X i386 (Vincent)
~~
  * ~~Add debs to ubuntu repo (Vincent)~~



### Announcements

  * Wiki: downloading, installation, getting source hints (Mattias) 
    * [Installing Lazarus](<Installing_Lazarus.md> "Installing Lazarus")
  * List of changes: [Lazarus 0.9.30 release notes](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") (Mattias)
  * Mailing lists (Mattias)
  * News item on www.lazarus.freepascal.org (Vincent)
  * Sourceforge (Vincent)
  * Freshmeat (Vincent)
  * Change IRC topic (Marc)
  * New versions in Mantis (Vincent)



### After release

  * Make sure snapshots are created correctly for the new version (Vincent)



### Further

  * Relax (all)
  * Plan next release

---

_Source: [https://wiki.freepascal.org/Lazarus_0.9.30.2_release_plan](https://web.archive.org/web/20250124213336/https://wiki.freepascal.org/Lazarus_0.9.30.2_release_plan)_
