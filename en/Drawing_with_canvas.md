# Drawing with canvas

│ **English (en)** │

**Drawing with canvas** can be done using several procedures e.g. 

  * [Canvas line](<Canvas_line.md> "Canvas line"), draws a line from coordinates (x1,y1) to (x2,y2)
  * [Canvas rectangle](<Canvas_rectangle.md> "Canvas rectangle"), draws a rectangle from upper left (x1,y1) to lower right (x2,y2)
  * [Canvas ellipse](</index.php?title=Canvas_ellipse&action=edit&redlink=1> "Canvas ellipse \(page does not exist\)"), draws an ellipse in a rectangle defined by (x1,y1) and (x2,y2). If x2-x1 = y2-y1, the ellipse will be a circle with radius (x2-x1)/2.
  * [Canvas Arc](</index.php?title=Canvas_Arc&action=edit&redlink=1> "Canvas Arc \(page does not exist\)"), Draws an arc of an ellipse.
  * [Canvas Polygon](</index.php?title=Canvas_Polygon&action=edit&redlink=1> "Canvas Polygon \(page does not exist\)"), Draws a filled polygon.
  * [Canvas TextOut](</index.php?title=Canvas_TextOut&action=edit&redlink=1> "Canvas TextOut \(page does not exist\)"), Draws text at a specified position.
  * [Canvas DrawText](</index.php?title=Canvas_DrawText&action=edit&redlink=1> "Canvas DrawText \(page does not exist\)"), Draws text with additional formatting options.



In the Canvas object the `Brush` and `Pen` objects are defined, both with a `Color` property, which indicate the color that makes the fill and stroke of the various objects that are drawn. To paint an object of one color, the first thing is to change the brush and color, before giving the instruction to draw, the order of the statements is important. This would be the code, notice how the color is changed before drawing the ellipse: 
    
    
      Canvas.Brush.Color:= clRed;
      Canvas.Ellipse(195, 117, 205, 128);
      Canvas.Brush.Color:= clBlue;
      Canvas.Rectangle (192, 130,208,160);
      Canvas.Brush.Color:= clGreen;
      Canvas.Rectangle (187, 130,191,162);
      Canvas.Brush.Color:= clYellow;
      Canvas.Rectangle (209, 130,213,162);
      Canvas.Brush.Color:= clMaroon;
      Canvas.Rectangle (193,161,199,200);
      Canvas.Brush.Color:= clPurple;
      Canvas.Rectangle (201,161,207,200);
    

or shorter: 
    
    
      with Canvas do begin
        Brush.Color:= clRed;
        Ellipse(195, 117, 205, 128);
        Brush.Color:= clBlue;
        Rectangle (192, 130,208,160);
        Brush.Color:= clGreen;
        Rectangle (187, 130,191,162);
        Brush.Color:= clYellow;
        Rectangle (209, 130,213,162);
        Brush.Color:= clMaroon;
        Rectangle (193,161,199,200);
        Brush.Color:= clPurple;
        Rectangle (201,161,207,200);
      end;
    

This will give something like this: [![canvas.png](https://wiki.freepascal.org/images/d/d1/canvas.png)](</File:canvas.png>)

---

_Source: [https://wiki.freepascal.org/Drawing_with_canvas](https://web.archive.org/web/20240913051525/https://wiki.freepascal.org/Drawing_with_canvas)_
