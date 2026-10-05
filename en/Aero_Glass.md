# Aero Glass

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[Deutsch (de)](</Aero_Glass/de> "Aero Glass/de")** │  **English (en)** │  **[español (es)](</Aero_Glass/es> "Aero Glass/es")** │  **[polski (pl)](</Aero_Glass/pl> "Aero Glass/pl")** │    
****

## Contents

  * 1 Aero Glass effect on Lazarus Form
  * 2 Windows 10
  * 3 Bugs
  * 4 About



## Aero Glass effect on Lazarus Form

[![aero glass lazarus.png](https://wiki.freepascal.org/images/8/83/aero_glass_lazarus.png)](</File:aero_glass_lazarus.png>)

First save the above code to a text file "glass.pas": 
    
    
    unit glass;
    
    {$mode delphi}
    //{$mode objfpc}{$H+}
    
    interface
    
    uses
      Windows, Forms, Graphics;
    
    type
      _MARGINS = packed record
        cxLeftWidth    : Integer;
        cxRightWidth   : Integer;
        cyTopHeight    : Integer;
        cyBottomHeight : Integer;
      end;
    
      PMargins = ^_MARGINS;
      TMargins = _MARGINS;
    
      DwmIsCompositionEnabledFunc      = function(pfEnabled: PBoolean): HRESULT; stdcall;
      DwmExtendFrameIntoClientAreaFunc = function(destWnd: HWND; const pMarInset: PMargins): HRESULT; stdcall;
      SetLayeredWindowAttributesFunc   = function(destWnd: HWND; cKey: TColor; bAlpha: Byte; dwFlags: DWord): BOOL; stdcall;
    
    const
      WS_EX_LAYERED = $80000;
      LWA_COLORKEY  = 1;
    
    procedure GlassForm(frm: TForm; tmpMargins: TMargins; cBlurColorKey: TColor = clFuchsia);
    function WindowsAeroGlassCompatible: Boolean;
    
    implementation
    
    function WindowsAeroGlassCompatible: Boolean;
    var
      osVinfo: TOSVERSIONINFO;
    begin
      ZeroMemory(@osVinfo, SizeOf(osVinfo));
      OsVinfo.dwOSVersionInfoSize := SizeOf(TOSVERSIONINFO);
      if (
      (GetVersionEx(osVInfo)   = True) and
      (osVinfo.dwPlatformId    = VER_PLATFORM_WIN32_NT) and
      (osVinfo.dwMajorVersion >= 6)
      )
      then Result:=True
      else Result:=False;
    end;
    
    procedure GlassForm(frm: TForm; tmpMargins: TMargins; cBlurColorKey: TColor = clFuchsia);
    var
      hDwmDLL: Cardinal;
      fDwmIsCompositionEnabled: DwmIsCompositionEnabledFunc;
      fDwmExtendFrameIntoClientArea: DwmExtendFrameIntoClientAreaFunc;
      fSetLayeredWindowAttributesFunc: SetLayeredWindowAttributesFunc;
      bCmpEnable: Boolean;
      mgn: TMargins;
    begin
      { Continue if Windows version is compatible }
      if WindowsAeroGlassCompatible then begin
        { Continue if 'dwmapi' library is loaded }
        hDwmDLL := LoadLibrary('dwmapi.dll');
        if hDwmDLL <> 0 then begin
          { Get values }
          @fDwmIsCompositionEnabled        := GetProcAddress(hDwmDLL, 'DwmIsCompositionEnabled');
          @fDwmExtendFrameIntoClientArea   := GetProcAddress(hDwmDLL, 'DwmExtendFrameIntoClientArea');
          @fSetLayeredWindowAttributesFunc := GetProcAddress(GetModulehandle(user32), 'SetLayeredWindowAttributes');
          { Continue if values are <> nil }
          if (
          (@fDwmIsCompositionEnabled <> nil) and
          (@fDwmExtendFrameIntoClientArea <> nil) and
          (@fSetLayeredWindowAttributesFunc <> nil)
          )
          then begin
            { Continue if composition is enabled }
            fDwmIsCompositionEnabled(@bCmpEnable);
            if bCmpEnable = True then begin
              { Set Form Color same as cBlurColorKey }
              frm.Color := cBlurColorKey;
              { ... }
              SetWindowLong(frm.Handle, GWL_EXSTYLE, GetWindowLong(frm.Handle, GWL_EXSTYLE) or WS_EX_LAYERED);
              { ... }
              fSetLayeredWindowAttributesFunc(frm.Handle, cBlurColorKey, 0, LWA_COLORKEY);
              { Set margins }
              ZeroMemory(@mgn, SizeOf(mgn));
              mgn.cxLeftWidth    := tmpMargins.cxLeftWidth;
              mgn.cxRightWidth   := tmpMargins.cxRightWidth;
              mgn.cyTopHeight    := tmpMargins.cyTopHeight;
              mgn.cyBottomHeight := tmpMargins.cyBottomHeight;
              { Extend Form }
              fDwmExtendFrameIntoClientArea(frm.Handle,@mgn);
            end;
          end;
          { Free loaded 'dwmapi' library }
          FreeLibrary(hDWMDLL);
        end;
      end;
    end;
    
    end.
    

Copy the "glass.pas" file to the main folder of your project: 
    
    
     MyProject\glass.pas
     
    

In the "uses" section of your project you need to add "glass": 
    
    
    unit form1;
      
    {$mode objfpc}{$H+}
      
    interface
      
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs
      glass; // This includes GlassForm procedure
    

OnActivate event of each form call the procedure in this way: 
    
    
    procedure TForm1.FormActivate(Sender: TObject);
    var
      tmpMargins: TMargins;
    begin
      { If all margins are -1 the whole form will be aero glass}
      tmpMargins.cxLeftWidth    := -1;
      tmpMargins.cxRightWidth   := -1;
      tmpMargins.cyBottomHeight := -1;
      tmpMargins.cyTopHeight    := -1;
      { FormName ; Margins ; TransparentColor }
      GlassForm(Self, tmpMargins, clFuchsia); 
    end;
    

Also you need to enable Themes to use this procedure, go to Options > Project Options > then select "Use Manifest to Enable Themes (Windows)". 

## Windows 10

[![aeroglasswin10.png](https://wiki.freepascal.org/images/8/84/aeroglasswin10.png)](</File:aeroglasswin10.png>)
    
    
    {
    author: vhanla
    http://vhanla.codigobit.info
    }
    unit Unit1;
    
    interface
    
    uses
      Windows, Messages, SysUtils, Variants, Classes, Graphics,
      Controls, Forms, Dialogs, DWMApi, png, IntfGraphics,  FPImage, GraphType,
      StdCtrls, ExtCtrls, BCButtonFocus, BCLabel,  Registry,  LMessages, LCLType, LCLIntf;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        BCButtonFocus1: TBCButtonFocus;
        BCLabel1: TBCLabel;
        Image1: TImage;
        Image2: TImage;
        Panel1: TPanel;
    
        procedure BCButtonFocus1Click(Sender: TObject);
        procedure FormCreate(Sender: TObject);
        procedure Button1Click(Sender: TObject);
        procedure Panel1MouseDown(Sender: TObject; Button: TMouseButton;
          Shift: TShiftState; X, Y: Integer);
      private
        { Private declarations }
        function TaskbarAccented:boolean;
        function TaskbarTranslucent:boolean;
        procedure EnableBlur;
        procedure AeroGlass;
        procedure UpdateColorization;
        function GetAccentColor:TColor;
      public
        { Public declarations }
      end;
    
      AccentPolicy = packed record
        AccentState: Integer;
        AccentFlags: Integer;
        GradientColor: Integer;
        AnimationId: Integer;
      end;
    
      TWinCompAttrData = packed record
        attribute: THandle;
        pData: Pointer;
        dataSize: ULONG;
      end;
    
    
    var
      Form1: TForm1;
    
    var
      SetWindowCompositionAttribute: function (Wnd: HWND; const AttrData: TWinCompAttrData): BOOL; stdcall = Nil;
    implementation
    
    {$R *.lfm}
    
    procedure SetAlphaColorPicture(
      const Col: TColor;
      const Alpha: Integer;
      Picture: TPicture;
      const _width: Integer;
      const _height: Integer
      );
    var
      IM: TLazIntfImage;
      x,y: integer;
      sl: pByteArray;
      I, J, W, H: Integer;
      FC: TFPColor;
    begin
        IM := TLazIntfImage.Create(0, 0, [riqfRGB, riqfAlpha]);
        try
          IM.SetSize(_width, _height);
          for I := 0 to _width - 1 do
          begin
            for J := 0 to _height - 1 do
            begin
              FC.red := (128 + I) shl 8;
              FC.green := (128 + J) shl 8;
              FC.blue := 128 shl 8;
              FC.alpha := 128 shl 8; // now works fine
              IM.Colors[I, J] := FC;
            end;
          end;
          Picture.Assign(IM);
    
    
        finally
          IM.Free;
    
        end;
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      Close;
    end;
    
    procedure TForm1.Panel1MouseDown(Sender: TObject; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer);
    begin
      ReleaseCapture;
       SendMessage(Form1.Handle, LM_SYSCOMMAND, 61458, 0) ;
    end;
    
    procedure TForm1.AeroGlass;
    var
      Aero: BOOL;
      Area: TRect;
      hDWM: THandle;
    begin
      hDWM:=LoadLibrary('dwmapi.dll');
      try
        @DwmIsCompositionEnabled:=GetProcAddress(hDWM,'DwmIsCompositionEnabled');
        if @DwmIsCompositionEnabled<>nil then
            DwmIsCompositionEnabled(Aero);
        if Aero then
        begin
          Area:=Rect(-1,-1,-1,-1);
          Color:=clBlack;
          @DwmExtendFrameIntoClientArea:=GetProcAddress(hDWM,'DwmExtendFrameIntoClientArea');
          if @DwmExtendFrameIntoClientArea<>nil then
              DwmExtendFrameIntoClientArea(Handle,@Area);
    
        end
        else ShowMessage('Aero is Disabled');
      finally
        FreeLibrary(hDWM);
      end;
    end;
    
    procedure TForm1.EnableBlur;
    const
      WCA_ACCENT_POLICY = 19;
      ACCENT_ENABLE_BLURBEHIND = 3;
      ACCENT_ENABLE_ACRYLICBLURBEHIND = 4;
      DrawLeftBorder = $20;
      DrawTopBorder = $40;
      DrawRightBorder = $80;
      DrawBottomBorder = $100;
    var
      dwm10: THandle;
      data : TWinCompAttrData;
      accent: AccentPolicy;
    begin
    
          dwm10 := LoadLibrary('user32.dll');
          try
            @SetWindowCompositionAttribute := GetProcAddress(dwm10, 'SetWindowCompositionAttribute');
            if @SetWindowCompositionAttribute <> nil then
            begin
              accent.AccentState := ACCENT_ENABLE_BLURBEHIND ;
              //accent.AccentState := ACCENT_ENABLE_ACRYLICBLURBEHIND;
              accent.GradientColor := (100 SHL 24) or ($00E3E0DE);
              accent.AccentFlags := DrawLeftBorder or DrawTopBorder or DrawRightBorder or DrawBottomBorder;
    
              data.Attribute := WCA_ACCENT_POLICY;
              data.dataSize := SizeOf(accent);
              data.pData := @accent;
              SetWindowCompositionAttribute(self.Handle, data);
            end
            else
            begin
              ShowMessage('Not found Windows 10 blur API');
            end;
          finally
            FreeLibrary(dwm10);
          end;
    
    end;
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      DoubleBuffered := True;
      Color := clBlack;
      BorderStyle := bsnoNe;
      BorderIcons := [biSystemMenu, biMinimize, biMaximize];
      //AeroGlass;
      if TaskbarTranslucent then
        EnableBlur;
    
      UpdateColorization;
    end;
    
    procedure TForm1.BCButtonFocus1Click(Sender: TObject);
    begin
      close;
    end;
    
    function TForm1.TaskbarAccented: boolean;
    var
      reg: TRegistry;
    begin
      Result := False;
      reg := TRegistry.Create;
      try
        reg.RootKey := HKEY_CURRENT_USER;
        reg.OpenKeyReadOnly('SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize');
        try
          if reg.ReadInteger('ColorPrevalence') = 1 then
          Result := True;
        except
          Result := False;
        end;
        reg.CloseKey;
    
      finally
        reg.Free;
      end;
    end;
    
    function TForm1.TaskbarTranslucent: boolean;
    var
      reg: TRegistry;
    begin
      Result := False;
      reg := TRegistry.Create;
      try
        reg.RootKey := HKEY_CURRENT_USER;
        reg.OpenKeyReadOnly('SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize');
        try
          if reg.ReadInteger('EnableTransparency') = 1 then
          Result := True;
        except
          Result := False;
        end;
        reg.CloseKey;
    
      finally
        reg.Free;
      end;
    end;
    
    
    
    procedure TForm1.UpdateColorization;
    begin
      if TaskbarTranslucent then
      begin
        SetAlphaColorPicture(clblack, 200, Image1.Picture, 10,10  );
        Image1.Align := alClient;
        Image1.Stretch := True;
        Image1.Visible := True;
      end
      else
        Image1.Visible := False;
    
      if TaskbarAccented then
      begin
        SetAlphaColorPicture(9338482, 110, Image2.Picture, 10, 10);
        Image2.Visible := True;
        Image2.Align := alClient;
        Image2.Stretch := True;
      end
      else
        Image2.Visible := False;
    end;
    
    function TForm1.GetAccentColor:TColor;
    var
      col: cardinal;
      opaque: longbool;
      newcolor: TColor;
      a,r,g,b: byte;
    begin
      DwmGetColorizationColor(col, opaque);
      a := Byte(col shr 24);
      r := Byte(col shr 16);
      g := Byte(col shr 8);
      b := Byte(col);
    
    
      newcolor := RGB(
          round(r*(a/255)+255-a),
          round(g*(a/255)+255-a),
          round(b*(a/255)+255-a)
      );
    
      Result := newcolor;
    
    
    end;
    
    
    initialization
      SetWindowCompositionAttribute := GetProcAddress(GetModuleHandle(user32), 'SetWindowCompositionAttribute');
    end.
    

## Bugs

As you can see in the first image Labels aren't displayed fine in Aero Glass, there are links with components and code that show how to make "Glow Labels": 

  * [GlowLabel for Delphi](<http://development.mwcs.de/glowlabel.html>)
  * [Aero Glass - ein Text (Label) hintergrund "machen"](<http://www.delphipraxis.net/953968-post4.html>)



Also if you clic the transparent part of the window the back window / desktop is selected (focus is lost in the form with aero glass). 

## About

This was converted to Lazarus using "{$mode delphi}" from "Aero Glass Effekt für Delphi-Forms, Delphi-Unit von Daniel Mitte (2006)": 

  * [Aero Glass Effekt für Delphi-Forms](<http://www.delphipraxis.net/74538-aero-glass-effekt-fuer-delphi-forms.html>)



There is a Delphi component here (to be ported in Lazarus): 

  * [D. Mitte in Torry's Delphi Page](<http://www.torry.net/authorsmore.php?id=6179>)

---

_Source: [https://wiki.freepascal.org/Aero_Glass](https://web.archive.org/web/20250114054912/https://wiki.freepascal.org/Aero_Glass)_
