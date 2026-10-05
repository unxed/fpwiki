# Detailed 2.4.0 Todo

This todo is for people working on fpc to write down what they want to do till 2.4.0 is released. It's not a wishlist what shall be done. Feature requests etc. should be added to mantis. 

## Contents

  * 1 General
  * 2 Compiler
  * 3 Packaging
  * 4 RTL
  * 5 FCL
  * 6 IDE
  * 7 Documentation
  * 8 Packages



### General

  * Document changes potentially affecting currently working code to [User Changes 2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") (all) 
    * Document regcal stack parameter order change
    * Document val change regarding the handling of #0
  * Use fppkg for package repository (Micha)
  * Fix bugs with [target 2.4.0](<http://www.freepascal.org/mantis/view_all_set.php?type=3&source_query_id=623>)



### Compiler

  * ~~packages (Florian/Peter)~~
  * ~~[Module handling rewrite](<Module_handling_rewrite.md> "Module handling rewrite") (Peter/Florian)~~
  * ~~Crosscompiling to Mac OS X~~ (Marco, been done, but not end user ready)
  * dispinterfaces (Florian)
  * improve HTML rendering of IDE (Florian/Tomas(?))
  * ~~Internal linker for ELF platforms?~~
  * generics support (Florian)
  * Fix all testsuite failures which are in 2.1.x but not in 2.0.x (all)
  * bug fixing (all)
  * Ansistrings/widestrings should show in gdb as string, not address. We can achieve this by emitting debug information for zero terminated strings.
  * Fix -pg gprof support.



### Packaging

  * Use fpmake for building (Michael/Peter)
  * get an initial set of packages for often used pkgs. (during RC'ing?)



### RTL

  * 64-bit file support for Solaris (?)
  * 64-bit file support for MacOS (?)
  * 64-bit file support for Netwlibc (?)
  * ~~Fix get_caller_frame stuff for code without stackframes~~ (Jonas)
  * ARM badly needs mod/divide helpers in assembler
  * basic template unit (map and list) (Neli)
  * Make decision about threading in Linux (NTPL, unit libc) (bug 6280) (???, Marco is willing to help with the implementation)
  * fix internal ThreadManager inconsistencies (Ales?, March 31? - probably not)



### FCL

  * DB: Restructure the db-testsuite (Joost)
  * DB: bufdataset: implement calculated fields (Joost)
  * DB: bufdataset: implement in-memory-filtering (Joost)
  * DB: bufdataset: make it possible to use bufdataset directly, thus not only it's descendents. (like TClientDataset) (Joost)
  * DB: bufdataset: save and read datapackets as XML (Joost)
  * DB: sqldb: improve speed by fetching more then one record at a time (Joost)
  * DB: sqldb: add the ability to make plugins (Joost)
  * DB: sqldb: a backup and restore plugin for firebird (Joost)
  * DB: sqldb: revise the TSQLScript component (2.4.0 ? Joost)
  * If possible: set up a test-suite for the fcl, like the db-testsuite. In such a way thet it can be used automatically, just like the compiler test-suite (Joost)



### IDE

  * Check and modify compile options window to reflect all new 2.2 compiler options.



### Documentation

  * Update runtime error list.
  * CHM distribution. (Switch textmode IDE docs to CHM?)



### Packages

  * Packages that use widestring, but are not com related should change to tunicodestring (or maybe RTLSTRING), e.g. xmlcfg*.pp

---

_Source: [https://wiki.freepascal.org/Detailed_2.4.0_Todo](https://web.archive.org/web/20220704025357/https://wiki.freepascal.org/Detailed_2.4.0_Todo)_
