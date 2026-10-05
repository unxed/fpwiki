# BGRABitmap tutorial 7

[ **Deutsch (de)**](</BGRABitmap_tutorial_7/de> "BGRABitmap tutorial 7/de") | **English (en)** | [**Français (fr)**](</BGRABitmap_tutorial_7/fr> "BGRABitmap tutorial 7/fr") | [**Español (es)**](</BGRABitmap_tutorial_7/es> "BGRABitmap tutorial 7/es") | [Edit](<http://wiki.lazarus.freepascal.org/index.php?title=Template:BGRABitmap_tutorial_7&action=edit>)   
****

[ **Home**](<BGRABitmap_tutorial.md> "BGRABitmap tutorial") | [ **Tutorial 1**](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [ **Tutorial 2**](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") | [ **Tutorial 3**](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") | [ **Tutorial 4**](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") | [ **Tutorial 5**](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") | [ **Tutorial 6**](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") |  **Tutorial 7** | [ **Tutorial 8**](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") | [ **Tutorial 10**](<BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") | [ **Tutorial 11**](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") | [ **Tutorial 12**](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") | [ **Tutorial 13**](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") | [ **Tutorial 14**](<BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") | [ **Tutorial 15**](<BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15") | [ **Tutorial 16**](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | [Edit](<http://wiki.lazarus.freepascal.org/index.php?title=Template:BGRABitmap_tutorial_index&action=edit>)

This tutorial shows how to use splines. 

## Contents

  * 1 Create a new project
  * 2 Draw an opened spline
  * 3 Draw a closed spline
  * 4 Run the program
  * 5 Using Bézier curves
  * 6 Run the program



### Create a new project

Create a new project and add a reference to [BGRABitmap](<BGRABitmap.md> "BGRABitmap"), the same way as in [the first tutorial](<BGRABitmap_tutorial.md> "BGRABitmap tutorial"). 

### Draw an opened spline

With the object inspector, add an OnPaint handler and write : 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      image: TBGRABitmap;
      pts: array of TPointF;
      storedSpline: array of TPointF;
      c: TBGRAPixel;
    
    begin
        image := TBGRABitmap.Create(ClientWidth,ClientHeight,ColorToBGRA(ColorToRGB(clBtnFace)));
        c := ColorToBGRA(ColorToRGB(clWindowText));
    
        //rectangular polyline
        setlength(pts,4);
        pts[0] := PointF(50,50);
        pts[1] := PointF(150,50);
        pts[2] := PointF(150,150);
        pts[3] := PointF(50,150);
        image.DrawPolylineAntialias(pts,BGRA(255,0,0,150),1);
    
        //compute spline points and draw as a polyline
        storedSpline := image.ComputeOpenedSpline(pts,ssVertexToSide);
        image.DrawPolylineAntialias(storedSpline,c,1);
    
        image.Draw(Canvas,0,0,True);
        image.free;  
    end;

There are two lines that draw the spline. The first line computes the spline points, and the second draw them. Notice that it is a specific function for opened splines. 

### Draw a closed spline

Before image.Draw, add these lines : 
    
    
        for i := 0 to 3 do
          pts[i].x += 200;
        image.DrawPolylineAntialias(pts,BGRA(255,0,0,150),1);
    
        storedSpline := image.ComputeClosedSpline(pts,ssVertexToSide);
        image.DrawPolygonAntialias(storedSpline,c,1);

Go with the text cursor on the 'i' identifier and press Ctrl-Shift-C to add the variable declaration. The loop offsets the points to the right. 

Two new lines draw a closed spline. Notice the specific function that computes closed splines and the call to DrawPolygonAntialias. 

You can avoid using a variable to store spline points like this : 
    
    
    image.DrawPolygonAntialias(image.ComputeClosedSpline(pts),c,1);

However, if you do so, you cannot use the computed points more than once, they must be calculated each time you use them. 

### Run the program

This should draw two splines, one opened on the left and one closed on the right. 

[![BGRATutorial7.png](https://wiki.freepascal.org/images/8/84/BGRATutorial7.png)](</File:BGRATutorial7.png>)

Notice that the spline goes through each point. If you want the curve to stay inside or define tangeants, you need to use control points, which are available in Bézier curves. 

### Using Bézier curves

Before image.Draw, add these lines : 
    
    
        storedSpline := image.ComputeBezierSpline([BezierCurve(PointF(50,50),PointF(150,50),PointF(150,100)),
                                                   BezierCurve(PointF(150,100),PointF(150,150),PointF(50,150))]);
        image.DrawPolylineAntialias(storedSpline,c,2);

The function BezierCurve defines a curve with an origin and a destination, and one or two control points. Here there is only one control point. Here the control points are defined so that the curve be tangeant to the rectangle defined before. 

A Bézier spline is simple a series of Bézier curve. Thus, the function ComputeBezierSpline concatenates an array of Bézier curves. Here, we construct a nice U-turn with two curves. 

### Run the program

You should see a bold Bézier curve inside the left rectangle. 

[![BGRATutorial7b.png](https://wiki.freepascal.org/images/a/aa/BGRATutorial7b.png)](</File:BGRATutorial7b.png>)

[Previous tutorial (line style)](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") [Next tutorial (textures)](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8")

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial_7](https://web.archive.org/web/20190918081341/https://wiki.freepascal.org/BGRABitmap_tutorial_7)_
