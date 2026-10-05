# packages(disambiguation)

## Packages

There are various different meanings of the term "packages" in the combined FPC/Lazarus projects. 

In nearly all cases packages are collections of units and some build and descriptive metadata. In a few cases there is more. 

The basic types: 

  * [Lazarus Runtime Packages](<Lazarus_Packages.md> "Lazarus Packages") Adding a dependency on a **runtime package** , causes Lazarus to add the files to the search path.
  * [FPMake](<FPMake.md> "FPMake") and [fppkg](<fppkg.md> "fppkg") contain info about the newer Free Pascal packaging system based on **fpmake.pp**.
  * [Package List](<Package_List.md> "Package List") is a master index page that lists packages in the FPC packages/ directory. These are packages with fpmake buildsystem and metadata.
  * [Fpcmake](<Fpcmake.md> "Fpcmake") contains info about the older (but still used) **Makefile.fpc** Free Pascal packaging system.
  * [Lazarus IDE Packages](<Lazarus_IDE_Packages.md> "Lazarus IDE Packages") is bundled with IDE and can be installed via main menu: Package -> Install/Uninstall Packages..



Types where there is something more going on: 

  * [Lazarus Designtime Packages](<Lazarus_Packages.md> "Lazarus Packages") are packages that on register themselves in the IDE, e.g. **designtime** editor components for the object inspector. It can also contain runtime-only units.


  * [Library Packages](<packages.md> "packages") a study about as of yet unimplemented **Library Packages** (like Delphi's .BPL), where packages are reusable parts of the program in special files (DLL with special interfaces). These can also be dynloaded.

---

_Source: [https://wiki.freepascal.org/packages(disambiguation)](https://web.archive.org/web/20250219174501/https://wiki.freepascal.org/packages(disambiguation))_
