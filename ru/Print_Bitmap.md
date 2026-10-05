# Print Bitmap

│ **[English (en)](<../en/Print_Bitmap.md>)** │  **русский (ru)** │

## Как отправить изображение на принтер
    
    
    // uses Printers; // необходимо подключить модуль
    var
      Scale :LongInt;
    begin
      with Printer do
        begin
          BeginDoc;
          Scale := Min(
            Printer.PageWidth div Image1.Picture.Bitmap.Width,
            Printer.PageHeight div Image1.Picture.Bitmap.Height);
    
          Printer.Canvas.StretchDraw(
            Rect(0, 0, Image1.Picture.Bitmap.Width*Scale, Image1.Picture.Bitmap.Height*Scale),
            Image1.Picture.Bitmap);
    
          EndDoc;
        end;
    end;
    

  


**Печать jpeg картинки с масштабированием через Bitmap на лист А4**
    
    
    // uses Printers; // необходимо подключить модуль
    procedure PrintImage(Image: TImage);
    var
      TempImageBMP: Graphics.TBitmap;
      TempImageJPEG: TImage;
      ScaleX, ScaleY: Double;
      RectLeft,RectTop,RectRight,RectBottom: Integer;
    begin
    
    TempImageBMP := Graphics.TBitmap.Create;
    TempImageBMP.Assign(Image.Picture.Graphic);
    TempImageJPEG := TImage.Create(Self);
    TempImageJPEG.Picture.Bitmap := TempImageBMP;
    
    Printer.BeginDoc;
    // Расчет соотношение сторон
    ScaleX := Printer.PageWidth / TempImageJPEG.Picture.Bitmap.Width;
    ScaleY := Printer.PageHeight / TempImageJPEG.Picture.Bitmap.Height;
    
    RectLeft:=0;
    RectTop:=0;
    RectRight := trunc(TempImageJPEG.Picture.Bitmap.Width * scaleX);
    RectBottom := trunc(TempImageJPEG.Picture.Bitmap.Height * scaleY);
    Printer.Canvas.StretchDraw(
            Rect(RectLeft,RectTop,RectRight,RectBottom),
            TempImageJPEG.Picture.Bitmap);
    Printer.EndDoc;
    
    TempImageBMP.Free;
    TempImageJPEG.Free;
    end;

---

_Source: [https://wiki.freepascal.org/Print_Bitmap/ru](https://web.archive.org/web/20250122171514/https://wiki.freepascal.org/Print_Bitmap/ru)_
