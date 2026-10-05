# Print Bitmap

│ **English (en)** │  **[русский (ru)](<../ru/Print_Bitmap.md>)** │


## How to send an image to the printer
    
    
    // uses Printers;
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

---

_Source: [https://wiki.freepascal.org/Print_Bitmap](https://web.archive.org/web/20190514230737/https://wiki.freepascal.org/Print_Bitmap)_
