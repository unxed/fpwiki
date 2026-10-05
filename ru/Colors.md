# Colors

│ **[English (en)](<../en/Colors.md>)** │  **русский (ru)** │

## Contents

  * 1 Обзор
    * 1.1 Преобразование значений между TColor и RGB
    * 1.2 Преобразование типа TColor в/из строки
    * 1.3 Преобразование типа TColor в/из HTML-строки #rrggbb
  * 2 Таблица стандартных цветов
  * 3 Системные цвета
    * 3.1 Пример: clInfoBk, clInfoText
    * 3.2 Изменения темы
    * 3.3 Таблица системных цветов
    * 3.4 Нахождение значений RGB системного цвета
  * 4 Рисование элементов соответственно теме на ваших пользовательских элементах управления
  * 5 См. также



## Обзор

В [LCL](<../en/LCL.md> "LCL") TColor - это стандартный тип цвета. Он совместим с TColor [Delphi](<../en/Delphi.md> "Delphi"). TColor может представляться _как_ в виде значения RGB (3x8bit), _так_ и в виде **системного** цвета наподобие **clDefault**. LCL также может работать с системой fpImage, которая использует тип TFPColor (RGBA (4x16bit), а не RGB (3x8bit), как TColor). 

### Преобразование значений между TColor и RGB

[Модуль](<../en/Unit.md> "Unit") **Graphics** предосталяет следующие [функции](<../en/Function.md> "Function"): 
    
    
    function ColorToRGB(Color: TColor): Longint;
    function Blue(rgb: TColor): BYTE; // не работает с системным цветом
    function Green(rgb: TColor): BYTE; // не работает с системным цветом
    function Red(rgb: TColor): BYTE; // не работает с системным цветом
    function RGBToColor(R, G, B: Byte): TColor;
    procedure RedGreenBlue(rgb: TColor; out Red, Green, Blue: Byte); // не работает с системным цветом
    function FPColorToTColor(const FPColor: TFPColor): TColor;
    function TColorToFPColor(const c: TColor): TFPColor; // не работает с системным цветом
    function IsSysColor(AColor: TColorRef): Boolean;
    

### Преобразование типа TColor в/из строки

Функции для преобразования строк вроде "25500" или "[$](<../en/Dollar_sign.md> "Dollar sign")AA0088" или "clNavy" в TColor: 

  * StringToColor
  * StringToColorDef



Чтобы преобразовать TColor в красивую строку, такую как "clNavy" или "$AA0002": 

  * ColorToString



### Преобразование типа TColor в/из HTML-строки #rrggbb

Смотрите код в [Преобразовать цвет в/из HTML](<../en/Convert_color_to/from_HTML.md> "Convert color to/from HTML"). 

## Таблица стандартных цветов

Предусмотрено около 20 предопределенных цветовых [Констант](<../en/Constant.md> "Constant"), которые совместимы с Delphi: 

Цветовая константа  | Описание цвета  | [Шестнадцатеричное](<Hexadecimal.md> "Hexadecimal/ru") представление для использования с TColor  | Пример   
---|---|---|---  
clBlack  | Черный  | TColor($000000);  |   
clMaroon  | Темно-бордовый  | TColor($000080);  |   
clGreen  | Зеленый  | TColor($008000);  |   
clOlive  | Оливково-зеленый  | TColor($008080);  |   
clNavy  | Темно-синий  | TColor($800000);  |   
clPurple  | Темно-фиолетовый  | TColor($800080);  |   
clTeal  | Сине-зеленый  | TColor($808000);  |   
clGray  | Серый  | TColor($808080);  |   
clSilver  | Серебристый  | TColor($C0C0C0);  |   
clRed  | Красный  | TColor($0000FF);  |   
clLime  | Салатовый  | TColor($00FF00);  |   
clYellow  | Желтый  | TColor($00FFFF);  |   
clBlue  | Синий  | TColor($FF0000);  |   
clFuchsia  | Фуксия  | TColor($FF00FF);  |   
clAqua  | Голубой  | TColor($FFFF00);  |   
clLtGray  | Светло-серый  | TColor($C0C0C0);  | псевдоним для clSilver   
clDkGray  | Темно-серый  | TColor($808080);  | псевдоним для clGray   
clWhite  | Белый  | TColor($FFFFFF);  |   
clCream  | Кремовый  | TColor($F0FBFF);  | Lazarus 1.2 и новее   
clMedGray  | Средне-серый  | TColor($A4A0A0);  | Lazarus 1.2 и новее   
clMoneyGreen  | Денежная зелень  | TColor($C0DCC0);  | Lazarus 1.2 и новее   
clSkyBlue  | Небесно-голубой  | TColor($F0CAA6);  | Lazarus 1.2 и новее   
  
