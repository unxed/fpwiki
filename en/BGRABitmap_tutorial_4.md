# BGRABitmap tutorial 4

│ **[Deutsch (de)](</BGRABitmap_tutorial_4/de> "BGRABitmap tutorial 4/de")** │  **English (en)** │  **[español (es)](</BGRABitmap_tutorial_4/es> "BGRABitmap tutorial 4/es")** │  **[français (fr)](</BGRABitmap_tutorial_4/fr> "BGRABitmap tutorial 4/fr")** │ 

  
****

[ **Home**](<BGRABitmap_tutorial.md> "BGRABitmap tutorial") | [ **Tutorial 1**](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [ **Tutorial 2**](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") | [ **Tutorial 3**](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") |  **Tutorial 4** | [ **Tutorial 5**](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") | [ **Tutorial 6**](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") | [ **Tutorial 7**](<BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7") | [ **Tutorial 8**](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") | [ **Tutorial 10**](<BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") | [ **Tutorial 11**](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") | [ **Tutorial 12**](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") | [ **Tutorial 13**](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") | [ **Tutorial 14**](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") | [ **Tutorial 15**](<BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15") | [ **Tutorial 16**](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | Edit

This tutorial shows you how to access directly to pixels. See reference on [direct pixel access](<TBGRABitmap_class.md> "TBGRABitmap class"). 

## Contents

  * 1 Create a new project
    * 1.1 Add a painting handler
    * 1.2 Run the program
  * 2 Using HSLA colorspace
    * 2.1 Color correction
    * 2.2 Run the program
  * 3 Using TXYZABitmap



## Create a new project

Create a new project and add a reference to [BGRABitmap](<BGRABitmap.md> "BGRABitmap"), the same way as in [the first tutorial](<BGRABitmap_tutorial.md> "BGRABitmap tutorial"). 

### Add a painting handler

With the object inspector, add an OnPaint handler and write: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var 
      x, y: integer;
      p: PBGRAPixel;
      image: TBGRABitmap;
    begin
      image := TBGRABitmap.Create(ClientWidth, ClientHeight);
    
      for y := 0 to image.Height-1 do
      begin
        p := image.Scanline[y];
        for x := 0 to image.Width-1 do
        begin
          p^.red := x * 256 div image.Width;
          p^.green := y * 256 div image.Height;
          p^.blue := 0;
          p^.alpha := 255;
          inc(p);
        end;
      end;
      image.InvalidateBitmap; // changed by direct access
    
      image.Draw(Canvas, 0, 0, True);
      image.free;
    end;
    

This procedure creates a bitmap of the same size as the available client space. Then the loops access directly to pixel data to render a bidimensional gradient. Finally the image is drawn and freed. 

To access to bitmap data, you can either use Data, if you do not mind the line order, or Scanline to access to a specific line. Within a line, pixels are ordered from left to right. Each component is defined. For example: 
    
    
    p^.red := x*256 div image.Width;
    

Defines a red component varying from 0 to 255 from left to right. The maximum value image.Width is never reached by x, so the red component never reach 256. 

### Run the program

You should see a form with a gradient where corners are black, red, yellow and green. When you resize the form, the gradient is resized accordingly. 

[![BGRATutorial4.png](https://wiki.freepascal.org/images/8/8b/BGRATutorial4.png)](</File:BGRATutorial4.png>)

## Using HSLA colorspace

You can use hue, saturation, and lightness. To do this, declare a [THSLAPixel](<BGRABitmap_Pixel_types.md> "BGRABitmap Pixel types"). It's values range from 0 to 65535. To convert it to standard RGB pixel, use HSLAToBGRA. 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var x,y: integer;
        p: PBGRAPixel;
        image: TBGRABitmap;
        hsla: THSLAPixel;
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight);
      hsla.lightness := 32768;
      hsla.alpha := 65535;
      for y := 0 to image.Height-1 do
      begin
        p := image.Scanline[y];
        hsla.saturation := y*65536 div image.Height;
        for x := 0 to image.Width-1 do
        begin
          hsla.hue := x*65536 div image.Width;
          p^:= HSLAToBGRA(hsla);
          inc(p);
        end;
      end;
      image.InvalidateBitmap; // changed by direct access
    
      image.Draw(Canvas,0,0,True);
      image.free;
    end;
    

[![BGRATutorial4b.png](https://wiki.freepascal.org/images/1/1b/BGRATutorial4b.png)](</File:BGRATutorial4b.png>)

#### Color correction

HSLA colors have gamma correction, but there are other possible corrections. For example, H means "hue". In the classical version of HSLA model, each range between primary colors (red/green/blue) is represented by 120 degrees, which equals 21845 colors in [THSLAPixel](<BGRABitmap_Pixel_types.md> "BGRABitmap Pixel types"). However, we do not perceive the same differences of colors in these different ranges. The functions HtoG and GtoH apply or unapply a correction, where in the G hue, ranges are not of the same size. To get a corrected hue, write: 
    
    
    hsla.hue := GtoH(x*65536 div image.Width);
    

Notice that the range of oranges is wider. Finally, in the HSLA model, the lightness L does not correspond to the perceived lightness. Instead of only correcting the hue, you can use GSBAToBGRA and BGRAToGSBA instead of HLSAToBGRA and BGRAToHSLA. The G here means that the hue is automatically corrected and the B means it's the perceived lightness (sometimes called Brightness) which is taken into account. So you don't need to call GtoH and HtoG explicitely. 

The code simple becomes: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var x,y: integer;
        p: PBGRAPixel;
        image: TBGRABitmap;
        hsla: THSLAPixel;
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight);
      hsla.lightness := 32768;
      hsla.alpha := 65535;
      for y := 0 to image.Height-1 do
      begin
        p := image.Scanline[y];
        hsla.saturation := y*65536 div image.Height;
        for x := 0 to image.Width-1 do
        begin
          hsla.hue := x*65536 div image.Width;
          p^:= GSBAToBGRA(hsla);
          inc(p);
        end;
      end;
      image.InvalidateBitmap; // changed by direct access
    
      image.Draw(Canvas,0,0,True);
      image.free;
    end;
    

Note that the THSLAPixel type is used, whatever the correction of the color model. 

### Run the program

With full color correction, the gradient is more progressive, and looks like what you would obtain with the Lab color model. 

[![BGRATutorial4c.png](https://wiki.freepascal.org/images/a/a6/BGRATutorial4c.png)](</File:BGRATutorial4c.png>)

## Using TXYZABitmap

Instead of converting directly the values of pixels, it is possible to use a bitmap with another pixel format. Here is how to use the XYZA colorspace. 

The TXYZA type contains floating point values from 0 to 1. In the example, the Y value (lightness) is set to 0.5 while the X and Z values varies to express different hues. 

The X and Z channels can be greater than one depending on the illuminant and typically in daylight the Z value for the brightest white is slightly greater than 1. In theory the Y channel can be greater than one as well if it expresses a light source stronger than the illuminant, but that will saturate when rendering it on a screen. 

Also here the GetCanvasScaleFactor function (value 1 or greater) is used to determine if there is a Retina display for which the coordinates can be double the pixel size. So in this case, we need to prepare a bitmap that is twice as big and in the end draw it zoomed out. 

To render on screen, the bitmap needs to be drawn on a BGRA bitmap. 
    
    
    uses BGRABitmap, BGRABitmapTypes, XYZABitmap; 
    
    procedure TForm1.FormPaint(Sender: TObject);
    var x,y: integer;
        image: TBGRABitmap;
        xyzImage: TXYZABitmap;
        p: PXYZA;
        xyz: TXYZA;
    begin
      // create a bitmap with XYZA colorspace
      // and take into account the scale factor (for MacOS)
      xyzImage := TXYZABitmap.Create(round(ClientWidth * GetCanvasScaleFactor),
                    round(ClientHeight * GetCanvasScaleFactor));
      xyz.alpha := 1;
      xyz.Y := 0.5;
      for y := 0 to xyzImage.Height-1 do
      begin
        p := xyzImage.Scanline[y];
        xyz.x := 0.35 + y/xyzImage.Height*0.3;
        for x := 0 to xyzImage.Width-1 do
        begin
          xyz.z := 0.15 + x/xyzImage.Width*0.5;
          p^:= xyz;
          inc(p);
        end;
      end;
      xyzImage.InvalidateBitmap; // changed by direct access
    
      // draw the XYZA bitmap on a BGRA bitmap for display
      image := TBGRABitmap.Create(xyzImage.Width, xyzImage.Height);
      image.PutImage(0,0, xyzImage, dmSet);
      // stretch (reduce) because of scale factor
      image.Draw(Canvas, rect(0,0,ClientWidth,ClientHeight), True); 
      image.free;
    end;
    

[![BGRATutorial4d.png](https://wiki.freepascal.org/images/4/41/BGRATutorial4d.png)](</File:BGRATutorial4d.png>)

[Previous tutorial (drawing with the mouse)](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") | [Next tutorial (layers and masks)](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5")

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial_4](https://web.archive.org/web/20241213033559/https://wiki.freepascal.org/BGRABitmap_tutorial_4)_
