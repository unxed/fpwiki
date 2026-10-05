# Developing with Graphics

│ **English (en)** │  **[русский (ru)](<../ru/Developing_with_Graphics.md>)** │

This page describes the basic classes and techniques regarding drawing graphics with Lazarus. Other more specific topics are in separate articles. 

## Contents

  * 1 Libraries
  * 2 Introduction to the Graphics model of the LCL
  * 3 Drawing shapes
    * 3.1 Drawing a rectangle
    * 3.2 Drawing a circle
    * 3.3 Drawing a polygon
      * 3.3.1 Simple polygon
      * 3.3.2 Self-overlapping polygons
      * 3.3.3 Polygon with a hole
      * 3.3.4 Polygon with several holes
  * 4 Drawing Text
    * 4.1 Text painting methods
    * 4.2 Using the default GUI font
    * 4.3 Drawing text to an exactly fitting width
    * 4.4 Drawing text with sharp edges (non antialiased)
  * 5 Working with TBitmap and other TGraphic descendents
    * 5.1 Loading/Saving an image from/to the disk
    * 5.2 Additional file formats for TImage
    * 5.3 Indirect pixel access
    * 5.4 Direct pixel access
    * 5.5 Drawing color transparent bitmaps
    * 5.6 Drawing alpha-channel transparent bitmaps
    * 5.7 Taking a screenshot of the screen
  * 6 Working with TLazIntfImage, TRawImage and TLazCanvas
    * 6.1 Initializing a TLazIntfImage
    * 6.2 TLazIntfImage.LoadFromFile
    * 6.3 Loading a TLazIntfImage into a TImage
    * 6.4 Creating and drawing a transparent bitmap for a TImage
    * 6.5 Fading example
    * 6.6 Image format specific example
    * 6.7 Conversion between TLazIntfImage and TBitmap
    * 6.8 Using the non-native StretchDraw from LazCanvas
    * 6.9 Clipping
      * 6.9.1 Clipping by a rectangle
      * 6.9.2 Clipping by a region
  * 7 Motion Graphics - How to Avoid flickering
    * 7.1 Draw to a TImage
      * 7.1.1 Resizing the bitmap of a TImage
      * 7.1.2 Painting on the bitmap of a TImage
      * 7.1.3 Painting on the volatile visual area of the TImage
    * 7.2 Draw on the OnPaint event
    * 7.3 Create a custom control which draws itself
  * 8 Image formats
    * 8.1 Converting formats
  * 9 Pixel Formats
    * 9.1 TColor
    * 9.2 TFPColor
  * 10 Drawing with fcl-image
  * 11 Common OnPaint Error
  * 12 Some useful examples
    * 12.1 Example 1: Drawing on loaded JPEG with TImage
    * 12.2 Example 2: Drawing on controls of Form
  * 13 See also
    * 13.1 2D drawing
    * 13.2 3D drawing
    * 13.3 Charts



## Libraries

[Graphics libraries](<Graphics_libraries.md> "Graphics libraries") \- here you can see the main graphic libraries you can use to develop. 

## Introduction to the Graphics model of the LCL

The Lazarus Component Library (LCL) provides two kinds of drawing class: Native classes and non-native classes. Native graphics classes are the most traditional way of drawing graphics in the LCL and are also the most important one, while the non-native classes are complementary, but also very important. The native classes are mostly located in the unit **Graphics** of the LCL. These classes are: [TBitmap](</index.php?title=TBitmap&action=edit&redlink=1> "TBitmap \(page does not exist\)"), [TCanvas](<TCanvas.md> "TCanvas"), [TFont](</index.php?title=TFont&action=edit&redlink=1> "TFont \(page does not exist\)"), [TBrush](<TBrush.md> "TBrush"), [TPen](</index.php?title=TPen&action=edit&redlink=1> "TPen \(page does not exist\)"), [TPortableNetworkGraphic](</index.php?title=TPortableNetworkGraphic&action=edit&redlink=1> "TPortableNetworkGraphic \(page does not exist\)"), etc. 

TCanvas is a class capable of executing drawings. It cannot exist alone and must either be attached to something visible (or at least which may possibly be visible), such as a visual control descending from TControl, or be attached to an off-screen buffer from a TRasterImage descendent (TBitmap is the most commonly used). TFont, TBrush and TPen describe how the drawing of various operations will be executed in the Canvas. 

TRasterImage (usually used via its descendant TBitmap) is a memory area reserved for drawing graphics, but it is created for maximum compatibility with the native Canvas and therefore in LCL-Gtk2 in X11 it is located in the X11 server, which makes pixel access via the Pixels property extremely slow. In Windows it is very fast because Windows allows creating a locally allocated image which can receive drawings from a Windows Canvas. 

Besides these there are also non-native drawing classes located in the units: 

  * **graphtype** : [TRawImage](</index.php?title=TRawImage&action=edit&redlink=1> "TRawImage \(page does not exist\)") is the storage and description of a memory area which contains an image.
  * **intfgraphics** : [TLazIntfImage](</index.php?title=TLazIntfImage&action=edit&redlink=1> "TLazIntfImage \(page does not exist\)") is an image which attaches itself to a TRawImage and takes care of converting between TFPColor and the real pixel format of the TRawImage.
  * **lazcanvas** : [TLazCanvas](</index.php?title=TLazCanvas&action=edit&redlink=1> "TLazCanvas \(page does not exist\)") is a non-native Canvas which can draw to an image in a TLazIntfImage.



The main difference between the native classes and the non-native ones is that the native ones do not perform exactly the same in all platforms, because the drawing is done by the underlying platform itself. The speed and also the exact final result of the image drawing can have differences. The non-native classes are guaranteed to perform exactly the same drawing in all platforms with a pixel level precision and they all perform reasonably fast in all platforms. 

In the widgetset LCL-CustomDrawn the native classes are implemented using the non-native ones. 

All of these classes will be better described in the sections below. 

  


## Drawing shapes

### Drawing a rectangle

Many controls expose their canvas as a public Canvas property (or via an OnPaint event). Such controls include [TForm](<TForm.md> "TForm"), [TPanel](<TPanel.md> "TPanel") and [TPaintBox](<TPaintBox.md> "TPaintBox"). Let's use TForm as an example to demonstrate how to paint on a canvas. 

Suppose we want to draw a red rectangle with a 5-pixel-thick blue border in the center of the form, and the the rectangle should be half the size of the form. For this purpose we must add code to the OnPaint event of the form. Never paint in an OnClick handler, because this painting is not persistent and will be erased whenever the operating system requests a repaint. Always paint in the OnPaint event! 

The TCanvas method for painting a rectangle is named very logically: `Rectangle()`. You can pass rectangle's edge coordinates to the method either as four separate x/y values, or as a single `TRect` record. The fill color is determined by the color of the canvas's Brush, and the border color is given by the color of the canvas's Pen: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      w, h: Integer;    // Width and height of the rectangle
      cx, cy: Integer;  // center of the form
      R: TRect;         // record containing the coordinates of the rectangle's left, top, right, bottom corners
    begin
      // Calculate form center
      cx := Width div 2;
      cy := Height div 2;
    
      // Calculate the size of the rectangle
      w := Width div 2;
      h := Height div 2;
    
      // Calculate the corner points of the rectangle
      R.Left := cx - w div 2;
      R.Top := cy - h div 2;
      R.Right := cx + w div 2;
      R.Bottom := cy + h div 2;
    
      // Set the fill color
      Canvas.Brush.Color := clRed;
      Canvas.Brush.Style := bsSolid;
    
      // Set the border color
      Canvas.Pen.Color := clBlue;
      Canvas.Pen.Width := 5;
      Canvas.Pen.Style := psSolid;
    
      // Draw the rectangle
      Canvas.Rectangle(R);
    end;
    

### Drawing a circle

The canvas does not have a direct method to draw a circle. But there is a method to draw an ellipse. Knowing that a circle is a special case of an ellipse with equal half-axes we can draw a circle as follows: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      radius: Integer;   // Radius of the circle
      center: TPoint;    // Center point of the circle
      R: TRect;          // Rectangle enclosing the circle
    begin
      // Set the fill color
      Canvas.Brush.Color := clYellow;
      Canvas.Brush.Style := bsSolid;
    
      // Set the border color
      Canvas.Pen.Color := clBlue;
      Canvas.Pen.Width := 3;
      Canvas.Pen.Style := psSolid;
    
      // We want the circle to be centered in the form
      center.X := Width div 2;
      center.Y := Height div 2;
    
      // The diameter should be 90% of the width or the height, whichever is smaller.
      // The radius, then, is half of this value.
      if Width > Height then
        radius := round(Height * 0.45)
      else
        radius := round(Width * 0.45);
    
      // The circle then will be enclosed by the rectangle between center.X +/- radius
      // and center.Y +/- radius
      R := Rect(center.X - radius, center.Y - radius, center.X + radius, center.Y + radius);
    
      // Draw the circle
      Canvas.Ellipse(R);
    end;
    

