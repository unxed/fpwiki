# PTCPas

**PTCPas** is a free, portable framebuffer library, written in Free Pascal. It allows low-level high-speed graphics access on multiple platforms and is distributed under the terms of a modified (to allow static linking) GNU LGPL license. Currently supports DirectX, X11, VBE1.2+ and fakemodes. It has been tested on Windows (all versions since Windows 95; on i386 and x86_64), Linux (i386, x86_64 and ppc), FreeBSD and DOS. 

Homepage: <http://ptcpas.sourceforge.net/>

# ptcgraph

There's a BGI-compatible **graph** unit, implemented on top of PTCPas, called **ptcgraph** (and two additional units that are meant to be used with it for keyboard & mouse input - **ptccrt** and **ptcmouse**). It runs on modern Linux distros (using only libX11, no need for svgalib) and offers better compatibility between platforms (meaning that it emulates quite well the graphics modes of the TP7 DOS and Go32v2 graph units under all supported operating systems). 

Quote from forum user _Nitorami_ : 

    "I used ptcgraph a lot in console programs because it is MUCH faster than graph and practically 100% compatible. Just exchange "uses graph" by "uses ptcgraph"... It depends on ptc, hermes, GLext and a few more so you'll need to tell the compiler where to find all those but once that is done, it works like a charm."

---

_Source: [https://wiki.freepascal.org/PTCPas](https://web.archive.org/web/20241127122841/https://wiki.freepascal.org/PTCPas)_
