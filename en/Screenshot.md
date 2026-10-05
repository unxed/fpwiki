# Screenshot

These code examples create the screenshot in the TBitmap object. You can then save this bitmap to a file. 

## Example 1

This page has the cross-platform example: [Developing with Graphics#Taking a screenshot of the screen](<Developing_with_Graphics.md> "Developing with Graphics"). 

This takes the whole desktop area (all monitors) and puts it into a TBitmap. 

## Example 2

Code from forum user _Manlio_. It works on Windows. It gives blank-area bitmap on Linux GTK2, if you replace "GetDesktopWindow" to 0. 

With Monitor=-1 it takes the whole desktop area, otherwise it takes only specified monitor. 
    
    
    uses
      Graphics, LCLType, LCLIntf;
    
    procedure ScreenshotToFile(const Filename: string; Monitor: integer);
    var
      BMP: Graphics.TBitmap;
      ScreenDC: HDC;
      M: TMonitor;
      W, H, X0, Y0: integer;
    begin
      // Initialize coordinates of full composite area
      X0 := Screen.DesktopLeft;
      Y0 := Screen.DesktopTop;
      W  := Screen.DesktopWidth;
      H  := Screen.DesktopHeight;
      // Monitor=-1 takes entire screen, otherwise takes specified monitor
      if (Monitor >= 0) and (Monitor < Screen.MonitorCount) then begin
        M  := Screen.Monitors[Monitor];
        X0 := M.Left;
        Y0 := M.Top;
        W  := M.Width;
        H  := M.Height;
      end;
      // prepare the bitmap
      BMP := Graphics.TBitmap.Create;
      BMP.Width  := W;
      BMP.Height := H;
      BMP.Canvas.Brush.Color := clWhite;
      BMP.Canvas.FillRect(0, 0, W, H);
      ScreenDC := GetDC(GetDesktopWindow);
      // copy the required area:
      BitBlt(BMP.Canvas.Handle, 0, 0, W, H, ScreenDC, X0, Y0, SRCCOPY);
      ReleaseDC(0, ScreenDC);
      // save to file (possibly to TStream, etc.)
      BMP.SaveToFile(Filename);
      BMP.Free;
    end;

---

_Source: [https://wiki.freepascal.org/Screenshot](https://web.archive.org/web/20230322115957/https://wiki.freepascal.org/Screenshot)_
