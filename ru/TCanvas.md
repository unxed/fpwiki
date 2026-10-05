# TCanvas

│ **[English (en)](<../en/TCanvas.md> "TCanvas")** │  **русский (ru)** │    
****

Класс **TCanvas** является базовым инструментом для рисования графики. 

Для рисования линий, прямоугольников и текста используются [TPen](</index.php?title=TPen/ru&action=edit&redlink=1> "TPen/ru \(page does not exist\)"), [TBrush](</index.php?title=TBrush/ru&action=edit&redlink=1> "TBrush/ru \(page does not exist\)") и [TFont](</index.php?title=TFont/ru&action=edit&redlink=1> "TFont/ru \(page does not exist\)"), чтобы определить размер и цвет карандаша, стиль и цвет кисти, стиль и цвет шрифта. 

текст | TextOut( txt ); TextOutXY( x, y, txt )   
---|---  
линии | Line( x1, y1, x2, y2 ); LineTo( x, y ) with MoveTo(x, y )   
прямоугольник | Rectangle( l, t, r, b ); Rectangle( R: TRect ); FillRect()   
окружность (эллипс) | Ellipse( l, t, r, b ); Ellipse( R: TRect )   
  
  


## Contents

  * 1 Рисование прямоугольника
  * 2 Использование шрифта по-умолчанию
  * 3 Рисование текста, ограниченного по ширине
  * 4 Рисование текста с четкими краями (без сглаживания)



## Рисование прямоугольника

Многие элементы управления отображают свой холст как общедоступное свойство или в событии OnPaint, например, [TForm](<../en/TForm.md> "TForm"), [TPanel](<../en/TPanel.md> "TPanel") и [TPaintBox](<../en/TPaintBox.md> "TPaintBox"). Давайте используем TForm в качестве примера, чтобы продемонстрировать, как рисовать на холсте. 

Предположим, мы хотим нарисовать красный прямоугольник с синей рамкой толщиной 5 пикселей в центре формы; размер прямоугольника должен составлять половину размера формы. Для этого мы должны добавить код в событие OnPaint формы. Не рисуйте в обработчике OnClick, потому что это рисование не является постоянным и будет стираться всякий раз, когда операционная система запросит перерисовку, всегда рисуйте в событии OnPaint! 

Метод TCanvas для рисования прямоугольника вызывается именно так: `Rectangle()`. Он получает координаты краев прямоугольника либо отдельно, либо в виде записи `TRect`. Цвет заливки определяется цветом кисти Brush холста, а цвет границы задается цветом пера Pen холста: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      w, h: Integer;    // ширина и высота прямоугольника
      cx, cy: Integer;  // центр формы
      R: TRect;         // запись, содержащая координаты левого, верхнего, правого и нижнего углов прямоугольника
    begin
      // высчитываем центр формы
      cx := Width div 2;
      cy := Height div 2;
    
      // высчитываем размеры прямоугольника
      w := Width div 2;
      h := Height div 2;
    
      // высчитываем точки углов прямоугольника
      R.Left := cx - w div 2;
      R.Top := cy - h div 2;
      R.Right := cx + w div 2;
      R.Bottom := cy + h div 2;
    
      // задаем цвета заливки
      Canvas.Brush.Color := clRed;
      Canvas.Brush.Style := bsSolid;
    
      // задаем цвет границ
      Canvas.Pen.Color := clBlue;
      Canvas.Pen.Width := 5;
      Canvas.Pen.Style := psSolid;
    
      // рисуем прямоугольник
      Canvas.Rectangle(R);
    end;
    

## Использование шрифта по-умолчанию

Это может быть выполнено с помощью следующего простого кода: 
    
    
    SelectObject(Canvas.Handle, GetStockObject(DEFAULT_GUI_FONT));
    

## Рисование текста, ограниченного по ширине

Используйте процедуру **DrawText** сначала с параметром DT_CALCRECT, а затем без него. 
    
    
    // Сначала вычисляем размер текста, затем рисуем его
    TextBox := Rect(0, currentPos.Y, Width, High(Integer));
    DrawText(ACanvas.Handle, PChar(Text), Length(Text),
      TextBox, DT_WORDBREAK or DT_INTERNAL or DT_CALCRECT);
    
    DrawText(ACanvas.Handle, PChar(Text), Length(Text),
      TextBox, DT_WORDBREAK or DT_INTERNAL);
    

## Рисование текста с четкими краями (без сглаживания)

Некоторые виджеты поддерживают этот способ с помощью свойства 
    
    
    Canvas.Font.Quality := fqNonAntialiased;
    

Но некоторые виджеты, например из набора gtk2, не поддерживают это свойство и всегда отображают текст сглаженным. Вот простая процедура для рисования текста с четкими краями для gtk2. В ней не рассмотрены все случаи, но в ней предлагается идея: 
    
    
    procedure PaintAliased(Canvas: TCanvas; x,y: integer; const TheText: string);
    var
      w,h: integer;
      IntfImg: TLazIntfImage;
      Img: TBitmap;
      dy: Integer;
      dx: Integer;
      col: TFPColor;
      FontColor: TColor;
      c: TColor;
    begin
      w:=0;
      h:=0;
      Canvas.GetTextSize(TheText,w,h);
      if (w<=0) or (h<=0) then exit;
      Img:=TBitmap.Create;
      IntfImg:=nil;
      try
        // рисуем текст в растре
        Img.Masked:=true;
        Img.SetSize(w,h);
        Img.Canvas.Brush.Style:=bsSolid;
        Img.Canvas.Brush.Color:=clWhite;
        Img.Canvas.FillRect(0,0,w,h);
        Img.Canvas.Font:=Canvas.Font;
        Img.Canvas.TextOut(0,0,TheText);
        // получаем картинку из памяти
        IntfImg:=Img.CreateIntfImage;
        // заменяем серые пиксели
        FontColor:=ColorToRGB(Canvas.Font.Color);
        for dy:=0 to h-1 do begin
          for dx:=0 to w-1 do begin
            col:=IntfImg.Colors[dx,dy];
            c:=FPColorToTColor(col);
            if c<>FontColor then
              IntfImg.Colors[dx,dy]:=colTransparent;
          end;
        end;
        // создаем растровую картинку
        Img.LoadFromIntfImage(IntfImg);
        // рисуем ее на холсте
        Canvas.Draw(x,y,Img);
      finally
        IntfImg.Free;
        Img.Free;
      end;
    end;

---

_Source: [https://wiki.freepascal.org/TCanvas/ru](https://web.archive.org/web/20230327103958/https://wiki.freepascal.org/TCanvas/ru)_
