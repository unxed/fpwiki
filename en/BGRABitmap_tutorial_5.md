# BGRABitmap tutorial 5

│ **English (en)** │

  
****

[ **Home**](<BGRABitmap_tutorial.md> "BGRABitmap tutorial") | [ **Tutorial 1**](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [ **Tutorial 2**](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") | [ **Tutorial 3**](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") | [ **Tutorial 4**](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") |  **Tutorial 5** | [ **Tutorial 6**](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") | [ **Tutorial 7**](<BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7") | [ **Tutorial 8**](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") | [ **Tutorial 10**](<BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") | [ **Tutorial 11**](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") | [ **Tutorial 12**](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") | [ **Tutorial 13**](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") | [ **Tutorial 14**](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") | [ **Tutorial 15**](<BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15") | [ **Tutorial 16**](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | Edit

This tutorial shows you how to use layers and masks. 

The first part shows how to do without a multi-layer image. 

The second part of the tutorial shows how to adapt the code to use _TBGRALayeredBitmap_ , which is located in the _BGRALayers_ unit. 

## Contents

  * 1 Without using a multi-layer image
    * 1.1 Create a new project
    * 1.2 About masks
    * 1.3 Erasing parts of an image
    * 1.4 Add a painting handler
    * 1.5 Run the program
    * 1.6 Add another layer with a sun
    * 1.7 Run the program
    * 1.8 Add a light layer
    * 1.9 Resulting code
    * 1.10 Run the program
  * 2 Using multi-layer images
    * 2.1 Some adjustments to make
    * 2.2 Resulting Code with TBGRALayeredBitmap
    * 2.3 Saving the image in an OpenRaster file



## Without using a multi-layer image

### Create a new project

Create a new project and add a reference to [BGRABitmap](<BGRABitmap.md> "BGRABitmap"), the same way as in [the first tutorial](<BGRABitmap_tutorial.md> "BGRABitmap tutorial"). 

### About masks

A mask is a grayscale image. When a mask is applied on an image, the parts of the image that overlap with black parts of the mask are erased and become transparent, whereas parts that overlap with white parts of the mask are kept. In other words, the mask is like an alpha channel the defines opacity. If the mask value is zero, then it becomes transparent, and if a mask value is 255, it becomes opaque. 

[![BGRATutorial5d.png](https://wiki.freepascal.org/images/0/05/BGRATutorial5d.png)](</File:BGRATutorial5d.png>)

In this example, the image, the start image is in the upper-left corner, the mask in the upper-right one, and the result when applying the mask in the lower-left corner. 

Code in OnPaint: 
    
    
    var temp,tex,mask: TBGRABitmap;
    begin
      temp:= TBGRABitmap.Create(640,480,ColorToBGRA(ColorToRGB(clBtnFace)));
    
      //loading and scaling texture
      tex := TBGRABitmap.Create('texture.png');
      BGRAReplace(tex,tex.Resample(128,80));
    
      //show image in the upper-left corner
      temp.PutImage(10,10,tex,dmDrawWithTransparency);
    
      //create a mask with ellipse and rectangle
      mask := TBGRABitmap.Create(128,80,BGRABlack);
      mask.FillEllipseAntialias(40,40,30,30,BGRAWhite);
      mask.FillRectAntialias(60,40,100,70,BGRAWhite);
    
      //show mask in the upper-right corner
      temp.PutImage(150,10,mask,dmDrawWithTransparency);
    
      //apply the mask to the image
      tex.ApplyMask(mask);
    
      //show the result image in the lower-left corner
      temp.PutImage(10,100,tex,dmDrawWithTransparency);
    
      mask.Free;
      tex.Free;
    
      //show everything on the screen
      temp.Draw(Canvas,0,0,True);
      temp.Free;
    end;
    

### Erasing parts of an image

Some functions allow to erase an ellipse, a rectangle etc. It means that the part of the image becomes transparent. It is thus possible to draw a hole in an image. If the alpha parameter is 255, the hole is completely transparent. If not, the hole is half-transparent. 

[![BGRATutorial5e.png](https://wiki.freepascal.org/images/6/62/BGRATutorial5e.png)](</File:BGRATutorial5e.png>)

Here an ellipse is erased on the left with an alpha parameter of 255, and another ellipse is erased on the right with an alpha parameter of 128. 

Code in OnPaint: 
    
    
    var image,tex: TBGRABitmap;
    begin
      image := TBGRABitmap.Create(640,480,ColorToBGRA(ColorToRGB(clBtnFace)));
    
      //load and scale texture
      tex := TBGRABitmap.Create('texture.png');
      BGRAReplace(tex,tex.Resample(128,80));
    
      //show image
      image.PutImage(10,10,tex,dmDrawWithTransparency);
    
      //erase parts
      tex.EraseEllipseAntialias(40,40,30,30,255);
      tex.EraseEllipseAntialias(80,40,30,30,128);
    
      //show result
      image.PutImage(10,100,tex,dmDrawWithTransparency);
    
      tex.Free;
    
      //show everything on the screen
      image.Draw(Canvas,0,0,True);
      image.Free;
    end;
    

### Add a painting handler

With the object inspector, add an OnPaint handler and write: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var image: TBGRABitmap;
        size: single;
    
      procedure DrawMoon;
      var layer: TBGRABitmap;
      begin
        layer := TBGRABitmap.Create(image.Width,image.Height);
        layer.FillEllipseAntialias(layer.Width/2,layer.Height/2,size*0.4,size*0.4,BGRA(224,224,224,128));
        layer.EraseEllipseAntialias(layer.Width/2+size*0.15,layer.Height/2,size*0.3,size*0.3,255);
        image.PutImage(0,0,layer,dmDrawWithTransparency);
        layer.Free;
      end;
    
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight);
    
      //Compute available space in both directions
      if image.Height < image.Width then
        size := image.Height
      else
        size := image.Width;
    
      image.GradientFill(0,0,image.Width,image.Height,
                         BGRA(128,192,255),BGRA(0,0,255),
                         gtLinear,PointF(0,0),PointF(0,image.Height),
                         dmSet);
    
      DrawMoon;
    
      image.Draw(Canvas,0,0,True);
      image.free;
    end;
    

The procedure creates an image and fills it with a blue gradient. This is the background layer. 

The procedure DrawMoon creates a layer, draws a moon in it. First a white disk is drawn, then a smaller disk is subtracted. Finally, this layer is merged with the background. 

### Run the program

You should see a blue sky with a moon. When you resize the form, the image is resized accordingly. 

[![Tutorial5a.png](https://wiki.freepascal.org/images/1/15/Tutorial5a.png)](</File:Tutorial5a.png>)

### Add another layer with a sun

In the OnPaint event, add the following subprocedure: 
    
    
      procedure DrawSun;
      var layer,mask: TBGRABitmap;
      begin
        layer := TBGRABitmap.Create(image.Width,image.Height);
        layer.GradientFill(0,0,layer.Width,layer.Height,
                           BGRA(255,255,0),BGRA(255,0,0),
                           gtRadial,PointF(layer.Width/2,layer.Height/2-size*0.15),PointF(layer.Width/2+size*0.45,layer.Height/2-size*0.15),
                           dmSet);
        mask := TBGRABitmap.Create(layer.Width,layer.Height,BGRABlack);
        mask.FillEllipseAntialias(layer.Width/2+size*0.15,layer.Height/2,size*0.25,size*0.25,BGRAWhite);
        layer.ApplyMask(mask);
        mask.Free;
        image.PutImage(0,0,layer,dmDrawWithTransparency);
        layer.Free;
      end;
    

This procedures creates a radial gradient of red and orange and applies a circular mask to it. This results in a colored disk. Finally, the layer is merged with the background. 

Add a call to this procedure to draw it after the moon. 

### Run the program

You should see a blue sky with a moon and a sun. When you resize the form, the image is resized accordingly. 

[![BGRATutorial5b.png](https://wiki.freepascal.org/images/9/9b/BGRATutorial5b.png)](</File:BGRATutorial5b.png>)

### Add a light layer

Add the following subprocedure in the OnPaint event: 
    
    
      procedure ApplyLight;
      var layer: TBGRABitmap;
      begin
        layer := TBGRABitmap.Create(image.Width,image.Height);
        layer.GradientFill(0,0,layer.Width,layer.Height,
                           BGRA(255,255,255),BGRA(64,64,64),
                           gtRadial,PointF(layer.Width*5/6,layer.Height/2),PointF(layer.Width*1/3,layer.Height/4),
                           dmSet);
        image.BlendImage(0,0,layer,boMultiply);
        layer.Free;
      end;
    

This procedure draws a layer with a white radial gradient. It is then applied to multiply the image. 

### Resulting code
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var image: TBGRABitmap;
        size: single;
    
      procedure DrawMoon;
      var layer: TBGRABitmap;
      begin
        layer := TBGRABitmap.Create(image.Width,image.Height);
        layer.FillEllipseAntialias(layer.Width/2,layer.Height/2,size*0.4,size*0.4,BGRA(224,224,224,128));
        layer.EraseEllipseAntialias(layer.Width/2+size*0.15,layer.Height/2,size*0.3,size*0.3,255);
        image.PutImage(0,0,layer,dmDrawWithTransparency);
        layer.Free;
      end;
    
      procedure DrawSun;
      var layer,mask: TBGRABitmap;
      begin
        layer := TBGRABitmap.Create(image.Width,image.Height);
        layer.GradientFill(0,0,layer.Width,layer.Height,
                           BGRA(255,255,0),BGRA(255,0,0),
                           gtRadial,PointF(layer.Width/2,layer.Height/2-size*0.15),PointF(layer.Width/2+size*0.45,layer.Height/2-size*0.15),
                           dmSet);
        mask := TBGRABitmap.Create(layer.Width,layer.Height,BGRABlack);
        mask.FillEllipseAntialias(layer.Width/2+size*0.15,layer.Height/2,size*0.25,size*0.25,BGRAWhite);
        layer.ApplyMask(mask);
        mask.Free;
        image.PutImage(0,0,layer,dmDrawWithTransparency);
        layer.Free;
      end;
    
      procedure ApplyLight;
      var layer: TBGRABitmap;
      begin
        layer := TBGRABitmap.Create(image.Width,image.Height);
        layer.GradientFill(0,0,layer.Width,layer.Height,
                           BGRA(255,255,255),BGRA(64,64,64),
                           gtRadial,PointF(layer.Width*5/6,layer.Height/2),PointF(layer.Width*1/3,layer.Height/4),
                           dmSet);
        image.BlendImage(0,0,layer,boMultiply);
        layer.Free;
      end;
    
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight);
    
      if image.Height < image.Width then
        size := image.Height
      else
        size := image.Width;
    
      image.GradientFill(0,0,image.Width,image.Height,
                         BGRA(128,192,255),BGRA(0,0,255),
                         gtLinear,PointF(0,0),PointF(0,image.Height),
                         dmSet);
    
      DrawMoon;
      DrawSun;
      ApplyLight;
    
      image.Draw(Canvas,0,0,True);
      image.free;
    end;
    

### Run the program

You should see a blue sky with a moon and a sun, with a light effect. When you resize the form, the image is resized accordingly. 

[![Tutorial5c.png](https://wiki.freepascal.org/images/f/f3/Tutorial5c.png)](</File:Tutorial5c.png>)

## Using multi-layer images

The "BGRALayers" unit provides a class "TBGRALayeredBitmap" which allows storing a multilayer image. You will use it to save your drawing in OpenRaster format. 

### Some adjustments to make

First, the result will be stored not in a "TBGRABitmap" but in a "TBGRALayeredBitmap": 
    
    
    var image: TBGRABitmap;
    
    ...
    
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight);
    
    ...
    
      image.Draw(Canvas,0,0); // there is no Opaque parameter
      image.free;
    end;
    

Then, the procedures that add elements should not draw directly on the image, but add layers. Thus, the following code: 
    
    
        image.PutImage(0,0,layer,dmDrawWithTransparency);
        layer.Free;
    

becomes: 
    
    
        image.AddOwnedLayer(layer);
    

And for lighting, the following code: 
    
    
        image.BlendImage(0,0,layer,boMultiply);
        layer.Free;
    

becomes: 
    
    
        image.AddOwnedLayer(layer,boMultiply);
    

The background must now also be a layer. Add a new procedure then: 
    
    
      procedure CreateBackground;
      var layer: TBGRABitmap;
      begin
        layer := TBGRABitmap.Create(image.Width,image.Height);
        layer.GradientFill(0,0,image.Width,image.Height,
                           BGRA(128,192,255),BGRA(0,0,255),
                           gtLinear,PointF(0,0),PointF(0,image.Height),
                           dmSet);
        image.AddOwnedLayer(layer);
      end;
    

### Resulting Code with TBGRALayeredBitmap

By moving the creation of the multilayer image into a separate function, "CreateMyImage", we get: 
    
    
    function CreateMyImage(AWidth,AHeight: integer): TBGRALayeredBitmap;
    var image: TBGRALayeredBitmap;
        size: single;
    
      procedure CreateMoon;
      var layer: TBGRABitmap;
      begin
        layer := TBGRABitmap.Create(image.Width,image.Height);
        layer.FillEllipseAntialias(layer.Width/2,layer.Height/2,size*0.4,size*0.4,BGRA(224,224,224,128));
        layer.EraseEllipseAntialias(layer.Width/2+size*0.15,layer.Height/2,size*0.3,size*0.3,255);
        image.AddOwnedLayer(layer);
      end;
    
      procedure CreateSun;
      var layer,mask: TBGRABitmap;
      begin
        layer := TBGRABitmap.Create(image.Width,image.Height);
        layer.GradientFill(0,0,layer.Width,layer.Height,
                           BGRA(255,255,0),BGRA(255,0,0),
                           gtRadial,PointF(layer.Width/2,layer.Height/2-size*0.15),PointF(layer.Width/2+size*0.45,layer.Height/2-size*0.15),
                           dmSet);
        mask := TBGRABitmap.Create(layer.Width,layer.Height,BGRABlack);
        mask.FillEllipseAntialias(layer.Width/2+size*0.15,layer.Height/2,size*0.25,size*0.25,BGRAWhite);
        layer.ApplyMask(mask);
        mask.Free;
        image.AddOwnedLayer(layer);
      end;
    
      procedure CreateLight;
      var layer: TBGRABitmap;
      begin
        layer := TBGRABitmap.Create(image.Width,image.Height);
        layer.GradientFill(0,0,layer.Width,layer.Height,
                           BGRA(255,255,255),BGRA(64,64,64),
                           gtRadial,PointF(layer.Width*5/6,layer.Height/2),PointF(layer.Width*1/3,layer.Height/4),
                           dmSet);
        image.AddOwnedLayer(layer,boMultiply);
      end;
    
      procedure CreateBackground;
      var layer: TBGRABitmap;
      begin
        layer := TBGRABitmap.Create(image.Width,image.Height);
        layer.GradientFill(0,0,image.Width,image.Height,
                           BGRA(128,192,255),BGRA(0,0,255),
                           gtLinear,PointF(0,0),PointF(0,image.Height),
                           dmSet);
        image.AddOwnedLayer(layer);
      end;
    
    begin
      image := TBGRALayeredBitmap.Create(AWidth,AHeight);
    
      if image.Height < image.Width then
        size := image.Height
      else
        size := image.Width;
    
      CreateBackground;
      CreateMoon;
      CreateSun;
      CreateLight;
    
      result := image;
    end;
    
    { TForm1 }
    
    procedure TForm1.FormPaint(Sender: TObject);
    var image: TBGRALayeredBitmap;
    begin
      image := CreateMyImage(ClientWidth,ClientHeight);
      image.Draw(Canvas,0,0);
      image.free;
    end;
    

### Saving the image in an OpenRaster file

Now, let's create a button with the following code: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var image: TBGRALayeredBitmap;
    begin
      image := CreateMyImage(ClientWidth,ClientHeight);
      image.SaveToFile('myimage.ora');
      image.free;
    end;
    

This code is fairly straightforward: it creates an image of the window size and saves it in OpenRaster format (.ora). You can then import this image into Krita, Gimp, or Paint.NET. 

[Previous tutorial (direct pixel access)](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") [Next tutorial (line style)](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6")

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial_5](https://web.archive.org/web/20241202174636/https://wiki.freepascal.org/BGRABitmap_tutorial_5)_
