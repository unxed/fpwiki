# TAChart Runtime FAQ

This article answers frequently asked questions related to usage of TAChart at runtime. 

## Contents

  * 1 Series
    * 1.1 How to add a series at runtime?
    * 1.2 How to plot x/y columns from a text file in a series?
    * 1.3 How to iterate through the series of a chart?
    * 1.4 How to delete/hide a series at runtime?
  * 2 Axes and axis transformations
    * 2.1 Fixed axis limits
    * 2.2 How to add an axis at runtime?
    * 2.3 How to add an axis transformation?
    * 2.4 How to remove an axis transformation?
    * 2.5 Logarithmic axis
    * 2.6 How to find the pixel coordinates of the point of the y axis?
  * 3 Miscellaneous
    * 3.1 What can I do to avoid compiler error _"Identifier not found 'TDoublePoint'"_?



## Series

### How to add a series at runtime?

Just create the series, set its properties and call the chart method `AddSeries`
    
    
    uses
      TATypes, TASeries;
    
    function AddLineSeries(AChart: TChart; ATitle: String; AColor: TColor): TChartSeries;
    begin
      Result := TLineSeries.Create(AChart);
      with TLineSeries(Result) do
      begin
        // Series title for the legend
        SeriesTitle := ATitle;
        // Show data point markers (specified fill color, black border)
        ShowPoints := true;  
        Pointer.Brush.Color := AColor;
        Pointer.Pen.Color := clBlack;
        Pointer.Style := psCircle;
        // Show line segments connecting the data points, color as specified.
        ShowLines := true;   
        LinePen.Style := psSolid;
        SeriesColor := AColor;
      end;
      // Add new series to the chart
      AChart.AddSeries(Result);
    end;
    

### How to plot x/y columns from a text file in a series?

