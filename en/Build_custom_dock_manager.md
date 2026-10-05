# Build custom dock manager

│ **English (en)** │  **[русский (ru)](<../ru/Build_custom_dock_manager.md>)** │

## Contents

  * 1 Introduction
  * 2 Possible docking features
  * 3 Dragging
  * 4 Docking subsystem
    * 4.1 TControl support
    * 4.2 TWinControl support
  * 5 Implementing basic custom DockManager
  * 6 See also



## Introduction

VCL and LCL support basic docking of controls. For advanced docking behavior, a custom dock manager has to be created. A basic abstract dock manager class TDockManager is placed in Controls unit. All custom managers have to be derived from this class. A basic LCL implementation of TDockManager is TDockTree which is capable of handling tree of dock zones. 

Recommended reading: [LCL Drag Dock](<LCL_Drag_Dock.md> "LCL Drag Dock")

## Possible docking features

  * Docking zone hierarchy 
    * One item
    * Linear list
    * Tree
    * Table
    * Anchored layout
    * Another complex organization
  * Tabbed docking
  * Docking multiple forms to floating conjoined window
  * Themes
  * Persistence, store/load docking layout
  * Tabbed docking with autoshow/autohide forms, pin button to switch autohide/visible
  * Custom header buttons
  * Building default layout directly from code, manual management
  * Supporting visual design-time components, docking customize form
  * Class for global docking operations



## Dragging

Docking is a basically special case of Drag & Drop action for forms and similar controls. Therefore basic events of controls are similar. 

Dragging events: OnDragOver, OnDragDrop, OnStartDrag, OnEndDrag Docking events: OnDockOver, OnDockDrop, OnStartDock, OnEndDock, OnUnDock 

The dragged object is represented by the TDragObject class. This class is further extended to TDragControlObject; this is then extended to TDragDockObject to support docking features. The whole dragging process is controlled by TDragManager, similarly to TDockManager controlling the docking process. 

## Docking subsystem

### TControl support
    
    
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
      property UndockHeight: Integer read GetUndockHeight write FUndockHeight; // Height used when undocked
      property UndockWidth: Integer read GetUndockWidth write FUndockWidth; // Width used when undocked
      ...
    end;
    

### TWinControl support
    
    
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
    

## Implementing basic custom DockManager

It is possible to derive our custom manager from default TDockTree. But for purposes of creating a completely custom manager, the parent class has to be TDockManager. All methods from the parent class should be overridden. 

  * For basic demonstration, the dock manager should be able to manage one control. That class can be extended later.



Here is a basic prototype based on TDockManager virtual methods. 
    
    
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
    

  * DockManager has to know to which TWinControl belong. So manager needs field FDockSite of type TWinControl which is assigned by constructor.


    
    
    private
      FDockSite: TWinControl;
    

  * Now we can implement a method PositionDockRect which determines dock frame size according to DockSite size.


    
    
    procedure TCustomDockManager.PositionDockRect(Client, DropCtl: TControl; DropAlign: TAlign;
      var DockRect: TRect); override; overload;
    begin
      DockRect := Rect(0, 0, FDockSite.ClientWidth, FDockSite.ClientHeight);
    end;
    

  * To make dock manager default for all dockable controls, insert an initialization section to the end of the unit. Then simple inclusion of the dock manager unit into main form unit will setup your docking system.


    
    
    initialization
      DefaultDockManagerClass := TCustomDockManager;
    

  * If dock site is resized, the ResetBounds method is executed. Then we can use it to position the docked control on dock site. We have to allocate some space on top of the control for the dock grabber.


    
    
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
    

  * Now we need to draw a grabber with control Name on top of the dock site.


    
    
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
    

For painting the dock site, the canvas responds to the PaintSite method, so we need to implement it. Only visible controls belonging to the dock site should be painted. 
    
    
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
    

  


_to be continued..._

## See also

  * [LCL Drag Dock](<LCL_Drag_Dock.md> "LCL Drag Dock")

---

_Source: [https://wiki.freepascal.org/Build_custom_dock_manager](https://web.archive.org/web/20250401051807/https://wiki.freepascal.org/Build_custom_dock_manager)_
