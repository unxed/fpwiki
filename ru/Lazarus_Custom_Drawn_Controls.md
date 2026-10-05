# Lazarus Custom Drawn Controls

│ **[English (en)](<../en/Lazarus_Custom_Drawn_Controls.md> "Lazarus Custom Drawn Controls")** │  **русский (ru)** │    
****

**Lazarus Custom Drawn Controls** это набор элементов управления, эквивалентный стандартным элементам управления Lazarus, но для самостоятельного рисования. Они могут иметь множество применений, включая: возможность полностью настроить прорисовку, возможность одинаково выглядеть на разных платформах, а также более высокую согласованность поведения. 

Этот набор элементов управления разделен на две части: 

  * настраиваемые элементы управления, необходимые для реализации наборов виджетов Lazarus и расположенные в модуле lazarus/lcl/customdrawncontrols.pas. Здесь их определения.
  * все другие пользовательские элементы управления, которые используются часто, но не являются необходимыми для реализации набора пользовательских элементов управления LCL. Те находятся в пакете lazarus/components/customdrawn. Они описаны на странице [Lazarus Custom Drawn Package](<../en/Lazarus_Custom_Drawn_Package.md> "Lazarus Custom Drawn Package")



## Contents

  * 1 Как начать использовать эти компоненты?
  * 2 Как работают эти компоненты?
    * 2.1 Цветовая палитра
  * 3 Drawer'а
    * 3.1 dsCommon
    * 3.2 dsWinCE
    * 3.3 dsWin2000
    * 3.4 dsWinXP
    * 3.5 dsKDEPlastique
    * 3.6 dsAndroid
    * 3.7 dsWindows7
  * 4 Элементы управления, которые имитируют Standard Palette
    * 4.1 TCDButton
    * 4.2 TCDEdit
    * 4.3 TCDCheckBox
    * 4.4 TCDRadioButton
    * 4.5 TCDScrollBar
    * 4.6 TCDGroupBox
  * 5 Элементы управления, имитирующие Additional Palette
    * 5.1 TCDStaticText
  * 6 Элементы управления, имитирующие Common Controls Palette
    * 6.1 TCDTrackBar
    * 6.2 TCDProgressBar
    * 6.3 TCDListView
    * 6.4 TCDTabControl
    * 6.5 TCDPageControl
  * 7 Пакет Custom Drawn
  * 8 Другие хорошие custom drawn компоненты для Lazarus
  * 9 Сопровождающие
  * 10 Смотрите также



## Как начать использовать эти компоненты?

Чтобы использовать их в конструкторе графического интерфейса Lazarus, установите пакет lazarus/components/customdrawn.lpk 

Чтобы создавать компоненты **customdrawn** используя код, можно просто добавить **customdrawncontrols** , **customdrawndrawers** и **customdrawn_common** в раздел **uses** , чтобы объявить и использовать их. Обратите внимание, что в настоящее время в LCL находится Drawer (прорисовщик интерфейса) только для Win2000, поэтому любые другие Drawer'а необходимо получить из пакета customdrawn.lpk 

Важно отметить, что стили доступны только в том случае, если вы добавляете объявление соответствующего модуля стиля в раздел **uses**. 

## Как работают эти компоненты?