Suppose a text file consisting of two comma-separated columns, just like this (data from <https://www.worldometers.info/world-population/world-population-by-year/>): 
    
    
    Year,World Population
    2023,8045311447
    2022,7975105156
    2021,7909295151
    2020,7840952880
    ...

and you want to plot the two columns as x and y in a series. There are several ways to read the file, here is the classical one using a TextFile: 
    
    
    uses
      TACustomSeries;
    
    procedure PlotFile(AFileName: String; ASeries: TChartSeries);
    var
      F: TextFile;
      s: String;
      p: Integer;
      x: Integer;
      y: Double;
    begin
      if ASeries = nil then
        raise Exception.Create('ASeries has not yet been created.');
    
      ASeries.BeginUpdate;
      ASeries.Clear;
      AssignFile(F, AFilename);
      Reset(F);
      // Read the non-numeric title line
      ReadLn(F, s);
      // Read the data lines, skip empty and invalid lines.
      while not EoF(F) do
      begin
        ReadLn(F, s);
        if s <> '' then
        begin
          p := Pos(',', s);
          if p <> 0 then
            if TryStrToInt(copy(s, 1, p-1), x) and TryStrToFloat(copy(s, p+1, Length(s)), y) then
              ASeries.AddXY(x, y);
        end;
      end;
      CloseFile(F);
      ASeries.EndUpdate;
    end;
    

### How to iterate through the series of a chart?

The series are accessible via the array-like property `Series` of the chart. Note that this returns only the most fundamental series type, TBasicChartSeries, and a type-cast may be necessage before being able to access the series properties. 

The following example re-colors all line series in red: 
    
    
    uses
      TASeries;
    var 
      i: Integer;
    begin
      for i := 0 to AChart.SeriesCount-1 do
        if AChart.Series[i] is TLineSeries then
          TLineSeries(AChart.Series[i]).SeriesColor := clRed;
    end;
    

If you prefer the more modern iterators you can use the syntax 
    
    
    uses
      TASeries, TAEnumerators;
    var
      ser: TCustomSeries;
    begin
      for ser in CustomSeries(AChart) do
        TLineSeries(ser).Series.Color := clRed;
    end;
    

### How to delete/hide a series at runtime?

  * If you don't need the series any more just destroy it: `series.Free`.
  * If you want to keept it for other usage, for example to insert it into another chart, call `Chart.DeleteSeries(series)`.
  * If you want to hide it, but keep it in the chart, set the property `Active` of the series to `false`.



## Axes and axis transformations

### Fixed axis limits

Normally the limits of the chart axes are determined such that the chart covers the available area as much as possible. But sometimes it is better to keep the axis limit frozen, independent of the data range. 

This code uses the Extent of the chart to define a constant range for the y axis between 0 and 100. The first example manipulates the Extent consecutively, the second one simultaneously by using an intermediate variable. Note that the first method may crash the program in the third line if, for some reason, the new maximum is smaller than the currently active minimum. Note that the second code freezes also the x axis; you'll have to call `Chart1.Extent.UseXMin := false` and `Chart1.Extent.UseXmax := false` to release it again. 
    
    
    Chart1.Extent.YMax := 100;
    Chart1.Extent.YMin := 0;
    Chart1.Extent.UseYMax := true;
    Chart1.Extent.UseYMin := true;
    

or: 
    
    
    var
      ex: TDoubleRect;
    begin
      ex := Chart1.GetFullExtent;
      ex.b.y := 100;
      ex.a.y := 0;
      Chart1.Extent.FixTo(ex);
    end;
    

### How to add an axis at runtime?

A chart can contain several axes which are stored in its `AxisList`. Since `AxisList` inherits from `TCollection` you can use its `Add` method to create a new axis: 
    
    
    uses
      TAChartAxis, TAChartUtils;
    
    function AddAxis(AParentChart: TChart; ATitle: String; AAlignment: TChartAxisAlignment): TChartAxis;
    begin
      // Create a new axis
      Result := AParentChart.AxisList.Add;
      // Axis orientation: calLeft, calTop, calRight or calBottom
      Result.Alignment := AAlignment;   
      // Axis title
      Result.Title.Caption := ATitle;
      Result.Title.Visible := true;
      // Rotate title of a vertical axis
      case AAlignment of
        calLeft  : Result.Title.LabelFont.Orientation := +900;
        calRight : Result.Title.LabelFont.Orientation := -900;
      end;
      // Show axis line
      Result.AxisPen.Visible := true;  
    end;
    

If you want to assign the new axis to AxisIndexX or AxisIndexY of a series you can use the property `Index` of the newly created axis. 

### How to add an axis transformation?

TChartAxisTransformations is a container for several TAxisTransform instances. It must be assigned to the `Transformations` property of an axis in order to be operative. It is created at runtime like any other component: 
    
    
    uses
      TATransformations;
    
    procedure TForm1.CreateAxisTransformationOfAxis(AChart: TChart; AxisIndex: Integer);
    begin
      AChart.AxisList[AxisIndex].Transformations := TChartAxisTransformations.Create(Self);
    end;
    

However, a bare TChartAxisTransformations component is useless. It must contain at least one TAxisTransform object. In the following code, we will add a TAutoScaleAxisTransform which is needed for paned charts. Please note that, although the TAxisTransform objects are collected in the TChartAxisTransformations property `List` which derives from TFPList, new objects must not be added by calling `List.Add`. Instead, assign the `Transformations` property of the new TAxisTransform to the TChartAxisTransformations component: 
    
    
    uses
      TATransformations;
    
    procedure TForm1.CreateAutoScaleAxisTransformOfAxis(AChart: TChart; AxisIndex: Integer);
    var
      T: TChartAxisTransformations;
      transf: TAxisTransform;
    begin
      // Create the TChartAxisTransformations component (or use the one dropped in the form at designtime)
      T := TChartAxisTransformations.Create(self);
      // and assign it to the Transformations property of the corresponding axis
      AChart.AxisList[AxisIndex].Transformations := T;
    
      // Create the AutoScaleAxisTransform object
      transf := TAutoscaleAxisTransform.Create(T);
      // and add it to the TChartAxisTransformations component
      transf.Transformations := T;
    end;
    

### How to remove an axis transformation?

Simply destroy it: 
    
    
    uses
      TATransformations;
    ...
      ChartAxisTransformations1AutoscaleAxisTransform1.Free;
    

### Logarithmic axis

Data varying over several orders of magnitude are usually plotted on a logarithmic axis. For achieving a logarithmic scale a ChartTransformations component containing a TLogarithmAxisTransform must be assigned to the corresponding axis. Some optimization must be done with the axis marks in order to get "nice" labels. Don't forget to set the index of the log axis to the `AxisIndexX` or `AxisIndexY` property of each series. 

The following code shows how an axis can be toggled between logarithmic and linear scale: 
    
    
    uses
      TAChartAxis, TAChartTransformations, TACustomSource;
    
    procedure LogAxis(AChart: TChart; AxisIndex: Integer; Enable: Boolean);
    var
      axis: TChartAxis;
      transf: TLogarithmAxisTransform;
    begin
      // Determine the axis to be processed
      axis := AChart.AxisList[AxisIndex];
    
      // If an AxisTransformations component has not yet been assigned to the axis, we must create one
      if axis.Transformations = nil then
        axis.Transformations := TChartAxisTransformations.Create(AChart.Owner);
    
      if axis.Transformations.List.Count = 0 then
      begin
        // Create the log transform...
        transf := TLogarithmAxisTransform.Create(Axis.Transformations);
        transf.Base := 10;
        transf.Transformations := Axis.Transformations;
      end else
        transf := (TAxisTransform(axis.Transformations.List[0]) as TLogarithmAxisTransform);
    
      // Enable the transformation for a log scale, or disable it for a linear scale
      transf.Enabled := Enable;
    
      // Find "nice" axis labels
      if Enable then begin
        axis.Intervals.Options := axis.Intervals.Options + [aipGraphCoords];
        // Depending on the size of the chart and the data extent you may have to
        // play with the following numbers to get "nice" log labels.
        axis.Intervals.MaxLength := 200;
        axis.Intervals.Tolerance := 100;
      end else begin
        // Adapt these numbers, too. These are the defaults of a linear scale.
        // Often the Intervals.MaxLength has to be increased to avoid overlapping labels.
        axis.Intervals.Options := axis.Intervals.Options - [aipGraphCoords];
        axis.Intervals.MaxLength := 50;
        axis.Intervals.Tolerance := 1;
      end;
    end;
    

In order to toggle between log and linear scale it must be known whether the axis currently is logarithmic or linear: 
    
    
    function IsLogAxis(AChart: TChart; AxisIndex: Integer): Boolean;
    var
      axis: TChartAxis;
      transf: TAxisTransform;
    begin
      Result := false;
      if Assigned(AChart) and (AxisIndex < AChart.AxisList.Count) then
      begin
        axis := AChart.AxisList[AxisIndex];
        if Assigned(axis.Transformations) and (axis.Transformations.List.Count > 0) then
        begin
          transf := TAxisTransform(axis.Transformations.List[0]);
          Result := transf.Enabled and (transf is TLogarithmAxisTransform);
        end;
      end;
    end;
    

In total, these routines could be called in a button's OnClick event handler to switch the left axis (index 0) between log and linear: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      LogAxis(Chart1, 0, not IsLogAxis(Chart1, 0));
    end;
    

### How to find the pixel coordinates of the point of the y axis?

Essentially, there are three coordinate systems which are involved when drawing a chart on the screen: 

  * The inner-most coordinates are the pixels on the screen, TAChart calls them **image coordinates**.
  * The outer-most coordinates are the values plotted on the axes; these are the units of the data values provided by the chart source. In TAChart speak these are called **axis coordinates**.
  * In between these two systems are the **graph coordinates**. This is an inivisible linear coordinate system layed over the chart. The connection between graph and image coordinates is made by the chart methods `GraphToImage` and `ImageToGraph`. The connection between graph and axis coordinates is defined by axis transformations. The axis transformations provide methods `GraphToAxis` and `AxisToGraph`. Normally, where there is no transformation, TAChart uses an identity transformation, i.e. assumes graph and axis coorindates to be equal. When the transformations object has been assigned to an axis you can also call the axis' `GraphToAxis` and `AxisToGraph` methods. And when a series has been assigned to an axis (by setting its `AxisIndexX` and/or `AxisIndexY`) you can also use the series' methods `GraphToAxis` and `AxisToGraph`.



Suppose you have a chart with two y axes, the right one displaying values between 0 and 100, and you want to determine the y pixel coordinates of some y axis value, say 27. Knowing the axis coordinates (27) you must determine the graph coordinate. Since the right axis is subject to an axis transformation you can call `Chart.AxisList[2].GetTransform` in order to dermine the `TAChartTransformations` applicable to this axis; this also takes care of the identity transform when no external transformations object is used. Then call the transformations method `AxisToGraph` to get the graph y value. Finally apply `Chart.YGraphToImage` to determine the pixel value: 
    
    
    uses
      TAChartAxis, TATransformations;
    
    function YAxisToPixels(AChart: TChart; AxisIndex: Integer; y: Double):  Integer;
    var
      axis: TChartAxis;
      grY: Double;
    begin
      axis := AChart.AxisList[AxisIndex];
      grY := axis.GetTransform.AxisToGraph(Y);
      Result := AChart.YGraphToImage(grY);
    end; 
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      ShowMessage(YAxisToPixels(Chart1, 1, 0.2).ToString);
    end;
    

## Miscellaneous

### What can I do to avoid compiler error _"Identifier not found 'TDoublePoint'"_?

`TDoublePoint` is declared in unit `TAChartUtils`, add it to the `uses` clause of the unit. It also contains the declaration of 

  * `TDoubleRect` which is needed for chart extents.



Other declarations, often not found when working with run-time code, are 

  * `TSeriesMarksStyle` for the `Style` of axis and series `Marks` \--> in unit `TAChartUtils`
  * `TSeriesPointerStyle` for the series pointer styles, e.g. `psCircle, psRectangle` \--> in unit `TATypes`
  * geometrical functions (creation of `TDoublePoint` and `TDoubleRect`, overloaded operators for `TPoint` and `TDoublePoints`) are in unit `TAGeometry`

---

_Source: [https://wiki.freepascal.org/TAChart_Runtime_FAQ](https://web.archive.org/web/20250418170635/https://wiki.freepascal.org/TAChart_Runtime_FAQ)_
