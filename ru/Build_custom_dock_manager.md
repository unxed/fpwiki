# Build custom dock manager

│ **[English (en)](<../en/Build_custom_dock_manager.md> "Build custom dock manager")** │  **русский (ru)** │    
****

## Contents

  * 1 Введение
  * 2 Возможные функции стыковки
  * 3 Перетаскивание
  * 4 Подсистема стыковки
    * 4.1 Поддержка TControl
    * 4.2 Поддержка TWinControl
  * 5 Реализация базового настраиваемого DockManager
  * 6 See also



## Введение

VCL и LCL поддерживают базовую стыковку элементов управления. Для расширенного режима стыковки необходимо создать настраиваемый диспетчер стыковочного узла. Базовый абстрактный класс диспетче растыковочного узла TDockManager расположен в модуле Controls. Все настраиваемые менеджеры должны быть производными от этого класса. Базовая реализация TDockManager в LCL - это TDockTree, способная обрабатывать дерево стыковочных зон. 

Рекомендуемая литература: [LCL Drag Dock](<../en/LCL_Drag_Dock.md> "LCL Drag Dock")

## Возможные функции стыковки

  * Иерархия зоны стыковки 
    * Один элемент
    * Линейный список
    * Дерево
    * Таблица
    * Привязанный макет
    * Еще одна сложная организация
  * Пристыкованные вкладки
  * Пристыковка нескольких форм к плавающему объединенному окну
  * Темы
  * Постоянство, хранение/загрузка состояния макета стыковки
  * Пристыковка вкладок с автоматическим отображением/скрытием форм, кнопка с изображением булавки для переключения автоматического скрытия/отображения
  * Пользовательские кнопки заголовка
  * Создание макета по умолчанию прямо из кода, ручное управление
  * Поддержка визуальных компонентов времени разработки, настройка формы стыковки
  * Класс для глобальных операций пристыковки



## Перетаскивание

Пристыковка - это, по сути, особый случай действия перетаскивания для форм и подобных элементов управления. Поэтому основные события элементов управления аналогичны. 

События перетаскивания: `OnDragOver`, `OnDragDrop`, `OnStartDrag`, `OnEndDrag`. События стыковки: `OnDockOver`, `OnDockDrop`, `OnStartDock`, `OnEndDock`, `OnUnDock`. 

Перетаскиваемый объект представлен классом `TDragObject`. Этот класс расширен до `TDragControlObject`; затем это расширяется до `TDragDockObject` для поддержки функций стыковки. Весь процесс перетаскивания контролируется `TDragManager`, аналогично `TDockManager`, управляющему процессом закрепления. 

## Подсистема стыковки

### Поддержка TControl
    
    
    TControl = class
      ...
      procedure DragDrop(Source: TObject; X,Y: Integer); virtual;
      procedure Dock(NewDockSite: TWinControl; ARect: TRect); virtual;
      function ManualDock(NewDockSite: TWinControl;
        DropControl: TControl = nil; ControlSide: TAlign = alNone;
        KeepDockSiteSize: Boolean = true): Boolean; virtual;
      function ManualFloat(TheScreenRect: TRect;
        KeepDockSiteSize: Boolean = true): Boolean; virtual;
    
      property DockOrientation: TDockOrientation read FDockOrientation write FDockOrientation;
      property Floating: Boolean read GetFloating;
      property FloatingDockSiteClass: TWinControlClass read GetFloatingDockSiteClass 
        write FFloatingDockSiteClass;
      property HostDockSite: TWinControl read FHostDockSite write SetHostDockSite;
      property LRDockWidth: Integer read GetLRDockWidth write FLRDockWidth;
      property TBDockHeight: Integer read GetTBDockHeight write FTBDockHeight;
      property UndockHeight: Integer read GetUndockHeight write FUndockHeight; // Высота, используемая при отстыковке
      property UndockWidth: Integer read GetUndockWidth write FUndockWidth; // Ширина, используемая при отстыковке
      ...
    end;
    

### Поддержка TWinControl
    
    
    TWinControl = class(TControl)
      ...
      property DockClientCount: Integer read GetDockClientCount;
      property DockClients[Index: Integer]: TControl read GetDockClients;
      property DockManager: TDockManager read FDockManager write SetDockManager;
      property DockSite: Boolean read FDockSite write SetDockSite default False;
      property OnUnDock: TUnDockEvent read FOnUnDock write FOnUnDock;
      property UseDockManager: Boolean read FUseDockManager
        write SetUseDockManager default False;
      property VisibleDockClientCount: Integer read GetVisibleDockClientCount;
      procedure DockDrop(DragDockObject: TDragDockObject; X, Y: Integer); virtual;
      ...
    end;
    

## Реализация базового настраиваемого DockManager

Наш собственный менеджер можно получить из TDockTree по умолчанию. Но для создания полностью настраиваемого менеджера родительский класс должен быть TDockManager. Все методы родительского класса следует переопределить. 

  * Для базовой демонстрации диспетчер стыковочного узла должен уметь управлять одним элементом управления. Позже этот класс может быть расширен.