## Системные цвета

### Пример: clInfoBk, clInfoText

Системные цвета - это цветовые константы с особым значением. Их реальное значение зависит от контекста и темы. Они не являются просто цветом. Например, **clInfoBk** : 
    
    
    Form1.Canvas.Brush.Color:=clInfoBk;  // используем цвет кисти по умолчанию для фона в окнах подсказок
    Form1.Canvas.FillRect(10,10,50,50);
    

Окно подсказок в MS Windows может иметь белый фон, поэтому вышеприведенный цвет будет отображаться белым. В Linux/gtk2 это может быть металлическая текстура, поэтому вышеприведенный будет рисовать текстуру. Если вы хотите поместить какой-либо текст в это окно, вам понадобится соответствующий контрастный цвет, например, **clInfoText** , иначе ваш текст может быть нечитаемым для пользователя. Например: 
    
    
    Form1.Canvas.Brush.Color:=clInfoBk;  // используем цвет кисти по умолчанию для фона в окнах подсказок
    Form1.Canvas.FillRect(10,10,50,50);
    Form1.Canvas.Font.Color:=clInfoText;  // используем цвет текста по умолчанию в окнах подсказок
    Form1.Canvas.TextOut(10,10,'Hint');
    

Системный цвет **clInfoBk** не может использоваться для Pen.Color и Font.Color. Если вы сделаете это, результат не определен и зависит от набора виджетов и пользовательской темы. То же самое для **clInfoText** : он может использоваться только как Font.Color. Использование его в качестве Brush.Color может не работать. На данный момент все виджеты позволяют использовать его как Pen.Color. 

### Изменения темы

Когда пользователь переключает тему, системные цвета меняются. **ClInfoBk** может измениться с белого на синий или с цвета на текстуру. Это изменение произойдет, когда вы назначите новый дескриптор кисти. Имейте в виду, что простое присвоение `Brush.Color:=clInfoBk` не назначает дескриптор кисти. Дескриптор кисти назначается в процессе использования. Например: 
    
    
    Form1.Canvas.Brush.Color:=clInfoBk; // это не создаст новый дескриптор кисти
    Form1.Canvas.FillRect(10,10,50,50); // а это создаст дескриптор кисти с текущей активной кистью темы для окон подсказок
    ...
    // если тема меняется в этот момент, Brush.Handle все еще заполнен старыми значениями
    ...
    Form1.Canvas.FillRect(10,10,50,50); // это все еще будет рисоваться кистью старой темы
    Form1.Canvas.Brush.Color:=clInfoBk; // присвоение старого значения не создаст новый дескриптор кисти
    Form1.Canvas.FillRect(10,10,50,50); // это все еще будет рисоваться кистью старой темы
    Form1.Canvas.Brush.Color:=clRed;    // присвоение нового цвета, старый Handle недействителен
    Form1.Canvas.Brush.Color:=clInfoBk; // присвоение нового цвета, старый Handle недействителен
    Form1.Canvas.FillRect(10,10,50,50); // это создаст новый дескриптор и нарисует новую тему
    

### Таблица системных цветов

В следующей таблице перечислены системные цвета и их значение. Использование их вне описанной области неоднозначно, и результат зависит от набора виджетов и темы. Например, **clDefault** \- обычная кисть фона используемого контекста устройства. Если вы хотите рисовать элементы кнопок на своих собственных элементах управления, используйте функции рисования модуля **Themes**. 

Константа | Определение LCL | Delphi notes | Поддерживаемые наборы виджетов   
---|---|---|---  
clNone | Обозначает «не раскрашивать». Использование его в качестве цвета элемента управления не определено. Элемент управления не станет прозрачным. | - | все   
clDefault | Означает, что применительно к кисти будет использоваться обычный фон целевого DC (контекст устройства). 

  * На холсте формы _FillRect_ закрашивает прямоугольную область, заполненную обычным фоном стандартного окна. Это то, что определяет набор виджетов и тема. Это может быть серый цвет, или градиент, или изображение.
  * Использование clDefault на холсте TListBox будет рисовать с нормальным фоном, который обычно белый в Windows. Таким образом, в TListBox clDefault совпадает с clWindow.
  * При использовании его в качестве цвета пера будет задействован цвет линии по умолчанию для контекста устройства.
  * При использование его в качестве цвета шрифта будет задействован обычный цвет текста контекста устройства.

