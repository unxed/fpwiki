# ListBox with separators

unit Unit1;
    
    {$mode objfpc}{$H+}
    //http://forum.lazarus.freepascal.org/index.php/topic,39220.0.html
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls, Types;
    
    type
      TForm1 = class(TForm)
        procedure FormCreate(Sender: TObject);
    
      private
        ListBox1: TListBox;
        procedure ListBox1DrawItem(Control: TWinControl; Index: integer; ARect: TRect; State: TOwnerDrawState);
        procedure ListBox1MeasureItem(Control: TWinControl; Index: integer; var AHeight: integer);
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    uses
      LCLType;
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      ListBox1 := TListBox.Create(self);
      ListBox1.Align := alClient;
      ListBox1.Style := lbOwnerDrawVariable;
      ListBox1.OnDrawItem := @ListBox1DrawItem;
      ListBox1.OnMeasureItem := @ListBox1MeasureItem;
      ListBox1.Parent := self;
    
      with ListBox1.Items do
      begin
        Add('Paris');
        Add('Rome');
        Add('London');
        Add('Berlin');
        Add('-Casablanca');
        Add('Cairo');
        Add('Khartoum');
        Add('Pretoria');
        Add('-Tokyo');
        Add('Beijing');
        Add('Manila');
        Add('-New York');
        Add('Chicago');
        Add('Rio de Janeiro');
        Add('Lima');
        Add('-');
      end;
    end;
    
    const
      MARGINLine = 2;
      MARGINText = 12;
      MARGINvert = 2;
      HLine = 1;
    
    procedure TForm1.ListBox1DrawItem(Control: TWinControl; Index: integer; ARect: TRect; State: TOwnerDrawState);
    var
      lb: TListbox;
      s: string;
      hasLine: boolean;
      dy: integer;
    
    begin
      lb := Control as TListbox;
      s := lb.Items[Index];
      hasLine := (s <> '') and (s[1] = '-');
    
      if hasLine then
        dy := HLine
      else
        dy := 0;
    
      lb.Canvas.Brush.Style := bsSolid;
      lb.Canvas.Font.Assign(lb.Font);
    
      if odSelected in State then
      begin
    
        if hasLine then
        begin
          lb.Canvas.Brush.Color := lb.Color;
          lb.Canvas.FillRect(ARect.Left, ARect.Top, ARect.Right, ARect.Top + dy);
          Inc(ARect.Top, hLine + 2);
        end;
    
        if lb.Focused then
          lb.Canvas.Brush.Color := clHighlight
        else
          lb.Canvas.Brush.Color := clGray;
    
        lb.Canvas.Font.Color := clHighlightText;
    
      end
      else
      begin
        lb.Canvas.Brush.Color := lb.Color;
        lb.Canvas.Font.Color := clWindowText;
        dy := 0;
      end;
    
      lb.Canvas.FillRect(ARect);
    
      if hasLine then
      begin
        lb.Canvas.Pen.Style := psSolid;
        lb.Canvas.Pen.Color := clWindowText;
        lb.Canvas.Line(ARect.Left + MARGINLine, ARect.Top + hLine, ARect.Right - MARGINLine, ARect.Top + hLine);
        Delete(s, 1, 1);
      end;
    
      lb.Canvas.Brush.Style := bsClear;
      lb.Canvas.Pen.Color := clWindowText;
      lb.Canvas.Font.Color := clRed;
      lb.Canvas.TextOut(ARect.Left + MARGINText, (ARect.Top + dy + ARect.Bottom - lb.Canvas.TextHeight('Tg')) div 2, s);
    end;
    
    procedure TForm1.ListBox1MeasureItem(Control: TWinControl; Index: integer; var AHeight: integer);
    var
      lb: TListbox;
      h: integer;
      s: string;
    
    begin
      lb := Control as TListBox;
      lb.Canvas.Font.Assign(lb.Font);
      h := lb.Canvas.TextHeight('Tg') + MARGINvert * 2;
      s := lb.Items[Index];
    
      if (s <> '') and (s[1] = '-') then
        Inc(h, HLine);
      AHeight := h;
    end;
    
    end.

---

_Source: [https://wiki.freepascal.org/ListBox_with_separators](https://web.archive.org/web/20220704054521/https://wiki.freepascal.org/ListBox_with_separators)_
