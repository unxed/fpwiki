# TAChart Tutorial: ColorMapSeries, Zooming

│ **English (en)** │  **[suomi (fi)](</TAChart_Tutorial:_ColorMapSeries,_Zooming/fi> "TAChart Tutorial: ColorMapSeries, Zooming/fi")** │    
****

## Contents

  * 1 Introduction
  * 2 Using TColormapSeries
  * 3 Setting up the color map
  * 4 The Mandelbrot set
    * 4.1 Calculation
    * 4.2 Drawing
  * 5 Zooming and panning
    * 5.1 Built-in zooming and unzooming
    * 5.2 Extents
    * 5.3 Zooming towards infinity...
    * 5.4 Using ChartTools
    * 5.5 Zoom history
  * 6 Source code
    * 6.1 project1.lpr
    * 6.2 main.pas
    * 6.3 main.lfm
  * 7 Related tutorials



## Introduction

[![Mandelbrot final.png](https://wiki.freepascal.org/images/0/02/Mandelbrot_final.png)](</File:Mandelbrot_final.png>)

The ["Mandelbrot set"](<http://en.wikipedia.org/wiki/Mandelbrot_set%7C>), named after Benoit Mandelbrot, is a so-called "fractal" -- a two-dimensional shape that is famous for its self-similarity at various scales. Magnifying a fractal reveals small-scale details similar to the large-scale characteristics. 

In this tutorial, we will use [TAChart](<TAChart.md> "TAChart") to draw the Mandelbrot set by means of a **colormap series** and practice a variety of zooming techniques, like using the **zoom drag tool** , or setting up an **extent history**. 

As usual, youl'll require a basic knowledge of working with TAChart which you can aquire by going through the ["Getting Started" tutorial](<TAChart_Tutorial__Getting_started.md> "TAChart Tutorial: Getting started"). And, of course, you must be familiar with the Lazarus IDE and the Object Pascal language. 

## Using TColormapSeries

What is a [`TColorMapSeries`](<TAChart_documentation.md> "TAChart documentation")? 

The series that you typically work with are for two-dimensional data -- there is one axis for the _x_ coordinates, and another one for the _y_ values which are assigned to the _x_ values. `TColorMapSeries` is one exception: it displays three-dimensional data: we have a point in the _xy_ plane and assign to it a third value as its height above the _xy_ plane. Often, 3D plots, are considered to be like a mountain landscape above a base plane. Unfortunately, TAChart cannot draw 3D projections. But there is an alternative: the height above the base plane can be can be mapped to a color, this means the chart fills the xy plane with colors which correspnd to the value of the third coordinate. 

`TColorMapSeries` is a functional series, this means that it cannot plot arbitrary data, but only data that can be calculated from a function. For this purpose, the series exposes an event `OnCalculate` which is called for every (_x_ ,_y_) point to pass the function value. In this sense, the `TColorMapSeries` is similar to the `TFuncSeries` which we meet in [another tutorial](<TAChart_Tutorial__Function_Series.md> "TAChart Tutorial: Function Series"). 

The second basic ingredient is a `ColorSource` which maps the function values to a color. It is convenient to use a `TListChartSource` for this purpose. In general, it stores _x_ and _y_ values along with a color value and a descriptive label in `TChartDataItem`s, but for the mapping purpose we need only the _x_ and color entries. 

Ready to start? 

Create a new project, and add a client-aligned `TChart` component. Don't make the window too large since the project in this tutorial will require a _lot_ of computation, and too many pixels will kill performance... 

Double-click on the chart to open the series editor and add a _"Color map series"_ to the chart. Next, drop a `TListChartSource` to the form. This will be the color source for the ColorMapSeries. So, name it "`ColorSource`" and assign it to the corresponding property of the series. 

[![Madelbrot ColorMapSeries.png](https://wiki.freepascal.org/images/3/32/Madelbrot_ColorMapSeries.png)](</File:Madelbrot_ColorMapSeries.png>)

## Setting up the color map

To get some familiarity in this new terrain, let us begin with a simple example: we use the ColorMapSeries to draw a gradient along the _x_ axis. 

At first, we write an event handler for the ColorMapSeries' `OnCalculate`. This is easy since we want only a simple linear gradient: 
    
    
    procedure TForm1.Chart1ColorMapSeries1Calculate(const AX, AY: Double;
      out AZ: Double);
    var
      ext: TDoubleRect;
    begin
      ext := Chart1.GetFullExtent;
      AZ := (AX - ext.a.x) /(ext.b.x - ext.a.x);
    end;
    

To be independent of the size of the chart we normalize the _x_ coordinate to the range of _x_ data (in [graph units](<TAChart_documentation.md> "TAChart documentation")). The _x_ range can be calculated from the [`FullExtent`](<TAChart_documentation.md> "TAChart documentation") of the chart -- this is a rectangle of double-precision values defined by the corner points of the chart's plotting rectangle before any zooming or panning is applied. The normalization as shown above makes sure that AZ is 0 at the left and 1 at the right end of the _x_ axis. 

Finally we have to populate the ColorSource to setup the gradient. This is very easy since the ColorMapSeries does all the interpolations if its property `Interpolate` is set to `true`. 

To define the gradient we begin with the value 0.0 and assign it to the color blue. The end of the gradient corresponds to the value 1.0 and it should appear in yellow. We make these assignments in a procedure `PopulateColorSource` that we call from the `OnCreate` event handler of the form; we use a separate procedure since we'll do some modifications throughout this tutorial and want to keep things separate. 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      PopulateColorSource;
    end;  
    
    procedure TForm1.PopulateColorSource;
    const
      DUMMY = 0.0;
    begin
      with ColorSource do begin
        Add(0.0, DUMMY, '', clBlue);      // 0.0 --> blue
        Add(1.0, DUMMY, '', clYellow);    // 1.0 --> yellow
      end;
    end;
    

Due to the syntax of `TListChartSource.Add` we also have to enter values for the _y_ coordinate and the marks text, we use the constant `DUMMY` and an empty string since we don't need them. 

Let's run the program. If you don't see a gradient you may have forgotten to activate the `Interpolate` setting of the ColorMapSource. If you look carefully you will see some narrow banding of the color shades particularly in the center transition region. This is due to the `StepX` and `StepY` properties of the ColorMapSource which are still at their default value of 4. This means that the xy plane is not scanned pixel by pixel, but in blocks of 4x4 pixels, to speed up drawing. Set these values to 1 to get rid of the banding. The improved resolution is also advantageous for the Mandelbrot set later. However, it will slow down program execution which may be quite dramatic if you have a slow computer... 

[![Madelbrot FirstGradient.png](https://wiki.freepascal.org/images/5/52/Madelbrot_FirstGradient.png)](</File:Madelbrot_FirstGradient.png>) [![Mandelbrot TwoGradients.png](https://wiki.freepascal.org/images/f/fc/Mandelbrot_TwoGradients.png)](</File:Mandelbrot_TwoGradients.png>)

Let's go a step further and add another pivot to our gradient. What about red in the middle corresponding to, say, 0.3? Just put another call to `Add` into the `PopulateColorSource` procedure. Since the interpolation requires a sorted list you should add the new color between the other ones, or you should call `Sort` after adding color-value pairs. The result of this tree-color gradient is shown above, we will use it now when we draw the Mandelbrot set. 
    
    
    procedure TForm1.PopulateColorSource;
    const
      DUMMY = 0.0;
    begin
      with ColorSource do begin
        Add(0.0, DUMMY, '', clBlue);      // 0.0 --> blue
        Add(0.3, DUMMY, '', clRed);       // 0.3 --> red
        Add(1.0, DUMMY, '', clYellow);    // 1.0 --> yellow
      end;
    end;
    

## The Mandelbrot set

So what is the Mandelbrot set? Roughly speaking, it is a set of points in the _xy_ plane that follow a certain rule, like a circle which has the rule that all points on the circle (i.e. in the set) must have the same distance from the origin. 

For the Mandelbrot set the rule is a bit more complex. The world "complex" is to be understood here in a two-fold sense: complex in the sense of "complicated", but also complex in the mathematical sense that the points in the _xy_ plane are treated as complex numbers. If you are not familiar with complex numbers, don't give up -- we will avoid calculations with these "strange" numbers completely. 

### Calculation

The best way to understand the Mandelbrot set is do its construction. We apply the following recipe for each 2d point in the _xy_ plane; these starting points will be called c = (cx, cy). 

  1. Begin by assigning c to another point z = (zx, zy) = (cx, cy).
  2. Calculate the "square" of z according to the following formula: z2 = (zx2 \- zy2, 2 * zx * zy) -- this formula may appear a bit strange, but if you are familiar with complex numbers then you will notice that it is the way how to calculate the square of a complex number. If you are not just take it as given.
  3. Then add the coordinates of c to those of `z2`. The result of this step is z2 \+ c = (zx2 \- zy2 \+ cx, 2 * zx * zy \+ cy)
  4. Now take that result and put it into the calculation in step 2 again as a new value of z.
  5. Repeat this procedure again and again. As a result the point z will follow some kind of trajectory in the _xy_ plane. In the figure we show some examples of that trace: 
     * [![Mandelbrot Tracks.png](https://wiki.freepascal.org/images/a/a6/Mandelbrot_Tracks.png)](</File:Mandelbrot_Tracks.png>)

The red and fuchsia curves eventually move away from the origin, these paths are **"unbounded"**. It can be shown that once the path has crossed a critical distance of 2 from the origin it will never return back and will escape into infinity. Usually the calculation counts the iterations until the distance from the origin exceeds 2. The number of iterations is mapped to a color which is used to draw the pixel at the starting point c.
     * The blue path, on the other hand, converges towards the origin. The green curve does not converge, but remains within the escape radius. Both cases are called **"bounded"**. The iterative calculation would go on forever. Therefore it is stopped after a maximum count of iterations. The starting points c for these trajectories are said to belong to the Mandelbrot set and will be drawn in black color.



Although this description may sound a bit complicated only a few lines of Pascal code are required to do the calculation. The following function determines whether a point c is in the Mandelbrot set or not. It returns the number of iterations needed and the coordinates of the point after the last iteration performed. 
    
    
    const
      MANDELBROT_NUM_ITERATIONS = 100;
      MANDELBROT_ESCAPE_RADIUS = 2.0;
      MANDELBROT_LIMIT = sqr(MANDELBROT_ESCAPE_RADIUS);     
    
    function InMandelbrotSet(c:TDoublePoint; out Iterations:Integer; out z: TDoublePoint): Boolean;
    var
      j: Integer;
    begin
      Iterations := 0;
      z := DoublePoint(0.0, 0.0);
      for j:=0 to MANDELBROT_NUM_ITERATIONS-1 do begin
        z := DoublePoint(
          sqr(z.X) - sqr(z.Y) + c.X,
          2 * z.X * z.Y + c.Y
        );
        if sqr(z.X) + sqr(z.Y) > MANDELBROT_LIMIT then begin
          Result := false;
          // point did escape --> c is not in Mandelbrot set
          exit;
        end;
        inc(Iterations);
      end;
      Result := true;
    end;
    

### Drawing

Now we want to draw the Mandelbrot set. You know what to do? Yes - we have to write a corresponding handler for `OnCalculate`. In this event handler we call our `InMandelbrotSet` procedure. If the point passed to the event handler is in the Mandelbrot set we set its color to black, if not we divide the number of iterations by the maximum number of iterations to get a value between 0 and 1 which has a corresponding entry in our ColorSource. Oh - but how can we give the points in the Mandelbrot set a black color? Just add another number-color pair to the ColorSource which links `clBlack` to, e.g., -1. 
    
    
    procedure TForm1.PopulateColorSource;
    const
      DUMMY = 0.0;
    begin
      with ColorSource do begin
        Clear;
        Add(-1.0, DUMMY, '', clBlack);
        Add( 0.0, DUMMY, '', clBlue);
        Add( 0.3, DUMMY, '', clRed);
        Add( 1.0, DUMMY, '', clYellow);
      end;
    end;
    
    procedure TForm1.Chart1ColorMapSeries1Calculate(const AX, AY: Double;
      out AZ: Double);
    var
      iterations: Integer;
      z: TDoublePoint;
    begin
      if InMandelBrotSet(DoublePoint(AX, AY), iterations, z) then
        AZ := -1
      else
        AZ := iterations / MANDELBROT_NUM_ITERATIONS;
    end;
    

And that's what we get - see the left image: 

[![Madelbrot FirstChart.png](https://wiki.freepascal.org/images/e/eb/Madelbrot_FirstChart.png)](</File:Madelbrot_FirstChart.png>) [![Mandelbrot SecondChart.png](https://wiki.freepascal.org/images/9/9e/Mandelbrot_SecondChart.png)](</File:Mandelbrot_SecondChart.png>)

Very nice. But as usual the first result is not yet the best one. These are two points that we can improve: 

  * The picture is **clipped**. This is because we did not define the calculation range that the ColorMapSeries occupies for _x_ and _y_. The overview of the Mandelbrot set is seen best when _x_ and _y_ range between -2.2 and 0.8, and between -1.5 and 1.5, respectively. Enter these numbers in the chart's `Extent` fields `XMin, XMax, YMin` and `YMax`, and set the fields `UseXMin, UseXMax, UseYMin` and `UseYMin` to `true` to activate these values as axis limits. You might as well use the `Extent` property of the series, but then you would have to set the chart's `Margins` to 0 in order to remove the non-filled background near the axes.


  * The picture looks somehow **distorted** if you compare it with the images in [[1]](<http://en.wikipedia.org/wiki/Mandelbrot_set>). This happens because _x_ and _y_ axes have different unit lengths. To avoid the distortion the distance between 0 and 1 must be equal for both axes. Of course, we could initialize the width and height of the form correctly, but the user could resize the form, and this would distort our image again. However, `TChart` has a practical property to enforce a "square" coordinate system: set `Proportional` to `true`. With this setting the extent of the _x_ and _y_ axes is adjusted automatically to always get equal units.



The result after these modifications is displayed above in the right image. You maybe notice that the _x_ axis range is wider than we had requested. This is a consequence of the `Proportional` setting since the aspect ratio of our window is not compatible with the _x_ and _y_ ranges set in the chart's `Extent`. 

Before we continue with the tutorial, here's a piece of "homework": Modify the program such that the Mandelbrot set is not black, but shows colors according to the distance of the trajectory from the origin when the iteration is aborted. 

## Zooming and panning

It was mentioned in the introduction that the Mandelbrot set is self-similar at magnified scales. This calls for zooming -- and this will be the subject of the next part of the tutorial. 

### Built-in zooming and unzooming

`TChart` has built-in zooming capabilities. You just have to drag a rectangle with the mouse from the top-left to the bottom-right corner of the region that you want to magnify and hold the left mouse button down while dragging. To restore the old viewport drag in any other direction, or just click into the image. 

[![Mandelbrot Zooming.png](https://wiki.freepascal.org/images/2/23/Mandelbrot_Zooming.png)](</File:Mandelbrot_Zooming.png>) [![Mandelbrot ZoomingResult.png](https://wiki.freepascal.org/images/f/f4/Mandelbrot_ZoomingResult.png)](</File:Mandelbrot_ZoomingResult.png>) [![Mandelbrot DeepZoom.png](https://wiki.freepascal.org/images/3/34/Mandelbrot_DeepZoom.png)](</File:Mandelbrot_DeepZoom.png>)

Of course, you can use the new magnified image to zoom again and again. However, at some zoom level, the axis labels will get longer and longer and may eventually overlap. This does not look nice. Do we need the axes at all? No -- let's turn them off by going to the left and the bottom axis and setting their property `Visible` to `false`. 

But maybe we should have some indication on the size of the current viewport. We could display the current magnification in a label. In order to have space for other info labels later in this tutorial, we'll use a panel as a container for the label. Align this "info panel" to the right and delete its `Caption`. You may want to resize the form to get a sufficiently large chart again. Add the label to the top of the panel and name it `LblMagnification`. 

### Extents

How do we get the current magnification? The viewport is described in TAChart by several ["extents"](<TAChart_documentation.md> "TAChart documentation"); these are rectangles like `TRect`, but the corner points are given by floating point numbers. This data type is called `TDoubleRect` and is declared in unit `TAChartUtils`. We already had discussed the function `GetFullExtent` of `TChart` which returns the unzoomed/unpanned viewport. After zooming or panning the visible viewport can be accessed by `CurrentExtent` or `LogicalExtent`. Both rectangles are very similar, the former one is slightly larger than the latter one since it also takes care of the space needed for the axis tick marks and the inner chart margins. Because this extra space is not considered in the full extent, we have to compare the widths of the full extent to that of the logical extent to calculate the magnification. We need not consider the extent heights because of the `Proportional` setting of the chart. 

It should be noted that the extents are always given in [graph units](<TAChart_documentation.md> "TAChart documentation"), i.e. the underlying coordinate system of the chart. Since we do not use [`TChartTransformations`](<TAChart_documentation.md> "TAChart documentation") this is the same as the coordinate system displayed at the axes (axes coordinates). 

When do we update the magnification label? Whenever the extent of the chart changes. `TChart` provides two events for this occasion: `OnExtentChanged` and `OnExtentChanging`. Both events are fired when the extent already has changed, OnExtentChanged is called from within the drawing routine, `OnExtentChanging` is called before any drawing. The difference is important if your event handler would initiate a re-draw of the chart again -- this might lock your application... In our application the difference is not important, so we'll vote against `OnExtentChanging` which is not yet available in the official Lazarus version. 
    
    
    procedure TForm1.Chart1ExtentChanged(ASender: TChart);
    var
      cex, fex: TDoubleRect;
      factor: double;
    begin
      cex := Chart1.CurrentExtent;
      fex := Chart1.GetFullExtent;
      if cex.b.x = cex.a.x then exit;
    
      factor := (fex.b.x - fex.a.x) / (cex.b.x - cex.a.x);
      if factor > 1e6 then
        LblMagnification.Caption := Format('Magnification: %.0e', [factor])
      else
        LblMagnification.Caption := Format('Magnification: %0.n', [factor]);
    end;
    

[![Mandelbrot ExtremeMag.png](https://wiki.freepascal.org/images/2/25/Mandelbrot_ExtremeMag.png)](</File:Mandelbrot_ExtremeMag.png>)

This is a possible version of the event handler. Since very large magnification factors will be difficult to read we switch to exponential numeric notation above 1 million. 

### Zooming towards infinity...

Play with the magnifications -- the image on the right is magnified by more than a 1 billion times (1E9). And when you are lucky you'll find locations where you can zoom to magnifications of 1E14 before the image gets ugly. Hold on for a moment and think about this number: if the original, non-zoomed window is 10 cm across -- then it would measure 1E13 meters at this magnification. This is the **size of the solar system**! It does not make sense to zoom any further because we reached the limit of double precision number used by TAChart internally. However, theoretically, it could go on and on and on... 

### Using ChartTools

Maybe you want to shift the viewport a bit to the left or right at the same magnification. This operation is called **panning**. In the trunk version of Lazarus, you can pan the image by dragging it with the right mouse button. In the release version there is not built-in panning support. But you can achieve panning by of the so-called [ChartTools](<TAChart_documentation.md> "TAChart documentation"). Most of all, these tools are important because they give better control of the zooming and panning process. 

We will not go into details of what ChartTools are and what they can do in general. Refer to the [corresponding tutorial](<TAChart_Tutorial__Chart_Tools.md> "TAChart Tutorial: Chart Tools") for an introduction if they are not familiar to you. 

[![TChartToolset-Icon.png](https://wiki.freepascal.org/images/4/40/TChartToolset-Icon.png)](</File:TChartToolset-Icon.png>)

To begin with, add a ChartToolSet component to the form and link it to the property `Toolset` of the chart. Then double-click on the toolset and add a [_Panning by drag_](<TAChart_documentation.md> "TAChart documentation") tool. Select the tool in the object inspector -- it is a child of the ChartToolset in the object tree -- and set its property `Shift` to `ssRight`. This will be the mouse button to activate the panning action. You may combine other keys in the `Shift` property if you like. 

Since the built-in tools are turned off by using a ChartToolset you also have to restore the zooming capabilities. -- Just the same procedure: add a [_Zoom by drag_](<TAChart_documentation.md> "TAChart documentation") and set its `Shift` property to `ssLeft`. 

If you run the program now you can pan even if you have the official Lazarus version. 

### Zoom history

For those who use the trunk version we can add another gadget, a **zoom history**. Since this was added to TAChart only recently, users of the official version are out of luck, they must skip this section - sorry... 

If you zoom deeper and deeper into the Mandelbrot set you may want to go one or more steps back to investigate details that are no longer visible at the current zoom level. The zoom history stores all the extents that once were active during the entire zooming and panning process. 

The units `TATypes` and `TAChartUtils` provide a class `TAChartExtentHistory` which allows to implement a zoom history. Since this is not a component it must be used by code. We create and destroy it in the `FormCreate` and `FormDestroy` events as the zoom history is needed throughout the entire program: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      PopulateColorSource;
      Chart1ColorMapseries1.ColorSource := ColorSource;
      // this is new:
      ZoomHistory := TChartExtentHistory.Create;
      ZoomHistory.Capacity := 100;
      // our history can capture 100 items until the oldest item is dropped.
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      ZoomHistory.Free;
    end;
    

Whenever zooming or panning events occur the visible viewport of the chart must be added to the history. Let's begin with zooming where we use the `AfterMouseUp` event of the ZoomDragTool for simplicity. We could also take the more general `OnExtentChanged` event of the chart, but here we have to be careful to skip the extent changes caused by unzooming -- please study the _extent demo_ that comes with TAChart for details. 

Note that the extent already has changed when the `AfterMouseUp` event fires. Therefore, the extent to be added to the history is the `PrevLogicalExtent` of the chart -- this is the viewport that was active before the extent was changed: 
    
    
    procedure TForm1.ChartToolset1ZoomDragTool1AfterMouseUp(ATool: TChartTool;
      APoint: TPoint);
    begin
      ZoomHistory.Add(Chart1.PrevLogicalExtent);
    end;
    

Panning is a bit more tricky because the extent changes occur while moving the mouse. At the moment of MouseUp the `PrevLogicalExtent` that was active when panning started has been overwritten many times. But the solution is simple: The PanDragTool can use above code in the `OnAfterMouseDown` event handler. 

It would be interesting to show in our info panel also the current zoom/pan level, this is the count of items in the history. The `TChartExtentHistory` has a corresponding property `Count` for that. So, let's add another label ("`LblHistoryCount`") to the info panel and modify the `OnExtentChanged` event handler of the chart by adding the following line: 
    
    
      LblHistoryCount.Caption := Format('History count: %d', [ZoomHistory.Count]);
    

And how to we **restore the previous zoom level**? By using one more ChartTool. We don't have any special requirements, so we take a [`UserDefinedChartTool`](<TAChart_documentation.md> "TAChart documentation"). Set its `Shift` property such that the tool reacts on a click of the middle mouse button. And in the `OnAfterMouseClick` event handler we read the last item from the history (`ZoomHistory.Pop`) and assign the extent rectangle to the chart's `LogicalExtent` which adjusts the viewport accordingly. The item is removed from the history stack after reading. The history count display in the info panel adjusts automatically because the event `OnExtentChanged` is triggered. Note that we have to check for an empty history to avoid an exception. 
    
    
    procedure TForm1.ChartToolset1UserDefinedTool1AfterMouseUp(ATool: TChartTool;
      APoint: TPoint);
    begin
      if ZoomHistory.Count > 0 then
        Chart1.LogicalExtent := ZoomHistory.Pop;
    end;
    

If you want to quickly **return to the unzoomed view at magnification 1** you can drag with the left mouse button in any direction other than left-top to right-bottom, or just click into the chart. Since we introduced the middle mouse button for unzooming the user interface may be more consistent if we'd add another [`UserDefinedChartTool`](<TAChart_documentation.md> "TAChart documentation") and assign its `Shift` property to `ssMiddle` and `ssShift` \-- I associate the `⇧ Shift` key with the action of making something bigger, like lowercase to uppercase characters. Therefore, the combination of the middle key for unzooming with the `⇧ Shift` key could be remembered to trigger unzooming in a big step. 

Nevertheless, it may be difficult for the user to keep in mind all the key combinations. Therefore, add to our info panel a few more labels in which you give **instructions on zooming, panning and unzooming**. 

So, here is the final result of this tutorial project. Have fun! 

[![Mandelbrot final.png](https://wiki.freepascal.org/images/0/02/Mandelbrot_final.png)](</File:Mandelbrot_final.png>)

## Source code

The sources of the tutorial project can be found in the folder _tutorial\mandelbrot_ of the trunk TAChart installation. 

### project1.lpr
    
    
    program mandelbrot;
     
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
    

### main.pas
    
    
    unit main;
     
    {$mode objfpc}{$H+}
     
    interface
     
    uses
      Classes, ExtCtrls, StdCtrls, SysUtils, TAGraph, TAFuncSeries,
      TASources, Forms, Controls, Graphics, Dialogs, TATypes, TATools, Types;
     
    type
     
      { TForm1 }
     
      TForm1 = class(TForm)
        Chart1: TChart;
        Chart1ColorMapSeries1: TColorMapSeries;
        ChartToolset1: TChartToolset;
        ChartToolset1PanDragTool1: TPanDragTool;
        ChartToolset1UserDefinedTool1: TUserDefinedTool;
        ChartToolset1UserDefinedTool2: TUserDefinedTool;
        ChartToolset1ZoomDragTool1: TZoomDragTool;
        ColorSource: TListChartSource;
        Label1: TLabel;
        Label2: TLabel;
        Label3: TLabel;
        Label4: TLabel;
        Label5: TLabel;
        Label6: TLabel;
        Label7: TLabel;
        Label8: TLabel;
        Label9: TLabel;
        LblMagnification: TLabel;
        LblHistoryCount: TLabel;
        Panel1: TPanel;
        Panel2: TPanel;
        Panel3: TPanel;
        procedure Chart1ColorMapSeries1Calculate(const AX, AY: Double;
          out AZ: Double);
        procedure Chart1ExtentChanged(ASender: TChart);
        procedure ChartToolset1PanDragTool1AfterMouseDown(ATool: TChartTool;
          APoint: TPoint);
        procedure ChartToolset1UserDefinedTool1AfterMouseUp(ATool: TChartTool;
          APoint: TPoint);
        procedure ChartToolset1UserDefinedTool2AfterMouseUp(ATool: TChartTool;
          APoint: TPoint);
        procedure ChartToolset1ZoomDragTool1AfterMouseUp(ATool: TChartTool;
          APoint: TPoint);
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
      private
        ZoomHistory: TChartExtentHistory;
        procedure PopulateColorSource;
      end;
     
    var
      Form1: TForm1;
     
    implementation
     
    {$R *.lfm}
     
    uses
      StrUtils,
      TAChartUtils, TAGeometry;
     
    const
      MANDELBROT_NUM_ITERATIONS = 100;
      MANDELBROT_ESCAPE_RADIUS = 2.0;
      MANDELBROT_LIMIT = sqr(MANDELBROT_ESCAPE_RADIUS);
     
    function InMandelBrotSet(
      AC: TDoublePoint; out AIterations: Integer; out AZ: TDoublePoint): Boolean;
    var
      j: Integer;
    begin
      AIterations := 0;
      AZ := DoublePoint(0.0, 0.0);
      for j := 0 to MANDELBROT_NUM_ITERATIONS - 1 do begin
        AZ := DoublePoint(
          Sqr(AZ.X) - Sqr(AZ.Y) + AC.X,
          2 * AZ.X * AZ.Y + AC.Y);
        if Sqr(AZ.X) + Sqr(AZ.Y) > MANDELBROT_LIMIT then
          // point did escape --> AC is not in Mandelbrot set
          exit(false);
        AIterations += 1;
      end;
      Result := true;
    end;
     
    { TForm1 }
     
    procedure TForm1.FormCreate(Sender:TObject);
    begin
      PopulateColorSource;
      ZoomHistory := TChartExtentHistory.Create;
      ZoomHistory.Capacity := 100;
    end;
     
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      ZoomHistory.Free;
    end;
     
    procedure TForm1.Chart1ColorMapSeries1Calculate(
      const AX, AY: Double; out AZ: Double);
    var
      iterations: Integer;
      z: TDoublePoint;
    begin
      if InMandelBrotSet(DoublePoint(AX, AY), iterations, z) then
        AZ := -1
        // or - as a solution to the "homework" exercise: 
        // AZ := sqrt(sqr(z.x) + sqr(z.y)) / MANDELBROT_ESCAPE_RADIUS
      else
        AZ := iterations / MANDELBROT_NUM_ITERATIONS;
    end;
     
    procedure TForm1.Chart1ExtentChanged(ASender: TChart);
    var
      cex, fex: TDoubleRect;
      factor: Double;
    begin
      cex := ASender.CurrentExtent;
      fex := ASender.GetFullExtent;
      if cex.b.x = cex.a.x then exit;
    
      factor := (fex.b.x - fex.a.x) / (cex.b.x - cex.a.x);
      LblMagnification.Caption :=
        'Magnification: ' + Format(IfThen(factor > 1e6, '%.0e', '%0.g'), [factor]);
      LblHistoryCount.Caption := Format('History count: %d', [ZoomHistory.Count]);
    end;
    
    procedure TForm1.ChartToolset1PanDragTool1AfterMouseDown(
      ATool: TChartTool; APoint: TPoint);
    begin
      Unused(ATool, APoint);
      ZoomHistory.Add(Chart1.PrevLogicalExtent);
    end;
     
    procedure TForm1.ChartToolset1UserDefinedTool1AfterMouseUp(
      ATool: TChartTool; APoint: TPoint);
    begin
      Unused(ATool, APoint);
      if ZoomHistory.Count > 0 then
        Chart1.LogicalExtent := ZoomHistory.Pop;
    end;
     
    procedure TForm1.ChartToolset1UserDefinedTool2AfterMouseUp(
      ATool: TChartTool; APoint: TPoint);
    begin
      Unused(ATool, APoint);
      Chart1.ZoomFull;
    end;
     
    procedure TForm1.ChartToolset1ZoomDragTool1AfterMouseUp(
      ATool: TChartTool; APoint: TPoint);
    begin
      Unused(ATool, APoint);
      ZoomHistory.Add(Chart1.PrevLogicalExtent);
    end;
     
    procedure TForm1.PopulateColorSource;
    const
      DUMMY = 0.0;
    begin
      with ColorSource do begin
        Clear;
        Add(-1.0, DUMMY, '', clBlack);
        Add( 0.0, DUMMY, '', clBlue);
        Add( 0.3, DUMMY, '', clRed);
        Add( 1.0, DUMMY, '', clYellow);
      end;
    end;
    
    end.
    

### main.lfm
    
    
    object Form1: TForm1
      Left = 326
      Height = 285
      Top = 155
      Width = 468
      Caption = 'Form1'
      ClientHeight = 285
      ClientWidth = 468
      OnCreate = FormCreate
      OnDestroy = FormDestroy
      LCLVersion = '1.1'
      object Chart1: TChart
        Left = 4
        Height = 277
        Top = 4
        Width = 288
        AxisList = <    
          item
            Visible = False
            Minors = <>
            Title.LabelFont.Orientation = 900
          end    
          item
            Visible = False
            Alignment = calBottom
            Minors = <>
          end>
        Extent.UseXMax = True
        Extent.UseXMin = True
        Extent.UseYMax = True
        Extent.UseYMin = True
        Extent.XMax = 0.8
        Extent.XMin = -2.2
        Extent.YMax = 1.5
        Extent.YMin = -1.5
        Foot.Brush.Color = clBtnFace
        Foot.Font.Color = clBlue
        Proportional = True
        Title.Brush.Color = clBtnFace
        Title.Font.Color = clBlue
        Title.Text.Strings = (
          'TAChart'
        )
        Toolset = ChartToolset1
        OnExtentChanged = Chart1ExtentChanged
        Align = alClient
        BorderSpacing.Around = 4
        DoubleBuffered = True
        ParentColor = False
        object Chart1ColorMapSeries1: TColorMapSeries
          ColorSource = ColorSource
          Interpolate = True
          OnCalculate = Chart1ColorMapSeries1Calculate
          StepX = 1
          StepY = 1
        end
      end
      object Panel1: TPanel
        Left = 296
        Height = 285
        Top = 0
        Width = 172
        Align = alRight
        BevelOuter = bvNone
        ClientHeight = 285
        ClientWidth = 172
        TabOrder = 1
        object Panel2: TPanel
          Left = 0
          Height = 213
          Top = 72
          Width = 172
          Align = alClient
          BevelOuter = bvNone
          ClientHeight = 213
          ClientWidth = 172
          TabOrder = 0
          object Label2: TLabel
            Left = 6
            Height = 13
            Top = 8
            Width = 69
            Caption = 'Instructions'
            Font.Style = [fsBold]
            ParentColor = False
            ParentFont = False
          end
          object Label1: TLabel
            Left = 6
            Height = 13
            Top = 37
            Width = 45
            Caption = 'Left-drag'
            Font.Style = [fsItalic]
            ParentColor = False
            ParentFont = False
          end
          object Label3: TLabel
            Left = 6
            Height = 13
            Top = 80
            Width = 53
            Caption = 'Middle-click'
            Font.Style = [fsItalic]
            ParentColor = False
            ParentFont = False
          end
          object Label4: TLabel
            Left = 6
            Height = 13
            Top = 120
            Width = 97
            Caption = 'Middle-click w/SHIFT'
            Font.Style = [fsItalic]
            ParentColor = False
            ParentFont = False
          end
          object Label5: TLabel
            Left = 6
            Height = 13
            Top = 161
            Width = 51
            Caption = 'Right-drag'
            Font.Style = [fsItalic]
            ParentColor = False
            ParentFont = False
          end
          object Label6: TLabel
            Left = 19
            Height = 13
            Top = 56
            Width = 25
            Caption = 'zoom'
            ParentColor = False
          end
          object Label7: TLabel
            Left = 22
            Height = 13
            Top = 96
            Width = 81
            Caption = 'unzoom (history)'
            ParentColor = False
          end
          object Label8: TLabel
            Left = 19
            Height = 13
            Top = 136
            Width = 54
            Caption = 'full unzoom'
            ParentColor = False
          end
          object Label9: TLabel
            Left = 19
            Height = 13
            Top = 177
            Width = 18
            Caption = 'pan'
            ParentColor = False
          end
        end
        object Panel3: TPanel
          Left = 0
          Height = 72
          Top = 0
          Width = 172
          Align = alTop
          BevelOuter = bvNone
          ClientHeight = 72
          ClientWidth = 172
          TabOrder = 1
          object LblMagnification: TLabel
            Left = 6
            Height = 13
            Top = 8
            Width = 67
            Caption = 'Magnification:'
            ParentColor = False
          end
          object LblHistoryCount: TLabel
            Left = 6
            Height = 13
            Top = 29
            Width = 68
            Caption = 'History count:'
            ParentColor = False
          end
        end
      end
      object ColorSource: TListChartSource
        left = 115
        top = 57
      end
      object ChartToolset1: TChartToolset
        left = 115
        top = 120
        object ChartToolset1ZoomDragTool1: TZoomDragTool
          Shift = [ssLeft]
          OnAfterMouseUp = ChartToolset1ZoomDragTool1AfterMouseUp
        end
        object ChartToolset1PanDragTool1: TPanDragTool
          Shift = [ssRight]
          OnAfterMouseDown = ChartToolset1PanDragTool1AfterMouseDown
        end
        object ChartToolset1UserDefinedTool1: TUserDefinedTool
          Shift = [ssMiddle]
          OnAfterMouseUp = ChartToolset1UserDefinedTool1AfterMouseUp
        end
        object ChartToolset1UserDefinedTool2: TUserDefinedTool
          Shift = [ssShift, ssMiddle]
          OnAfterMouseUp = ChartToolset1UserDefinedTool2AfterMouseUp
        end
      end
    end
    

## Related tutorials

  * [TAChart Tutorial: Getting started](<TAChart_Tutorial__Getting_started.md> "TAChart Tutorial: Getting started"): first steps with TAChart
  * [TAChart Tutorial: Chart Tools](<TAChart_Tutorial__Chart_Tools.md> "TAChart Tutorial: Chart Tools"): introduction to chart tools
  * [TAChart Tutorial: Function Series](<TAChart_Tutorial__Function_Series.md> "TAChart Tutorial: Function Series"): introduces `TFuncSeries` which follows a similar concept as `TColorMapSeries`

---

_Source: [https://wiki.freepascal.org/TAChart_Tutorial%3A_ColorMapSeries%2C_Zooming](https://web.archive.org/web/20230610003122/https://wiki.freepascal.org/TAChart_Tutorial%3A_ColorMapSeries%2C_Zooming)_
