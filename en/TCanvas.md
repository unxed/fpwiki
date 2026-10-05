# TCanvas

│ **English (en)** │  **[русский (ru)](<../ru/TCanvas.md>)** │

A **TCanvas** is the primary tool to draw graphics. 

To draw lines, boxes and text a [TPen](</index.php?title=TPen&action=edit&redlink=1> "TPen \(page does not exist\)"), [TBrush](<TBrush.md> "TBrush") and [TFont](</index.php?title=TFont&action=edit&redlink=1> "TFont \(page does not exist\)") are to be used to determine pen size and color, brush style and color, font style and color. 

Render text | TextOut( x, y, text )   
---|---  
Draw line | Line( x1, y1, x2, y2 ); LineTo( x, y ) with MoveTo(x, y )   
Draw a box | Rectangle( l, t, r, b ); Rectangle( R: TRect ); FillRect(...)   
Draw a circle | Ellipse( l, t, r, b ); Ellipse( R: TRect )   
  
  


## Contents

  * 1 Drawing shapes
    * 1.1 Drawing a rectangle
    * 1.2 Drawing a circle
    * 1.3 Drawing a polygon
      * 1.3.1 Simple polygon
      * 1.3.2 Self-overlapping polygons
      * 1.3.3 Polygon with a hole
      * 1.3.4 Polygon with several holes
  * 2 Drawing Text
    * 2.1 Text painting methods
    * 2.2 Using the default GUI font
    * 2.3 Drawing text to an exactly fitting width
    * 2.4 Drawing text with sharp edges (non antialiased)



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

---

_Source: [https://wiki.freepascal.org/TCanvas](https://web.archive.org/web/20240709183742/https://wiki.freepascal.org/TCanvas)_
