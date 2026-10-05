# Sample Graphics

│ **English (en)** │

This gallery is to show the designs can be created from Lazarus and drawing tools, like BGRABitmap. 

Most examples are included in [BGRABitmap](<BGRABitmap.md> "BGRABitmap") & [BGRAControls](<BGRAControls.md> "BGRAControls") packages. 

You can grab more BGRABitmap demos here <https://github.com/bgrabitmap/bgracontest>

## Contents

  * 1 Gradient Graphics
    * 1.1 Flash Player Button
    * 1.2 Windows 7 Explorer Toolbar
    * 1.3 Double Gradient with Alpha
  * 2 Progress Bars
    * 2.1 Flash Player Progress Bar
  * 3 About Dialogs
    * 3.1 Lazarus About
  * 4 Shadow
  * 5 Text Shadow
    * 5.1 Single & Multi-Shadow
    * 5.2 Stretch to Form height
  * 6 Text Outline
  * 7 Three-State Button



### Gradient Graphics

**Description:** Shows how to create buttons and gradients with and without transparency in Lazarus using [BGRABitmap](<BGRABitmap.md> "BGRABitmap") and an enhanced version of [Double Gradient](<Double_Gradient.md> "Double Gradient") that supports alpha. 

**Notes:** Tested on Win32. Tested on Linux with Kubuntu and Qt, work fine 

**Win32:**

