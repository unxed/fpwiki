# Free Vision

│ **[Deutsch (de)](</Free_Vision/de> "Free Vision/de")** │  **English (en)** │  **[日本語 (ja)](</Free_Vision/ja> "Free Vision/ja")** │  **[português (pt)](</Free_Vision/pt> "Free Vision/pt")** │    
****

## Contents

  * 1 Overview
  * 2 To do list
  * 3 Unicode version
  * 4 Turbo Vision
    * 4.1 Turbo Vision code bases
      * 4.1.1 C++
      * 4.1.2 Pascal
  * 5 Problems on macOS
  * 6 Tutorial



## Overview

**Free Vision** is an application framework that allows you to design user friendly text mode applications. It is designed to be API-compatible with Borland's Turbo Vision framework. 

Free Vision implements a full windowing system, where each window can be updated, moved and operated independently from other windows. It is implemented on top of the [video](<video_unit.md> "video unit"), [keyboard](</index.php?title=keyboard_unit&action=edit&redlink=1> "keyboard unit \(page does not exist\)") and [mouse](</index.php?title=mouse_unit&action=edit&redlink=1> "mouse unit \(page does not exist\)") units and Free Vision applications can therefore run on most FPC platforms using the _write once, compile anywhere_ principle. 

The [Free Pascal IDE](<Textmode_IDE.md> "Textmode IDE") uses Free Vision for its user interface. [Textmode IDE development](<Textmode_IDE_development.md> "Textmode IDE development") lists a simple object chart of IDE + FV objects. 

Free Vision is not 100% complete: 

  * the colorsel unit is missing, and the support to use it (palette support) might not be complete and/or debugged since the TV->FV transition in +/- 2000.



