# major version to do

This is a page for reminders with regard to release engineering and the next "major" FPC version. Mostly it is a list of reminders which deprecated functionality can be removed. 

Usually these are reminders to review deprecated functionality, or changes that need to be done relatively late minute to ease the pain of dual-maintenance. 

# 3.0.x (working title for 1st major release after 2.6 series)

  * ~~Remove deprecated functions in unit "unix" - Most of these have been deprecated since before 2007.~~
  * ~~remove deprecated functions from unixutils. (deprecated during the 2.4.0 release preparation),/strike >~~
~~
  * ~~sockets unit has old non fp functions under ifdef. Can be removed at any time, inactive~~
~~
~~

  * Linux stat record cleanup? Has been deprecated for at least the whole 2.6 cycle.
  * ~~sockets inaddr unit has non standard fieldnames from 1.0.x time in union (roughly same issue as linux stat. Has been deprecated for a while too.~~
  * remove deprecated records and functions (shlobj) from unit Windows.
  * same for messages.

~~~~

# 3.2.0 (2nd major release after 2.6 series)

~~~~

  * ~~consider inverting the $R statement to become delphi compatible<http://bugs.freepascal.org/view.php?id=20652>~~ not necessary anymore considering Sven's comment.
  * review all deprecations in unix/ dir again. Most are relatively unused and harmless. 
    * Review deprecations in unit unix (some minor linux specific things and leftovers deprecated during 2.6 cycle)
    * remove deprecated functions from unixutils. (deprecated during the 2.6.0 release preparation)
  * do we still need fs32bit support for Linux (i386/ and unix/ directories)?
  * consider removing makefile date line. Only causes conflicts.
  * clean out old VER_ defines. Keep in mind that ver3_0 is still needed for bootstrapping
  * review various "old" files in regexpr directory

~~~~~~

= 3.4.0

~~

  * review various "old" files in regexpr directory

~~

~~~~

---

_Source: [https://wiki.freepascal.org/major_version_to_do](https://web.archive.org/web/20200809045156/https://wiki.freepascal.org/major_version_to_do)_
