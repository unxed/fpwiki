# BGRABitmap tutorial TAChart

│ **English (en)** │  [**français (fr)**](</BGRABitmap_tutorial_TAChart/fr> "BGRABitmap tutorial TAChart/fr") │    
****

You can make beautiful charts using [BGRABitmap](<BGRABitmap.md> "BGRABitmap"). Add TAChartBGRA package with the project inspector. 

Maybe the package will not appear in the list or will not be installed. In this case, open it manually with the menu Package, Open package file (.lpk). It must be in _component\tachart_. Then click on the button to install the package. 

## Contents

  * 1 Add a TAChart
  * 2 Decorate in normal rendering mode
  * 3 Using BGRABitmap to render the whole chart
  * 4 Drawing a chart on any Canvas
    * 4.1 Simple drawing
    * 4.2 Text effects



## Add a TAChart

[TAChart](<TAChart.md> "TAChart") in fact has the classname TChart. To add a chart, browse the components, choose Chart tab and click on TChart icon. Click and draw a rectangle on the window to drop it. 

In order to supply data, click on TRandomChartSource component icon and click once in the window to drop it. In the Object Inspector, define PointsNumber to 10, XMax to 10 and YMax to 10. 

Now let's create a series that uses this source. To do that, click on your chart and in the Object Inspector, go to Series property. Click on the three dots, and add a _Bar series_. In its properties, go to Source property and choose RandomChartSource1. The chart should be filled with red bars. 

[![tachartrandombars.png](https://wiki.freepascal.org/images/8/86/tachartrandombars.png)](</File:tachartrandombars.png>)

As we are going to do some 3D, we need some depth. So set the Depth property of the series to 10. 

[![tachartrandombars3D.png](https://wiki.freepascal.org/images/f/f1/tachartrandombars3D.png)](</File:tachartrandombars3D.png>)

Finally, click on the chart and set the Align property to alClient in order to fill the window. 

## Decorate in normal rendering mode

Go to the Series property of your chart. Choose the bar series, and in the Object Inspector, go to the events and define OnBeforeDrawBar. Add the following code: 
    
    
    uses TABGRAUtils;
    
    procedure TForm1.BeforeDrawBarHandler(ASender: TBarSeries; ACanvas: TCanvas;
      const ARect: TRect; APointIndex, AStackIndex: Integer;
      var ADoDefaultDrawing: Boolean);
    begin
      ADoDefaultDrawing:= false;
      DrawPhong3DBar(ASender, ACanvas, ARect, APointIndex);
    end;

Then run the program. 

Your chart should look like this: [![chartwithbgra.png](https://wiki.freepascal.org/images/7/70/chartwithbgra.png)](</File:chartwithbgra.png>)

You can also use the DrawChocolateBar function. Set PointsNumber property of RandomChartSource1 to 5. Change the code of the OnBeforeDrawBarEvent to: 
    
    
    uses TABGRAUtils;
    
    procedure TForm1.BeforeDrawBarHandler(ASender: TBarSeries; ACanvas: TCanvas;
      const ARect: TRect; APointIndex, AStackIndex: Integer;
      var ADoDefaultDrawing: Boolean);
    begin
      ADoDefaultDrawing:= false;
      DrawChocolateBar(ASender, ACanvas, ARect, APointIndex, True);
    end;

[![chartwithbgrachocolate.png](https://wiki.freepascal.org/images/6/6e/chartwithbgrachocolate.png)](</File:chartwithbgrachocolate.png>)

## Using BGRABitmap to render the whole chart

Bars are not very different with or without antialiasing. Go to the Series property and remove the bars, add a _Line series_ , and set its Source to RandomChartSource1. Then, set its property Depth to 10 and its SeriesColor property to clRed. 

[![tachartlines3D.png](https://wiki.freepascal.org/images/a/a7/tachartlines3D.png)](</File:tachartlines3D.png>)

In order to change the rendering engine, look for the TChartGUIConnectorBGRA component in the Chart tab. Click to drop it on the chart. Then, click on the chart background and with the object inspector, set the GUIConnector property by choosing ChartGUIConnectorBGRA1. The rendering is now done with antialiasing, no need to start the program to see: 

[![tachartlines3Dbgra.png](https://wiki.freepascal.org/images/a/ac/tachartlines3Dbgra.png)](</File:tachartlines3Dbgra.png>)

## Drawing a chart on any Canvas

### Simple drawing

First add the TAChartBGRA package. To render your chart, you can for example use a PaintBox. In the OnPaint event, write : 
    
    
    uses BGRABitmap, TADrawerBGRA;
    
    procedure TForm1.PaintBox1Paint(Sender: TObject);
    var
      bmp: TBGRABitmap;
      id: IChartDrawer;
      rp: TChartRenderingParams;
    begin
      bmp := TBGRABitmap.Create(PaintBox1.Width, PaintBox1.Height);
      Chart1.DisableRedrawing;
      try
        id := TBGRABitmapDrawer.Create(bmp);
        id.DoGetFontOrientation := @CanvasGetFontOrientationFunc;
        rp := Chart1.RenderingParams;
        Chart1.Draw(id, Rect(0, 0, PaintBox1.Width, PaintBox1.Height));
        Chart1.RenderingParams := rp;
        bmp.Draw(PaintBox1.Canvas, 0, 0);
      finally
        Chart1.EnableRedrawing;
        bmp.Free;
      end;
    end;

[![chartbgra.png](https://wiki.freepascal.org/images/7/7e/chartbgra.png)](</File:chartbgra.png>)

To avoid flickering, consider using TBGRAVirtualScreen from [BGRAControls](<BGRAControls.md> "BGRAControls"). 

### Text effects

To add text effects, you need the latest version of BGRABitmap, and the latest SVN of Lazarus. There is a FontRenderer property in each TBGRABitmap image, that you can define. You must not free the renderer, as it is automatically freed by the image. 

The text effects are provided by the BGRATextFX unit that contains the TBGRATextEffectFontRenderer class. To add a golden contour to the letters and a shadow, add the following lines : 
    
    
    uses BGRATextFX, BGRAGradientScanner;
    ...
    var
      ...
      fontRenderer: TBGRATextEffectFontRenderer;
      gold: TBGRAGradientScanner;
    begin
      ...
      fontRenderer:= TBGRATextEffectFontRenderer.Create;
      fontRenderer.ShadowVisible := true;     //adds a shadow
      fontRenderer.OutlineVisible := true;    //show the outline
      gold := TBGRAGradientScanner.Create(CSSGold,CSSGoldenrod,gtLinear,PointF(0,0),PointF(20,20),true,true);
      fontRenderer.OutlineTexture := gold;    //define the texture used for the outline
      fontRenderer.OuterOutlineOnly := true;  //just the outer pixels of the text
      bmp.FontRenderer := fontRenderer;       //gives the FontRenderer to the image (which becomes the owner of the renderer)
      ...
      gold.Free;
    end;

[![chartbgrafonteffect.png](https://wiki.freepascal.org/images/8/81/chartbgrafonteffect.png)](</File:chartbgrafonteffect.png>)

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial_TAChart](https://web.archive.org/web/20190825180854/https://wiki.freepascal.org/BGRABitmap_tutorial_TAChart)_
