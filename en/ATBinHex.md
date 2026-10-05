# ATBinHex

## Contents

  * 1 About
  * 2 Limitations of Lazarus port
  * 3 View modes
  * 4 Download



# About

_ATBinHex_ is a control that implements the quick file (stream) viewer. Only visible part of file (or stream) is loaded into viewer, so it's suitable to show files of unlimited size. 

Original Delphi code is used for a long time inside "Universal Viewer" application. 

Author: Alexey Torgashin 

# Limitations of Lazarus port

  * Some features were disabled via ATBinHexOptions.inc file (searching of text, printing, RegEx highlighting of URLs).
  * Removed method OpenFile, because it used Win32 API; use OpenStream instead.



# View modes

There are 5 view modes available: 

Text: file is shown in text form 

[![atbinhex ModeText.gif](https://wiki.freepascal.org/images/5/51/atbinhex_ModeText.gif)](</File:atbinhex_ModeText.gif>)

Binary: file is shown in binary form (with fixed line length) 

[![atbinhex ModeBinary.gif](https://wiki.freepascal.org/images/9/9b/atbinhex_ModeBinary.gif)](</File:atbinhex_ModeBinary.gif>)

Hex: file is shown in hex dump 

[![atbinhex ModeHex.gif](https://wiki.freepascal.org/images/f/fd/atbinhex_ModeHex.gif)](</File:atbinhex_ModeHex.gif>)

Unicode: Unicode contents of file is shown 

[![atbinhex ModeUnicode.gif](https://wiki.freepascal.org/images/7/7e/atbinhex_ModeUnicode.gif)](</File:atbinhex_ModeUnicode.gif>)

Unicode/Hex: combined Hex and Unicode modes 

[![atbinhex ModeUHex.gif](https://wiki.freepascal.org/images/0/0b/atbinhex_ModeUHex.gif)](</File:atbinhex_ModeUHex.gif>)

# Download

  * Homepage of Lazarus port: <https://github.com/Alexey-T/ATBinHex-Lazarus>
  * Homepage of Delphi original, with all functions including printing, searching, support for many codepages: <https://github.com/Alexey-T/ATViewer>

---

_Source: [https://wiki.freepascal.org/ATBinHex](https://web.archive.org/web/20250417141241/https://wiki.freepascal.org/ATBinHex)_
