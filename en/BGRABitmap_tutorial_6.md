# BGRABitmap tutorial 6

│ **English (en)** │

  
****

[ **Home**](<BGRABitmap_tutorial.md> "BGRABitmap tutorial") | [ **Tutorial 1**](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [ **Tutorial 2**](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") | [ **Tutorial 3**](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") | [ **Tutorial 4**](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") | [ **Tutorial 5**](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") |  **Tutorial 6** | [ **Tutorial 7**](<BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7") | [ **Tutorial 8**](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") | [ **Tutorial 10**](<BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") | [ **Tutorial 11**](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") | [ **Tutorial 12**](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") | [ **Tutorial 13**](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") | [ **Tutorial 14**](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") | [ **Tutorial 15**](<BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15") | [ **Tutorial 16**](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | Edit

This tutorial shows you how to use different line styles and shapes. 

## Contents

  * 1 Create a new project
  * 2 Add a painting handler
  * 3 Run the program
  * 4 Change join style
  * 5 Run the program
  * 6 Mix join styles
  * 7 Change pen style
  * 8 Change line cap
  * 9 Opened line drawing



### Create a new project

Create a new project and add a reference to [BGRABitmap](<BGRABitmap.md> "BGRABitmap"), the same way as in [the first tutorial](<BGRABitmap_tutorial.md> "BGRABitmap tutorial"). 

### Add a painting handler

With the object inspector, add an OnPaint handler and write: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var image: TBGRABitmap;
        c: TBGRAPixel;
    begin
      image := TBGRABitmap.Create(ClientWidth, ClientHeight, clBtnFace);
      c := clWindowText; 
    
      image.RectangleAntialias(80,80,300,200, c, 50);
    
      image.Draw(Canvas,0,0,True);
      image.free;
    end;
    

### Run the program

This should draw a rectangle with a wide black pen. 

[![BGRATutorial6a.png](https://wiki.freepascal.org/images/0/06/BGRATutorial6a.png)](</File:BGRATutorial6a.png>)

### Change join style

If you want round corners, you can specify: 
    
    
        image.JoinStyle := pjsRound;
    

### Run the program

This should draw a rectangle with a wide black pen with rounded corners. 

[![BGRATutorial6b.png](https://wiki.freepascal.org/images/3/34/BGRATutorial6b.png)](</File:BGRATutorial6b.png>)

### Mix join styles

You can mix join styles for a rectangle like this: 
    
    
        image.FillRoundRectAntialias(80,80,300,200, 20,20, c, [rrTopRightSquare,rrBottomLeftSquare]);
    

This function use round corners by default, but you can override it with square corners or bevel corners. You should obtain the following image. 

[![BGRATutorial6e.png](https://wiki.freepascal.org/images/5/54/BGRATutorial6e.png)](</File:BGRATutorial6e.png>)

### Change pen style

You can draw a dotted line like this: 
    
    
        image.JoinStyle := pjsBevel;
        image.PenStyle := psDot;
        image.DrawPolyLineAntialias([PointF(40,200), PointF(120,100), PointF(170,140), PointF(250,60)],c,10);
    

Note that the pen style can be defined independently of a bitmap by using the TBGRAPenStroker class of the BGRAPen[[1]](<https://github.com/bgrabitmap/bgrabitmap/blob/master/bgrabitmap/bgrapen.pas>) unit.  
---  
  
You should obtain the following image. Notice that line begins with a round cap. 

[![Tutorial6c.png](https://wiki.freepascal.org/images/3/30/Tutorial6c.png)](</File:Tutorial6c.png>)

### Change line cap

You can draw a polyline with a square cap like this: 
    
    
        image.JoinStyle := pjsBevel;
        image.LineCap := pecSquare;
        image.PenStyle := psSolid;
        image.DrawPolyLineAntialias([PointF(40,200), PointF(120,100), PointF(170,140), PointF(250,60)],c,10);
    

[![BGRATutorial6d.png](https://wiki.freepascal.org/images/7/73/BGRATutorial6d.png)](</File:BGRATutorial6d.png>)

### Opened line drawing

You can draw a line which is opened, i.e. the end of the line is rounded inside. 
    
    
        image.DrawPolyLineAntialias([PointF(40,200), PointF(120,100), PointF(170,140), PointF(250,60)],c,10,False);
    

[![BGRATutorial6f.png](https://wiki.freepascal.org/images/2/2f/BGRATutorial6f.png)](</File:BGRATutorial6f.png>)

This way you can connect lines one after another without drawing the junction twice, which is useful with semi-transparent drawing. You can compare it like this: 
    
    
        c := BGRA(0,0,0,128);
    
        image.DrawLineAntialias(40,150, 120,50, c, 10);
        image.DrawLineAntialias(120,50, 170,90, c, 10);
        image.DrawLineAntialias(170,90, 250,10, c, 10);
    
        image.DrawLineAntialias(40,250, 120,150, c, 10, False);
        image.DrawLineAntialias(120,150, 170,190, c, 10, False);
        image.DrawLineAntialias(170,190, 250,110, c, 10, True);
    

[![Tutorial6g.png](https://wiki.freepascal.org/images/8/8f/Tutorial6g.png)](</File:Tutorial6g.png>)

[Previous tutorial (layers and masks)](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") [Next tutorial (splines)](<BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7")

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial_6](https://web.archive.org/web/20241202174513/https://wiki.freepascal.org/BGRABitmap_tutorial_6)_
