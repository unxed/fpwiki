# Detailed Lazarus release template todo

_This page can be used as template for a Detailed Lazarus release 1.x todo_

## Contents

  * 1 Release preparation
  * 2 Bugs to be fixed
  * 3 Tagging release
  * 4 Building release
  * 5 Announcements
  * 6 After release
  * 7 Further
  * 8 See also



### Release preparation

  * Gather list of todos from developers
  * Add new LazTarget to the mantis for so we can postpone issue one release.



### Bugs to be fixed

Things that need to be fixed before tagging: 

  * 


### Tagging release

  * Set version to 1.0 in fixes_1_0 branch (Mattias) 
    * open lazarus/lazarus.lpi in the IDE and change the version numbers in the project options dialog 1.0.1.1 for RC1, 1.0.1.2 for RC2, 1.0.1.3 for final
    * lazarus/ide/version.inc 1.0 RC2, 1.0
    * lazarus/lcl/lclversion.pas 1.0.1.1 for RC1, 1.0.1.2 for RC2, 1.0.1.3 for final
    * lclbase.lpk
    * lcl.lpk
    * lazarus/debian/changelog 1.0.1-1 for RC1, 1.0.1-2 for RC2, 1.0.1-3 for final
    * lazarus/lazarus.app/Contents/Info.plist v1.0 RC2
    * tools/install/linux/environmentoptions.xml same as version.inc
    * tools/install/win/environmentoptions.xml same as version.inc
    * tools/install/macos/environmentoptions.xml same as version.inc
    * check if lpl files needs updating by running ./tools/lplupdate -c (Mattias)
    * run ./tools/updatemakefiles (Mattias)
  * Tag fixes_1_0 branch to tags/release_1_0_RC2 (Mattias)


    
    
    svn copy svn+<ssh://svn.freepascal.org/FPC/svn/lazarus/branches/fixes_1_2> svn+<ssh://svn.freepascal.org/FPC/svn/lazarus/tags/lazarus_1_2_4> -m 'tagged 1.2.4 as tags/lazarus_1_2_4'
    

  * Set version to 1.1 in trunk (Vincent)



### Building release

  * win32 (Vincent)
  * source (Mattias)
  * linux source rpm (Mattias)
  * linux i386 rpm (Mattias)
  * linux i386 deb (Mattias)
  * linux x86_64 rpm (Vincent)
  * Mac OS X powerpc (Vincent)



### Announcements

  * List of changes (Mattias)
  * Mailing lists (Mattias)
  * News item on www.lazarus.freepascal.org (Vincent)
  * Twitter (who?)
  * Sourceforge (Mattias)
  * Freshmeat (Vincent)
  * Wiki: downloading, installation, getting source hints (Mattias)
  * Change IRC topic (Marc)
  * New versions in Mantis (Marc)



### After release

  * Make sure snapshots are created correctly for the new version (Vincent)



### Further

  * Relax (all)
  * Plan next release



### See also

  * [comparable FPC document](<Release_Template.md> "Release Template")

---

_Source: [https://wiki.freepascal.org/Detailed_Lazarus_release_template_todo](https://web.archive.org/web/20250219055419/https://wiki.freepascal.org/Detailed_Lazarus_release_template_todo)_
