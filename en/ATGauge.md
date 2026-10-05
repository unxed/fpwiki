# ATGauge

## About

_ATGauge_ is OS-independant progress indicator component. It's like Delphi TGauge. The same properties (with some additions) but totally new code (it's smaller than Delphi 7 code). It has class name and properties compatible with the Delphi 7 version, but different unit name. 

[![atgauge.png](https://wiki.freepascal.org/images/b/b2/atgauge.png)](</File:atgauge.png>)

"Kind" property supports same kinds as D7: 

  * only text
  * horiz bar
  * vert bar
  * needle (half-circle)
  * pie (full circle)



Author: Alexey Torgashin (Russia) 

License: MPL 2.0 or LGPL. 

## Download

Homepage at github: <https://github.com/Alexey-T/ATFlatControls>

## Property ShowTextInverted

This property is to look like Delphi's component. 

  * Off: text (e.g. "20%") is painted with color Font.Color at all places. This is fast.
  * On: text is painted with inverted color regarding image under it. Font.Color is ignored. Temp bitmap is created (with a size of text), then this bitmap is copied over image-bitmap with Canvas.CopyMode=cmSrcInvert. This is slower and don't work on GTK2 (I see pixelated text over green bar). This is OK on Windows and QT.

---

_Source: [https://wiki.freepascal.org/ATGauge](https://web.archive.org/web/20250417161603/https://wiki.freepascal.org/ATGauge)_
