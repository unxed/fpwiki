# BGRABitmap

│ **[Deutsch (de)](</BGRABitmap/de> "BGRABitmap/de")** │  **[English (en)](<../en/BGRABitmap.md> "BGRABitmap")** │  **[español (es)](</BGRABitmap/es> "BGRABitmap/es")** │  **[français (fr)](</BGRABitmap/fr> "BGRABitmap/fr")** │  **русский (ru)** │  **[中文（中国大陆） (zh_CN)](</BGRABitmap/zh_CN> "BGRABitmap/zh CN")** │    
****

[![bgrabitmap logo.jpg](https://wiki.freepascal.org/images/7/7b/bgrabitmap_logo.jpg)](</File:bgrabitmap_logo.jpg>)

См.также: [Developing with Graphics](<Developing_with_Graphics.md> "Developing with Graphics/ru")

## Contents

  * 1 Описание
  * 2 Дополнительные пакеты
  * 3 Использование BGRABitmap
    * 3.1 Руководства
    * 3.2 Обзор
    * 3.3 Ссылка на модуль BGRABitmapTypes
    * 3.4 Установка
    * 3.5 Простой пример
  * 4 Понятия
  * 5 Встроенные функции рисования
  * 6 Рисование с помощью холста
  * 7 Прямой доступ к пикселям
  * 8 Манипуляции с изображениями
  * 9 Сочетание изображений
  * 10 Скриншоты
  * 11 Лицензия
  * 12 Скачать



### Описание

**BGRABitmap** \- это набор модулей, предназначенных для изменения и создания изображений с прозрачностью (альфа-канал). Прямой пиксельный доступ позволяет быстро обрабатывать изображения. Библиотека была протестирована на Windows, Ubuntu и Mac OS X (последняя версия не работает на Mac), с наборами виджетов win32, gtk1, gtk2 и carbon. 

Основным классом является [TBGRABitmap](<../en/TBGRABitmap_class.md> "TBGRABitmap class"), который является производным от [TFPCustomImage](<../en/fcl-image.md> "fcl-image"). Существует также [TBGRAPtrBitmap](</index.php?title=TBGRAPtrBitmap_class&action=edit&redlink=1> "TBGRAPtrBitmap class \(page does not exist\)"), позволяющий редактировать данные BGRA, которые уже распределены. Этот формат состоит из 4 байтов для каждого пикселя (синий, зеленый, красный и альфа в этом порядке). 

Изображение может быть отображено на обычном Canvas или на [OpenGL surface](<../en/BGRABitmap_and_OpenGL.md> "BGRABitmap and OpenGL"). 

### Дополнительные пакеты

Некоторые пакеты используют BGRABitmap для обеспечения элементов управления приятной графикой: 

  * BGLControls: предоставляет TBGLVirtualScreen для рисования на поверхности OpenGL. Этот пакет находится в архиве BGRABitmap.
  * [BGRAControls](<../en/BGRAControls.md> "BGRAControls"): ярлыки с тенями, красивыми кнопками, фигурами и т.д.
  * [uE Controls](<../en/uE_Controls.md> "uE Controls"): датчики, светодиоды и т.д.
  * [Material Design](<../en/Material_Design.md> "Material Design"): Компоненты пользовательского интерфейса, основанные на принципах разработки материалов Google.
  * BGRAControlsFX[[1]](<https://forum.lazarus.freepascal.org/index.php?topic=34534.0>): элементы управления рендеринга на поверхности OpenGL.



Некоторые примеры в папке с примерами используют BGRAControls и BGLControls. Вам может понадобиться установить их, чтобы открыть эти проекты в Lazarus. См. [Install Packages](<../en/Install_Packages.md> "Install Packages"). 

### Использование BGRABitmap

#### Руководства

  * [Как конвертировать приложение из TCanvas в BGRABitmap(англ.)](<http://www.youtube.com/watch?v=HGYSLgtYx-U>)
  * [Using BGRABitmap to render a TAChart](<../en/BGRABitmap_tutorial_TAChart.md> "BGRABitmap tutorial TAChart")
  * [A series of tutorials to learn step by step](<../en/BGRABitmap_tutorial.md> "BGRABitmap tutorial")
  * Примеры на [GitHub](<https://github.com/bgrabitmap>).
  * [Цикл уроков по компоненту BGRABitmap](<http://dropdoc.ru/Zyr2>) от пользователя [sign](<http://www.freepascal.ru/forum/memberlist.php?mode=viewprofile&u=9593>)



#### Обзор

Функции имеют длинные имена, чтобы быть понятными. Почти все доступно в виде функции или с помощью свойства объекта [TBGRABitmap](<../en/TBGRABitmap_class.md> "TBGRABitmap class"). Например, вы можете использовать CanvasBGRA, чтобы иметь некоторые холсты, похожие на TCanvas (с прозрачностью и сглаживанием) и Canvas2D, чтобы иметь те же функции, что и [HTML canvas](<https://developer.mozilla.org/en/HTML:Canvas>). 

Некоторые специальные функции требуют использования модулей, но они могут вам не понадобиться: 

  * TBGRAMultishapeFiller иметь сглаженные соединения многоугольников в BGRAPolygon
  * TBGRATextEffect в BGRATextFX
  * 2D преобразования находятся в BGRATransform
  * TBGRAScene3D в BGRAScene3D
  * Если вам нужны слои, BGRALayers предоставляет TBGRALayeredBitmap



Двойная буферизация на самом деле не является частью BGRABitmap, потому что она больше о том, как обрабатывать формы. Для двойной буферизации вы можете использовать TBGRAVirtualScreen, который находится в пакете [BGRAControls](<../en/BGRAControls.md> "BGRAControls"). Кроме того, двойная буферизация в BGRABitmap работает как любая двойная буферизация. Вам нужно иметь растровое изображение, в котором вы храните свой чертеж и которое вы отображаете с помощью одной инструкции Draw. 

#### Ссылка на модуль BGRABitmapTypes

  * [Pixel types and functions](<BGRABitmap_Pixel_types.md> "BGRABitmap Pixel types/ru"): _TBGRAPixel_ , _THSLAPixel_...
  * [Types imported from Graphics](<../en/BGRABitmap_Types_imported_from_Graphics.md> "BGRABitmap Types imported from Graphics"): _TColor_ , стиль пера...
  * [Color definitions](<../en/BGRABitmap_Color_definitions.md> "BGRABitmap Color definitions"): _VGAColors_ , _CSSColors_...
  * [Geometry types](<../en/BGRABitmap_Geometry_types.md> "BGRABitmap Geometry types"): _TPointF_ , кривые Берзье, _TArcDef_...
  * [Miscellaneous types](<../en/BGRABitmap_Miscellaneous_types.md> "BGRABitmap Miscellaneous types"): шрифт, формат изображения, ресэмплинг...
  * [TBGRACustomBitmap and IBGRAScanner](<../en/TBGRACustomBitmap_and_IBGRAScanner.md> "TBGRACustomBitmap and IBGRAScanner"): базовый класс для TBGRABitmap и сканеров



#### Установка

После распаковки архива BGRA часто не компилируется в Linux. Попробуйте использовать макрос IDE 
    
    
     LCLWidgetType:=gtk2
    

в таких случаях. Тем не менее, некоторые другие части могут не скомпилироваться. 

#### Простой пример

Создайте проект и откройте bgrabitmappackage.lpk с помощью Lazarus. В окне пакета нажмите "Use > Add to Project". Затем в исходном коде основного файла (основная форма или основная программа) добавьте в раздел uses модуль BGRABitmap. Вам также может понадобиться добавить модуль BGRAGraphics, если вы используете определенные типы, которые унаследованы от LCL. 
    
    
    Uses 
      Classes, SysUtils, BGRABitmap, BGRABitmapTypes;
    

Модуль BGRABitmapTypes содержит общие определения, но можно объявить только BGRABitmap для загрузки и отображения растрового изображения. Затем первым шагом является создание объекта [TBGRABitmap](<../en/TBGRABitmap_class.md> "TBGRABitmap class"): 
    
    
    var
      bmp: TBGRABitmap;
    begin
      bmp := TBGRABitmap.Create(100, 100, BGRABlack); // создаем изображение размером 100x100 пикселей с черным фоном
    
      bmp.FillRect(20, 20, 60, 60, BGRAWhite, dmSet); // рисуем белый квадрат без прозрачности
      bmp.FillRect(40, 40, 80, 80, BGRA(0, 0, 255, 128), dmDrawWithTransparency); // рисуем прозрачный синий квадрат
      ...
    end;
    

Наконец, чтобы показать растровое изображение: 
    
    
    procedure TFMain.FormPaint(Sender: TObject);
    begin
      bmp.Draw(Canvas, 0, 0, True); // рисуем растровое изображение в непрозрачном режиме (быстрее)
    end;
    

Смотрите полный исходный код в [tutorial 1](<../en/BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1"). 

### Понятия

Пиксели в растровом изображении с прозрачностью хранятся с 4 значениями, здесь 4 байта [расположены] в порядке Blue, Green, Red, Alpha. Последний канал определяет уровень непрозрачности (0 означает прозрачный, 255 означает непрозрачный), другие каналы определяют цвет и яркость. 

Есть два основных режима рисования. Первый состоит в замене содержимого информации пикселя, второй состоит в смешении уже имеющегося пикселя с новым, который называется альфа-смешением. 

Функции BGRABitmap предлагают 4 режима: 

  * dmSet : заменяет четыре байта нарисованного пикселя, прозрачность не обрабатывается.
  * dmDrawWithTransparency : рисует с альфа-каналом и с гамма-коррекцией (см. ниже).
  * dmFastBlend или dmLinearBlend : рисует с альфа-каналом, но без гамма-коррекции (быстрее, но влечет за собой искажения цвета с низкой интенсивностью).
  * dmXor : применяет Xor к каждому компоненту, включая альфа (если вы хотите инвертировать цвет, но сохранить альфа, используйте BGRA (255,255,255,0)).



### Встроенные функции рисования

  * рисование/стирание пикселей
  * рисование линии с или без сглаживания
  * координаты с плавающей точкой
  * ширина пера с плавающей точкой
  * прямоугольник (рамка или заливка)
  * эллипс и многоугольники со сглаживанием
  * расчет сплайна (скругленная кривая)
  * простая заливка (Floodfill) или прогрессивная заливка
  * отрисовка цветового градиента (линейная, радиальная ...)
  * скругленные прямоугольники
  * тексты с прозрачностью



### Рисование с помощью холста

Можно рисовать с помощью объекта _Canvas_ , с обычными функциями, но без сглаживания. Непрозрачность рисования определяется свойством _CanvasOpacity_. Этот способ медленнее, потому что он требует преобразования изображения. Если вы можете, используйте вместо этого _CanvasBGRA_ , который позволяет [использовать] прозрачность и сглаживание, имея те же имена функций, что и в TCanvas. 

### Прямой доступ к пикселям

Для доступа к пикселям есть два свойства: _Data_ и _Scanline_. Первый дает указатель на первый пиксель изображения, а второй дает указатель на первый пиксель данной строки. 
    
    
    var 
      bmp: TBGRABitmap;
      p: PBGRAPixel;
      n: integer;
    
    begin
      bmp := TBGRABitmap.Create('image.png');
      p := bmp.Data;
      for n := bmp.NbPixels-1 downto 0 do
      begin
        p^.red := not p^.red; // инвертируем красный канал
        inc(p);
      end;
      bmp.InvalidateBitmap;   // обратите внимание, что мы получили доступ к пикселям напрямую
      bmp.Draw(Canvas, 0, 0, True);
      bmp.Free;
    end;
    

Необходимо вызвать функцию _InvalidateBitmap_ , чтобы пересобрать изображение, например, при следующем вызове _Draw_. Обратите внимание, что порядок строк может быть обратным, в зависимости от свойства _LineOrder_. 

Смотрите также сравнение между [direct pixel access methods](<../en/Fast_direct_pixel_access.md> "Fast direct pixel access"). 

### Манипуляции с изображениями

Доступные фильтры (с префиксом Filter): 

  * Radial blur : ненаправленное размытие
  * Motion blur : направленное размытие
  * Custom blur : размытие по маске


  * Median : вычисляет медиану цветов вокруг каждого пикселя, что смягчает углы
  * Pixelate : упрощает изображение с прямоугольниками одного цвета
  * Smooth : смягчает все изображение, дополнительно к [фильтру] Sharpen
  * Sharpen :делает контуры более четкими, дополнительно к [фильтру] Smooth


  * Contour : рисует контуры на белом фоне (как карандашный рисунок)
  * Emboss : рисует контуры с тенью
  * EmbossHighlight : рисует контуры выделения, определенного в оттенках серого


  * Grayscale : преобразует цвета в оттенки серого с гамма-коррекцией
  * Normalize : использует весь спектр яркости цвета


  * Rotate : вращение изображения вокруг точки
  * Sphere : искажает изображение, чтобы оно выглядело, как проецируемое на сферу
  * Twirl : искажает изображение с эффектом закручивания
  * Cylinder : искажает изображение, чтобы оно выглядело, как проецируемое на цилиндр
  * Plane : вычисляет проекцию с высокой точностью на горизонтальную плоскость. Это довольно медленно.
  * SmartZoom3 : изменяет размер изображения х3 и определяет границы, чтобы иметь полезный зум с древними игровыми спрайтами



Некоторые функции не имеют префикса Filter, потому что они не возвращают вновь выделенное изображение. Они изменяют изображение на месте: 

  * VerticalFlip : переворачивает изображение по вертикали
  * HorizontalFlip : переворачивает изображение по горизонтали
  * Negative : инвертирует цвета
  * LinearNegative : инверсия без гамма-коррекции
  * SwapRedBlue : меняет местами красный и синий каналы (для конвертации между BGRA и RGBA)
  * ConvertToLinearRGB : конвертирует из sRGB в RGB. Обратите внимание, что формат, используемый BGRABitmap - **sRGB** при использовании _dmDrawWithTransparency_ и **RGB** при использовании _dmLinearBlend_.
  * ConvertFromLinearRGB : конвертирует из RGB в sRGB.



### Сочетание изображений

PutImage является основной функцией рисования изображений, а BlendImage позволяет комбинировать изображения, как слои программ для редактирования изображений. Доступны следующие режимы: 

  * LinearBlend : простое наложение без гамма-коррекции (эквивалент dmFastBlend)
  * Transparent : наложение с гамма-коррекцией
  * Multiply : умножение значений цвета (с гамма-коррекцией)
  * LinearMultiply : умножение значений цвета (без гамма-коррекции)
  * Additive : добавление значений цвета (с гамма-коррекцией)
  * LinearAdd : добавление значений цвета (без гамма-коррекции)
  * Difference : разница значений цвета (с гамма-коррекцией)
  * LinearDifference : разница значений цвета (без гамма-коррекции)
  * Negation : устранение общих цветов (с гамма-коррекцией)
  * LinearNegation : исчезновение общих цвета (без гамма-коррекции)
  * Reflect, Glow : для световых эффектов
  * ColorBurn, ColorDodge, Overlay, Screen : разные фильтры
  * Lighten : сохраняет самые светлые цветовые значения
  * Darken : сохраняет самые темные значения цвета
  * Xor : исключающее ИЛИ цветовых значений



Эти режимы можно использовать в TBGRALayeredBitmap, что упрощает эту задачу, поскольку BlendImage предоставляет только основные операции смешивания. 

### Скриншоты

  * [![Lazpaint contour.png](https://wiki.freepascal.org/images/5/5d/Lazpaint_contour.png)](</File:Lazpaint_contour.png>)



  


  * [![Lazpaint curve redim.png](https://wiki.freepascal.org/images/2/25/Lazpaint_curve_redim.png)](</File:Lazpaint_curve_redim.png>)



  


  * [![Bgra wirecube.png](https://wiki.freepascal.org/images/2/23/Bgra_wirecube.png)](</File:Bgra_wirecube.png>)



  


  * [![Bgra chessboard.jpg](https://wiki.freepascal.org/images/b/b0/Bgra_chessboard.jpg)](</File:Bgra_chessboard.jpg>)



  


### Лицензия

модифицированная LGPL 

Автор: [Johann ELSASS](<http://johann-elsass.net>) ([Facebook](<http://www.facebook.com/johann.elsass.1>)) 

### Скачать

Последняя версия: <https://github.com/bgrabitmap/bgrabitmap/releases>

Sourceforge with [LazPaint](<../en/LazPaint.md> "LazPaint"): <http://sourceforge.net/projects/lazpaint/files/src/>

GitHub: <https://github.com/bgrabitmap/>

---

_Source: [https://wiki.freepascal.org/BGRABitmap/ru](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/BGRABitmap/ru)_
