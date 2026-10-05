# LCL Internals

│ **English (en)** │  **[русский (ru)](<../ru/LCL_Internals.md>)** │

* * *

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This document is work in progress. You can help by writing sections of this document. If you cannot find the information you are looking for in this document, please add your question to the [discussion page](</Talk:LCL_Internals> "Talk:LCL Internals"). This will help us to write documentation at a level that is neither too simple nor too complicated, and fully cover the areas that people want to know about.

* * *

## Contents

  * 1 Internals of the LCL
    * 1.1 Pie and RadialPie
    * 1.2 Adding a new unit to the LCL
  * 2 How to create a new Widgetset
    * 2.1 How to implement a new windowed component
    * 2.2 Implementing TBitmap
    * 2.3 TBitmap.LoadFromDevice for screenshot taking
    * 2.4 Implementing drawing in the OnPaint event of a form or another control
    * 2.5 Implementing TLabel
    * 2.6 Implementing visibility for forms and controls and window state
    * 2.7 Implementing TStrings based Components
    * 2.8 Implementing Menus
      * 2.8.1 Menu Creation Order
    * 2.9 Control enabling/disabling
    * 2.10 Return Control Text (Captions)
    * 2.11 Shaped Windows
    * 2.12 System colors
    * 2.13 ShowMessage
    * 2.14 SpinEdit
    * 2.15 Clipboard
    * 2.16 TSpeedButton
    * 2.17 TRadioButton and TCheckButton
    * 2.18 FullScreen support
  * 3 An example of how the LCL interfaces work
  * 4 Other Interfaces
    * 4.1 Platform specific Tips
    * 4.2 Interface Development Articles
  * 5 Minimum Toolkit versions



## Internals of the LCL

There is the LCL, and the "interface". The LCL is the part that is platform independent, and it resides in the lazarus/lcl/ directory. This directory contains mainly class definitions. Many of the different controls are actually implemented in the lazarus/lcl/include/ directory in the various .inc files. This is to find the implementation of a specific control, TCustomMemo for example, faster (which is in custommemo.inc). Every .inc starts with a line {%MainUnit ...} to define where it is included. 

Then there is the "interface" which lives in a subdirectory of the lazarus/lcl/interfaces/ directory. The gtk interface is in gtk/, win32 in win32/, etc. They all have a Interfaces unit, which is used by the lcl and creates the main interface object. Usually the main interface object is defined in XXint.pp (win32int.pp), and implemented in various inc files, XXobject.inc, for the interface specific methods, XXwinapi.inc for winapi implementation methods, XXlistsl.inc for implementation of the stringlist used by the TComboBox, TListBox, and other such controls, XXcallback.inc for handling of widget events and taking appropriate action to notify the LCL. 

Every control has a WidgetSetClass property which is of the 'mirror' class in the interfaces directory, for example: mirror of TCustomEdit is TWSCustomEdit, which methods are implemented by TWin32WSCustomEdit in win32wsstdctrls. This is the way the LCL communicates with the interface, and how it lets the interface do things. 

Communication of interface back to LCL is mostly done by sending messages, usually 'DeliverMessage' which calls TControl.Perform(<message_id>, wparam, lparam) with wparam and lparam being the extra info for the message. 

### Pie and RadialPie

