# LCL Tips

│ **[Deutsch (de)](</LCL_Tips/de> "LCL Tips/de")** │  **English (en)** │  **[français (fr)](</LCL_Tips/fr> "LCL Tips/fr")** │  **[русский (ru)](<../ru/LCL_Tips.md> "LCL Tips/ru")** │  **[中文（中国大陆）‎ (zh_CN)](</LCL_Tips/zh_CN> "LCL Tips/zh CN")** │    
****

## Contents

  * 1 Creating a GUI by code
  * 2 Create controls manually without overhead
    * 2.1 Set the Parent as last
    * 2.2 Avoid early Handle creation
    * 2.3 Use SetBounds instead of Left, Top, Width, Height
  * 3 DisableAlign / EnableAlign
  * 4 Creating a non-rectangular window or control
  * 5 Simulating Mouse and Keyboard input
  * 6 Showing the Virtual Keyboard in Smartphones/tablets
  * 7 Iterating through all child controls of a TWinControl
  * 8 Responding only once to an event



## Creating a GUI by code

It is possible to create the [GUI](<Overview_of_Free_Pascal_and_Lazarus.md> "Overview of Free Pascal and Lazarus") (Graphical User Interface) code completely by pascal code in Lazarus. Everything accessible from the IDE is also accessible by code. The example program and unit files below (codegui.lpr and mainform.pas) give you a template you can adapt. The most important part is not forgetting to set the [Parent](</index.php?title=Parent&action=edit&redlink=1> "Parent \(page does not exist\)") property of the components. The creation of controls inside the form is best done in the constructor of the form: 

Main program file: 
    
    
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
    

And a unit containing a form: 
    
    
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
      //Hint: FormCreate() is called BEFORE Create() !
      //so You can also put this code into FormCreate()
      //(This is not the case when creating components ..)
    
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
        Caption := 'Close';
        OnClick := ButtonClick;
        Parent := Self;
      end;
    
      // Add other component creation and property setting code here
    end;
    
    end.
    

## Create controls manually without overhead

### Set the Parent as last

**For Delphians** : Contrary to Delphi the LCL (Lazarus Component Library) allows you to set nearly all properties in any order. For example under Delphi you cannot position a control if it has no parent. The LCL allows this and this feature can be used to reduce overhead. 
    
    
    with TButton.Create(Form1) do begin
      // 1. creating a button sets the default size
      // 2. change position. No side effects, because Parent=nil
      SetBounds(10,10,Width,Height);
      // 3. change size depending on theme. Not yet, because Parent=nil
      AutoSize:=true;
      // 4. changing size because of AutoSize=true. Not yet, because Parent=nil
      Caption:='Ok';
      // 5. Set Parent. Now all the above takes place, but in a single action.
      Parent:=Form1;
    end;
    

When a control has a Parent, then all properties take effect immediately. Without a Parent many properties do nothing more than store the value. And as soon as the Parent is set every property is applied. This is especially true for grand children: 
    
    
    GroupBox1:=TGroupBox.Create(Self);
    with GroupBox1 do begin
      with TButton1.Create(Self) do begin
        AutoSize:=true;
        Caption:='Click me';
        Parent:=GroupBox1;
      end;
      Parent:=Form1;
    end;
    Form1.Show;
    

Autosizing starts only after every parent is set up and the form becomes visible. 

### Avoid early Handle creation

As soon as the Handle of a TWinControl is created, every change of a property changes the visual thing (called the widget). Even if a control is not visible, when it has a Handle, changes are still expensive. 

### Use SetBounds instead of Left, Top, Width, Height

Instead of 
    
    
    with Button1 do begin
      Left:=10;
      Top:=10;
      Width:=100;
      Height:=25;
    end;
    

Use 
    
    
    with Button1 do begin
      SetBounds(10,10,100,25);
    end;
    

Left, Top, Width, Height are calling SetBounds. And every change of position or size invokes recalculation of all sibling controls and maybe recursively the parent and/or the grandchild controls. 

## DisableAlign / EnableAlign

