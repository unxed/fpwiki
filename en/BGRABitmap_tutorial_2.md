# BGRABitmap tutorial 2

│ **[Deutsch (de)](</BGRABitmap_tutorial_2/de> "BGRABitmap tutorial 2/de")** │  **English (en)** │  **[español (es)](</BGRABitmap_tutorial_2/es> "BGRABitmap tutorial 2/es")** │  **[français (fr)](</BGRABitmap_tutorial_2/fr> "BGRABitmap tutorial 2/fr")** │  **[русский (ru)](<../ru/BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2/ru")** │ 

  
****

[ **Home**](<BGRABitmap_tutorial.md> "BGRABitmap tutorial") | [ **Tutorial 1**](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") |  **Tutorial 2** | [ **Tutorial 3**](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") | [ **Tutorial 4**](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") | [ **Tutorial 5**](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") | [ **Tutorial 6**](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") | [ **Tutorial 7**](<BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7") | [ **Tutorial 8**](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") | [ **Tutorial 10**](<BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") | [ **Tutorial 11**](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") | [ **Tutorial 12**](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") | [ **Tutorial 13**](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") | [ **Tutorial 14**](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") | [ **Tutorial 15**](<BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15") | [ **Tutorial 16**](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | Edit

This tutorial shows you how to load an image and draw it on a form. 

## Contents

  * 1 Create a new project
  * 2 Load the bitmap
  * 3 Draw the bitmap
  * 4 Code
  * 5 Run the program
  * 6 Center the image
  * 7 Stretch the image



### Create a new project

Create a new project and add a reference to [BGRABitmap](<BGRABitmap.md> "BGRABitmap"), the same way as in [the first tutorial](<BGRABitmap_tutorial.md> "BGRABitmap tutorial"). 

### Load the bitmap

Copy an image into your project directory. Let's suppose it's name is _image.png_. 

Add a private variable to the main form to store the image: 
    
    
    TForm1 = class(TForm)
      private
        { private declarations }
        image: TBGRABitmap;
      public
        { public declarations }
      end;
    

Load the image when the form is created. To do this, double-click on the form, a procedure should appear in the code editor. Add the following [loading instruction](<TBGRACustomBitmap_and_IBGRAScanner.md> "TBGRACustomBitmap and IBGRAScanner") (and don't forget to destroy the image at the end): 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      image := TBGRABitmap.Create('image.png');
    end; 
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      image.Free;
    end;
    

### Draw the bitmap

Add an OnPaint handler. To do this, select the main form, then go to the object inspector, in the event tab, and double-click on the OnPaint line. Then, add the drawing code: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    begin
      image.Draw(Canvas,0,0,True);
    end;
    

Notice that the last parameter is set to True, which means opaque. If you want to take transparent pixels into account, encoded in the alpha channel, you must use False instead. But it can be slow to use transparent drawing on standard canvas, so if it is not necessary, use opaque drawing only. 

### Code

Finally you should have something like: 
    
    
    unit UMain;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, LResources, Forms, Controls, Graphics, Dialogs,
      BGRABitmap, BGRABitmapTypes;
    
    type
      { TForm1 }
    
      TForm1 = class(TForm)
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
        procedure FormPaint(Sender: TObject);
      private
        { private declarations }
        image: TBGRABitmap;
      public
        { public declarations }
      end; 
    
    var
      Form1: TForm1; 
    
    implementation
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      image := TBGRABitmap.Create('image.png');
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      image.free;
    end;
    
    procedure TForm1.FormPaint(Sender: TObject);
    begin
      image.Draw(Canvas,0,0,True);
    end;
    
    initialization
      {$I UMain.lrs}
    
    end.
    

### Run the program

You should see a form with an image drawn in it at the upper-left corner. 

[![BGRATutorial2.png](https://wiki.freepascal.org/images/d/db/BGRATutorial2.png)](</File:BGRATutorial2.png>)

### Center the image

You may want to center the image on the form. To do this, modify the FormPaint procedure: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var ImagePos: TPoint;
    begin
      ImagePos := Point( (ClientWidth - Image.Width) div 2,
                         (ClientHeight - Image.Height) div 2 );
    
      // test for negative position
      if ImagePos.X < 0 then ImagePos.X := 0;
      if ImagePos.Y < 0 then ImagePos.Y := 0;
    
      image.Draw(Canvas,ImagePos.X,ImagePos.Y,True);
    end;
    

To compute the position, we need to calculate the space between the image and the left border (X coordinate) and the space between the image and the top border (Y coordinate). The expression ClientWidth - Image.Width returns the available horizontal space, and we divide it by 2 to obtain the left margin. 

The result can be negative if the image is bigger than the client width. In this case, the margin is just set to zero. 

You can run the program and see if it works. Notice what happens if we remove the test for negative position. 

### Stretch the image

To stretch the image, we need to create a temporary stretched image: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var stretched: TBGRABitmap;
    begin
      stretched := image.Resample(ClientWidth, ClientHeight) as TBGRABitmap;
      stretched.Draw(Canvas,0,0,True);
      stretched.Free;
    end;
    

By default, it uses fine resample, but you can precise if you want to use simple stretch instead (faster): 
    
    
    stretched := image.Resample(ClientWidth, ClientHeight, rmSimpleStretch) as TBGRABitmap;
    

You can also specify the interpolation filter with the [ResampleFilter](<TBGRACustomBitmap_and_IBGRAScanner.md> "TBGRACustomBitmap and IBGRAScanner") property: 
    
    
    image.ResampleFilter := rfMitchell;
    stretched := image.Resample(ClientWidth, ClientHeight) as TBGRABitmap;
    

[First tutorial](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [Next tutorial (drawing with the mouse)](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3")

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial_2](https://web.archive.org/web/20241104220947/https://wiki.freepascal.org/BGRABitmap_tutorial_2)_