Вот базовый прототип, основанный на виртуальных методах TDockManager. 
    
    
    TCustomDockManager = class(TDockManager)
      constructor Create(ADockSite: TWinControl); override;
      procedure BeginUpdate; override;
      procedure EndUpdate; override;
      procedure GetControlBounds(Control: TControl;
        out AControlBounds: TRect); override;
      function GetDockEdge(ADockObject: TDragDockObject): boolean; override;
      procedure InsertControl(ADockObject: TDragDockObject); override; overload;
      procedure InsertControl(Control: TControl; InsertAt: TAlign;
        DropCtl: TControl); override; overload;
      procedure LoadFromStream(Stream: TStream); override;
      procedure PaintSite(DC: HDC); override;
      procedure MessageHandler(Sender: TControl; var Message: TLMessage); override;
      procedure PositionDockRect(ADockObject: TDragDockObject); override; overload;
      procedure PositionDockRect(Client, DropCtl: TControl; DropAlign: TAlign;
        var DockRect: TRect); override; overload;
      procedure RemoveControl(Control: TControl); override;
      procedure ResetBounds(Force: Boolean); override;
      procedure SaveToStream(Stream: TStream); override;
      procedure SetReplacingControl(Control: TControl); override;
      function AutoFreeByControl: Boolean; override;    
    end;
    

  * DockManager должен знать, к какому TWinControl принадлежит. Итак, менеджеру необходимо поле FDockSite типа TWinControl, которое назначается конструктором.


    
    
    private
      FDockSite: TWinControl;
    

  * Теперь мы можем реализовать метод PositionDockRect, который определяет размер фрейма стыковочного узла в соответствии с размером DockSite.


    
    
    procedure TCustomDockManager.PositionDockRect(Client, DropCtl: TControl; DropAlign: TAlign;
      var DockRect: TRect); override; overload;
    begin
      DockRect := Rect(0, 0, FDockSite.ClientWidth, FDockSite.ClientHeight);
    end;
    

  * Чтобы сделать диспетчер стыковочного узла по умолчанию для всех пристыковываемых элементов управления, вставьте раздел инициализации в конец модуля. Затем простое включение модуля менеджера стыковочного узла в модуль основной формы настроит вашу систему стыковки.


    
    
    initialization
      DefaultDockManagerClass := TCustomDockManager;
    

  * Если размер стыковочного узла изменяется, выполняется метод `ResetBounds`. Затем мы можем использовать его для размещения стыкуемого элемента управления на стыковочном узле. Нам нужно выделить место поверх элемента управления для граббера(захватчика) стыкуемого элемента.


    
    
    const
      GrabberSize = 18;
    
    procedure TCustomDockManager.ResetBounds(Force: Boolean);
    var
      I: Integer;
      Control: TControl;
      R: TRect;
    begin
      for I := 0 to FDockSite.ControlCount - 1 do
        begin
          Control := FDockSite.Controls[I];
          if Control.Visible and (Control.HostDockSite = FDockSite) then
          begin
            R := Control.BoundsRect;
            Control.SetBounds(0, GrabberSize, FDockSite.Width - Control.Left,
              FDockSite.Height - Control.Top);
          end;
        end;
    end;
    

  * Теперь нам нужно нарисовать граббер с именем элемента поверх стыковочного узла.


    
    
    procedure TCustomDockManager.DrawGrabber(Canvas: TControlCanvas; AControl: TControl);
    begin
      with Canvas do begin
        Brush.Color := clBtnFace;
        Pen.Color := clBlack;
        FillRect(0, 0, AControl.Width, GrabberSize);
        Rectangle(1, 1, AControl.Width - 1, GrabberSize - 1);
        TextOut(6, 2, AControl.Caption);
      end;
    end;
    

Для отрисовки стыковочного узла холст использует метод `PaintSite`, поэтому нам нужно его реализовать. Должны быть отрисованы только видимые элементы управления, принадлежащие стыковочному узлу. 
    
    
    procedure TCustomDockManager.PaintSite(DC: HDC);
    var
      Canvas: TControlCanvas;
      Control: TControl;
      I: Integer;
      R: TRect;
    begin
      Canvas := TControlCanvas.Create;
      try
        Canvas.Control := FDockSite;
        Canvas.Lock;
        try
          Canvas.Handle := DC;
          try
            for I := 0 to FDockSite.ControlCount - 1 do
            begin
              Control := FDockSite.Controls[I];
              if Control.Visible and (Control.HostDockSite = FDockSite) then
              begin
                R := Control.BoundsRect;
                Control.SetBounds(0, GrabberSize, FDockSite.Width - Control.Left,
                  FDockSite.Height - Control.Top);
                Canvas.FillRect(R);
                DrawGrabber(Canvas, Control);
              end;
            end;
          finally
            Canvas.Handle := 0;
          end;
        finally
          Canvas.Unlock;
        end;
      finally
        Canvas.Free;
      end;
    end;
    

  


_продолжение следует..._

## See also

  * [LCL Drag Dock](<LCL_Drag_Dock.md> "LCL Drag Dock/ru")

---

_Source: [https://wiki.freepascal.org/Build_custom_dock_manager/ru](https://web.archive.org/web/20250117040728/https://wiki.freepascal.org/Build_custom_dock_manager/ru)_
