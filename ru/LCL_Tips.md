# LCL Tips

│ **[Deutsch (de)](</LCL_Tips/de> "LCL Tips/de")** │  **[English (en)](<../en/LCL_Tips.md> "LCL Tips")** │  **[français (fr)](</LCL_Tips/fr> "LCL Tips/fr")** │  **русский (ru)** │  **[中文（中国大陆） (zh_CN)](</LCL_Tips/zh_CN> "LCL Tips/zh CN")** │    
****

## Contents

  * 1 Создание графического интерфейса с помощью кода
  * 2 Создание элементов управления вручную без потери быстродействия
    * 2.1 Устанавливайте свойство Parent в последнюю очередь
    * 2.2 Избегайте раннего создания хэндла (Handle)
    * 2.3 Используйте SetBounds вместо Left, Top, Width, Height
  * 3 DisableAlign/EnableAlign
  * 4 Создание не прямоугольных окон или элементов управления
  * 5 Имитация событий мыши и ввода с клавиатуры
  * 6 Показ виртуальной клавиатуры в смартфоне/планшете
  * 7 Перебор по всем дочерним элементам управления TWinControl



## Создание графического интерфейса с помощью кода

При работе в среде Lazarus, графический пользовательский интерфейс (GUI) можно полностью создавать с помощью Pascal-кода. При этом доступны все возможности, которые обычно используются через интерфейс IDE. Далее рассмотрены примеры программы и модуля (codegui.lpr and mainform.pas) которые вы можете использовать в качестве шаблона. Важнее всего – не забывать устанавливать свойство Parent компонентов. Элементы управления для размещения на форме лучше всего создавать в конструкторе формы: 

Файл основной программы: 
    
    
    program codedgui;
    
    {$MODE DELPHI}{$H+}
    
    uses
      Interfaces, Forms, StdCtrls,
      MainForm;
    
    var
      MyForm: TMyForm;
    begin
      Application.Initialize;
      Application.CreateForm(TMyForm, MyForm);
      Application.Run;
    end.
    

Модуль, содержащий форму: 
    
    
    unit mainform;
    
    {$MODE DELPHI}{$H+}
    
    interface
    
    uses Forms, StdCtrls;
    
    type
      TMyForm = class(TForm)
      public
        MyButton: TButton;
        procedure ButtonClick(ASender: TObject);
        constructor Create(AOwner: TComponent); override;
      end;
    
    implementation
    
    procedure TMyForm.ButtonClick(ASender:TObject);
    begin
      Close;
    end;
    
    constructor TMyForm.Create(AOwner: TComponent);
    begin
      inherited Create(AOwner);
    
      Position := poScreenCenter;
      Height := 400;
      Width := 400;
    
      VertScrollBar.Visible := False;
      HorzScrollBar.Visible := False;
    
      MyButton := TButton.Create(Self);
      with MyButton do
      begin
        Height := 30;
        Left := 100;
        Top := 100;
        Width := 100;
        Caption := 'Закрыть';
        OnClick := ButtonClick;
        Parent := Self;
      end;
    
      // Сюда можно добавить код создания других компонентов и установки их свойств
    end;
    
    end.
    

## Создание элементов управления вручную без потери быстродействия

### Устанавливайте свойство Parent в последнюю очередь

**Для пользователей, перешедших с Delphi** : В отличие от Delphi, LCL позволяет устанавливать практически все свойства в любой последовательности. Например, в Delphi невозможно установить позицию элемента управления, если для него не установлен родительский элемент (свойство Parent). LCL позволяет это сделать, и это можно использовать для повышения быстродействия. 
    
    
      with TButton.Create(Form1) do begin
        // 1. создаём кнопку с размером по умолчанию
        // 2. изменяем положение. Пока ничего не меняется, потому что Parent=nil
        SetBounds(10,10,Width,Height);
        // 3. изменяем размер в зависимости от темы. 
        // Пока что размер не меняется, потому что Parent=nil
        AutoSize:=true;
        // 4. далее размер должен поменяться из-за включенного свойства AutoSize
        // Но опять это изменение откладывается, потому что Parent=nil
        Caption:='ОК';
        // 5. устанавливаем свойство Parent. Вот теперь все изменения применяются, 
        // но за один раз, как одно действие.
        Parent:=Form1;
      end;
    

