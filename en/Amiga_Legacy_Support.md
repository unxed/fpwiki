# Amiga Legacy Support

[![Amiga-checkmark.png](https://wiki.freepascal.org/images/f/f2/Amiga-checkmark.png)](</File:Amiga-checkmark.png>)

This article applies to [Amiga](</Category:AmigaOS> "Category:AmigaOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

This page details compiling for "legacy" AmigaOS/m68k versions, specifically AmigaOS 1.x; traditionally also called Kickstart 1.x. (Kickstart describes the ROM part of the operating system, while Workbench is the desktop, usually loaded from a floppy or hard disk.) At this point the support is in an initial state, with some features missing. These will be added as needed. Note that the support is only present in the SVN trunk. There are no plans to backport it to the stable 3.2.x branch. 

The traditional [AmigaOS 3.x](<Amiga.md> "Amiga") support serves as a baseline for all information here, this page only details the differences. 

## Contents

  * 1 Requirements
  * 2 Compiler
  * 3 Default CPU
  * 4 Stack and Heap
  * 5 Compiling the RTL with "legacy" support
  * 6 Linker
  * 7 Missing features



## Requirements

At this point, the minimum version a "legacy Amiga" Free Pascal RTL can work with is AmigaOS v1.2 (internal version v33). This was the lowest version ever shipped on a physical ROM chip in any Amiga system. 

## Compiler

The same compiler which targets "high-end" classic Amiga systems can also generate binaries for "legacy" systems with the right configuration and command line switches. There is no need to use a separate compiler. Only a separate set of RTL and Packages units are needed. 

## Default CPU

Most "legacy" Amiga systems run with the stock Motorola 68000 CPU, while Free Pascal defaults to generating code for the Motorola 68020 or newer CPUs. This can be changed with the **-Cp68000** command line argument. For further information on legacy m68k CPU support, including special support for unaligned accesses, see the [m68k](<m68k.md> "m68k") CPU page. 

## Stack and Heap

Free Pascal's run time library on Amiga tries to allocate 256KiB stack for safety reasons, and tries to allocate 1MiB of heap on startup for performance reasons. When targeting limited systems, it is recommended to lower these values using the **-Cs** and **-Ch** command line arguments, or by adding the **{$MEMORY}** directive to the main program. Example: 
    
    
    // This will allocate 8KiB stack and 64KiB heap on program start
    {$MEMORY 8192,65536}
    

## Compiling the RTL with "legacy" support

To compile an RTL which is able to run on AmigaOS versions lower than 3.0 (pre-v39), you need to specify the define **AMIGA_V1_2_ONLY** while compiling the RTL. This can be achieved by adding the **-dAMIGA_V1_2_ONLY** argument to the OPT part of _make_ arguments. This will automatically exclude code from the RTL which depends on newer OS functions, and automatically include some helper functions which are required to support the RTL on these very old systems. Example: 
    
    
     make rtl OPT="-Cp68000 -dAMIGA_V1_2_ONLY"
    

Note that the "legacy" RTL is functional on a newer AmigaOS as well, but as a trade-off it has to use quite a few hacks and digging into system structures, to bridge the gap created by the lack of necessary OS features. Additionally, it also uses a less advanced memory allocation scheme, and might lack some features. Therefore it is not recommended to ship executables linked with the legacy RTL, when the primary targets are "high-end" Amiga systems. 

## Linker

Free Pascal defaults to _vlink_ on Amiga. For the legacy support to work correctly, **vlink 0.16c or newer** is recommended. 

## Missing features

  * FindFirst/FindNext functions of the RTL's DOS and sysutils units depend on _dos.library v36+_ functions, replacements are partially implemented with basic functionality and don't support full pattern matching for example (further improvements planned)
  * environment variable support of the RTL's DOS and sysutils units depend on _dos.library v36+_ functions, these are not functional (planned)
  * setting a file's datestamp is not trivial, so related DOS and sysutils functions might not be functional (might not be possible)
  * some file operations are non-functional, like _truncate()_ (not easily possible)
  * ParamStr(0) will not include the full path of the executable in all cases (not easily fixable)
  * probably more...

---

_Source: [https://wiki.freepascal.org/Amiga_Legacy_Support](https://web.archive.org/web/20250122180434/https://wiki.freepascal.org/Amiga_Legacy_Support)_