[![gdgfx.png](https://wiki.freepascal.org/images/9/92/gdgfx.png)](</File:gdgfx.png>)

**Kubuntu 10.04 with Qt:**

[![gdgfx qt.png](https://wiki.freepascal.org/images/b/b3/gdgfx_qt.png)](</File:gdgfx_qt.png>)

Top-Left: 'like flash player setup button' / Top-Right: 'like win7 explorer toolbar' / Bottom: double gradient with alpha / Background: same as bottom. 

#### Flash Player Button

  * Also you can use [ Like Flash Player Setup Controls](<BGRAButton_Gallery.md> "BGRAButton Gallery") using [BGRAControls](<BGRAControls.md> "BGRAControls") to draw the button & related stuff.



Add a new TPaintBox, set 'btn1' as name. Go OnPaint event and add this code: 
    
    
    var
      tempbmp1, tempbmp2: TBGRABitmap;
    begin
      // Background Gradient
      tempbmp1:=TBGRABitmap.Create(btn1.Width,btn1.Height,BGRA(104,104,104,255));
      tempbmp1.Canvas.GradientFill(Rect(1,Round(btn1.Height*0.25),btn1.Width-1,btn1.Height-1),$686868,$404040,gdVertical);
    
      // Frame Border
      tempbmp1.Canvas.Brush.Color:=$181818;
      tempbmp1.Canvas.FrameRect(btn1.ClientRect);
    
      // Light Gradient
      tempbmp2:=TBGRABitmap.Create(btn1.Width,btn1.Height,BGRA(0,0,0,0));
      tempbmp2.GradientFill(1,1,btn1.Width-1,btn1.Height-1,
      BGRA(255,255,255,34),
      BGRA(255,255,255,10), gtLinear,
      PointF(btn1.ClientRect.Right,btn1.ClientRect.Top),
      PointF(btn1.ClientRect.Right,btn1.ClientRect.Bottom),
      dmDrawWithTransparency,True,False);
      tempbmp2.AlphaFillRect(2,2,btn1.Width-2,btn1.Height-2,0);
    
      // Merge Bitmaps
      //tempbmp1.Canvas.Draw(0,0,tempbmp2.Bitmap);
      tempbmp2.Draw(tempbmp1.Canvas,0,0,False);
    
      // Paint in Canvas
      //btn1.Canvas.Draw(0,0,tempbmp1.Bitmap);
      tempbmp1.Draw(btn1.Canvas,0,0,False);
    
      // Free Bitmaps
      tempbmp1.Free;
      tempbmp2.Free;
    end;
    

#### Windows 7 Explorer Toolbar

  * Also you can use [ Like Windows 7 Explorer ToolBar Control](<BGRAButton_Gallery.md> "BGRAButton Gallery") using [BGRAControls](<BGRAControls.md> "BGRAControls") to draw the Button.



[![skintoolbar.png](https://wiki.freepascal.org/images/3/38/skintoolbar.png)](</File:skintoolbar.png>)

Add a new TToolBar with a TToolButton and set this properties: 

`AutoSize=True; ButtonHeight=48; EdgeBorders=[]; EdgeInner=esNone; EdgeOuter=esNone; ShowCaptions=True; Transparent=True;`

Then go Form1 OnPaint event and add this code: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      gradBmp: TBGRABitmap;
    begin
      with ToolBar1 do begin
        gradBmp := DoubleGradientAlphaFill(ClientRect,BGRA(245,250,255,255),BGRA(230,240,250,255),
        BGRA(220,230,244,255),BGRA(221,233,247,255),gdVertical,gdVertical,gdVertical,0.50);
        gradBmp.Rectangle(0,0,Width,Height-2,BGRA(255,255,255,100),dmDrawWithTransparency);
        gradBmp.SetHorizLine(0,Height-1,Width-1,BGRA(160,175,195,255));
        gradBmp.SetHorizLine(0,Height-2,Width-1,BGRA(205,218,234,255));
      end;
      gradBmp.Draw(Canvas,ToolBar1.ClientRect);
      gradBmp.Free;
    end;
    

#### Double Gradient with Alpha

Add a new TPaintBox, set 'btn3' as name. Go OnPaint event and add this code: 
    
    
    var
      myBitmap: TBGRABitmap;
    begin
      myBitmap:= DoubleGradientAlphaFill(
      btn3.ClientRect,
      BGRA(0,0,0,100),BGRA(255,255,255,100),
      BGRA(100,100,100,100),BGRA(150,150,150,100),
      gdVertical,gdVertical,gdVertical,0.5);
      //btn3.Canvas.Draw(0,0,myBitmap.Bitmap);
      myBitmap.Draw(btn3.Canvas,0,0,False);
      myBitmap.Free;
    end;
    

### Progress Bars

A progress bar is a component in a graphical user interface used to convey the progress of a task, such as a download or file transfer. 

#### Flash Player Progress Bar

  * Also you can use [ BGRAFlashProgressBar](<BGRAControls.md> "BGRAControls") control in [BGRAControls](<BGRAControls.md> "BGRAControls").



[![flprogressbar.png](https://wiki.freepascal.org/images/6/68/flprogressbar.png)](</File:flprogressbar.png>) 'Like flash player setup progress bar'. Requires [BGRABitmap](<BGRABitmap.md> "BGRABitmap") and [Double Gradient](<Double_Gradient.md> "Double Gradient") with Alpha. 

**Notes:** Tested on Win32, Kubuntu 10.04 with GTK and Qt. 

Add a new TPaintBox, set 'progressbar' as name. Go OnPaint event and add this code: 
    
    
    var
      ABitmap, ABitmap2: TBGRABitmap; ARect: TRect;
    begin
      // Initialization
      // Self.Color:=$004D4D4D;
      ABitmap:=TBGRABitmap.Create(progressbar.Width,progressbar.Height);
      ARect:=progressbar.ClientRect;
    
      // Percent of Progress
      ARect.Right:=Round(ABitmap.Width*0.75);
    
      // Background Gradient
      ABitmap.Canvas.GradientFill(progressbar.ClientRect,$303030,$323232,gdVertical);
    
      // Background Border
      ABitmap.Rectangle(0,0,ABitmap.Width,ABitmap.Height,BGRA(28,28,28,255),dmSet);
    
      // Progress Gradient
      ABitmap2:=DoubleGradientAlphaFill(
      ARect,
      BGRA(102,163,226,255),BGRA(83,135,186,255),
      BGRA(75,121,175,255),BGRA(56,93,135,255),
      gdVertical,gdVertical,gdVertical,0.5);
    
      // Progress Border
      ABitmap2.Rectangle(0,0,ARect.Right,ARect.Bottom,BGRA(28,28,28,255),dmSet);
    
      // Progress Light
      ABitmap2.Rectangle(1,1,ARect.Right-1,ARect.Bottom-1,BGRA(153,212,255,100),dmDrawWithTransparency);
    
      // Merge Bitmaps
      //ABitmap.Canvas.Draw(0,0,ABitmap2.Bitmap);
      ABitmap2.Draw(ABitmap.Canvas,0,0,False);
    
      // Draw Bitmap
      //progressbar.Canvas.Draw(0,0,ABitmap.Bitmap);
      ABitmap.Draw(progressbar.Canvas,0,0,False);
    
      // Free Bitmap
      ABitmap.Free;
      ABitmap2.Free;
    end;
    

You can change the 'Percent' of progress changing this line: 
    
    
      // Percent of Progress - 33%
      ARect.Right:=Round(ABitmap.Width*0.33);
    
      // Percent of Progress - 90%
      ARect.Right:=Round(ABitmap.Width*0.90);
    

### About Dialogs

"The Generic application includes an About dialog box. Every application should include an About dialog box. The dialog box displays such information as the application's name and copyright information." [Definition](<http://msdn.microsoft.com/en-us/library/aa383735%28v=vs.85%29.aspx>)

#### Lazarus About

Description: Lazarus-like about dialog. 

[![lazlogo.png](https://wiki.freepascal.org/images/f/f2/lazlogo.png)](</File:lazlogo.png>)

Requires [BGRABitmap](<BGRABitmap.md> "BGRABitmap"). 

  * **ToDO:**
    * ~~Smooth text~~
    * ~~Paw~~
    * Bitmap with leopard



Add a new TPaintBox, and set Width = 450, set Height = 300, set Name = 'logo' 

(you can set another width and height, for example 900 x 600) 

Go to OnPaint event and add this code: 
    
    
    uses BGRABitmap, BGRABitmapTypes, BGRAGradients;   
    
    procedure TForm1.logoPaint(Sender: TObject); 
    var
      bmp, bmp2, bmp3, bmp4, bmp5, bmp6: TBGRABitmap;
      rct: TRect;
    
      procedure DrawBackground;
      begin
        // Background
        bmp:= TBGRABitmap.Create(logo.Width,logo.Height,BGRAWhite);
    
        rct:= logo.ClientRect;
        rct.Left:= 1;
        rct.Top:= 1;
        rct.Right:= logo.Width-1;
        rct.Bottom:= Round(logo.Height*0.935);
    
        //radial
        bmp.GradientFill( rct.Left, rct.Top, rct.Right, rct.Bottom,
                          BGRA(117,172,224,255), BGRA(49,74,132,255), gtRadial,
                          PointF(Round(rct.Right*0.85), Round(rct.Bottom*0.50)),
                          PointF(Round(rct.Left), Round(rct.Bottom)), dmSet );
      end;
    
      procedure DrawEclipse;
      begin
        bmp2:= TBGRABitmap.Create(rct.Right,rct.Bottom);
        bmp2.FillEllipseAntialias( Round(rct.Right*0.57), Round(rct.Bottom*0.58),
                                   Round(rct.Right*0.31), Round(rct.Bottom*0.53),
                                   BGRA(255,255,255,30) );
        bmp2.EraseEllipseAntialias( Round(rct.Right*0.585), Round(rct.Bottom*0.58),
                                    Round(rct.Right*0.31*0.88), Round(rct.Bottom*0.53*0.88), 
                                    255 );
      end;
    
      procedure ThinCircleAndFrame;
      var mask: TBGRABitmap;
      begin
        bmp5:= TBGRABitmap.Create( rct.Right, rct.Bottom );
        bmp5.GradientFill( 0, 0, bmp5.width, bmp5.Height,
                           BGRA(117,172,224,255), BGRA(49,74,132,255), gtLinear,
                           PointF(Round(rct.Right*0.5),Round(rct.Bottom*0.50)),
                           PointF(Round(rct.Right),Round(rct.Bottom)),  dmSet);
        mask:= TBGRABitmap.Create( rct.Right, rct.Bottom, BGRABlack );
        mask.EllipseAntialias( Round(rct.Right*0.53), Round(rct.Bottom*0.50),
                               Round(rct.Right*0.40), Round(rct.Bottom*0.65),
                               BGRA(255,255,255,192), 0.6 );
        bmp5.ApplyMask( mask );
        mask.free;
    
        // Frame
        bmp6 := TBGRABitmap.Create( logo.Width,logo.Height );
        bmp6.Rectangle( logo.ClientRect,BGRABlack, BGRA(0,0,0,0), dmDrawWithTransparency );
        bmp6.DrawLine( 1, logo.Height-2, logo.Width-2, logo.Height-2, BGRA(0,0,0,75), True );
        bmp6.DrawLine( logo.Width-2, 1, logo.Width-2, logo.Height-2, BGRA(0,0,0,75), True );
      end;
    
      procedure DrawText;
      begin
        // Text
        bmp4 := TBGRABitmap.Create( logo.Width, logo.Height );
        // 'Free Pascal'
        bmp5.FontAntialias := True;
        bmp5.FontStyle := [fsBold];
        bmp5.FontHeight := Round(logo.Height*0.065);
        bmp5.TextOut( Round(logo.Width*0.15), Round(logo.Height*0.045), 'Free Pascal', BGRAWhite );
    
        // 'Lazarus'
        bmp5.FontHeight := Round(logo.Height*0.18);
        bmp5.TextOut( Round(logo.Width*0.05), Round(logo.Height*0.07),'Lazarus',BGRAWhite );
    
        // 'Project'
        bmp4.FontHeight := Round(logo.Height*0.052);
        bmp4.TextOut( Round(logo.Width*0.20), Round(logo.Height*0.27), 'Project', BGRAWhite );
    
        // 'Write Once'
        bmp4.FontHeight := Round(logo.Height*0.07);
        bmp4.TextOut( Round(logo.Width*0.02), Round(logo.Height*0.57), 'Write Once', BGRA(255,255,255,100) );
        bmp4.GradientFill( Round(logo.Width*0.02), Round(logo.Height*0.66),
                           Round(logo.Width*0.46), Round(logo.Height*0.66)+1,
                           BGRA(255,255,255,20), BGRA(0,0,0,30), gtLinear,
                           PointF(logo.Width*0.22,0), PointF(logo.Width*0.48,0),
                           dmDrawWithTransparency );
    
        // 'Compile Anywhere'
        bmp4.TextOut( Round(logo.Width*0.26), Round(logo.Height*0.8), 'Compile Anywhere', BGRA(255,255,255,100) );
        bmp4.GradientFill( Round(logo.Width*0.26), Round(logo.Height*0.89),
                           Round(logo.Width*0.70), Round(logo.Height*0.89)+1,
                           BGRA(255,255,255,20), BGRA(0,0,0,50), gtLinear,
                           PointF(logo.Width*0.26,0), PointF(logo.Width*0.70,0),
                           dmDrawWithTransparency );
      end;
    
      procedure RedRectangle;
      var
        rct2: TRect;
        gradInfo: array of TnGradientInfo;
        bmpGrad: TBGRABitmap;
      begin
        bmp3 := TBGRABitmap.Create(logo.Width,logo.Height);
    
        rct2 := logo.ClientRect;
        rct2.Left := Round(rct2.Right*0.06);
        rct2.Top := Round(rct2.Bottom*0.27);
        rct2.Right := Round(rct2.Right*0.48);
        rct2.Bottom := Round(rct2.Bottom*0.337);
    
        setlength(gradInfo,4); // Red Alpha Gradient
        gradInfo[0] := nGradientInfo(BGRAPixelTransparent, BGRA(255,0,0,255),    gdHorizontal, 0.25);
        gradInfo[1] := nGradientInfo(BGRA(255,0,0,255),    BGRA(160,0,0,180),    gdHorizontal, 0.5);
        gradInfo[2] := nGradientInfo(BGRA(160,0,0,180),    BGRA(255,0,0,255),    gdHorizontal, 0.75);
        gradInfo[3] := nGradientInfo(BGRA(255,0,0,255),    BGRAPixelTransparent, gdHorizontal, 1);
    
        bmpGrad := nGradientAlphaFill( rct2, gdHorizontal, gradInfo );
        bmp3.PutImage( rct2.Left, rct2.Top, bmpGrad, dmDrawWithTransparency );
        bmpGrad.Free;
      end;
    
      procedure DrawPaw(bmp: TBGRABitmap; center: TPointF);
      begin
        with bmp.Canvas2D do
        begin
          save;
          translate(center.X-93,center.Y-83);
          globalAlpha := 0.4;
          fillStyle ('#3f5e99');
          beginPath();
          moveTo(89.724698,11.312043);
          bezierCurveTo(95.526308,14.494575,100.52322000000001,18.838808,102.75144,24.966412);
          bezierCurveTo(114.24578,26.586847,123.07072,43.010127999999995,118.71826,54.504664);
          bezierCurveTo(114.77805000000001,64.910473,93.426098,68.10145299999999,89.00143800000001,59.252123);
          bezierCurveTo(86.231818,53.712894999999996,90.877898,48.213108999999996,88.853498,42.139906999999994);
          bezierCurveTo(87.401408,37.78364299999999,82.208048,33.87411899999999,85.595888,27.098436999999993);
          bezierCurveTo(87.071858,24.146481999999992,94.76621800000001,25.279547999999995,94.863658,23.444067999999994);
          bezierCurveTo(95.066728,19.618834999999994,92.648878,18.165403999999995,90.221828,15.326465999999995);
          closePath();
          fill();
          beginPath();
          moveTo(53.024288,20.876975);
          bezierCurveTo(50.128958,26.827119000000003,48.561707999999996,33.260252,50.284608,39.548662);
          bezierCurveTo(41.840728,47.513997,44.130318,66.017003,54.325338,72.88213300000001);
          bezierCurveTo(63.554708000000005,79.09700300000002,82.823918,69.36119300000001,81.320528,59.58223300000001);
          bezierCurveTo(80.379498,53.461101000000006,73.409408,51.65791100000001,71.551608,45.53168800000001);
          bezierCurveTo(70.219018,41.13739400000001,72.197818,34.94548700000001,65.517188,31.373877000000007);
          bezierCurveTo(62.606638000000004,29.817833000000007,56.98220800000001,35.18931200000001,55.841908000000004,33.74771500000001);
          bezierCurveTo(53.465478000000004,30.743354000000007,54.598668,28.159881000000006,54.938648,24.44039800000001);
          closePath();
          fill();
          beginPath();
          moveTo(16.284108,78.650993);
          bezierCurveTo(16.615938,85.259863,18.344168,91.651623,22.885208,96.330453);
          bezierCurveTo(19.327327999999998,107.37975,30.253377999999998,122.48687000000001,42.495058,123.58667);
          bezierCurveTo(53.577238,124.58229,65.765908,106.76307,59.734438,98.920263);
          bezierCurveTo(55.959047999999996,94.01106300000001,48.983098,95.791453,44.402058,91.319753);
          bezierCurveTo(41.116108,88.112233,39.864737999999996,81.73340300000001,32.289848,81.824883);
          bezierCurveTo(28.989708,81.864783,26.651538,89.282293,24.957518,88.569003);
          bezierCurveTo(21.427108,87.08246299999999,21.174458,84.272723,19.679208,80.85010299999999);
          closePath();
          fill();
          beginPath();
          moveTo(152.77652,37.616125);
          bezierCurveTo(156.68534,42.955439,159.37334,49.006564,158.79801,55.501293);
          bezierCurveTo(168.5256,61.835313,169.5682,80.450283,160.75895,89.021463);
          bezierCurveTo(152.78409,96.780823,132.08894,90.63274299999999,131.82654,80.742363);
          bezierCurveTo(131.6623,74.551503,138.19976,71.535693,138.93671,65.17653299999999);
          bezierCurveTo(139.46532,60.615162999999995,136.41531,54.87470199999999,142.35299,50.170306999999994);
          bezierCurveTo(144.93985,48.12074299999999,151.43107,52.404562999999996,152.29636,50.78291599999999);
          bezierCurveTo(154.09968999999998,47.403324999999995,152.52446999999998,45.062994999999994,151.52745,41.463536999999995);
          closePath();
          fill();
          beginPath();
          moveTo(139.65359,109.38478);
          bezierCurveTo(179.13505,123.79982000000001,142.51298,146.31478,119.19800000000001,151.55864);
          bezierCurveTo(95.883018,156.8025,41.93790800000001,157.82316,75.508908,123.02183);
          bezierCurveTo(78.980078,119.42344999999999,79.61785800000001,104.19731999999999,82.074898,99.283253);
          bezierCurveTo(86.361158,93.329663,106.23528,86.908083,113.13709,88.929193);
          bezierCurveTo(128.23085,93.960443,125.96716,106.89633,139.65359,109.38478);
          closePath();
          fill();
          restore;
        end;
      end;
    
    begin
      DrawBackground;
      DrawEclipse;
      ThinCircleAndFrame;
      DrawText;
      RedRectangle;
    
      // Merge Bitmaps
      bmp.PutImage(0,0,bmp2,dmDrawWithTransparency);
      bmp.PutImage(0,0,bmp3,dmDrawWithTransparency);
      bmp.PutImage(0,0,bmp4,dmDrawWithTransparency);
      bmp.PutImage(0,0,bmp5,dmDrawWithTransparency);
      bmp.PutImage(0,0,bmp6,dmDrawWithTransparency);
      DrawPaw(bmp,PointF(258,161));
    
      // Draw in Canvas
      bmp.Draw(logo.Canvas,0,0,True);
    
      // Free Bitmaps
      bmp.Free;
      bmp2.Free;
      bmp3.Free;
      bmp4.Free;
      bmp5.Free;
      bmp6.Free;
    end;
    

### Shadow

[![shadowmoon.png](https://wiki.freepascal.org/images/1/15/shadowmoon.png)](</File:shadowmoon.png>)

Using the same code in BGRABitmap Tutorial 5 we can make a moon with shadow using the Shadow function. 

Go OnPaint event and add this code: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var image: TBGRABitmap;
        size: single;
    
      // Shadow Function
      function Shadow(ASource: TBGRABitmap; AShadowColor: TBGRAPixel; AOffSetX,AOffSetY: Integer; ARadius: Integer; AShowSource: Boolean = True): TBGRABitmap;
      var
       bmpOut: TBGRABitmap;
       n: integer;
       p: PBGRAPixel;
      begin
       bmpOut:= TBGRABitmap.Create(ASource.Width+2*ARadius+AOffSetX,ASource.Height+2*ARadius+AOffSetY);
    
       bmpOut.PutImage(AOffSetX,AOffSetY,ASource,dmDrawWithTransparency);
    
       p := bmpOut.Data;
       for n := 1 to bmpOut.NbPixels do begin
        if p^.alpha <> 0 then begin
         p^.red := AShadowColor.red;
         p^.green := AShadowColor.green;
         p^.blue := AShadowColor.blue;
        end;
        inc(p);
       end;
    
       BGRAReplace(bmpOut,bmpOut.FilterBlurRadial(ARadius,rbFast));
    
       if AShowSource = True then bmpOut.PutImage(0,0,ASource,dmDrawWithTransparency);
    
       Result:= bmpOut;
      end;
    
      procedure DrawMoon;
      var layer: TBGRABitmap;
      begin
        layer := TBGRABitmap.Create(image.Width,image.Height);
        layer.FillEllipseAntialias(layer.Width/2,layer.Height/2,size*0.4,size*0.4,BGRA(224,224,224,128));
        layer.EraseEllipseAntialias(layer.Width/2+size*0.15,layer.Height/2,size*0.3,size*0.3,255);
        BGRAReplace(layer,Shadow(layer,BGRABlack,5,5,5));
        image.PutImage(0,0,layer,dmDrawWithTransparency);
        layer.Free;
      end;
    
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight);
    
      //Compute available space in both directions
      if image.Height < image.Width then
        size := image.Height
      else
        size := image.Width;
    
      image.GradientFill(0,0,image.Width,image.Height,
                         BGRA(128,192,255),BGRA(0,0,255),
                         gtLinear,PointF(0,0),PointF(0,image.Height),
                         dmSet);
    
      DrawMoon;
    
      image.Draw(Canvas,0,0,True);
      image.free;
    end;
    

### Text Shadow

Usage of BGRABitmap TextShadow function, create cool text with blured shadows and custom font properties. 

#### Single & Multi-Shadow

[![textshadow.png](https://wiki.freepascal.org/images/3/3d/textshadow.png)](</File:textshadow.png>)

This requires BGRABitmap, BGRABitmapTypes & BGRAGradients. 

Go OnPaint event and add this code: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      bmp1,bmp2: TBGRABitmap; txt: String;
    begin
      txt:= 'TextShadow!';
    
      // Simple Shadow
      bmp1:= TextShadow(200,40,txt,32,BGRAWhite,BGRABlack,1,1);
      bmp1.Draw(Canvas,0,0,False);
      bmp1.Free;
    
      // Blured Shadow
      bmp1:= TextShadow(200,40,txt,32,BGRABlack,BGRA(128,128,255,255),4,4,5);
      bmp1.Draw(Canvas,0,40,False);
      bmp1.Free;
    
      // Neon Shadow ''(better with Black background)''
      bmp1:= TextShadow(200,40,txt,32,BGRA(255,255,255,200),BGRA(0,255,0,255),0,0,5);
      bmp1.Draw(Canvas,0,80,False);
      bmp1.Free;    
    
      // Multi Shadow
      bmp1:= TextShadow(250,50,txt,32,BGRAWhite,BGRA(255,0,0,255),-5,-5,4,[fsBold,fsUnderline],'Tahoma',False);
      bmp2:= TextShadow(250,50,txt,32,BGRAWhite,BGRA(0,0,255,255),5,5,4,[fsBold,fsUnderline],'Tahoma',True);
      bmp1.PutImage(0,0,bmp2,dmDrawWithTransparency);
      bmp1.Draw(Canvas,0,120,False);
      bmp1.Free;
      bmp2.Free;
    end;
    

#### Stretch to Form height

[![shadow.png](https://wiki.freepascal.org/images/5/51/shadow.png)](</File:shadow.png>)

Go OnPaint event and add this code: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var
      bmp: TBGRABitmap;
    begin
      Color:= clBlack;
      bmp:= TextShadow(Width,Height,'Shadow',Round(Height*0.5),BGRA(128,0,0,255),BGRAWhite,10,10,5,[fsBold],'Chiller');
      bmp.Draw(Canvas,0,0,False);
      bmp.Free;
    end;
    

### Text Outline

If you need quality text outline use BGRABitmap Text Effect unit. 

[![textoutline.png](https://wiki.freepascal.org/images/e/eb/textoutline.png)](</File:textoutline.png>)

Go OnPaint event and add this code: 
    
    
    uses Types;
    
    procedure TForm1.FormPaint(Sender: TObject);
    const textContent = 'Some text'; fontheight = 30;
    var image,textBmp,outline: TBGRABitmap;
        size: TSize;
        p: PBGRAPixel;
        n: Integer;
    begin
      image := TBGRABitmap.Create(ClientWidth,ClientHeight, ColorToBGRA(ColorToRGB(clBtnFace)) );
    
      //define font param
      image.FontAntialias := true;
      image.FontHeight := fontheight;
      image.FontStyle := [fsBold];
    
      //create text image
      size := image.TextSize(textContent);
      textBmp := TBGRABitmap.Create(size.cx+2,size.cy+2,BGRAWhite);
      image.CopyPropertiesTo(textBmp);
      textBmp.TextOut(1,1,textContent,BGRABlack);
    
      //create outline
      outline := textbmp.FilterContour as TBGRABitmap;
      textBmp.Free;
      p := outline.data;
      for n := 0 to outline.NbPixels-1 do
      begin
        p^.alpha := 255-(GammaExpansionTab[p^.red] shr 8);
        p^.red := 0; //outline color
        p^.green := 0;
        p^.blue := 192;
        inc(p);
      end;
    
      //draw outline
      image.PutImage(20-1,20-1,outline,dmDrawWithTransparency);
      outline.Free;
    
      //draw inner text
      image.TextOut(20,20,textContent,BGRAWhite);
    
      image.Draw(Canvas,0,0,True);
      image.free;
    end;
    

### Three-State Button

This way to create a Three-State Button is obsolete. Now you can use [BGRAControls](<BGRAControls.md> "BGRAControls") and add TBGRAButton that is a fully customizable button. You can see some examples in the [BGRAButton Gallery](<BGRAButton_Gallery.md> "BGRAButton Gallery"). 

[![statebutton.png](https://wiki.freepascal.org/images/3/33/statebutton.png)](</File:statebutton.png>)

This is how to create a 3 state button ('Normal','Enter','Pressed') with a [PaintBox](</index.php?title=PaintBox&action=edit&redlink=1> "PaintBox \(page does not exist\)") & [BGRABitmap](<BGRABitmap.md> "BGRABitmap"). 

**1) Add a new PaintBox:**

Name=btn 

Width=128 

Height=64 

**2) Add this code:**
    
    
    unit Unit1; 
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, ExtCtrls,
      BGRABitmap, BGRABitmapTypes, BGRAGradients;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        btn: TPaintBox;
        procedure btnPaint(Sender: TObject);
        procedure btnMouseDown(Sender: TObject; Button: TMouseButton; Shift: TShiftState; X, Y: Integer);
        procedure btnMouseEnter(Sender: TObject);
        procedure btnMouseLeave(Sender: TObject);
        procedure btnMouseUp(Sender: TObject; Button: TMouseButton; Shift: TShiftState; X, Y: Integer);
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
      private
        { private declarations }
        btnCaption: String;
        btnState: String;
      public
        { public declarations }
      end;
    
      function DrawButton(ARect: TRect; AText: String; cl1,cl2,cl3,cl4: TBGRAPixel; AValue: Single): TBGRABitmap;
    
    var
      Form1: TForm1; 
    
    implementation
    
    function DrawButton(ARect: TRect; AText: String; cl1,cl2,cl3,cl4: TBGRAPixel; AValue: Single): TBGRABitmap;
    var
      bmpBtn,bmpTxt: TBGRABitmap;
    begin
      bmpBtn:= DoubleGradientAlphaFill(ARect,cl1,cl2,cl3,cl4,gdVertical,gdVertical,gdVertical,AValue);
      bmpBtn.Rectangle(0,0,ARect.Right,ARect.Bottom,BGRA(0,0,0,150),dmDrawWithTransparency);
      bmpTxt:= TextShadow(ARect.Right,ARect.Bottom,AText,Round(ARect.Bottom*0.5),BGRAWhite,BGRABlack,1,1,1);
      bmpBtn.PutImage(0,0,bmpTxt,dmDrawWithTransparency);
      bmpTxt.Free;
      Result:=bmpBtn;
    end;
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.btnPaint(Sender: TObject);
    var
      bmpBtn: TBGRABitmap;
    begin
      if btnState = 'Normal' then begin
        bmpBtn:= DrawButton(btn.ClientRect,btnCaption,BGRABlack,BGRAWhite,BGRAWhite,BGRABlack,0.5); end
      else if btnState = 'Pressed' then begin
        bmpBtn:= DrawButton(btn.ClientRect,btnCaption,BGRAWhite,BGRABlack,BGRABlack,BGRAWhite,0.5); end
      else if btnState = 'Enter' then begin
        bmpBtn:= DrawButton(btn.ClientRect,btnCaption,BGRA(100,100,100,255),BGRAWhite,BGRAWhite,BGRA(100,100,100,255),0.5);
      end;
      bmpBtn.Draw(btn.Canvas,0,0,False);
      bmpBtn.Free;
    end;
    
    procedure TForm1.btnMouseDown(Sender: TObject; Button: TMouseButton; Shift: TShiftState; X, Y: Integer);
    begin
      btnState:= 'Pressed';
      btnPaint(nil);
    end;
    
    procedure TForm1.btnMouseEnter(Sender: TObject);
    begin
      btnState:= 'Enter';
      btnPaint(nil);
    end;
    
    procedure TForm1.btnMouseLeave(Sender: TObject);
    begin
      btnState:= 'Normal';
      btnPaint(nil);
    end;
    
    procedure TForm1.btnMouseUp(Sender: TObject; Button: TMouseButton; Shift: TShiftState; X, Y: Integer);
    begin
      btnState:= 'Enter';
      btnPaint(nil);
    end;
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      btnCaption:= 'Button';
      btnState:= 'Normal';
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      btn.Free;
    end;
    
    end.

---

_Source: [https://wiki.freepascal.org/Sample_Graphics](https://web.archive.org/web/20250114101617/https://wiki.freepascal.org/Sample_Graphics)_