Когда для элемента управления задано свойство Parent, все изменения его свойств сразу же применяются. Когда свойство Parent не задано, многие свойства только изменяют хранимое значение, и как только Parent будет установлен, все изменения будут применены. Это особенно актуально для компонентов-"внуков": 
    
    
      GroupBox1:=TGroupBox.Create(Self);
      with GroupBox1 do begin
        with TButton1.Create(Self) do begin
          AutoSize:=true;
          Caption:='Нажми меня';
          Parent:=GroupBox1;
        end;
        Parent:=Form1;
      end;
      Form1.Show;
    

Автоматическая установка размера начинается только когда для всех элементов установлены родительские элементы и форма становится видимой. 

### Избегайте раннего создания хэндла (Handle)

Как только для TWinControl создаётся хэндл, все изменения свойств элемента начинают менять его графическое представление (называемое виджетом). Даже если элемент невидим, если у него есть хэндл, изменения требуют много вычислительных ресурсов. 

### Используйте SetBounds вместо Left, Top, Width, Height

Вместо 
    
    
      with Button1 do begin
        Left:=10;
        Top:=10;
        Width:=100;
        Height:=25;
      end;
    

используйте 
    
    
      with Button1 do begin
        SetBounds(10,10,100,25);
      end;
    

Left, Top, Width, Height вызывают SetBounds, и каждое изменение размера или позиции приводит к повторному вычислению параметров соседних элементов, и возможно рекурсивному вычислению параметров родительских и/или дочерних элементов. 

## DisableAlign/EnableAlign

При позиционировании многих элементов управления полезно отключать автоматическое вычисление размера, выравнивания и привязок. 
    
    
      DisableAlign;
      try
        ListBox1.Width:=ClientWidth div 3;
        ListBox2.Width:=ClientWidth div 3;
        ListBox3.Width:=ClientWidth div 3;
      finally
        EnableAlign;
      end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Каждый вызов DisableAlign требует вызова EnableAlign. Например, если вы вызываете DisableAlign два раза, то должны вызвать EnableAlign тоже два раза.

**Для пользователей, перешедших с Delphi** : Данные вызовы работают рекурсивно. DisableAlign запрещает выравнивание всех дочерних элементов, элементов-"внуков" и т.д. 

## Создание не прямоугольных окон или элементов управления

В Lazarus можно легко создавать не прямоугольные окна или элементы управления. Для этого можно просто вызвать TWinControl.SetShape с видимой областью в качестве параметра. Обратите внимание, что это будет работать как для окон, так и для элементов управления, так как TCustomForm и TCustomControl происходят от TWinControl. Можно также вызвать подпрограмму LCLIntf SetWindowRgn, которая полностью эквивалентна вызову метода SetShape. 

С использованием SetWindowRgn код будет похож на следующий: 
    
    
    uses LCLIntf, LCLType;
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      MyRegion: HRGN;
    begin
      MyRegion := CreateRectRgn(0, 0, 100, 100);
      SetWindowRgn(Handle, MyRegion, True);
      DeleteObject(MyRegion);
    end;
    

Эквивалентный код, который использует объект TRegion более высокого уровня, доступен в Lazarus 0.9.31+ (обратите внимание, что предыдущий способ работы все еще поддерживается и будет [поддерживаться] в будущем): 
    
    
    uses Graphics;
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      MyRegion: TRegion;
    begin
      MyRegion := TRegion.Create;
      try
        MyRegion.AddRectangle(0, 0, 100, 100);
        Self.SetShape(MyRegion);
      finally
        MyRegion.Free;
      end;
    end;
    

Результат этой операции в окне в macOS с использованием набора виджетов Qt можно увидеть здесь: 

