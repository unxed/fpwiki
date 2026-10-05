# BGRABitmap tutorial 14

│ **[Deutsch (de)](</BGRABitmap_tutorial_14/de> "BGRABitmap tutorial 14/de")** │  **English (en)** │ 

  
****

[ **Home**](<BGRABitmap_tutorial.md> "BGRABitmap tutorial") | [ **Tutorial 1**](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [ **Tutorial 2**](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") | [ **Tutorial 3**](<BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") | [ **Tutorial 4**](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") | [ **Tutorial 5**](<BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") | [ **Tutorial 6**](<BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") | [ **Tutorial 7**](<BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7") | [ **Tutorial 8**](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") | [ **Tutorial 10**](<BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") | [ **Tutorial 11**](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") | [ **Tutorial 12**](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") | [ **Tutorial 13**](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") |  **Tutorial 14** | [ **Tutorial 15**](<BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15") | [ **Tutorial 16**](<BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | Edit

This tutorial shows how to use the Canvas2D property of BGRABitmap. Canvas2D is designed to work like the Javascript 2D context of the HTML Canvas object. 

## Contents

  * 1 First program
  * 2 Complex shapes
  * 3 Using transformations
  * 4 Gradients and memory usage
  * 5 More examples



## First program

Here is a very simple example: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var bmp: TBGRABitmap;
        ctx: TBGRACanvas2D;   
    begin
      bmp := TBGRABitmap.Create(ClientWidth,ClientHeight,BGRA(210,210,210));
    
      ctx := bmp.Canvas2D;
      ctx.fillStyle('rgb(240,128,0)');
      ctx.fillRect(30,30,80,60);
      ctx.strokeRect(50,50,80,60);  
    
      bmp.Draw(Canvas,0,0);
      bmp.Free;
    end;
    

Note that path fonctions are defined in the [IBGRAPath interface](<BGRABitmap_Geometry_types.md> "BGRABitmap Geometry types"). The **TBGRAPath** class of unit BGRAPath[[1]](<https://github.com/bgrabitmap/bgrabitmap/blob/master/bgrabitmap/bgrapath.pas>) provides an independent path object that you can use to create shapes. A sample project can be found in [BGRABitmap AggPas](<BGRABitmap_AggPas.md> "BGRABitmap AggPas").  
---  
  
The bitmap has a Canvas2D property which provides the fillRect and strokeRect function. The fillStyle is defined to orange by specifying a css color string. When the shape is filled, it uses the fill style, and when the border is drawn, it uses the stroke style. 

The code above is equivalent to this Javascript code: 
    
    
     var canvas = document.getElementsByTagName("canvas")[0];
     canvas.width = 200
     canvas.height = 200
     if (canvas.getContext){
       var ctx = canvas.getContext("2d"); 
       ctx.fillStyle = "rgb(240,128,0)";
       ctx.fillRect(30,30,80,60);
       ctx.strokeRect(50,50,80,60);
     }
    

[![BGRATutorial14a.png](https://wiki.freepascal.org/images/5/5d/BGRATutorial14a.png)](</File:BGRATutorial14a.png>)

## Complex shapes

To draw a complex shape, it is necessary to define a path: 
    
    
      procedure pave();
      begin
        ctx.fillStyle ('rgb(130,100,255)');
        ctx.strokeStyle ('rgb(0,0,255)');
        ctx.beginPath();
        ctx.lineWidth:=2;
        ctx.moveTo(5,5);ctx.lineTo(20,10);ctx.lineTo(55,5);ctx.lineTo(45,18);ctx.lineTo(30,50);
        ctx.closePath();
        ctx.stroke();
        ctx.fill();
      end;  
     
    begin
      bmp := TBGRABitmap.Create(ClientWidth,ClientHeight,BGRA(210,210,210));
      ctx := bmp.Canvas2D;
      pave();
      bmp.Draw(Canvas,0,0);
      bmp.Free;
    end;
    

Notice that the line width is defined by the property lineWidth and that the path begins with a beginPath call. If you want more information on the way path works, see the [Javascript documentation](<https://developer.mozilla.org/en/Canvas_tutorial/Drawing_shapes>). 

[![Tutorial14b.png](https://wiki.freepascal.org/images/a/a4/Tutorial14b.png)](</File:Tutorial14b.png>)

## Using transformations

Now we can draw the triangle six times with a rotation by calling transformation functions. 
    
    
      procedure six();
      var
        i: Integer;
      begin
         ctx.save();
         for i := 0 to 5 do
         begin
            ctx.rotate(2*PI/6);
            pave();
         end;
         ctx.restore();
      end;
    begin
      bmp := TBGRABitmap.Create(ClientWidth,ClientHeight,BGRA(210,210,210));
      ctx := bmp.Canvas2D;
      ctx.translate(80,80);
      six;
      bmp.Draw(Canvas,0,0);
      bmp.Free;
    end;
    

[![BGRATutorial14c.png](https://wiki.freepascal.org/images/9/91/BGRATutorial14c.png)](</File:BGRATutorial14c.png>)

## Gradients and memory usage

To use gradients, Canvas2D provides the createLinearGradient and createPattern functions. Theses functions return an interfaced object. You must not free them explicitely. They are freed when there are no more references to them. For example: 
    
    
    var
      grad: IBGRACanvasGradient2D;
    begin
      grad := ctx.createLinearGradient(0,0,320,240);
      grad.addColorStop(0.3, '#ff0000');
      grad.addColorStop(0.6, '#0000ff');
      ctx.fillStyle(grad);
    
      grad := ctx.createLinearGradient(0,0,320,240);
      grad.addColorStop(0.3, '#ffffff');
      grad.addColorStop(0.6, '#000000');
      ctx.strokeStyle(grad);
      ctx.lineWidth := 5;
    
      ctx.moveto(160,120);
      ctx.arc(160,120,100,Pi/6,-Pi/6,false);
      ctx.fill();
      ctx.stroke();
    end;
    

The grad variable is assigned with the gradient objects, but there is no call to Free. 

[![BGRATutorial14d.png](https://wiki.freepascal.org/images/9/96/BGRATutorial14d.png)](</File:BGRATutorial14d.png>)

## More examples

Other examples are in the testcanvas2d directory of BGRABitmap archive. Scripts are taken from the [Jean-Paul Davalan web site](<https://web.archive.org/web/20190207062328/http://jean-paul.davalan.pagesperso-orange.fr/lang/jsc/js09.html>) (Internet Archive link) which contains HTML Canvas examples. 

[Previous tutorial (pixel coordinates)](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") [Next tutorial (3D)](<BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15")

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial_14](https://web.archive.org/web/20241209224300/https://wiki.freepascal.org/BGRABitmap_tutorial_14)_
