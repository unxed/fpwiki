# BGRABitmap tutorial 3

│ **English (en)** │

  
****

[ **Home**](<BGRABitmap_tutorial.md> "BGRABitmap tutorial") | [ **Tutorial 1**](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [ **Tutorial 2**](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") |  **Tutorial 3** | [ **Tutorial 4**](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") | [ **Tutorial 5**](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") | [ **Tutorial 6**](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") | [ **Tutorial 7**](<BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7") | [ **Tutorial 8**](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") | [ **Tutorial 10**](<BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") | [ **Tutorial 11**](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") | [ **Tutorial 12**](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") | [ **Tutorial 13**](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") | [ **Tutorial 14**](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") | [ **Tutorial 15**](<BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15") | [ **Tutorial 16**](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | Edit

This tutorial shows you how to draw on a bitmap with the mouse. 

## Contents

  * 1 Create a new project
  * 2 Create a new image
  * 3 Draw the bitmap
  * 4 Handle mouse
  * 5 Code
  * 6 Run the program
  * 7 Continuous drawing
  * 8 Code
  * 9 Run the program



### Create a new project

Create a new project and add a reference to [BGRABitmap](<BGRABitmap.md> "BGRABitmap"), the same way as in [the first tutorial](<BGRABitmap_tutorial.md> "BGRABitmap tutorial"). 

### Create a new image

Add a private variable to the main form to store the image: 
    
    
    TForm1 = class(TForm)
      private
        { private declarations }
        image: TBGRABitmap;
      public
        { public declarations }
      end;
    

Create the image when the form is created. To do this, double-click on the form, a procedure should appear in the code editor. Add the create instruction: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      image := TBGRABitmap.Create(640,480,BGRAWhite);  //create a 640x480 image
    end;
    

### Draw the bitmap

Add an OnPaint handler. To do this, select the main form, then go to the object inspector, in the event tab, and double-click on the OnPaint line. Then, add the drawing code: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    begin
      PaintImage;
    end;
    

Add the PaintImage procedure: 
    
    
    procedure TForm1.PaintImage;
    begin
      image.Draw(Canvas,0,0,True);
    end;
    

After writing this, put the cursor on PaintImage and press Ctrl-Shift-C to add the declaration to the interface. 

### Handle mouse

With the object inspector, add handlers for MouseDown and MouseMove events: 
    
    
    procedure TForm1.FormMouseDown(Sender: TObject; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer);
    begin
      if Button = mbLeft then DrawBrush(X,Y);
    end;
    
    procedure TForm1.FormMouseMove(Sender: TObject; Shift: TShiftState; X,
      Y: Integer);
    begin
      if ssLeft in Shift then DrawBrush(X,Y);
    end;
    

Add the DrawBrush procedure: 
    
    
    procedure TForm1.DrawBrush(X, Y: Integer);
    const radius = 5;
    begin
      image.GradientFill(X-radius,Y-radius, X+radius,Y+radius,
        BGRABlack,BGRAPixelTransparent, gtRadial,
        PointF(X,Y), PointF(X+radius,Y), dmDrawWithTransparency);
    
      PaintImage;
    end;
    

After writing this, put the cursor on DrawBrush and press Ctrl-Shift-C to add the declaration to the interface. 

This procedure draws as radial gradient (gtRadial): 

  * the bounding rectangle is (X-radius,Y-radius, X+radius,Y+radius).
  * the center is black, the border is transparent
  * the center is at (X,Y) and the border at (X+radius,Y)



### Code
    
    
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
        procedure FormMouseDown(Sender: TObject; Button: TMouseButton;
          Shift: TShiftState; X, Y: Integer);
        procedure FormMouseMove(Sender: TObject; Shift: TShiftState; X, Y: Integer);
        procedure FormPaint(Sender: TObject);
      private
        { private declarations }
        image: TBGRABitmap;
        procedure DrawBrush(X, Y: Integer);
        procedure PaintImage;
      public
        { public declarations }
      end; 
    
    var
      Form1: TForm1; 
    
    implementation
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      image := TBGRABitmap.Create(640,480,BGRAWhite);
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      image.Free;
    end;
    
    procedure TForm1.FormMouseDown(Sender: TObject; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer);
    begin
      if Button = mbLeft then DrawBrush(X,Y);
    end;
    
    procedure TForm1.FormMouseMove(Sender: TObject; Shift: TShiftState; X,
      Y: Integer);
    begin
      if ssLeft in Shift then DrawBrush(X,Y);
    end;
    
    procedure TForm1.FormPaint(Sender: TObject);
    begin
      PaintImage;
    end;
    
    procedure TForm1.DrawBrush(X, Y: Integer);
    const radius = 20;
    begin
      image.GradientFill(X-radius,Y-radius, X+radius,Y+radius,
        BGRABlack,BGRAPixelTransparent,gtRadial,
        PointF(X,Y), PointF(X+radius,Y), dmDrawWithTransparency);
    
      PaintImage;
    end;
    
    procedure TForm1.PaintImage;
    begin
      image.Draw(Canvas,0,0,True);
    end;
    
    initialization
      {$I UMain.lrs}
    
    end.
    

### Run the program

You should be able to draw on the form. 

[![BGRATutorial3.png](https://wiki.freepascal.org/images/a/aa/BGRATutorial3.png)](</File:BGRATutorial3.png>)

### Continuous drawing

To have a continuous drawing, we need additionnal variables: 
    
    
      
    TForm1 = class(TForm)
        ...
      private
        { private declarations }
        image: TBGRABitmap;
        mouseDrawing: boolean;
        mouseOrigin: TPoint;
    

mouseDrawing will be True during the drawing (with left button pressed) and mouseOrigin will be the starting point of the segment being drawn. 

The clicking handler becomes a little bit more complicated: 
    
    
    procedure TForm1.FormMouseDown(Sender: TObject; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer);
    begin
      if Button = mbLeft then
      begin
        mouseDrawing := True;
        mouseOrigin := Point(X,Y);
        DrawBrush(X,Y,True);        //   or   DrawBrush(X,Y); 
      end;
    end;
    

It initialises the drawing with the current position. Then, it draws as closed segment (note the new parameter for DrawBrush). Indeed, at the beginning, the segment is closed and has a length of zero, which gives a disk: 

[![BGRATutorial3b.png](https://wiki.freepascal.org/images/b/b1/BGRATutorial3b.png)](</File:BGRATutorial3b.png>)

Little by little, we add a new part, which is an opened segment: 

[![BGRATutorial3c.png](https://wiki.freepascal.org/images/d/d5/BGRATutorial3c.png)](</File:BGRATutorial3c.png>)

That's why we need a new parameter for the DrawBrush function, which becomes: 
    
    
    procedure TForm1.DrawBrush(X, Y: Integer; Closed: Boolean);
    const brushRadius = 20;
    begin
      image.DrawLineAntialias(X,Y,mouseOrigin.X,mouseOrigin.Y,BGRA(0,0,0,128),brushRadius,Closed);
      mouseOrigin := Point(X,Y);
    
      PaintImage;
    end;
    

We transmit the parameter Closed to DrawLineAntialias, to indicate whether the segment is closed or not. Note coordinates order. The start position and the end position are swapped. Indeed, for DrawLineAntialias, it's the end that is opened, whereas in this case, we want that the beginning be opened. 

DrawBrush definition must be updated in the interface. 

The MouseMove handler becomes: 
    
    
    procedure TForm1.FormMouseMove(Sender: TObject; Shift: TShiftState; X,
      Y: Integer);
    begin
      if mouseDrawing then DrawBrush(X,Y,False);
    end;
    

Finally, we need to add a MouseUp handler to update mouseDrawing: 
    
    
    procedure TForm1.FormMouseUp(Sender: TObject; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer);
    begin
      if Button = mbLeft then
        mouseDrawing := False;
    end;
    

### Code
    
    
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
        procedure FormMouseDown(Sender: TObject; Button: TMouseButton;
          Shift: TShiftState; X, Y: Integer);
        procedure FormMouseMove(Sender: TObject; Shift: TShiftState; X, Y: Integer);
        procedure FormMouseUp(Sender: TObject; Button: TMouseButton;
          Shift: TShiftState; X, Y: Integer);
        procedure FormPaint(Sender: TObject);
      private
        { private declarations }
        image: TBGRABitmap;
        mouseDrawing: boolean;
        mouseOrigin: TPoint;
        procedure DrawBrush(X, Y: Integer; Closed: boolean);
        procedure PaintImage;
      public
        { public declarations }
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      image := TBGRABitmap.Create(640,480,BGRAWhite);
    end;
    
    procedure TForm1.FormMouseDown(Sender: TObject; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer);
    begin
      if Button = mbLeft then
      begin
        mouseDrawing := True;
        mouseOrigin := Point(X,Y);
        DrawBrush(X,Y,True);
      end;
    end;
    
    procedure TForm1.FormMouseMove(Sender: TObject; Shift: TShiftState; X,
      Y: Integer);
    begin
      if mouseDrawing then DrawBrush(X,Y,False);
    end;
    
    procedure TForm1.FormMouseUp(Sender: TObject; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer);
    begin
      if Button = mbLeft then
        mouseDrawing := False;
    end;
    
    procedure TForm1.FormPaint(Sender: TObject);
    begin
      PaintImage;
    end;
    
    procedure TForm1.DrawBrush(X, Y: Integer; Closed: Boolean);
    const brushRadius = 20;
    begin
      image.DrawLineAntialias(X,Y,mouseOrigin.X,mouseOrigin.Y,BGRA(0,0,0,128),brushRadius,Closed);
      mouseOrigin := Point(X,Y);
    
      PaintImage;
    end;
    
    procedure TForm1.PaintImage;
    begin
      image.Draw(Canvas,0,0,True);
    end;
    
    initialization
      {$I UMain.lrs}
    
    end.
    

### Run the program

Now the drawing is almost continous: 

[![BGRATutorial3d.png](https://wiki.freepascal.org/images/9/9a/BGRATutorial3d.png)](</File:BGRATutorial3d.png>)

[Previous tutorial (image loading)](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") | [Next tutorial (direct pixel access)](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4")

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial_3](https://web.archive.org/web/20240803142139/https://wiki.freepascal.org/BGRABitmap_tutorial_3)_
