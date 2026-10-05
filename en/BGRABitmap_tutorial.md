# BGRABitmap tutorial

│ **English (en)** │  **[русский (ru)](<../ru/BGRABitmap_tutorial.md>)** │

**Home** | [ **Tutorial 1**](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [ **Tutorial 2**](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") | [ **Tutorial 3**](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") | [ **Tutorial 4**](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") | [ **Tutorial 5**](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") | [ **Tutorial 6**](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") | [ **Tutorial 7**](<BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7") | [ **Tutorial 8**](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") | [ **Tutorial 10**](<BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") | [ **Tutorial 11**](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") | [ **Tutorial 12**](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") | [ **Tutorial 13**](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") | [ **Tutorial 14**](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") | [ **Tutorial 15**](<BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15") | [ **Tutorial 16**](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | Edit

Welcome to the index of the tutorials for the [BGRABitmap](<BGRABitmap.md> "BGRABitmap") library. You can browse tutorials by number with the bar on the top, or by the following categories: 

## Contents

  * 1 Install BGRABitmap and draw basic shapes
  * 2 Textures and scanners
  * 3 Other drawing contexts
  * 4 More



### Install BGRABitmap and draw basic shapes

TBGRABitmap images have drawing functions using floating point coordinates or integer coordinates. 

  * [Installing BGRABitmap (No. 1)](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1")
  * [Loading and displaying an image (No. 2)](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2")
  * [Drawing with the mouse (No. 3)](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3")
  * [Line styles (No. 6)](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6")
  * [Splines and Bézier curves (No. 7)](<BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7")
  * [Text functions (No. 12)](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12")
  * [Integer coordinates and floating point coordinates (No. 13)](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13")



### Textures and scanners

Pixels are a table in memory containing values in the TBGRAPixel format. At this level, we can do various operations: 

  * [Direct pixel access with Scanline (No. 4)](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4")
  * [Combining layers of pixels (No. 5)](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5")
  * [Generating textures (No. 8)](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8")
  * [Phong shading using textures (No. 9)](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9")
  * [Texture mapping (No. 10)](<BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10")
  * [Using scanners to combine transformations (No. 11)](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11")



### Other drawing contexts

It is possible to have other contexts, that provide/allow other basic drawing functions: 

  * Standard Canvas (Canvas and CanvasOpacity properties) : avoid using it because of the slowness of conversions of bitmap data
  * Canvas with features brought by BGRABitmap (CanvasBGRA property, Brush and Pen have an Opacity property) 
    * [How to convert your application from TCanvas to CanvasBGRA (video)](<http://www.youtube.com/watch?v=HGYSLgtYx-U>)
  * [Drawing with a 2D canvas with affine transformations (No. 14)](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14")
  * [Real 3D rendering (No. 15)](<BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15")
  * [Using textures with 3D rendering (No. 16)](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16")



### More

You can use BGRABitmap to [improve TAChart rendering](<BGRABitmap_tutorial_TAChart.md> "BGRABitmap tutorial TAChart"). 

You can use it as well to display icons with the [SVG Image List](<SVG_Image_List.md> "SVG Image List"). 

More classes are available (you need to create them when you need them): 

  * TBGRATextEffect, in unit BGRATextFX, allows to prepare the drawing of text line, to add effects like contour and shadow.
  * TBGRALayeredBitmap, in unit BGRALayers, allow to create a multi-layered bitmap. Units BGRAPaintNet and BGRAOpenRaster contain implementations to read and write in Paint.NET format (read only) and OpenRaster format (read and write).
  * Units BGRAGradientScanner and BGRATransform contain scanners to do various effects.
  * Unit BGRAGradients contain procedures to generate gradients and TPhongShading class for Phong shading.
  * TBGRACompressableBitmap, in unit BGRACompressableBitmap, allow to store and compress images.



Other units contient low level functions, and you should not need to use them for a normal usage.

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial](https://web.archive.org/web/20240121085316/https://wiki.freepascal.org/BGRABitmap_tutorial)_
