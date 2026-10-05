# Unit categorization

**Unit categorization** of units provided in the [Free Pascal](<Free_Pascal.md> "Free Pascal") [Runtime Library](<RTL.md> "RTL") is based on different criteria into several groups. 

Criteria used below include different programming models (procedural, [Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal")/[Borland Pascal](<Borland_Pascal.md> "Borland Pascal") style OOP, [Delphi](<Delphi.md> "Delphi") style OOP), whether ansistrings (or more generally, reference counted types) are used or not (ansistrings are by default on with -Mdelphi/$MODE DELPHI or $H+), whether the units are cross-platform or platform specific units (with information about their availability/useability for different targets). Furthermore, basic information about type of functionality to be found in individual units should hopefully help especially new users to understand better where to search for particular routines. Finally, availability of individual units in other Pascal compilers (at least whether they are available with TP/BP, Delphi or whether they are FPC specific) are mentioned. 

## Contents

  * 1 Procedural programming, short strings, cross-platform
  * 2 Procedural programming, ansistrings, cross-platform
  * 3 TP/BP style OOP, shortstrings, cross-platform
  * 4 Delphi style OOP, ansistrings, cross-platform
  * 5 Procedural programming, shortstrings
    * 5.1 Various units available for more than one platform
    * 5.2 Linux specific units
    * 5.3 GO32v2 specific units
    * 5.4 Classic Mac OS specific units
    * 5.5 MorphOS specific units
    * 5.6 Netware specific units
    * 5.7 OS/2 specific units
    * 5.8 (Free)BSD specific units



## Procedural programming, short strings, cross-platform

System
    Basic type definition, execution control, parameter processing, string handling, type conversions, mathematic operations, memory management, basic routines for console and disk I/O, constants and variables describing the particular platform and/or current environment (available in TP/BP and Delphi)

Strings
    Operations with [PChar](<PChar.md> "PChar") strings (available in TP/BP)

Matrix
    Vector and matrix routines (FPC specific)

UComplex
    Operations with complex numbers (FPC specific)

GetOpts
    Routines for more advanced access to program parameters

Keyboard
    Low-level access to keyboard (FPC specific)

Mouse
    Mouse support (FPC specific)

Video
    Routines for low-level access to screen (FPC specific)

Printer
    Very basic access to printer (available in TP/BP)

Sockets
    Basic support for TCP/IP socket communication (FPC specific)

CMem
    Replacement memory manager based on C run-time library to allow easier integration with some code written in C (FPC specific)

CTypes
    Definition of C language types for easier integration with routines written in C (FPC specific)

InitC
    Some functions required for linking of some libraries written in C

MacPas
    Special unit supporting constructs formerly available in some Pascal compilers for Mac OS platform (ThinkPascal, Metrowerks etc.); available for all platforms with FPC to support -Mmacpas/$MODE MACPAS (FPC specific)

HeapTrc
    Debugging support unit for tracing of incorrect memory allocation (more or less FPC specific, at least not available with TP/BP and probably Delphi either)

LineInfo
    Debugging support unit to allow displaying of location (line) in source files in case of an error based on debug information linked to executable file (FPC specific)

Crt
    More advanced console (keyboard and screen) routines (TP/BP compatibility unit plus some FPC specific functions)

Dos
    More advanced disk I/O, execution flow (including running of other programs), date and time handling and access to environment variables, plus some compatibility routines only available/making sense for DOS platform (TP/BP compatibility unit plus some FPC specific functions; some of the functions were provided in WinDos unit in older Delphi versions)

Graph
    Simple graphics support (TP/BP compatibility unit)

## Procedural programming, ansistrings, cross-platform

SysUtils
    More advanced disk I/O, execution control (including exception handling and running of other programs), access to environment variables, string routines (support of ansistrings and widestrings), conversion routines (strings, numbers, date/time), internationalization support (Delphi compatibility plus some FPC specific functions)

StrUtils
    Various more advanced string routines (Delphi compatibility)

DateUtils
    Date/time handling routines (Delphi compatibility)

ConvUtils
    Conversion between different units of measurement (Delphi compatibility)

SysConst
    Resource texts for SysUtils (Delphi compatibility?)

RTLConst
    Resource texts for other RTL libraries (especially unit Classes)

DynLibs
    On-demand (run time) loading of dynamic libraries

Math
    More complex mathematic and statistic routines; no ansistrings really used here directly, although it loads unit SysUtils and thus installs its exception handler (Delphi compatibility with FPC specific additions)

FreeBIDI
    Helper routines for bidirectional text output (FPC specific)

Utf8BIDI
    Helper routines for conversion of UTF-8 encoded text supporting bidirectional output (FPC specific)

ObjPas
    Basic support for -Mobjfpc and -Mdelphi including some type redefinitions and resource string support; unit used automatically for those modes, no need to add it manually (FPC specific, although the provided routines are compatible to routines available System unit with Delphi)

## TP/BP style OOP, shortstrings, cross-platform

Objects
    Base of the object hierarchy, disk I/O and memory operations for OOP programs - streams, resources, lists and collections (TP/BP compatibility)

FV (Free Vision) units
    Complete framework for writing TP/BP-style OOP programs with text-mode windowing user interface (TP/BP compatibility - more or less compatible to Turbo Vision)

## Delphi style OOP, ansistrings, cross-platform

Classes
    Base of the class hierarchy, disk I/O, execution control and memory operations for OOP programs - streams, resources, lists, collections, threads, components, actions, interfaces, data modules etc. (Delphi compatibility)

VarUtils
    Variant support routines (Delphi compatibility?)

FCL units
    Non-visual classes for OOP programming (partly similar to Delphi VCL, but only non-visual parts are provided here; visual/GUI classes are provided with [Lazarus](<Lazarus.md> "Lazarus"))

## Procedural programming, shortstrings

Ansistrings may be used/supported in some cases too, platform-specific. 

This group is fairly extensive, only information about availability for individual platforms is given here. See the description of individual units for more information. In general, these units are FPC specific except for the Windows, WinCrt and WinSock units available with Delphi, and unit LibC available with Kylix. 

### Various units available for more than one platform

Ports
    Support of port access construct provided with TP/BP on x86 platforms

CPU and MMX
    Support of some low-level CPU-specific functions on x86 platforms

UnixType, BaseUnix, Unix, UnixUtil, SysCall, IPC
    Unix platforms (Linux, *BSD including Mac OS X, Solaris, NetwLibC, plus possibly BeOS and QNX again in the future)

GPM
    Unix platforms that support this library (Linux). It is less broadly supported than previous units.

x86
    Unix platforms running on x86

Windows, WinMouse, WinEvent, WinSysUt, WinCrt (plus package WinUnits provided outside of RTL in "Packages")
    MS Windows platforms (Win32, Win64, WinCE)

WinSock
    MS Windows, OS/2 and Netware

### Linux specific units

Linux
    As the names implies, contains routines and symbols specific to Linux.

Libc
    is Linux/x86 only, see [libc_unit](<libc_unit.md> "libc unit") for more information

### GO32v2 specific units

Go32, Emu387, DpmiExcp, DxeType, DxeLoad

### Classic Mac OS specific units

MacOS, MacUtils

### MorphOS specific units

Exec, Timer, DosLib, Utility, Hardware, InputEvent, Graphics, Layers, Intuition, Clipboard, ASL, AHI and Get9

### Netware specific units

Netware, NWCalls, NWNit, NWPre, NWProt, NWServ, NWSNUt

### OS/2 specific units

OS2Def
    Definition of common types and constants

DosCalls
    So-called "Control Program functions" - execution control (including running of other programs, threading, exceptions and error management, critical sections and semaphores), memory management, system and environment information (including environment variables, program and system parameters and national language support), disk I/O, date/time handling, inter-process communication (pipes), low-level system functions (IOCTL and communication with device drivers)

KbdCalls
    Low-level access to keyboard (primarily for non-PM applications)

MouCalls
    Low-level mouse support (primarily for non-PM applications)

VioCalls
    Low-level video (screen) functions (non-PM)

MonCalls
    Support for character device monitors (non-PM applications)

PMWin
    Primary interface for GUI windowing functions provided by Presentation Manager (PM) environment

PMGpi
    Support for graphical operations (PM)

PMStdDlg
    Standard dialogs plus drag and drop support (PM)

PMBitmap
    Bitmap picture operations (PM)

PMBidi
    Support for bidirection text output (PM)

PMHelp
    Interface unit for help feature (PM)

PMSpl
    Access to printer spooler (PM)

PMShl
    Operations with desktop including access to system ini files (PM)

PMWP
    Operations with WorkPlace Shell classes (PM)

PMWSock
    Support for OS/2 socket library with interface modelled after Winsock 1.1

PMDev
    Support for different output devices and their capabilities (PM)

(plus package OS2Units provided outside of RTL in "Packages") 

### (Free)BSD specific units

Some of these may apply to other BSDs too. 

Console
    general "syscons" header.
sysctl
    general sysctl header. Note that many other unices support sysctl, but the MIB names will probably be different on the detail level. (darwin also supports this one)

---

_Source: [https://wiki.freepascal.org/Unit_categorization](https://web.archive.org/web/20250515072458/https://wiki.freepascal.org/Unit_categorization)_
