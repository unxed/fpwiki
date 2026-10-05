# Canvas line

The **Canvas.Line(x1, y1, x2, y2)** statement will draw a line from point (x1, y1) to (x2, y2). 

The coordinates of the canvas are: 

[![canvas2.png](https://wiki.freepascal.org/images/7/73/canvas2.png)](</File:canvas2.png>)

  
This code will draw the diagonals in a form: 
    
    
    procedure TForm1.Form1Paint(Sender: TObject);
    begin
      Canvas.Line(0, 0, Width-1, Height-1);
      Canvas.Line(0, Height-1, Width-1 ,0);
    end;
    

## See also

  * [Drawing with canvas](<Drawing_with_canvas.md> "Drawing with canvas")

---

_Source: [https://wiki.freepascal.org/Canvas_line](https://web.archive.org/web/20240709183823/https://wiki.freepascal.org/Canvas_line)_