**Availability:**

  * Debian Linux: => install the package: [fp-units-fv](<http://packages.debian.org/hu/stable/fp-units-fv>)
  * Other distributions/OSes: Free Vision is included with the FPC source package.



## To do list

These items were written on a separate page, last edited May 2008: 

  * getting it fully up to speed and compatible with TV
  * endianness and 64-bit clean work.
  * filepaths to ansistring ? TP compat less of a problem nowadays. (copy to separate version if necessary?)



## Unicode version

Original Turbo Vision stored screen characters in 1-byte cells allowing only single byte code pages to be used (like 437, 866, 1251 etc), also did Free Vision. This is a serious limiting factor for use of Free Vision on modern systems. An [issue about Unicode support](<https://gitlab.com/freepascal.org/fpc/source/-/issues/21373>) presents in FPC bug tracker for a long time. 

As of February, 2022, adoption of Free Vision to Unicode has been merged into the FPC main branch. This branch has already been tested on Linux and Windows and considered stable at least on these two platforms. 

It requires FPC 3.1.1 or later, because it uses [management operators](<management_operators.md> "management operators"). It uses them to store Unicode character cells in the enhanced video unit buffer. Each "character" on the screen is actually a unicode string, because it can contain several unicode code points, e.g. combining characters, etc. Management operators was used to avoid extra overhead of the unicode string (an extra heap block + reference count, etc) when a character cell is only a single unicode code point, which improves performance a lot with commonly used characters, such as ASCII, Cyrillic, etc, while still supporting languages with combining characters. 

[Instruction](<https://github.com/elfmz/far2l/issues/1255#issuecomment-1042898940>) on how to build Free Vision Unicode test app from sources on recent Ubuntu. 

**Supported features:**

1\. non-BMP characters 

2\. East Asian (wide) character support 

3\. combining characters 

4\. fallback to legacy non-Unicode KVM drivers for operating systems that don't support Unicode 

5\. backward compatibility FV units that use legacy codepages. All the unicode-enabled FV units have a 'U' prefix, i.e. 'UDrivers', 'UDialogs', 'UApp', etc. A lot of code sharing between the two sets of units (unicode and legacy) is achieved by using include files and ifdefs. 

6\. unlimited string length in the new Unicode FV units (they're based on the UnicodeString type, which is UTF-16 and unlimited length). Legacy units still have shortstring limitations in certain places. 

Things **not planned** to be implemented for now (maybe later, after merging the branch): 

1\. bidirectional text & right-to-left language support 

2\. Arabic shaping 

Reason: author is entirely unfamiliar with right-to-left languages and also support in consoles is still rare and not very well standardized. However, the gnome-terminal now supports them, and the authors have proposed a [spec](<https://terminal-wg.pages.freedesktop.org/bidi/implementations.html>) for it, which hopefully may be adopted by other terminals also, so it should be possible to implement support for terminals that follow this spec. Any help, from people who know a right-to-left language, or have experience in implementing bidi text support is welcome. 

**Remaining things to be done:**

1\. Support the new Windows 10 console that supports non-BMP characters 

2\. Port and test any remaining FV units and controls (most of them are probably ported, but the author might have left some unit out - look for FV units that don't have any Unicode versions with the 'U' prefix) 

3\. Port and test other platforms, besides Windows and Linux 

4\. [Port](<https://gitlab.com/freepascal.org/fpc/source/-/issues/40674>) the IDE to the new Unicode FV 

5\. (not very important) Implement better handling of emoji width (e.g. currently the infamous U+1F4A9 character (💩) is double width, but is currently treated as single width, which results in broken rendering, so these kind of things should be handled properly in the EastAsianWidth unit) 

## Turbo Vision

The Turbo Vision framework originates from Borland, and was delivered with the later incarnations on the Turbo range of compilers. There were two versions, a C++ one, and a Pascal one. The C++ was open sourced (PD) at some point, the Pascal didn't follow, which prompted FPC to search for an alternative. 

This was found in the shape of Free Vision, which (afaik) was a backport of the PD C++ code back to Pascal by Leon de Boer for use in graphics mode. As far as I can remember, Pierre and Peter backported the code back to textmode (coordinate system change that affected a lot of code!), ported missing parts, improved compatibility, and hooked it into the Textmode IDE. Note that the sf.net TVISION page falsely states that FPC uses Pascal sources donated by Borland, however it seems that nobody reacts to mails there. 

### Turbo Vision code bases

There are currently a handful of Turbo Vision based codebases, all open source ones derived from the C++ version released by Borland. 

#### C++

  * The C++ version as released by Borland (Public Domain). Unfortunately after all the mergers of Borland, the FTP server it was on is dead.
  * Possibly non free versions as delivered with Turbo C++ (and Object Vision?).
  * The SF.net C++ version, but it is GPL (a GPL library, which severely hampers usage).
  * Another alternative is an older C++ Turbo Vision version (BSD license) at [this homepage](<http://www.sigala.it/sergio/tvision/>).
  * There is also a modern alternative with support for C++17, UTF-8, etc., at this [GitHub repository](<https://github.com/magiblot/tvision>). This one is MIT licensed.



#### Pascal

  * The commercial versions packaged with Turbo Pascal compilers. These are still not open source in any form.
  * The (AFAIK heavily modified) Borland Pascal 7 version that was used for Virtual Pascal (not open).
  * Free Vision (LGPL license with linking exception). See above for details. Based on the C++ version.
  * Free Vision [adopted for UTF-8](<https://github.com/unxed/fv_utf8>).
  * Free Vision [Unicode adoption branch](<https://gitlab.com/freepascal.org/fpc/source/-/issues/21373#note_833019965>).



## Problems on macOS

User wrote: Unable to close Free Vision program on macOS, when Alt+X keyboard shortcut is pressed. 

FPC developer Jonas Maebe replied: 

Under native macOS programs, by default Option+key ("Option" is the name of the "Alt" key on macOS) is a way to enter a special character; Option/Alt is not mapped to Meta like on most other Unix platforms. If you want to map Option to Meta in Terminal, go to "Preferences -> Profiles -> Keyboard" and tick the "Use Option as Meta key" checkbox at the bottom. 

## Tutorial

German (Deutsches) tutorial to Free Vision: [GitHub repo](<https://github.com/sechshelme/Lazarus-FreeVision-Tutorial>). 

The documentation of the individual components is available directly in the sources.

---

_Source: [https://wiki.freepascal.org/Free_Vision](https://web.archive.org/web/20250523120154/https://wiki.freepascal.org/Free_Vision)_
