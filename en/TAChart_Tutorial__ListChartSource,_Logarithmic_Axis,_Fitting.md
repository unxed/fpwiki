# TAChart Tutorial: ListChartSource, Logarithmic Axis, Fitting

│ **English (en)** │  **[русский (ru)](<../ru/TAChart_Tutorial__ListChartSource,_Logarithmic_Axis,_Fitting.md>)** │

## Contents

  * 1 Introduction
  * 2 Preparation
    * 2.1 Setting up the chart
    * 2.2 Creating a point series
  * 3 Entering data
    * 3.1 ListChartSource
    * 3.2 The ListChartSource data points editor
    * 3.3 Displaying datapoint marks
  * 4 Setting up a logarithmic axis
    * 4.1 TChartTransformations and LogarithmicAxisTransform
    * 4.2 Automatic finding of axis labels
    * 4.3 Manual finding of axis labels
    * 4.4 Minor tick marks
  * 5 Fitting
    * 5.1 TFitSeries
    * 5.2 Fit results
  * 6 Source code
    * 6.1 Project file
    * 6.2 Unit1.pas
    * 6.3 Unit1.lfm



## Introduction

[![TAChart LogAx Tutorial15.png](https://wiki.freepascal.org/images/3/3d/TAChart_LogAx_Tutorial15.png)](</File:TAChart_LogAx_Tutorial15.png>)

Do you know [Moore's law](<http://www.wikipedia.org/wiki/Moore%27s_law> "wikipedia:Moore's law")? It is in front of you... 

When you are reading this you are probably sitting in front of your desktop computer or have your notebook on your knees, or you are holding your smartphone in your hand. A tremendous progress of microtechnology has been happening during the last decades and is still on-going. This is due to miniaturization of electronic devices allowing to pack ever more functions onto a single silicon chip. And this is what Moore's law is about: the number of transistors on a chip has doubled approximately every two years. 

In this tutorial we will take published data of an important manufacturer of microprocessors and use TAChart to plot the number of transistors versus the year of market introduction of the products. From these data we will try to confirm Moore's law. 

You can find the data on the site [www.intel.com/pressroom/kits/quickreffam.htm](<http://www.intel.com/pressroom/kits/quickreffam.htm>) which contains a list of microprocessors, their date of market introduction, and their transistor count per chip. 

When working through this example you will learn 

  * how to **enter data at designtime**
  * how to create a **point series with marks above each data point**
  * how to **create a logarithmic axis**
  * how to **fit an equation to data**.



You should have some basic knowledge of how to work with TAChart; if you don't you should have a look at the [Getting started tutorial](<TAChart_Tutorial__Getting_started.md> "TAChart Tutorial: Getting started") of TAChart. Of course, you must be familiar with [Lazarus](<Lazarus.md> "Lazarus") and [Object Pascal](<Object_Pascal.md> "Object Pascal"). And you should not be too afraid of math, you'll need logarithms and the exponential function. 

## Preparation

### Setting up the chart

  * Create a new project.
  * Since we will get some long axis labels increase the size of the form to something like 540 x 320 pixels.
  * Add a `TChart` component, align it to `alClient`, set its `BackColor` to `clWhite` and the `Grid.Color` of each axis to `clSilver`.
  * Add a title to the x axis ("Year of market introduction") and to the y axis ("Number of transistors").
  * Use the text "Progress in Microelectronics" as the chart's title.
  * Set these font styles to `fsBold`.
  * Show a reference for our data in the footer. Use the chart's property [`Foot`](<TAChart_documentation.md> "TAChart documentation") for that. Note that the property editor of `Foot.Text` (as well as that of `Title.Text`) allows to enter multi-lined titles.



[![TAChart LogAx Tutorial1.png](https://wiki.freepascal.org/images/1/17/TAChart_LogAx_Tutorial1.png)](</File:TAChart_LogAx_Tutorial1.png>)

### Creating a point series

We want to draw each TDataRecord as a single data point - this is called "point series" in other charting programs. The TAChart series editor does not offer this type of series because [`TLineSeries`](<TAChart_documentation.md> "TAChart documentation") can do this as well. We only have to set its property `ShowPoints` to `true` and to turn off the connecting lines (`LineType` = `ltNone`). The symbols are determined by the property `Pointer`. 

So, add a LineSeries to the form and set the properties to create a point series. Additionally, let's set the `Pointer.Brush.Color` to `clRed`, and the `Pointer.Style` to `psCircle` to draw a red circle at each data point. 

## Entering data

### ListChartSource

There are many ways that data could be entered into the chart. We will use a [ListChartSource](<TAChart_documentation.md> "TAChart documentation") today. Usually data are loaded into this source dynamically at run-time, for example after reading a file. In our project, however, we want to enter them at design-time by means of the data point editor of the ListChartSource. This has the main advantage that all our changes that we do with the chart are displayed immediately, there is no need to compile the project. As you will see, however, there are some drawbacks as well. 

The ListChartSource stores the data to be plotted in a list - that's where its name comes from. The list items are so-called `TChartDataItem` objects containing for each data point: 

  * the `x` and `y` coordinates,
  * a `Text` to be used for data marks,
  * a `Color` that overrides the SeriesColor,
  * as well as a `YList` which contains additional y values which are needed in some specialized series types.



The ListChartSource is the same type of chart source that is used by many series classes internally. Having it as a separate component, however, has the big advantage that the same chart source can be used again for other series - we will need this feature later when applying the FitSeries. 

[![TAChart LogAx Tutorial2.png](https://wiki.freepascal.org/images/3/3b/TAChart_LogAx_Tutorial2.png)](</File:TAChart_LogAx_Tutorial2.png>)

We start by adding a TListChartSource to our form. This is the second icon in the chart component palette. Connect the series to this chart source by pointing its property `Source` to the new `ListChartSource1`. 

### The ListChartSource data points editor

[![TAChart LogAx Tutorial16.png](https://wiki.freepascal.org/images/a/af/TAChart_LogAx_Tutorial16.png)](</File:TAChart_LogAx_Tutorial16.png>)

Clicking at the ellipsis button next to the property `DataPoints` opens the datapoints editor of the chart source. This is a string grid into which you can enter the data. Type in the data from the preceding image. They are an excerpt from the web site mentioned in the introduction. This site shows the time of market introduction by year and month - for simplicity, I skipped the month and rounded to the nearest calendar year. Enter the years into column **X**. Column **Y** will get the number of transistors per chip. The name of each microprocessor will go into column **Text**. Since we do not want to give each data point an individual color we leave the column **Color** alone - but, of course, you can experiment with that feature. 

When you close the datapoints editor the series is automatically updated and displays the current data set. Wow! 

### Displaying datapoint marks

Why don't we see the processor names that we entered in the grid? [Data point marks](<TAChart_documentation.md> "TAChart documentation") are turned off by default. Select the series and go to `Marks`. Look at the subproperty `Style`, it is `smsNone` which means "off". Open the dropdown list. You will see a variety of options how the data points can be labeled. Play with these settings to learn what they mean. Here we want to use `smsLabel` which displays the `Text` field of the `TChartDataItem`. In order to assign **Marks.Style** through source code, `uses ... TAChartUtils` shall be added. 

There is also a connecting line between the label and the datapoint. Its color is white by default, that's why we cannot see it on the white background. The property name for this line is `LinkPen`. You may want to set the `LinkPen.Color` to `clGray`. 

The position of the marks is OK here, but you should know that the series has a property [`MarkPositions`](<TAChart_documentation.md> "TAChart documentation") to control positioning to some degree. 

[![TAChart LogAx Tutorial3.png](https://wiki.freepascal.org/images/a/a4/TAChart_LogAx_Tutorial3.png)](</File:TAChart_LogAx_Tutorial3.png>)

Maybe we compile the project at this time. 

There is nothing new - we saw everything already at design-time. This is a great advantage of working with the datapoints editor of a ListChartSource. 

There are two things which can be improved at this stage: 

  * The point labels are cut off at the edges of the chart. We can fix this by increasing the chart's `Margin.Left` and `Margin.Right` to 24. [`Margin`](<TAChart_documentation.md> "TAChart documentation") defines the space surrounding the inner plot area to keep it free from data points. There is also a property [`MarginExternal`](<TAChart_documentation.md> "TAChart documentation") which surrounds the outer border of the chart and can be used to change the distance to neighboring controls.
  * Since almost all data points are crowded at the bottom of the chart the plot is not very meaningful. This is why we need a **logarithmic axis**.



## Setting up a logarithmic axis

Plotting data on a logarithmic axis means that the logarithms of the data values are plotted, not the values directly. The y values in our diagram, for example, range between 2300 and 731 million. When we calculate the ([decadic](<http://www.wikipedia.org/wiki/Common_logarithm> "wikipedia:Common logarithm")/log10) logarithms, the range is only between about 3.3 and 7.1 - in such a diagram the data can be distinguished much easier. 

### TChartTransformations and LogarithmicAxisTransform

[![TAChart LogAx Tutorial4.png](https://wiki.freepascal.org/images/3/36/TAChart_LogAx_Tutorial4.png)](</File:TAChart_LogAx_Tutorial4.png>)

Calculating the logarithm can be done by TAChart automatically. The main advantage is that the original data units are drawn on the axis while the logarithms are used for plotting. 

In fact, there is an entire group of components for axis transformations: [`TChartAxisTransformations`](<TAChart_documentation.md> "TAChart documentation"). An axis transformation is a function which maps "real world" data in units as displayed on the axis (_"axis coordinates"_) to internal units that are common to all series in the same chart (_"graph coordinates"_). The axis coordinate of the transistor count of the 4004 processor, for example, is 2300, the graph coordinate is the logarithm of that number, i.e. log10(2300) = 3.36. 

TAChart provides a variety of transforms. In addition to the logarithm transform, there is a [linear transform](<TAChart_documentation.md> "TAChart documentation") which allows to multiply the data by a factor and to add an offset. The [auto-scaling transform](<TAChart_documentation.md> "TAChart documentation") is useful when several independently scaled series have to be drawn on the same axis. The [user-defined transform](<TAChart_documentation.md> "TAChart documentation") allows to apply any arbitrary transformation. 

[![TAChart LogAx Tutorial5.png](https://wiki.freepascal.org/images/5/55/TAChart_LogAx_Tutorial5.png)](</File:TAChart_LogAx_Tutorial5.png>)

Add a `TAChartTransformations` component to the form, double-click on it (or right-click on it in the object tree and select "Edit axis transformations"), click on "Add" and select "Logarithmic". This will create a `ChartAxisTransformations1LogarithmAxisTransform1` component - what a name! Anyway, there's a high chance that you will not have to type it... 

In the object inspector, you will see only a few properties - the most important one is `Base`. This is the base of the logarithm to be calculated. Change it to 10, since we want to calculate the [decadic logarithms](<http://www.wikipedia.org/wiki/Common_logarithm> "wikipedia:Common logarithm") \- this case comprises 99% of all logarithmic charts. 

A side-note: it is very important that you **save the project at this time**. Why? You will see in a minute... 

Now we must identify the axis which is to be transformed. For this purpose each axis has a property `Transformations`. In our case, the huge numbers are plotted on the y axis. So, go to the left axis and set its property `Transformations` to `ChartAxisTransformations1`. 

[![TAChart LogAx Tutorial17.png](https://wiki.freepascal.org/images/0/0d/TAChart_LogAx_Tutorial17.png)](</File:TAChart_LogAx_Tutorial17.png>)

Oh - what's that? An error message pops up reporting a range check error. Hopefully you had saved the project. If you click on Cancel Lazarus will shut down, everything will be lost. If you click on OK suddenly the chart is gone, it reappears when you click somewhere, but the faulty axis is hidden ... 

Are you getting desperate? 

This is the main disadvantage of working at design-time. If something goes wrong, there is no debugger, no indication of what caused the trouble. Since components are compiled into Lazarus you would have to debug the IDE. Sounds complicated... 

Let's sit down and think what we did. We assigned the logarithmic transform to the y axis. The transform does not yet have any connection to the data, therefore the data are still in "real" units, their maximum is 731 million. But the transform "thinks" that the data are already in graph units (logarithms). When it calculates the axis labels in axis units it takes hundreds of millions to the power of 10! That's what causes the range check error. However, the situation is not always so dramatic; the least thing that could happen is that the data are not transformed, but the axis is. 

What can we do against that? Each series has properties `AxisIndexX` and `AxisIndexY`. The transform can use this information to calculate the logarithms of the correct coordinates before the axis labels are updated. This solves our problem. 

So, click on Cancel to shutdown Lazarus. Restart and reload the project. In the saved state, the transformation is not yet connected to the axis. 

Now have a look at the object tree above the object inspector and you will see that the left axis has index 0. So set the series' `AxisIndexY` to this value. Although not necessary it may be a good idea to disarm also the x axis by assigning its index 1 to the `AxisIndexX` of the series - who can guarantee that we won't transform the x axis in the future? 

After this is done you can set the `LeftAxis.Transformation` to `ChartTransformations1` without the range check error. 

With the logarithm transform activated the data points spread nicely across the y axis range now. But what's wrong with the y axis labels? And the years on the x axis are too close and partly overlap. 

[![TAChart LogAx Tutorial6.png](https://wiki.freepascal.org/images/d/d0/TAChart_LogAx_Tutorial6.png)](</File:TAChart_LogAx_Tutorial6.png>)

Finding axis labels is a non-trivial task, in particular when transformations are active that heavily distort the axis intervals. Unfortunately, logarithmic axes belong to that group. Basically, there are two ways to control label positioning, an **automatic** and a **manual** way. 

### Automatic finding of axis labels

For automatic label positioning, each axis has a property [`Intervals`](<TAChart_documentation.md> "TAChart documentation") which gives access to several, partly mutually excluding parameters - please see [TAChart documentation](<TAChart_documentation.md> "TAChart documentation") for an explanation. In case of the logarithmic axis the issue is usually caused by the fact that the option `aipGraphCoordinates` is not set. This option, if set, enforces calculation of the tick intervals for the transformed data ("graph coordinates"), not the "real world" data ("axis coordinates"). So, set `aipGraphCoordinates` in the `LeftAxis.Intervals.Options`. The labels jump to more evenly distributed locations. 

[![TAChart LogAx Tutorial7.png](https://wiki.freepascal.org/images/f/f1/TAChart_LogAx_Tutorial7.png)](</File:TAChart_LogAx_Tutorial7.png>)

Depending on the size of your form you may get quite nice, or not so good labels. If you resize the form you will see some "crooked" labels jump in. 

You can improve the quality of label presentation in the following way: 

  * Increase the `Intervals.Tolerance`. This allows for varying spacing between tick marks.
  * Adjust the range, in pixels, in which the label distance can vary. This is defined by the properties `Intervals.MaxLength` and `Intervals.MinLength`. The optimum value depends on the size of the chart and on the range of the data. In our example project, good labels are obtained by setting these properties to 100 and 50, respectively. Usually `Intervals.MaxLength` provides better results.



In the same way, the overlapping year labels of the x axis can be addressed. Just increase the `BottomAxis.Intervals.MaxLength` to 70. 

What is left now is the "1" that appears at the y axis between "10000000" and "1E009". This is due to a bug in some FPC versions. If you have that as well, simply change the property `LeftAxis.Marks.Format`. This string is passed to the `Format` function to convert the numbers to strings. The format specifier "%0.0n" for example avoids that conversion error and, additionally, adds nice thousand separators to the labels which makes them much more readable. 

[![TAChart LogAx Tutorial8.png](https://wiki.freepascal.org/images/7/7e/TAChart_LogAx_Tutorial8.png)](</File:TAChart_LogAx_Tutorial8.png>)

### Manual finding of axis labels

This is the best we can do with automatic label positioning. It is not perfect because when we increase the height of the window, or zoom in, the half-decade values may appear, or the label interval may be two decades as in above figure. 

If you are not happy with that you have to use **manual axis label selection**. For this purpose, each axis has a property `Source` which can be linked to a ListChartSource containing only the allowed axes labels. So when this chart source contains only full-decade labels there is no risk of half-decade labels or omitting every other label. On the other hand, when you zoom into the chart you may come to a point where no labels are visible any more. 

Add a second ListChartSource to the form. You can use the DataPoints editor again to enter the full-decade numbers. This has the advantage that you can complete most of this project without writing a single line of code! 

But you could also populate the listsource easily in the `FormCreate` event: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    const
      MIN = 0;
      MAX = 12;
    var
      i: Integer;
      value: double;
    begin
      for i:=MIN to MAX do begin
        value := Power(10, i);
        ListChartSource2.Add(value, value);
      end;
    end;
    

This procedure adds powers of 10 in a wide enough range to the ListChartSource by means of its method `Add`. 

Connect `ListChartSource1` to `LeftAxis.Marks.Source` to activate the manual labels of the ListChartSource. You should also remove all flags from the `Options` property. Otherwise automatic tick finding will still be active to some degree. If you did not use the DataPoints editor you must compile to see the effect. 

[![TAChart LogAx Tutorial9.png](https://wiki.freepascal.org/images/5/51/TAChart_LogAx_Tutorial9.png)](</File:TAChart_LogAx_Tutorial9.png>)

### Minor tick marks

Very often minor tick marks are placed between the major tick marks. TAChart allows to add several sets of minor ticks to each axis. We only need one here. Go to `LeftAxis` and click on the ellipsis button next to the property `Minors`. This opens the editor for `Chart1.AxisList[0].Minors`. Click on "Add" and on the "M" in the list below. Now you can adjust the parameters in the object inspector to get "good" minor ticks. If the major ticks on a logarithmic axis are at full decades then the minor ticks usually are at 2, 3, 4,..., 8, 9, and, of course, powers of 10. This can be achieved easily by turning off all `Intervals.Options` except for `aipUseCount` and setting `Intervals.Count = 9`. Of course, this makes sense only when the major labels are fixed at full decades like in the manual approach above. 

Usually the plot gets too crowded by the minor grid which appears now, you should set the minor's `Grid.Visible` to `false`. 

[![TAChart LogAx Tutorial10.png](https://wiki.freepascal.org/images/5/5c/TAChart_LogAx_Tutorial10.png)](</File:TAChart_LogAx_Tutorial10.png>)

## Fitting

Now let's look for a relation between the data, i.e. we want to find a mathematical formula which describes the dependence of transistor count on market introduction year. This is called "fitting": we select a formula with parameters and adjust the parameters such that the deviation to the data is as small as possible. 

TAChart does not contain a full-fledged fitting engine. It uses the fitting routines from the FPC numerical library [numlib](<numlib.md> "numlib"). Therefore, TAChart cannot address all variants of fitting, but it covers the most important case, fitting of a [polynomial](<http://www.wikipedia.org/wiki/Polynomial> "wikipedia:Polynomial") by means of the [linear least squares technique](<http://www.wikipedia.org/wiki/Least_squares> "wikipedia:Least squares"). This is about the level available to Excel users when they add a "trend line" to their chart. 

### TFitSeries

TAChart provides a specialized [`TFitSeries`](<TAChart_documentation.md> "TAChart documentation") for fitting. This series has a property `FitEquation` which defines the formula that is used: 

  * `fePolynomial`: _y_ = _b_ 0 \+ _b_ 1 _x_ \+ _b_ 2 _x_ 2 \+ … + _b nx_ _n_. Specify the number of fitting parameters _a i_ by the property `ParamCount` = _n_ \+ 1.
  * `feLinear`: _y_ = _a_ \+ _bx_ \-- this is a special case of the general polynomial with _n_ = 1 and fitting parameters _a_ and _b_. It is made available as a separate item because straight lines define the most important fitting conditions.
  * `feExp`: _y_ = _a_ * _e bx_ \-- This equation can also be reduced to the polynomial case although this is not straightforward to see. But take the (natural) logarithm of this equation, and you get to ln(_y_) = _a_ \+ _bx_. Now when we fit ln(_y_) instead of _y_ we have the linear case again.
  * `fePower`: _y_ = _a_ * _x b_. Again, this can be reduced to a linear equation by a logarithmic transformation.



[![TAChart LogAx Tutorial11.png](https://wiki.freepascal.org/images/b/b5/TAChart_LogAx_Tutorial11.png)](</File:TAChart_LogAx_Tutorial11.png>)

Enough of theory. Let's add a FitSeries to the chart: double-click on the chart, and in the series editor click on "Add" and select the entry "Least squares fit series" from the dropdown list. 

At first, we need to tell the fit series where it finds its data. For this purpose, we connect the series' `Source` with `ListChartSource1` as we had done with the line series. You see: the same chart source can be used for several series. 

You hopefully remember the disaster above with the AxisIndex. So, set the `AxisIndexY` to the index of the left axis as we did with the line series. 

Which one of the four `FitEquation` possibilities do we select? Well, the data look like lying on a straight line. So let's select `feLinear`. 

[![TAChart LogAx Tutorial12.png](https://wiki.freepascal.org/images/0/06/TAChart_LogAx_Tutorial12.png)](</File:TAChart_LogAx_Tutorial12.png>)

Oops... We see the black fitted curve, but it does not "fit" at all. And we wanted a straight line, but we get a twisted curve. How can this be? 

The reason is the logarithmic transform that we applied to the y data. Therefore, our plot shows the logarithms, but the fit takes the "raw" data. We are effectively fitting the straight line to the data in the screenshot in the section Displaying datapoint marks where the log transform had not yet been introduced - it is clear that the line would not "fit" there. And when the fitted function is drawn the log transform distorts the straight line to the twisted curve that we see. 

On the other hand, if the **log data** follow a straight line our fitting law is not linear, but exponential. Let's set `FitEquation` to `feExp` and try again. 

[![TAChart LogAx Tutorial13.png](https://wiki.freepascal.org/images/a/ac/TAChart_LogAx_Tutorial13.png)](</File:TAChart_LogAx_Tutorial13.png>)

Ah - much better! 

Now we know that the exponential law, _y_ = _a_ * _x b_, is a good description of our data. But how do we get the fitting parameters _a_ and _b_? 

### Fit results

The fit series has a public array property `Param` which contains the fitting parameters. `a` is in `Params[0]`, and `b` is in `Params[1]`. Of course, these values are correct only when a valid fit has been performed. How do we know that? Well, the fit series provides an event `OnFitComplete` that is generated when the fit complete successfully. That's where we can evaluate the obtained fit parameters. As an example, let's display the fit results in a message along with the fit equation: 
    
    
    procedure TForm1.Chart1FitSeries1FitComplete(Sender: TObject);
    begin
      with Chart1FitSeries1 do
        ShowMessage(Format(
          'Fit result: a = %g, b = %g', [
          Param[0], Param[1]
        ]));
    end;
    

And that's what we get: 

[![TAChart LogAx Tutorial14.png](https://wiki.freepascal.org/images/d/d1/TAChart_LogAx_Tutorial14.png)](</File:TAChart_LogAx_Tutorial14.png>)

Now we want to calculate the time T until the number of transistors on a chip is doubled. As an exercise try to show that 
    
    
    T = ln(2) / b
    

It would be nice to show the doubling time as an additional line of the chart title. For this, we modify the `OnFitComplete` event handler as follows: 
    
    
    procedure TForm1.Chart1FitSeries1FitComplete(Sender: TObject);
    begin
      Chart1.Title.Text.Add(Format(
        'The number of transistors doubles every %.0f years',
        [ln(2) / Chart1FitSeries1.Param[1]]
      ));
    end;
    

[![TAChart LogAx Tutorial15.png](https://wiki.freepascal.org/images/3/3d/TAChart_LogAx_Tutorial15.png)](</File:TAChart_LogAx_Tutorial15.png>)

Wow! This is Moore's law: "The number of transistors per chip doubles every two years"... 

## Source code

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
      //RequireDerivedFormResource := True;
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    end.
    

### Unit1.pas
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, TAGraph, TASeries, TASources, Forms, Controls,
      Graphics, Dialogs, TACustomSource, TATransformations, TAFuncSeries;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Chart1: TChart;
        Chart1FitSeries1: TFitSeries;
        Chart1LineSeries1: TLineSeries;
        ChartAxisTransformations1: TChartAxisTransformations;
        ChartAxisTransformations1LogarithmAxisTransform1: TLogarithmAxisTransform;
        ListChartSource1: TListChartSource;
        ListChartSource2: TListChartSource;
        procedure Chart1FitSeries1FitComplete(Sender: TObject);
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
    
    uses
      math;
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    const
      MIN = 0;
      MAX = 12;
    var
      i: Integer;
      value: double;
    begin
      for i:=MIN to MAX do begin
        value := IntPower(10, i);
        ListChartSource2.Add(value, value);
      end;
      Chart1FitSeries1.ExecFit;
    end;
    
    procedure TForm1.Chart1FitSeries1FitComplete(Sender: TObject);
    begin
      {
      with Chart1FitSeries1 do
        ShowMessage(Format(
          'Fit result: a = %g, b = %g', [
          Param[0], Param[1]
        ]));
      }
      Chart1.Title.Text.Add(Format(
        'The number of transistors doubles every %.0f years',
        [ln(2) / Chart1FitSeries1.Param[1]]
      ));
    end;
    
    end.
    

### Unit1.lfm
    
    
    object Form1: TForm1
      Left = 244
      Height = 356
      Top = 193
      Width = 552
      Caption = 'Form1'
      ClientHeight = 356
      ClientWidth = 552
      OnCreate = FormCreate
      LCLVersion = '1.1'
      object Chart1: TChart
        Left = 0
        Height = 356
        Top = 0
        Width = 552
        AxisList = <    
          item
            Grid.Color = clSilver
            Marks.Format = '%0:.0n'
            Marks.Source = ListChartSource2
            Marks.Style = smsCustom
            Minors = <        
              item
                Grid.Visible = False
                Intervals.Count = 9
                Intervals.MinLength = 5
                Intervals.Options = [aipUseCount]
              end>
            Title.LabelFont.Orientation = 900
            Title.LabelFont.Style = [fsBold]
            Title.Visible = True
            Title.Caption = 'Number of transistors'
            Transformations = ChartAxisTransformations1
          end    
          item
            Grid.Color = clSilver
            Intervals.MaxLength = 60
            Alignment = calBottom
            Minors = <>
            Title.LabelFont.Style = [fsBold]
            Title.Visible = True
            Title.Caption = 'Year of market introduction'
          end>
        BackColor = clWhite
        Foot.Alignment = taLeftJustify
        Foot.Brush.Color = clBtnFace
        Foot.Font.Color = clBlue
        Foot.Text.Strings = (
          'Source:'
          'http://www.intel.com/pressroom/kits/quickreffam.htm'
        )
        Foot.Visible = True
        Margins.Left = 24
        Margins.Right = 24
        Title.Brush.Color = clBtnFace
        Title.Font.Color = clBlue
        Title.Font.Style = [fsBold]
        Title.Text.Strings = (
          'Progress in Microelectronics'
        )
        Title.Visible = True
        Align = alClient
        ParentColor = False
        object Chart1LineSeries1: TLineSeries
          Marks.Format = '%2:s'
          Marks.LinkPen.Color = clGray
          Marks.Style = smsLabel
          AxisIndexY = 0
          LineType = ltNone
          Pointer.Brush.Color = clRed
          Pointer.Style = psCircle
          ShowPoints = True
          Source = ListChartSource1
        end
        object Chart1FitSeries1: TFitSeries
          AxisIndexX = 1
          AxisIndexY = 0
          FitEquation = feExp
          OnFitComplete = Chart1FitSeries1FitComplete
          ParamCount = 2
          Source = ListChartSource1
        end
      end
      object ListChartSource1: TListChartSource
        DataPoints.Strings = (
          '1972|2300|?|4004'
          '1974|6000|?|8080'
          '1978|29000|?|8086'
          '1982|134000|?|80286'
          '1986|275000|?|80386'
          '1989|1200000|?|80486'
          '1993|3100000|?|Pentium'
          '1997|7500000|?|Pentium II'
          '2001|42000000|?|Xeon'
          '2006|152000000|?|Core Duo'
          '2009|731000000|?|Core i7'
        )
        left = 240
        top = 40
      end
      object ChartAxisTransformations1: TChartAxisTransformations
        left = 243
        top = 96
        object ChartAxisTransformations1LogarithmAxisTransform1: TLogarithmAxisTransform
          Base = 10
        end
      end
      object ListChartSource2: TListChartSource
        left = 243
        top = 176
      end
    end

---

_Source: [https://wiki.freepascal.org/TAChart_Tutorial%3A_ListChartSource%2C_Logarithmic_Axis%2C_Fitting](https://web.archive.org/web/20250121210031/https://wiki.freepascal.org/TAChart_Tutorial%3A_ListChartSource%2C_Logarithmic_Axis%2C_Fitting)_
