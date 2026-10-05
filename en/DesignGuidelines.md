# DesignGuidelines

│ **English (en)** │  **[русский (ru)](<../ru/DesignGuidelines.md>)** │

## Coding Guidelines for Lazarus

Coding style
    

  * Since one style is easier to read, Lazarus follows the [CodeGear Coding Style Guide](<http://edn.embarcadero.com/article/10280>) lines. Of course, almost anyone will find some points there, that are arguably less readable than other styles. That's OK, just try to follow at least 90%. See the LCL for examples.
  * Try to avoid unit cycles (uses section in implementation sections). Reasons: 1. FPC has problems with them. 2. This allows to split units and packages when they have grown big.
  * Minimize the number of calls from Interfaces to LCL, when performing an action requested by the LCL. The interfaces only notify the LCL, never force something. The LCL decides.
  * Naming convention: see [Nomenclature](<Nomenclature.md> "Nomenclature")
  * All code must work with all checks (range, I/O, overflow, stack) on. Apart from the fact that this helps debugging, some users put these checks into their fpc.cfg, so they are applied to whole Lazarus - including packages and examples.
  * Names in comments: Comments should help the reader. They are not for the glory of the writer. You can add a name if you are the maintainer aka get the bug reports assigned or if the name is a good search term to find help on this topic.



New files
    

  * Every file should start with a header containing the license and a few lines describing the content.
  * Pascal sources should have lowercase filenames (.pas, .pp, .inc, .lfm, .lrs). You can use CamelCase for unit names.



Include files
    

  * should start with the {%MainUnit } directive



Packages
    

  * should have an .lpl entry in packager/globallinks/
  * should have an author, description and license



Main Menu Items
    

  * Should have a key in keymapping.pp



* * *

_The authoritative version can be found in[svn](<http://svn.freepascal.org/svn/lazarus/trunk/docs/DesignGuidelines.txt>). Proposals for improvement can be added to talk page (discussion)_. 

### GUI

See [GUI design guidelines](<GUI_design_guidelines.md> "GUI design guidelines")

## See also

  * Free Pascal (FPC) coding standard (used for the compiler and other FPC code): [Coding style](<Coding_style.md> "Coding style")

---

_Source: [https://wiki.freepascal.org/DesignGuidelines](https://web.archive.org/web/20240914130034/https://wiki.freepascal.org/DesignGuidelines)_
