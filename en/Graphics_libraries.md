# Graphics libraries

This is a list of libraries designed **only** to work with graphics. If you're looking for a library to be used for [game development](<Portal_Game_Development.md> "Portal:Game Development") you should use a [game framework](<Game_framework.md> "Game framework") or a [game engine](<Game_Engine.md> "Game Engine") instead. 

## Graphics libraries

Name  | Developers  | Platforms  | License  | Raster  | Vectors  | 3D  | Supported File Formats  | Color formats or framework   
---|---|---|---|---|---|---|---|---  
[AggPas](<http://www.crossgl.com/aggpas/>) | Milano | Windows, Linux, Mac OS | [Custom](<http://www.crossgl.com/aggpas/aggpas-licence.htm>) | Yes | Yes | No | BMP, SVG, Flash (TXT), True Type Font | 8-bit grayscale, 15-bit, 16-bit, 24-bit RGB and 32-bit RGB with alpha   
[Andorra 2D](<http://andorra.sourceforge.net/>) | [Team](<http://andorra.sourceforge.net/index1013.html?section=credits>) | Windows and Linux | Common Public License, GNU GPL | Yes | No | No | BMP, PNG, TGA | DirectX and OpenGL   
[BGRABitmap](<BGRABitmap.md> "BGRABitmap") | Circular | Win32, Win64, Linux (Gtk & Qt), Mac OS (Carbon) | Modified LGPL | Yes | Yes | Yes | read-write: BMP (Windows, OS/2, MioMap), ICO/CUR, JPEG, animated PNG and GIF, PCX, LZP, ORA, OXO, SVG, TGA, TIFF (multipage), XWD, XPM; read-only: PDN, PSD; with library: AVIF, WebP | 8-bit grayscale, 32-bit BGRA, 64 and 128-bit RGBA, 64 and 128-bit XYZA, Canvas2d (like HTML5), CanvasBGRA (like TCanvas), OpenGL, ZenGL   
[fcl-image](<fcl-image.md> "fcl-image") | Team | All platforms | Modified LGPL | Yes | No | No | BMP, JPG, PCX, PNG, TGA | Various formats up to 64-bit RGBA   
[fpVectorial](<fpvectorial.md> "fpvectorial") |  |  |  | Yes | Yes | Yes | SVG, WMF, PDF, Corel Draw, DXF | Supports raster images placed inside the vectorial document. Supports documents with multiple pages.   
[Graphics32](<http://graphics32.org>) | Team Graphics32 | Win32 and OS X | MPL and LGPL | Yes | No | No | BMP, JPG, WMF, EMF | 32-bit RGBA   
Lazarus [TCanvas](<Developing_with_Graphics.md> "Developing with Graphics") | Lazarus developers | All supported by the LCL | Modified LGPL | Yes | No | No | BMP, ICO, ICNS, PNG, JPEG, GIF, etc | Represents the native canvas of the widgetset   
Lazarus [TLazCanvas+TLazIntfImage](<Developing_with_Graphics.md> "Developing with Graphics") | Lazarus developers | All supported by the LCL | Modified LGPL | Yes | No | No | Only via fcl-image | Supports almost any pixel format. Cannot render to the screen directly, but instead is utilized in connection with something that can, like TCanvas or LCL-CustomDrawn   
[LazRGBGraphics](<LazRGBGraphics.md> "LazRGBGraphics") | Abandoned (formerly Tom Gregorovic) |  | Modified LGPL | Yes | No | No |  | 8-bit grayscale and 32 bit RGB without alpha   
[Vampyre Imaging Library](<http://imaginglib.sourceforge.net/>) | Marek Mauder | Windows x86/AMD64, Linux x86/AMD64, FreeBSD x86/AMD64, Mac OS X on Intel | Mozilla Public License, GNU LGPL | Yes | No | No | BMP, JPEG, PNG/APNG, GIF, DDS, TGA, MNG, JNG, JPEG2000, TIFF, PSD, PBM, PGM, PPM, PAM, PFM, PCX, XPM, and more | 8, 16, 24, 32, 48 and 64 bit RGB and ARGB formats, indexed formats, grayscale formats, floating point formats (IEEE754 and half precision), compressed formats like DXT1/3/5, 3Dc, and BTC   
  
## See also

  * [Game framework](<Game_framework.md> "Game framework")
  * [Game Engine](<Game_Engine.md> "Game Engine")

---

_Source: [https://wiki.freepascal.org/Graphics_libraries](https://web.archive.org/web/20240414002845/https://wiki.freepascal.org/Graphics_libraries)_
