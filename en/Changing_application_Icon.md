# Changing application Icon

unit main;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
      ExtCtrls;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Image1: TImage;
        ImageList1: TImageList;
        Timer1: TTimer;
        procedure FormShow(Sender: TObject);
        procedure Timer1Timer(Sender: TObject);
      private
        FIndex: Integer;
        procedure ChangeIcon(const AIndex: Integer);
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.ChangeIcon(const AIndex: Integer);
    begin
      ImageList1.GetBitmap(AIndex, Image1.Picture.BitMap);
      Application.Icon.Assign(Image1.Picture.Graphic);
    end;
    
    procedure TForm1.FormShow(Sender: TObject);
    begin
      FIndex := -1;
      Timer1.Enabled := True;
    end;
    
    procedure TForm1.Timer1Timer(Sender: TObject);
    begin
      Inc(FIndex);
      ChangeIcon(FIndex);
      if FIndex = ImageList1.Count - 1 then
        FIndex := -1;
    end;
    
    end.

---

_Source: [https://wiki.freepascal.org/Changing_application_Icon](https://web.archive.org/web/20210418234537/https://wiki.freepascal.org/Changing_application_Icon)_
