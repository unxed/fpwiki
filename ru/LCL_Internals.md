# LCL Internals

│ **[English (en)](<../en/LCL_Internals.md>)** │  **русский (ru)** │

## Contents

  * 1 Другие интерфейсы
    * 1.1 Советы для конкретных платформ
    * 1.2 Статьи по разработке интерфейсов
  * 2 Минимальные версии Toolkit
  * 3 Внутренности LCL
    * 3.1 Pie и RadialPie
    * 3.2 Интерфейсы
    * 3.3 Добавление нового модуля в LCL
  * 4 Как создать новый Widgetset
    * 4.1 Как реализовать новый оконный компонент
    * 4.2 Реализация TBitmap
    * 4.3 TBitmap.LoadFromDevice для снятия скриншота
    * 4.4 Реализация рисования в событии OnPaint формы или другого элемента управления
    * 4.5 Внедрение TLabel
    * 4.6 Реализация видимости для форм и элементов управления и состояния окна
      * 4.6.1 Видимость для форм и состояния окна
      * 4.6.2 Видимость для элементов управления
    * 4.7 Реализация компонентов на основе TStrings
    * 4.8 Реализация меню
      * 4.8.1 Порядок создания меню
    * 4.9 Включение/отключение элемента управления
    * 4.10 Фигурные окна
    * 4.11 Системные цвета
    * 4.12 ShowMessage
    * 4.13 SpinEdit
    * 4.14 Буфер обмена
    * 4.15 TSpeedButton
    * 4.16 TRadioButton и TCheckButton
    * 4.17 Поддержка FullScreen
  * 5 Пример работы интерфейсов LCL



