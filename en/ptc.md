# ptc

PTCPas is a free, portable framebuffer library, written in Free Pascal. It allows low-level high-speed graphics access on multiple platforms and is distributed under the terms of a modified (to allow static linking) GNU LGPL license. Currently supports DirectX, X11, VBE1.2+ and fakemodes. It has been tested on Windows (all versions since Windows 95; on i386 and x86_64), Linux (i386, x86_64 and ppc), FreeBSD and DOS. 

There's a BGI-compatible graph unit, implemented on top of PTCPas, called _ptcgraph_ (and two additional units that are meant to be used with it for keyboard & mouse input - _ptccrt_ and _ptcmouse_). It runs on modern Linux distros (using only libX11, no need for svgalib) and offers better compatibility between platforms (meaning that it emulates quite well the graphics modes of the TP7 DOS and Go32v2 graph units under all supported operating systems).

---

_Source: [https://wiki.freepascal.org/ptc](https://web.archive.org/web/20210127112906/https://wiki.freepascal.org/ptc)_
