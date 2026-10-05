# Application full screen mode

│ **English (en)** │  **[français (fr)](</Application_full_screen_mode/fr> "Application full screen mode/fr")** │    
****

## Contents

  * 1 Introduction
  * 2 Standard way
  * 3 Universal way to use full-screen
  * 4 Win32 way to use full-screen
    * 4.1 Win32 example program
    * 4.2 Win32: get task bar size
  * 5 GTK way to use full-screen
  * 6 Changing screen resolution
  * 7 See also
  * 8 External links



## Introduction

Some applications like some web browsers allow show in full screen mode using key `F11`. In full screen mode window hasn't border and uses entire screen space. 

## Standard way

You can switch form to full screen mode simply by assigning **wsFullScreen** value to **TForm.WindowState** property. You can do that directly in design time in Object Inspector window or in runtime: 
    
    
    procedure TForm1.FormShow(Sender: TObject);
    begin
      WindowState := wsFullScreen;
    end;
    

## Universal way to use full-screen

Universal way exists, using LCL, you need to call ShowWindow() with SW_SHOWFULLSCREEN. This ShowWindow call is handled inside widgetset, using low-level API (for example, OS X Carbon API, or GTK2 API). Example: 
    
    
    uses
      ..., LCLIntf, LCLType;
    
    procedure TfmMain.SetFullScreen_Universal(AValue: boolean);
    begin
      if AValue then
        ShowWindow(Handle, SW_SHOWFULLSCREEN)
      else
        ShowWindow(Handle, SW_SHOWNORMAL); 
        //maybe consider here previous maximized state?
    end;
    

## Win32 way to use full-screen

Universal way, above, works for Win32, but window border still stays, so better code which works: 
    
    
    procedure TfmMain.SetFullScreen_Win32(AValue: boolean);
    begin
      if AValue then
      begin
        FOrigWndState:= WindowState;
        FOrigBounds:= BoundsRect;
    
        BorderStyle:= bsNone;
        BoundsRect:= Monitor.BoundsRect;
      end
      else
      begin
        WindowState:= FOrigWndState;
        BoundsRect:= FOrigBounds;
        BorderStyle:= bsSizeable;
        BoundsRect:= FOrigBounds; //again
      end;
    end;
    

### Win32 example program

This is example for Win32 only, since it don't use native full-screen mode on Linux/MacOS, it is here from old times. 

To be able to switch to full screen mode you will need: 

  * Remember previous form state as position and size and window state
  * Be able to determine screen size
  * Capture some key and perform operation



Best way to capture desired key is using Actions. Simply define new action in action manager and assign key shortcut. Than shortcut will work even if main form would lost focus. 
    
    
    unit Unit1; 
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
      Menus;
    
    type
      TForm1 = class(TForm)
        procedure FormKeyDown(Sender: TObject; var Key: Word; Shift: TShiftState);
      private
      public
        OriginalBounds: TRect;
        OriginalWindowState: TWindowState;
        ScreenBounds: TRect;
        procedure SwitchFullScreen;
      end; 
    
    var
      Form1: TForm1; 
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.FormKeyDown(Sender: TObject; var Key: Word; Shift: TShiftState);
    begin
      if Key = VK_F11 then
      begin
        SwitchFullScreen;
        Key := 0;
      end;
    end;
    
    procedure TForm1.SwitchFullScreen;
    begin
      if BorderStyle <> bsNone then begin
        // To full screen
        OriginalWindowState := WindowState;
        OriginalBounds := BoundsRect;
    
        BorderStyle := bsNone;
        BoundsRect := Screen.MonitorFromWindow(Handle).BoundsRect;
      end else begin
        // From full screen
        BorderStyle := bsSizeable;
        if OriginalWindowState = wsMaximized then
          WindowState := wsMaximized
        else
          BoundsRect := OriginalBounds;
      end;
    end;
    
    end.
    

### Win32: get task bar size

If you want to have task bar visible you have to adjust size according task bar position and size. For Windows: 
    
    
    function GetTaskBarSize: TRect;
    begin
      SystemParametersInfo(SPI_GETWORKAREA, 0, @Result, 0);
    end;
    

## GTK way to use full-screen

This is low-level code for GTK2, it is used already by LCL in "universal way" above. 

To switch form to full screen dimensions: 
    
    
    gdk_window_fullscreen(PGtkWidget(Handle)^.window);
    

To roll back to normal mode: 
    
    
    gdk_window_unfullscreen(PGtkWidget(Handle)^.window);
    

This is GTK2 only. You must add to uses section such modules as gtk2, gdk2, glib2. You can do it by the directive 
    
    
    {$IFDEF LCLGTK2}, gtk2, gdk2, glib2{$ENDIF}
    

## Changing screen resolution

There is no OS independent way to switch screen resolution now. 

For Windows you can find information in article for Delphi [Get and Set Screen Resolution (Display Device Modes)](<http://delphi.about.com/od/graphics/a/change_display.htm>)

## See also

  * [WinCE Programming Tips - Making a Fullscreen Application](<WinCE_Programming_Tips.md> "WinCE Programming Tips")



## External links

  * [Run Your Delphi Application in Full Screen - Implement "F11 - Full Screen"](<http://delphi.about.com/od/delphitips2010/qt/delphi-application-full-screen-mode-f11.htm>)
  * [Very simple way to make your form Fullscreen & Restore (LazPlanet)](<http://lazplanet.blogspot.com/2014/02/make-your-form-fullscreen.html>)

---

_Source: [https://wiki.freepascal.org/Application_full_screen_mode](https://web.archive.org/web/20250401005741/https://wiki.freepascal.org/Application_full_screen_mode)_
