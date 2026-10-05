# BGRABitmap tutorial 10

│ **English (en)** │

  
****

[ **Home**](<BGRABitmap_tutorial.md> "BGRABitmap tutorial") | [ **Tutorial 1**](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [ **Tutorial 2**](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") | [ **Tutorial 3**](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") | [ **Tutorial 4**](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") | [ **Tutorial 5**](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") | [ **Tutorial 6**](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") | [ **Tutorial 7**](<BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7") | [ **Tutorial 8**](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") |  **Tutorial 10** | [ **Tutorial 11**](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") | [ **Tutorial 12**](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") | [ **Tutorial 13**](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") | [ **Tutorial 14**](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") | [ **Tutorial 15**](<BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15") | [ **Tutorial 16**](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | Edit

This tutorial shows how to use texture mapping. A sample project can be found in [BGRABitmap AggPas](<BGRABitmap_AggPas.md> "BGRABitmap AggPas"). 

## Contents

  * 1 Create a new project
  * 2 Using no particular mapping
  * 3 Run the program
  * 4 Affine transformation
  * 5 Run the program
  * 6 Texture mapping
    * 6.1 Linear mapping
    * 6.2 Run the program
    * 6.3 Perspective mapping
    * 6.4 Run the program
  * 7 Conclusion



### Create a new project

Create a new project and add a reference to [BGRABitmap](<BGRABitmap.md> "BGRABitmap"), the same way as in [the first tutorial](<BGRABitmap_tutorial.md> "BGRABitmap tutorial"). 

### Using no particular mapping

Let's see what happens if we draw a polygon with a texture using default mapping: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var image: TBGRABitmap;
        tex: TBGRABitmap;
    begin
      //black background
      image := TBGRABitmap.Create(ClientWidth,ClientHeight, BGRABlack );
    
      tex:= TBGRABitmap.Create('image.png'); //load a bitmap
      image.FillPolyAntialias( [PointF(110,10), PointF(250,10), PointF(350,160), PointF(10,160)], tex);
      tex.Free;
    
      image.Draw(Canvas,0,0,True); //draw on the screen
      image.free;
    end;
    

### Run the program

You should obtain something like this: 

[![BGRATutorial10a.png](https://wiki.freepascal.org/images/7/7e/BGRATutorial10a.png)](</File:BGRATutorial10a.png>)

As you can see the image is not deformed. 

### Affine transformation

We can apply an affine transformation like this: 
    
    
    uses BGRABitmap, BGRABitmapTypes, BGRATransform;
    
    procedure TForm1.PaintImage;
    var image: TBGRABitmap;
        tex: TBGRABitmap;
        affine: TBGRAAffineBitmapTransform;
    
    begin
      //black background
      image := TBGRABitmap.Create(ClientWidth,ClientHeight, BGRABlack );
    
      tex:= TBGRABitmap.Create('image.png'); //load a bitmap
    
      //create a rotation of 45°
      affine := TBGRAAffineBitmapTransform.Create(tex,True);
      affine.RotateDeg(45);
    
      //use this transformation as parameter instead of tex
      image.FillPolyAntialias( [PointF(110,10), PointF(250,10), PointF(350,160), PointF(10,160)], affine); 
    
      affine.Free;
      tex.Free;
    
      image.Draw(Canvas,0,0,True); //draw on the screen
      image.free;
    end;
    

### Run the program

You should obtain a rotated picture in the polygon: 

[![BGRATutorial10b.png](https://wiki.freepascal.org/images/2/2a/BGRATutorial10b.png)](</File:BGRATutorial10b.png>)

### Texture mapping

Now, if we want the texture to be aligned with the polygon border, we can use texture mapping. 

#### Linear mapping

Linear mapping stretched the image linearly along the borders. To do this: 
    
    
    procedure TForm1.PaintImage;
    var image: TBGRABitmap;
        tex: TBGRABitmap;
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight, BGRABlack );
    
      tex:= TBGRABitmap.Create('image.png');
      image.FillPolyLinearMapping( [PointF(110,10), PointF(250,10), PointF(350,160), PointF(10,160)], tex,
                 [PointF(0,0), PointF(tex.width-1,0), PointF(tex.Width-1,tex.Height-1), PointF(0,tex.Height-1)], true);
      tex.Free;
    
      image.Draw(Canvas,0,0,True);
      image.free;
    end;
    

To do the mapping, we use FillPolyLinearMapping. Some new parameters appear. Texture coordinates define, for each point of the polygon, the location in the texture. Interpolation option is used for best quality. 

#### Run the program

Now the texture is deformed according to the polygonal shape. 

[![BGRATutorial10c.png](https://wiki.freepascal.org/images/f/f3/BGRATutorial10c.png)](</File:BGRATutorial10c.png>)

#### Perspective mapping

The perspective mapping allow to change the depth of each point. 
    
    
    procedure TForm1.PaintImage;
    var image: TBGRABitmap;
        tex: TBGRABitmap;
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight, BGRABlack );
    
      tex:= TBGRABitmap.Create('image.png');
      image.FillPolyPerspectiveMapping( [PointF(110,10), PointF(250,10), PointF(350,160), PointF(10,160)],
                                        [75,             75,             50,              50],
           tex, [PointF(0,0), PointF(tex.width-1,0), PointF(tex.Width-1,tex.Height-1), PointF(0,tex.Height-1)], true);
      tex.Free;
    
      image.Draw(Canvas,0,0,True);
      image.free;
    end;
    

Here the depth is 75 for the top of the polygon and 50 for the bottom of the polygon. It means that the bottom is closer to the observer, as if it were horizontal, like a floor. 

#### Run the program

Now it seems that it is a 3D polygon: 

[![BGRATutorial10d.png](https://wiki.freepascal.org/images/6/67/BGRATutorial10d.png)](</File:BGRATutorial10d.png>)

### Conclusion

Using these techniques, it is possible to deform an image, like in LazPaint tool "grid deformation", or to render 3D objects with textures, like in tests 19-21 of testbgrafunc (also in LazPaint archive). 

[Previous tutorial (phong shading)](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") [Next tutorial (combining transformations)](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11")

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial_10](https://web.archive.org/web/20240714075109/https://wiki.freepascal.org/BGRABitmap_tutorial_10)_
