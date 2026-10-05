# TAChart Tutorial: Userdefined ChartSource

│ **English (en)** │

## Contents

  * 1 Introduction
  * 2 Preparation
    * 2.1 Data
    * 2.2 Combobox as category selector
    * 2.3 Preparing TChart
  * 3 Creating an area series
  * 4 User-defined chart source
  * 5 Fine-tuning
  * 6 Source code
    * 6.1 Project file
    * 6.2 population.pas
    * 6.3 Unit1.pas
    * 6.4 Unit1.lfm



## Introduction

[![TAChart Population6.png](https://wiki.freepascal.org/images/4/40/TAChart_Population6.png)](</File:TAChart_Population6.png>)

In this tutorial we use [TAChart](<TAChart.md> "TAChart") to plot the **age distribution of world population**. In doing this, you will learn 

  * how to work with an [area series](<TAChart_documentation.md> "TAChart documentation")
  * and, most of all, how to apply a [user-defined chart source](<TAChart_documentation.md> "TAChart documentation")



As usual, we assume that you are familiar with Lazarus and the [Object Pascal](<Object_Pascal.md> "Object Pascal") language and have some basic knowledge of the TAChart package that you would acquire by working through the [Getting Started](<TAChart_Tutorial__Getting_started.md> "TAChart Tutorial: Getting started") tutorial. 

## Preparation

### Data

The site [www.census.gov](<http://www.census.gov>) contains a wealth of demographic data. This [link](<http://www.census.gov/population/international/data/worldpop/tool_population.php>) leads you directly to the data that we want to plot: world population as a function of age and gender. 

Go to that page, select a year and click _"submit"_. In the lower part of the window, you will see a table with the columns _"Age"_ , _"Both Sexes Population"_ , _"Male population"_ , _"Female population"_ , and _"Sex ratio"_. Use the mouse to select the data in the table (including header) and copy them to the clipboard. Paste the data into a text editor and save as _"population.txt"_. 

Open Lazarus and create a new project. At first we have to load the data to make them available for plotting. 

We define a `TPopulationRecord` and an [array](<Array.md> "Array") of type `TPopulationArray` to hold the data of the population table: 
    
    
    type
      TPopulationRecord = record
        Age: Integer;
        Total: Double;
        Male: Double;
        Female: Double;
        Ratio: Double;
      end;
    
      TPopulationArray = array of TPopulationRecord;
    

Now let's read the file. Have a look at the [unit](<Unit.md> "Unit") `population.pas` in the [Source Code](<TAChart_Tutorial__Userdefined_ChartSource.md> "TAChart Tutorial: Userdefined ChartSource") section of this tutorial. You will find there a 
    
    
    procedure LoadPopulationData(const AFilename:string; var Data:TPopulationArray)
    

which does the file reading and stores the data in a `TPopulationArray`. 

Just include the unit `population` in your [`uses`](<Uses.md> "Uses") list to get access to this [procedure](<Procedure.md> "Procedure"). Add a [variable](<Variable.md> "Variable") `PopulationData` of type `TPopulationArray` to the [`private`](<Private.md> "Private") section of the main form. Now we have everything ready to call `LoadPopulationData` from the form's OnCreate event handler. This will read our data file into the array `PopulationData`. 
    
    
    uses
      ..., population;
    
    type
      TForm1 = class(TForm)
      // ...
      private
        PopulationData : TPopulationArray;
      // ...
      end;
    
    const
      POPULATION_FILE = 'population.txt';
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      LoadPopulationData(POPULATION_FILE, PopulationData); 
    end;
    

Compile to check for obvious errors. You may get a [runtime error](<runtime_error.md> "runtime error") on that the data file cannot be found. This is because our data file is assumed to reside in the same directory as the exe file. To fix this, either change the [constant](<Constant.md> "Constant") `POPULATION_FILE` to the correct storage location, or copy _"population.txt"_ to the exe folder. Now the program should run fine, you will see an unspectacularly empty form, though. 

### Combobox as category selector

The file contains various categories: total, male, and femal population, as well as male-to-female ratio. Why not display these data in our chart? 

To prepare for this, 

  * add a **[panel](<TPanel.md> "TPanel")** to the form, align it to `alTop`, and delete its caption.
  * Add a **[combobox](<TComboBox.md> "TComboBox")** to this panel, set its `Style` to `csDropdownList`, add the strings "Total population", "Male population", "Female population", and "Ratio male/female (%)" to the `Items` [property](</Property> "Property"), and set `ItemIndex` to 0.



### Preparing TChart

Now we are finished with preparations, and it's time for charting... 

  * Add a `TChart` component to the form, align it to `alClient`.
  * Set the chart's `BackColor` to `clWhite` and the `Grid.Color` of each axis to `clSilver`.
  * Get axis titles shown by setting `LeftAxis.Visible` and `BottomAxis.Visible` to `true`.
  * Set the caption of the bottom axis to "Age", and that of the left axis to "Total population" (or leave it empty - we will later replace it at [runtime](<runtime.md> "runtime") by the selected item of the combobox).
  * Use the text "World population" as the chart's title.
  * Set these font styles to `fsBold`.
  * Maybe it is a good idea to display our data reference in the [footer](<TAChart_documentation.md> "TAChart documentation") \- the chart property `Foot` can be used for that. Note that the property editor of `Foot.Text` (as well as that of `Title.Text`) allows to enter linefeeds for multi-lined titles.



[![TAChart Population2.png](https://wiki.freepascal.org/images/6/61/TAChart_Population2.png)](</File:TAChart_Population2.png>)

## Creating an area series

This time, we want to display our data as an [area series](<http://www.wikipedia.org/wiki/Area_chart> "wikipedia:Area chart"). This type of series connects the data points as in a line series, but also fills the area underneath by a given color. It is very common to display demographic data, like a [population pyramid](<http://www.wikipedia.org/wiki/Population_pyramid> "wikipedia:Population pyramid"), in this way. 

So, add a [`TAreaSeries`](<TAChart_documentation.md> "TAChart documentation") to the chart. As usual, you don't see it at this time because it does not yet have any data. But we can prepare some properties: 

  * Set its `SeriesColor` to `clSkyBlue`. This is the fill color of the area series.
  * Hide the vertical connecting lines by setting the `AreaLinesPen` to `psClear`.
  * If you want you can also change `ConnectType` to `ctStepXY` or `ctStepYX` to get a step effect.



Later, when the series has data, you should play around with these and other properties to see what they do. 

## User-defined chart source

Now, how do we get our data to the area series? 

The first idea might be iterate through our `PopulationData` array and copy the data to the series by means of `AddXY` calls, like this: 
    
    
    var
      i: Integer;
    begin
      for i:=0 to High(PopulationData) do
        Chart1AreaSeries1.AddXY(PopulationData[i].Age, PopulationData[i].Total);
    end;
    

The disadvantage of this approach is that we would have the data in memory twice: in the `PopulationData` array and in the series - not so good: we would be wasting memory, and we would have to synchronize both storages when data points are added, deleted or edited. 

[![TAChart Population3.png](https://wiki.freepascal.org/images/e/e0/TAChart_Population3.png)](</File:TAChart_Population3.png>)

To avoid this, we add a [`TUserDefinedChartSource`](<TAChart_documentation.md> "TAChart documentation") to the form, it is the third or fourth icon from the left in the [Chart component palette](<Chart_tab.md> "Chart tab"). This chart source is made to interface to any kind of data storage, the source itself does not store any data. You just have to write an event handler for `OnGetChartDataItem` in order to transfer the data. For this purpose, the event takes a [`var` parameter](<Variable_parameter.md> "Variable parameter") `AItem` of type `TChartDataItem` that is defined as follows (with elements omitted that are not needed here): 
    
    
    type
      TChartDataItem = object
        X, Y: Double;
        // ...
      end;
    

The fields `X` and `Y` are the coordinates of the data point. 

Now we write the event handler for `OnGetChartDataItem`: 
    
    
    procedure TForm1.UserDefinedChartSource1GetChartDataItem(
      ASource: TUserDefinedChartSource; AIndex: Integer; var AItem: TChartDataItem);
    begin
      AItem.X := PopulationData[AIndex].Age;
      case Combobox1.ItemIndex of
        0: AItem.Y := PopulationData[AIndex].Total;
        1: AItem.Y := PopulationData[AIndex].Male;
        2: AItem.Y := PopulationData[AIndex].Female;
        3: AItem.Y := PopulationData[AIndex].Ratio;
      end;
    end;
    

`AIndex` is the index of the datapoint which is queried. Since both chart source and our `PopulationData` array begin at index 0 we just look into our data array at this index and copy the data to the corresponding elements of the `AItem`. Note that we pick the y coordinate according to what is selected in the combobox. In this way, the UserDefinedChartSource can provide a multitude of data. 

There are still some important things to do: 

  * Tell the series to use the UserDefinedChartSource instead of the built-in source. For this purpose, we point the series' property `Source` to `UserDefinedChartSource1`.
  * Tell the UserDefinedChartSource how many data points the external data array contains. We have to enter this number in the property `PointsNumber` of the UserDefinedChartSource. The data count is known after our procedure `LoadPopulationData` has finished. So we add a line which sets `PointsNumber` correctly to the `FormCreate` method after `LoadPopulationData` is called.
  * Since the UserDefinedChartSource does not know anything about the structure of the external data we have to notify it whenever data are available or have been changed. We do this by calling `UserDefinedChartSource1.Reset` at an appropriate place. Sometimes you are lucky that some other method may have done this already. But keep in mind that whenever a chart with a UserDefinedChartSource does not behave as you expect there is a chance that you may have forgotten to call `Reset`. To see what happens let's "forget" to call `Reset` for the moment.



## Fine-tuning

[![TAChart Population4.png](https://wiki.freepascal.org/images/4/48/TAChart_Population4.png)](</File:TAChart_Population4.png>)

Compile the project in this stage. Not too bad, some flaws, though. 

  * The labels of the y axis are strange. Why do they jump from 80000000 to 1, and then to 12? This seems to be due to a bug of the `Format` function for large numbers in some FPC versions. If you have that issue as well, simply change the property `LeftAxis.Marks.Format`. This string is passed to the `Format` function to convert numbers to strings. The format specifier "%.0n" for example avoids that conversion error and, additionally, adds nice thousand separators to the labels which makes them much more readable. However, be aware that the "0" cuts off any decimals. Therefore, you should use this format specifier only for large numbers.
  * There is a small gap between the y axis and the filled area series. Similarly, the filled area reaches a bit below the x axis. This is caused by the property `Margins` of the chart. This property is usually quite helpful to create an empty area near the plot axes, free from overlapping symbols and marks labels. But here, it is unfavorable. So just set `Chart.Margins.Left` and `Chart.Margins.Bottom` to 0.
  * Ah, and the combobox is not working yet. We need to assign a handler for its `OnSelect` event. What does it have to do? Well, when another combobox item has been selected the UserDefinedChartSource reports a different y value to the series - that's what we want. But how we get the `OnGetChartDataItem` being called? Just by redrawing the chart! When the series is redrawn it always has to query the data from the chart source, so it will get the updated data automatically. OK - let's call `Chart1.Invalidate`:


    
    
    procedure TForm1.ComboBox1Select(Sender: TObject);
    begin
      Chart1.Invalidate;
    end;
    

When you compile and select another combobox item you will find that the series does nicely update to the selected category. But the axis is frozen, it should rescale automatically. Is this a bug? 

No. A few lines above, it was said that whenever a chart with a UserDefinedChartSource does not behave as expected there is a chance that you may have forgotten to call `UserDefinedChartSource.Reset`. That's it: we replace the `Invalidate` by the `Reset`. 

And we can fix another issue: the caption of the y axis should change whenever a different category is selected. For this, we copy the text of the selected combobox item to the chart's `LeftAxis.Title.Caption`. We add a corresponding line the the combobox OnSelect event handler: 
    
    
    procedure TForm1.ComboBox1Select(Sender: TObject);
    begin
      Chart1.LeftAxis.Title.Caption := Combobox1.Items[Combobox1.ItemIndex];
      UserDefinedChartSource1.Reset;
    end;
    

Now switching of data categories by the combobox is working fine. 

A minor flaw left: Since our y data do not contain a 0 the y axis does not begin at 0 due to the automatic axis scaling. But it would be nice if there were a zero. For this, we must partially turn off automatic axis scaling. The `Chart.Extent` defines a rectangle for the minimum and maximum x and y values. When we set `Chart.Extent.UseYMin` to `true`, the y axis minimum is taken from the value of `Chart.Extent.YMin`, and this is 0 by default. Therefore, we have our zero back. 

[![TAChart Population5.png](https://wiki.freepascal.org/images/1/13/TAChart_Population5.png)](</File:TAChart_Population5.png>)

But there is another small problem now: the lowest grid line is drawn on top of the x axis. First of all, the black lines that we see as axes are not the axes themselves, but belong to the frame drawn around the plotting area by default. You can test this by setting `Chart1.Frame.Visible` temporarily to `false`. For the axes, there is a property `AxisPen` where the "true" axis line can be turned on by `AxisPen.Visible = true`. After after doing this, the x axis is still a dashed line. This is caused by the drawing sequence. By default, the y axis, along with its grid, is drawn _after_ the x axis. If you want to have the x axis drawn first you go to the object tree and drag the x axis above the y axis. 

When you compile the chart is perfect. 

[![TAChart Population6.png](https://wiki.freepascal.org/images/4/40/TAChart_Population6.png)](</File:TAChart_Population6.png>)

[![TAChart Population7.png](https://wiki.freepascal.org/images/f/fc/TAChart_Population7.png)](</File:TAChart_Population7.png>)

## Source code

For Lazarus v2.1+ you can find the [source code](<Source_code.md> "Source code") of this tutorial project in folder **components/tachart/tutorials/population1** of your Lazarus installation. For older versions, copy and paste the following code to the corresponding files. 

### Project file
    
    
    program project1;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Interfaces, // this includes the LCL widgetset
      Forms, Unit1, tachartlazaruspkg, population
      { you can add units after this };
    
    {$R *.res}
    
    begin
      RequireDerivedFormResource := True;
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    end.
    

### population.pas
    
    
    unit population;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils;
    
    type
      TPopulationRecord = record
        Age: Integer;
        Total: Double;
        Male: Double;
        Female: Double;
        Ratio: Double;
      end;
    
    type
      TPopulationArray = array of TPopulationRecord;
    
    procedure LoadPopulationData(const AFileName: String; var Data: TPopulationArray);
    
    
    implementation
    
    procedure LoadPopulationData(const AFileName: String; var Data: TPopulationArray);
    
      function StripThousandSep(const s: String): String;
      var
        i: Integer;
      begin
        Result := s;
        for i:=Length(Result) downto 1 do
          if Result[i] = ',' then
            Delete(Result, i, 1);
      end;
    
    var
      List1, List2: TStringList;
      i, j, n: Integer;
      s: String;
      ds: char;
    begin
      ds := FormatSettings.DecimalSeparator;
      List1 := TStringList.Create;
      try
        List1.LoadFromFile(AFileName);
        n := List1.Count;
        SetLength(Data, n-2);
        FormatSettings.DecimalSeparator := '.';
        List2 := TStringList.Create;
        try
          List2.Delimiter := #9;
          List2.StrictDelimiter := true;
          j := 0;
          for i:=2 to n-1 do begin
            List2.DelimitedText := List1[i];
            s := List1[i];
            with Data[j] do begin
              if i < n-1 then
                Age := StrToInt(trim(List2[0]))
              else
                Age := 100;
              Total := StrToFloat(StripThousandSep(trim(List2[1])));
              Male := StrToFloat(StripThousandSep(trim(List2[2])));
              Female := StrToFloat(StripThousandSep(trim(List2[3])));
              Ratio := StrToFloat(trim(List2[4]));
            end;
            inc(j);
          end;
        finally
          List2.Free;
        end;
      finally
        FormatSettings.DecimalSeparator := ds;
        List1.Free;
      end;
    end;
    
    end.
    

### Unit1.pas
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, TAGraph, TASources, TASeries, Forms, Controls,
      Graphics, Dialogs, ExtCtrls, StdCtrls, population, TACustomSource;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Chart1: TChart;
        Chart1AreaSeries1: TAreaSeries;
        ComboBox1: TComboBox;
        Panel1: TPanel;
        UserDefinedChartSource1: TUserDefinedChartSource;
        procedure ComboBox1Select(Sender: TObject);
        procedure FormCreate(Sender: TObject);
        procedure UserDefinedChartSource1GetChartDataItem(
          ASource: TUserDefinedChartSource; AIndex: Integer;
          var AItem: TChartDataItem);
      private
        { private declarations }
        PopulationData: TPopulationArray;
      public
        { public declarations }
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    const
      POPULATION_FILE = 'population.txt';
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      LoadPopulationData(POPULATION_FILE, PopulationData);
      UserDefinedChartSource1.PointsNumber := Length(PopulationData);
      Chart1.LeftAxis.Title.Caption := Combobox1.Items[Combobox1.ItemIndex];
    end;
    
    procedure TForm1.ComboBox1Select(Sender: TObject);
    begin
      Chart1.LeftAxis.Title.Caption := Combobox1.Items[Combobox1.ItemIndex];
      UserDefinedChartSource1.Reset;
    end;
    
    procedure TForm1.UserDefinedChartSource1GetChartDataItem(
      ASource: TUserDefinedChartSource; AIndex: Integer; var AItem: TChartDataItem);
    begin
      AItem.X := PopulationData[AIndex].Age;
      case Combobox1.ItemIndex of
        0: AItem.Y := PopulationData[AIndex].Total;
        1: AItem.Y := PopulationData[AIndex].Male;
        2: AItem.Y := PopulationData[AIndex].Female;
        3: AItem.Y := PopulationData[AIndex].Ratio;
      end;
    end;
    
    end.
    

### Unit1.lfm
    
    
    object Form1: TForm1
      Left = 342
      Height = 360
      Top = 128
      Width = 480
      Caption = 'World population'
      ClientHeight = 360
      ClientWidth = 480
      OnCreate = FormCreate
      Position = poScreenCenter
      LCLVersion = '1.1'
      object Panel1: TPanel
        Left = 0
        Height = 36
        Top = 0
        Width = 480
        Align = alTop
        ClientHeight = 36
        ClientWidth = 480
        TabOrder = 0
        object ComboBox1: TComboBox
          Left = 12
          Height = 23
          Top = 5
          Width = 196
          ItemHeight = 15
          ItemIndex = 0
          Items.Strings = (
            'Total population'
            'Male population'
            'Female population'
            'Ratio male/female (%)'
          )
          OnSelect = ComboBox1Select
          Style = csDropDownList
          TabOrder = 0
          Text = 'Total population'
        end
      end
      object Chart1: TChart
        Left = 0
        Height = 324
        Top = 36
        Width = 480
        AxisList = <    
          item
            Grid.Visible = False
            Alignment = calRight
            AxisPen.Visible = True
            Marks.Visible = False
            Minors = <>
          end    
          item
            Grid.Color = clSilver
            Alignment = calBottom
            AxisPen.Visible = True
            Minors = <>
            Title.LabelFont.Style = [fsBold]
            Title.Visible = True
            Title.Caption = 'Age'
          end    
          item
            Grid.Color = clSilver
            AxisPen.Visible = True
            Marks.Format = '%.0n'
            Marks.Style = smsCustom
            Minors = <>
            Title.LabelFont.Orientation = 900
            Title.LabelFont.Style = [fsBold]
            Title.Visible = True
          end>
        BackColor = clWhite
        Extent.UseYMin = True
        Foot.Alignment = taLeftJustify
        Foot.Brush.Color = clBtnFace
        Foot.Font.Color = clBlue
        Foot.Text.Strings = (
          'Source:'
          'http://www.census.gov/population/international/data/worldpop/tool_population.php'
        )
        Foot.Visible = True
        Margins.Left = 0
        Margins.Right = 0
        Margins.Bottom = 0
        Title.Brush.Color = clBtnFace
        Title.Font.Color = clBlue
        Title.Font.Style = [fsBold]
        Title.Text.Strings = (
          'World population'
        )
        Title.Visible = True
        Align = alClient
        ParentColor = False
        object Chart1AreaSeries1: TAreaSeries
          AreaBrush.Color = clSkyBlue
          AreaLinesPen.Style = psClear
          Source = UserDefinedChartSource1
        end
      end
      object UserDefinedChartSource1: TUserDefinedChartSource
        OnGetChartDataItem = UserDefinedChartSource1GetChartDataItem
        left = 552
        top = 152
      end
    end

---

_Source: [https://wiki.freepascal.org/TAChart_Tutorial%3A_Userdefined_ChartSource](https://web.archive.org/web/20231002144032/https://wiki.freepascal.org/TAChart_Tutorial%3A_Userdefined_ChartSource)_