The LCLIntf unit contains two functions to draw pie-shapes: 
    
    
    function [Pie](<http://lazarus-ccr.sourceforge.net/docs/lcl/lclintf/pie.html> "doc:lcl/lclintf/pie.html")(DC: HDC; x1, y1, x2, y2, sx, sy, ex, ey: Integer): Boolean;
    function [RadialPie](<http://lazarus-ccr.sourceforge.net/docs/lcl/lclintf/radialpie.html> "doc:lcl/lclintf/radialpie.html")(DC: HDC; x1, y1, x2, y2, Angle1, Angle2: Integer): Boolean; 
    

The Pie function uses a two points (sx,sy) and (ex,ey) to indicate start and end of the arc. RadialPie uses Angles to indicate start and end of the arc. 

Pie calls TWidgetSet.Pie and RadialPie calls TWidgetSet.RadialPie. The default implementation of TWidgetSet.Pie is to convert the parameters to angles and to call TWidgetSet.RadialPie. TWidgetSet.RadialPie creates an array of points for the arc and calls TWidgetSet.Polygon. 

The win32 widgetset overrides TWidgetSet.Pie to call the Windows Pie function directly. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** in older versions of Lazarus there existed RadialPie with angles which did the same as the current RadialPie and RadialPie, which does the same as our Pie. Those functions have been removed in Lazarus 0.9.21.

### Adding a new unit to the LCL

First add the new unit name into allclunits.pp. 

To make sure the unit is registered in the components palette see the files RegisterLCL.pas and pkgfileslcl.inc, they are located in lazarus/packager. 

## How to create a new Widgetset

This is a step-by-step tutorial of developing a new [widgetset](<Widgetset.md> "Widgetset"). It is based on my experience creating the basics of the new qt4 interface. 

To start with, why would someone want to add an Widgetset? The answer is to be able to port existing lazarus software to more platforms, without modifying their code. 

Now, let´s write the widgetset. First of all, you need to have pascal bindings for the widget and know how to use it. Normally this is not hard. A few hours doing basic tutorials available on the internet should be enougth to get started. If the bindings do not exist already, you need to create them. If the tutorials are on another language, translate them to pascal and make them work. 

Now, for Qt I utilized Den Jean qt4 bindings for pascal, and created a very basic Qt program using them: 
    
    
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
    

The above project compiles and creates a qt4 program. Now we will use its code to write a new widgetset. After we are done, the lazarus program below will compile fine into a qt4 software: 
    
    
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
    

Where the form is maintained by Lazarus IDE and designed visually. 

The first thing to do on a new widgetset is add an empty skeleton for it. Very early development widgetsets, like qt and carbon, can serve as an skeleton. 

Looking at the files on the many widgets you can see the first file to be called by the lcl: Interfaces.pas This file just calls another called QtInt.pas or similar. QtInt.pas has the code for the TWidgetSet class, which we must implement. On an empty skeleton you can see that the class has various functions it must implement: 
    
    
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
    

### How to implement a new windowed component

Windowed components are all descendents from TWinControl. Those controls have a Handle and thus, should be created by the Widgetset. It's easy to add new windowed components to a widgetset. 

Let's say you want to add TQtWSCustomEdit to Qt Widgetset. To start with TCustomEdit is a descendent of TWinControl and is located on the StdCtrls unit. 

Now, go to QtWSStrCtrls unit and look for the declaration of TQtWSCustomEdit. 
    
    
      
    TQtWSCustomEdit = class(TWSCustomEdit)
      private
      protected
      public
      end;
    

Add static methods that are declared on TWSCustomEdit and override them. The code should now look like this: 
    
    
      
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
    

The commented part of the code are procedures you need to implement for TCustomEdit to be fully functional, but just CreateHandle and DestroyHandle should be enough for it to be show on the form and be editable, so it fits our needs in this article. 

Hit CTRL+SHIFT+C to code complete and the implement CreateHandle and DestroyHandle. In the case of Qt4 the code will be like this: 
    
    
    { TQtWSCustomEdit }
    
    class function TQtWSCustomEdit.CreateHandle(const AWinControl: TWinControl;
      const AParams: TCreateParams): HWND;
    var
      Widget: QWidgetH;
      Str: WideString;
    begin
      // Creates the widget
      WriteLn('Calling QTextDocument_create');
      Str := WideString((AWinControl as TCustomMemo).Lines.Text);
      Widget := QTextEdit_create(@Str, QWidgetH(AWinControl.Parent.Handle));
    
      // Sets it's initial properties
      QWidget_setGeometry(Widget, AWinControl.Left, AWinControl.Top,
       AWinControl.Width, AWinControl.Height);
    
      QWidget_show(Widget);
    
      Result := THandle(Widget);
    end;
    
    class procedure TQtWSCustomEdit.DestroyHandle(const AWinControl: TWinControl);
    begin
      QTextEdit_destroy(QTextEditH(AWinControl.Handle));
    end;
    

Now uncomment the like "RegisterWSComponent(TCustomEdit, TQtWSCustomEdit);" on the bottom of the unit and that's it! 

You can now drop a TCustomEdit on the bottom of a form and expect it to work. :^) 

### Implementing TBitmap

To implement TBitmap it is necessary to understand TRawImage and TLazIntfImage as explained here: [Developing with Graphics#Working with TLazIntfImage.2C TRawImage and TLazCanvas](<Developing_with_Graphics.md> "Developing with Graphics")

So, let's say you want to compile the following code: 
    
    
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
    

Below is the order on which functions from the widgetset interface are called when executing that code: 

1 - BeginPaint 

This will be called only if the OnPaint event sends zero as the DC for the paint event 

2 - GetDC(0); 

Just create a device context. 

3 - TCDWidgetSet.RawImage_QueryDescription 

The default implementation of this routine is good for most widgetsets 

4 - TCDWidgetSet.RawImage_CreateBitmaps 

Here you need to create a native image object and load it from RawData.Data where the information is stored based on your description of the pixel format on item 2. 

5 - CreateCompatibleDC(0) 

This creates a temporary DC just to store the image, but at this point there is no information about the image so at this point this DC is really dummy 

6 - SelectObject 

With the image as the object to be selected and the DC just created above as target DC. 

7 - StretchMaskBlt 

Finally the drawing function! DestDC is the DC allocated on BeginPaint. 

8 - EndPaint 

Again, not always utilized. 

### TBitmap.LoadFromDevice for screenshot taking

It is recomended that you first implement TBitmap before trying this step. 

On LCL you can use the following code takes a screenshot from the entire screen and paints it on the canvas: 
    
    
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
    

If you already implemented TBitmap, there are only 2 new functions to be implemented for LoadFromDevice: GetDeviceSize and GetRawImageFromDevice 

Below is a big trace, covering all widgetset functions being called on a OnPaint event that takes a screenshot and paints it on the screen. This trace was taken with Qt widgetset, and may have some imperfections. The Handle numbers should be used to check which object is being utilized on the functions. 
    
    
     [WinAPI BeginPaint] Result=-1220713544
     [WinAPI GetClientBounds]
     [WinAPI SetWindowOrgEx] DC: -1220713544 NewX: 0 NewY: 0
    
    
    
    //Enters on Paint event
    
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
    

Now exited the OnPaint event 
    
    
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
    

### Implementing drawing in the OnPaint event of a form or another control

For drawing in the OnPaint event of a form, the event itself comes from the underlaying widgetset library. The LCL interface should handle this event and create an appropriate DC object on the event handler and then call LCLSendPaintMsg. Besides that one should also implement the corresponding drawing methods, such as Rectangle or ExtTextOut. Pen, Brush and Font related routines might also be useful. 

Here is the OnPaint event handler from the Cocoa widgetset which shows how it calls LCLSentPaintMsg: 
    
    
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
    

And a list of WinAPI routines which were called when running this event: 
    
    
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
    

### Implementing TLabel

Implementing TLabel is particularly hard, despite it being such a basic component, because it requires that almost all painting be implemented. TLabel is not a windowed control, instead it depends on paint messages to be drawn directly into the form canvas. 

Before trying to get TLabel working it is recomended to test if drawing functions such as Rectangle work inside a form's OnPaint event. 

Several WinAPI methods need to be implemented, particularly: 

**Device Context Methods**

BeginPaint, GetDC, EndPaint, ReleaseDC, CreateCompatibleDC 

see [Device Contexts and GDI objects in the LCL interfaces](<Device_Contexts_and_GDI_objects_in_the_LCL_interfaces.md> "Device Contexts and GDI objects in the LCL interfaces")

**GDI Objects Methods**

SelectObject, DeleteObject, CreateFontIndirect, CreateFontIndirectEx 

**Miscelaneous functions**

InvalidateRect, GetClientBounds, SetWindowOrgEx 

**Text drawing Methods**

DrawText. Instead of implementing DrawText one can also use the default TWidgetSet.DrawText, like the Carbon and Cocoa widgetsets do. But in this case it is necessary that one implements at least GetTextMetrics, GetTextExtentPoint and ExtTextOut. Without GetTextMetrics a form with a label will crash because the autosize will not be able to calculate the appropriate size for the label. 

**Region functions to determine if the control is behind another**

CombineRgn, CreateRectRgn, GetClipRGN, RectVisible 

  
Below is the order in which paint procedures are called on a form with only one TLabel, to better understand the painting sequence: 

1 - GetDC is called once on software startup with hWnd = 0 

2 - The form is shown 

3 - GetDC is called again (this wouldn't happen without the label). A few font related functions are called, as well as DrawText with CalcRect set to True to calculate the size of the label. 

4 - InvalidateRect is called on the form canvas 

5 - Control goes back to the operating system until a paint message comes from the widgetset 

6 - BeginPaint is called, and at this point code on OnPaint event of the form will be executed 

7 - DrawText is called again with CalcRect set to false 

8 - The Painting ends. 

### Implementing visibility for forms and controls and window state

The code that controls visibility is split between visibility for forms, and for controls 

**Visibility for forms and window state**

This part also controls the state of the window (minimized, maximized or normal). It is implemented as a copy of the Windows API function ShowWindow, so you must implemente the TMyWidgetset.ShowWindow on the file mywinapi.inc Don´t forget to also add a header to the file mywinapih.inc 

Below is code that implements this function on the Qt widgetset. It should be very easy to understand, copy and implement on your own widgetset. You can also take a look how Gtk implements this. On Windows, the Windows API is called directly, of course, so there is no code to look at. 
    
    
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
    

**Visibility for controls**

For controls inside a form you need to implement TMyWSWinControl.ShowHide class function that resides on the TMyWSWinControl class on the file mywscontrols.pp 

Remember that most controls are descendent from TWinControl, so implementing this function there will guarantee that the Visible property is implemented for all standard controls that have it. Below is a sample code for Qt widgetset. 
    
    
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
    

### Implementing TStrings based Components

Some components use a TStrings to store the information they display, such as: TCustomMemo, TCustomListBox and TCustomComboBox. 

To implement those it is not enougth to only implement their functions on the TQtCustomMemo class for example. One of the functions to implement will be called GetStrings, which looks like this: 
    
    
    class function TQtWSCustomListBox.GetStrings(const ACustomListBox: TCustomListBox): TStrings;
    var
      ListWidgetH: QListWidgetH;
    begin
      ListWidgetH := QListWidgetH((TQtWidget(ACustomListBox.Handle).Widget));
      Result := TQtListStrings.Create(ListWidgetH, ACustomListBox);
    end;
    

This function must return a TStrings descendent designed to detect when strings are added to or removed from the string list and to send this information to the widgetset to update the control. For example, the following declaration shows what TQtListStrings looks like: 
    
    
      
    TQtListStrings = class(TStrings)
      private
        FListChanged: Boolean; // StringList and QtListWidget out of sync
        FStringList: TStringList; // Holds the items to show
        FQtListWidget: QListWidgetH;  // Qt Widget
        FOwner: TWinControl;      // Lazarus Control Owning ListStrings
        FUpdating: Boolean;       // We're changing Qt Widget
        procedure InternalUpdate;
        procedure ExternalUpdate(var Astr: TStringList; Clear: Boolean = True);
        procedure IsChanged; // OnChange triggered by program action
      protected
        function GetTextStr: string; override;
        function GetCount: integer; override;
        function Get(Index : Integer) : string; override;
        //procedure SetSorted(Val : boolean); virtual;
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
    

You can see its implementation in the qtobjects.pas unit of the qt interface 

### Implementing Menus

Menus are available on the LCL to create main menus or popup menus. A TMenu is the owner of a larger menu structure with many items. Items can have subitems, and don't need extra TMenus. 

Also remember that on LCL the handle is only created when needed and at that time all properties of the controls are already initialized. This helps a lot on widgetsets where depending on the properties of a menu item it can be of one class or another, like Qt. 

The following things need to be implemented in order for the menus to work: 

1) All methods on the QtWSMenus unit, which will implement menu creation and modification 

2) function TWinCEWidgetSet.SetMenu(AWindowHandle: HWND; AMenuHandle: HMENU): Boolean; from the wincewinapi.inc file, which will implement support for a main menu associated with a window. 

#### Menu Creation Order

One important thing to understand when implementing menus, is in which order they are created. For example, we want to create the following menu structure: 

[![Menu creation order.png](https://wiki.freepascal.org/images/6/69/Menu_creation_order.png)](</File:Menu_creation_order.png>)

And when our application is executed, there will be a 'Creating MenuItem' message with the caption of the menu each time TQtWSMenuItem.CreateHandle is called, and a 'Creating Menu' message with the name of the menu (TMenu descendents don't have a caption), each time TQtWSMenu.CreateHandle is called. 

Here is the resulting output of such software: 
    
    
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
    

For all MenuItems one can use GetParentMenu to get their parent owner: Menu (TMainMenu). 

### Control enabling/disabling

The current way to set control enabling/disabling is by implementing the winapi EnableWindow. This API should work generically on any control. It should enable/disable mouse and keyboard input for the specified window or control, but also mark it as uneditable by the user, by making it greyed for example. 
    
    
    {------------------------------------------------------------------------------
      Method:  EnableWindow
      Params: HWnd    - handle to window
              BEnable -  whether to enable the window
      Returns: If the window was previously disabled
    
      Enables or disables mouse and keyboard input to the specified window or
      control.
     ------------------------------------------------------------------------------}
    function TWin32WidgetSet.EnableWindow(hWnd: HWND; bEnable: Boolean): Boolean;
    

### Return Control Text (Captions)

The method to return control text is GetText(). 

However, the method doesn't always have to return text (even if it can). It should only return text (by assigning AText value and setting result to TRUE), if a user can modify the text on the control itself (typically by typing it in by keyboard or some other manipulation). 

If the text itself is considered to be "static" then method can return FALSE at all times. 

LCL caches the text most of the time, and thus if control returns false, it would pull the cached value. 
    
    
    {------------------------------------------------------------------------------
      Function: TWSWinControl.GetText
      Params:  Sender: The control to retrieve the text from
      Returns: the requested text
    
      Retrieves the text from a control. 
     ------------------------------------------------------------------------------}
    class function TWSWinControl.GetText(const AWinControl: TWinControl; var AText: String): Boolean;
    

### Shaped Windows

Windows can be shaped based on a TBitmap or on a TRegion. The region is the visible part. 

To implement shaped windows based on a TBitmap, implement TWSWinControl.setShape 

To implement shaped windows based on a TRegion implement LCLIntf.SetWindowRgn 

See also: [LCL Tips#Creating a non-rectangular window or control](<LCL_Tips.md> "LCL Tips")

### System colors

Some color constants are actually system colors, like clBtnFace, clForm, clWindow, etc, etc. 

To implement support for system colors the WinAPI routine GetSysColor should be implemented: 
    
    
    function GetSysColor(nIndex: Integer): DWORD; override;
    

And here is a snip of the color constants which should be supported. Check LCLType for the latest values: 
    
    
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

These standard dialogs are implemented purely in the LCL in the following places: 

  * Class TPromptDialog file lcl/include/promptdialog.inc



### SpinEdit

Both TFloatSpinEdit and TSpinEdit are implemented in the class TWSFloatSpinEdit. 

### Clipboard

Clipboard support is implemented in lclintf by implementing Windows API routines. The routines are: 
    
    
    function ClipboardFormatToMimeType(FormatID: TClipboardFormat): string; {$IFDEF IF_BASE_MEMBER}virtual;{$ENDIF}
    function ClipboardGetData(ClipboardType: TClipboardType;
      FormatID: TClipboardFormat; Stream: TStream): boolean; {$IFDEF IF_BASE_MEMBER}virtual;{$ENDIF}
    // ! ClipboardGetFormats: List will be created. You must free it yourself with FreeMem(List) !
    function ClipboardGetFormats(ClipboardType: TClipboardType;
      var Count: integer; var List: PClipboardFormat): boolean;  {$IFDEF IF_BASE_MEMBER}virtual;{$ENDIF}
    function ClipboardGetOwnerShip(ClipboardType: TClipboardType;
      OnRequestProc: TClipboardRequestEvent;  FormatCount: integer;
      Formats: PClipboardFormat): boolean; {$IFDEF IF_BASE_MEMBER}virtual;{$ENDIF}
    function ClipboardRegisterFormat(const AMimeType: string): TClipboardFormat; {$IFDEF IF_BASE_MEMBER}virtual;{$ENDIF}
    

Sequence of calls during program startup: 
    
    
    TCocoaWidgetSet.ClipboardRegisterFormat AMimeType=image/bmp Result=51027040
    TCocoaWidgetSet.ClipboardRegisterFormat AMimeType=image/delphi.bitmap Result=0
    TCocoaWidgetSet.ClipboardRegisterFormat AMimeType=image/xpm Result=0
    TCocoaWidgetSet.ClipboardRegisterFormat AMimeType=image/png Result=0
    TCocoaWidgetSet.ClipboardRegisterFormat AMimeType=image/jpeg Result=0
    

Sequence of calls during "Caption := Clipboard.AsText": 
    
    
    TCocoaWidgetSet.ClipboardRegisterFormat AMimeType=text/plain Result=51026976
    TCocoaWidgetSet.ClipboardGetData ClipboardType=clipboard FormatID: 51026976
    

### TSpeedButton

This control is a TGraphicControl descendent, so it has no handle and paints itself. To retain native look, it uses the function ThemeServices.DrawElement(Details.Element=teButton) to paint itself, so implement this function to have this control working. Naturally, lot's of other DC, WindowOrg, Text Drawing and Painting routines will have to be working, as explained for TLabel. 

### TRadioButton and TCheckButton

**Messages:**

Basically the Widgetset must do: 

  * Send LM_CHANGE when a radio button is unchecked/checked (LCL will take care if OnClick will be called or not)
  * Dont send LM_CHANGE when TWSCustomCheckBox.SetState is called (SetChecked)



See also this bug report: <http://bugs.freepascal.org/view.php?id=13939>

### FullScreen support

FullScreen is implemented as a Windows state wsFullScreen, and as such is implemented in LCLIntf.ShowWindow where the constant SW_SHOWFULLSCREEN should be handled. 

## An example of how the LCL interfaces work

Below is a simple example. Suppose you have developed a trayicon component. How would you implement it in the LCL to work correctly on the various different supported platforms? 

You would need to generate the following files: 

\trayicon.pas 

\wstrayicon.pas 

\gtk\gtkwstrayicon.pas 

\gtk\trayintf.pas 

\win32\win32wstrayicon.pas 

\win32\trayintf.pas 

  
Providing separate wsXXX.pas and \$(LCLWidgetType)\XXXintf.pas files avoids the need for ifdefs altogether. You will need to add as a unit path $(LCLWidgetType) to the XXXintf.pas file in order initialize the correct trayintf.pas file, which in turn initializes the correct WS Tray class. 

Inside trayicon.pas you include wstrayicon. Derive your main class from an LCL class, and only use wstrayicon in its implementation. All LCL classes that communicate with a widgetset, must be derived from [TLCLComponent](<http://lazarus-ccr.sourceforge.net/docs//lcl/lclclasses/tlclcomponent.html> "doc:/lcl/lclclasses/tlclcomponent.html") declared in the [LCLClasses](<http://lazarus-ccr.sourceforge.net/docs//lcl/lclclasses> "doc:/lcl/lclclasses") unit. 
    
    
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
      // Call wstrayicon
    end;
    
    end.
    

in trayintf you use gtkwstrayicon or win32trayicon depending on which trayintf file it is. 

in wstrayicon you create a class like so: 
    
    
    unit WSTrayIcon;
    
    uses WSLCLClasses, Controls, TrayIcon; // and other things as well
    
    TWSTrayIconClass = class of TWSTrayIcon;
    TWSTrayIcon = class(TWSWinControl);
    public
     class procedure EmbedTrayIcon(const ATrayIcon: TCustomTrayIcon);
    virtual; // these must all be virtual and class procedures!!
     class procedure RemoveTrayIcon(const ATrayIcon: TCustomTrayIcon); virtual;
     ....
    end;
    ...
    
    implementation
    
    procedure TWSTrayIcon.EmbedTrayIcon(const ATrayIcon: TCustomTrayIcon);
    begin
     //do nothing
    end;
    
    procedure TWSTrayIcon.RemoveTrayIcon(const ATrayIcon: TCustomTrayIcon);
    begin
     //do nothing
    end;
    

now in gtkwstrayicon.pas do this: 
    
    
    uses WSTrayIcon, WSLCLClasses, Controls, TrayIcon, gtk, gdk;
    
    TGtkWSTrayIcon = class(TWSTrayIcon);
    private
     class function FindSystemTray(const ATrayIcon: TCustomTrayIcon):
    TWindow; virtual;
    public
     class procedure EmbedTrayIcon(const ATrayIcon: TCustomTrayIcon); override;
     class procedure RemoveTrayIcon(const ATrayIcon: TCustomTrayIcon);
    override;
     class function  CreateHandle(const AWinControl: TWinControl; const
    AParams: TCreateParams): HWND; override;
     ....
    end;
    ...
    
    implementation
    
    procedure TGtkWSTrayIcon.CreateHandle(const AWinControl: TWinControl;
    const AParams: TCreateParams): HWND;
    var
    WidgetInfo: PWidgetInfo;
    begin
    
     Result := gtk_plug_new;
     WidgetInfo := CreateWidgetInfo(AWinControl, Result); // it's something
    like this anyway
     TGtkWSWincontrolClass(WidgetSetClass).SetCallbacks(AWinControl);
     // and more stuff
    end;
    
    function TGtkWSTrayIcon.FindSystemTray(const ATrayIcon:
    TCustomTrayIcon): TWindow;
    begin
     // do something
    end;
    
    
    procedure TGtkWSTrayIcon.EmbedTrayIcon(const ATrayIcon: TCustomTrayIcon);
    var
    SystemTray: TWindow;
    begin
     SystemTray := FindSystemTray(ATrayIcon);
     //do something
    end;
    
    procedure TGtkWSTrayIcon.RemoveTrayIcon(const ATrayIcon: TCustomTrayIcon);
    begin
     //do something
    end;
    
    ......
    
    initialization
    
    RegisterWSComponent(TCustomTrayIcon, TGtkWSTrayIcon); //this is very
    important!!!
    
    end.
    

then finally in trayicon.pas you go as normal 
    
    
    uses WSTrayIcon; //etc. you DON'T include GtkWSTrayIcon here!
    
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
    

  


## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<Carbon_Interface.md> "Carbon Interface") \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
  * [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa 64 bit interface for macOS
  * [Qt Interface](<Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [Qt5 Interface](<Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [GTK1 Interface](<GTK1_Interface.md> "GTK1 Interface") \- The gtk1 interface for Unixes, macOS (X11), Windows
  * [GTK2 Interface](<GTK2_Interface.md> "GTK2 Interface") \- The gtk2 interface for Unixes, macOS (X11), Windows
  * [GTK3 Interface](<GTK3_Interface.md> "GTK3 Interface") \- The gtk3 interface for Unixes, macOS (X11), Windows
  * [fpGUI Interface](<fpGUI_Interface.md> "fpGUI Interface") \- Based on the fpGUI library, which is a cross-platform toolkit completely written in Object Pascal
  * [Custom Drawn Interface](<Custom_Drawn_Interface.md> "Custom Drawn Interface") \- A cross-platform LCL backend written completely in Object Pascal inside Lazarus. The Lazarus interface to Android.



### Platform specific Tips

  * [Android Programming](<Android_Programming.md> "Android Programming") \- For Android smartphones and tablets
  * [iPhone/iPod development](<iPhone/iPod_development.md> "iPhone/iPod development") \- About using Objective Pascal to develop iOS applications
  * [FreeBSD Programming Tips](<FreeBSD_Programming_Tips.md> "FreeBSD Programming Tips") \- FreeBSD programming tips
  * [Linux Programming Tips](<Linux_Programming_Tips.md> "Linux Programming Tips") \- How to execute particular programming tasks in Linux
  * [macOS Programming Tips](<macOS_Programming_Tips.md> "macOS Programming Tips") \- Lazarus tips, useful tools, Unix commands, and more...
  * [WinCE Programming Tips](<WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Using the telephone API, sending SMSes, and more...
  * [Windows Programming Tips](<Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips



### Interface Development Articles

  * [Carbon interface internals](<Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * LCL Internals \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset



## Minimum Toolkit versions

Lazarus version | Min. FPC | Min. Gtk 2 | Min. Qt 4 | Min. Windows | Min. Windows CE | Min. macOS (Carbon) | Min. macOS (Cocoa) | Min. Req. of LCL-CustomDrawn   
---|---|---|---|---|---|---|---|---  
0.9.24  | 2.2.0  | 2.6+  | 4.2+  | Windows 98+  | Recommended 4.0+  | 10.4  | N/A  | N/A   
0.9.26  | 2.2.2  | 2.6+  | 4.3+  | Windows 98+  | Recommended 4.0+  | 10.4  | N/A  | N/A   
0.9.28  | 2.2.4  | 2.8+  | 4.5+  | Windows 98+  | Recommended 4.0+  | 10.4  | N/A  | N/A   
0.9.30  | 2.4.0  | 2.8+  | 4.5+  | Windows 98+  | Recommended 5.0+  | 10.4  | N/A  | N/A   
0.9.31  | 2.4.4  | 2.8+  | 4.5+  | Windows 98+  | Recommended 5.0+  | 10.4  | 10.6  | Android 2.2+, Windows 2000+, X11, Snow Leopard 10.6+   
1.2.6  | 2.6.4  | 2.8+  | 4.5+*  | Windows 98+  | Recommended 5.0+  | 10.4  | 10.6  | Android 2.2+, Windows 2000+, X11, Snow Leopard 10.6+   
  
  * \- Actually 4.5.0 ...4.8.4 with pre-built Qt4Pas.dll, 4.8.5+ requires a different Qt4Pas.dll



See Also: [FPC + Lazarus Compatibility Matrix](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS").

---

_Source: [https://wiki.freepascal.org/LCL_Internals](https://web.archive.org/web/20250124125155/https://wiki.freepascal.org/LCL_Internals)_
