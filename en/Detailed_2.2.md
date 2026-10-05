# Detailed 2.2.0 Todo

This todo is for people working on fpc to write down what they want to do till 2.2.0 is released. It's not a wishlist what shall be done. Feature requests etc. should be added to mantis. 

## Contents

  * 1 General
  * 2 Compiler
  * 3 Packaging
  * 4 RTL
  * 5 FCL
  * 6 IDE
  * 7 Documentation



### General

  * Document changes potentially affecting currently working code to [User_Changes_2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") (all) 
    * Document regcal stack parameter order change
    * Document val change regarding the handling of #0
  * Fix bugs with [target 2.2.0](<http://www.freepascal.org/mantis/view_all_set.php?type=3&source_query_id=623>)



### Compiler

  * ~~packages (Florian/Peter)~~ (2.4.0)
  * ~~[Module handling rewrite](<Module_handling_rewrite.md> "Module handling rewrite") (Peter/Florian)~~ (2.4.0)
  * ~~Crosscompiling to Mac OS X~~ (Marco, been done, but not end user ready ->2.4.0)
  * ~~rewrite assigning of automated types like variants and widestrings (Florian)~~
  * ~~complete unique type support (Florian)~~
  * dispinterfaces (Florian)
  * improve HTML rendering of IDE (Florian/Tomas(?))
  * ~~Internal linker for ELF platforms?~~ (2.4.0)
  * ~~better string append support (Peter)~~
  * ~~Use Sysutils (Peter)~~
  * ~~Directory caching (Peter)~~
  * ~~All path/filename handling in the compiler ansistring. (Thomas)~~
  * generics support (Florian)
  * ~~PIC support for Darwin/ppc and Darwin/i386 and compile rtl, packages and fcl with PIC support because non-PIC dynamic libraries are unsupported (although they work in simple situations) (Jonas)~~
  * Fix all testsuite failures which are in 2.1.x but not in 2.0.x (all)
  * bug fixing (all) 
    * ~~Delphi incompatibility regarding register calling convention (push order of stack based arguments) (Florian)~~
    * ~~fix bugs in *nix tthread.inc's (joining detached threads, possibly some more dataraces) (Jonas)~~
  * ~~Binary writer generates incorrect debug info, for example enums do not show. (Peter)~~
  * Ansistrings/widestrings should show in gdb as string, not address. We can achieve this by emitting debug information for zero terminated strings.
  * Fix -pg gprof support.



### Packaging

  * Use fpmake for building (Michael/Peter)
  * Use fppkg for package repository (Michael/Peter)
  * get an initial set of packages for often used pkgs. (during RC'ing?)



### RTL

  * 64-bit file support for Solaris (?)
  * 64-bit file support for MacOS (?)
  * 64-bit file support for Netwlibc (?)
  * ~~64-bit file support for Linux (Neli)~~
  * ~~64-bit file support for Linux/FPC_USE_LIBC~~ (Neli, bug 7987)
  * ~~Fix get_caller_frame stuff for code without stackframes~~ (Jonas)
  * ARM badly needs mod/divide helpers in assembler
  * ~~add {$optimization nostackframe} to objects unit and to all code which uses it (IDE, fvision)~~ (superseded by r5146)
  * basic template unit (map and list) (Neli)
  * ~~TFormatSettings (bug 6242).~~ (Marco/Michael)
  * Make decision about threading in Linux (NTPL, unit libc) (bug 6280) (???, Marco is willing to help with the implementation)
  * ~~bug 7247 (TStringList incompat, Marco/Michael. Scheduled fro XMas week)~~
  * ~~bug 6824 (stored=false property storage, Lazarus angles?)~~
  * ~~bug 7947 (Syncobjs cleanup)~~
  * ~~properly scan and clean up sockets on all platforms~~ (Ales)
  * fix internal ThreadManager inconsistencies (Ales?, March 31? - probably not)
  * ~~Make declarations in Windows unit for WinCE compatible with those in the windows unit for Win32. (Yury)~~



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

  * ~~Try to implement a TP like evaluate/modify window~~ (Daniel)
  * Check and modify compile options window to reflect all new 2.2 compiler options.



### Documentation

  * Update runtime error list.

---

_Source: [https://wiki.freepascal.org/Detailed_2.2.0_Todo](https://web.archive.org/web/20250113193015/https://wiki.freepascal.org/Detailed_2.2.0_Todo)_
