# LazRGBGraphics

│ **English (en)** │

## Contents

  * 1 About
  * 2 Screenshots
  * 3 Author
  * 4 License
  * 5 Download
  * 6 Change Log
  * 7 TODO
  * 8 Notes
  * 9 Installation



### About

LazRGBGraphics is a run-time package for fast in memory image processing and pixel manipulations (like scan line). The main advantage is direct memory access to bitmap pixels with keeping ability to draw bitmap onto canvas without any time consuming widgetset memory format converting. 

The main class is TRGB32Bitmap which is analog to TBitmap. 

TRGB32Bitmap features: 

  * load from file, save to file
  * creating from TBitmap
  * drawing and stretchdrawing to TCanvas
  * rotating, stretching
  * inverting colors
  * drawing primitives via canvas (TRGB32Canvas) with emphasis on accuracy
  * per pixel manipulation via GetPixelPtr



The download contains the package and simple example application. 

This package was designed for cross-platform usage. 

### Screenshots

[![Example application](https://wiki.freepascal.org/images/d/d4/LazRGBGraphics_example.png)](</File:LazRGBGraphics_example.png> "Example application") [![Drawing primitives](https://wiki.freepascal.org/images/2/29/LazRGBGraphics_canvas.png)](</File:LazRGBGraphics_canvas.png> "Drawing primitives")

### Author

[Tom Gregorovic](</User:Tombo> "User:Tombo")

### License

Modified LGPL 

### Download

[LazRGBGraphics on the Lazarus CCR at SourceForge.net](<http://sourceforge.net/projects/lazarus-ccr/files/LazRGBGraphics/>)
    
    
    svn checkout <https://svn.code.sf.net/p/lazarus-ccr/svn/components/rgbgraphics>
    

Or get the tarball [rgbgraphics.tar.gz](<http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/components/rgbgraphics.tar.gz?view=tar>)

### Change Log

  * Version 0.2.1 
    * fixed compilation for GTK, use define GTK_POST_0924 if you are using Lazarus 0.9.25 or higher
    * fixed saving, loading and copying
    * _requires Lazarus 0.9.24 or higher_
  * Version 0.2 
    * implementation for Carbon
    * _requires Lazarus 0.9.24 or higher_
  * Version 0.1



### TODO

  * halftone stretching 0.3
  * polygons 0.3
  * masking 0.3
  * alpha blending 0.4



### Notes

Status: Alpha 

Issues: 

  * Does not yet work with QT widgetset
  * tested with win32/64 and gtk2 on Windows XP
  * tested with carbon and gtk2 on Mac OS X 10.4.7 (Intel)
  * tested with carbon and gtk1 on Mac OS X 10.4.7 (PPC)
  * tested with gtk1 and gtk2 under Linux (Kubuntu 6.06)
  * tested with gtk1 under FreeBSD 6.1 (by Almindor)
  * tested on AMD64 with gtk1 Debian/Etch (by Tanila)



### Installation

Add LazRGBGraphics package as dependancy to the project and RGBGraphics to the uses section.

---

_Source: [https://wiki.freepascal.org/LazRGBGraphics](https://web.archive.org/web/20240909221746/https://wiki.freepascal.org/LazRGBGraphics)_