| - | все   
clScrollBar | Тело полосы прокрутки | - | все   
clBackground | Цвет фона рабочего стола | - | все   
clActiveCaption | Строка заголовка активного окна | - | никакой   
clInactiveCaption | Строка заголовка неактивного окна | - | никакой   
clMenu | Обычный цвет пункта меню | - | никакой   
clWindow | Обычная фоновая кисть невыделенного текста. Определено для элементов управления, таких как TEdit, TComboBox, TMemo, TListBox, TTreeView. | - | никакой   
clWindowFrame | Цвет рамки вокруг окна | - | никакой   
clMenuText | Цвет шрифта для использования вместе с clMenu | - | никакой   
clWindowText | Цвет шрифта для использования вместе с clWindow | - | никакой   
clCaptionText | Цвет строки заголовка активного окна | - | никакой   
clActiveBorder | ? | - | никакой   
clInactiveBorder | ? | - | никакой   
clAppWorkspace | Фон формы MDIMain | - | никакой   
clHighlight | Цвет кисти выбранного элемента | - | никакой   
clHighlightText | Цвет шрифта выделенного текста (использовать вместе с clHighligh). | - | никакой   
clBtnFace | Фон кнопки | - | никакой   
clBtnShadow | Цвет тени кнопки (внизу справа), используемый для достижения эффекта 3D | - | никакой   
clGrayText | Цвет шрифта отключенного элемента | - | никакой   
clBtnText | Цвет шрифта кнопки для использования вместе с clBtnFace | - | никакой   
clInactiveCaptionText | Цвет текста заголовка неактивного окна | - | никакой   
clBtnHighlight | Цвет подсветки кнопки (вверху слева), используемый для достижения эффекта 3D | - | никакой   
cl3DDkShadow | ? | - | никакой   
cl3DLight | ? | - | никакой   
clInfoText | Цвет шрифта для окон с подсказками. Используется вместе с clInfoBk | - | все   
clInfoBk | Цвет кисти для окон с подсказками. Используется вместе с clInfoText | - | все   
clHotLight | ? | - | никакой   
clGradientActiveCaption | Второй цвет, используемый для создания градиента строки заголовка активного окна | - | никакой   
clGradientInactiveCaption | Второй цвет, используемый для создания градиента строки заголовка неактивного окна | - | никакой   
clMenuHighlight | Цвет фона выбранного пункта меню | - | никакой   
clMenuBar | Цвет фона строки меню | - | никакой   
clForm | ? | - | никакой   
clColorDesktop | ? | - | никакой   
cl3DFace | ? | - | никакой   
cl3DShadow | ? | - | никакой   
cl3DHiLight | ? | - | никакой   
clBtnHiLight | Такой же как у clBtnHighlight | - | никакой   
  
### Нахождение значений RGB системного цвета

Используйте функцию `ColorToRGB` (в модуле `Graphics`), чтобы определить компоненты rgb системного цвета. Эта функция определяет, является ли цвет системным цветом, и, если это так, находится ли системный цвет в цветовой гамме текущей темы. Возвращаемое значение типа [Longint](<../en/Longint.md> "Longint") можно понимать как обычный цвет: 
    
    
    // в этом примере имеется TColorBox на форме и используется обработчик OnChange для TColorbox:
    procedure TForm1.ColorBox1Change(Sender: TObject);
    var
      c: TColor;
    begin
      c := ColorToRGB(ColorBox1.Selected);
      Caption := Format('R%d G%d B%d', [Red(c), Green(c), Blue(c)]);
    end;
    

## Рисование элементов соответственно теме на ваших пользовательских элементах управления

Модуль **Themes** предоставляет функции для рисования отдельных элементов стандартных элементов управления. Например, чтобы нарисовать знак раскрытия узла, например, [TTreeView](<../en/TTreeView.md> "TTreeView"), используйте следующий код: 
    
    
    uses Themes;
    
    ...
    
    procedure TYourCustomControl.Paint;
    const
      PlusMinusDetail: array[Boolean {Hot}, Boolean {Expanded}] of TThemedTreeview =
      (
        (ttGlyphClosed, ttGlyphOpened),
        (ttHotGlyphClosed, ttHotGlyphOpened)
      );
    var
      Details: TThemedElementDetails;
      R: TRect;
      Collapse: boolean;
    begin
      ...
      //рисуем знак раскрытия узла дерева соответственно теме
      Details := ThemeServices.GetElementDetails(PlusMinusDetail[False, Collapse]);
      R := Rect(ALeft, ATop, ARight + 1, ABottom + 1);
      ThemeServices.DrawElement(Canvas.Handle, Details, R, nil);
      ...
    end;
    

## См. также

  * [Цвета в Delphi](<http://diversenok.ho.ua/articles/TColor>)
  * [Таблица названий цветов в английском языке](<http://www.falsefriends.ru/english-colors.htm>)

---

_Source: [https://wiki.freepascal.org/Colors/ru](https://web.archive.org/web/20250207042904/https://wiki.freepascal.org/Colors/ru)_
