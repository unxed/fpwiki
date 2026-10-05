# BGRABitmap

│ **English (en)** │  **[русский (ru)](<../ru/BGRABitmap.md>)** │

[![bgrabitmap logo.jpg](https://wiki.freepascal.org/images/7/7b/bgrabitmap_logo.jpg)](</File:bgrabitmap_logo.jpg>)

## Contents

  * 1 Description
  * 2 Additional packages
    * 2.1 NoGUI, NoLCL
  * 3 Using BGRABitmap
    * 3.1 Tutorial
    * 3.2 Overview
    * 3.3 BGRABitmapTypes unit reference
    * 3.4 Installation
    * 3.5 Simple example
  * 4 Notions
  * 5 Integrated drawing functions
  * 6 Drawing with the Canvas
  * 7 Direct access to pixels
    * 7.1 InvalidateBitmap
  * 8 Image manipulation
  * 9 Images combination
  * 10 Screenshots
  * 11 Licence
  * 12 Download
  * 13 See also



## Description

**BGRABitmap** is a set of units designed to modify and create images with transparency (alpha channel). Direct pixel access allows fast image processing. The library has been tested on Windows, Ubuntu and macOS, with widgetsets win32, gtk1, gtk2, Carbon (32 bit) and Cocoa (64 bit). 

The main class is [TBGRABitmap](<TBGRABitmap_class.md> "TBGRABitmap class"), which is derived from [TFPCustomImage](<fcl-image.md> "fcl-image"). There is also [TBGRAPtrBitmap class](<https://bgrabitmap.github.io/doc/BGRADefaultBitmap.TBGRAPtrBitmap.html>) which allows to edit BGRA data that are already allocated. This format consists of 4 bytes for each pixel (blue, green, red and alpha in that order on Windows). 

The image can be rendered on a regular Canvas or on an [OpenGL surface](<BGRABitmap_and_OpenGL.md> "BGRABitmap and OpenGL"). 

As of version 10, additional bitmap types are available: 

  * [TBGRABitmap](<TBGRABitmap_class.md> "TBGRABitmap class"): sRGB, 8 bit per channel.
  * [TGrayscaleMask](<https://bgrabitmap.github.io/doc/BGRAGrayscaleMask.TGrayscaleMask.html>): linear 8 bit grayscale. It now has drawing functions, so you can prepare a mask in 8 bit per pixel, avoiding to consume memory.
  * [TExpandedBitmap](<https://bgrabitmap.github.io/doc/ExpandedBitmap.TExpandedBitmap.html>): linear RGB, 16 bit per channel. It has more precision than [TBGRABitmap](<TBGRABitmap_class.md> "TBGRABitmap class") and is linear, so that dmLinearBlend and dmDrawWithTransparency are equivalent.
  * [TLinearRGBABitmap](<https://bgrabitmap.github.io/doc/LinearRGBABitmap.TLinearRGBABitmap.html>): linear RGB, 32 bit per channel (single precision float). It has even more precision. Not really recommended though as it uses a lot of memory.
  * [TWordXYZABitmap](<https://bgrabitmap.github.io/doc/WordXYZABitmap.TWordXYZABitmap.html>): [XYZ](<BGRABitmap_XYZ.md> "BGRABitmap XYZ"), 16 bit per channel. Can store any real and reflect color with great precision.
  * [TXYZABitmap](<https://bgrabitmap.github.io/doc/XYZABitmap.TXYZABitmap.html>): [XYZ](<BGRABitmap_XYZ.md> "BGRABitmap XYZ"), 32 bit per channel (single precision float). It has even more precision and also a wider range, so that it can store fluorescent colors and light sources that would otherwise saturate.



## Additional packages

Some package use BGRABitmap to provide controls with nice graphics: 

  * BGLControls: provides TBGLVirtualScreen to draw on an OpenGL surface. This package is in BGRABitmap archive.
  * [BGRAControls](<BGRAControls.md> "BGRAControls"): lablels wth shadows, beautiful buttons, shapes, etc.
  * [uE Controls](<uE_Controls.md> "uE Controls"): gauges, LEDs, etc.
  * [Material Design](<Material_Design.md> "Material Design"): Google material design guidelines based UI components.
  * BGRAControlsFX[[1]](<https://forum.lazarus.freepascal.org/index.php?topic=34534.0>): controls rendering on OpenGL surface



Some examples in the test folder use BGRAControls and BGLControls. You may need to install them to open these projects within Lazarus. See [Install Packages](<Install_Packages.md> "Install Packages"). 

### NoGUI, NoLCL

BGRABitmap package has additional .lpk packages: 

  * bgrabitmappack4nogui.lpk: it is in fact related to the LCL, but without graphic user interface (no GUI). So one can use for example LazFreeType with it.


  * bgrabitmappack4nolcl.lpk: it is completely independent from the LCL. There is no more font rendering (neither system rendering nor from TrueType files). Though with another program that has the LCL, you could prepare a TBGRAVectorizedFont, compute necessary glyphs, save it to a file with SaveGlyphsToFile. Then in the program without LCL, load the file with LoadGlyphsFromFile. Note that MSEgui provides system rendering of fonts, so this incorporated into an MSEgui program, you have the usual font rendering: [GitHub issue](<https://github.com/bgrabitmap/bgrabitmap/issues/139#issuecomment-1672184739>).



## Using BGRABitmap

### Tutorial

  * [A series of tutorials to learn step by step](<BGRABitmap_tutorial.md> "BGRABitmap tutorial")
  * [How to convert an application from TCanvas to BGRABitmap](<http://www.youtube.com/watch?v=HGYSLgtYx-U>)
  * [Using BGRABitmap to render a TAChart](<BGRABitmap_tutorial_TAChart.md> "BGRABitmap tutorial TAChart")
  * [A series of projects demonstrating advanced capabilities similar to AggPas](<BGRABitmap_AggPas.md> "BGRABitmap AggPas")
  * Examples in [GitHub](<https://github.com/bgrabitmap>)



### Overview

Functions have long names in order to be understandable. Almost everything is accessible as a function or using a property of the [TBGRABitmap](<TBGRABitmap_class.md> "TBGRABitmap class") object. For example, you can use CanvasBGRA to have some canvas similar to TCanvas (with opacity and antialiasing) and Canvas2D to have the same features as the [HTML canvas](<https://developer.mozilla.org/en/HTML:Canvas>). 

Some special features require the use of units, but you may not need them : 

  * [TBGRAMultishapeFiller](<https://bgrabitmap.github.io/doc/BGRAPolygon.TBGRAMultishapeFiller.html>) to have an antialiased junctions of polygons is in BGRAPolygon
  * [TBGRATextEffect](<https://bgrabitmap.github.io/doc/BGRATextFX.TBGRATextEffect.html>) is in BGRATextFX
  * 2D transformations are in [BGRATransform unit](<https://bgrabitmap.github.io/doc/BGRATransform.html>)
  * [TBGRAScene3D](<https://bgrabitmap.github.io/doc/BGRAScene3D.TBGRAScene3D.html>) is in BGRAScene3D
  * If you need to have layers, BGRALayers provides [TBGRALayeredBitmap](<https://bgrabitmap.github.io/doc/BGRALayers.TBGRALayeredBitmap.html>)



Double-buffering is not really part of BGRABitmap, because it is more about how to handle forms. To do double-buffering, you can use TBGRAVirtualScreen which is in the [BGRAControls](<BGRAControls.md> "BGRAControls") package. Apart from that, double-buffering in BGRABitmap works like any double-buffering. You need to have a bitmap where you store your drawing and that you display with a single Draw instruction. 

### BGRABitmapTypes unit reference

  * [Pixel types and functions](<BGRABitmap_Pixel_types.md> "BGRABitmap Pixel types"): _TBGRAPixel_ , _THSLAPixel_...
  * [Types imported from Graphics](<BGRABitmap_Types_imported_from_Graphics.md> "BGRABitmap Types imported from Graphics"): _TColor_ , pen style...
  * [Color definitions](<BGRABitmap_Color_definitions.md> "BGRABitmap Color definitions"): _VGAColors_ , _CSSColors_...
  * [Geometry types](<BGRABitmap_Geometry_types.md> "BGRABitmap Geometry types"): _TPointF_ , Bezier curves, _TArcDef_...
  * [Miscellaneous types](<BGRABitmap_Miscellaneous_types.md> "BGRABitmap Miscellaneous types"): font, image format, resampling...
  * [TBGRACustomBitmap and IBGRAScanner](<TBGRACustomBitmap_and_IBGRAScanner.md> "TBGRACustomBitmap and IBGRAScanner"): the base class for _TBGRABitmap_ and scanners



### Installation

After unpacking a checkout, BGRA often does not compile in Linux. Try using the IDE Macro 
    
    
     LCLWidgetType:=gtk2
    

in such cases. Still some other part may not compile. 

See [BGRA Installation on Linux](<BGRA_Installation_on_Linux.md> "BGRA Installation on Linux") for step-by-step installation instructions. 

### Simple example

Create a project and open bgrabitmappackage.lpk with Lazarus. In the package window, click on "Use > Add to Project". Then in the source code of the main file (main form or main program), add to uses clause BGRABitmap units. You may need to add BGRAGraphics unit as well if you use certain types that are inherited from the LCL. 
    
    
    Uses 
      Classes, SysUtils, BGRABitmap, BGRABitmapTypes;
    

The unit BGRABitmapTypes contains common definitions, but one can declare only BGRABitmap in order to load and show a bitmap. Then, the first step is to create a [TBGRABitmap](<TBGRABitmap_class.md> "TBGRABitmap class") object: 
    
    
    var
      bmp: TBGRABitmap;
    begin
      bmp := TBGRABitmap.Create(100, 100, BGRABlack); // creates a 100x100 pixels image with black background
    
      bmp.FillRect(20, 20, 60, 60, BGRAWhite, dmSet); // draws a white square without transparency
      bmp.FillRect(40, 40, 80, 80, BGRA(0, 0, 255, 128), dmDrawWithTransparency); // draws a transparent blue square
      ...
    end;
    

Finally to show the bitmap: 
    
    
    procedure TFMain.FormPaint(Sender: TObject);
    begin
      bmp.Draw(Canvas, 0, 0, True); // draw the bitmap in opaque mode (faster)
    end;
    

See a full source code in [tutorial 1](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1"). 

## Notions

Pixels in a bitmap with transparency are stored with 4 values, here 4 bytes in the order Blue, Green, Red, Alpha. The last channel defines the level of opacity (0 signifies transparent, 255 signifies opaque), other channels define color and luminosity. 

There are basically two drawing modes. The first consists in replacing the content of the pixel information, the second consists in blending the pixel already here with the new one, which is called alpha blending. 

BGRABitmap functions propose 4 modes: 

  * dmSet : replaces the four bytes of the drawn pixel, transparency not handled
  * dmDrawWithTransparency : draws with alphablending and with gamma correction (see below)
  * dmFastBlend or dmLinearBlend : draws with alphablending but without gamma correction (faster but entails color distortions with low intensities).
  * dmXor : apply Xor to each component including alpha (if you want to invert color but keep alpha, use BGRA(255,255,255,0) )



## Integrated drawing functions

  * draw/erase pixels
  * draw a line with or without antialiasing
  * floating point coordinates
  * floating point pen width
  * rectangle (frame or fill)
  * ellipse and polygons with antialiasing
  * spline computation (rounded curve)
  * simple fill (Floodfill) or progressive fill
  * color gradient rendering (linear, radial...)
  * round rectangles
  * texts with transparency



## Drawing with the Canvas

It is possible to draw with a _Canvas_ object, with usual functions but without antialiasing. Opacity of drawing is defined by the _CanvasOpacity_ property. This way is slower because it needs image transformations. If you can, use CanvasBGRA instead, which allows transparency and antialiasing while having the same function names as with TCanvas. 

## Direct access to pixels

To access pixels, there are two properties, _Data_ and _Scanline_. The first gives a pointer to the first pixel of the image, and the second gives a pointer to the first pixel of a given line. 
    
    
    var 
      bmp: TBGRABitmap;
      p: PBGRAPixel;
      n: integer;
    
    begin
      bmp := TBGRABitmap.Create('image.png');
      p := bmp.Data;
      for n := bmp.NbPixels-1 downto 0 do
      begin
        p^.red := not p^.red; // invert red channel
        inc(p);
      end;
      bmp.InvalidateBitmap;   // note that we have accessed pixels directly
      bmp.Draw(Canvas, 0, 0, True);
      bmp.Free;
    end;
    

It is necessary to call the function _InvalidateBitmap_ to rebuild the image in a next call to _Draw_ for example. Notice that the line order can be reverse, depending on the _LineOrder_ property. 

See also the comparison between [direct pixel access methods](<Fast_direct_pixel_access.md> "Fast direct pixel access"). 

### InvalidateBitmap

Basically, BGRABitmap can store an image that may not be drawable as such, not a bitmap object of the system. In order to keep track of changes, the function InvalidateBitmap is called whenever pixel data is modified. So when the bitmap object is requested, the library knows that it needs to be rebuilt. 

To sum up, the only time a regular user would call this function is after modifying pixels pointed to by Data / ScanLine directly and before drawing the bitmap or requesting the bitmap object. 

## Image manipulation

Available filters (prefixed with Filter) : 

  * Radial blur : non directional blur
  * Motion blur : directional blur
  * Custom blur : blur according to a mask


  * Median : computes the median of colors around each pixel, which softens corners
  * Pixelate : simplifies the image with rectangles of the same color
  * Smooth : soften whole image, complementary to Sharpen
  * Sharpen : makes contours more accute, complementary to Smooth


  * Contour : draws contours on a white background (like a pencil drawing)
  * Emboss : draws contours with shadow
  * EmbossHighlight : draws contours of a selection defined with grayscale


  * Grayscale : converts colors to grayscale with gamma correction
  * Normalize : uses whole range of color luminosity


  * Rotate : rotation of the image around a point
  * Sphere : distorts the image to make it look like projected on a sphere
  * Twirl : distorts the image with a twirl effect
  * Cylinder : distorts the image to make it look like projected on a cylinder
  * Plane : computes a high precision projection on a horizontal plane. This is quite slow.
  * SmartZoom3 : resizes the image x3 and detects borders, to have a useful zoom with ancient games sprites



Some functions are not prefixed with Filter, because they do not return a newly allocated image. They modify the image in-place : 

  * VerticalFlip : flips the image vertically
  * HorizontalFlip : flips the image horizontally
  * Negative : inverse of colors
  * LinearNegative : inverse without gamma correction
  * SwapRedBlue : swap red and blue channels (to convert between BGRA and RGBA)
  * ConvertToLinearRGB : to convert from sRGB to RGB. Note the format used by BGRABitmap is sRGB when using dmDrawWithTransparency and RGB when using dmLinearBlend.
  * ConvertFromLinearRGB : convert from RGB to sRGB.



## Images combination

[PutImage](<https://bgrabitmap.github.io/doc/BGRABitmapTypes.TBGRACustomBitmap.html#PutImage-integer-integer-TBitmap-TDrawMode-byte->) is the basic image drawing function and [BlendImage](<https://bgrabitmap.github.io/doc/BGRABitmapTypes.TBGRACustomBitmap.html#BlendImage-integer-integer-TBGRACustomBitmap-TBlendOperation->) allows to combine images, like layers of image editing softwares. Available modes are the following: 

  * LinearBlend : simple superimposition without gamma correction (equivalent to dmFastBlend)
  * Transparent : superimposition with gamma correction
  * Multiply : multiplication of color values (with gamma correction)
  * LinearMultiply : multiplication of color values (without gamma correction)
  * Additive : addition of color values (with gamma correction)
  * LinearAdd : addition of color values (without gamma correction)
  * Difference : difference of color values (with gamma correction)
  * LinearDifference : difference of color values (without gamma correction)
  * Negation : makes common colors disappear (with gamma correction)
  * LinearNegation : makes common colors disappear (without gamma correction)
  * Reflect, Glow : for light effects
  * ColorBurn, ColorDodge, Overlay, Screen : misc. filters
  * Lighten : keeps the lightest color values
  * Darken : keeps the darkest color values
  * Xor : exclusive or of color values



These modes can be used in TBGRALayeredBitmap, which makes it easier because BlendImage only provides the basic blending operations. 

## Screenshots

[![Lazpaint contour.png](https://wiki.freepascal.org/images/5/5d/Lazpaint_contour.png)](</File:Lazpaint_contour.png>) [![Lazpaint curve redim.png](https://wiki.freepascal.org/images/2/25/Lazpaint_curve_redim.png)](</File:Lazpaint_curve_redim.png>) [![Bgra wirecube.png](https://wiki.freepascal.org/images/2/23/Bgra_wirecube.png)](</File:Bgra_wirecube.png>) [![Bgra chessboard.jpg](https://wiki.freepascal.org/images/b/b0/Bgra_chessboard.jpg)](</File:Bgra_chessboard.jpg>)

## Licence

modified LGPL 

Author: [Juliette ELSASS](<http://johann-elsass.net>) ([Facebook](<http://www.facebook.com/johann.elsass.1>)) 

## Download

Latest version: <https://github.com/bgrabitmap/bgrabitmap/releases>

Sourceforge with [LazPaint](<LazPaint.md> "LazPaint"): <http://sourceforge.net/projects/lazpaint/files/src/>

GitHub: <https://github.com/bgrabitmap/>

## See also

  * [Developing with Graphics](<Developing_with_Graphics.md> "Developing with Graphics")

---

_Source: [https://wiki.freepascal.org/BGRABitmap](https://web.archive.org/web/20250401023948/https://wiki.freepascal.org/BGRABitmap)_
