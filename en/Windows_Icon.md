# Windows Icon

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Icon Editor
  * 2 Default Set
  * 3 DPI Modes
  * 4 External Links



## Icon Editor

Those are programs that you can use on design time of your icon: 

  * [Lazarus Image Editor](<Lazarus_Image_Editor.md> "Lazarus Image Editor") _Open source_ and written in Lazarus
  * [LazPaint](<LazPaint.md> "LazPaint") _Open source_ and written in Lazarus
  * [Greenfish Icon Editor Pro](<http://greenfishsoftware.blogspot.nl/2012/07/greenfish-icon-editor-pro.html>) _Freeware_
  * [GIMP](<http://gimp.org/>) _Open source_
  * [Inkscape](<http://inkscape.org/>) _Open source_



With _LazPaint_ , _GIMP_ , _Inkscape_ or the application you want design the icon. With _Lazarus Image Editor_ or _Greenfish Icon Editor Pro_ save as .ico format. 

## Default Set

The default set contains the icon sizes that work in Windows in a DPI setting of 96 (100% - Default). 

  * 16x16: Displayed on the task bar, lists and headers
  * 32x32: Displayed on the desktop, control panel
  * 48x48: Displayed in Explorer when thumbnail or titles view is selected
  * 256x256: Windows Vista+



**16x16:** This is used for Windows Explorer "_Detail view_ ", "_List view_ ", "_Small icons_ ", it is the application icon in a window and the icon in the notification area. 

**32x32:** This is used for Windows Explorer "_Content view_ ", desktop "_Small icons_ ", application icon in taskbar and start menu icons. 

**48x48:** This is used for Windows Explorer "_Mosaic view_ ", "_Medium size icon_ " and desktop "_Medium size icon_ ". 

**256x256:** This is used for Windows Explorer "_Big icons_ " and desktop "_Big icons_ ". 

Also the 256x256 icon is scaled, depending of the OS needs, to intermediate sizes between 48x48 and 256x256. 

## DPI Modes

The default set is for 96 DPI, but if you are running in [High DPI](<High_DPI.md> "High DPI") you need bigger icons, scaled depending on the DPI setting. 

**120 DPI (125%):**

  * _16x16_ > 20x20
  * _32x32_ > 40x40
  * _48x48_ > 60x60
  * 256x256



**144 DPI (150%):**

  * _16x16_ > 24x24
  * _32x32_ > 48x48
  * _48x48_ > 72x72
  * 256x256



**192 DPI (200%):**

  * _16x16_ > 32x32
  * _32x32_ > 64x64
  * _48x48_ > 96x96
  * 256x256



The most used High DPI settings are 120 & 144\. If you want **quality icons** in **High DPI** you must include in your set: 
    
    
    16x16 ; 32x32 ; 48x48 ; 256x256 // Default set 96 DPI
    20x20 ; 40x40 ; 60x60           // Additional used in 120 DPI
    24x24 ; 72x72                   // Additional used in 144 DPI
    

If you include only the **default set** your image icons will be **scaled by the OS** to the needed size but **loss of quality** will occur. 

_Image: icon set comparison in High DPI._

[![window icon comparison.png](https://wiki.freepascal.org/images/1/16/window_icon_comparison.png)](</File:window_icon_comparison.png>)

_Top:_ full icon set for High DPI. Quality icon. 

_Bottom:_ icon set for 96 DPI. Pixelated icon. 

## External Links

  * [Microsoft: Windows Icons](<https://docs.microsoft.com/en-us/windows/desktop/uxguide/vis-icons>) Design Concepts and Guidelines.



(_Note_ : In this article some things may differ to those explained in our article which was written based on tests in Windows 7).

---

_Source: [https://wiki.freepascal.org/Windows_Icon](https://web.archive.org/web/20240910120509/https://wiki.freepascal.org/Windows_Icon)_
