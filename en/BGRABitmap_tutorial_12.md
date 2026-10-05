# BGRABitmap tutorial 12

│ **English (en)** │

  
****

[ **Home**](<BGRABitmap_tutorial.md> "BGRABitmap tutorial") | [ **Tutorial 1**](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [ **Tutorial 2**](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") | [ **Tutorial 3**](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") | [ **Tutorial 4**](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") | [ **Tutorial 5**](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") | [ **Tutorial 6**](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") | [ **Tutorial 7**](<BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7") | [ **Tutorial 8**](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") | [ **Tutorial 10**](<BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") | [ **Tutorial 11**](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") |  **Tutorial 12** | [ **Tutorial 13**](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") | [ **Tutorial 14**](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") | [ **Tutorial 15**](<BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15") | [ **Tutorial 16**](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | Edit

This tutorial shows how to use the text functions. For advanced features, see [font renderers](<BGRABitmap_tutorial_Font_rendering.md> "BGRABitmap tutorial Font rendering"). 

## Contents

  * 1 Create a new project
  * 2 Simple text
    * 2.1 Using alignment
  * 3 Rotated text
  * 4 Word wrapped text
  * 5 Text with shadow
  * 6 Text with gradient



### Create a new project

Create a new project and add a reference to [BGRABitmap](<BGRABitmap.md> "BGRABitmap"), the same way as in [the first tutorial](<BGRABitmap_tutorial.md> "BGRABitmap tutorial"). 

### Simple text

You can draw a simple text like this: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      image: TBGRABitmap;
      c: TBGRAPixel;
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight, ColorToBGRA(ColorToRGB(clBtnFace)) );
      c := ColorToBGRA(ColorToRGB(clBtnText)); //retrieve default text color
    
      image.FontHeight := 30;
      image.FontAntialias := true;
      image.FontStyle := [fsBold];
      image.TextOut (5, 5, 'Hello world', c);
      image.SetPixel (5, 5, c);
    
      image.free;
    end;
    

Here the font size is set to 30 pixels, with antialiasing. Using font antialiasing is slower but more beautiful. 

The upper-left corner of the text is at (5,5). This origin is shown with a SetPixel. 

[![BGRATutorial12a.png](https://wiki.freepascal.org/images/4/40/BGRATutorial12a.png)](</File:BGRATutorial12a.png>)

#### Using alignment

Just replace the TextOut and SetPixel lines by: 
    
    
      image.TextOut(ClientWidth-5,5,'Hello world',c,taRightJustify);   
      image.SetPixel(ClientWidth-5,5,c);
    

Now the origin is on the right side of the form, and the text is aligned to the right. 

[![BGRATutorial12b.png](https://wiki.freepascal.org/images/6/61/BGRATutorial12b.png)](</File:BGRATutorial12b.png>)

### Rotated text

It is also simple to draw rotated text. To do this use TextOutAngle or set FontOrientation property: 
    
    
      image.TextOutAngle(30,5,-450,'Hello world',c, taLeftJustify);
      image.SetPixel(30,5,c);
    

The angle is in tenth of degrees and positive value means counter clock wise. 

[![BGRATutorial12c.png](https://wiki.freepascal.org/images/b/b5/BGRATutorial12c.png)](</File:BGRATutorial12c.png>)

Notice where the text origin is (the added pixel). 

### Word wrapped text

There is an easy to use version of TextRect: 
    
    
      image.TextRect(rect(5,5,ClientWidth-5,ClientHeight-5),'This is a text that should be word wrapped',taCenter,tlCenter,c);
      image.Rectangle(rect(5,5,ClientWidth-5,ClientHeight-5),c,dmSet);
    

Parameters are: 

  * the bounding rectangle
  * the text
  * horizontal alignment
  * vertical alignment
  * color



[![BGRATutorial12d.png](https://wiki.freepascal.org/images/c/ce/BGRATutorial12d.png)](</File:BGRATutorial12d.png>)

### Text with shadow

You can make a text shadow with a blur effect: 
    
    
    var
      image,txt: TBGRABitmap;
      grad: TBGRAGradientScanner;
      c: TBGRAPixel;
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight, ColorToBGRA(ColorToRGB(clBtnFace)) );
      c := ColorToBGRA(ColorToRGB(clBtnText));
    
      txt := TextShadow(ClientWidth,ClientHeight,'Hello world',30,c,BGRABlack,5,5,5);
      image.PutImage(0,0,txt,dmDrawWithTransparency);
      txt.Free;
    
      image.Draw(Canvas,0,0,True);
      image.free;
    end;
    

The procedure TextShadow creates a bitmap that contains the text with a shadow. The parameters are: 

  * The size of the bitmap
  * The text
  * Font height
  * Font color
  * Shadow color
  * Shadow offset and blur size



Do not forget to free the bitmap after using it. 

[![BGRATutorial12e.png](https://wiki.freepascal.org/images/b/b7/BGRATutorial12e.png)](</File:BGRATutorial12e.png>)

### Text with gradient

Like other drawing functions, you can supply a gradient or a texture to fill the text with. Here is an example: 
    
    
    uses BGRAGradientScanner;
    
    var
      image: TBGRABitmap;
      grad: TBGRAGradientScanner;
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight, ColorToBGRA(ColorToRGB(clBtnFace)) );
    
      grad := TBGRAGradientScanner.Create(BGRA(255,255,0),BGRA(255,0,0),gtLinear,PointF(0,0),PointF(0,35),True,True);
      image.FontHeight := 30;
      image.FontAntialias := true;
      image.FontStyle := [fsBold];
      image.TextOut(6,6,'Hello world',BGRABlack);  //draw a black border
      image.TextOut(5,5,'Hello world',grad);       //draw gradient text 
      grad.free;
    
      image.Draw(Canvas,0,0,True);
      image.free;
    end;
    

First a horizontal sine gradient is created, with color yellow and red. It is then used as a texture. 

[![BGRATutorial12f.png](https://wiki.freepascal.org/images/7/7e/BGRATutorial12f.png)](</File:BGRATutorial12f.png>)

[Previous tutorial (combining transformations)](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") [Next tutorial (coordinate system)](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13")

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial_12](https://web.archive.org/web/20250409130251/https://wiki.freepascal.org/BGRABitmap_tutorial_12)_
