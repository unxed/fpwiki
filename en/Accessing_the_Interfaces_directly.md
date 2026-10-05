# Accessing the Interfaces directly

│ **English (en)** │

This page describes how to write access the underlying [widgetset](<Widgetset.md> "Widgetset") used by lazarus directly (for example: The Windows API, or Gtk, or Qt, etc). One should always try to avoid relying on widgetset specials. This document is here for both people improving the widgetsets and for people writing components that really need to access the widgets directly. 

## Contents

  * 1 Overview
  * 2 Per-Widgetset Details
    * 2.1 Qt
    * 2.2 Carbon
    * 2.3 Windows
  * 3 List of Handles on various LCL classes
    * 3.1 TFont
    * 3.2 TCanvas
    * 3.3 TBrush
    * 3.4 TBitmap
    * 3.5 TCustomForm
    * 3.6 TMenu, TMainMenu, TMenuItem and TPopUpMenu
  * 4 See also



## Overview

Every TWinControl has a handle, and the LCL does not need to know, what a Handle is. The meaning of the Handle is totally up to the interface: 

  * under [GTK](</Category:GTK_Widgetsets> "Category:GTK Widgetsets") a handle is often a PGtkWidget
  * under Windows a handle is often a HWnd.
  * under Carbon a handle is often a object descendant of TCarbonWidget.
  * under Qt, a handle is often a pointer to a object descendent of TQtWidget.



## Per-Widgetset Details

### Qt

Handles on [Qt](<Qt_Interface.md> "Qt Interface") are mostly pointers to objects. Every time a control is created, a helper object is also created and both are connected. The helper object is necessary to receive events from Qt. After receiving an event it will send it to the LCL. 

Most of those objects are descendents from TQtWidget as declared below: 
    
    
    { TQtWidget }
    
    TQtWidget = class(TObject)
    private
      ...
    public
      Widget: QWidgetH;
      LCLObject: TWinControl;
    public
      function  GetContainerWidget: QWidgetH; virtual;
      ...
    end;
    

Notice the two variables they all contain: 

  * Widget - This is a connect to the Qt object for this Widget
  * LCLObject - Is a bridge with the LCL object it is linked to.



And also this method: 

  * GetContrainerWidget() - When you wish to create a new widget and set it's parent, don't set it to Widget. Set it to GetContrainerWidget() instead. This happens because forms have 2 widgets. The main form widget, and also a container widget, where all child controls should be put to ensure that the size of the menu is handled automatically. Most other controls will have just one widget, but by using this function you avoid surprises in case the implementation of a control changes.



### Carbon

Handles on [Carbon](<Carbon_Interface.md> "Carbon Interface") are very similar to Qt ones. The helper classes are descended from abstract class TCarbonWidget. 
    
    
    { TCarbonWidget }
    
    TCarbonWidget = class(TObject)
    private
    public
      Widget: Pointer;
      LCLObject: TWinControl;
    end;
    

Notice the two variables they all contain: 

  * Widget - This is a connect to the Carbon object for this Widget: ControlRef (TCarbonControl descendants) or WindowRef (TCarbonWindow descendants).
  * LCLObject - Is a bridge with the LCL object it is linked to.



### Windows

Handles on [Win32](<Win32/64_Interface.md> "Win32/64 Interface") or [WinCE](<Windows_CE_Interface.md> "Windows CE Interface") are almost always the same as their corresponding Win api handles. For example the handle for a TMenu is an HMENU, the handle for a TBrush is an HBRUSH. You simply pass one such handle as a parameter in a Windows api call. 

## List of Handles on various LCL classes

### TFont

  * Win32, WinCE: HFONT
  * Qt: A TQtFont object from qtprivate.pas
  * Carbon: A TCarbonFont object from carbongdiobjects.pp



### TCanvas

  * Win32, WinCE: HDC
  * Gtk: A TDeviceContext object from gtkdef.pp 
    * You can get a drawable, for example with: TDeviceContext(MyCanvas.Handle).Drawable
  * Qt: A TQtDeviceContext object from qtprivate.pas
  * Carbon: A TCarbonDeviceContext object from carboncanvas.pp 
    * The native handle can be accessed with TCarbonDeviceContext(MyCanvas.Handle).CGContext of type CGContextRef



### TBrush

  * Win32, WinCE: HBRUSH
  * Qt: A TQtBrush object from qtprivate.pas
  * Carbon: A TCarbonBrush object from carbongdiobjects.pp



### TBitmap

  * Win32, WinCE: HBITMAP
  * Gtk: PGDIObject (defined on unit GtkDef)
  * Qt: A TQtImage object from qtobjects.pas unit
  * Carbon: A TCarbonBitmap object from carbongdiobjects.pp



You can access this on Gtk using: 
    
    
    var
      GDIObject: PGDIObject;
      Bitmap: TBitmap;
      AImage: PGtkWidget;
    begin
      ...
    
      GDIObject := PgdiObject(Bitmap.Handle);
    
      AImage := gtk_image_new_from_pixmap(GDIObject^.GDIPixmapObject,
       GDIObject^.GDIBitmapMaskObject);
    
      gtk_widget_show(AImage);
    
      gtk_container_add(GTK_CONTAINER(MyForm.Handle), AImage);
    end;
    

### TCustomForm

  * Win32, WinCE: it´s a HWND, a native window handle
  * Gtk: It´s a pointer to a structure.
  * Qt: The Handle is a pointer to a object from the TQtMainWindow class declared at qtwidgets.pas. 
    * To have access to the QMainWindow Qt object it represents do: TQtMainWindow(Handle).Widget
    * To have access to the container widget of the form, do: TQtMainWindow(Handle).GetContrainerWidget
  * Carbon: TCarbonWindow from carbonprivate.pp



### TMenu, TMainMenu, TMenuItem and TPopUpMenu

  * Win32, WinCE : HMENU
  * Qt: TMainMenu is a pointer to a TQtMenuBar object. TPopUpMenu is a TQtMenu object. TMenuItem can either be a TQtAction if it represents a clickable item or a separator or a TQtMenu if it is an item that has submenus.
  * Carbon: TMenuItem is a TCarbonMenu object, TMenu (TMainMenu and TPopupMenu) is represented by its root menu item



## See also

  * [Interfaces](<Interfaces.md> "Interfaces")
  * [How To Use Interfaces](<How_To_Use_Interfaces.md> "How To Use Interfaces")
  * [Understanding Interfaces](<Understanding_Interfaces.md> "Understanding Interfaces")

---

_Source: [https://wiki.freepascal.org/Accessing_the_Interfaces_directly](https://web.archive.org/web/20230604013146/https://wiki.freepascal.org/Accessing_the_Interfaces_directly)_
