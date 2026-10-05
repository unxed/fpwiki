# LazPaint Tools

│ **English (en)** │  **[suomi (fi)](</LazPaint_Tools/fi> "LazPaint Tools/fi")** │    
****

## Contents

  * 1 Hand
  * 2 Color picker
  * 3 Pen
  * 4 Brush
  * 5 Eraser
  * 6 Clone tool



### Hand

**Action:** move and scroll picture.  
**How to use it:** click and hold the button while you move the mouse. 

This tool does not modify the content of the picture. It only changes how it is displayed. 

The canvas is the area where it is possible to draw. When it is empty, it is displayed as a checkerboard pattern. 

It can be smaller, equal, or greater than the blue area of the main window. 

If the canvas is smaller than the blue area, then it can be moved around. 

If the canvas is larger, then it can be scrolled. 

### Color picker

**Action:** retrieves a color of the picture.  
**How to use it:** hover a pixel and click to get its color. 

When doing a left click, the color of the pixel is stored in the pen color. When doing a right click, it is stored in the back color. Those displayed at the top of the window, in the middle of the toolbar. It is also displayed in the [color window](<LazPaint_Windows.md> "LazPaint Windows"), if this window is visible. 

### Pen

**Action:** draws a stroke with a circular shaped pen.  
**How to use it:** click and hold the button while you move the mouse.  
**Modifier:** hold CTRL key to snap to pixels. 

This tool modifies the content of the picture. To undo, press CTRL-Z or use the menu Edit > Undo. 

When using the left click, the pen color is used. When using the right click, the back color is used instead. If a texture is selected, then it is used instead of the pen and back colors. 

To choose a texture, use the texture toolbar. It contains an icon "no tex" if no texture is loaded, and otherwise shows a thumbnail of the texture. Clicking on the thumbnail allows you to edit the texture. 

Pen width can be modified in a toolbar as well. You can set the pen width either by using the up/down control, or by hovering over the pen width toolbar, which shows a preview of the pen and two icons (+) and (-) that can be clicked. The up/down arrows allows a fine tuning of the width, while the (+) and (-) changes the value faster. 

### Brush

**Action:** draws a stroke with a customisable brush.  
**How to use it:** click and hold the button while you move the mouse 

The brush is similar to the pen in most respects, however the shape can be customised and contain a gradiant of opacity. When selecting the brush tool, an additional toolbar appears to specify the shape of the brush. 

The first button pops up a window to create a brush that has a regular polygonal shape. 

A brush can also be loaded from a file. Such a file can be any format of image. White color means that it is transparent, and black means that it is opaque. Gray indicates shades in between. So a brush is a picture of the brush if it is drawn with a black color on a white background. 

A combobox allows to select among loaded brushes. The number of brushes is limited, so keep a directory with your brushes, and load them when you need them. The list of brushes is stored in the configuration, so that if you quit and reopen the programs, the list will be kept. 

The spacing parameter specifies the distance between each individual brush pattern along the stroke. 

### Eraser

**Action:** erases or blurs a part of the image.  
**How to use it:** click and hold the button while you move the mouse.  
**Modifier:** hold CTRL key to snap to pixels. 

The eraser has a width, that specifies the size of the area to be eraser. 

In the eraser toolbar, the opacity parameter specifies how much transparency is applied. 

It is also possible to choose between erasing and softening. When erasing, the content is made transparent and removed from the picture. When softening, the content is blurred so that it slowly becomes one color. 

### Clone tool

**Action:** copies an area to another area.  
**How to use it:** RIGHT click on the source and LEFT click on the destination. 

The clone tool is similar to a brush, except that the content that is drawn comes from the image itself. The source can be located in another layer. So the full procedure is: 

\- select the source layer  
\- RIGHT click on the location in the layer that you want to copy  
\- select the destination layer (it may be the same layer or another layer)  
\- LEFT click and hold to draw a copy of the source

---

_Source: [https://wiki.freepascal.org/LazPaint_Tools](https://web.archive.org/web/20250403182810/https://wiki.freepascal.org/LazPaint_Tools)_
