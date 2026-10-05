# Developing with Graphics

│ **[English (en)](<../en/Developing_with_Graphics.md>)** │  **русский (ru)** │

Эта страница описывает основные классы и технологии рисования графики в Lazarus. Какие-то специфические вещи ищите в других статьях. 

## Contents

  * 1 Графические библиотеки
  * 2 Другие статьи по графике
    * 2.1 2D-рисование
    * 2.2 3D-рисование
    * 2.3 Диаграммы
  * 3 Введение в графическую модель LCL
  * 4 Работа с TCanvas
    * 4.1 Рисование прямоугольника
    * 4.2 Использование шрифта GUI по умолчанию
    * 4.3 Рисование ограниченного по ширине текста
    * 4.4 Рисование текста с резкими краями (без сглаживания)
  * 5 Работа с TBitmap и другими потомками TGraphic
    * 5.1 Загрузка/Сохранение изображения из/на диск
    * 5.2 Дополнительные форматы файлов для TImage
    * 5.3 Прямой доступ к пикселям
    * 5.4 Рисование растровых изображений прозрачным цветом
    * 5.5 Получение скриншота экрана
  * 6 Работа с TLazIntfImage, TRawImage и TLazCanvas
    * 6.1 Инициализация TLazIntfImage
    * 6.2 TLazIntfImage.LoadFromFile
    * 6.3 Загрузка TLazIntfImage в TImage
    * 6.4 Пример обесцвечивания
    * 6.5 Пример конкретного формата изображения
    * 6.6 Преобразование между TLazIntfImage и TBitmap
    * 6.7 Использование не-нативного StretchDraw из LazCanvas
  * 7 Графика движения - как избежать мерцания
    * 7.1 Рисование на TImage
      * 7.1.1 Изменение размера растрового изображения TImage
      * 7.1.2 Рисование на растровом изображении TImage
      * 7.1.3 Рисование на изменчивой визуальной области TImage
    * 7.2 Рисование в событии OnPaint
    * 7.3 Создание пользовательского элемента управления с самостоятельной отрисовкой
  * 8 Форматы изображений
    * 8.1 Преобразование форматов
  * 9 Пиксельные форматы
    * 9.1 TColor
    * 9.2 TFPColor
  * 10 Рисование с помощью fcl-image
  * 11 Распространенная ошибка [при использовании] OnPaint
  * 12 Несколько полезных примеров
    * 12.1 Пример 1: Рисование на загруженном JPEG с [помощью] TImage
    * 12.2 Пример 2: Рисование на элементах управления формы
  * 13 См. также



## Графические библиотеки

[Графические библиотеки](<../en/Graphics_libraries.md> "Graphics libraries") \- здесь вы можете посмотреть, какие есть основные графические библиотеки. 

## Другие статьи по графике