Базовые методы программирования, используемые этим набором элементов управления, объясняется на [Developing with Graphics#Create a custom control which draws itself](<../en/Developing_with_Graphics.md> "Developing with Graphics") странице. 

Код самого элемента управления реализован в модуле customdrawncontrols.pas, но этот модуль совсем не имеет кода для рисования. Customdrawncontrols.pas предоставляет код, который обрабатывает все события клавиатуры и мыши элемента управления, реализуя все действия мыши и реакции на нажатия клавиш. Каждый экземпляр элемента управления имеет связанный с ним Drawer. Drawer в нашем представлении (экземпляр класса TCDDrawer) похож на движок рендеринга или нечто подобное. Модуль **customdrawndrawers.pas** управляет списком всех известных Drawer'ов, а также объявляет все основные типы данных, необходимые для State и StateEx. Эти свойства содержат всю информацию о текущем состоянии элемента управления, которая передается в Drawer, чтобы он имел информацию, необходимую для правильного рисования элемента управления. В программе может существовать только один экземпляр каждого Drawer'а, и все элементы управления ссылаются на этот единственный экземпляр через менеджер в customdrawndrawers.pas. Каждый элемент управления имеет свойство DrawStyle, которое позволяет вам выбрать (из перечисляемого типа), какой стиль рисования использовать, а затем customdrawndrawers.pas преобразует эту информацию в экземпляр Drawer. Вы можете указать dsDefault DrawStyle, который затем будет использовать стиль, указанный в глобальной переменной DefaultStyle. Это было сделано для одновременного изменения стиля всех элементов управления. Просто измените эту глобальную переменную и затем вызовите Invalidate для элементов управления. Ниже вы можете увидеть фрагмент кода из customdrawndrawers.pas, который показывает методы, которые были представлены для управления списком Drawer'ов, а также стиль по умолчанию: 
    
    
    unit customdrawndrawers;
    //...
      TCDDrawStyle = (
        // Значение по умолчанию задается глобальной переменной DefaultStyle
        // Не использовать ничего для этого Drawer
        dsDefault = 0,
        // Это общий Drawer, основа для всех остальных. Он реализует вид от dsWin2000
        dsCommon,
        // Стили операционный систем
        dsWinCE, dsWin2000, dsWinXP,
        dsKDEPlastique, dsGNOME, dsMacOSX,
        dsAndroid,
        // Другие специальные стили для пользователя
        dsExtra1, dsExtra2, dsExtra3, dsExtra4, dsExtra5,
        dsExtra6, dsExtra7, dsExtra8, dsExtra9, dsExtra10
        );
    //...
    procedure RegisterDrawer(ADrawer: TCDDrawer; AStyle: TCDDrawStyle);
    function GetDefaultDrawer: TCDDrawer;
    function GetDrawer(AStyle: TCDDrawStyle): TCDDrawer;
    
    var
      DefaultStyle: TCDDrawStyle = dsCommon; // В начале по умолчанию стандартный, позже для каждой платформы
    

Каждый класс Drawer имеет методы для рисования всех элементов управления, а также всех доступных примитивов. Все новые Drawer'а, созданные пользователями, должны наследоваться от класса TCDCommonDrawer (который является основным общим Drawer, объявленным в модуле customdrawn_common.pas). Если Вы не будете наследоваться от TCDCommonDrawer, Ваше приложение может аварийно завершить работу, если оно попадет в абстрактный метод. TCDCommonDrawer наиболее полный блок, который реализует все абстрактные методы из своего базового класса, поэтому наследование от него гарантирует, что никогда не произойдет сбоев из-за методов, которые Вы еще не реализовали. Другие Drawer'а могут переопределять нужные методы для изменения стиля рисования. 

Стиль прорисовки dsCommon использует только TCanvas для всего своего рендеринга, но другие темы могут также использовать TLazIntfImage и TFPImageCanvas, чтобы иметь более быстрый доступ к пикселям. Этот метод программирования описан в [Developing with Graphics#Working with TLazIntfImage](<../en/Developing_with_Graphics.md> "Developing with Graphics"). 

### Цветовая палитра

Drawer'а также имеют доступ к цветовой палитре, которая имеет обычные системные цвета LCL. Drawer'а по умолчанию будут загружать свои цвета из операционной системы через LCL в соответствии с собственной платформой. Однако, за пределами своей родной платформы, ящики будут использовать стандартную цветовую палитру. 
    
    
      TCDColorPalette = class
      public
        ScrollBar, Background, ActiveCaption, InactiveCaption,
        Menu, Window, WindowFrame, MenuText, WindowText, CaptionText,
        ActiveBorder, InactiveBorder, AppWorkspace, Highlight, HighlightText,
        BtnFace, BtnShadow, GrayText, BtnText, InactiveCaptionText,
        BtnHighlight, color3DDkShadow, color3DLight, InfoText, InfoBk,
        //
        HotLight, GradientActiveCaption, GradientInactiveCaption,
        MenuHighlight, MenuBar, Form: TColor;
      end;
    
      { Существует 5 возможных источников ввода цветовых палитр:
       palDefault  - Использовать palNative, когда операционная система соответствует стилю Drawer, в противном случае - palFallback
       palNative   - Получить из операционной системы
       palFallback - Использовать запасные цвета Drawer
       palUserConfig-Загрузить из пользовательских файлов конфигурации, ToDo
       palCustom   - Пользовательское приложение установило свою палитру
      }
      TCDPaletteKind = (palDefault, palNative, palFallback, palUserConfig, palCustom);
    
      { TCDDrawer }
    
      TCDDrawer = class
      protected
      public
        Palette: TCDColorPalette;
        constructor Create; virtual;
        destructor Destroy; override;
        procedure SetPaletteKind(APaletteKind: TCDPaletteKind);
        procedure LoadNativePaletteColors;
        procedure LoadFallbackPaletteColors; virtual;
    

Как показывает приведенный выше код, Вы можете выбрать, откуда должны поступать цвета - либо из текущей темы операционной системы, либо из заданного по умолчанию Drawer, либо из пользовательской схемы, предоставленной Вашим приложением. Это позволяет вам изменять все цвета вашего приложения во время выполнения, просто изменяя палитру. 

## Drawer'а

### dsCommon

Это базовый Drawer для всех остальных, в нем реализован внешний вид Win2000 с некоторыми небольшими улучшениями, такими как лучшая фокусировка рисунков на элементах управления. 

### dsWinCE

Имитирует внешний вид Windows CE с некоторыми небольшими улучшениями, такими как улучшение фокусировки рисунков на элементах управления 

### dsWin2000

Имитирует внешний вид Windows 2000. Поскольку dsCommon обеспечивает внешний вид Windows 2000, то этот класс вообще не реализует собственные методы. 

### dsWinXP

Имитирует внешний вид Windows XP 

### dsKDEPlastique

Имитирует внешний вид темы Plastique Qt / KDE 

### dsAndroid

Имитирует внешний вид Android 

### dsWindows7

[![customdrawnwindows7.png](https://wiki.freepascal.org/images/7/78/customdrawnwindows7.png)](</File:customdrawnwindows7.png>)

Имитирует внешний вид Windows 7, а также может использоваться для имитации внешнего вида Windows XP и Windows 8, а также большинства стилей или оболочек Windows. В настоящее время доступен только с графической библиотекой BGRABitmap, и вы можете получить этот Drawer в пакете BGRAControls: [BGRAControls](<../en/BGRAControls.md> "BGRAControls")

Вы можете попытаться заставить это работать, если Вы конвертируете модуль slicescaling.pas. Техника, используемая для рисования изображений, - это 9-фрагментное масштабирование. 

Некоторый рабочий код для создания 9-фрагментного масштабирования доступен здесь: <http://sourceforge.net/p/bmpthemeutils/home/Home/>

## Элементы управления, которые имитируют Standard Palette

### TCDButton

Это кнопка, с полностью настраиваемой прорисовкой. 

Пример использования: 
    
    
    uses customdrawnextras;
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      MyButton: TCDButton;
    begin
      MyButton := TCDButton.Create(Self);
      MyButton.Parent := Self;
      MyButton.DrawStyle := dsWin2000;
      MyButton.Left := 100;
      MyButton.Top := 100;
      MyButton.Width := 200;
      MyButton.Height := 50;
      MyButton.Caption := 'My Button';
      MyButton.Color := clRed;
      MyButton.OnClick := @HandleButtonClick;
    end;
    

[![Custom drawn buttons.png](https://wiki.freepascal.org/images/5/54/Custom_drawn_buttons.png)](</File:Custom_drawn_buttons.png>)

### TCDEdit

Это текстовое поле, с полностью настраиваемой прорисовкой. 

[![TCDEdit.png](https://wiki.freepascal.org/images/b/bd/TCDEdit.png)](</File:TCDEdit.png>)

### TCDCheckBox

Это флажок, с полностью настраиваемой прорисовкой. 

[![TCDCheckBox.png](https://wiki.freepascal.org/images/2/20/TCDCheckBox.png)](</File:TCDCheckBox.png>)

### TCDRadioButton

Это переключатель, с полностью настраиваемой прорисовкой. 

[![TCDRadioButton.png](https://wiki.freepascal.org/images/3/34/TCDRadioButton.png)](</File:TCDRadioButton.png>)

### TCDScrollBar

Это панель прокрутки, с полностью настраиваемой прорисовкой. 

[![TCDScrollBar.png](https://wiki.freepascal.org/images/e/e8/TCDScrollBar.png)](</File:TCDScrollBar.png>)

### TCDGroupBox

Это групповой контейнер, с полностью настраиваемой прорисовкой. 

[![TCDGroupBox.png](https://wiki.freepascal.org/images/4/48/TCDGroupBox.png)](</File:TCDGroupBox.png>)

## Элементы управления, имитирующие Additional Palette

### TCDStaticText

## Элементы управления, имитирующие Common Controls Palette

### TCDTrackBar

Замена для TTrackBar 

[![TCDTrackBar.png](https://wiki.freepascal.org/images/b/b5/TCDTrackBar.png)](</File:TCDTrackBar.png>)

### TCDProgressBar

[![TCDProgressBar.png](https://wiki.freepascal.org/images/a/af/TCDProgressBar.png)](</File:TCDProgressBar.png>)

### TCDListView

В разработке. 

### TCDTabControl

Замена для TTabControl 

### TCDPageControl

Замена для TPageControl 

[![TCDPageControl.png](https://wiki.freepascal.org/images/a/ab/TCDPageControl.png)](</File:TCDPageControl.png>)

## Пакет Custom Drawn

Находится здесь: [Lazarus Custom Drawn Package](<../en/Lazarus_Custom_Drawn_Package.md> "Lazarus Custom Drawn Package")

## Другие хорошие custom drawn компоненты для Lazarus

  * [VirtualTreeview](<../en/VirtualTreeview.md> "VirtualTreeview")
  * [ATTabs](<../en/ATTabs.md> "ATTabs")
  * [ATStatusBar](<../en/ATStatusBar.md> "ATStatusBar")
  * [ATScrollBar](<../en/ATScrollBar.md> "ATScrollBar")



## Сопровождающие

  * [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
  * JiXian Yang



## Смотрите также

  * [Lazarus Components Directory](<../en/Lazarus_Components_Directory.md> "Lazarus Components Directory")
  * [Lazarus Custom Drawn Package](<../en/Lazarus_Custom_Drawn_Package.md> "Lazarus Custom Drawn Package")
  * [Custom Drawn Interface](<../en/Custom_Drawn_Interface.md> "Custom Drawn Interface")

---

_Source: [https://wiki.freepascal.org/Lazarus_Custom_Drawn_Controls/ru](https://web.archive.org/web/20231203213940/https://wiki.freepascal.org/Lazarus_Custom_Drawn_Controls/ru)_