[![non rectangular window.png](https://wiki.freepascal.org/images/e/ef/non_rectangular_window.png)](</File:non_rectangular_window.png>)

Обратите внимание, что SetShape также может принять TBitmap для описания прозрачной области. 

См.также: 

  * [LCL_Internals#Shaped_Windows](<../en/LCL_Internals.md> "LCL Internals")



Соответствующая документация: 

  * <http://lazarus-ccr.sourceforge.net/docs/lcl/lclintf/setwindowrgn.html>
  * <http://lazarus-ccr.sourceforge.net/docs/lcl/controls/twincontrol.setshape.html>



Ограничения: 

В Gtk2 регион может быть установлен только после того, как окно реализовано. Вызов SetWindowRgn в обработчике события OnShow не работает, единственный способ - вызвать его, например, из таймера, установленного с интервалом 1. Включите таймер в Form.OnShow и отключите его в обработчике OnTimer. 

## Имитация событий мыши и ввода с клавиатуры

Имитировать ввод с помощью мыши и клавиатуры в LCL очень легко, просто используйте процедуры из модуля LCLMessageGlue, как это делают все интерфейсы widgetset. Этот модуль имеет процедуры, такие как: 
    
    
    unit LCLMessageGlue;
    
    function LCLSendMouseMoveMsg(const Target: TControl; XPos, YPos: smallint;
      ShiftState: TShiftState = []): PtrInt;
    function LCLSendMouseDownMsg(const Target: TControl; XPos, YPos: smallint;
      Button: TMouseButton; ShiftState: TShiftState = []): PtrInt;
    function LCLSendMouseUpMsg(const Target: TControl; XPos, YPos: smallint;
      Button: TMouseButton; ShiftState: TShiftState = []): PtrInt;
    function LCLSendMouseWheelMsg(const Target: TControl; XPos, YPos, WheelDelta: smallint;
      ShiftState: TShiftState = []): PtrInt;
    function LCLSendCaptureChangedMsg(const Target: TControl): PtrInt;
    function LCLSendSelectionChangedMsg(const Target: TControl): PtrInt;
    function LCLSendClickedMsg(const Target: TControl): PtrInt;
    function LCLSendMouseEnterMsg(const Target: TControl): PtrInt;
    function LCLSendMouseLeaveMsg(const Target: TControl): PtrInt;
    ...
    function LCLSendKeyDownEvent(const Target: TControl; var CharCode: word;
      KeyData: PtrInt; BeforeEvent, IsSysKey: boolean): PtrInt;
    function LCLSendKeyUpEvent(const Target: TControl; var CharCode: word;
      KeyData: PtrInt; BeforeEvent, IsSysKey: boolean): PtrInt;
    

## Показ виртуальной клавиатуры в смартфоне/планшете

Чтобы показать виртуальную клавиатуру, когда виджет получает фокус, в ControlStyle просто добавьте csRequiresKeyboardInput: 
    
    
    constructor TMyTextEditor.Create(AOwner : TComponent);
    begin
      inherited Create(AOwner);
      ControlStyle := ControlStyle + [csRequiresKeyboardInput];
    ...
    end;
    

[![Virtual keyboard.png](https://wiki.freepascal.org/images/2/29/Virtual_keyboard.png)](</File:Virtual_keyboard.png>)

## Перебор по всем дочерним элементам управления TWinControl

Это очень легко, просто используйте цикл для перебора TWinControl.ControlCount и TWinControl.Controls[Index]. Индекс начинается с нуля. 
    
    
    procedure TWinControl.WriteLayoutDebugReport(const Prefix: string);
    var
      i: Integer;
    begin
      inherited WriteLayoutDebugReport(Prefix);
      for i:=0 to ControlCount-1 do
        Controls[i].WriteLayoutDebugReport(Prefix+'  ');
    end;

---

_Source: [https://wiki.freepascal.org/LCL_Tips/ru](https://web.archive.org/web/20241201000000/https://wiki.freepascal.org/LCL_Tips/ru)_
