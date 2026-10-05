# LazPaint

│ **[Deutsch (de)](</LazPaint/de> "LazPaint/de")** │  **English (en)** │  **[español (es)](</LazPaint/es> "LazPaint/es")** │  **[suomi (fi)](</LazPaint/fi> "LazPaint/fi")** │  **[français (fr)](</LazPaint/fr> "LazPaint/fr")** │  **[русский (ru)](<../ru/LazPaint.md> "LazPaint/ru")** │ 

  
****

**LazPaint**

  


Free cross-platform image editor with raster and vector layers, written in Lazarus (Free Pascal).  
  
[Blog](<https://lazpaint.blogspot.com/>) [Last changes](<https://lazpaint.github.io/last/>) Documentation [Download](<https://lazpaint.github.io/>) [Screenshots](<http://lazpaint.blogspot.com/p/screenshots.html>) [Forum](<https://forum.lazarus.freepascal.org/index.php/board,46.0.html>) [f](<https://www.facebook.com/LazPaint>)

[Donate](<http://sourceforge.net/donate/index.php?group_id=404555>)   
**History** LazPaint was started to demonstrate the capabilities of the graphic library [BGRABitmap](<BGRABitmap.md> "BGRABitmap"). It provides advanced drawing functions in [Lazarus](<Lazarus.md> "Lazarus") development environment. Both provided a source of inspiration for the other and finally LazPaint became real image editor. Thanks to the help of Lazarus community, the program has been compiled on Windows, Linux and macOS.  |  |  **Features** [Files](<LazPaint_File.md> "LazPaint File"): read and write a variety of file formats, including layered bitmaps and 3D files. [Tools](<LazPaint_Tools.md> "LazPaint Tools"): many tools are available to draw on the layers. [Edit/Select](<LazPaint_Edit.md> "LazPaint Edit"): select parts of an image with antialiasing and modify the selection as a mask. [View](<LazPaint_Windows.md> "LazPaint Windows"): color window, layer stack window and toolbox window. [Command line](<LazPaint_Command_line.md> "LazPaint Command line"): call LazPaint from a console. [Scripts](<LazPaint_scripts.md> "LazPaint scripts"): scripts are provided to do layer effects. You can as well write your own [Python](<https://www.python.org/>) scripts.   
---|---|---  
  
## Contents

  * 1 Quick start
  * 2 Download
  * 3 Compilation of latest version
  * 4 Screenshots
    * 4.1 LazPaint on Linux (and dark theme)
    * 4.2 LazPaint on Windows
    * 4.3 LazPaint on Puppy Linux
  * 5 Interface
  * 6 Image manipulation
  * 7 Color manipulation
  * 8 Filters



### Quick start

**Useful keys**

  * Maintaining Space key down switches temporarily in move mode
  * F6 key hides/shows all tool windows
  * Ctrl key aligns to image pixels and limits possible angles with rotation tool
  * Backspace key erases last point in a polygon or last letter in a text
  * Enter key releases the selection

Right mouse button can be used to: 

  * Swap drawing colors temporarily
  * Subtract from selection (selection tool)
  * Define reference position (light position for shaded text and shapes, rotation center, clone tool source)
  * Finish a shape (polygon, curve)
  * Rotate a shape (clicking corners)

Left clicking in the small color preview will select the pen / background / outline color or if it is a gradient, the start / end color, to be edited with the color chooser.  |  |  **Video tutorials** Video tutorials are available on Youtube : 

  * [Getting started](<http://www.youtube.com/watch?v=9G6XcQdBtDo>)
  * [Advanced drawing tools](<http://www.youtube.com/watch?v=UEGQk1UhJ2w>)
  * [Using selection tools](<http://www.youtube.com/watch?v=6zWohQYLW3g>)
  * [Text with projected shadow](<http://www.youtube.com/watch?v=EmKzqKneJeI>)
  * [Drawing a vortex](<http://www.youtube.com/watch?v=9z6WgIaUYwQ>)
  * [Export for Krita](<http://www.youtube.com/watch?v=FFEZpl15Rb4>)
  * [Using layers, magic wand and perspective](<http://www.youtube.com/watch?v=Hxinfs6ziFc>)
  * [Draw a fire](<http://www.youtube.com/watch?v=yWgo0fcdOSg>) (_overlay_ blending mode)
  * [Draw lightnings](<http://www.youtube.com/watch?v=69cHd5W-Zyg>) (_negation_ blending mode)
  * [Draw a neon with blur](<http://www.youtube.com/watch?v=1rtHSPAUDnA>) (_lighten_ blending mode)
  * [Spline selection and glow effect](<http://www.youtube.com/watch?v=Lewg3iYkln8>) (_glow_ blending mode)
  * [Drop shadow](<https://www.youtube.com/watch?v=rsdPha36y50>) (example of layer effect)

  
---|---|---  
  
### Download

_Binaries can be downloaded on GitHub for Windows, Linux and macOS._   
**[Download](<https://lazpaint.github.io>)** **[Portable](<https://portableapps.com/apps/graphics_pictures/lazpaint-portable>)** [Support this project](<http://sourceforge.net/donate/index.php?group_id=404555>) [How to make a portable version](<LazPaint_Make_it_portable.md> "LazPaint Make it portable") |  |  **Source code** The source code can be downloaded on GitHub: LazPaint: <https://github.com/bgrabitmap/lazpaint/> BGRAControls: <https://github.com/bgrabitmap/bgracontrols/> BGRABitmap: <https://github.com/bgrabitmap/bgrabitmap/> Dependencies: 

  1. LazPaint depends on [BGRAControls](<BGRAControls.md> "BGRAControls") and [BGRABitmap](<BGRABitmap.md> "BGRABitmap").
  2. [BGRAControls](<BGRAControls.md> "BGRAControls") depends on [BGRABitmap](<BGRABitmap.md> "BGRABitmap").

You can download the source code and compile it for a specific platform. MacOS version is now fully functional.   
---|---|---  
  
### Compilation of latest version

  1. Get **Lazarus** as a package in your distribution or download it at <https://sourceforge.net/projects/lazarus/files/>
  2. Open **Lazarus** to check it works
  3. Download latest version of _BGRABitmap_ at <https://github.com/bgrabitmap/bgrabitmap/releases>
  4. Open the package file _BGRABitmapPack.lpk_ in **Lazarus**
  5. Download latest version of _BGRAControls_ at <https://github.com/bgrabitmap/bgracontrols/releases>
  6. Open the package file _bgracontrols.lpk_ in **Lazarus**
  7. Download _LazPaint_ at <https://github.com/bgrabitmap/lazpaint/releases>
  8. Open the package file _lazpaintcontrols.lpk_ in **Lazarus**
  9. Open the project file _lazpaint.lpi_ or _lazpaint.lpr_ in **Lazarus** (choose to Open project if prompted)
  10. Click the green triangle "Run" and check if the program is working
  11. Close _LazPaint_ and close **Lazarus** and go in the _release_ folder of the project



The essential files are: the executable file, the readme file, the _models_ , _i18n_ and _scripts_ folders. 

Additional files and instructions are in _release_ folder for each platform: 

  * _**windows**_ : run the batch file _stage.bat_ to generate a _lazpaint32_ and _lazpaint64_ folders that contains everything (it adds particular support for WebP and RAW images by adding _dcraw.exe_ and _libwebp.dll_).
  * _**macOS**_ : go in the folder with the terminal and call _./makedmg.sh_ to generate the bundle.
  * _**debian**_ : go in the folder with the terminal and call _./makedeb.sh_ to generate the DEB package and the TAR.GZ file.



Note: when debugging on Windows, you may want to have support for WebP and RAW images. You can do that by copying the _dcraw.exe_ and _libwebp.dll_ files next to the compiled files (for example _debug\i386-win32_ for 32-bit version). 

### Screenshots

#### LazPaint on Linux (and dark theme)

[![Lazpaint version 7.png](https://upload.wikimedia.org/wikipedia/commons/c/c0/Lazpaint_version_7.png)](</File:Lazpaint_version_7.png>)

#### LazPaint on Windows

[![Lazpaint curve redim.png](https://wiki.freepascal.org/images/2/25/Lazpaint_curve_redim.png)](</File:Lazpaint_curve_redim.png>)

#### LazPaint on Puppy Linux

[![lazpaint6 puppy.png](https://wiki.freepascal.org/images/5/57/lazpaint6_puppy.png)](</File:lazpaint6_puppy.png>)

### Interface

Many common actions can be done with the toolbar. Zoom can be changed with the magnifying glass (+ or -), or by clicking on the 1:1 button to show the image at its original size in pixels, or with the zoom fit button to set the zoom so that the whole image be within the window. 

It is possible to undo/redo the 200 last operations. If you have a doubt on what you are drawing, undo back to the beginning, save a copy, and redo the modifications before going further. 

### Image manipulation

An image can be resampled, flipped horizontally and vertically. 

Smart zoom x3 : resize the image x3 and detects borders; this provides a useful zoom with ancient games sprites. 

### Color manipulation

  * Colorize : set the color of an image while preserving intensities
  * Shift colors : cycle colors and change colorness (saturation)
  * Intensity : make colors lighter or darker without making them white
  * Lightness : make colors lighter or darker by making them whiter
  * Normalize : use the whole range of each color channel and alpha channel
  * Negative : invert colors (with gamma correction)
  * Linear negative : invert colors (without gamma correction)
  * Grayscale : converts colors to grayscale with gamma correction



### Filters

Filters can be applied to the whole image or to the active selection. 

  * Radial blur : non directional blur
  * Motion blur : directional blur
  * Custom blur : blur according to a mask


  * Sharpen : makes contours more accute, complementary to Smooth
  * Smooth : softens whole image, complementary to Sharpen
  * Median : computes the median of colors around each pixel, which softens corners


  * Contour : draws contours on a white background (like a pencil drawing)
  * Emboss : draws contours with shadow


  * Sphere : spherical projection
  * Cylinder : cylinder projection
  * Clouds : add clouds of the current pen color

---

_Source: [https://wiki.freepascal.org/LazPaint](https://web.archive.org/web/20250518090425/https://wiki.freepascal.org/LazPaint)_
