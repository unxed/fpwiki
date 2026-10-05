# BGRABitmap tutorial 11

│ **English (en)** │

[ **Home**](<BGRABitmap_tutorial.md> "BGRABitmap tutorial") | [ **Tutorial 1**](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [ **Tutorial 2**](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") | [ **Tutorial 3**](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") | [ **Tutorial 4**](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") | [ **Tutorial 5**](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") | [ **Tutorial 6**](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") | [ **Tutorial 7**](<BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7") | [ **Tutorial 8**](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") | [ **Tutorial 10**](<BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") |  **Tutorial 11** | [ **Tutorial 12**](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") | [ **Tutorial 13**](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") | [ **Tutorial 14**](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") | [ **Tutorial 15**](<BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15") | [ **Tutorial 16**](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | [Edit](<http://wiki.lazarus.freepascal.org/index.php?title=Template:BGRABitmap_tutorial_index&action=edit>)

This tutorial shows how to combine transformations. 

## Contents

  * 1 Create a new project
  * 2 Affine transformation of a radial gradient
  * 3 Run the program
  * 4 Combine with a twirl
  * 5 Run the program
  * 6 Use a custom scanner
  * 7 Run the program
  * 8 Make it beautiful



### Create a new project

Create a new project and add a reference to [BGRABitmap](<BGRABitmap.md> "BGRABitmap"), the same way as in [the first tutorial](<BGRABitmap_tutorial.md> "BGRABitmap tutorial"). 

### Affine transformation of a radial gradient

Here is an affine transformation applied on a gradient like we did with textures : 
    
    
    uses BGRAGradientScanner, BGRATransform;   
    
    procedure TForm1.FormPaint(Sender: TObject);
    var image: TBGRABitmap;
        grad: TBGRAGradientScanner;
        affine: TBGRAAffineScannerTransform;
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight, BGRABlack );
    
      grad := TBGRAGradientScanner.Create(BGRA(0,0,255),BGRAWhite,gtRadial,PointF(0,0),PointF(1,0),True,True);
    
      affine := TBGRAAffineScannerTransform.Create(grad);
      affine.Scale(150,80);
      affine.RotateDeg(-30);
      affine.Translate(ClientWidth/2, ClientHeight/2);
    
      image.Fill(affine);
    
      affine.free;
      grad.free;
    
      image.Draw(Canvas,0,0,True);
      image.free;
    end;

The base gradient is radial, centered at origin (0,0), of radius 1. 

The affine transformation does the following : 

  * stretch the gradient to a size of 150x80
  * rotate 30 degrees in counterclockwise direction
  * center it in the form



The Fill instruction draws the result into the image. 

### Run the program

You should obtain a blue and white ellipse gradient. 

[![BGRATutorial11a.png](https://wiki.freepascal.org/images/a/a9/BGRATutorial11a.png)](</File:BGRATutorial11a.png>)

### Combine with a twirl

We can add another transformation to it like this : 
    
    
    var image: TBGRABitmap;
        grad: TBGRAGradientScanner;
        affine: TBGRAAffineScannerTransform;
        twirl: TBGRATwirlScanner;
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight, BGRABlack );
    
      grad := TBGRAGradientScanner.Create(BGRA(0,0,255),BGRAWhite,gtRadial,PointF(0,0),PointF(1,0),True,True);
    
      affine := TBGRAAffineScannerTransform.Create(grad);
      affine.Scale(150,80);
      affine.RotateDeg(-30);
      affine.Translate(ClientWidth/2, ClientHeight/2);
    
      twirl := TBGRATwirlScanner.Create(affine,Point(ClientWidth div 2, ClientHeight div 2),100);
      image.Fill(twirl);
      twirl.Free;
    
      affine.free;
      grad.free;
    
      image.Draw(Canvas,0,0,True);
      image.free;
    end;

Here, we simply create a twirl transformation applied to the previous one, and fill with it. 

### Run the program

Now the center of the gradient is twirled. 

[![BGRATutorial11b.png](https://wiki.freepascal.org/images/b/b0/BGRATutorial11b.png)](</File:BGRATutorial11b.png>)

### Use a custom scanner

We may want to create our own gradient generator. Here is for example a multiply gradient : 
    
    
    type
      { TBGRAMultiplyScanner }
    
      TBGRAMultiplyScanner = class(TBGRACustomScanner)
        function ScanAt(X, Y: Single): TBGRAPixel; override;
      end;
    
    { TBGRAMultiplyScanner }
    
    function TBGRAMultiplyScanner.ScanAt(X, Y: Single): TBGRAPixel;
      function cycle512(value: integer): integer; inline;
      begin
        result := value and 511;
        if result >= 256 then result := 511-result;
      end;
    
    var
      mul: integer;
    begin
      mul := cycle512(round(x*y));
      result := BGRA(mul,mul,mul,255);
    end;

It is derived from TBGRACustomScanner to be used for filling, and the ScanAt function is overridden. It computes the product of both coordinates and make a cycle of 512 with it (from 0 to 255 and then from 255 down to 0). 

Let's draw it on the screen with a simple affine transformation : 
    
    
    var image: TBGRABitmap;
        grad: TBGRAMultiplyScanner;
        affine: TBGRAAffineScannerTransform;
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight, BGRABlack );
    
      grad := TBGRAMultiplyScanner.Create;
      affine := TBGRAAffineScannerTransform.Create(grad);
      affine.Scale(6,4);
      affine.Translate(ClientWidth/2, ClientHeight/2);
      image.Fill(affine);
      affine.free;
      grad.free;
    
      image.Draw(Canvas,0,0,True);
      image.free;
    end;

### Run the program

It should look like this : 

[![BGRATutorial11c.png](https://wiki.freepascal.org/images/e/e0/BGRATutorial11c.png)](</File:BGRATutorial11c.png>)

### Make it beautiful

Add some colors by modifying the ScanAt procedure of the multiply gradient : 
    
    
    var
      mul: integer;
    begin
      mul := round(x*y);
      result := BGRA(cycle512(round(x*10)),cycle512(mul),cycle512(round(y*10)),255);
    end;

The red and blue intensities are filled with a cycle of x and y positions. 

And add some rotation : 
    
    
      affine := TBGRAAffineScannerTransform.Create(grad);
      affine.Scale(6,4);
      affine.RotateDeg(-30);
      affine.Translate(ClientWidth/2, ClientHeight/2);

[![BGRATutorial11d.png](https://wiki.freepascal.org/images/e/e8/BGRATutorial11d.png)](</File:BGRATutorial11d.png>)

[Previous tutorial (texture mapping)](<BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") [Next tutorial (text functions)](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12")

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial_11](https://web.archive.org/web/20190818192736/https://wiki.freepascal.org/BGRABitmap_tutorial_11)_
