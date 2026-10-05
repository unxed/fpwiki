# Cocoa DPI

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Cocoa DPI
    * 1.1 High-DPI on Cocoa (Retina)
    * 1.2 Remedy for the difference between LCL-Win32 High-DPI and LCL-Cocoa Retina
    * 1.3 Possible remedies for the difference between LCL 96 DPI and native Cocoa 72 DPI
      * 1.3.1 For a specific control
      * 1.3.2 For the whole application
    * 1.4 Why LCL-Cocoa uses GetCanvasScaleFactor for High-DPI instead of DPI/PixelsPerInch
  * 2 See Also



# Cocoa DPI

The native OS DPI for Cocoa is 72. The LCL-Cocoa binding uses the value 96 instead. This is because other widget sets (Windows, Gtk2, Qt) use 96 DPI for 100% scaling as well. As a result font sizes scale equally across all LCL widget sets including Cocoa and also the High-DPI system works correctly. 

On the other hand, there is the disadvantage that fonts of the same size in a LCL application appear bigger by the ratio 96/72=4/3 than in a native Cocoa application. 

## High-DPI on Cocoa (Retina)

High-DPI on LCL-Cocoa is achieved with TControl.GetCanvasScaleFactor ([backingScaleFactor](<https://developer.apple.com/documentation/appkit/nswindow/1419459-backingscalefactor>)) function result, unlike on LCL-win32 where the DPI/PixelsPerInch value is increased and TControl.GetCanvasScaleFactor is 1. On Retina, TControl.GetCanvasScaleFactor returns 2. 

If a cross-platform application should support pixel-precise drawing on Windows&Mac, it has to support the TControl.GetCanvasScaleFactor function result. All bitmap images must be drawn to a control's canvas with StretchDraw() and they need to be in TControl.GetCanvasScaleFactor-times higher resolution (for Retina a 2-times higher resolution) than the target rect in TCanvas. See below for sample code. 

The TImageList component can be taken as an example of a component that fully supports High-DPI on all OS/WS in the LCL, including Cocoa+Retina. 

## Remedy for the difference between LCL-Win32 High-DPI and LCL-Cocoa Retina

As described above, High-DPI is achieved with the increase of TControl.GetCanvasScaleFactor instead of the increase of TFont.PixelsPerInch. 

As a result, a (physical) 1px line cannot be painted directly on Cocoa control's canvas. A 1px (LCL) line will appear as 2px (physical Retina). 

In order to draw a 1px line (or any other pixel-precise graphics) on physical Retina display, you have to create a TBitmap with TControl.GetCanvasScaleFactor-times higher size and then paint this bitmap on the control's canvas with StretchDraw(). 

Here is a sample code: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      B: TBitmap;
      F: Double;
    begin
      F := Self.GetCanvasScaleFactor;
      B := TBitmap.Create;
      try
        B.SetSize(Round(100*F), Round(100*F));
        B.Canvas.Brush.Color := clWhite;
        B.Canvas.FillRect(B.Canvas.ClipRect);
        B.Canvas.Font.PixelsPerInch := Round(Self.PixelsPerInch*F);
        B.Canvas.Font.Size := 10;
        // now do your drawing
        B.Canvas.Pen.Color := clRed;
        B.Canvas.MoveTo(0, 0);
        B.Canvas.LineTo(B.Width, B.Height);
        B.Canvas.Font.Color := clBlue;
        B.Canvas.TextOut(Round(10*F), Round(10*F), 'Hello');
        Self.Canvas.StretchDraw(Rect(0, 0, 100, 100), B);
      finally
        B.Free;
      end;
    end;
    

## Possible remedies for the difference between LCL 96 DPI and native Cocoa 72 DPI

### For a specific control

If you want to use the OS native font sizes for some specific control (e.g. TMemo with editing text where the user is able to select the font size from a combo box and you want the TMemo text size be the same height as in a native Cocoa application - e.g. TextView), just set the Font.PixelsPerInch accordingly: 
    
    
    Memo1.Font.PixelsPerInch := 72;
    

### For the whole application

If you want your whole application to appear with 72 DPI and you are fine with the disadvantage of different font size from other LCL widgetsets you can set the value of CocoaBasePPI variable to 72 in the initialization of your application. (It is a variable in CocoaInt.pas.) 

In this case fonts with custom sizes (Font.Size<>0) will have the same height like native Cocoa applications but in the same LCL application they will appear smaller on Cocoa than on Windows/Linux. 

If you write a cross-platform application and you are fine with the above difference and you want to use the High-DPI scaling system of the LCL, you have to follow these 2 steps in addition to setting the CocoaBasePPI value as described above: 

  1. Design your forms always at 96 DPI. Be sure your Lazarus IDE runs internally at 96 DPI as well = do not change the CocoaBasePPI value directly by editing CocoaInt.pas.



(It is recommended to design your forms at 96 DPI anyway to avoid DPI modifications in LFMs for your VCS.) 

  1. Set "Application.Scaled := False;" on Mac. This disables the High-DPI scaling on Mac, thus 72 PPI is used everywhere and controls don't get scaled down. Keep it enabled for other OS/WS to support scaling there. You will need to use compiler conditions in your application's LPR file:


    
    
    {$IFNDEF LCLCocoa}
      Application.Scaled := True;
    {$ENDIF}
    

## Why LCL-Cocoa uses GetCanvasScaleFactor for High-DPI instead of DPI/PixelsPerInch

Principally it is possible to emulate LCL-Win32 High-DPI with PixelsPerInch on LCL-Cocoa. As a result, the GetCanvasScaleFactor will be unnecessary (it will be 1 on LCL-Cocoa) and Font.PixelsPerInch will scale according to the physical scaling. 

In this case all TControl and TCanvas coordinates must be scaled between the LCL and native Cocoa. This is possible because Cocoa uses floating-point coordinates. 

The biggest problem and the show-stopper is when more displays are used that have different PPI settings. Because native Cocoa keeps the PPI value constant and changes the backingScaleFactor between the monitors, a form doesn't need to be rescaled when moved from one monitor to another. This is different from Windows where the form keeps the physical resolution between monitors and it has to be rescaled by the LCL when moved to another monitor. 

If LCL-Cocoa emulated LCL-win32 behavior, the LCL coordinates (Left/Top/Width/Height) of the form and all its controls would get invalid and had to be reassigned when the form is moved to a different monitor. That would result in a lot of Resize/Paint calls and probably also result in the form flashing/blinking until the correct coordinates are all sorted. 

The current solution (change of GetCanvasScaleFactor value) does not suffer from the above problem. 

# See Also

  * <https://www.qt.io/blog/2013/04/25/retina-display-support-for-mac-os-ios-and-x11>

---

_Source: [https://wiki.freepascal.org/Cocoa_DPI](https://web.archive.org/web/20250114104752/https://wiki.freepascal.org/Cocoa_DPI)_
