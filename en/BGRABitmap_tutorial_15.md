# BGRABitmap tutorial 15

[ **Deutsch (de)**](</BGRABitmap_tutorial_15/de> "BGRABitmap tutorial 15/de") | **English (en)** | [**Français (fr)**](</index.php?title=BGRABitmap_tutorial_15/fr&action=edit&redlink=1> "BGRABitmap tutorial 15/fr \(page does not exist\)") | [**Español (es)**](</index.php?title=BGRABitmap_tutorial_15/es&action=edit&redlink=1> "BGRABitmap tutorial 15/es \(page does not exist\)") | [Edit](<http://wiki.lazarus.freepascal.org/index.php?title=Template:BGRABitmap_tutorial_15&action=edit>)   
****

[ **Home**](<BGRABitmap_tutorial.md> "BGRABitmap tutorial") | [ **Tutorial 1**](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [ **Tutorial 2**](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") | [ **Tutorial 3**](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") | [ **Tutorial 4**](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") | [ **Tutorial 5**](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") | [ **Tutorial 6**](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") | [ **Tutorial 7**](<BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7") | [ **Tutorial 8**](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") | [ **Tutorial 10**](<BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") | [ **Tutorial 11**](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") | [ **Tutorial 12**](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") | [ **Tutorial 13**](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") | [ **Tutorial 14**](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") |  **Tutorial 15** | [ **Tutorial 16**](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | [Edit](<http://wiki.lazarus.freepascal.org/index.php?title=Template:BGRABitmap_tutorial_index&action=edit>)

This tutorial shows how to render 3D objects using the TBGRAScene3D object. 

## Scene object

The scene object is in the BGRAScene3D unit. Here is a simple example : 
    
    
    uses BGRAScene3D, BGRABitmap, BGRABitmapTypes; 
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      scene: TBGRAScene3D;
      bmp: TBGRABitmap;
      base: array of IBGRAVertex3D;
      topV: IBGRAVertex3D;
    begin
      bmp := TBGRABitmap.Create(ClientWidth,ClientHeight,BGRABlack);
    
      scene := TBGRAScene3D.Create(bmp);
      //create a pyramid
      with scene.CreateObject(BGRA(255,240,128)) do
      begin
        //create vertices
        topV := MainPart.Add(0,-15,0);
        //pyramid base is in a clockwise order if we look at the pyramid from below
        base := MainPart.Add([-20,15,-20, 20,15,-20, 20,15,20, -20,15,20]);
    
        AddFace(base);
        //add four faces, the three vertices are in a clockwise order
        AddFace([base[0],topV,base[1]]);
        AddFace([base[1],topV,base[2]]);
        AddFace([base[2],topV,base[3]]);
        AddFace([base[3],topV,base[0]]);
        topV := nil;
        base := nil;
      end;
      scene.Render;
      scene.Free;
    
      bmp.Draw(Canvas,0,0);
      bmp.Free;
    end;

The scene object draws itself on a TBGRABitmap object. You can either pass the bitmap as a parameter when creating the object, as done here, or assign the Surface property afterwards. The scene is automatically centered in the bitmap. 

The scene object provides the CreateObject function, which returns an interface to the created object. Objects inside a scene are freed automatically when you free the scene. 

An object has a MainPart property which allows to create and access to vertices. Coordinates can be given as three single values, TPoint3D records, or an array of single values which length must be a multiple of 3. When you create a vertex, you receive an IBGRAVertex3D interface which can be used to create faces. You can add sub parts inside MainPart by using CreatePart method. There can be nested subparts, each being rotated with its own matrix relative to main part. 

The X axis points to the right, the Y axis points to bottom, and the Z axis points forward (behind the screen). It means that XY works the same way as in a bitmap, and there is a depth value. The faces must be in clockwise order to be visible. 

Here the color is set for the whole object when creating it, but you can set it individually for each face, or for each vertex. 

[![BGRATutorial15a.png](https://wiki.freepascal.org/images/a/af/BGRATutorial15a.png)](</File:BGRATutorial15a.png>)

As you can see, the pyramid is viewed from one side, so we can only see one face. There is no lighting either. 

In the 'with' block, add the following lines : 
    
    
        MainPart.Scale(1.3);
        MainPart.RotateYDeg(30);
        MainPart.RotateXDeg(20);
        MainPart.Translate(0,-5,0);

The first line makes the object a little bigger. The two rotations are applied. The first is around the Y axis and the second around the X axis. To figure out the rotation sign, imagine you look in the direction of the axis. A positive value in degree means a clockwise rotation, and a positive value in radian means an anti-clockwise rotation. 

Finally a vertical translation is applied to center the object. 

[![BGRATutorial15b.png](https://wiki.freepascal.org/images/2/29/BGRATutorial15b.png)](</File:BGRATutorial15b.png>)

Now let's add some lighting : 
    
    
      //set ambiant lightness to dark (1 is normal lightness, 2 is complete whiteness)
      scene.AmbiantLightness := 0.5;
      //add a directional light from top-left, maximum lightness will be 0.5 + 1 = 1.5
      scene.AddDirectionalLight(Point3D(1,1,1), 1);

The example uses lightness values. 0 means black, 1 means unchanged color, and 2 means white. The directional takes a Point3D parameter to indicate the direction of the ray. It does not need to be normalized. 

[![BGRATutorial15c.png](https://wiki.freepascal.org/images/5/5d/BGRATutorial15c.png)](</File:BGRATutorial15c.png>)

## Deriving from TBGRAScene3D

The scene code can be embedded in an object. Here is the previous example in a single unit : 
    
    
    unit ex1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, BGRAScene3D, BGRABitmapTypes;
    
    type
    
      { TExample1 }
    
      TExample1 = class(TBGRAScene3D)
        SandColor: TBGRAPixel;
        constructor Create;
        procedure Render; override;
      end;
    
    implementation
    
    { TExample1 }
    
    constructor TExample1.Create;
    var
      base: array of IBGRAVertex3D;
      top: IBGRAVertex3D;
    begin
      inherited Create;
    
      SandColor := BGRA(255,240,128);
    
      //create a pyramid
      with CreateObject(SandColor) do
      begin
        top := MainPart.Add(0,-15,0);
        //pyramid base is in a clockwise order if we look at the pyramid from below
        base := MainPart.Add([-20,15,-20, 20,15,-20, 20,15,20, -20,15,20]);
        AddFace(base);
        //add four faces, the three vertices are in a clockwise order
        AddFace([base[0],top,base[1]]);
        AddFace([base[1],top,base[2]]);
        AddFace([base[2],top,base[3]]);
        AddFace([base[3],top,base[0]]);
    
        MainPart.Scale(1.3);
        MainPart.RotateYDeg(30);
        MainPart.RotateXDeg(20);
        MainPart.Translate(0,-5,0);
      end;
    
      //set ambiant lightness to dark (1 is normal lightness, 2 is complete whiteness)
      AmbiantLightness := 0.5;
      //add a directional light from top-left, maximum lightness will be 0.5 + 1 = 1.5
      AddDirectionalLight(Point3D(1,1,1),1);
    
      //we can have antialiasing because it is a simple scene
      Antialiasing := True;
    end;
    
    procedure TExample1.Render;
    begin
      //fill background
      Surface.GradientFill(0,0,Surface.Width,Surface.Height,SandColor,MergeBGRA(SandColor,BGRABlack),gtRadial,PointF(0,0),PointF(Surface.Width,Surface.Height),dmSet);
    
      inherited Render;
    end;
    
    end.

Now, to draw the scene, simply do : 
    
    
    uses BGRABitmap, BGRABitmapTypes, BGRAScene3D, ex1;
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      bmp: TBGRABitmap;
      scene: TBGRAScene3D;
    begin
      bmp := TBGRABitmap.Create(ClientWidth,ClientHeight,BGRABlack);
    
      scene := TExample1.Create;
      scene.Surface := bmp;
      scene.Render;
      scene.Free;
    
      bmp.Draw(Canvas,0,0);
      bmp.Free;
    end;

By the way, antialiasing and a background have been added. Note that antialiasing works only for simple scenes. Complex scenes with texture and transparent colors may not render correctly. 

[![BGRATutorial15d.png](https://wiki.freepascal.org/images/9/95/BGRATutorial15d.png)](</File:BGRATutorial15d.png>)

  
[Previous tutorial (canvas 2D)](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") | [Next tutorial (textures with 3D objects)](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16")

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial_15](https://web.archive.org/web/20191213155508/https://wiki.freepascal.org/BGRABitmap_tutorial_15)_