### Drawing a polygon

#### Simple polygon

A polygon is drawn by the `Polygon` method of the canvas. The polygon is defined by an array of points (`TPoint`) which are connected by straight lines drawn with the current `Pen`, and the inner area is filled by the current `Brush`. The polygon is closed automatically, i.e. the last array point does not necessarily need to coincide with the first point (although there are cases where this is required -- see below). 

[![Simple Polygon: Pentagon](https://wiki.freepascal.org/images/2/25/Simple_Polygon.png)](</File:Simple_Polygon.png> "Simple Polygon: Pentagon")

**Example:** Pentagon 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      P: Array[0..4] of TPoint;
      i: Integer;
      phi: Double;
    
    begin
      for i := 0 to 4 do
      begin
        phi := 2.0 * pi / 5 * i + pi * 0.5;;
        P[i].X := round(100 * cos(phi) + 110);
        P[i].Y := round(100 * sin(phi) + 110);
      end;
      Canvas.Brush.Color := clRed;
      Canvas.Polygon(P); 
    end;
    

#### Self-overlapping polygons

Here is a modification of the polygon example: Let's rearrange the polygon points so that the first point is connected to the 3rd initial point, the 3rd point is connected to the 5th point, the 5th point to the 2nd point and the 2nd point to to 4th point. This is a self-overlapping polygon and results in a star-shape. However, owing to the overlapping, different effects can be obtained which depend on the optional `Winding` parameter of the `Polygon()` method. When `Winding` is `False` an area is filled by the "even-odd rule" (<https://en.wikipedia.org/wiki/Even%E2%80%93odd_rule>), otherwise by the "non-zero winding rule" (<https://en.wikipedia.org/wiki/Nonzero-rule>). The following code example compares both cases: 

[![Self-overlapping Polygon](https://wiki.freepascal.org/images/2/25/Self-overlapping_Polygon.png)](</File:Self-overlapping_Polygon.png> "Self-overlapping Polygon")
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      P: Array[0..4] of TPoint;
      P1, P2: Array[0..4] of TPoint;
      i: Integer;
      phi: Double;
    begin
      for i := 0 to 4 do
      begin
        phi := 2.0 * pi / 5 * i + pi * 0.5;;
        P[i].X := round(100 * cos(phi) + 110);
        P[i].Y := round(100 * sin(phi) + 110);
      end;
      P1[0] := P[0];
      P1[1] := P[2];
      P1[2] := P[4];
      P1[3] := P[1];
      P1[4] := P[3];
      for i:= 0 to 4 do P2[i] := Point(P1[i].X + 200, P1[i].Y);  // offset polygon
      Canvas.Brush.Color := clRed;
      Canvas.Polygon(P1, false);  // false --> Even-odd rule
      Canvas.Polygon(P2, true);   // true ---> Non-zero winding rule
    end;
    

#### Polygon with a hole

Suppose you want to draw the shape of a country with a large lake inside from both of which you have some boundary points. Basically the `Polygon()` method of the LCL canvas is ready for this task. However, you need to consider several important points: 

  * You must prepare the array of polygon vertices such that each polygon is closed (i.e. last point = first point), and that both first and last polygon points are immediately adjacent in the array.
  * The order of the inner and outer polygon points in the array does not matter.
  * Make sure that both polygons have opposite orientations, i.e. if the outer polygon has its vertices in **clockwise** order, then the inner polygon must have the points in **counter-clockwise** order.



[![Polygon with hole](https://wiki.freepascal.org/images/b/b2/Polygon_with_hole_1.png)](</File:Polygon_with_hole_1.png> "Polygon with hole")

Example: 
    
    
    const
      P: array of [0..8] of TPoint = (
        // outer polygon: a rectangle 
        (X: 10; Y: 10),   // <--- first point of the rectangle
        (X:190; Y: 10),
        (X:190; Y:190),   //      (clockwise orientation)
        (X: 10; Y:190),
        (X: 10; Y: 10),   // <--- last point of the rectangle = first point
    
        // inner polygon: a triangle
        (X: 20; Y: 20),   // <--- first point of the triangle
        (X: 40; Y:180),   //      ( counter-clockwise orientation) 
        (X: 60; Y: 20),
        (X: 20; Y: 20)    // <--- last point of the triangle = first point
      );
    
    procedure TForm1.FormPaint(Sender: TObject);
    begin
      Canvas.Brush.Color := clRed;
      Canvas.Polygon(Pts);
    end;
    

You may notice that there is a line connecting the starting point of the inner triangle back to the starting point of the outer rectangle (marked by a blue circle in the screenshot). This is because the `Polygon()` method closes the entire polygon, i.e. it connects the very first with the very last array point. You can avoid this by drawing the polygon and the border separately. To draw the fill the `Pen.Style` should be set to `psClear` to hide the outline. The `PolyLine()` method can be used to draw the border; this method needs arguments for the starting point index and also a count of the array points to be drawn. 

[![Polygon with hole](https://wiki.freepascal.org/images/1/13/Polygon_with_hole_2.png)](</File:Polygon_with_hole_2.png> "Polygon with hole")
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    begin
      Canvas.Brush.Color := clRed;
      Canvas.Pen.Style := psClear;
      Canvas.Polygon(Pts);
    
      Canvas.Pen.Style := psSolid;
      Canvas.Pen.Color := clBlack;
      Canvas.Polyline(Pts, 0, 5);  // rectangle starts at index 0 and consists of 5 array elements
      Canvas.Polyline(Pts, 5, 4);  // triangle starts at index 5 and consists of 4 array elements
    end;
    

#### Polygon with several holes

Applying the rules for the single hole in a polygon, we extend the example from the previous section by adding two more triangles inside the outer rectangle. These triangles have the same orientation as the first triangle, opposite to the outer rectangle, and thus should be considered to be holes. 

[![Polygon with holes](https://wiki.freepascal.org/images/9/90/Polygon_with_holes_1.png)](</File:Polygon_with_holes_1.png> "Polygon with holes")
    
    
    const
      Pts: array[0..16] of TPoint = (
        // outer polygon: a rectangle
        (X: 10; Y: 10),    // clockwise
        (X:190; Y: 10),
        (X:190; Y:190),
        (X: 10; Y:190),
        (X: 10; Y: 10),
    
        // inner polygon: a triangle
        (X:  20; Y: 20),   // counter-clockwise
        (X:  80; Y:180),
        (X: 140; Y: 20),
        (X:  20; Y: 20),
    
        // 2nd inner triangle
        (X: 150; Y: 50),   // counter-clockwise
        (X: 150; Y:100),
        (X: 180; Y: 50),
        (X: 150; Y: 50),
    
        // 3rd inner triangle
        (X: 180; Y: 80),  // counter-clockwise
        (X: 160; Y:120),
        (X: 180; Y:120),
        (X: 180; Y: 80)
      );
    

Rendering this by a simple `Polygon()` fill is disappointing because there are new additional areas with are not expected. The reason is that this model does not return to the starting point correctly. The trick is to add two further points (one per shape). These are added to the above single-hole-in-polygon case: the first additional point duplicates the first point of the 2nd inner triangle, and the second additional point duplicates the first point of the 1st inner triangle. By so doing, the polygon is closed along the imaginary path the holes were connected by initially, and no additional areas are introduced: 

[![Polygon with holes: no additional points](https://wiki.freepascal.org/images/7/79/Polygon_with_holes_2.png)](</File:Polygon_with_holes_2.png> "Polygon with holes: no additional points")

[![Polygon with holes: with additional points](https://wiki.freepascal.org/images/f/f9/Polygon_with_holes_3.png)](</File:Polygon_with_holes_3.png> "Polygon with holes: with additional points")
    
    
    const
      Pts: array[0..18] of TPoint = (
        // outer polygon: a rectangle
        (X: 10; Y: 10),    // clockwise
        (X:190; Y: 10),
        (X:190; Y:190),
        (X: 10; Y:190),
        (X: 10; Y: 10),
    
        // 1st inner triangle
        (X:  20; Y: 20),   // counter-clockwise --> hole
        (X:  80; Y:180),
        (X: 140; Y: 20),
        (X:  20; Y: 20),
    
        // 2nd inner triangle
        (X: 150; Y: 50),   // counter-clockwise --> hole
        (X: 150; Y:100),
        (X: 180; Y: 50),
        (X: 150; Y: 50),
    
        // 3rd inner triangle
        (X: 180; Y: 80),  // counter-clockwise --> hole
        (X: 160; Y:120),
        (X: 180; Y:120),
        (X: 180; Y: 80),
    
        (X: 150; Y: 50),  // duplicates 1st point of 2nd inner triangle
        (X:  20; Y: 20)   // duplicates 1st point of 1st inner triangle
      );
    

The last image at the right is drawn again with separate `Polygon()` and `PolyLine()` calls. 

## Drawing Text

### Text painting methods

There are two basic methods how to draw text by means of Canvas methods: 
    
    
    TCanvas.TextOut(x, y: Integer; const AText: String)
    

This is the simplest way to draw the given text. Its top/left corner is at the position x/y. 
    
    
    TCanvas.TextRect(R: TRect; x, y: Integer; const AText: String; const Style: TTextStyle)
    

The (optional) `TTextStyle` parameter allows to apply various options to control the text output: 

  * `Alignment: TAlignment = (taLeftJustify, taRightJustify, taCenter)`: horizontal alignment of the text within the rectangle given as parameter R. When `Alignment` is `taLeftJustify` the text begins at <x> (measured relative to the canvas); otherwise the text is centered or right-aligned in the rectangle (x is ignored now).
  * `Layout: TTextLayout = (tlTop, tlCenter, tlBottom)`: Analogous to `Alignment`, but for the vertical direction.
  * `SingleLine: Boolean`: If WordBreak is false then process #13, #10 as standard chars and perform no line breaking.
  * `Wordbreak: boolean`: If line of text is too long to fit between left and right boundaries, it is attempted to break the text between words into multiple lines. See also `EndEllipsis`.
  * `EndEllipsis: Boolean`: If line of text is too long to fit between left and right boundaries, the text is truncated the text and an ellipsis (`'...'`) is added. If `Wordbreak` is set as well, `Workbreak` will dominate.
  * `Clipping: boolean`: Clips the text to the passed rectangle.
  * `ExpandTabs: boolean`: Replaces #9 by appropriate amount of spaces (default is usually 8).
  * `ShowPrefix: boolean`: Processes the first single '&' per line as an underscore and draws '&&' as '&'.
  * `Opaque: boolean`: Fills background with the current Brush
  * `SystemFont: Boolean`: Uses the system font instead of Canvas Font
  * `RightToLeft: Boolean`: For RightToLeft text reading (Text Direction)



Alternatively there is also the Windows-like `DrawText` function. Although it follows Windows syntax the procedure in unit `LCLIntf` is cross-platform. 
    
    
    function DrawText(DC: HDC; Str: PChar; Count: Integer; var Rect: TRect; Flags: Cardinal): Integer
    

  * `DC`: Handle of the canvas, e.g. `Paintbox1.Canvas.Handle`
  * `Str`: Text to be written, cast to `PChar`
  * `Count`: Number of bytes to be sent to the `DrawText` function, use `Length(Str)`
  * `Rect`: Rectangle within the output should occur. Returns the smallest rectangle occupied by the text.
  * `Flags`: Can contain a long list of values combined by logical "or" representing options to control the output. Among them `DT_LEFT, DT_CENTER, DT_RIGHT` for horizontal, `DT_TOP, DT_VCENTER, DT_BOTTOM` for vertical alignment, or `DT_SINGLELINE, DT_WORDBREAK, DT_END_ELLIPSIS` to control multiline behaviour. An important option is `DT_CALCRECT` which suppresses painting but returns the size needed for the text output in the `TRect` parameter. See <https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-drawtext> for a complete list.



### Using the default GUI font

This can be done with the following simple code: 
    
    
    SelectObject(Canvas.Handle, GetStockObject(DEFAULT_GUI_FONT));
    

or: 
    
    
    Canvas.Font.Name := 'default';
    

### Drawing text to an exactly fitting width

Use the DrawText routine, first with DT_CALCRECT and then without it. 
    
    
    // First calculate the text size then draw it
    TextBox := Rect(0, currentPos.Y, Width, High(Integer));
    DrawText(ACanvas.Handle, PChar(Text), Length(Text),
      TextBox, DT_WORDBREAK or DT_INTERNAL or DT_CALCRECT);
    
    DrawText(ACanvas.Handle, PChar(Text), Length(Text),
      TextBox, DT_WORDBREAK or DT_INTERNAL);
    

### Drawing text with sharp edges (non antialiased)

Some widgetsets support this via 
    
    
    Canvas.Font.Quality := fqNonAntialiased;
    

Some widgetsets like the gtk2 do not support this and always paint antialiased. Here is a simple procedure to draw text with sharp edges under gtk2. It does not consider all cases, but it should give an idea: 
    
    
    procedure PaintAliased(Canvas: TCanvas; x, y: integer; const TheText: string);
    var
      w, h, dx, dy: Integer;
      IntfImg:      TLazIntfImage;
      Img:          TBitmap;
      col:          TFPColor;
      FontColor, c: TColor;
    begin
      w := 0;
      h := 0;
      Canvas.GetTextSize(TheText, w, h);
      if (w <= 0) or (h <= 0) then exit;
      Img := TBitmap.Create;
      IntfImg := nil;
      try
        // paint text to a bitmap
        Img.Masked := true;
        Img.SetSize(w,h);
        Img.Canvas.Brush.Style := bsSolid;
        Img.Canvas.Brush.Color := clWhite;
        Img.Canvas.FillRect(0, 0, w, h);
        Img.Canvas.Font := Canvas.Font;
        Img.Canvas.TextOut(0, 0, TheText);
        // get memory image
        IntfImg := Img.CreateIntfImage;
        // replace gray pixels
        FontColor := ColorToRGB(Canvas.Font.Color);
        for dy := 0 to h - 1 do begin
          for dx := 0 to w - 1 do begin
            col := IntfImg.Colors[dx, dy];
            c := FPColorToTColor(col);
            if c <> FontColor then
              IntfImg.Colors[dx, dy] := colTransparent;
          end;
        end;
        // create bitmap
        Img.LoadFromIntfImage(IntfImg);
        // paint
        Canvas.Draw(x, y, Img);
      finally
        IntfImg.Free;
        Img.Free;
      end;
    end;
    

## Working with TBitmap and other TGraphic descendents

The TBitmap object stores a bitmap where you can draw before showing it to the screen. When you create a bitmap, you must specify the height and width, otherwise it will be zero and nothing will be drawn. And in general all other TRasterImage descendents provide the same capabilities. One should use the one which matches the format desired for output/input from the disk or TBitmap in case disk operations will not be performed as well as for the Windows Bitmap (*.bmp) format. 

### Loading/Saving an image from/to the disk

To load an image from the disk use [TGraphic.LoadFromFile](<http://lazarus-ccr.sourceforge.net/docs/lcl/graphics/tgraphic.loadfromfile.html>) and to save it to another disk file use [TGraphic.SaveToFile](<http://lazarus-ccr.sourceforge.net/docs/lcl/graphics/tgraphic.savetofile.html>). Use the appropriate TGraphic descendent which matches the format expected. See [Developing_with_Graphics#Image_formats](<Developing_with_Graphics.md> "Developing with Graphics") for a list of available image format classes. 
    
    
    var
      MyBitmap: TBitmap;
    begin
      MyBitmap := TBitmap.Create;
      try
        // Load from disk
        MyBitmap.LoadFromFile(MyEdit.Text);
    
        // Here you can use MyBitmap.Canvas to read/write to/from the image
    
        // Write back to another disk file
        MyBitmap.SaveToFile(MyEdit2.Text);
      finally
        MyBitmap.Free;
      end;
    end;
    

When using any other format the process is completely identical, just use the adequate class. For example, for PNG images: 
    
    
    var
      MyPNG: TPortableNetworkGraphic;
    begin
      MyPNG := TPortableNetworkGraphic.Create;
      try
        // Load from disk
        MyPNG.LoadFromFile(MyEdit.Text);
    
        // Here you can use MyPNG.Canvas to read/write to/from the image
    
        // Write back to another disk file
        MyPNG.SaveToFile(MyEdit2.Text);
      finally
        MyPNG.Free;
      end;
    end;
    

If you do not know beforehand the format of the image, use TPicture which will determine the format based in the file extension. Note that TPicture does not support all formats supported by Lazarus, as of Lazarus 0.9.31 it supports BMP, PNG, JPEG, Pixmap and PNM while Lazarus also supports ICNS and other formats: 
    
    
    var
      MyPicture: TPicture;
    begin
      MyPicture := TPicture.Create;
      try
        // Load from disk
        MyPicture.LoadFromFile(MyEdit.Text);
    
        // Here you can use MyPicture.Graphic.Canvas to read/write to/from the image
    
        // Write back to another disk file
        MyPicture.SaveToFile(MyEdit2.Text);
      finally
        MyPicture.Free;
      end;
    end;
    

The canvas of a form cannot be saved directly into a file and must fist be copied into an appropriate image. The following procedure copies a rectangle of the form's canvas into a .PNG file (which compresses text and graphs with good quality). If you wish to copy into a .JPG file (ideal for compressing photographs) instead of TPortableNetworkGraphic use tJPEGimage. If you wish to copy into a .BMP file (which does not compress the canvas) instead of TPortableNetworkGraphic use tBitmap: 
    
    
    procedure saveCanvasToPNGfile(fromCanvas:tCanvas;left,bottom,right,top:integer;toFileName:string); //left,bottom,right,top define the rectangle of the canvas to be save into disk
    var rectangleFrom,rectangleTo: TRect;
        myImage: TPortableNetworkGraphic; //use tJPEGimage to create JPGfile, or tBitmap to create BMP file
    begin
       rectangleFrom:=rect(left,bottom,right,top);
       rectangleTo:=rect(0,0,right-left+1,top-bottom+1);
       myImage:=tPortableNetworkGraphic.create;
       myImage.width:=right-left+1;
       myImage.height:=top-bottom+1;
       myImage.canvas.copyRect(rectangleTo,fromCanvas,rectangleFrom);
       myImage.SaveToFile(toFileName);
       myImage.free;
    end;
    

### Additional file formats for TImage

You can add additional file format support by adding the [fcl-image](<fcl-image.md> "fcl-image") fpread* and/or fpwrite* units to your uses clause. In this way, you can e.g. add support for TIFF for TImage 

### Indirect pixel access

Color for pixels can be simply read or written using TBitmap.Canvas.Pixels property. Access using Pixels property is generally slow as lot of internal management operations need to be done on each execution. This can be speed up little bit be enclosing access to Pixels with TBitmap BeginUpdate and EndUpdate. 

### Direct pixel access

To directly access the pixels of bitmaps one can use: 

  * TBitmap.ScanLine - which needs to be enclosed in BeginUpdate and EndUpdate. It uses internally the same access method as described in the next item TBitmap.RawImage.
  * TBitmap.RAWImage - allows to directly access bitmap internal memory. Color channels can be swapped for some platforms so ReadChannels and WriteChannels need to be used to convert pixel color to TColor or TFPColor correctly. Or pixel data can be accessed directly with respect to TRAWImage.Description.
  * external libraries - see [Graphics libraries](<Graphics_libraries.md> "Graphics libraries")
  * use the Lazarus native TLazIntfImage.
  * implement custom optimized bitmap structure. For a comparison of pixel access methods, see [fast direct pixel access](<Fast_direct_pixel_access.md> "Fast direct pixel access").



On some Lazarus widgetsets (notably LCL-Gtk2), the bitmap data is not stored in memory location which can be accessed by the application and in general the LCL native interfaces draw only through native Canvas routines, so each SetPixel / GetPixel operation involves a slow call to the native Canvas API. In LCL-CustomDrawn this is not the case since the bitmap is locally stored for all backends and SetPixel / GetPixel is fast. For obtaining a solution which works in all widgetsets one should use TLazIntfImage. As Lazarus is meant to be platform independent and work in gtk2. There is a GetDataLineStart function, equivalent to Scanline, but only available for memory images like [TLazIntfImage](<Developing_with_Graphics.md> "Developing with Graphics") which internally uses TRawImage. 

### Drawing color transparent bitmaps

Bitmap files (*.bmp) usually do not store any information about transparency. However, a feature, implemented already in Lazarus 0.9.11, is "color transparency": This means that one particular color is defined to be transparent and is not painted. This common trick is applied when the `Transparent` property of the bitmap instance is set to `true`. Usually the color in the bottom-left corner of the bitmap will be taken to be the transparent color unless the property `TransparentColor` is set to another color value. 

The following example loads a bitmap from a resource named 'FOLDER_OPEN', selects a color to be transparent (clFuchsia) and then draws it to the form's Canvas in its `OnPaint` event. The assignment of the `TransparentColor` can be omitted if the bottom/left pixel does already contain the transparent color. In any case, it is highly recommended to change the `Transparent` property to `true` at the end of the assignment sequence. 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      bmp: TBitmap;
    begin
      bmp := TBitmap.Create;
      try
        bmp.LoadFromResourceName(HINSTANCE, 'FOLDER_OPEN');
        bmp.TransparentColor := clFuchsia;
        bmp.Transparent := true;
        Canvas.Draw(8, 8, bmp);
      finally
        bmp.Free;
      end;
    end;
    

### Drawing alpha-channel transparent bitmaps

Descendants of TGraphic using a 32-bit-per-pixel pixelformat can have an "alpha" channel besides the normal red, green and blue color channels. This additional channel is used to provide transparency information in a gradual way. alpha=0 means "fully transparent", alpha=255 means "fully opaque", but any intermediate states are possible as well. bmp files usually do not come in this format, but the `TPortableNetworkGraphic` class (file extension: .png) is a very prominent example of an image format supporting an alpha channel by default. In the next code sample, we load a png file from the Lazarus images folder and draw it on the form in a transparent way: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      png: TCustomBitmap;
    begin
      png := TPortableNetworkGraphic.Create;
      try
        png.LoadFromFile(LazarusDir + 'images/actions/execute.png');
        Canvas.Draw(8, 8, png);
      finally
        png.Free;
      end;
    end;
    

Note that the transparency-related settings discussed in the previous section are ignored this way. 

### Taking a screenshot of the screen

Since Lazarus 0.9.16 you can use LCL to take screenshots of the screen in a cross-platform way. The following example code does it: 
    
    
    uses Graphics, LCLIntf, LCLType;
      ...
    var
      MyBitmap: TBitmap;
      ScreenDC: HDC;
    begin
      MyBitmap := TBitmap.Create;
      ScreenDC := GetDC(0);
      MyBitmap.LoadFromDevice(ScreenDC);
      ReleaseDC(0, ScreenDC);
      ...
    

Taking a screen shot but excluding your application from the screenshot: 
    
    
    uses
      Graphics, LCLIntf, LCLType, ...;
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      ScreenDC: HDC;
      LocalBitmap: TBitmap;
    begin
      Hide;
      Application.ProcessMessages;
      LocalBitmap := TBitmap.Create;
      ScreenDC := GetDC(0);
      try
        LocalBitmap.LoadFromDevice(ScreenDC);
        LocalBitmap.SaveToFile('screenshot.bmp');
      finally
        LocalBitmap.Free;
        ReleaseDC(0, ScreenDC);
        Show;
      end;
    end;
    

## Working with TLazIntfImage, TRawImage and TLazCanvas

TLazIntfImage is a non-native equivalent of TRasterImage (more commonly utilized in the form of it's descendent TBitmap). The first thing to be aware about this class is that unlike TBitmap it will not automatically allocate a memory area for the bitmap, one should first initialize a memory area and then give it to the TLazIntfImage. Right after creating a TLazIntfImage one should either connect it to a TRawImage or load it from a TBitmap. 

TRawImage is of the type object and therefore does not need to be created nor freed. It can either allocate the image memory itself when one calls TRawImage.CreateData or one can pass a memory block allocated for examply by a 3rd party library such as the Windows API of the Cocoa Framework from Mac OS X and pass the information of the image in TRawImage.Description, TRawImage.Data and TRawImage.DataSize. Instead of attaching it to a RawImage one could also load it from a TBitmap which will copy the data from the TBitmap and won't be syncronized with it afterwards. The TLazCanvas cannot exist alone and must always be attached to a TLazIntfImage. 

The example below shows how to choose a format for the data and ask the TRawImage to create it for us and then we attach it to a TLazIntfImage and then attach a TLazCanvas to it: 
    
    
    uses graphtype, intfgraphics, lazcanvas;
    
    var
      AImage: TLazIntfImage;
      ACanvas: TLazCanvas;
      lRawImage: TRawImage;
    begin
        lRawImage.Init;
        lRawImage.Description.Init_BPP32_A8R8G8B8_BIO_TTB(AWidth, AHeight);
        lRawImage.CreateData(True);
        AImage := TLazIntfImage.Create(0,0);
        AImage.SetRawImage(lRawImage);
        ACanvas := TLazCanvas.Create(AImage);
    

### Initializing a TLazIntfImage

One cannot simply create an instance of TLazIntfImage and start using it. It needs to add a storage to it. There are 3 ways to do this: 

1\. Attach it to a TRawImage 

2\. Load it from a TBitmap. Note that it will copy the memory of the TBitmap so it won't remain connected to it. 
    
    
       SrcIntfImg:=TLazIntfImage.Create(0,0);
       SrcIntfImg.LoadFromBitmap(ABitmap.Handle,ABitmap.MaskHandle);
    

3\. Load it from a raw image description, like this: 
    
    
       IntfImg := TLazIntfImage.Create(0,0);
       IntfImg.DataDescription:=GetDescriptionFromDevice(0);
       IntfImg.SetSize(10,10);
    

The 0 device in **GetDescriptionFromDevice(0)** uses the current screen format. 

### TLazIntfImage.LoadFromFile

Here is an example how to load an image directly into a TLazIntfImage. It initializes the TLazIntfImage to a 32bit RGBA format. Keep in mind that this is probably not the native format of your screen. 
    
    
    uses LazLogger, Graphics, IntfGraphics, GraphType;
    procedure TForm1.FormCreate(Sender: TObject);
    var
      AImage: TLazIntfImage;
      lRawImage: TRawImage;
    begin
      // create a TLazIntfImage with 32 bits per pixel, alpha 8bit, red 8 bit, green 8bit, blue 8bit,
      // Bits In Order: bit 0 is pixel 0, Top To Bottom: line 0 is top
      lRawImage.Init;
      lRawImage.Description.Init_BPP32_A8R8G8B8_BIO_TTB(0,0);
      lRawImage.CreateData(false);
      AImage := TLazIntfImage.Create(0,0);
      try
        AImage.SetRawImage(lRawImage);
        // Load an image from disk.
        // It uses the file extension to select the right registered image reader.
        // The AImage will be resized to the width, height of the loaded image.
        AImage.LoadFromFile('lazarus/examples/openglcontrol/data/texture1.png');
        debugln(['TForm1.FormCreate ',AImage.Width,' ',AImage.Height]);
      finally
        AImage.Free;
      end;
    end;
    

### Loading a TLazIntfImage into a TImage

The pixel data of a **TImage** is the **TImage.Picture** property, which is of type _TPicture_. **TPicture** is a multi format container containing one of several common image formats like Bitmap, Icon, Jpeg or PNG . Usually you will use the _TPicture.Bitmap_ to load a **TLazIntfImage** : 
    
    
        Image1.Picture.Bitmap.LoadFromIntfImage(IntfImg);
    

**Notes:**

  * To load a **color-transparent** TLazIntfImage you have to set the **Image1.Transparent** to true. In "color-transparency", one color (usually the one of the pixel (0,0)) is declared to be transparent and is not drawn. This must be distinguished from "alpha-channel" transparency where a fourth channel is added to RGB images to describe the transparency of each pixel. See the next section for an example.
  * TImage uses the screen format. If the TLazIntfImage has a different format then the pixels will be converted. Hint: You can use **IntfImg.DataDescription:=GetDescriptionFromDevice(0);** to initialize the TLazIntfImage with the screen format.



### Creating and drawing a transparent bitmap for a TImage

"Transparency" in this section is understood as "alpha-channel transparency" where an additional color channel defines the transparency of each pixel. Usually, 24-bits-per-pixel images are extended to 32 bits-per-pixel to provide the alpha channel. 

In the following example we draw some semi-transparent shapes on a transparent image which then is displayed on a TImage component. A `TBitmap` is created for this purpose as buffer for the image. We activate its alpha channel by setting it `Pixelformat` to `pf32Bit` so that semi-transparent drawing is possible. Drawing itself it done on a `TLazIntfImage` and a `TLazCanvas` which give an easy access to the underlying fcl-image features, but simultaneously provide an easy interface to the LCL class `TBitmap`: 
    
    
    uses
      fpImage, fpCanvas, IntfGraphics, LazCanvas;
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      bmp: TBitmap;
      img: TLazIntfImage;
      cnv: TLazCanvas;
    begin
      bmp := TBitmap.Create;
      try
        bmp.PixelFormat := pf32Bit;           // IMPORTANT!
        bmp.SetSize(Image1.Width, Image1.Height);
        img := bmp.CreateIntfImage;
        try
          cnv := TLazCanvas.Create(img);
          try
            cnv.DrawingMode := dmAlphaBlend;  // This activates the alpha-blend mode.
    
            // Background
            cnv.Pen.Style := psClear;
            cnv.Brush.FPColor := colTransparent;
            cnv.FillRect(0, 0, img.Width, img.Height);
    
            // Yellow opaque rectangle
            cnv.Brush.FPColor := FPColor($FFFF, $FFFF, 0);
            cnv.Rectangle(10, 10, 190, 100);
    
            // Overlapping semi-transparent red circle
            cnv.Brush.FPColor := FPColor($FFFF, 0, 0, $4000);
            cnv.Ellipse(60, 60, 140, 140);
    
            // Overlapping semi-transparent blue circle
            cnv.Brush.FPColor := FPColor(0, 0, $FFFF, $8000);
            cnv.Ellipse(0, 100, 100, 200);
          finally
            cnv.Free;
          end;
    
          bmp.LoadFromIntfImage(img);
          Image1.Picture.Assign(bmp);
        finally
          img.Free;
        end;
      finally
        bmp.Free;
      end;
    end;
    

### Fading example

A fading example with TLazIntfImage 
    
    
    { This code has been taken from the $LazarusPath/examples/lazintfimage/fadein1.lpi project. }
    uses LCLType, // HBitmap type
         IntfGraphics, // TLazIntfImage type
         fpImage; // TFPColor type
    ...
     procedure TForm1.FadeIn(ABitMap: TBitMap);
     var
       SrcIntfImg, TempIntfImg: TLazIntfImage;
       ImgHandle,ImgMaskHandle: HBitmap;
       FadeStep: Integer;
       px, py: Integer;
       CurColor: TFPColor;
       TempBitmap: TBitmap;
     begin
       SrcIntfImg:=TLazIntfImage.Create(0,0);
       SrcIntfImg.LoadFromBitmap(ABitmap.Handle,ABitmap.MaskHandle);
       TempIntfImg:=TLazIntfImage.Create(0,0);
       TempIntfImg.LoadFromBitmap(ABitmap.Handle,ABitmap.MaskHandle);
       TempBitmap:=TBitmap.Create;
       for FadeStep:=1 to 32 do begin
         for py:=0 to SrcIntfImg.Height-1 do begin
           for px:=0 to SrcIntfImg.Width-1 do begin
             CurColor:=SrcIntfImg.Colors[px,py];
             CurColor.Red:=(CurColor.Red*FadeStep) shr 5;
             CurColor.Green:=(CurColor.Green*FadeStep) shr 5;
             CurColor.Blue:=(CurColor.Blue*FadeStep) shr 5;
             TempIntfImg.Colors[px,py]:=CurColor;
           end;
         end;
         TempIntfImg.CreateBitmaps(ImgHandle,ImgMaskHandle,false);
         TempBitmap.Handle:=ImgHandle;
         TempBitmap.MaskHandle:=ImgMaskHandle;
         Canvas.Draw(0,0,TempBitmap);
       end;
       SrcIntfImg.Free;
       TempIntfImg.Free;
       TempBitmap.Free;
     end;
    

### Image format specific example

If you know that the TBitmap is using blue 8bit, green 8bit, red 8bit you can directly access the bytes, which is somewhat faster: 
    
    
    uses LCLType, // HBitmap type
         IntfGraphics, // TLazIntfImage type
         fpImage; // TFPColor type
    ...
    type
      TRGBTripleArray = array[0..32767] of TRGBTriple;
      PRGBTripleArray = ^TRGBTripleArray;
    
    procedure TForm1.FadeIn2(aBitMap: TBitMap);
     var
       IntfImg1, IntfImg2: TLazIntfImage;
       ImgHandle,ImgMaskHandle: HBitmap;
       FadeStep: Integer;
       px, py: Integer;
       CurColor: TFPColor;
       TempBitmap: TBitmap;
       Row1, Row2: PRGBTripleArray;
     begin
       IntfImg1:=TLazIntfImage.Create(0,0);
       IntfImg1.LoadFromBitmap(aBitmap.Handle,aBitmap.MaskHandle);
    
       IntfImg2:=TLazIntfImage.Create(0,0);
       IntfImg2.LoadFromBitmap(aBitmap.Handle,aBitmap.MaskHandle);
    
       TempBitmap:=TBitmap.Create;
       
       //with Scanline-like
       for FadeStep:=1 to 32 do begin
         for py:=0 to IntfImg1.Height-1 do begin
           Row1 := IntfImg1.GetDataLineStart(py); //like Delphi TBitMap.ScanLine
           Row2 := IntfImg2.GetDataLineStart(py); //like Delphi TBitMap.ScanLine
           for px:=0 to IntfImg1.Width-1 do begin
             Row2^[px].rgbtRed:= (FadeStep * Row1^[px].rgbtRed) shr 5;
             Row2^[px].rgbtGreen := (FadeStep * Row1^[px].rgbtGreen) shr 5; // Fading
             Row2^[px].rgbtBlue := (FadeStep * Row1^[px].rgbtBlue) shr 5;
           end;
         end;
         IntfImg2.CreateBitmaps(ImgHandle,ImgMaskHandle,false);
         
         TempBitmap.Handle:=ImgHandle;
         TempBitmap.MaskHandle:=ImgMaskHandle;
         Canvas.Draw(0,0,TempBitmap);
       end; 
    
       IntfImg1.Free;
       IntfImg2.Free;
       TempBitmap.Free;
     end;
    

### Conversion between TLazIntfImage and TBitmap

The best way to access the pixels of an image in a fast way for both reading and writing is by using `TLazIntfImage`. The `TBitmap` can be converted to a `TLazIntfImage` by using the bitmap's `CreateIntfImage()` method. After modifying the pixels it can be converted back to a `TBitmap` by using the bitmap's `LoadFromIntfImage()` method. This technique applies also to any other descendant of `TRasterImage`, such as `TPortableNetworkGraphic` or `TJpegImage`. 

Here's sample code how to create `TLazIntfImage` from `TBitmap`, modify it and then go back to the `TBitmap`. 
    
    
    uses ...
      Graphics,     // for TBitmap
      FPImage,      // for colGreen
      IntfGraphics, // for TLazIntfImage
      ...;
    
    procedure TForm1.Button4Click(Sender: TObject);
    var
      bmp: TBitmap;
      img: TLazIntfImage;
    begin
      bmp := TBitmap.Create;
      try
        // Load bitmap from image file.
        bmp.LoadFromFile('test.bmp');
        // Create LazIntfImage from bitmap
        img := bmp.CreateIntfImage;
        try
          // Read and/or write to the pixels
          img.Colors[10,20] := colGreen;
          // Convert LazIntfImage back to bitmap
          bmp.LoadFromIntfImage(img);
        finally
          img.Free;
        end;
        // Do something with the bitmap
        bmp.SaveToFile('test-mod.bmp');
      finally
        bmp.Free;
      end;
    end;
    

### Using the non-native StretchDraw from LazCanvas

Just like TCanvas.StretchDraw there is TLazCanvas.StretchDraw but you need to specify the interpolation which you desire to use. The interpolation which provides a Windows-like StretchDraw with a very sharp result (the opposite of anti-aliased) can be added with: 
    
    
    TLazCanvas.Interpolation := TFPSharpInterpolation.Create;
    

There are other interpolations available in the unit fpcanvas. 
    
    
    uses 
      intfgraphics, lazcanvas;
    
    procedure TForm1.StretchDrawBitmapToBitmap(SourceBitmap, DestBitmap: TBitmap; DestWidth, DestHeight: integer);
    var
      DestIntfImage, SourceIntfImage: TLazIntfImage;
      DestCanvas: TLazCanvas;
    begin
      // Prepare the destination
      DestIntfImage := TLazIntfImage.Create(0, 0);
      try
        DestIntfImage.LoadFromBitmap(DestBitmap.Handle, 0);
        DestCanvas := TLazCanvas.Create(DestIntfImage);
        try
          //Prepare the source
          SourceIntfImage := TLazIntfImage.Create(0, 0);
          try
            SourceIntfImage.LoadFromBitmap(SourceBitmap.Handle, 0);
            // Execute the stretch draw via TFPSharpInterpolation
            DestCanvas.Interpolation := TFPSharpInterpolation.Create;
            try
              DestCanvas.StretchDraw(0, 0, DestWidth, DestHeight, SourceIntfImage);
              // Reload the image into the TBitmap
              DestBitmap.LoadFromIntfImage(DestIntfImage);
            finally
              DestCanvas.Interpolation.Free;
            end;
          finally
            SourceIntfImage.Free;
          end;
        finally
          DestCanvas.Free;
        end;
      finally
        DestIntfImage.Free;
      end;
    end; 
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      Bmp, DestBitmap: TBitmap;
    begin
      // Prepare the destination
      DestBitmap := TBitmap.Create;
      try
        DestBitmap.SetSize(100, 100);
        // Create and draw the source bitmap: a small red rectangle
        Bmp := TBitmap.Create;
        try
          Bmp.SetSize(10, 10);
          Bmp.Canvas.Pen.Color := clBlack;
          Bmp.Canvas.Brush.Color := clRed;
          Bmp.Canvas.Rectangle(0, 0, 10, 10);
          Canvas.Draw(10, 10, Bmp);
          // Stretch-draw the source bitmap onto the destination bitmap canvas
          StretchDrawBitmapToBitmap(Bmp, DestBitmap, 100, 100);
          Canvas.Draw(100, 100, DestBitmap);
        finally
          Bmp.Free;
        end;
      finally
        DestBitmap.Free;
      end;
    end;
    

### Clipping

#### Clipping by a rectangle

Anything outside the specified `ClipRect` of the canvas is removed when `Clipping` is true: 

[![LazCanvas ClipRect.png](https://wiki.freepascal.org/images/a/a8/LazCanvas_ClipRect.png)](</File:LazCanvas_ClipRect.png>)
    
    
    uses
      FPImage, IntfGraphics, LazCanvas;
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      bmp: TBitmap;
      img: TLazIntfImage;
      cnv: TLazCanvas;
      Rc: TRect;
      polyPts: array[0..4] of TPoint = (
        (x:100;y:10), (x:190;y:80), (x:170;y:190), (x:30;y:190), (x:10;y:80)
      );
    begin
      // Buffer bitmap
      bmp := TBitmap.Create;
      try
        bmp.SetSize(Width, Height);
        img := bmp.CreateIntfImage;
        try
          cnv := TLazCanvas.Create(img);
          try
            cnv.Brush.FPColor := colWhite;
            cnv.Clear;
    
            // To better understand what is removed, we draw the outline of the
            // polygon to be clipped
            cnv.Pen.Style := psDot;
            cnv.Pen.FPColor := colRed;
            cnv.Polygon(polyPts);
    
            // Define the clipping rectangle
            Rc :=  Rect(50, 10, 180, 170);
            // To understand where the clipping rectangle is we draw its outline
            cnv.Brush.Style := bsClear;
            cnv.Pen.FPColor := colSilver;
            cnv.Rectangle(Rc);
    
            // Activate clipping
            cnv.ClipRect := Rc;
            cnv.Clipping := true;
    
            // Draw the polygon to be clipped
            cnv.Brush.Style := bsSolid;
            cnv.Brush.FPColor := colRed;
            cnv.Polygon(polyPts);
          finally
            cnv.Free;
          end;
          bmp.LoadFromIntfImage(img);
          Canvas.Draw(0, 0, bmp);
        finally
          img.Free;
        end;
      finally
        bmp.Free;
      end;
    end;
    

#### Clipping by a region

For more complex clipping `TLazCanvas` provides several kinds of "regions". Pixels are drawn only when they are inside the parts added to the region. `TLazCanvas` supports rectangular, elliptical and polygonal parts. In the following example, a region is created which contains an ellipse and a rectangle. Clipping is activated by using this region in the `SetLazClipRegion()` method of the `LazCanvas`: 

[![LazCanvas ClipRegion.png](https://wiki.freepascal.org/images/a/af/LazCanvas_ClipRegion.png)](</File:LazCanvas_ClipRegion.png>)
    
    
    uses
      FPImage, FPCanvas, IntfGraphics, LazCanvas, LazRegions;
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      bmp: TBitmap;
      img: TLazIntfImage;
      cnv: TLazCanvas;
      rgn: TLazRegion;
      Rc1, Rc2: TRect;
      polyPts: array[0..4] of TPoint = (
        (x:100;y:10), (x:190;y:80), (x:170;y:190), (x:30;y:190), (x:10;y:80)
      ); 
    begin
      // Buffer bitmap
      bmp := TBitmap.Create;
      try
        bmp.SetSize(ClientWidth, ClientHeight);
        img := bmp.CreateIntfImage;
        try
          cnv := TLazCanvas.Create(img);
          cnv.Brush.FPColor := colWhite;
          cnv.Clear;
    
          // To better understand what is clipped, draw the outline of the polygon
          // to be clipped
          cnv.Pen.Style := psDot;
          cnv.Pen.FPColor := colRed;
          cnv.Brush.Style := bsClear;
          cnv.Polygon(polyPts);
    
          // ... and we draw the outline of the parts in the clipping region:
          // We clip by an elliptical region...
          Rc1 := Rect(20, 20, 140, 140);
          cnv.Pen.FPColor := colSilver;
          cnv.Ellipse(Rc1);
          // ... and by a rectangular region
          Rc2 := Rect(100, 100, 200, 170);
          cnv.Rectangle(Rc2);
    
          // Create the clipping region
          rgn := TLazRegion.Create;
          try
            // Show pixels that are inside the ellipse...
            rgn.AddEllipse(Rc1.Left, Rc1.Top, Rc1.Right, Rc1.Bottom);
            // ... and the rectangle.
            rgn.AddRectangle(Rc2);
            // Activate the clip region
            cnv.SetLazClipRegion(rgn);
    
            // Draw the polygon to be clipped
            cnv.Brush.FPColor := colRed;
            cnv.Brush.Style := bsSolid;
            cnv.Pen.Style := psSolid;
            cnv.Pen.FPColor := colBlack;
            cnv.Polygon(polyPts);
          finally
            cnv.Free;
          end;
    
          // Draw the buffer bitmap in the Paintbox
          bmp.LoadFromIntfImage(img);
          Canvas.Draw(0, 0, bmp);
        finally
          img.Free;
        end;
      finally
        bmp.Free;
      end;
    end;
    

## Motion Graphics - How to Avoid flickering

Many programs draw their output to the GUI as 2D graphics. If those graphics need to change quickly you will soon face a problem: quickly changing graphics often flicker on the screen. This happens when users sometimes see the whole images and sometimes only when it is partially drawn. It occurs because the painting process requires time. 

How can you avoid the flickering and get the best drawing speed? Of course you could work with hardware acceleration using OpenGL, but this approach is quite heavy for small programs or old computers. 

Another solution is drawing to a TCanvas. If you need help with OpenGL, take a look at the example that comes with Lazarus. You can also use A.J. Venter's gamepack, which provides a double-buffered canvas and a sprite component. 

A brief and very helpful article on avoiding flicker can be found at <http://delphi.about.com/library/bluc/text/uc052102g.htm>. Although written for Delphi, the techniques work well with Lazarus. 

Now we will examine the options we have for drawing to a Canvas: 

  * Draw to a TImage
  * Draw on the OnPaint event of the form, a TPaintBox or another control
  * Create a custom control which draws itself
  * Using A.J. Venter's gamepack



### Draw to a TImage

A TImage consists of 2 parts: A TGraphic, usually a TBitmap, holding the persistent picture and the visual area, which is repainted on every OnPaint. Resizing the TImage does **not** resize the bitmap. The graphic (or bitmap) is accessible via Image1.Picture.Graphic (or Image1.Picture.Bitmap). The canvas is Image1.Picture.Bitmap.Canvas. The canvas of the visual area of a TImage is only accessible during Image1.OnPaint via Image1.Canvas. 

**Important** : Never use the OnPaint of the Image1 event to draw to the graphic/bitmap of a TImage. The graphic of a TImage is buffered so all you need to do is draw to it from anywhere and the change is there forever. However, if you are constantly redrawing, the image will flicker. In this case you can try the other options. Drawing to a TImage is considered slower then the other approaches. 

#### Resizing the bitmap of a TImage

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Do not use this during OnPaint.
    
    
    with Image1.Picture.Bitmap do begin
      Width:=100;
      Height:=120;
    end;
    

Same in one step: 
    
    
    with Image1.Picture.Bitmap do begin
      SetSize(100, 120);
    end;
    

#### Painting on the bitmap of a TImage

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Do not use this during OnPaint.
    
    
    with Image1.Picture.Bitmap.Canvas do begin
      // fill the entire bitmap with red
      Brush.Color := clRed;
      FillRect(0, 0, Width, Height);
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Inside of Image1.OnPaint the Image1.Canvas points to the volatile visible area. Outside of Image1.OnPaint the Image1.Canvas points to Image1.Picture.Bitmap.Canvas.

Another example: 
    
    
    procedure TForm1.BitBtn1Click(Sender: TObject);
    var
      x, y: Integer;
    begin
      // Draws the backgroung
      MyImage.Canvas.Pen.Color := clWhite;
      MyImage.Canvas.Rectangle(0, 0, Image.Width, Image.Height);
       
      // Draws squares
      MyImage.Canvas.Pen.Color := clBlack;
      for x := 1 to 8 do
        for y := 1 to 8 do
          MyImage.Canvas.Rectangle(Round((x - 1) * Image.Width / 8), Round((y - 1) * Image.Height / 8),
            Round(x * Image.Width / 8), Round(y * Image.Height / 8));
    end;
    

#### Painting on the volatile visual area of the TImage

You can only paint on this area during OnPaint. OnPaint is eventually called automatically by the LCL when the area was invalidated. You can invalidate the area manually with Image1.Invalidate. This will not immediately call OnPaint and you can call Invalidate as many times as you want. 
    
    
    procedure TForm.Image1Paint(Sender: TObject);
    begin
      // paint a line
      Canvas.Pen.Color := clRed;
      Canvas.Line(0, 0, Width, Height);
    end;
    

### Draw on the OnPaint event

In this case all the drawing has to be done on the OnPaint event of the form, or of another control. The drawing isn't buffered like in the TImage, and it needs to be fully redrawn in each call of the OnPaint event handler. 
    
    
    procedure TForm.Form1Paint(Sender: TObject);
    begin
      // paint a line
      Canvas.Pen.Color := clRed;
      Canvas.Line(0, 0, Width, Height);
    end;
    

### Create a custom control which draws itself

Creating a custom control has the advantage of structuring your code and you can reuse the control. This approach is very fast, but it can still generate flickering if you don't draw to a TBitmap first and then draw to the canvas. On this case there is no need to use the OnPaint event of the control. 

Here is an example custom control: 
    
    
    uses
      Classes, SysUtils, Controls, Graphics, LCLType;
     
    type
      TMyDrawingControl = class(TCustomControl)
      public
        procedure EraseBackground(DC: HDC); override;
        procedure Paint; override;
      end;
     
    implementation
     
    procedure TMyDrawingControl.EraseBackground(DC: HDC);
    begin
      // Uncomment this to enable default background erasing
      //inherited EraseBackground(DC);
    end; 
     
    procedure TMyDrawingControl.Paint;
    var
      x, y: Integer;
      Bitmap: TBitmap;
    begin
      Bitmap := TBitmap.Create;
      try
        // Initializes the Bitmap Size
        Bitmap.Height := Height;
        Bitmap.Width := Width;
     
        // Draws the background
        Bitmap.Canvas.Pen.Color := clWhite;
        Bitmap.Canvas.Rectangle(0, 0, Width, Height);
     
        // Draws squares
        Bitmap.Canvas.Pen.Color := clBlack;
        for x := 1 to 8 do
          for y := 1 to 8 do
            Bitmap.Canvas.Rectangle(Round((x - 1) * Width / 8), Round((y - 1) * Height / 8),
              Round(x * Width / 8), Round(y * Height / 8));
           
        Canvas.Draw(0, 0, Bitmap);
      finally
        Bitmap.Free;
      end;
     
      inherited Paint;
    end;
    

and how we create it on the form: 
    
    
    procedure TMyForm.FormCreate(Sender: TObject);
    begin
      MyDrawingControl := TMyDrawingControl.Create(Self);
      MyDrawingControl.Height := 400;
      MyDrawingControl.Width := 500;
      MyDrawingControl.Top := 0;
      MyDrawingControl.Left := 0;
      MyDrawingControl.Parent := Self;
      MyDrawingControl.DoubleBuffered := True;
    end;
    

It is destroyed automatically, because we use Self as owner. 

Setting Top and Left to zero is not necessary, since this is the standard position, but is done so to reinforce where the control will be put. 

"MyDrawingControl.Parent := Self;" is very important and you won't see your control if you don't do so. 

"MyDrawingControl.DoubleBuffered := True;" is required to avoid flickering on Windows. It has no effect on gtk. 

## Image formats

Here is a table with the correct class to use for each image format. 

Format | Image class | Unit   
---|---|---  
Cursor (cur) | TCursor | Graphics   
Bitmap (bmp) | TBitmap | Graphics   
Windows icon (ico) | TIcon | Graphics   
Mac OS X icon (icns) | TicnsIcon | Graphics   
Pixmap (xpm) | TPixmap | Graphics   
Portable Network Graphic (png) | TPortableNetworkGraphic | Graphics   
JPEG (jpg, jpeg) | TJpegImage | Graphics   
PNM (pnm) | TPortableAnyMapGraphic | Graphics   
Tiff (tif, tiff) | TTiffImage | Graphics   
  
See also the list of [fcl-image supported formats](<fcl-image.md> "fcl-image"). 

### Converting formats

Sometimes it is necessary to convert one graphic type to another. One of the ways is to convert a graphic to intermediate format, and then convert it to TBitmap. Most of the formats can create an image from TBitmap. 

Converting Bitmap to PNG and saving it to a file: 
    
    
    procedure SaveToPng(const bmp: TBitmap; PngFileName: String);
    var
      png : TPortableNetworkGraphic; 
    begin 
      png := TPortableNetworkGraphic.Create;
      try
        png.Assign(bmp);
        png.SaveToFile(PngFileName);
      finally 
        png.Free;
      end;
    end;
    

## Pixel Formats

### TColor

The internal pixel format for TColor in the LCL is the XXBBGGRR format, which matches the native Windows format and is opposite to most other libraries, which use AARRGGBB. The XX part is used to identify if the color is a fixed color, which case XX should be 00 or if it is an index to a system color. There is no space reserved for an alpha channel. 

To convert from separate RGB channels to TColor use: 
    
    
    RGBToColor(RedVal, GreenVal, BlueVal);
    

To get each channel of a TColor variable use the Red, Green and Blue functions: 
    
    
    RedVal := Red(MyColor);
    GreenVal := Green(MyColor);
    BlueVal := Blue(MyColor);
    

### TFPColor

TFPColor uses the AARRGGBB format common to most libraries, but it uses 16-bits for the depth of each color channel, totaling 64-bits per pixel, which is unusual. This does not necessarily mean that images will consume that much memory, however. Images created using TRawImage+TLazIntfImage can have any internal storage format and then on drawing operations TFPColor is converted to this internal format. 

The unit Graphics provides routines to convert between TColor and TFPColor: 
    
    
    function FPColorToTColorRef(const FPColor: TFPColor): TColorRef;
    function FPColorToTColor(const FPColor: TFPColor): TColor;
    function TColorToFPColor(const c: TColorRef): TFPColor; overload;
    function TColorToFPColor(const c: TColor): TFPColor; overload; // does not work on system color
    

## Drawing with fcl-image

You can draw images which won't be displayed in the screen without the LCL, by just using fcl-image directly. For example a program running on a webserver without X11 could benefit from not having a visual library as a dependency. FPImage (alias fcl-image) is a very generic image and drawing library written completely in Pascal. In fact the LCL uses FPImage too for all the loading and saving from/to files and implements the drawing function through calls to the widgetset (winapi, gtk, carbon, ...). Fcl-image on the other hand also has drawing routines. 

For more information, please read the article about [fcl-image](<fcl-image.md> "fcl-image"). 

## Common OnPaint Error

A common error that causes many false bug reports is to call an OnPaint event for one object from another object. When using the LCL, this may work in GTK2 and Windows but will probably fail with Qt, Carbon and Cocoa. It is not normally necessary to call Invalidate. However, it may sometimes be needed in the Button1Click procedure. 

This is bad: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      Shape1Paint(Self); // Call Shape1 OnPaint event
      Shape1.Invalidate; // Invoke actual painting
    
      //more code for Button1
    end;
    

This is good: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      //code for Button1
      //set some condition
      Shape1.Invalidate; // May be necessary on some occasions
    end;
    
    // Shape1Paint should be attached to the OnPaint event of shape object !
    procedure TForm1.Shape1Paint(Sender: TObject);
    begin   
      if some_condition then 
        with Shape1.Canvas do
        begin
          //lots of stuff
        end;
    end;
    

## Some useful examples

### Example 1: Drawing on loaded JPEG with TImage

Add procedure LoadAndDraw to the public section of your form, and paste next code to implemantation section: 
    
    
    procedure TForm1.LoadAndDraw(const sFileName: String);
    var 
      jpg: TPicture;
    begin
      jpg := TPicture.Create;
      try
        jpg.LoadFromFile(sFileName);
     
        Image.Picture.Bitmap.SetSize(jpg.Width, jpg.Height);
        Image.Picture.Bitmap.Canvas.Draw(0, 0, jpg.Bitmap);
     
        Image.Picture.Bitmap.Canvas.Pen.Color := clRed;
        Image.Picture.Bitmap.Canvas.Line(0, 0, 140, 140);
      finally
        jpg.Free;
      end;
    end;
    

### Example 2: Drawing on controls of Form

1) Create a New project - Application, add to uses section next modules if needed: Types, Controls, Graphics. 

2) Place on form Button1, GroupBox1 and RadioGroup1 

3) Place on GroupBox1 one more button - Button2 

4) Your TForm1.Create should looks like: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      i: Integer;
    begin
      for i := 0 to Self.ControlCount - 1 do
        RadioGroup1.Items.AddObject(Controls[i].Name, Controls[i]);
    
      RadioGroup1.Items.AddObject(Button2.Name,Button2);
    end;
    

5) For RadioGroup1 create handler of OnSelectionChanged event: 
    
    
    procedure TForm1.RadioGroup1SelectionChanged(Sender: TObject);
    begin
      Self.Repaint;
    end;
    

6) Add to public section of your form procedure HighlightControl: 
    
    
    procedure TForm1.HighlightControl(AControl: TControl);
    var
      R: Types.TRect;
      aCC: TControlCanvas;
    begin
      R := AControl.BoundsRect;
      InflateRect(R, 2, 2);          // make rect a bit bigger then control
      aCC := TControlCanvas.Create;
      aCC.Control := AControl.Parent;
      aCC.Pen.Color := clGreen;
      aCC.Pen.Width := 5;
      aCC.Pen.Style := psSolid;
      aCC.Brush.Style := bsClear;
      aCC.Rectangle(R);
      aCC.free;
    end;
    

## See also

  * [Fast direct pixel access](<Fast_direct_pixel_access.md> "Fast direct pixel access")



### 2D drawing

  * [ZenGL](<ZenGL.md> "ZenGL") \- cross-platform game development library using OpenGL.
  * [BGRABitmap](<BGRABitmap.md> "BGRABitmap") \- Drawing shapes and bitmaps with transparency, direct access to pixels, etc.
  * [LazRGBGraphics](<LazRGBGraphics.md> "LazRGBGraphics") \- A package for fast in memory image processing and pixel manipulations (like scan line).
  * [fpvectorial](<fpvectorial.md> "fpvectorial") \- Offers support to read, modify and write vectorial images.
  * [Double Gradient](<Double_Gradient.md> "Double Gradient") \- Draw 'double gradient' & 'n gradient' bitmaps easy.
  * [Gradient Filler](<Gradient_Filler.md> "Gradient Filler") \- TGradientFiller is the best way to create custom n gradients in Lazarus.
  * [PascalMagick](<PascalMagick.md> "PascalMagick") \- an easy to use API for interfacing with [ImageMagick](<http://www.imagemagick.org>), a multiplatform free software suite to create, edit, and compose bitmap images.
  * [Sample Graphics](<Sample_Graphics.md> "Sample Graphics") \- graphics gallery created with Lazarus and drawing tools
  * [Fast direct pixel access](<Fast_direct_pixel_access.md> "Fast direct pixel access") \- speed comparison of some methods for direct bitmap pixel access
  * [AggPas](<http://www.crossgl.com/aggpas/>) \- AggPas is an Object Pascal native port of the Anti-Grain Geometry library. It is fast and very powerful with anti-aliased drawing and subpixel accuracy. You can think of AggPas as of a rendering engine that produces pixel images in memory from some vectorial data. Both Lazarus and [fpGUI](<fpGUI.md> "fpGUI") bundle AggPas.



### 3D drawing

  * [OpenGL](<OpenGL.md> "OpenGL") \- OpenGL (Open Graphics Library) is a cross-platform API for producing 3D computer graphics.
  * [GLScene](<GLScene.md> "GLScene") \- A port of the 3D visual OpenGL graphics Library [GLScene](<http://www.glscene.org>).
  * [macOS Metal Framework](<Metal_Framework.md> "Metal Framework") \- Render advanced 3D graphics and perform data-parallel computations using graphics processors.
  * [Castle Game Engine](<Castle_Game_Engine.md> "Castle Game Engine") \- An open-source cross-platform 3D and 2D game engine ([official website](<https://castle-engine.io/>)).



### Charts

  * [TAChart](<TAChart.md> "TAChart") \- Charting component for Lazarus
  * [PlotPanel](<PlotPanel.md> "PlotPanel") \- A plotting and charting component for animated graphs
  * [Perlin Noise](<Perlin_Noise.md> "Perlin Noise") \- An article about using Perlin Noise on LCL applications.

---

_Source: [https://wiki.freepascal.org/Developing_with_Graphics](https://web.archive.org/web/20240909185905/https://wiki.freepascal.org/Developing_with_Graphics)_