## Другие интерфейсы

  * [Lazarus known issues (things that will never be fixed)](<../en/Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- Список проблем совместимости интерфейса
  * [Win32/64 Interface](<../en/Win32/64_Interface.md> "Win32/64 Interface") \- Интерфейс winapi для Windows 95/98/Me/2K/XP/Vista, но не для CE
  * [Windows CE Interface](<../en/Windows_CE_Interface.md> "Windows CE Interface") \- Для карманных ПК и смартфонов
  * [Carbon Interface](<../en/Carbon_Interface.md> "Carbon Interface") \- Интерфейс Carbon для macOS
  * [Cocoa Interface](<../en/Cocoa_Interface.md> "Cocoa Interface") \- Интерфейс Cocoa для macOS
  * [Qt Interface](<../en/Qt_Interface.md> "Qt Interface") \- Интерфейс Qt4 для Unix, macOS, Windows и КПК на базе Linux
  * [Qt5 Interface](<../en/Qt5_Interface.md> "Qt5 Interface") \- Интерфейс Qt5 для Unix, macOS, Windows и КПК на базе Linux
  * [GTK1 Interface](<../en/GTK1_Interface.md> "GTK1 Interface") \- Интерфейс gtk1 для Unix, macOS (X11), Windows
  * [GTK2 Interface](<../en/GTK2_Interface.md> "GTK2 Interface") \- Интерфейс gtk2 для Unix, macOS (X11), Windows
  * [GTK3 Interface](<../en/GTK3_Interface.md> "GTK3 Interface") \- Интерфейс gtk3 для Unix, macOS (X11), Windows
  * [fpGUI Interface](<../en/fpGUI_Interface.md> "fpGUI Interface") \- Основан на библиотеке fpGUI, которая представляет собой кроссплатформенный инструментарий, полностью написанный на Object Pascal
  * [Custom Drawn Interface](<../en/Custom_Drawn_Interface.md> "Custom Drawn Interface") \- Кроссплатформенный бэкэнд LCL, написанный полностью на Object Pascal внутри Lazarus. Интерфейс Lazarus для Android.



### Советы для конкретных платформ

  * [Windows Programming Tips](<../en/Windows_Programming_Tips.md> "Windows Programming Tips") \- Cоветы по программированию на desktop-Windows.
  * [Linux Programming Tips](<../en/Linux_Programming_Tips.md> "Linux Programming Tips") \- Как выполнять определенные задачи программирования в Linux
  * [macOS Programming Tips](<../en/macOS_Programming_Tips.md> "macOS Programming Tips") \- Советы по Lazarus, полезные инструменты, команды Unix и многое другое ...
  * [WinCE Programming Tips](<../en/WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Использование телефонного API, отправка SMS-сообщений и многое другое ...
  * [Android Programming](<../en/Android_Programming.md> "Android Programming") \- Для Android смартфонов и планшетов
  * [iPhone/iPod development](<../en/iPhone/iPod_development.md> "iPhone/iPod development") \- Об использовании Objective Pascal для разработки приложений для iOS



### Статьи по разработке интерфейсов

  * [Carbon interface internals](<../en/Carbon_interface_internals.md> "Carbon interface internals") \- Если вы хотите помочь улучшить интерфейс Carbon
  * [Windows CE Development Notes](<../en/Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- Для карманных ПК и смартфонов
  * [Adding a new interface](<../en/Adding_a_new_interface.md> "Adding a new interface") \- Как добавить новый интерфейс набора виджетов
  * [LCL Defines](<../en/LCL_Defines.md> "LCL Defines") \- Выбор правильных опций для перекомпиляции LCL
  * [LCL Internals](<../en/LCL_Internals.md> "LCL Internals") \- Некоторая информация о внутренней работе LCL
  * [Cocoa Internals](<../en/Cocoa_Internals.md> "Cocoa Internals") \- Некоторая информация о внутренней работе виджета Cocoa



## Минимальные версии Toolkit

Lazarus version | Мин. FPC | Мин. Gtk 2 | Мин. Qt 4 | Мин. Windows | Мин. Windows CE | Мин. macOS (Carbon) | Мин. macOS (Cocoa) | Мин. треб. LCL-CustomDrawn   
---|---|---|---|---|---|---|---|---  
0.9.24  | 2.2.0  | 2.6+  | 4.2+  | Windows 98+  | Рекомендуется 4.0+  | 10.4  | N/A  | N/A   
0.9.26  | 2.2.2  | 2.6+  | 4.3+  | Windows 98+  | Рекомендуется 4.0+  | 10.4  | N/A  | N/A   
0.9.28  | 2.2.4  | 2.8+  | 4.5+  | Windows 98+  | Рекомендуется 4.0+  | 10.4  | N/A  | N/A   
0.9.30  | 2.4.0  | 2.8+  | 4.5+  | Windows 98+  | Рекомендуется 5.0+  | 10.4  | N/A  | N/A   
0.9.31  | 2.4.4  | 2.8+  | 4.5+  | Windows 98+  | Рекомендуется 5.0+  | 10.4  | 10.6  | Android 2.2+, Windows 2000+, X11, Mac 10.6+   
1.2.6  | 2.6.4  | 2.8+  | 4.5+*  | Windows 98+  | Рекомендуется 5.0+  | 10.4  | 10.6  | Android 2.2+, Windows 2000+, X11, Mac 10.6+   
  
  * \- На самом деле 4.5.0 ... 4.8.4 с предварительно собранным Qt4Pas.dll, 4.8.5+ требует другого Qt4Pas.dll



См.также: [совместимость](</index.php?title=Installing_Lazarus_on_MacOS_X/ru&action=edit&redlink=1> "Installing Lazarus on MacOS X/ru \(page does not exist\)")

## Внутренности LCL

Есть LCL и есть «интерфейс». LCL - это часть, которая не зависит от платформы и находится в каталоге lazarus/lcl/. Этот каталог содержит в основном определения классов. Многие различные элементы управления фактически реализованы в каталоге lazarus/lcl/include/ в различных файлах .inc. Это делается для того, чтобы быстрее найти реализацию определенного элемента управления, например TCustomMemo (который находится в custommemo.inc). Каждый .inc начинается со строки {% MainUnit ...}, чтобы определить, где он включен. 

Также есть «интерфейс», который находится в подкаталоге каталога lazarus/lcl/interfaces/. Интерфейс gtk находится в gtk/, win32 в win32/ и т.д. Все они имеют модуль Interfaces, который используется lcl и создает основной объект интерфейса. Обычно основной интерфейсный объект определяется в XXint.pp (win32int.pp) и реализуется в различных inc-файлах, XXobject.inc - для методов, специфичных для интерфейса, XXwinapi.inc - для методов реализации winapi, XXlistsl.inc - для реализации списка строк, использующихся TComboBox, TListBox и другими подобными элементами управления, XXcallback.inc - для обработки событий виджета и принятия соответствующих действий для уведомления LCL. 

Каждый элемент управления имеет свойство WidgetSetClass, принадлежащее "зеркальному" классу в каталоге интерфейсов, например: "зеркало" для TCustomEdit - это TWSCustomEdit, методы которого реализуются TWin32WSCustomEdit в win32wsstdctrls. Это способ, которым LCL взаимодействует с интерфейсом, и то, как он позволяет интерфейсу делать свое дело. 

Передача интерфейса обратно в LCL в основном осуществляется путем отправки сообщений, обычно 'DeliverMessage', который вызывает TControl.Perform (<message_id>, wparam, lparam), где wparam и lparam являются дополнительной информацией для сообщения. 

### Pie и RadialPie

Модуль LCLIntf содержит две функции для рисования округлых фигур: 
    
    
    function [Pie](<http://lazarus-ccr.sourceforge.net/docs/lcl/lclintf/pie.html> "doc:lcl/lclintf/pie.html")(DC: HDC; x1, y1, x2, y2, sx, sy, ex, ey: Integer): Boolean;
    function [RadialPie](<http://lazarus-ccr.sourceforge.net/docs/lcl/lclintf/radialpie.html> "doc:lcl/lclintf/radialpie.html")(DC: HDC; x1, y1, x2, y2, Angle1, Angle2: Integer): Boolean; 
    

Функция Pie использует две точки (sx,sy) и (ex,ey) для указания начала и конца дуги. RadialPie использует углы, чтобы указать начало и конец дуги. 

Pie вызывает TWidgetSet.Pie, а RadialPie вызывает TWidgetSet.RadialPie. Реализация TWidgetSet.Pie по умолчанию заключается в преобразовании параметров в углы и вызове TWidgetSet.RadialPie. TWidgetSet.RadialPie создает массив точек для дуги и вызывает TWidgetSet.Polygon. 

Набор виджетов win32 переопределяет TWidgetSet.Pie для непосредственного вызова функции Windows Pie. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** в более старых версиях Lazarus существовал RadialPie с углами, которые делали то же, что и нынешние RadialPie и RadialPie, которые делают то же самое, что и наша [функция] Pie. Эти функции были удалены в Lazarus 0.9.21.

### Интерфейсы

См. [другие интерфейсы](<LCL_Internals.md> "LCL Internals/ru")

### Добавление нового модуля в LCL

Сначала добавьте новое имя модуля в allclunits.pp. 

Чтобы убедиться, что модуль зарегистрирован в палитре компонентов, смотрите файлы RegisterLCL.pas и pkgfileslcl.inc, они расположены в lazarus/packager. 

## Как создать новый Widgetset

Это пошаговое руководство по разработке нового [widgetset](<../en/Widgetset.md> "Widgetset"). Он основан на моем опыте создания основ нового интерфейса qt4. 

Для начала, почему кто-то хочет добавить Widgetset? Ответ заключается в том, чтобы иметь возможность портировать существующее программное обеспечение Lazarus на большее количество платформ без изменения их кода. 

Итак, давайте напишем набор виджетов. Прежде всего, вам нужно иметь паскаль привязки для виджета и знать, как его использовать. Обычно это не сложно. Несколько часов работы с основными учебными пособиями, доступными в Интернете, должны быть достаточными для начала. Если привязки еще не существуют, их нужно создать. Если учебные пособия написаны на другом языке, переведите их на паскаль и заставьте их работать. 

Теперь для Qt я использовал привязки Den Jean qt4 для паскаля и создал очень простую программу Qt, используя их: 
    
    
    program qttest;
    
    uses qt4;
    
    var
      App: QApplicationH;
      MainWindow: QMainWindowH;
    begin
      App := QApplication_Create(@argc,argv);
    
      MainWindow := QMainWindow_Create;
    
      QWidget_show(MainWindow);
    
      QApplication_Exec;
    end.
    

Вышеуказанный проект компилируется и создает программу qt4. Теперь мы будем использовать его код для написания нового набора виджетов. После того, как мы закончим, приведенная ниже программа lazarus прекрасно скомпилируется в программу qt4: 
    
    
    program qttest;
    
    {$mode objfpc}{$H+}
    
    uses
      Interfaces, Classes, Forms,
      { Add your units here }
      qtform;
    
    begin
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    end.
    

где форма поддерживается Lazarus IDE и разрабатывается визуально. 

Первое, что нужно сделать с новым набором виджетов, это добавить пустой скелет для него. Очень ранние наборы виджетов разработки, такие как qt и carbon, могут служить скелетом. 

Просматривая файлы на многих виджетах, вы можете увидеть первый файл, который будет вызван lcl: Interfaces.pas. Этот файл просто вызывает другой файл с именем QtInt.pas или аналогичный. QtInt.pas содержит код для класса TWidgetSet, который мы должны реализовать. На пустом скелете вы можете видеть, что у класса есть различные функции, которые он должен реализовать: 
    
    
      TQtWidgetSet = Class(TWidgetSet)
      private
        App: QApplicationH;
      public
        {$I qtwinapih.inc}
        {$I qtlclintfh.inc}
      public
        // Application
        procedure AppInit(var ScreenInfo: TScreenInfo); override;
        procedure AppRun(const ALoop: TApplicationMainLoop); override;
        procedure AppWaitMessage; override;
        procedure AppProcessMessages; override;
        procedure AppTerminate; override;
        procedure AppMinimize; override;
        procedure AppBringToFront; override;
      public
        constructor Create;
        destructor Destroy; override;
        function  DCGetPixel(CanvasHandle: HDC; X, Y: integer): TGraphicsColor; override;
        procedure DCSetPixel(CanvasHandle: HDC; X, Y: integer; AColor: TGraphicsColor); override;
        procedure DCRedraw(CanvasHandle: HDC); override;
        procedure SetDesigning(AComponent: TComponent); override;
    
        function  InitHintFont(HintFont: TObject): Boolean; override;
    
        // create and destroy
        function CreateComponent(Sender : TObject): THandle; override; // deprecated
        function CreateTimer(Interval: integer; TimerFunc: TFNTimerProc): integer; override;
        function DestroyTimer(TimerHandle: integer): boolean; override;
      end;
    

### Как реализовать новый оконный компонент

Все оконные компоненты являются потомками TWinControl. Эти элементы управления имеют дескриптор и поэтому должны создаваться набором виджетов. Добавлять новые оконные компоненты в набор виджетов [довольно] легко. 

Допустим, вы хотите добавить TQtWSCustomEdit в Qt Widgetset. [Нужно] начать с TCustomEdit, [который] является потомком TWinControl и находится в модуле StdCtrls. 

Теперь перейдите к модулю QtWSStrCtrls и найдите объявление TQtWSCustomEdit. 
    
    
      TQtWSCustomEdit = class(TWSCustomEdit)
      private
      protected
      public
      end;
    

Добавьте статические методы, объявленные в TWSCustomEdit, и переопределите их. Код должен теперь выглядеть так: 
    
    
      TQtWSCustomEdit = class(TWSCustomEdit)
      private
      protected
      public
        class function CreateHandle(const AWinControl: TWinControl;
              const AParams: TCreateParams): HWND; override;
        class procedure DestroyHandle(const AWinControl: TWinControl); override;
    {    class function  GetSelStart(const ACustomEdit: TCustomEdit): integer; override;
        class function  GetSelLength(const ACustomEdit: TCustomEdit): integer; override;
    
        class procedure SetCharCase(const ACustomEdit: TCustomEdit; NewCase: TEditCharCase); override;
        class procedure SetEchoMode(const ACustomEdit: TCustomEdit; NewMode: TEchoMode); override;
        class procedure SetMaxLength(const ACustomEdit: TCustomEdit; NewLength: integer); override;
        class procedure SetPasswordChar(const ACustomEdit: TCustomEdit; NewChar: char); override;
        class procedure SetReadOnly(const ACustomEdit: TCustomEdit; NewReadOnly: boolean); override;
        class procedure SetSelStart(const ACustomEdit: TCustomEdit; NewStart: integer); override;
        class procedure SetSelLength(const ACustomEdit: TCustomEdit; NewLength: integer); override;
    
        class procedure GetPreferredSize(const AWinControl: TWinControl;
                            var PreferredWidth, PreferredHeight: integer); override;}
      end;
    

Закомментированная часть кода - это процедуры, которые необходимо реализовать, чтобы TCustomEdit был полностью функциональным, но достаточно просто [наличия] CreateHandle и DestroyHandle, чтобы он отображался в форме и был редактируемым, поэтому он соответствует нашим потребностям в этой статье. 

Нажмите Ctrl+⇧ Shift+C, чтобы завершить код и реализовать CreateHandle и DestroyHandle. В случае Qt4 код будет таким: 
    
    
    { TQtWSCustomEdit }
    
    class function TQtWSCustomEdit.CreateHandle(const AWinControl: TWinControl;
      const AParams: TCreateParams): HWND;
    var
      Widget: QWidgetH;
      Str: WideString;
    begin
      // Создаем виджет
      WriteLn('Calling QTextDocument_create');
      Str := WideString((AWinControl as TCustomMemo).Lines.Text);
      Widget := QTextEdit_create(@Str, QWidgetH(AWinControl.Parent.Handle));
    
      //Задаем его начальные свойства
      QWidget_setGeometry(Widget, AWinControl.Left, AWinControl.Top,
       AWinControl.Width, AWinControl.Height);
    
      QWidget_show(Widget);
    
      Result := THandle(Widget);
    end;
    
    class procedure TQtWSCustomEdit.DestroyHandle(const AWinControl: TWinControl);
    begin
      QTextEdit_destroy(QTextEditH(AWinControl.Handle));
    end;
    

Теперь раскомментируйте строку "RegisterWSComponent (TCustomEdit, TQtWSCustomEdit);" в конце модуля и ... все! 

Теперь вы можете поместить TCustomEdit внизу формы и ожидать, что он будет работать. :^) 

### Реализация TBitmap

Для реализации TBitmap необходимо понимать TRawImage и TLazIntfImage, как описано здесь: [Developing with Graphics#Working with TLazIntfImage.2C TRawImage and TLazCanvas](<../en/Developing_with_Graphics.md> "Developing with Graphics")

Итак, допустим, вы хотите скомпилировать следующий код: 
    
    
    procedure TMyForm.HandleOnPaint(Sender: TObject);
    var
      Bitmap: TBitmap;
    begin
      Bitmap := TBitmap.Create;
      try
        Bitmap.LoadFromFile('myfile.bmp');
        Canvas.Draw(0, 0, Bitmap);
      finally
        Bitmap.Free;
      end;
    end;
    

Ниже приведен порядок вызова функций из интерфейса widgetset при выполнении этого кода: 
    
    
     1 - BeginPaint
    

Это будет вызвано только, если событие OnPaint отправляет ноль в качестве DC для события рисования 

  

    
    
     2 - GetDC(0);
    

Просто создает контекст устройства. 

  

    
    
     3 - TCDWidgetSet.RawImage_QueryDescription 
    

Реализация этой процедуры по умолчанию подходит для большинства наборов виджетов. 

  

    
    
     4 - TCDWidgetSet.RawImage_CreateBitmaps
    

Здесь вам нужно создать собственный объект изображения и загрузить его из RawData.Data, где хранится информация на основе вашего описания формата пикселя в пункте 2. 

  

    
    
     5 - CreateCompatibleDC(0)
    

Это создает временный DC только для хранения изображения, но на данный момент нет информации об изображении, поэтому в этот момент этот DC действительно фиктивный 

  

    
    
     6 - SelectObject
    

С изображением в качестве объекта, который будет выбран, и DC, созданный выше, в качестве целевого DC. 

  

    
    
     7 - StretchMaskBlt
    

Наконец, функция рисования! DestDC - это DC, размещенный на BeginPaint. 

  

    
    
     8 - EndPaint
    

Опять же, не всегда используется. 

### TBitmap.LoadFromDevice для снятия скриншота

Рекомендуется сначала реализовать Bitmap, прежде чем делать этот шаг. 

В LCL вы можете использовать следующий код, делающий скриншот со всего экрана и рисующий его на холсте: 
    
    
    var
      ScreenDC: HDC;
      Bitmap: TBitmap;
    begin
      Bitmap := TBitmap.Create;
      try
        ScreenDC := GetDC(0);
        Bitmap.LoadFromDevice(ScreenDC);
        ReleaseDC(0, ScreenDC);
        Canvas.Draw(0, 0, Bitmap);
      finally
        Bitmap.Free;
      end;
    end;
    

Если вы уже внедрили TBitmap, для LoadFromDevice нужно реализовать только 2 новые функции: GetDeviceSize и GetRawImageFromDevice 

Ниже приведен большой трейс, охватывающий все функции набора виджетов, вызываемые для события OnPaint, которое делает снимок экрана и рисует его на экране. Эта трассировка была сделана с помощью набора виджетов Qt и может иметь некоторые недостатки. Номера дескрипторов должны использоваться для проверки того, какой объект используется в функциях. 
    
    
    [WinAPI BeginPaint] Result=-1220713544
    [WinAPI GetClientBounds]
    [WinAPI SetWindowOrgEx] DC: -1220713544 NewX: 0 NewY: 0
    
    
    //Входим в событие OnPaint
    Bitmap := TBitmap.Create;
    try
      ScreenDC := GetDC(0);
    
    
    
    [WinAPI GetDC] hWnd: 0 Result: -1220712920
    
    
    Bitmap.LoadFromDevice(ScreenDC);
    
    
    
     [WinAPI GetDeviceSize]
     [WinAPI GetRawImageFromDevice] SrcDC: -1220712920 SrcWidth: 0 SrcHeight: 0
     [WinAPI CreateBitmapFromRawImage] Width:1024 Height:768 DataSize: 3145728 CreateMask: False Bitmap:-1220746696
     [WinAPI GetObject] GDIObj: -1220746696 Result=84 ObjectType=Image
    
    
    
      ReleaseDC(0, ScreenDC);
    
    
    
    [WinAPI ReleaseDC] hWnd: 0 DC: -1220712920
    
    
    Canvas.Draw(0, 0, Bitmap);
    
    
    
     [WinAPI CreateCompatibleDC] DC: 0
     [WinAPI GetDC] hWnd: 0 Result: -1220712920
     [WinAPI SelectObject] DC=-1220712920 GDIObj=-1220746696 Result=0 ObjectType=Image
     [WinAPI StretchMaskBlt] DestDC:-1220713544 SrcDC:-1220712920 Image:137185120 X:0 Y:0 W:1024 H:768 XSrc:0 YSrc:0 WSrc:1024 HSrc:768
    
    
    
    finally
      Bitmap.Free;
    end;
    
    
    
     [WinAPI SelectObject] DC=-1220712920 GDIObj=0 Invalid GDI Object
     [WinAPI DeleteObject] GDIObject: -1220746696 Result=False ObjectType=Image
    
    
    
    //Здесь мы вышли из события OnPaint
    
    
    
     [WinAPI DeleteObject] GDIObject: 0
     [WinAPI DeleteObject] GDIObject: 0
     [WinAPI DeleteObject] GDIObject: 0
     [WinAPI DeleteObject] GDIObject: 0
     [WinAPI SetWindowOrgEx] DC: -1220713544 NewX: -152 NewY: -246
     [WinAPI DeleteObject] GDIObject: 0
     [WinAPI DeleteObject] GDIObject: 0
     [WinAPI DeleteObject] GDIObject: 0
     [WinAPI DeleteObject] GDIObject: 0
     TWidgetSet.InitializeCriticalSection
     TWidgetSet.EnterCriticalSection
     [WinAPI SelectObject] DC=-1220713544 GDIObj=0 Invalid GDI Object
     [WinAPI MoveToEx] DC:-1220713544 X:0 Y:0
     [WinAPI SelectObject] DC=-1220713544 GDIObj=-1220746760 Result=-1220746856 ObjectType=Brush
     [WinAPI SelectObject] DC=-1220713544 GDIObj=-1220746856 Result=-1220746856 ObjectType=Brush
     TWidgetSet.LeaveCriticalSection
     [WinAPI SetWindowOrgEx] DC: -1220713544 NewX: 0 NewY: 0
     [WinAPI EndPaint] Handle: -1220611768 PS.HDC: -1220713544
    

### Реализация рисования в событии OnPaint формы или другого элемента управления

Для рисования в событии OnPaint формы само событие приходит из библиотеки лежащих в основе виджетов. Интерфейс LCL должен обработать это событие и создать соответствующий объект DC в обработчике событий, а затем вызвать LCLSendPaintMsg. Кроме того, следует также реализовать соответствующие методы рисования, такие как Rectangle или ExtTextOut. Также могут быть полезны процедуры, связанные с Pen(перо), Brush(кисть) и Font(шрифт). 

Вот обработчик события OnPaint из набора виджетов Cocoa, который показывает, как он вызывает LCLSentPaintMsg: 
    
    
    procedure TLCLCommonCallback.Draw(ControlContext: NSGraphicsContext;
      const bounds, dirty:NSRect);
    var
      struct : TPaintStruct;
    begin
      if not Assigned(Context) then Context:=TCocoaContext.Create;
    
      Context.ctx:=ControlContext;
      if Context.InitDraw(Round(bounds.size.width), Round(bounds.size.height)) then
      begin
        FillChar(struct, SizeOf(TPaintStruct), 0);
        struct.hdc := HDC(Context);
        {$IFDEF VerboseWinAPI}
          DebugLn(Format('[TLCLCommonCallback.Draw] OnPaint event started context: %x', [HDC(context)]));
        {$ENDIF}
        LCLSendPaintMsg(Target, HDC(Context), @struct);
        {$IFDEF VerboseWinAPI}
          DebugLn('[TLCLCommonCallback.Draw] OnPaint event ended');
        {$ENDIF}
      end;
    end;
    

И список подпрограмм WinAPI, которые были вызваны при запуске этого события: 
    
    
     [TCocoaWidgetSet.GetDC] hWnd: 0 Result: 12400C0
     [TLCLCommonCallback.Draw] OnPaint event started context: 1240340
     TCocoaWidgetSet.CreatePenIndirect
     TCocoaWidgetSet.SelectObject DC: 1240340 GDIObj: 10EC460
     TCocoaWidgetSet.SelectObject Result: 0
     TCocoaWidgetSet.SelectObject DC: 1240340 GDIObj: 10EBF00
     TCocoaWidgetSet.SelectObject Result: 0
     [TCocoaWidgetSet.Rectangle] DC: 1240340 X1: 100 Y1: 100 X2: 200 Y2: 200
     TCocoaWidgetSet.SelectObject DC: 1240340 GDIObj: 10EC4A0
     TCocoaWidgetSet.SelectObject Result: 0
     [TCocoaWidgetSet.GetTextExtentPoint] DC: 1240340 Str: Some text Count: 9
     [TCocoaWidgetSet.GetTextExtentPoint] Size: 65,17
     [TLCLCommonCallback.Draw] OnPaint event ended
    

### Внедрение TLabel

Внедрить TLabel особенно сложно, несмотря на то, что он является этаким базовым компонентом, потому что это требует, чтобы была реализована почти вся отрисовка. TLabel не является оконным элементом управления, вместо этого он зависит от сообщений отрисовки, рисующих [компонент] непосредственно на холсте формы. 

Прежде чем пытаться заставить работать TLabel, рекомендуется проверить, работают ли функции рисования, такие как Rectangle, внутри события OnPaint формы. 

Необходимо реализовать несколько методов WinAPI, в частности: 

**Методы контекста устройства**

BeginPaint, GetDC, EndPaint, ReleaseDC, CreateCompatibleDC 

см. [Контексты устройств и объекты GDI в интерфейсах LCL](<../en/Device_Contexts_and_GDI_objects_in_the_LCL_interfaces.md> "Device Contexts and GDI objects in the LCL interfaces")

**Методы объектов GDI**

SelectObject, DeleteObject, CreateFontIndirect, CreateFontIndirectEx 

**Разные функции**

InvalidateRect, GetClientBounds, SetWindowOrgEx 

**Методы рисования текста**

DrawText.  
Вместо реализации DrawText можно также использовать стандартный TWidgetSet.DrawText, как это делают наборы виджетов Carbon и Cocoa. Но в этом случае необходимо, чтобы каждый реализовывал хотя бы GetTextMetrics, GetTextExtentPoint и ExtTextOut. Без GetTextMetrics форма с TLabel рухнет, потому что авторазмер не сможет рассчитать подходящий размер для TLabel. 

**Функции Region для определения, что элемент управления находится за другим**

CombineRgn, CreateRectRgn, GetClipRGN, RectVisible 

  
Ниже приведен порядок вызова процедур рисования в форме с одним TLabel, чтобы лучше понять последовательность рисования: 
    
    
    1 - GetDC вызывается один раз при запуске программного обеспечения с hWnd = 0
    2 - Показывается форма
    3 - GetDC вызывается снова (это не произошло бы без TLabel). Вызывается несколько функций, связанных со шрифтами, а также DrawText с CalcRect, установленным в True, чтобы вычислить размер TLabel.
    4 - InvalidateRect вызывается на холсте формы
    5 - Управление возвращается к операционной системе до тех пор, пока из набора виджетов не придет сообщение отрисовки
    6 - Вызывается BeginPaint, и в этот момент будет выполнен код в событии OnPaint формы
    7 - DrawText вызывается снова с CalcRect установленным в false
    8 - Отрисовка закончена

### Реализация видимости для форм и элементов управления и состояния окна

Код, который управляет видимостью, разделен между видимостью для форм и для элементов управления. 

#### Видимость для форм и состояния окна

Эта часть также управляет состоянием окна (свернуто, развернуто или нормально). Это реализовано как копия функции Windows API ShowWindow, поэтому вы должны реализовать TMyWidgetset.ShowWindow в файле mywinapi.inc. Не забудьте также добавить заголовок в файл mywinapih.inc. 

Ниже приведен код, который реализует эту функцию в наборе виджетов Qt. Это должно [помочь] очень легко понять, скопировать и реализовать его на вашем собственном виджете. Вы также можете посмотреть, как это реализует Gtk. В Windows, конечно, API Windows вызывается напрямую, поэтому нет кода, на котором его можно [продемонстрировать]. 
    
    
    {------------------------------------------------------------------------------
      function ShowWindow(hWnd: HWND; nCmdShow: Integer): Boolean;
    
      nCmdShow:
        SW_SHOWNORMAL, SW_MINIMIZE, SW_SHOWMAXIMIZED
    ------------------------------------------------------------------------------}
    function TQtWidgetSet.ShowWindow(hWnd: HWND; nCmdShow: Integer): Boolean;
    var
      Widget: QWidgetH;
    begin
      {$ifdef VerboseQtWinAPI}
        WriteLn('WinAPI ShowWindow');
      {$endif}
    
      Result := False;
      
      Widget := QWidgetH(hWnd);
    
    //  if Widget = nil then RaiseException('TQtWidgetSet.ShowWindow  hWnd is nil');
    
      case nCmdShow of
        SW_SHOW: QWidget_setVisible(Widget, True);
        SW_SHOWNORMAL: QWidget_showNormal(Widget);
        SW_MINIMIZE: QWidget_setWindowState(Widget, QtWindowMinimized);
        SW_SHOWMINIMIZED: QWidget_showMinimized(Widget);
        SW_SHOWMAXIMIZED: QWidget_showMaximized(Widget);
        SW_HIDE: QWidget_setVisible(Widget, False);
      end;
    
      Result := True;
    end;
    

#### Видимость для элементов управления

Для элементов управления внутри формы вам необходимо реализовать функцию класса TMyWSWinControl.ShowHide, которая находится в классе TMyWSWinControl в файле mywscontrols.pp 

Помните, что большинство элементов управления наследуются от TWinControl, поэтому реализация этой функции гарантирует, что свойство Visible реализовано для всех стандартных элементов управления, которые его имеют. Ниже приведен пример кода для набора виджетов Qt. 
    
    
    {------------------------------------------------------------------------------
      Method: TQtWSWinControl.ShowHide
      Params:  AWinControl     - the calling object
    
      Returns: Nothing
    
      Shows or hides a widget.
     ------------------------------------------------------------------------------}
    class procedure TQtWSWinControl.ShowHide(const AWinControl: TWinControl);
    begin
      if AWinControl = nil then exit;
    
      if not AWinControl.HandleAllocated then exit;
    
      if AWinControl.HandleObjectShouldBeVisible then
       QWidget_setVisible(TQtWidget(AWinControl.Handle).Widget, True)
      else QWidget_setVisible(TQtWidget(AWinControl.Handle).Widget, False);
    end;
    

### Реализация компонентов на основе TStrings

Некоторые компоненты используют TStrings для хранения отображаемой информации, например: TCustomMemo, TCustomListBox и TCustomComboBox. 

Для их реализации недостаточно просто реализовать их функции, например, в классе TQtCustomMemo. Одна из реализуемых функций будет называться GetStrings и выглядеть следующим образом: 
    
    
    class function TQtWSCustomListBox.GetStrings(const ACustomListBox: TCustomListBox): TStrings;
    var
      ListWidgetH: QListWidgetH;
    begin
      ListWidgetH := QListWidgetH((TQtWidget(ACustomListBox.Handle).Widget));
      Result := TQtListStrings.Create(ListWidgetH, ACustomListBox);
    end;
    

Эта функция должна возвращать потомок TStrings, предназначенный для отслеживания, что строки добавляются или удаляются из списка, и отправлять эту информацию в набор виджетов для обновления элемента управления. Например, следующее объявление показывает, как выглядит TQtListStrings: 
    
    
      TQtListStrings = class(TStrings)
      private
        FListChanged: Boolean; // StringList и QtListWidget не синхронизированы
        FStringList: TStringList; // Содержит пункты для показа
        FQtListWidget: QListWidgetH;  // Qt Widget
        FOwner: TWinControl;      // Lazarus'овский владелец элементов управления ListStrings
        FUpdating: Boolean;       // Мы меняем Qt Widget
        procedure InternalUpdate;
        procedure ExternalUpdate(var Astr: TStringList; Clear: Boolean = True);
        procedure IsChanged; // событие OnChange, вызваемое действием программы
      protected
        function GetTextStr: string; override;
        function GetCount: integer; override;
        function Get(Index : Integer) : string; override;
        //procedure SetSorted(Val : boolean); virtual;
      public
        constructor Create(ListWidgetH : QListWidgetH; TheOwner: TWinControl);
        destructor Destroy; override;
        procedure Assign(Source : TPersistent); override;
        procedure Clear; override;
        procedure Delete(Index : integer); override;
        procedure Insert(Index : integer; const S: string); override;
        procedure SetText(TheText: PChar); override;
        //procedure Sort; virtual;
      public
        //property Sorted: boolean read FSorted write SetSorted;
        property Owner: TWinControl read FOwner;
        function ListChangedHandler(Sender: QObjectH; Event: QEventH): Boolean; cdecl;
      end;
    

Вы можете увидеть его реализацию в модуле qtobjects.pas интерфейса qt. 

### Реализация меню

Меню доступны на LCL для создания основных меню или всплывающих меню. TMenu является владельцем более крупной структуры меню со многими пунктами. Элементы могут иметь подэлементы, и не нуждаются в дополнительном TMenus. 

Также помните, что в LCL дескриптор создается только при необходимости, и в это время все свойства элементов управления уже инициализированы. Это очень помогает для наборов виджетов, где в зависимости от свойств элемента меню он может принадлежать к одному или другому классу, например, Qt. 

Следующие вещи должны быть реализованы для того, чтобы меню работали: 

1) Все методы модуля QtWSMenus, которые будут реализовывать создание и изменение меню 

2) функция `TWinCEWidgetSet.SetMenu(AWindowHandle: HWND; AMenuHandle: HMENU): Boolean;` из файла wincewinapi.inc, в котором будет реализована поддержка главного меню, связанного с окном. 

#### Порядок создания меню

При реализации меню важно понимать, в каком порядке они создаются. Например, мы хотим создать следующую структуру меню: 

[![Menu creation order.png](https://wiki.freepascal.org/images/6/69/Menu_creation_order.png)](</File:Menu_creation_order.png>)

И когда наше приложение будет выполнено, при каждом вызове TQtWSMenuItem.CreateHandle будет появляться сообщение 'Creating MenuItem'(Создание MenuItem) с заголовком меню и сообщение 'Creating Menu'(Создание меню) с названием меню (потомки TMenu не имеют подпись), каждый раз, когда вызывается TQtWSMenu.CreateHandle. 

Вот результирующий вывод такого программного обеспечения: 
    
    
     Creating Menu. Name: MainMenu1
     Creating MenuItem: Item1           Parent=Menu.Items : TMenuItem
     Creating MenuItem: SubItem11       Parent=Item1      : TMenuItem
     Creating MenuItem: SubItem12       Parent=Item1      : TMenuItem
     Creating MenuItem: SubItem13       Parent=Item1      : TMenuItem
     Creating MenuItem: SubItem14       Parent=Item1      : TMenuItem
     Creating MenuItem: SubSubItem141   Parent=SubItem14  : TMenuItem
     Creating MenuItem: SubSubItem142   Parent=SubItem14  : TMenuItem
     Creating MenuItem: SubSubItem143   Parent=SubItem14  : TMenuItem
     Creating MenuItem: SubSubItem144   Parent=SubItem14  : TMenuItem
     Creating MenuItem: Item2           Parent=Menu.Items : TMenuItem
     Creating MenuItem: SubItem21       Parent=Item2      : TMenuItem
     Creating MenuItem: SubItem22       Parent=Item2      : TMenuItem
     Creating MenuItem: SubItem23       Parent=Item2      : TMenuItem
     Creating MenuItem: Item3           Parent=Menu.Items : TMenuItem
     Creating MenuItem: Item4           Parent=Menu.Items : TMenuItem
    

Для всех MenuItems можно использовать GetParentMenu, чтобы получить владельца их родителя: Menu (TMainMenu). 

### Включение/отключение элемента управления

Текущий способ установить включение/отключение элемента управления - реализация winapi EnableWindow. Этот API должен работать в общем на любом элементе управления. Он должен включать/отключать ввод с помощью мыши и клавиатуры для указанного окна или элемента управления, а также помечать его как недоступный для редактирования пользователем, например, делая его серым. 
    
    
    {------------------------------------------------------------------------------
      Method:  EnableWindow
      Params: HWnd    - handle to window / дескриптор окна
              BEnable -  whether to enable the window / нужно ли включать окно
      Returns: If the window was previously disabled / возращаемый результат: если окно было ранее отключено
    
      Enables or disables mouse and keyboard input to the specified window or
      control / Включает или отключает ввод с клавиатуры и мыши в указанное окно или
      элемент управления
     ------------------------------------------------------------------------------}
    function TWin32WidgetSet.EnableWindow(hWnd: HWND; bEnable: Boolean): Boolean;
    

### Фигурные окна

Окна могут быть сформированы на основе растрового изображения (TBitmap) или региона(TRegion). Регион является видимой частью 

Чтобы реализовать фигурные окна на основе TBitmap, реализуйте TWSWinControl.setShape 

Для реализации фигурных окон на основе TRegion реализуйте LCLIntf.SetWindowRgn 

См.также: [Создание не прямоугольных окон или элементов управления](<LCL_Tips.md> "LCL Tips/ru")

### Системные цвета

Некоторые константы цвета на самом деле являются системными цветами, такими как clBtnFace, clForm, clWindow и проч. и проч. 

Для реализации поддержки системных цветов должна быть реализована подпрограмма WinAPI GetSysColor: 
    
    
    function GetSysColor(nIndex: Integer): DWORD; override;
    

А вот фрагмент цветовых констант, которые должны поддерживаться. Проверьте LCLType для последних значений: 
    
    
    //==============================================
    // API system Color constants  pbd
    // note these are usually shown ORed with
    // $80000000 as these would have interfered with
    // other MS color enumerations
    // GetSysColor and SetSysColor expects the values
    // below
    //==============================================
    
    type
      COLORREF = LongInt;
      TColorRef = COLORREF;
    
    const
      CLR_INVALID = TColorRef($FFFFFFFF);
    
      COLOR_SCROLLBAR = 0;
      COLOR_BACKGROUND = 1;
      COLOR_ACTIVECAPTION = 2;
      COLOR_INACTIVECAPTION = 3;
      COLOR_MENU = 4;
      COLOR_WINDOW = 5;
      COLOR_WINDOWFRAME = 6;
      COLOR_MENUTEXT = 7;
      COLOR_WINDOWTEXT = 8;
      COLOR_CAPTIONTEXT = 9;
      COLOR_ACTIVEBORDER = 10;
      COLOR_INACTIVEBORDER = 11;
      COLOR_APPWORKSPACE = 12;
      COLOR_HIGHLIGHT = 13;
      COLOR_HIGHLIGHTTEXT = 14;
      COLOR_BTNFACE = 15;
      COLOR_BTNSHADOW = 16;
      COLOR_GRAYTEXT = 17;
      COLOR_BTNTEXT = 18;
      COLOR_INACTIVECAPTIONTEXT = 19;
      COLOR_BTNHIGHLIGHT = 20;
      COLOR_3DDKSHADOW = 21;
      COLOR_3DLIGHT = 22;
      COLOR_INFOTEXT = 23;
      COLOR_INFOBK = 24;
      // PBD: 25 is unassigned in all the docs I can find
      //      if someone finds what this is supposed to be then fill it in
      //      note defaults below, and cl[ColorConst] in graphics
      COLOR_HOTLIGHT = 26;
      COLOR_GRADIENTACTIVECAPTION = 27;
      COLOR_GRADIENTINACTIVECAPTION = 28;
      COLOR_MENUHILIGHT = 29;
      COLOR_MENUBAR = 30;
    
      COLOR_FORM = 31;
    
      COLOR_ENDCOLORS = COLOR_FORM;
    
      COLOR_DESKTOP = COLOR_BACKGROUND;
      COLOR_3DFACE = COLOR_BTNFACE;
      COLOR_3DSHADOW = COLOR_BTNSHADOW;
      COLOR_3DHIGHLIGHT = COLOR_BTNHIGHLIGHT;
      COLOR_3DHILIGHT = COLOR_BTNHIGHLIGHT;
      COLOR_BTNHILIGHT = COLOR_BTNHIGHLIGHT;
    
      MAX_SYS_COLORS = COLOR_ENDCOLORS;
      SYS_COLOR_BASE = TColorRef($80000000);
    

### ShowMessage

Эти стандартные диалоги реализованы исключительно в LCL в следующих местах: 

  * Файл класса TPromptDialog lcl/include/promptdialog.inc



### SpinEdit

И TFloatSpinEdit, и TSpinEdit реализованы в классе TWSFloatSpinEdit. 

### Буфер обмена

Поддержка буфера обмена реализована в lclintf путем реализации подпрограмм Windows API. Это процедуры: 
    
    
    function ClipboardFormatToMimeType(FormatID: TClipboardFormat): string; {$IFDEF IF_BASE_MEMBER}virtual;{$ENDIF}
    function ClipboardGetData(ClipboardType: TClipboardType;
      FormatID: TClipboardFormat; Stream: TStream): boolean; {$IFDEF IF_BASE_MEMBER}virtual;{$ENDIF}
    // ! ClipboardGetFormats: List будет создан. Вы должны освободить его самостоятельно с помощью FreeMem(List) !
    function ClipboardGetFormats(ClipboardType: TClipboardType;
      var Count: integer; var List: PClipboardFormat): boolean;  {$IFDEF IF_BASE_MEMBER}virtual;{$ENDIF}
    function ClipboardGetOwnerShip(ClipboardType: TClipboardType;
      OnRequestProc: TClipboardRequestEvent;  FormatCount: integer;
      Formats: PClipboardFormat): boolean; {$IFDEF IF_BASE_MEMBER}virtual;{$ENDIF}
    function ClipboardRegisterFormat(const AMimeType: string): TClipboardFormat; {$IFDEF IF_BASE_MEMBER}virtual;{$ENDIF}
    

Последовательность вызовов при запуске программы: 
    
    
    TCocoaWidgetSet.ClipboardRegisterFormat AMimeType=image/bmp Result=51027040
    TCocoaWidgetSet.ClipboardRegisterFormat AMimeType=image/delphi.bitmap Result=0
    TCocoaWidgetSet.ClipboardRegisterFormat AMimeType=image/xpm Result=0
    TCocoaWidgetSet.ClipboardRegisterFormat AMimeType=image/png Result=0
    TCocoaWidgetSet.ClipboardRegisterFormat AMimeType=image/jpeg Result=0
    

Последовательность вызовов во время присвоения "Caption:= Clipboard.AsText": 
    
    
    TCocoaWidgetSet.ClipboardRegisterFormat AMimeType=text/plain Result=51026976
    TCocoaWidgetSet.ClipboardGetData ClipboardType=clipboard FormatID: 51026976
    

### TSpeedButton

Этот элемент управления является потомком TGraphicControl, поэтому он не имеет дескриптора и рисуется сам. Чтобы сохранить собственный внешний вид, он использует функцию `ThemeServices.DrawElement(Details.Element=teButton)` для отрисовки самого себя, поэтому реализуйте эту функцию, чтобы этот элемент управления работал. Естественно, многие другие DC, WindowOrg, рисование текста и рисование должны работать, как объяснено для TLabel. 

### TRadioButton и TCheckButton

**Сообщения:**

В основном Widgetset должен делать: 

  * Отправлять LM_CHANGE, когда радиокнопка не отмечена/отмечена (LCL позаботится о том, будет ли вызываться OnClick или нет)
  * Не отправлять LM_CHANGE, когда вызывается TWSCustomCheckBox.SetState(SetChecked)



См. также этот отчет об ошибке: <http://bugs.freepascal.org/view.php?id=13939>

### Поддержка FullScreen

FullScreen(окно на весь экран без рамки - прим.перев.) реализован как состояние окна wsFullScreen и, как таковой, реализован в LCLIntf.ShowWindow, где должна обрабатываться константа SW_SHOWFULLSCREEN. 

## Пример работы интерфейсов LCL

Ниже приведен простой пример. Предположим, вы разработали компонент trayicon. Как бы вы реализовали его в LCL для правильной работы на различных поддерживаемых платформах? 

Вам нужно будет сгенерировать следующие файлы: 
    
    
    \trayicon.pas
    \wstrayicon.pas
    \gtk\gtkwstrayicon.pas
    \gtk\trayintf.pas
    \win32\win32wstrayicon.pas
    \win32\trayintf.pas

Предоставление отдельных файлов wsXXX.pas и \$(LCLWidgetType)\XXXintf.pas полностью исключает необходимость использования ifdefs. Вам нужно будет добавить в качестве пути модуля $(LCLWidgetType) в файл XXXintf.pas, чтобы инициализировать корректный файл trayintf.pas, который, в свою очередь, инициализирует корректный класс WS Tray. 

Внутри trayicon.pas вы включаете wstrayicon. Извлеките свой основной класс из класса LCL и используйте только wstrayicon в его реализации. Все классы LCL, которые взаимодействуют с набором виджетов, должны быть получены из [TLCLComponent](<http://lazarus-ccr.sourceforge.net/docs//lcl/lclclasses/tlclcomponent.html> "doc:/lcl/lclclasses/tlclcomponent.html"), объявленного в модуле [LCLClasses](<http://lazarus-ccr.sourceforge.net/docs//lcl/lclclasses> "doc:/lcl/lclclasses"). 
    
    
    unit TrayIcon;
    
    interface
    
    type
      TTrayIcon = class(TLCLComponent)
      public
        procedure DoTray;
      end;
    
    implementation
    
    uses wstrayicon;
    
    procedure TTrayIcon.DoTray;
    begin
      // вызов wstrayicon
    end;
    
    end.
    

в trayintf вы используете gtkwstrayicon или win32trayicon, в зависимости от того, какой это файл trayintf. 

в wstrayicon вы создаете класс следующим образом: 
    
    
    unit WSTrayIcon;
    
    uses WSLCLClasses, Controls, TrayIcon; // и другие модули
    
    TWSTrayIconClass = class of TWSTrayIcon;
    TWSTrayIcon = class(TWSWinControl);
    public
     // все они должны быть виртуальными и классовыми процедурами !!
     class procedure EmbedTrayIcon(const ATrayIcon: TCustomTrayIcon); virtual; 
     class procedure RemoveTrayIcon(const ATrayIcon: TCustomTrayIcon); virtual;
     ....
    end;
    ...
    
    implementation
    
    procedure TWSTrayIcon.EmbedTrayIcon(const ATrayIcon: TCustomTrayIcon);
    begin
     //ничего не делаем
    end;
    
    procedure TWSTrayIcon.RemoveTrayIcon(const ATrayIcon: TCustomTrayIcon);
    begin
     //ничего не делаем
    end;
    

теперь в gtkwstrayicon.pas сделайте так: 
    
    
    uses WSTrayIcon, WSLCLClasses, Controls, TrayIcon, gtk, gdk;
    
    
    TGtkWSTrayIcon = class(TWSTrayIcon);
    private
     class function FindSystemTray(const ATrayIcon: TCustomTrayIcon): TWindow; virtual;public
     class procedure EmbedTrayIcon(const ATrayIcon: TCustomTrayIcon); override;
     class procedure RemoveTrayIcon(const ATrayIcon: TCustomTrayIcon); override;
     class function  CreateHandle(const AWinControl: TWinControl; const AParams: TCreateParams): HWND; override;
     ....
    end;
    ...
    
    implementation
    
    procedure TGtkWSTrayIcon.CreateHandle(const AWinControl: TWinControl; const AParams: TCreateParams): HWND;
    var
    WidgetInfo: PWidgetInfo;
    begin
     Result := gtk_plug_new;
     WidgetInfo := CreateWidgetInfo(AWinControl, Result); // как-то так или иначе
     TGtkWSWincontrolClass(WidgetSetClass).SetCallbacks(AWinControl);
     // и многое другое
    end;
    
    function TGtkWSTrayIcon.FindSystemTray(const ATrayIcon:
    TCustomTrayIcon): TWindow;
    begin
     // что-то делаем
    end;
    
    
    procedure TGtkWSTrayIcon.EmbedTrayIcon(const ATrayIcon: TCustomTrayIcon);
    var
    SystemTray: TWindow;
    begin
     SystemTray := FindSystemTray(ATrayIcon);
     //что-то делаем
    end;
    
    procedure TGtkWSTrayIcon.RemoveTrayIcon(const ATrayIcon: TCustomTrayIcon);
    begin
     //что-то делаем
    end;
    
    ......
    
    initialization
    
    RegisterWSComponent(TCustomTrayIcon, TGtkWSTrayIcon); //вот это очень важно!!!
    
    end.
    

затем наконец-то в trayicon.pas вы пишете как обычно 
    
    
    uses WSTrayIcon; //и т.д., вы НЕ включаете сюда GtkWSTrayIcon!
    
    TCustomTrayIcon = class(TWinControl)
    public
     procedure EmbedControl;
    ....
    end;
    
    ...
    procedure TTrayIcon.EmbedControl;
    begin
     TWSTrayIconClass(WidgetSetClass).EmbedControl(Self);
    end;
    

* * *

Этот документ находится в стадии разработки. Вы можете помочь, написав разделы этого документа. Если вы не можете найти информацию, которую вы ищете в этом документе, пожалуйста, добавьте свой вопрос на [страницу обсуждения](</Talk:LCL_Internals> "Talk:LCL Internals"). Это поможет нам писать документацию на уровне, который не является ни слишком простым, ни слишком сложным, и полностью охватывает области, о которых люди хотят знать.

---

_Source: [https://wiki.freepascal.org/LCL_Internals/ru](https://web.archive.org/web/20211201091234/https://wiki.freepascal.org/LCL_Internals/ru)_