### 2D-рисование

  * [ZenGL](<ZenGL.md> "ZenGL/ru") \- кроссплатформенная библиотека для разработки игр на основе OpenGL.
  * [BGRABitmap](<BGRABitmap.md> "BGRABitmap/ru") \- Рисование фигур, прозрачных изображений, прямой доступ к пикселям и др.
  * [LazRGBGraphics](<../en/LazRGBGraphics.md> "LazRGBGraphics") \- Пакет для быстрой обработки изображения в памяти и работы с пикселями (например, scan line).
  * [fpvectorial](<../en/fpvectorial.md> "fpvectorial") \- Предоставляет возможность работы с векторной графикой.
  * [Double Gradient](<../en/Double_Gradient.md> "Double Gradient") \- Рисуйте легко растровые изображения 'double gradient' и 'n-gradient'.
  * [Gradient Filler](<../en/Gradient_Filler.md> "Gradient Filler") \- TGradientFiller - лучший способ создания пользовательских n-градиентов в Lazarus.
  * [PascalMagick](<../en/PascalMagick.md> "PascalMagick") \- простой в использовании API для взаимодействия с [ImageMagick](<http://www.imagemagick.org>), многоплатформенным пакетом бесплатного программного обеспечения для создания, редактирования и создания растровых изображений.
  * [Sample Graphics](<../en/Sample_Graphics.md> "Sample Graphics") \- графическая галерея, созданная с помощью Lazarus и инструментов для рисования.
  * [Fast direct pixel access](<../en/Fast_direct_pixel_access.md> "Fast direct pixel access") \- сравнение скорости некоторых методов для прямого доступа к растровому пикселю.
  * [AggPas](<http://www.crossgl.com/aggpas/>) \- это нативный порт Object Pascal библиотеки Anti-Grain Geometry. Он быстр и очень мощен [при работе] со сглаженным рисунком и субпиксельной точностью. Вы можете думать об AggPas как о механизме рендеринга, который создает пиксельные изображения в памяти из некоторых векторных данных.



### 3D-рисование

  * [GLScene](<../en/GLScene.md> "GLScene") \- Порт 3D визуальной графической библиотеки [GLScene OpenGL](<http://www.glscene.org>)



### Диаграммы

  * [TAChart](<../en/TAChart.md> "TAChart") \- компонента для рисования диаграмм в Lazarus
  * [PlotPanel](<../en/PlotPanel.md> "PlotPanel") \- Создание и черчение анимированных графиков
  * [Perlin Noise](<../en/Perlin_Noise.md> "Perlin Noise") \- статья об использовании Perlin Noise в LCL приложениях.



## Введение в графическую модель LCL

LCL предоставляет два вида классов рисования: нативные [(собственные)] классы и не-нативные [(сторонние)] классы. Нативные графические классы являются наиболее традиционным способом рисования графики в LCL, а также являются наиболее важными, в то время как не-нативные классы являются дополнительными, но также очень важными. Собственные классы в основном расположены в модуле **Graphics** LCL и являются хорошо известными классами: TBitmap, TCanvas, TFont, TBrush, TPen, TPortableNetworkGraphic и т.д. 

TCanvas - это класс, способный выполнять рисование. Он не может существовать один и должен быть либо прикреплен к чему-то видимому (или, по крайней мере, который может быть видимым), например, к визуальному элементу управления, происходящему из TControl, либо должен быть присоединен к внеэкранному буферу потомка от TRasterImage (TBitmap является наиболее часто использующимся). TFont, TBrush и TPen описывают, как рисование различных операций будет выполняться в Canvas. 

TRasterImage (обычно используется через его потомка TBitmap) - это область памяти, зарезервированная для рисования графики, но она создана для максимальной совместимости с собственным Canvas и, следовательно, в LCL-Gtk2 в X11 она расположена на сервере X11, который обеспечивает попиксельный доступ через свойство Pixels чрезвычайно медленно. В Windows это очень быстро, потому что Windows позволяет создавать локально выделенное изображение, которое может получать рисунки из Windows Canvas. 

Помимо них существуют также не-нативные классы рисования, расположенные в графическом типе модулей (TRawImage), intfgraphics (TLazIntfImage) и lazcanvas (TLazCanvas, этот существует в Lazarus [с версии] 0.9.31+). TRawImage - это хранилище и описание области памяти, которая содержит изображение. TLazIntfImage - это изображение, которое присоединяется к TRawImage и обеспечивает преобразование между TFPColor и форматом настоящего пикселя TRawImage. TLazCanvas - это не нативный Canvas, который может рисовать изображение в TLazIntfImage. 

Основное различие между нативными и не-нативными классами состоит в том, что нативные классы не выполняются одинаково на всех платформах, потому что рисование выполняется самой базовой платформой. Скорость, а также точный конечный результат рисования изображения могут иметь различия. Не-нативные классы гарантированно выполняют одинаковое рисование на всех платформах с точностью до пикселя, и все они работают достаточно быстро на всех платформах. 

В наборе виджетов LCL-CustomDrawn нативные классы реализованы с использованием не-нативных классов. 

Все эти классы будут лучше описаны в разделах ниже. 

## Работа с TCanvas

### Рисование прямоугольника

Многие элементы управления, например TForm, TPanel или TPaintbox, отображают свой холст как общедоступное свойство или событие OnPaint. Давайте используем TForm в качестве примера, чтобы продемонстрировать, как рисовать на холсте. 

Предположим, мы хотим нарисовать красный прямоугольник с синей рамкой толщиной 5 пикселей в центре формы; размер прямоугольника должен составлять половину размера формы. Для этого мы должны добавить код в событие OnPaint формы. Не рисуйте в обработчике OnClick, потому что это рисование не является постоянным и будет стираться всякий раз, когда операционная система запрашивает перерисовку, всегда рисуйте в событии OnPaint! 

Метод TCanvas для рисования прямоугольника вызывается именно так: `Rectangle()`. Он получает координаты краев прямоугольника либо отдельно, либо в виде записи `TRect`. Цвет заливки определяется цветом кисти Холста, а цвет границы задается цветом пера холста: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      w, h: Integer;    // Ширина и высота прямоугольника
      cx, cy: Integer;  // центр формы
      R: TRect;         // запись, содержащая координаты левого, верхнего, правого, нижнего углов прямоугольника
    begin
      // Высчитываем центр формы
      cx := Width div 2;
      cy := Height div 2;
    
      // Рассчитываем размер прямоугольника
      w := Width div 2;
      h := Height div 2;
    
      // Рассчитываем углы прямоугольника
      R.Left := cx - w div 2;
      R.Top := cy - h div 2;
      R.Right := cx + w div 2;
      R.Bottom := cy + h div 2;
    
      // Устанавливаем цвет заливки
      Canvas.Brush.Color := clRed;
      Canvas.Brush.Style := bsSolid;
    
      // Устанавливаем цвет границы
      Canvas.Pen.Color := clBlue;
      Canvas.Pen.Width := 5;
      Canvas.Pen.Style := psSolid;
    
      // Рисуем прямоугольник
      Canvas.Rectangle(R);
    end;
    

### Использование шрифта GUI по умолчанию

Это можно сделать с помощью этого простого кода: 
    
    
    SelectObject(Canvas.Handle, GetStockObject(DEFAULT_GUI_FONT));
    

### Рисование ограниченного по ширине текста

Используйте процедуру DrawText, сначала с DT_CALCRECT, а затем без него. 
    
    
    // Сначала рассчитываем размер текста, затем рисуем его
    TextBox := Rect(0, currentPos.Y, Width, High(Integer));
    DrawText(ACanvas.Handle, PChar(Text), Length(Text), TextBox, DT_WORDBREAK or DT_INTERNAL or DT_CALCRECT);
    
    DrawText(ACanvas.Handle, PChar(Text), Length(Text), TextBox, DT_WORDBREAK or DT_INTERNAL);
    

### Рисование текста с резкими краями (без сглаживания)

Некоторые виджеты поддерживают это через 
    
    
    Canvas.Font.Quality := fqNonAntialiased;
    

Некоторые виджеты, такие как gtk2, не поддерживают это и всегда рисуют сглаживание. Вот простая процедура рисования текста с резкими краями под gtk2. Она не предусматривает все случаи, но должна дать представление: 
    
    
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
        // рисуем текст в растровое изображение
        Img.Masked:=true;
        Img.SetSize(w,h);
        Img.Canvas.Brush.Style:=bsSolid;
        Img.Canvas.Brush.Color:=clWhite;
        Img.Canvas.FillRect(0,0,w,h);
        Img.Canvas.Font:=Canvas.Font;
        Img.Canvas.TextOut(0,0,TheText);
        // получаем изображение в памяти
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
        // создаем растровое изображение
        Img.LoadFromIntfImage(IntfImg);
        // рисуем
        Canvas.Draw(x,y,Img);
      finally
        IntfImg.Free;
        Img.Free;
      end;
    end;
    

## Работа с TBitmap и другими потомками TGraphic

Объект TBitmap хранит растровое изображение, где вы можете рисовать, прежде чем показывать его на экране. Когда вы создаете растровое изображение, вы должны указать высоту и ширину, иначе это будет ноль, и ничто не будет нарисовано. И вообще, все остальные потомки TRasterImage предоставляют те же возможности. Следует использовать тот, который соответствует формату, необходимому для вывода/ввода с диска или TBitmap, в случае, если операции с дисками не будут выполняться, а также для формата Windows Bitmap (*.bmp). 

### Загрузка/Сохранение изображения из/на диск

Чтобы загрузить изображение с диска, используйте [TGraphic.LoadFromFile](<http://lazarus-ccr.sourceforge.net/docs/lcl/graphics/tgraphic.loadfromfile.html>) и для сохранения его на другом диске, используйте [TGraphic.SaveToFile](<http://lazarus-ccr.sourceforge.net/docs/lcl/graphics/tgraphic.savetofile.html>). Используйте соответствующий потомок TGraphic, который соответствует ожидаемому формату. См. [форматы изображений](<Developing_with_Graphics.md> "Developing with Graphics/ru") для [просмотра] списка доступных классов формата изображения. 
    
    
    var
      MyBitmap: TBitmap;
    begin
      MyBitmap := TBitmap.Create;
      try
        // Загрузка с диска
        MyBitmap.LoadFromFile(MyEdit.Text);
    
        // Здесь вы можете использовать MyBitmap.Canvas для чтения/записи в/из изображения
    
        // Записываем обратно на другой диск
        MyBitmap.SaveToFile(MyEdit2.Text);
      finally
        MyBitmap.Free;
      end;
    end;
    

При использовании любого другого формата процесс полностью идентичен, просто используйте соответствующий класс. Например, для изображений PNG: 
    
    
    var
      MyPNG: TPortableNetworkGraphic;
    begin
      MyPNG := TPortableNetworkGraphic.Create;
      try
        // Загрузка с диска
        MyPNG.LoadFromFile(MyEdit.Text);
    
        // Здесь вы можете использовать MyPNG.Canvas для чтения/записи в/из изображения
    
        // Записываем обратно на другой диск
        MyPNG.SaveToFile(MyEdit2.Text);
      finally
        MyPNG.Free;
      end;
    end;
    

Если вы заранее не знаете формат изображения, используйте TPicture, который определит формат на основе расширения файла. Обратите внимание, что TPicture не поддерживает все форматы, поддерживаемые Lazarus, с Lazarus 0.9.31 он поддерживает BMP, PNG, JPEG, Pixmap и PNM, в то время как Lazarus также поддерживает ICNS и другие форматы: 
    
    
    var
      MyPicture: TPicture;
    begin
      MyPicture := TPicture.Create;
      try
        // Загрузка с диска
        MyPicture.LoadFromFile(MyEdit.Text);
    
        // Здесь вы можете использовать MyPicture.Graphic.Canvas для чтения/записи в/из изображения
    
        // Записываем обратно на другой диск
        MyPicture.SaveToFile(MyEdit2.Text);
      finally
        MyPicture.Free;
      end;
    end;
    

### Дополнительные форматы файлов для TImage

Вы можете добавить дополнительную поддержку форматов файлов, добавив модули [fcl-image](<../en/fcl-image.md> "fcl-image") fpread* и/или fpwrite* в вашу секцию uses. Таким образом, вы можете, например, добавить поддержку TIFF для TImage 

### Прямой доступ к пикселям

Для непосредственного доступа к пикселям растровых изображений можно использовать внешние библиотеки, такие как [BGRABitmap](<../en/BGRABitmap.md> "BGRABitmap"), [LazRGBGraphics](<../en/LazRGBGraphics.md> "LazRGBGraphics") и [Graphics32](<Current_conversion_projects.md> "Current conversion projects/ru"), или использовать нативный Lazarus'овский TLazIntfImage. Сравнение методов доступа к пикселям см. [fast direct pixel access](<../en/Fast_direct_pixel_access.md> "Fast direct pixel access"). 

В некоторых наборах виджетов Lazarus (в частности, LCL-Gtk2) данные растрового изображения не сохраняются в памяти, к которой может обращаться приложение, и в общем случае собственные интерфейсы LCL рисуются только через собственные процедуры Canvas, поэтому каждая операция SetPixel / GetPixel включает медленный вызов родного Canvas API. В LCL-CustomDrawn это не так, поскольку растровое изображение локально сохраняется для всех бэкэндов, а SetPixel / GetPixel работает быстро. Для получения решения, которое работает во всех наборах виджетов, следует использовать TLazIntfImage. Поскольку Lazarus должен быть независимым от платформы и работать в gtk2, класс TBitmap не предоставляет такое свойство, как Scanline. Существует функция GetDataLineStart, эквивалентная Scanline, но доступная только для образов памяти, таких как [TLazIntfImage](<Developing_with_Graphics.md> "Developing with Graphics/ru"), которая внутренне использует TRawImage. 

Подводя итог, можно сказать, что со стандартным TBitmap вы можете только косвенно изменять пиксели, используя TCanvas.Pixels. Вызов собственного API для рисования / чтения отдельного пикселя, конечно, медленнее, чем прямой доступ к пикселям, особенно в LCL-gtk2 и LCL-Carbon. 

* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): Есть весьма [неплохая статья](<https://stackoverflow.com/questions/13583451/how-to-use-scanline-property-for-24-bit-bitmaps>) (правда, на английском, чуть позже сделаю перевод и выложу на какой-нибудь ресурс), объясняющая суть работы свойства Scanline. 

* * *

### Рисование растровых изображений прозрачным цветом

Новая функция, реализованная в Lazarus 0.9.11, - это растровые изображения с прозрачными цветами. В растровых файлах (*.BMP) не может храниться информация о прозрачности, но они могут работать так же, как если бы вы выбрали цвет для представления прозрачной области. Это распространенный прием, используемый в приложениях Win32. 

В следующем примере загружается растровое изображение из ресурса Windows, выбирается прозрачный цвет (clFuchsia) и затем он рисуется на холсте. 
    
    
    procedure MyForm.MyButtonOnClick(Sender: TObject);
    var
      buffer: THandle;
      bmp: TBitmap;
      memstream: TMemoryStream;
    begin
      bmp := TBitmap.Create;
    
      buffer := Windows.LoadBitmap(hInstance, MAKEINTRESOURCE(ResourceID));
    
      if (buffer = 0) then exit; // Ошибка загрузки растрового изображения
    
      bmp.Handle := buffer;
      memstream := TMemoryStream.create;
      try
        bmp.SaveToStream(memstream);
        memstream.position := 0;
        bmp.LoadFromStream(memstream);
      finally
        memstream.free;
      end;
    
      bmp.Transparent := True;
      bmp.TransparentColor := clFuchsia;
    
      MyCanvas.Draw(0, 0, bmp);
    
      bmp.Free; // Освобождаем выделенный ресурс
    end;
    

Обратите внимание на операции с памятью, выполненные с помощью [TMemoryStream](<http://lazarus-ccr.sourceforge.net/docs/rtl/classes/tmemorystream.html> "doc:rtl/classes/tmemorystream.html"). Они необходимы для обеспечения правильной загрузки изображения. 

### Получение скриншота экрана

Начиная с Lazarus 0.9.16, вы можете использовать LCL, чтобы делать скриншоты экрана кросс-платформенным способом. Следующий пример кода делает это (работает на gtk2 и win32, но не на gtk1 в настоящее время): 
    
    
    uses Graphics, LCLIntf, LCLType;
      ...
    var
      MyBitmap: TBitmap;
      ScreenDC: HDC;
    begin
      MyBitmap := TBitmap.Create;
      ScreenDC := GetDC(0);
      MyBitmap.LoadFromDevice(ScreenDC);
      ReleaseDC(0,ScreenDC);
      ...
    

## Работа с TLazIntfImage, TRawImage и TLazCanvas

TLazIntfImage не является нативным эквивалентом TRasterImage (чаще используется в форме потомка TBitmap). Первое, что нужно знать об этом классе, это то, что в отличие от TBitmap, он не будет автоматически выделять область памяти для растрового изображения, сначала нужно инициализировать область памяти, а затем передать ее в TLazIntfImage. Сразу после создания TLazIntfImage нужно либо подключить его к TRawImage, либо загрузить из TBitmap. 

TRawImage относится к типу объект и поэтому не нуждается ни в создании, ни в освобождении. Он может либо выделить память для самого изображения при вызове TRawImage.CreateData, либо передать блок памяти, выделенный, например, сторонней библиотекой, такой как Windows API Cocoa Framework из Mac OS X, и передать информацию об изображении в TRawImage.Description, TRawImage.Data и TRawImage.DataSize. Вместо того, чтобы прикреплять его к RawImage, можно также загрузить его из TBitmap, который будет копировать данные из TBitmap и впоследствии не будет синхронизироваться с ним. TLazCanvas не может существовать один и всегда должен быть присоединен к TLazIntfImage. 

В приведенном ниже примере показано, как выбрать формат для данных и попросить TRawImage создать его для нас, а затем мы присоединим его к TLazIntfImage и потом присоединим к нему TLazCanvas: 
    
    
    uses graphtype, intfgraphics, lazcanvas;
    
    var
      AImage: TLazIntfImage;
      ACanvas: TLazCanvas;
      lRawImage: TRawImage;
    begin
        lRawImage.Init;
        lRawImage.Description.Init_BPP32_A8R8G8B8_BIO_TTB(AWidth, AHeight);
        lRawImage.CreateData(True);
        AImage := TLazIntfImage.Create(0,0);
        AImage.SetRawImage(lRawImage);
        ACanvas := TLazCanvas.Create(AImage);
    

### Инициализация TLazIntfImage

Нельзя просто создать экземпляр TLazIntfImage и начать его использовать. Нужно добавить к нему хранилище. Есть 3 способа сделать это: 

1\. Прикрепить его к TRawImage 

2\. Загрузить его из TBitmap. Обратите внимание, что он скопирует память TBitmap, чтобы [экземпляр TLazIntfImage] не оставался подключенным к нему: 
    
    
       SrcIntfImg:=TLazIntfImage.Create(0,0);
       SrcIntfImg.LoadFromBitmap(ABitmap.Handle,ABitmap.MaskHandle);
    

3\. Загрузить его из описания необработанного изображения, например так: 
    
    
       IntfImg := TLazIntfImage.Create(0,0);
       IntfImg.DataDescription:=GetDescriptionFromDevice(0);
       IntfImg.SetSize(10,10);
    

Устройство 0 в **GetDescriptionFromDevice(0)** использует текущий формат экрана. 

### TLazIntfImage.LoadFromFile

Вот пример того, как загрузить изображение непосредственно в TLazIntfImage. Он инициализирует TLazIntfImage в 32-битном формате RGBA. Имейте в виду, что это, вероятно, не собственный формат вашего экрана. 
    
    
    uses LazLogger, Graphics, IntfGraphics, GraphType;
    procedure TForm1.FormCreate(Sender: TObject);
    var
      AImage: TLazIntfImage;
      lRawImage: TRawImage;
    begin
      // создаем TLazIntfImage с 32 битами на пиксель, альфа 8 бит, красный 8 бит, зеленый 8 бит, синий 8 бит,
      // порядок битов: бит 0 - это пиксель 0, сверху вниз: строка 0 - это верх
      lRawImage.Init;
      lRawImage.Description.Init_BPP32_A8R8G8B8_BIO_TTB(0,0);
      lRawImage.CreateData(false);
      AImage := TLazIntfImage.Create(0,0);
      try
        AImage.SetRawImage(lRawImage);
        // Загружаем изображение с диска.
        // Используется расширение файла для правильного выбора изображения зарегистрированным ридером.
        // AImage будет масштабирован до  ширины и высоты загруженного изображения.
        AImage.LoadFromFile('lazarus/examples/openglcontrol/data/texture1.png');
        debugln(['TForm1.FormCreate ',AImage.Width,' ',AImage.Height]);
      finally
        AImage.Free;
      end;
    end;
    

### Загрузка TLazIntfImage в TImage

Пиксельные данные для **TImage** \- это свойство **TImage.Picture** , которое имеет тип _TPicture_. **TPicture** \- это мультиформатный контейнер, содержащий один из нескольких распространенных форматов изображений, таких как Bitmap, Icon, Jpeg или PNG. Обычно вы будете использовать _TPicture.Bitmap_ для загрузки **TLazIntfImage** : 
    
    
        Image1.Picture.Bitmap.LoadFromIntfImage(IntfImg);
    

**Примечание:**

  * Чтобы загрузить **прозрачный** TLazIntfImage, вы должны установить для **Image1.Transparent** значение true.
  * TImage использует формат экрана. Если TLazIntfImage имеет другой формат, то пиксели будут преобразованы.



Подсказка: вы можете использовать **IntfImg.DataDescription:=GetDescriptionFromDevice(0);** для инициализации TLazIntfImage с форматом экрана. 

### Пример обесцвечивания

Пример обесцвечивания с [использованием] TLazIntfImage 
    
    
    { Этот код был взят из $LazarusPath/examples/lazintfimage/fadein1.lpi project. }
    uses LCLType, // HBitmap type
         IntfGraphics, // TLazIntfImage type
         fpImage; // TFPColor type
    ...
     procedure TForm1.FadeIn(ABitMap: TBitMap);
     var
       SrcIntfImg, TempIntfImg: TLazIntfImage;
       ImgHandle,ImgMaskHandle: HBitmap;
       FadeStep: Integer;
       px, py: Integer;
       CurColor: TFPColor;
       TempBitmap: TBitmap;
     begin
       SrcIntfImg:=TLazIntfImage.Create(0,0);
       SrcIntfImg.LoadFromBitmap(ABitmap.Handle,ABitmap.MaskHandle);
       TempIntfImg:=TLazIntfImage.Create(0,0);
       TempIntfImg.LoadFromBitmap(ABitmap.Handle,ABitmap.MaskHandle);
       TempBitmap:=TBitmap.Create;
       for FadeStep:=1 to 32 do begin
         for py:=0 to SrcIntfImg.Height-1 do begin
           for px:=0 to SrcIntfImg.Width-1 do begin
             CurColor:=SrcIntfImg.Colors[px,py];
             CurColor.Red:=(CurColor.Red*FadeStep) shr 5;
             CurColor.Green:=(CurColor.Green*FadeStep) shr 5;
             CurColor.Blue:=(CurColor.Blue*FadeStep) shr 5;
             TempIntfImg.Colors[px,py]:=CurColor;
           end;
         end;
         TempIntfImg.CreateBitmaps(ImgHandle,ImgMaskHandle,false);
         TempBitmap.Handle:=ImgHandle;
         TempBitmap.MaskHandle:=ImgMaskHandle;
         Canvas.Draw(0,0,TempBitmap);
       end;
       SrcIntfImg.Free;
       TempIntfImg.Free;
       TempBitmap.Free;
     end;
    

### Пример конкретного формата изображения

Если вы знаете, что TBitmap использует синий 8-битный, зеленый 8-битный, красный 8-битный [каналы], вы можете напрямую получить доступ к байтам, что несколько быстрее: 
    
    
    uses LCLType, // HBitmap type
         IntfGraphics, // TLazIntfImage type
         fpImage; // TFPColor type
    ...
    type
      TRGBTripleArray = array[0..32767] of TRGBTriple;
      PRGBTripleArray = ^TRGBTripleArray;
    
    procedure TForm1.FadeIn2(aBitMap: TBitMap);
     var
       IntfImg1, IntfImg2: TLazIntfImage;
       ImgHandle,ImgMaskHandle: HBitmap;
       FadeStep: Integer;
       px, py: Integer;
       CurColor: TFPColor;
       TempBitmap: TBitmap;
       Row1, Row2: PRGBTripleArray;
     begin
       IntfImg1:=TLazIntfImage.Create(0,0);
       IntfImg1.LoadFromBitmap(aBitmap.Handle,aBitmap.MaskHandle);
    
       IntfImg2:=TLazIntfImage.Create(0,0);
       IntfImg2.LoadFromBitmap(aBitmap.Handle,aBitmap.MaskHandle);
    
       TempBitmap:=TBitmap.Create;
       
       //со Scanline-подобным [свойством]
       for FadeStep:=1 to 32 do begin
         for py:=0 to IntfImg1.Height-1 do begin
           Row1 := IntfImg1.GetDataLineStart(py); //как Delphi TBitMap.ScanLine
           Row2 := IntfImg2.GetDataLineStart(py); //как Delphi TBitMap.ScanLine
           for px:=0 to IntfImg1.Width-1 do begin
             Row2^[px].rgbtRed:= (FadeStep * Row1^[px].rgbtRed) shr 5;
             Row2^[px].rgbtGreen := (FadeStep * Row1^[px].rgbtGreen) shr 5; // Исчезновение
             Row2^[px].rgbtBlue := (FadeStep * Row1^[px].rgbtBlue) shr 5;
           end;
         end;
         IntfImg2.CreateBitmaps(ImgHandle,ImgMaskHandle,false);
         
         TempBitmap.Handle:=ImgHandle;
         TempBitmap.MaskHandle:=ImgMaskHandle;
         Canvas.Draw(0,0,TempBitmap);
       end; 
    
       IntfImg1.Free;
       IntfImg2.Free;
       TempBitmap.Free;
     end;
    

### Преобразование между TLazIntfImage и TBitmap

Поскольку Lazarus не имеет свойства TBitmap.ScanLines, лучший способ быстрого доступа к пикселям изображения для чтения и записи - использование TLazIntfImage. TBitmap можно преобразовать в TLazIntfImage с помощью TBitmap.CreateIntfImage (), а после изменения пикселей он может быть преобразован обратно в TBitmap с помощью TBitmap.LoadFromIntfImage (); Вот пример того, как создать TLazIntfImage из TBitmap, изменить его и затем вернуться к TBitmap. 
    
    
    uses
      ...GraphType, IntfGraphics, LCLType, LCLProc,  LCLIntf ...
    
    procedure TForm1.Button4Click(Sender: TObject);
    var
      b: TBitmap;
      t: TLazIntfImage;
    begin
      b := TBitmap.Create;
      try
        b.LoadFromFile('test.bmp');
        t := b.CreateIntfImage;
    
        // Читаем и/или записываем в пиксели
        t.Colors[10,20] := colGreen;
    
        b.LoadFromIntfImage(t);
      finally
        t.Free;
        b.Free;
      end;
    end;
    

### Использование не-нативного StretchDraw из LazCanvas

Как и в случае с TCanvas.StretchDraw, существует TLazCanvas.StretchDraw, но вам нужно указать интерполяцию, которую вы хотите использовать. Интерполяция, которая обеспечивает Windows-подобный StretchDraw с очень резким результатом (противоположным сглаживанию), может быть добавлена с помощью: `TLazCanvas.Interpolation: = TFPSharpInterpolation.Create;`

В модуле fpcanvas доступны другие [способы] интерполяции. 
    
    
    uses intfgraphics, lazcanvas;
    
    procedure TForm1.StretchDrawBitmapToBitmap(SourceBitmap, DestBitmap: TBitmap; DestWidth, DestHeight: integer);
    var
      DestIntfImage, SourceIntfImage: TLazIntfImage;
      DestCanvas: TLazCanvas;
    begin
      // Подготовка получателя
    
      DestIntfImage := TLazIntfImage.Create(0, 0);
      DestIntfImage.LoadFromBitmap(DestBitmap.Handle, 0);
    
      DestCanvas := TLazCanvas.Create(DestIntfImage);
    
      //Подготовка источника
      SourceIntfImage := TLazIntfImage.Create(0, 0);
      SourceIntfImage.LoadFromBitmap(SourceBitmap.Handle, 0);
    
      // Выполнение процесса растяжения [рисунка] с помощью TFPSharpInterpolation
      DestCanvas.Interpolation := TFPSharpInterpolation.Create;
      DestCanvas.StretchDraw(0, 0, DestWidth, DestHeight, SourceIntfImage);
    
      // Перезагрузка изображения в TBitmap
      DestBitmap.LoadFromIntfImage(DestIntfImage);
    
      SourceIntfImage.Free;
      DestCanvas.Interpolation.Free;  
      DestCanvas.Free;
      DestIntfImage.Free;
    end;
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      Bmp, DestBitmap: TBitmap;
    begin
      // Подготовка получателя
      DestBitmap := TBitmap.Create;
      DestBitmap.Width := 100;
      DestBitmap.Height := 100;
    
      Bmp := TBitmap.Create;
      Bmp.Width := 10;
      Bmp.Height := 10;
      Bmp.Canvas.Pen.Color := clYellow;
      Bmp.Canvas.Brush.Color := clYellow;
      Bmp.Canvas.Rectangle(0, 0, 10, 10);
      StretchDrawBitmapToBitmap(Bmp, DestBitmap, 100, 100);
      Canvas.Draw(0, 0, Bmp);
      Canvas.Draw(100, 100, DestBitmap);
    end;
    

## Графика движения - как избежать мерцания

Многие программы выводят свои данные в графический интерфейс в виде 2D-графики. Если эту графику нужно быстро менять, вы скоро столкнетесь с проблемой: быстро меняющаяся графика часто мерцает на экране. Это происходит, когда пользователи иногда видят целые изображения, а иногда только когда они частично прорисованы. Это происходит потому, что процесс отрисовки требует времени. 

Но как я могу избежать мерцания и получить лучшую скорость рисования? Конечно, вы можете работать с аппаратным ускорением, используя OpenGL, но этот подход довольно тяжел для небольших программ или старых компьютеров. Этот урок будет сосредоточен на рисовании в TCanvas. Если вам нужна помощь с OpenGL, взгляните на пример, который поставляется с Lazarus. Вы также можете использовать игровой пакет A.J. Venter'а, который предоставляет холст с двойной буферизацией и спрайт-компонент. 

Краткую и очень полезную статью по предотвращению мерцания можно найти [здесь](<http://delphi.about.com/library/bluc/text/uc052102g.htm>). Хотя эти методы написаны для Delphi, они хорошо работают [и] с Lazarus. 

Теперь мы рассмотрим варианты рисования на холсте: 

  * [Рисование на TImage](<Developing_with_Graphics.md> "Developing with Graphics/ru")
  * [Рисование в событии OnPaint формы, TPaintBox или другого элемента управления](<Developing_with_Graphics.md> "Developing with Graphics/ru")
  * [Создание пользовательского элемента управления с самостоятельной отрисовкой](<Developing_with_Graphics.md> "Developing with Graphics/ru")



### Рисование на TImage

TImage состоит из 2 частей: TGraphic, обычно TBitmap, в котором хранится постоянное изображение и визуальная область, которые перекрашиваются на каждом OnPaint. Изменение размера TImage **не** изменяет размер растрового изображения. Графическое изображение (или растровое изображение) доступно через Image1.Picture.Graphic (или Image1.Picture.Bitmap). Холст - это Image1.Picture.Bitmap.Canvas. Холст визуальной области TImage доступен только во время [наступления события] Image1.OnPaint через Image1.Canvas. 

**Важно** : Никогда не используйте событие OnPaint [компонента] Image1 для рисования в графическом/растровом изображении TImage. Графическое изображение TImage буферизуется, поэтому все, что вам нужно сделать, это нарисовать его из любого места, и изменения будут там навсегда. Однако, если вы постоянно перерисовываете, изображение будет мерцать. В этом случае вы можете попробовать другие варианты. Рисование в TImage считается медленнее, чем другие подходы. 

#### Изменение размера растрового изображения TImage

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Не используйте это в событии OnPaint.
    
    
    with Image1.Picture.Bitmap do begin
      Width:=100;
      Height:=120;
    end;
    

То же самое в один присест: 
    
    
    with Image1.Picture.Bitmap do begin
      SetSize(100, 120);
    end;
    

#### Рисование на растровом изображении TImage

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Не используйте это в событии OnPaint.
    
    
    with Image1.Picture.Bitmap.Canvas do begin
      // заполняем все растровое изображение красным
      Brush.Color := clRed;
      FillRect(0, 0, Width, Height);
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Внутри Image1.OnPaint'а Image1.Canvas указывает на изменчивую видимую область. За пределами Image1.OnPaint'а объект Image1.Canvas указывает на Image1.Picture.Bitmap.Canvas.

Другой пример: 
    
    
    procedure TForm1.BitBtn1Click(Sender: TObject);
    var
      x, y: Integer;
    begin
      // Рисуем фон
      MyImage.Canvas.Pen.Color := clWhite;
      MyImage.Canvas.Rectangle(0, 0, Image.Width, Image.Height);
       
      // Рисуем квадраты
      MyImage.Canvas.Pen.Color := clBlack;
      for x := 1 to 8 do
        for y := 1 to 8 do
          MyImage.Canvas.Rectangle(Round((x - 1) * Image.Width / 8), Round((y - 1) * Image.Height / 8),
            Round(x * Image.Width / 8), Round(y * Image.Height / 8));
    end;
    

#### Рисование на изменчивой визуальной области TImage

Вы можете рисовать только в этой области в событии OnPaint. [Событие] OnPaint в конечном итоге автоматически вызывается [библиотекой] LCL, когда область перерисовывается. Вы можете перерисовать область вручную с помощью Image1.Invalidate. Это вызовет OnPaint не сразу, и вы можете вызывать Invalidate столько раз, сколько захотите. 
    
    
    procedure TForm.Image1Paint(Sender: TObject);
    begin
      // рисуем линию
      Canvas.Pen.Color := clRed;
      Canvas.Line(0, 0, Width, Height);
    end;
    

### Рисование в событии OnPaint

В этом случае все рисование должно выполняться по событию OnPaint формы или другого элемента управления. Рисунок не буферизуется, как в TImage, и его необходимо полностью перерисовывать при каждом вызове обработчика события OnPaint. 
    
    
    procedure TForm.Form1Paint(Sender: TObject);
    begin
      // рисуем линию
      Canvas.Pen.Color := clRed;
      Canvas.Line(0, 0, Width, Height);
    end;
    

### Создание пользовательского элемента управления с самостоятельной отрисовкой

Создание пользовательского элемента управления имеет преимущество структурирования вашего кода, и вы можете использовать его повторно. Этот подход очень быстрый, но он все равно может вызывать мерцание, если вы сначала не рисуете на TBitmap'е, а лишь затем [начнете] рисовать на холсте. В этом случае нет необходимости использовать событие OnPaint элемента управления. 

Вот пример пользовательского элемента управления: 
    
    
    uses
      Classes, SysUtils, Controls, Graphics, LCLType;
     
    type
      TMyDrawingControl = class(TCustomControl)
      public
        procedure EraseBackground(DC: HDC); override;
        procedure Paint; override;
      end;
     
    implementation
     
    procedure TMyDrawingControl.EraseBackground(DC: HDC);
    begin
      // Раскомментируйте это, чтобы включить удаление фона по умолчанию
      //inherited EraseBackground(DC);
    end; 
     
    procedure TMyDrawingControl.Paint;
    var
      x, y: Integer;
      Bitmap: TBitmap;
    begin
      Bitmap := TBitmap.Create;
      try
        // Инициализируем размер растрового изображения
        Bitmap.Height := Height;
        Bitmap.Width := Width;
     
        // Рисуем фон
        Bitmap.Canvas.Pen.Color := clWhite;
        Bitmap.Canvas.Rectangle(0, 0, Width, Height);
     
        // Рисует квадраты
        Bitmap.Canvas.Pen.Color := clBlack;
        for x := 1 to 8 do
          for y := 1 to 8 do
            Bitmap.Canvas.Rectangle(Round((x - 1) * Width / 8), Round((y - 1) * Height / 8),
              Round(x * Width / 8), Round(y * Height / 8));
           
        Canvas.Draw(0, 0, Bitmap);
      finally
        Bitmap.Free;
      end;
     
      inherited Paint;
    end;
    

и как мы создаем это на форме: 
    
    
    procedure TMyForm.FormCreate(Sender: TObject);
    begin
      MyDrawingControl := TMyDrawingControl.Create(Self);
      MyDrawingControl.Height := 400;
      MyDrawingControl.Width := 500;
      MyDrawingControl.Top := 0;
      MyDrawingControl.Left := 0;
      MyDrawingControl.Parent := Self;
      MyDrawingControl.DoubleBuffered := True;
    end;
    

Он уничтожается автоматически, потому что мы используем Self в качестве владельца. 

Установка Top и Left на ноль не обязательна, так как это стандартная позиция, но это делается для того, чтобы усилить то место, где будет помещен элемент управления. 

"`MyDrawingControl.Parent := Self;`" - очень важная [строка кода], и вы не увидите свой элемент управления, если вы этого не сделаете. 

"`MyDrawingControl.DoubleBuffered := True;`" требуется, чтобы избежать мерцания в Windows. Это не [оказывает никакого] влияения на GTK. 

## Форматы изображений

Вот таблица с правильным классом для каждого формата изображения. 

Format | Image class | Unit   
---|---|---  
Cursor (cur) | TCursor | Graphics   
Bitmap (bmp) | TBitmap | Graphics   
Windows icon (ico) | TIcon | Graphics   
Mac OS X icon (icns) | TicnsIcon | Graphics   
Pixmap (xpm) | TPixmap | Graphics   
Portable Network Graphic (png) | TPortableNetworkGraphic | Graphics   
JPEG (jpg, jpeg) | TJpegImage | Graphics   
PNM (pnm) | TPortableAnyMapGraphic | Graphics   
Tiff (tif) | TTiffImage | Graphics   
  
См. также список [fcl-image supported formats](<../en/fcl-image.md> "fcl-image"). 

### Преобразование форматов

Иногда необходимо преобразовать один графический тип в другой. Одним из способов является преобразование графического объекта в промежуточный формат, а затем преобразование его в TBitmap. Большинство форматов могут создавать изображения из TBitmap. 

Преобразование растрового изображения в PNG и сохранение его в файл: 
    
    
    procedure SaveToPng(const bmp: TBitmap; PngFileName: String);
    var
      png : TPortableNetworkGraphic; 
    begin 
      png := TPortableNetworkGraphic.Create;
      try
        png.Assign(bmp);
        png.SaveToFile(PngFileName);
      finally 
        png.Free;
      end;
    end;
    

## Пиксельные форматы

### TColor

Внутренний пиксельный формат для TColor в LCL - это формат XXBBGGRR, который соответствует собственному формату Windows и противоположен большинству других библиотек, которые используют AARRGGBB. Часть XX используется для определения того, является ли цвет фиксированным цветом, в этом случае XX должен быть 00 или это индекс системного цвета. Нет места для альфа-канала. 

Для преобразования из отдельных каналов RGB в TColor используйте: 
    
    
    RGBToColor(RedVal, GreenVal, BlueVal);
    

Чтобы получить каждый канал переменной TColor, используйте функции Red, Green и Blue: 
    
    
    RedVal := Red(MyColor);
    GreenVal := Green(MyColor);
    BlueVal := Blue(MyColor);
    

### TFPColor

TFPColor использует формат AARRGGBB, общий для большинства библиотек, но он использует 16-битную глубину каждого цветового канала, что составляет 64 бита на пиксель, что является необычным. Однако это не обязательно означает, что изображения будут занимать столько памяти. Изображения, созданные с помощью TRawImage + TLazIntfImage, могут иметь любой внутренний формат хранения, а затем при операциях рисования TFPColor преобразуется в этот внутренний формат. 

Модуль Graphics предоставляет процедуры для преобразования между TColor и TFPColor: 
    
    
    function FPColorToTColorRef(const FPColor: TFPColor): TColorRef;
    function FPColorToTColor(const FPColor: TFPColor): TColor;
    function TColorToFPColor(const c: TColorRef): TFPColor; overload;
    function TColorToFPColor(const c: TColor): TFPColor; overload; // не работает для системного цвета
    

## Рисование с помощью fcl-image

Вы можете рисовать изображения, которые не будут отображаться на экране без LCL, просто используя fcl-image напрямую. Например, программа, работающая на веб-сервере без X11, может выиграть от отсутствия визуальной библиотеки в качестве зависимости. FPImage (псевдоним fcl-image) - это очень общая библиотека изображений и графики, полностью написанная на паскале. Фактически, LCL использует FPImage также для всех загрузок и сохранений из/в файлы и реализует функцию рисования через вызовы наборов виджетов (winapi, gtk, carbon, ...). Fcl-изображение с другой стороны также имеет процедуры рисования. 

Для получения дополнительной информации, пожалуйста, прочитайте статью о [fcl-image](<../en/fcl-image.md> "fcl-image"). 

## Распространенная ошибка [при использовании] OnPaint

Распространенной ошибкой, которая вызывает много ложных сообщений об багах, является вызов события Onpaint для одного объекта из другого объекта. При использовании LCL это может работать в GTK2 и Windows, но, вероятно, не удастся с Qt, Carbon и Cocoa. Обычно не нужно вызывать `Invalidate`. Тем не менее, иногда это может быть необходимо в процедуре Button1Click, 

вот так - плохо: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      Shape1Paint(Self); // Вызов события Shape1Onpaint
      Shape1.Invalidate; // Вызываем фактическую перерисовку
    
      ... больше кода для Button1 ...  
    end;
    

Вот так - хорошо: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      ... код для Button1 ... 
      <Устанавливаем некоторые условия>; 
      // Shape1.Invalidate; // Может быть необходимо в некоторых случаях
    end;
    
    // Shape1Paint должен быть прописан в  событии OnPaint объекта shape!
    procedure TForm1.Shape1Paint(Sender: TObject);
    var
      Myrect: TRect;
    begin   
      if <некоторое условие> then 
        with Shape1.Canvas do
        begin
          ... куча кода ...
        end;
    end;
    

## Несколько полезных примеров

### Пример 1: Рисование на загруженном JPEG с [помощью] TImage

Добавьте процедуру LoadAndDraw в секцию public вашей формы и вставьте следующий код в раздел implementation: 
    
    
    procedure TForm1.LoadAndDraw(const sFileName: String);
    var 
      jpg: TPicture;
    begin
      jpg := TPicture.Create;
      try
        jpg.LoadFromFile(sFileName);
     
        Image.Picture.Bitmap.SetSize(jpg.Width, jpg.Height);
        Image.Picture.Bitmap.Canvas.Draw(0, 0, jpg.Bitmap);
     
        Image.Picture.Bitmap.Canvas.Pen.Color := clRed;
        Image.Picture.Bitmap.Canvas.Line(0, 0, 140, 140);
      finally
        jpg.Free;
      end;
    end;
    

### Пример 2: Рисование на элементах управления формы

1) Создайте новый проект New project -> Application, добавьте в раздел uses следующие модули, если необходимо: Types, Controls, Graphics. 

2) Положите на форму Button1, GroupBox1 и RadioGroup1 

3) Положите на GroupBox1 еще одну кнопку - Button2 

4) Ваша TForm1.Create должна выглядеть так: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      i: Integer;
    begin
      for i := 0 to Self.ControlCount - 1 do
        RadioGroup1.Items.AddObject(Controls[i].Name, Controls[i]);
    
      RadioGroup1.Items.AddObject(Button2.Name,Button2);
    end;
    

5) Для RadioGroup1 создайте обработчик события OnSelectionChanged: 
    
    
    procedure TForm1.RadioGroup1SelectionChanged(Sender: TObject);
    begin
      Self.Repaint;
    end;
    

6) Добавьте в секцию public вашей формы процедуру HighlightControl: 
    
    
    procedure TForm1.HighlightControl(AControl: TControl);
    var
      R: Types.TRect;
      aCC: TControlCanvas;
    begin
      R := AControl.BoundsRect;
      InflateRect(R, 2, 2);          // сделайте прямоугольник немного больше, чем элемент управления
      aCC := TControlCanvas.Create;
      aCC.Control := AControl.Parent;
      aCC.Pen.Color := clGreen;
      aCC.Pen.Width := 5;
      aCC.Pen.Style := psSolid;
      aCC.Brush.Style := bsClear;
      aCC.Rectangle(R);
      aCC.free;
    end;
    

## См. также

  * [Fast direct pixel access](<../en/Fast_direct_pixel_access.md> "Fast direct pixel access")

---

_Source: [https://wiki.freepascal.org/Developing_with_Graphics/ru](https://web.archive.org/web/20250219204825/https://wiki.freepascal.org/Developing_with_Graphics/ru)_
