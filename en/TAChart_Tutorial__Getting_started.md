# TAChart Tutorial: Getting started

│ **English (en)** │  **[русский (ru)](<../ru/TAChart_Tutorial__Getting_started.md>)** │


## Contents

  * 1 Introduction
  * 2 What is needed?
  * 3 A new project
  * 4 Adding series
  * 5 Adding data
  * 6 Formatting the series
  * 7 Adding a legend
  * 8 Fine-tuning
  * 9 Finished!
  * 10 Source code
    * 10.1 Project file
    * 10.2 Unit1.pas
    * 10.3 Unit.lfm



## Introduction

[![tachart getting started step6.png](https://wiki.freepascal.org/images/b/b0/tachart_getting_started_step6.png)](</File:tachart_getting_started_step6.png>)

If you want to add a plot or a chart to your Lazarus form you should have a look at [TAChart](<TAChart.md> "TAChart"). This is the standard Lazarus package for drawing plots and charts. 

In this tutorial we will draw some elemental mathematical functions by TAChart. We will create a chart with three line series for the functions 

  * y=cos(x), 
  * y=sin(x), and 
  * y=cos(x)*sin(x). 



Maybe it is necessary to expain the words "series" and "chart" here: the entire diagram is the "chart", and each of the functions will be displayed as a curve called "series". We will connect the data points by line sections, therefore, we will use the "line series". TAChart, of course, allows to use many other types of series, for example bar series, area series, pie series, or even advanced types like spline or fit series. 

## What is needed?

This guide will be very elemental. Of course, you'll need basic knowledge of the Object Pascal language and the Lazarus IDE. 

TAChart is included in Lazarus, so there is no need for seeking this package somewhere in the internet. However, don't use a too-old version of Lazarus since TAChart is being actively developed and some features may change from version to version. 

It is best if you follow this tutorial step by step. But if you want you can use the source code of the finished project at the end of this article. 

## A new project

[![TAChart component palette.png](https://wiki.freepascal.org/images/9/95/TAChart_component_palette.png)](</File:TAChart_component_palette.png>)

To begin with, we create a new project in Lazarus and, in the component palette we select the page "Chart". Click on the left-most icon, the TChart component, and add it to the form. You will see an empty diagram with standardized x and y axes. 

[![TAChart GettingStarted Step1.png](https://wiki.freepascal.org/images/6/63/TAChart_GettingStarted_Step1.png)](</File:TAChart_GettingStarted_Step1.png>)

## Adding series

Now let's add series. For this purpose, double-click on the chart (or right-click, and select "Edit series" from the context menu). The TAChart series editor appears. It is still empty, but when you select "Add" you will see a menu with all the series types available within the TAChart package. Select the "line series". Repeat two times to create, in total, three line series that are assigned to the chart. They are not yet displayed in the chart, because they do not yet have any data. No data - no display. We will fix that in a minute. 

[![TAChart GettingStarted Step1a.png](https://wiki.freepascal.org/images/6/6f/TAChart_GettingStarted_Step1a.png)](</File:TAChart_GettingStarted_Step1a.png>)

Before we do that let us have a look at the object inspector tree. TAChart uses a quite complex architecture of classes and containers, so it is important to understand the object tree. In the current state of our project we see the TChart component and its children, the three line series and the AxisList with Left and Bottom axes as children again. The series names are constructed according to `Chart<number><SeriesType><Number>`. 

[![tachart getting started object tree.png](https://wiki.freepascal.org/images/4/42/tachart_getting_started_object_tree.png)](</File:tachart_getting_started_object_tree.png>)

Why don't we give them more descriptive names, like `SinSeries`, `CosSeries`, and `SinCosSeries`? For this purpose, click on each series, and change the series `Name` in the corresponding field of the object inspector below. 

## Adding data

Now it's time to add data. The easiest option is to create the data along with the form, i.e. we write a handler for the forms's `OnCreate` event. Of course, in a "real" project, you will add data at other occasions, for example after a button click which might initiate a calculation or read data from a file. 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    const
      N = 100;
      MIN = -10;
      MAX = 10;
    var
      i: Integer;
      x: Double;
    begin
      for i:=0 to N-1 do begin
        x := MIN + (MAX - MIN) * i /(N - 1);
        SinSeries.AddXY(x, sin(x));
        CosSeries.AddXY(x, cos(x));
        SinCosSeries.AddXY(x, sin(x)*cos(x));
      end;
    end;

This procedure creates 100 data points for each series. The x values are calculated to be equidistant between `MIN` and `MAX`, i.e. -10 and 10. The important lines are the calls to the `AddXY` method of each series. This method takes the x and y coordinates of each data point and adds them to an internal list. There are also overloaded versions of this method where you can also pass a text label for each data point, as well as an individual color, but we don't need this feature here. 

The mentioned list is a so-called chart source - this is a class which provides the data for plotting. You can use the built-in chart source, or you can link the series to a separate one. There is a variety of chart sources in the component palette, like 

  * `TListChartSource` which stores the data in a list (this type is used by our series internally) 
  * `TDBChartSource` which interfaces to the records and fields in a database 
  * `TUserDefinedChartSource` which gives a very general access to data, for example, stored in an array of user-defined records 
  * `TCalculatedChartSource` which allows to analyze one series and display the results in another one. 



But enough on chart sources - the built-in source is sufficient for this introductory project. 

[![tachart getting started step2.png](https://wiki.freepascal.org/images/3/34/tachart_getting_started_step2.png)](</File:tachart_getting_started_step2.png>)

Ok, let's compile the program. We see the three curves, and the x axis automatically covers our data range between -10 and 10. Well - not too bad, but far from being perfect: The diagram is very confusing, we cannot distinguish between the individual lines, and we cannot tell which series belongs to which function. 

## Formatting the series

Why don't we change the color of each series? To do this select each series in the object tree and, in the object inspector below, change the property `SeriesColor` as you want it to be - `SeriesColor` is the color of the lines connecting the data points. 

When you recompile you will see the series colored. But we still cannot distinguish between the different functions. What we need is a legend. 

## Adding a legend

Highlight the chart and, in the object inspector, scroll down to `Legend` and open the sub-properties. At the end, there is `Visible` which is `false` by default - so, set it to `true`, and recompile. 

[![tachart getting started step3.png](https://wiki.freepascal.org/images/6/6e/tachart_getting_started_step3.png)](</File:tachart_getting_started_step3.png>)

Well, it's getting better. What is missing, is an explanatory text for each series, like `y = sin(x)`. For this purpose each series has a property `Title` \- this is the text which will appear in the legend for each entry along with the symbol for the corresponding series. Therefore you have to go through all series again and use the object inspector to set `Title` to `y=sin(x)`, `y=cos(x)`, and `y=sin(x)*cos(x)`, respectively. 

[![tachart getting started step4.png](https://wiki.freepascal.org/images/6/67/tachart_getting_started_step4.png)](</File:tachart_getting_started_step4.png>) [![tachart getting started step4a.png](https://wiki.freepascal.org/images/c/c1/tachart_getting_started_step4a.png)](</File:tachart_getting_started_step4a.png>)

Now when you recompile, the chart is almost perfect. Almost - because the legend is quite large, and the diagram is squeezed into the remaining space. Of course, we could increase the size of the form. But can we also put the legend underneath the diagram? No problem: go back to the chart's property `Legend` and set `Alignment` to `laBottomCenter`. You should also set `ColumnCount` to `3`, otherwise the legend items will be in a column instead of in a row. 

[![tachart getting started step5.png](https://wiki.freepascal.org/images/2/20/tachart_getting_started_step5.png)](</File:tachart_getting_started_step5.png>)

Ah! -- the third legend entry is truncated because the window is not wide enough. Align the chart to `alClient` and increase the width of the window a bit. 

## Fine-tuning

A few more things can still be improved: The axes should have a title, like "x axis" and "y axis", respectively, and there should also be a title above the chart. 

To set the title for the x axis select in the object tree the item `Bottom` of the AxisList child component of TChart, or go to `Bottom axis` in the object inspector. Open the sub-properties where you'll find `Title` which has sub-properties again. The text in the field `Caption` will be displayed as a title above the chart. Don't forget to set `Visible` to `true`. You may also want to set the font style of the title to bold - you find that property under `LabelFont`. Repeat the same with `Left axis`. 

Essentially the same procedure can be used to set the chart title: Scroll to the chart's property `Title`, enter the title in the field `Text` (you may even use a multi-line text here), and set `Visible` to `true` again. 

And finally, you might prefer to use a white background of the chart area, this is changed by the property `BackColor` of TChart. And maybe the grid should be less emphasized, i.e. it might look better with color `clSilver`. Use the left and bottom axes property `Grid.Color` to change that. 

## Finished!

In the end, our chart looks like this. Neat, isn't it? And it was not difficult at all... 

If you want to learn more about TAChart have a look at the official [TAChart documentation](<TAChart_documentation.md> "TAChart documentation"), and study some of the many demos that come with TAChart. They can be found in the folder `components\tachart\demo` of your Lazarus installation. 

[![tachart getting started step6.png](https://wiki.freepascal.org/images/b/b0/tachart_getting_started_step6.png)](</File:tachart_getting_started_step6.png>)

## Source code

The source code of this tutorial project can be found in the folder _tutorials/getting_started_ of trunk TAChart installations. 

### Project file
    
    
    program project1;
     
    {$mode objfpc}{$H+}
     
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Interfaces, // this includes the LCL widgetset
      Forms, Unit1, tachartlazaruspkg
      { you can add units after this };
     
    {$R *.res}
     
    begin
      RequireDerivedFormResource := True;
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    end.

### Unit1.pas
    
    
    unit Unit1;
     
    {$mode objfpc}{$H+}
     
    interface
     
    uses
      Classes, SysUtils, FileUtil, TAGraph, TASeries, Forms, Controls, Graphics,
      Dialogs;
     
    type
     
      { TForm1 }
     
      TForm1 = class(TForm)
        Chart1: TChart;
        SinSeries: TLineSeries;
        CosSeries: TLineSeries;
        SinCosSeries: TLineSeries;
        procedure FormCreate(Sender: TObject);
      private
        { private declarations }
      public
        { public declarations }
      end;
     
    var
      Form1: TForm1;
     
    implementation
     
    {$R *.lfm}
     
    { TForm1 }
     
    procedure TForm1.FormCreate(Sender: TObject);
    const
      N = 100;
      MIN = -10;
      MAX = 10;
    var
      i: Integer;
      x: Double;
    begin
      for i:=0 to N - 1 do begin
        x := MIN + (MAX - MIN) * i / (N - 1);
        SinSeries.AddXY(x, sin(x));
        CosSeries.AddXY(x, cos(x));
        SinCosSeries.AddXY(x, sin(x)*cos(x));
      end;
     
    end;
     
    end.

### Unit.lfm
    
    
    object Form1: TForm1
      Left = 14
      Height = 244
      Top = 137
      Width = 347
      Caption = 'Form1'
      ClientHeight = 244
      ClientWidth = 347
      OnCreate = FormCreate
      LCLVersion = '1.1'
      object Chart1: TChart
        Left = 0
        Height = 244
        Top = 0
        Width = 347
        AxisList = <    
          item
            Grid.Color = clSilver
            Minors = <>
            Title.LabelFont.Orientation = 900
            Title.LabelFont.Style = [fsBold]
            Title.Visible = True
            Title.Caption = 'y axis'
          end    
          item
            Grid.Color = clSilver
            Alignment = calBottom
            Minors = <>
            Title.LabelFont.Style = [fsBold]
            Title.Visible = True
            Title.Caption = 'x axis'
          end>
        BackColor = clWhite
        Foot.Brush.Color = clBtnFace
        Foot.Font.Color = clBlue
        Legend.Alignment = laBottomCenter
        Legend.ColumnCount = 3
        Legend.Visible = True
        Title.Brush.Color = clBtnFace
        Title.Font.Color = clBlue
        Title.Font.Style = [fsBold]
        Title.Text.Strings = (
          'My first chart'
        )
        Title.Visible = True
        Align = alClient
        ParentColor = False
        object SinSeries: TLineSeries
          Title = 'y=sin(x)'
          LinePen.Color = clRed
        end
        object CosSeries: TLineSeries
          Title = 'y=cos(x)'
          LinePen.Color = clBlue
        end
        object SinCosSeries: TLineSeries
          Title = 'y=sin(x)*cos(x)'
          LinePen.Color = clGreen
        end
      end
    end

---

_Source: [https://wiki.freepascal.org/TAChart_Tutorial%3A_Getting_started](https://web.archive.org/web/20190605185021/https://wiki.freepascal.org/TAChart_Tutorial%3A_Getting_started)_
