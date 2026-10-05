# BGRABitmap tutorial 13

[ **Deutsch (de)**](</BGRABitmap_tutorial_13/de> "BGRABitmap tutorial 13/de") | ****English (en)**** | [**Français (fr)**](</index.php?title=BGRABitmap_tutorial_13/fr&action=edit&redlink=1> "BGRABitmap tutorial 13/fr \(page does not exist\)") | [**Español (es)**](</index.php?title=BGRABitmap_tutorial_13/es&action=edit&redlink=1> "BGRABitmap tutorial 13/es \(page does not exist\)") | [Edit](<http://wiki.lazarus.freepascal.org/index.php?title=Template:BGRABitmap_tutorial_13&action=edit>)   


[ **Home**](<BGRABitmap_tutorial.md> "BGRABitmap tutorial") | [ **Tutorial 1**](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [ **Tutorial 2**](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") | [ **Tutorial 3**](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") | [ **Tutorial 4**](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") | [ **Tutorial 5**](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") | [ **Tutorial 6**](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") | [ **Tutorial 7**](<BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7") | [ **Tutorial 8**](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") | [ **Tutorial 10**](<BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") | [ **Tutorial 11**](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") | [ **Tutorial 12**](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") | ****Tutorial 13**** | [ **Tutorial 14**](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") | [ **Tutorial 15**](<BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15") | [ **Tutorial 16**](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | [Edit](<http://wiki.lazarus.freepascal.org/index.php?title=Template:BGRABitmap_tutorial_index&action=edit>)

This tutorial talks about the coordinate system of BGRABitmap. 

## Contents

  * 1 Pixel coordinates
  * 2 Floating coordinates
    * 2.1 Top-left distance floating point coordinates
    * 2.2 Pixel-center floating point coordinates
  * 3 Create a new project
  * 4 Canvas and BGRACanvas
  * 5 Subtle considerations
    * 5.1 Filled shapes with canvas



### Pixel coordinates

Standard canvas routines use integer coordinates. This is also the case of the CanvasBGRA property, which emulates standard canvas functions, but with antialiasing (AntialiasingMode), alphablending (Opacity property of Pen, Brush and Font) and gamma correction. 

When using pure integer coordinates without antialiasing, the coordinates specify a pixel position, i.e. a square. So when drawing a line from (0,0) to (5,5), the top-left pixel is the first painted pixel. In standard canvas, the last pixel is not painted, so the end of the line will be at (4,4). 

[![BGRATutorial13a.png](https://wiki.freepascal.org/images/a/a3/BGRATutorial13a.png)](</File:BGRATutorial13a.png>)

When drawing an ellipse, the bounding rectangle specifies the pixels that will be used to render the ellipse. Once again, in standard canvas, the lower-right coordinates are excluded from the drawing, so filling the rectangle (0,0)-(5,5) will in fact fill pixels having coordinates from 0 to 4. 

### Floating coordinates

Now, when working with floating point values, the coordinates specify a position that can be anywhere on the pixels. The width value of the pen means really a distance, and not a number of pixels. So what does (0.0,0.0) mean ? 

#### Top-left distance floating point coordinates

It can be for example the distance from the top-left corner (this is not the case of BGRABitmap). In this case, this coordinate would be the upper-left corner of the first pixel. But if we do so, the behavior is slightely different between integer coordinates and floating point coordinates. Indeed, imagine we draw an horizontal segment on pixels (0,1) to (4,1). In pixel coordinates it would be the line (0,1)-(5,1) and the width would be 1 pixel. Now if we want to define this segment with floating point coordinates. The left side would be at 0.0, the right at 5.0, that's ok. The top side would be at 1.0 and the bottom at 2.0. Here is the problem. The center of the line is at a vertical coordinate of 1.5. So to draw this line, we should supply coordinates (0.0,1.5)-(5.0,1.5). 

[![BGRATutorial13b.png](https://wiki.freepascal.org/images/1/18/BGRATutorial13b.png)](</File:BGRATutorial13b.png>)

In fact every 1-pixel wide line with integer coordinates would be drawn between pixels, and with antialiasing, this leads to blurred lines. What appeared to be ok with horizontal coordinates is in fact an illusion, because if the line has caps, the correct coordinates are (0.5,1.5)-(4.5,1.5). If we ignore the last pixel issue, we see that these coordinates are simply 0.5 greater. 

#### Pixel-center floating point coordinates

The coordinates can be the distance from the center of the top-left pixel. In other words, integer values are at the center of the pixels.This is the case of BGRABitmap functions. Using these coordinates, the line that fills pixels (0,1) to (4,1) is simply (0.0,1.0)-(4.0,1.0). 

[![BGRATutorial13c.png](https://wiki.freepascal.org/images/6/6c/BGRATutorial13c.png)](</File:BGRATutorial13c.png>)

This is strange from a mathematical point of view, but very convenient, because you can use integer coordinates to draw normal 1-pixel wide lines. This way, there is little difference between calls to CanvasBGRA functions and BGRABitmap normal floating point functions. 

### Create a new project

Create a new project and add a reference to [BGRABitmap](<BGRABitmap.md> "BGRABitmap"), the same way as in [the first tutorial](<BGRABitmap_tutorial.md> "BGRABitmap tutorial"). 

### Canvas and BGRACanvas

Let's try to show what happens at the pixel level. 

First, we'll use the standard Canvas : 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var image: TBGRABitmap;
    begin
      image := TBGRABitmap.Create(10,10);
      with image.Canvas do
      begin
        //clear with white
        brush.color := clWhite;
        FillRect(0,0,image.width,image.height);
        //blue ellipse with black border
        brush.style := bsClear;
        pen.color := clBlack;  
        Ellipse(0,0,9,9);
      end;
      /stretch the image so we can see the pixels
      BGRAReplace(image,image.Resample(image.Width*10,image.Height*10,rmSimpleStretch));
      image.Draw(Canvas,0,0,True);
      image.free;
    end;

The resample option rmSimpleStretch prevents to use interpolation filters. 

You should obtain this : 

[![BGRATutorial13d.png](https://wiki.freepascal.org/images/7/75/BGRATutorial13d.png)](</File:BGRATutorial13d.png>)

The pixels of coordinate 9 are not filled as explained above about rectangle bounds. Now, let's draw this BGRACanvas. Just change the 'with' line : 
    
    
    with image.CanvasBGRA do

Now you should obtain this : 

[![BGRATutorial13e.png](https://wiki.freepascal.org/images/b/b0/BGRATutorial13e.png)](</File:BGRATutorial13e.png>)

The result is very similar except with antialiasing. 

### Subtle considerations

Suppose we want to fill an antialiased ellipse that fit exactly in a 9x9 bitmap. We could try with this code : 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var image: TBGRABitmap;
    begin
      image := TBGRABitmap.Create(9,9,BGRAWhite);
      image.FillEllipseAntialias(4,4, 4,4, BGRABlack);  
      BGRAReplace(image,image.Resample(image.Width*10,image.Height*10,rmSimpleStretch));
      image.Draw(Canvas,0,0,True);
      image.free;
    end;

We obtain : 

[![BGRATutorial13f.png](https://wiki.freepascal.org/images/2/25/BGRATutorial13f.png)](</File:BGRATutorial13f.png>)

As you can see, the border is not completely filled. The ellipse is smaller than could be expected. In fact, the center of the ellipse is (4,4) so the left border is at (0,4). But remember it is the center of the pixel. So if we want the ellipse to go to the border of the pixel, we need to add 0.5 to the radius : 
    
    
      image.FillEllipseAntialias(4,4, 4.5,4.5, BGRABlack);

[![BGRATutorial13g.png](https://wiki.freepascal.org/images/4/46/BGRATutorial13g.png)](</File:BGRATutorial13g.png>)

#### Filled shapes with canvas

Note that with standard canvas, the result is very surprising. Suppose we want to do the same thing. We could try this : 
    
    
        brush.color := clWhite;
        FillRect(0,0,image.width,image.height);
        brush.color := clBlue;
        pen.style := psClear;
        Ellipse(0,0,9,9);

We obtain this : 

[![BGRATutorial13h.png](https://wiki.freepascal.org/images/e/eb/BGRATutorial13h.png)](</File:BGRATutorial13h.png>)

So in this case, there is yet another pixel substracted. The ellipse is 8 pixel wide where as the supplied rectangle is 9 pixel wide. 

[Previous tutorial (text functions)](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") [Next tutorial (canvas2D)](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14")

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial_13](https://web.archive.org/web/20181218073125/https://wiki.freepascal.org/BGRABitmap_tutorial_13)_