When positioning many controls, it is a good idea to disable the recalculation of all auto sizing, aligning, anchoring. 
    
    
    DisableAlign;
    try
      ListBox1.Width:=ClientWidth div 3;
      ListBox2.Width:=ClientWidth div 3;
      ListBox3.Width:=ClientWidth div 3;
    finally
      EnableAlign;
    end;
    

**Note** : Every DisableAlign call needs an EnableAlign call. For example if you call DisableAlign two times, you must call EnableAlign twice as well. 

**For Delphians** : This works recursively. That means DisableAlign stops aligning in all child and grandchild controls. 

## Creating a non-rectangular window or control

One can easily create non-rectangular windows or controls in Lazarus. For this, one can simply call TWinControl.SetShape, with the visible region as a parameter. Note that this will work for both windows and controls, as both TCustomForm and TCustomControl descend from TWinControl. One can also call the LCLIntf routine SetWindowRgn, which is completely equivalent to calling the SetShape method. 

Using SetWindowRgn the code will be similar to this: 
    
    
    uses LCLIntf, LCLType;
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      MyRegion: HRGN;
    begin
      MyRegion := CreateRectRgn(0, 0, 100, 100);
      SetWindowRgn(Handle, MyRegion, True);
      DeleteObject(MyRegion);
    end;
    

An equivalent code, which uses a higher level TRegion object available in Lazarus 0.9.31+ (note that the previous way of doing things is still supported and will be in the future too), is: 
    
    
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
    

The result of this operation in a window in macOS using the Qt widgetset can be seen here: 

[![non rectangular window.png](https://wiki.freepascal.org/images/e/ef/non_rectangular_window.png)](</File:non_rectangular_window.png>)

Note that SetShape can also accept a TBitmap to describe the transparent region. 

See also: 

  * [LCL_Internals#Shaped_Windows](<LCL_Internals.md> "LCL Internals")



Documentation entries: 

  * <http://lazarus-ccr.sourceforge.net/docs/lcl/lclintf/setwindowrgn.html>
  * <http://lazarus-ccr.sourceforge.net/docs/lcl/controls/twincontrol.setshape.html>



Limitations: 

In Gtk2 a region can only be set after a window is realized. Calling SetWindowRgn in the OnShow event handler doesn't work, the only way seems to be to call it from a timer set with interval 1, for example. Enable the timer in Form.OnShow and disable it in it's OnTimer handler. 

## Simulating Mouse and Keyboard input

It is very easy to simulate mouse and keyboard input in the LCL, just use the routines from the unit LCLMessageGlue like all widgetset interfaces do. This unit has routines like: 
    
    
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
    

## Showing the Virtual Keyboard in Smartphones/tablets

To show the virtual keyboard when a widget receives focus just add the csRequiresKeyboardInput ControlStyle: 
    
    
    constructor TMyTextEditor.Create(AOwner : TComponent);
    begin
      inherited Create(AOwner);
      ControlStyle := ControlStyle + [csRequiresKeyboardInput];
    ...
    end;
    

[![Virtual keyboard.png](https://wiki.freepascal.org/images/2/29/Virtual_keyboard.png)](</File:Virtual_keyboard.png>)

## Iterating through all child controls of a TWinControl

This is very easy, just use a loop to iterate over TWinControl.ControlCount and TWinControl.Controls[Index]. The index is zero based. 
    
    
    procedure TWinControl.WriteLayoutDebugReport(const Prefix: string);
    var
      i: Integer;
    begin
      inherited WriteLayoutDebugReport(Prefix);
      for i:=0 to ControlCount-1 do
        Controls[i].WriteLayoutDebugReport(Prefix+'  ');
    end;
    

## Responding only once to an event

Sometimes one wants an event handler to be called just once. Say, for example, that you want to do something only the first time a form is activated, once the form and all its controls are fully loaded (which might not be the case when OnCreate is fired). 

The solution is easy: simply set the corresponding property to Nil at the beginning of the handler: 
    
    
    procedure TMyForm.FormActivate(const Sender: TObject);
    begin
      OnActivate := Nil;
      {... rest of the code here ...}
    end;
    

and the event handler won't be called again.

---

_Source: [https://wiki.freepascal.org/LCL_Tips](https://web.archive.org/web/20240121090438/https://wiki.freepascal.org/LCL_Tips)_
