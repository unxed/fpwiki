# Example: Anchors. How to reliably align dynamically created controls under changing visibility

[![Screenshot How to reliably align.png](https://wiki.freepascal.org/images/b/bc/Screenshot_How_to_reliably_align.png)](</File:Screenshot_How_to_reliably_align.png>)

  


![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This example works since Lazarus 0.9.25 rev.12800 ([read](<Anchor_Sides.md> "Anchor Sides"))

  


For testing, you can copy text to files: 

  * Code: **unit1.pas**
  * Form: **unit1.lfm**



  


### Code
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, ExtCtrls,
      Buttons, StdCtrls;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        CheckGroup1: TCheckGroup;
        Panel1: TPanel;
        procedure CheckGroup1ItemClick(Sender: TObject; Index: integer);
        procedure FormCreate(Sender: TObject);
      private
        FButtons: array of TButton;
      public
        { public declarations }
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      i: Integer;
    begin
      if CheckGroup1.Items.Count = 0 then
        Exit;
      SetLength(FButtons, CheckGroup1.Items.Count);
    
      for i:=Low(FButtons) to High(FButtons) do
      begin
        FButtons[i] := TButton.Create(Self);
        with FButtons[i] do
        begin
          Parent := Panel1;
          Caption := IntToStr(i+1);
          CheckGroup1.Checked[i] := True;
          Anchors := [akTop, akLeft, akRight];
    
          AnchorSide[akLeft].Control := Parent;
          AnchorSide[akLeft].Side := asrLeft;
    
          AnchorSide[akRight].Control := Parent;
          AnchorSide[akRight].Side := asrRight;
    
          if i=0 then
          begin
            AnchorSide[akTop].Control := Parent;
            AnchorSide[akTop].Side := asrTop;
          end
          else
          begin
            AnchorSide[akTop].Control := FButtons[i-1];
            AnchorSide[akTop].Side := asrBottom;
          end;
        end;
      end;
    end;
    
    procedure TForm1.CheckGroup1ItemClick(Sender: TObject; Index: integer);
    begin
      FButtons[Index].Visible := (Sender as TCheckGroup).Checked[Index];
    end;
    
    end.
    

  


### Form
    
    
    object Form1: TForm1
      Left = 527
      Height = 187
      Top = 346
      Width = 270
      Caption = 'Form1'
      ClientHeight = 187
      ClientWidth = 270
      OnCreate = FormCreate
      Position = poDesktopCenter
      LCLVersion = '1.6.0.2'
      object CheckGroup1: TCheckGroup
        Left = 16
        Height = 105
        Top = 8
        Width = 65
        AutoFill = True
        Caption = 'Visible'
        ChildSizing.LeftRightSpacing = 6
        ChildSizing.TopBottomSpacing = 6
        ChildSizing.EnlargeHorizontal = crsHomogenousChildResize
        ChildSizing.EnlargeVertical = crsHomogenousChildResize
        ChildSizing.ShrinkHorizontal = crsScaleChilds
        ChildSizing.ShrinkVertical = crsScaleChilds
        ChildSizing.Layout = cclLeftToRightThenTopToBottom
        ChildSizing.ControlsPerLine = 1
        ClientHeight = 87
        ClientWidth = 61
        Items.Strings = (
          '1'
          '2'
          '3'
          '4'
          '5'
        )
        OnItemClick = CheckGroup1ItemClick
        TabOrder = 0
        Data = {
          050000000202020202
        }
      end
      object Panel1: TPanel
        Left = 120
        Height = 171
        Top = 8
        Width = 138
        Anchors = [akTop, akLeft, akRight, akBottom]
        TabOrder = 1
      end
    end
    

## See also

  * [Anchor Sides](<Anchor_Sides.md> "Anchor Sides")

---

_Source: [https://wiki.freepascal.org/Example%3A_Anchors._How_to_reliably_align_dynamically_created_controls_under_changing_visibility](https://web.archive.org/web/20250219030918/https://wiki.freepascal.org/Example%3A_Anchors._How_to_reliably_align_dynamically_created_controls_under_changing_visibility)_
