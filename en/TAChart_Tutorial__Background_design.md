# TAChart Tutorial: Background design

│ **English (en)** │  **[suomi (fi)](</TAChart_Tutorial:_Background_design/fi> "TAChart Tutorial: Background design/fi")** │    
****

## Contents

  * 1 Introduction
  * 2 Preparation
    * 2.1 Data
    * 2.2 Setting up the chart
  * 3 The TChart painting events
    * 3.1 Showing the background image
    * 3.2 Showing a background gradient
    * 3.3 A note on Lazarus version 1.9 or newer
    * 3.4 Finishing up
  * 4 Summary
  * 5 Source code
    * 5.1 Project file
    * 5.2 Unit main.pas
    * 5.3 Form file main.lfm



## Introduction

[![tachart background finished.png](https://wiki.freepascal.org/images/8/83/tachart_background_finished.png)](</File:tachart_background_finished.png>)

Standard charts created by the TAChart package have a gray background. Using the `Color` property it is possibly to modify the color of the entire chart background, and the `BackColor` controls the color of the chart area enclosed by the axes, the so-called "back wall". In any case, the design is quite conventional compared to other charting packages and applications. Is it possible to give the chart background a more interesting design? 

In this tutorial, we will demonstrate that this is a relatively easy task by taking advantage of one or two events of TChart. We will create a plot charting changes in the number of Lazarus downloads per year over recent years. To highlight the connection with Lazarus we plan to show the Lazarus splash screen as a background image of the chart. 

## Preparation

### Data

Sourceforge provides a list of the Lazarus download counts per month. From <http://sourceforge.net/projects/lazarus/files/Lazarus%20Windows%2032%20bits/stats/timeline?dates=2003-01-01+to+2014-09-02> I extracted the download count per year for the windows-32 installation file: 

Year  | 2005  | 2006  | 2007  | 2008  | 2009  | 2010  | 2011  | 2012  | 2013   
---|---|---|---|---|---|---|---|---|---  
Downloads  | 53299  | 119613  | 158060  | 218915  | 190567  | 230108  | 267858  | 298335  | 280586   
  
These data will be basis of our chart. 

### Setting up the chart

Create a new project and add a `TChart` to the form. Client-align it such that the chart fills the entire form. Drag the form borders such that the window is about 400 pixels wide and 300 pixels high. 

We will plot the "Years" along the `x` axis, and the "Download count" along the `y` axis of the chart. Therefore, go to bottom axis, find the property `Title` and enter "Years" in the field for `Caption`. Show the title by setting its property `Visible` to `true`. Adapt other properties to your liking: I often prefer to have the title in bold font and larger size (12). And sometimes the grid is annoying, you can turn if off by setting the `Visible` property of the axis' `Grid` to `false`. Repeat with the vertical axis, the title should be named "Downloads per year". 

We should also show a title above the chart. Select the text "Lazarus downloads" for the `Text` of the chart's `Title`. Again, set the property `Visible` to `true` in order to show the title. You may also want to switch the title font to bold and increase its size a bit. 

[![tachart background no data.png](https://wiki.freepascal.org/images/1/18/tachart_background_no_data.png)](</File:tachart_background_no_data.png>) [![tachart background datapoints editor.png](https://wiki.freepascal.org/images/2/20/tachart_background_datapoints_editor.png)](</File:tachart_background_datapoints_editor.png>)

A good series type for time-series data is a bar series. At first we add a `TBarSeries` to the chart: Double-click on the chart to open the series editor, click "Add" and select "Bar series" from the dropdown list. We do not see the series because it has no data yet. 

There are various ways to assign data to the series. Since we have static data which do not change during the program it is convenient to use a `TListSource` which provides a datapoint editor for entering data at design-time. You find the ListChartSource in the "Chart" component palette, it is the second icon. In order to enter data click on the ellipsis button next to the property `DataPoints` of the component - this opens the DataPoints editor. Enter the data from the table above into the editor grid: the "year" goes into the "X" column, the "downloads" go into the "Y" column. There is no need to enter anything into the "Color" and "Text" columns. 

Finally, we connect the ListChartSource to the BarSeries. Each series has a property `Source`. Click on the dropdown arrow and select the ListChartSource from the list. Immediately the bar chart shows up. 

[![tachart background first version.png](https://wiki.freepascal.org/images/d/d3/tachart_background_first_version.png)](</File:tachart_background_first_version.png>)

## The TChart painting events

So far, the project has been standard. Now let's intercept the drawing process... 

`TChart` offers several events where our own code can be hooked in: 

  * At the beginning of the drawing pipeline there is `OnChartPaint`. This event is marked as experimental and can only be reached from code, not from the object inspector. It fires before anything is drawn by the chart and offers a `var` parameter `ADoDefaultDrawing`. Setting this one to `false`, therefore, by-passes the entire built-in drawing process and allows to paint the chart completely on your own. A very exciting feature, but too drastic for our purpose...
  * The next event happens before the background of the entire chart is drawn: `OnBeforeDrawBackground`. It, again, has the `ADoDefaultDrawing` parameter. If we set it to `false` we can replace the background drawn by the chart by our own procedures. Very good for our purpose. If left at `true` the chart will paint its standard background that we can control by the object inspector.
  * After the background has been painted - either the default or the custom one - there is another event, `OnAfterDrawBackground`. It can be used to paint something on the background which is always underneath the series or axes which are painted later.
  * Still in the very early stages, we have the `OnBeforeDrawBackwall` event. It fires before the area enclosed by the axis rectangle ("back wall") is filled by its background color/pattern. Taking advantage of the `ADoDefaultDrawing` parameter again we can replace the back wall by our own procedure. Exactly what we need!
  * Then the chart paints the titles, the axes, the grid, the series, and the legend. Unfortunately, there are no events to intercept their painting (except for the legend which provides an `OnDrawLegend` event). But we do have some control of the drawing process within a group of chart elements by means of the their `ZPosition` property. If, for example, you have a chart with a bar and a line series and do not want the line series to be partly covered by the bars you have to give the line series a greater `ZPosition` than the bar series - the element with the larger `ZPosition` is drawn later, i.e. on top of the element with the smaller `ZPosition`.
  * Only at the very end of the drawing process there is another event, `OnAfterDraw`. It is usually intended for administrative purposes, for example measuring the painting time by reading a clock that was started in the `OnChartPaint` event.



### Showing the background image

The Lazarus logo makes a nice background for our chart of Lazarus download counts. We want to paint it in the chart area enclosed by the axis, the back wall. As you saw above in the listing of chart event there is an event which perfectly fits our need: `OnBeforeDrawBackwall`. 

At first we have to make the logo available to the program. The logo file has the name "splash_logo.png" and can be found in the folder "images" of your Lazarus installation. For simplicity, copy it to the folder which will hold the exe file of our project (or use the full file path in the code below). Since the logo is a png file we need an instance of a TPortableNetworkGraphic, or - which is more flexible - a TPicture. Let's declare a variable `FBackImage` of the latter type to the form and add code in the form's OnCreate event to load the image file. Of course, don't forget to free the image when the program closes: 
    
    
    type
      TForm1 = class(TForm)
      ...
      private
        FBackImage: TPicture;
      ...
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      FBackImage := TPicture.Create;
      FBackImage.LoadFromFile('splash_logo.png');
      // or: FBackImage.LoadFromFile('c:\lazarus\images\splash_logo.png');
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      FBackImage.Free;
    end;
    

Loading the image directly from file requires that the image is distributed along with the exe file. This can be avoided if the image is added as a resource to the binary. For this purpose you have to create a resource file containing the image - see the wiki article .... on how to do that. The Lazarus images folder contains a ready-made resource file with the logo, "splash_logo.res". If you want to use this approach, copy this file to the project folder and use this alternative `OnCreate` code (don't forget to add `{$R splash_logo.res}` at the beginning of the `implemenation` section of the unit): 
    
    
    ...
    implementation
    
    {$R *.lfm}
    {$R splash_logo.res}
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      FBackImage := TPicture.Create;
      FBackImage.LoadFromResourceName(HInstance, 'splash_logo');
    end;
    

Whichever method you decide to use - now it's the time to add code to the `OnBeforeDrawBackwall` event of the chart. It is very simple: we just "stretch-draw" the image on the canvas into the rectangle provided as a parameter, and - of course - we set the `ADeDefaultDrawing` to `false` to by-pass the default painting method. It is important that you draw on the canvas provided as a parameter, not on the chart's canvas, because painting of the chart is executed by special backend classes which can provide different canvases, for example, if the chart is to be printed. 
    
    
    procedure TForm1.Chart1BeforeDrawBackWall(ASender: TChart; ACanvas: TCanvas;
      const ARect: TRect; var ADoDefaultDrawing: Boolean);
    begin
      ACanvas.StretchDraw(ARect, FBackImage.Graphic);
      ADoDefaultDrawing := false;
    end;
    

Because the event handler is run-time code you have to compile the program to see the current state of the chart: 

[![tachart background backwall.png](https://wiki.freepascal.org/images/4/49/tachart_background_backwall.png)](</File:tachart_background_backwall.png>)

### Showing a background gradient

In the outer region of the chart we still see the famous gray background. How about replacing it by a decent gradient? From the predominant blue colors of the Lazarus logo we could select a vertical transition from bright blue (`clSkyBlue`) at the top to white a the bottom. 

We know from the summary of painting events that the `OnBeforeDrawBackground` is the right event which we can use to draw the chart background on our own. But how to paint a gradient? Very easy: the canvas has a method `GradientFill` which accepts start and end colors and the gradient direction. 
    
    
    procedure TForm1.Chart1BeforeDrawBackground(ASender: TChart; ACanvas: TCanvas;
      const ARect: TRect; var ADoDefaultDrawing: Boolean);
    begin
      ACanvas.GradientFill(ARect, clSkyBlue, clWhite, gdVertical);
      ADoDefaultDrawing := false;
    end;
    

[![tachart background backwall gradient.png](https://wiki.freepascal.org/images/c/cd/tachart_background_backwall_gradient.png)](</File:tachart_background_backwall_gradient.png>)

### A note on Lazarus version 1.9 or newer

If you have Lazarus version 1.9 or newer you many notice that the events `OnBeforeDrawBackground` and `OnBeforeDrawBackWall` are marked as deprecated, it is recommended to use `OnBeforeCustomDrawBackground` and `OnBeforeCustomDrawBackWall`. This was introduced because the new events support the TAChart drawing backend architecture which enables drawing on devices which do not have an LCL-compatible canvas (e.g. an OpenGL rendering context, or an SVG file). A chart drawer essentially provides the same commands as TCanvas, however, some differences exist here and there - unfortunately also in drawing of images and gradients: Images are requested as descendants of TFPCustomImage. And gradients are not supported at all, but may be provided after painting on an auxiliary bitmap. 

Here is the code which works for the `OnBeforeCustomDrawBackWall` event: 
    
    
    procedure TForm1.Chart1BeforeCustomDrawBackWall(ASender: TChart;
      ADrawer: IChartDrawer; const ARect: TRect; var ADoDefaultDrawing: Boolean);
    var
      bmp: TBitmap;
      img: TLazIntfImage;
    begin
      img := TLazIntfImage.Create(0, 0);
      try
        bmp := TBitmap.Create;
        try
          bmp.SetSize(ARect.Right - ARect.Left, ARect.Bottom - ARect.Top);
          bmp.Canvas.StretchDraw(Rect(0, 0, bmp.Width, bmp.Height), FBackImage.Graphic);
          img.LoadFromBitmap(bmp.Handle, bmp.MaskHandle);
        finally
          bmp.Free;
        end;
        ADrawer.PutImage(ARect.Left, ARect.Top, img);
        ADoDefaultDrawing := false;
      finally
        img.Free;
      end;
    end;
    

The image painting command of the drawer is `PutImage`. It gets the image as a `TLazIntfImage` which, unlike TBitmap, is a descendant of TFPCustomImage. The originally loaded picture is stretch-drawn onto an auxiliary bitmap because the drawer does not support this operation. This auxiliary bitmap, finally is passed to the `LazIntfImage` via its method `LoadFromBitmap`. 

The `OnBeforeCustomDrawBackground` event handler follows the same principle. But now the auxiliary bitmap is painted with the gradient requested. 
    
    
    procedure TForm1.Chart1BeforeCustomDrawBackground(ASender: TChart;
      ADrawer: IChartDrawer; const ARect: TRect; var ADoDefaultDrawing: Boolean);
    var
      bmp: TBitmap;
      img: TLazIntfImage;
    begin
      img := TLazIntfImage.Create(0, 0);
      try
        bmp := TBitmap.Create;
        try
          bmp.SetSize(ARect.Right - ARect.Left, ARect.Bottom - ARect.Top);
          bmp.Canvas.GradientFill(Rect(0, 0, bmp.Width, bmp.Height), clSkyBlue, clWhite, gdVertical);
          img.LoadFromBitmap(bmp.Handle, bmp.MaskHandle);
        finally
          bmp.Free;
        end;
        ADrawer.PutImage(ARect.Left, ARect.Top, img);
        ADoDefaultDrawing := false;
      finally
        img.Free;
      end;
    end;
    

### Finishing up

The red bars of the series look a bit "aggressive". How about a more decent sky-blue again? Select the bar series in the object inspector and set the `SeriesColor` to `clSkyBlue`. 

And it would be better to see more of the Lazarus cheetah which is largely covered by the bars. The bar series has a property `Transparency`. The default value, `0`, means opaque, the maximum allowable value, `255` means fully transparent. A low value, such as `64`, adds some transparency to show more details of the cheetah, but does not push the bars too much in the background. 

And this brings us to the chart which is displayed at the top of this page, and to the end of this tutorial. 

## Summary

These are the basic steps for **user-painting of chart backgrounds** : 

  * Use the **`OnBeforeDrawBackWall` event** to provide your own painting procedure for the **area enclosed by the axis rectangle**.
  * Similarly, the **`OnBeforeDrawBackground` event** for painting of the entire **chart background**.
  * In these drawing procedures set **`ADoDefaultDrawing`** to **`false`** in order to call your own painting procedures.



## Source code

The source code of this tutorial project can be found in the folder _tutorials/background_ of trunk TAChart installations. 

### Project file
    
    
    program backimage;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Interfaces, // this includes the LCL widgetset
      Forms, main, tachartlazaruspkg
      { you can add units after this };
    
    {$R *.res}
    
    begin
      RequireDerivedFormResource := True;
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    end.
    

### Unit `main.pas`
    
    
    unit main;
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, TAGraph, TASeries, TASources, Forms, Controls,
      Graphics, Dialogs;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Chart1: TChart;
        Chart1BarSeries1: TBarSeries;
        ListChartSource1: TListChartSource;
        procedure Chart1BeforeDrawBackground(ASender: TChart; ACanvas: TCanvas;
          const ARect: TRect; var ADoDefaultDrawing: Boolean);
        procedure Chart1BeforeDrawBackWall(ASender: TChart; ACanvas: TCanvas;
          const ARect: TRect; var ADoDefaultDrawing: Boolean);
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
      private
        { private declarations }
        FBackImage: TPicture;
      public
        { public declarations }
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    //{$R splash_logo.res}
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      FBackImage := TPicture.Create;
    
      FBackImage.LoadFromFile('splash_logo.png');
      // this assumes that the logo is in the same folder as the binary
    
      // or, using resources:
      //FBackImage.LoadFromResourceName(HInstance, 'splash_logo');
      // Don't forget ths {$R directive above...
    end;
    
    procedure TForm1.Chart1BeforeDrawBackWall(ASender: TChart; ACanvas: TCanvas;
      const ARect: TRect; var ADoDefaultDrawing: Boolean);
    begin
      ACanvas.StretchDraw(ARect, FBackImage.Graphic);
      ADoDefaultDrawing := false;
    end;
    
    procedure TForm1.Chart1BeforeDrawBackground(ASender: TChart; ACanvas: TCanvas;
      const ARect: TRect; var ADoDefaultDrawing: Boolean);
    begin
      ACanvas.GradientFill(ARect, clSkyBlue, clWhite, gdVertical);
      ADoDefaultDrawing := false;
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      FBackImage.Free;
    end;
    
    end.
    

### Form file `main.lfm`
    
    
    object Form1: TForm1
      Left = 340
      Height = 300
      Top = 154
      Width = 400
      Caption = 'Form1'
      ClientHeight = 300
      ClientWidth = 400
      OnCreate = FormCreate
      OnDestroy = FormDestroy
      LCLVersion = '1.3'
      object Chart1: TChart
        Left = 0
        Height = 300
        Top = 0
        Width = 400
        AxisList = <    
          item
            Grid.Color = clSkyBlue
            Grid.Visible = False
            Marks.Format = '%.0n'
            Marks.Style = smsCustom
            Minors = <>
            Title.LabelFont.Height = -16
            Title.LabelFont.Orientation = 900
            Title.LabelFont.Style = [fsBold]
            Title.Visible = True
            Title.Caption = 'Downloads per year'
          end    
          item
            Grid.Visible = False
            Alignment = calBottom
            Minors = <>
            Title.LabelFont.Height = -16
            Title.LabelFont.Style = [fsBold]
            Title.Visible = True
            Title.Caption = 'Year'
          end>
        Foot.Brush.Color = clBtnFace
        Foot.Font.Color = clBlue
        Title.Brush.Color = clBtnFace
        Title.Brush.Style = bsClear
        Title.Font.Color = clNavy
        Title.Font.Height = -17
        Title.Font.Style = [fsBold]
        Title.Text.Strings = (
          'Lazarus Downloads'
        )
        Title.Visible = True
        OnBeforeDrawBackground = Chart1BeforeDrawBackground
        OnBeforeDrawBackWall = Chart1BeforeDrawBackWall
        Align = alClient
        ParentColor = False
        object Chart1BarSeries1: TBarSeries
          Shadow.Color = clNavy
          Shadow.OffsetX = 4
          BarBrush.Color = clSkyBlue
          Source = ListChartSource1
        end
      end
      object ListChartSource1: TListChartSource
        DataPoints.Strings = (
          '2005|53299|?|'
          '2006|119613|?|'
          '2007|158060|?|'
          '2008|218915|?|'
          '2009|190567|?|'
          '2010|230108|?|'
          '2011|267858|?|'
          '2012|298335|?|'
          '2013|280586|?|'
        )
        left = 120
        top = 56
      end
    end

---

_Source: [https://wiki.freepascal.org/TAChart_Tutorial%3A_Background_design](https://web.archive.org/web/20250122120303/https://wiki.freepascal.org/TAChart_Tutorial%3A_Background_design)_
