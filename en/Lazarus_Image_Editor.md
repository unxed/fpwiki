# Lazarus Image Editor

The **Lazarus Image Editor** is a raster image and icon editor for Windows, Linux, Mac OS X and all other desktop platforms supported by Lazarus. 

## Contents

  * 1 Recent screenshots
  * 2 Features
  * 3 Tools
    * 3.1 Authors
    * 3.2 License
    * 3.3 Download
    * 3.4 Notes
  * 4 Roadmap



## Recent screenshots

[![Icon Editor under Windows XP](https://wiki.freepascal.org/images/c/cc/Icon_Editor_win.png)](</File:Icon_Editor_win.png> "Icon Editor under Windows XP")

**Lazarus Icon Editor** is a simple icon editor with the ability to create lrs files. 

[Icon Editor Roadmap](<Icon_Editor_Roadmap.md> "Icon Editor Roadmap")

## Features

  * load and save in various graphics formats
  * save as lrs (Lazarus resource)
  * picture preview
  * multiple picture editing in notebook panels
  * palette
  * custom zoom
  * grid
  * pen, brush and paper color



## Tools

  * masks
  * eraser
  * flood fill
  * color picker
  * pen
  * line
  * rectangle / round rectangle
  * polygon
  * ellipse
  * spray



### Authors

  * [Tom Gregorovic](</User:Tombo> "User:Tombo")
  * [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
  * Yang JiXian



### License

GPL2 

### Download
    
    
    svn checkout <https://svn.code.sf.net/p/lazarus-ccr/svn/applications/lazimageeditor>
    

You can also download the whole SVN directory in a [compressed tarball archive](<http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/applications/lazimageeditor.tar.gz?view=tar>). 

You must also download and install: [LazRGBGraphics](<LazRGBGraphics.md> "LazRGBGraphics") (latest SVN), [ColorPalette](<ColorPalette.md> "ColorPalette"). 

### Notes

Status: Beta 

## Roadmap

x - completely implemented, / - partially implemented 

Feature | Type | Target | Implemented | Remark   
---|---|---|---|---  
Tool icons | Design | 0.1 | x |   
Lazarus Icon Editor icon | Design | 0.1 | / |   
Command icons | Design | 0.1 | x |   
About Lazarus Icon Editor image | Design | 0.1 | x |   
About Lazarus Icon Editor | Docs | 0.1 | x |   
Info on the web | Docs | 0.1 |  |   
Load/save bmp, xpm, png | Formats | 0.1 | x | ico, xpm and png transparency   
Export as *.lrs | Formats | 0.1 | x | can add to existing lrs?   
Picture preview | GUI | 0.1 | x |   
Show grid | GUI | 0.1 | x |   
Editing multiple pictures in notebook | GUI | 0.1 | x |   
Palette | GUI | 0.1 | x |   
Pen, brush, paper color | GUI | 0.1 | x |   
Mouse pos | GUI | 0.1 | x |   
Mouse delta | GUI | 0.1 | x |   
Color under mouse | GUI | 0.1 | x |   
Enable/disable commands | GUI | 0.1 | x |   
Fix TScrollBox bugs | LCL | 0.1 |  | GTK + Win32   
Fix mouse capture bug | LCL | 0.1 |  | GTK   
Resize truncate | Picture | 0.1 | x |   
Resize paper | Picture | 0.1 | x |   
Flip horizontally/vertiacally | Picture | 0.1 | x |   
Rotate 90°/180°/270° | Picture | 0.1 | x |   
Colors invert | Picture | 0.1 | x |   
Colors disable | Picture | 0.1 | x |   
Colors grayscale | Picture | 0.1 | x |   
Clip paper to mask | Picture | 0.1 | x |   
Invert/remove mask | Picture | 0.1 | x |   
Cut/copy/delete/select all | Picture | 0.1 | x |   
Public release | Release | 0.1 |  |   
Color picker | Tools | 0.1 | x |   
Pen | Tools | 0.1 | x |   
Eraser/replacer | Tools | 0.1 | x |   
Flood fill | Tools | 0.1 | x |   
Spray | Tools | 0.1 | x |   
Line | Tools | 0.1 | x |   
Rectangle/round rectangle | Tools | 0.1 | x |   
Ellipse | Tools | 0.1 | x |   
Drawing shape while dragging | Tools | 0.1 | x |   
Mask | Tools | 0.1 | x |   
Own color dialog | GUI | 0.2 |  |   
Localization support | GUI | 0.2 |  |   
Doublebuffering on GTK | LCL | 0.2 |  |   
Undo/redo | Picture | 0.2 |  |   
Apply spray/eraser while dragging | Tools | 0.2 |  |   
Polygon | Tools | 0.2 |  |   
Objects | Picture | 0.3 |  |   
Paste | Picture | 0.3 |  |   
Help topics | Docs | 1.0 |  |   
Save ico | Formats | 1? |  |   
Load/save gif | Formats | 1? |  |   
Custom zoom | GUI | 1? |  |   
Palette modification | GUI | 1? |  |   
Printing | GUI | 1? |  |   
Custom rotate | Picture | 1? |  |   
Resize smooth | Picture | 1? |  |   
Blur | Picture | 1? |  |   
Noise | Picture | 1? |  |   
Find edges | Picture | 1? |  |   
Thick lines | Tools | 1? |  |   
Antialiasing | Tools | 1? |  |   
Text | Tools | 1? |  |   
Alpha blending | Tools | 1? |  |

---

_Source: [https://wiki.freepascal.org/Lazarus_Image_Editor](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/Lazarus_Image_Editor)_
