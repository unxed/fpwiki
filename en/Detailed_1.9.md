# Detailed 1.9.8 Todo

* fix bugs (all) 
    * ~~AltGr in win32 ide doesn't work anymore after an external tool is called~~
    * test suite
    * web bugs
  * ~~fix lazarus sparc/linux (Florian/Peter)~~ _\- postponed to 1.9.10_
  * ~~fix lazarus x86_64/linux (Florian/Peter)~~ _\- postponed to 1.9.10_
  * fix ide powerpc/linux (Florian/Jonas/Marco)
  * ~~fix ide powerpc/macosx (Jonas/Olle)~~ Seems to work except for terminal glitches. 
    * already works, except for some graphic glitches on the user output screen, and no gdb support
  * ~~fix ide debugger sparc/linux (Peter/Florian)~~
  * ppc386 man page must become ppcppc for PowerPC, ppcsparc for Sparc etc 
    * ~~fpc man page is also more or less 386 specific, and does not mention Mac OS (X)~~
  * IDE help files (Michael)
  * ~~unique types (Florian)~~
  * ~~macpas macros (Olle)~~
  * ~~fix arm/linux (Florian)~~
  * ~~support gtk2 linking (Peter)~~
  * ~~fix[powerpc aix calling conventions](<http://developer.apple.com/documentation/DeveloperTools/Conceptual/MachORuntime/PowerPCConventions/chapter_3_section_5.html>) (Jonas/Peter)~~
    * ~~remember to test records of size 3 (may not load extra byte after it, could cause a crash)~~
  * ~~fix afterconstruction, webbug 3101 (Peter)~~
  * Go32v2 specific issues 
    * fix debugging in go32v2 ide
    * fix GO32v2 mouse cursor (FVision)
    * fix "empty" buttons (FVision - GO32v2 only?!) (Marco: I've seen them from time to time too in *nix or win32. I think it was a temporary bug)
    * fix DiskSize/DiskFree for large drives (> 2GB) for GO32v2 running under WinXX (Tomas)
    * fix Go32v2 SIGFPE problems
  * OS/2 target RTL todos 
    * 64-bit file functions under new OS/2 versions (Tomas/Yuri)
    * ~~EA functions - GetLongName/GetShortName, SetDefaultOS2FileType/SetDefaultOS2Creator (Tomas/Yuri)~~ _no time - postponed to 2.0 :-(_
    * ~~port access without EMX (Tomas/Yuri)~~ _no time - postponed to 2.0 :-(_
    * ~~exception handler (Tomas/Yuri)~~ _no time - postponed to 2.0 :-(_
    * ~~Unix compatible sockets unit (Tomas/Yuri)~~ _no time - postponed to 2.0 :-(_
  * ~~fix EMX target (Tomas)~~ _no time - postponed to 2.0 :-(_
  * Packaging 
    * ~~Win32 include ld.exe and windres.exe from 1.0.10~~
    * ~~fix make sourcezip FPC_VERSION (Peter)~~
    * ~~docs installation in separate dir under win32~~
    * win32 install over existing snapshot should work. (works if export first)
  * [test packages](</index.php?title=test_packages&action=edit&redlink=1> "test packages \(page does not exist\)")
  * fix language files 
    * German (Florian)
    * Dutch (Jonas/Michael/Peter/Marco)
    * French (Jonas/Michael)
    * Russia (?)
    * Polish (?)
    * Spanish (?)
  * Review docs according to mac specific things (Olle)

---

_Source: [https://wiki.freepascal.org/Detailed_1.9.8_Todo](https://web.archive.org/web/20250113201155/https://wiki.freepascal.org/Detailed_1.9.8_Todo)_
