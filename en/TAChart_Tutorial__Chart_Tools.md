# TAChart Tutorial: Chart Tools

│ **English (en)** │

## Contents

  * 1 Introduction
  * 2 Preparation
  * 3 Zooming and Panning - the easy way
    * 3.1 Zooming
    * 3.2 Panning
  * 4 Fundamentals on chart tools
    * 4.1 What are chart tools?
    * 4.2 Properties of chart tools
    * 4.3 TChartToolset
  * 5 Zooming by the mouse-wheel
    * 5.1 Some properties
    * 5.2 Using the ExtentSizeLimit
    * 5.3 Horizontal and vertical zooming
  * 6 Panning by the mouse-wheel
  * 7 Reading data from the series
    * 7.1 Using a TDataPointClickTool
    * 7.2 Showing permanent labels
    * 7.3 Removing permanent labels
  * 8 Related tutorials
  * 9 Source code
    * 9.1 project.lpr
    * 9.2 unit1.pas
    * 9.3 unit1.lfm



## Introduction

[![ZoomPan06.png](https://wiki.freepascal.org/images/c/c6/ZoomPan06.png)](</File:ZoomPan06.png>)

Charts are powerful tools to display relationships between data. They get even more powerful if the user can interact with them, for example: 

  * by **zooming** into a crowded series to display more details,
  * by **panning** the visible viewport to other regions,
  * by **displaying data values** from the series or measuring characteristic parameters of the plot.



The [TAChart](<TAChart.md> "TAChart") package is equipped with a powerful collection of tools to help creating interactive charts. In this tutorial, we want to show how to apply these tools to create interactive charts. 

If you are not familiar with TAChart we recommend that you have a look at the [Getting started tutorial](<TAChart_Tutorial__Getting_started.md> "TAChart Tutorial: Getting started"). Of course, you must have some experience with Lazarus and FPC. 

## Preparation

We need a chart to play with. Instead of creating a new chart let's reuse the simple chart of the [Getting started tutorial](<TAChart_Tutorial__Getting_started.md> "TAChart Tutorial: Getting started"). In this project we had created a chart of some mathematical functions for x between -10 and 10. The functions were drawn by means of `TLineSeries` storing their data in the built-in list source. Copy the project files (`project1.lpi, project1.lpr, unit1.pas, unit1.lfm`) into a separate folder for the new tutorial. 

[![tachart getting started step6.png](https://wiki.freepascal.org/images/b/b0/tachart_getting_started_step6.png)](</File:tachart_getting_started_step6.png>)

## Zooming and Panning - the easy way

### Zooming

Zooming is very easy -- you don't have to do anything to implement it since it is built into the heart of the chart component. You just have to drag a rectangle with the **left** mouse button down around the feature of interest that you want to see in detail. Keep in mind that you have to drag the mouse from the top-left to the right-bottom corner of the rectangle to get the zoom effect. After you release the mouse button, the region is blown up to fill the entire chart. Easy. 

[![ZoomPan01.png](https://wiki.freepascal.org/images/3/34/ZoomPan01.png)](</File:ZoomPan01.png>) [![ZoomPan02.png](https://wiki.freepascal.org/images/f/f8/ZoomPan02.png)](</File:ZoomPan02.png>)

You can repeat this procedure to zoom into the chart deeper and deeper. 

If you want to restore the original viewport just click into the chart with the left button, or left-drag a rectangle in any direction, except of course from top-left to right-bottom -- this would zoom in even further. 

Maybe, for some reason, you do not like this behavior. `TChart` has a property `AllowZoom` with which you can turn off the zooming capability. Beyond that, there is nothing else to control the default zooming action. But don't give up: a bit further down we will discuss chart tools, and they give you almost unlimited access to any interactive feature. 

### Panning

After you have zoomed into a chart you may want to shift the viewport a bit to find a better view. This operation is called "panning". For the built-in panning operation you hold down the **right** mouse button and drag the viewport into the desired direction. More panning options are available by means of the Chart Tools. 

[![ZoomPan03.png](https://wiki.freepascal.org/images/8/8b/ZoomPan03.png)](</File:ZoomPan03.png>)

As with zooming you can restore the original viewport by clicking into the chart, or by dragging a rectangle in any direction, except from top-left to bottom-right. Note that you use the left mouse button for "un-panning" although you had used the right button for panning. 

Since Lazarus v2.1+, TChart has a property `AllowPanning` to disable panning. As we will see shortly, panning can be turned off also with the chart tools, so there is no real disadvantage for older versions. 

## Fundamentals on chart tools

### What are chart tools?

As you have seen, to add zooming and panning capabilities to your program can't be easier -- there's nothing to do, they are already there. But on the other hand, usage of the built-in routines is quite limited: zooming is only possible by using the left mouse button, panning by using the right mouse button; no mouse-wheel support; no vertical-only or horizontal-only zooming; no reading of data values, etc. 

For more versatile user interaction a set of [chart tools](<TAChart_documentation.md> "TAChart documentation") has been added to TAChart. Each tool is specialized to some specific action. From the user point of view, there are two types of tools: [Extent tools](<TAChart_documentation.md> "TAChart documentation") and [data tools](<TAChart_documentation.md> "TAChart documentation"). 

  * [**Extent tools**](<TAChart_documentation.md> "TAChart documentation") modifiy the **extent** of the chart -- this is the visible viewport -- i.e. they are responsible for zooming and panning. For zooming, you can select from [TZoomDragTool](<TAChart_documentation.md> "TAChart documentation"), [TZoomClickTool](<TAChart_documentation.md> "TAChart documentation"), or [TZoomMouseWheelTool](<TAChart_documentation.md> "TAChart documentation"); for panning, you can use [TPanDragTool](<TAChart_documentation.md> "TAChart documentation"), [TPanClickTool](<TAChart_documentation.md> "TAChart documentation"), or [TPanMouseWheelTool](<TAChart_documentation.md> "TAChart documentation"). Note that there are no dedicated tools for unzooming or "un-panning", this feature is built into the tools used for zooming.
  * [**Data tools**](<TAChart_documentation.md> "TAChart documentation") give interactive access to data displayed in the series, or even allow to modify the data: you will find [TDataPointClickTool](<TAChart_documentation.md> "TAChart documentation"), [TDataPointDragTool](<TAChart_documentation.md> "TAChart documentation"), [TDataPointHintTool](<TAChart_documentation.md> "TAChart documentation"), [TDataPointCrosshairTool](<TAChart_documentation.md> "TAChart documentation"), or [TDataPointDistanceTool](<TAChart_documentation.md> "TAChart documentation").
  * If nothing else fits your needs, [TUserDefinedTool](<TAChart_documentation.md> "TAChart documentation") allows to build your own tool according to your specific requirements.



### Properties of chart tools

All these tools have a property called **`Shift`** containing a set of shift states that activate the tool (see the standard LCL enumeration `TShiftState`). 

An example: If the tool's `Shift` contains `ssCtrl` and `ssLeft`, then the tool becomes active when the user presses the `Ctrl` key _and_ left mouse button. Note that all conditions must be met, the tool does not activate if the left mouse button is pressed alone. Each tool must have its own set of unique `Shift` combinations. 

A note on the keyboard states: In order to receive messages on keyboard events the chart must have the focus. If tools do not work properly try to call `Chart.SetFocus`. There is also a property `AutoFocus` which automatically focuses the chart when the user moves the mouse into the chart. The disadvantage of `AutoFocus` should not be left unmentioned: if you type some text into a memo, move the mouse over the chart and want to continue typing you will notice that the memo has lost the focus, and the keystrokes typed will be ignored... 

There is another condition that must be met for the tool to become active: Its property **`Enabled`** must be set to `true`, but this is the default setting. This property is useful when several tools share the same `Shift` combinations and are activated by some other code, for example by toolbar buttons. 

Finally, all tools have in common various **events** that are fired before or after a key or mouse button is pressed or released; in case of the mouse, there are also events that fire before or after the mouse is moved. Usually, chart tools work "out of the box", and these events are not needed. But they are necessary when some functionality is to be added. We will see examples in the course of this tutorial. 

### TChartToolset

[![TChartToolset-Icon2.png](https://wiki.freepascal.org/images/9/9c/TChartToolset-Icon2.png)](</File:TChartToolset-Icon2.png>)

Chart tools are collected in a special component, `TChartToolset`. You can find it somewhere in the center of the "Chart" page of the Lazarus component palette, it has the icon with the red screw driver. 

The `TChartToolset` manages communications between tools and chart. Therefore, the chart has a property `Toolset` which has to be assigned to the `TChartToolset` instance. Note that this is a 1:n relationship, i.e. one toolset can handle multiple charts, a very nice feature! I should also mention that I usually forget to make that assignment -- and have to spend time in seeking for the reason why the tools do not work... 

## Zooming by the mouse-wheel

But now, enough of theory! Let's practise instead. 

Let us add tools to our demo chart. For this purpose we need a `TChartToolset` [![TChartToolset-Icon.png](https://wiki.freepascal.org/images/4/40/TChartToolset-Icon.png)](</File:TChartToolset-Icon.png>). Drop it onto your form and link it to the `Toolset` property of the chart. 

Although we did not yet add any tools to the toolset, maybe this is a good point to compile and run our program. Try to zoom or pan, as we did above. Oh -- no longer working! This is an important observation: by adding a `TChartToolset` to the form we replaced the default toolset built into the chart. So, the built-in tools for zooming and panning are not operational any more. 

In order to re-establish zooming and panning for our application we have to add one (or more) zooming or panning tools to the toolset. 

What do want to achieve? Maybe, instead of using the left and right mouse buttons we could take advantage of the mouse wheel for zooming and panning. And we could assign the left mouse button to a tool which allows us to read data values from the series while clicking onto a data point. 

To add a mouse-wheel tool for zooming to the chart you double-click on the `ChartToolset1` on the form, or double-click on it on the object tree of the object inspector, or right-click on it and select _"Edit tools"_ , as you do with all collection editors of TAChart. Press _"Add"_ button, select _"Zoom by mouse-wheel"_ from the dropdown list, and a new item `ChartToolset1ZoomMousewheelTool1` appears in the object tree as a child of `ChartToolset1`. 

[![ChartToolset Editor.png](https://wiki.freepascal.org/images/2/29/ChartToolset_Editor.png)](</File:ChartToolset_Editor.png>)

If you would run the program at this time, zooming still would not work because the tool has properties `ZoomFactor` and `ZoomRatio` that define the amount of zooming per mouse-wheel step, and they are still at their default of 1. So, set ` ZoomFactor` to 1.1, and run the program. Now zooming works fine. 

When you scroll the mouse-wheel towards yourself the chart is magnified as if you bring the chart closer to you. Many programs work the other way round: for the same scrolling direction the chart gets smaller as if you move away from the chart. If you want this behavior set the `ZoomFactor` to a number less then 1, for example 0.9. (We will stick to 1.1 for the rest of the tutorial, though). 

#### Some properties

Let's have a look at the properties of the zoom mousewheel tool. There are **`AnimationInterval`** and **`AnimationsSteps`** which can be used to achieve an animation of the zooming effect. 

What is **`FixedPoint`**? If `true` the center of the zooming operation is the position of the mouse cursor. So, if you want to zoom into a particular feature in the chart move the mouse to the feature and rotate the mouse wheel. If `false` the zooming is always relative to the chart center, no matter where the mouse cursor is. In this case you have to be very careful because it is extremely easy to lose orientation in the chart. 

#### Using the ExtentSizeLimit

[![ZoomPan04.png](https://wiki.freepascal.org/images/a/a3/ZoomPan04.png)](</File:ZoomPan04.png>)

When you zoom _out_ you will expose empty regions. Because the user can zoom out further and further the chart may eventually disappear. To prevent this we should restrict the axis ranges such that always data are displayed. 

For this purpose, TChart has a property **`ExtentSizeLimit`**. Our chart has an x extent from -10 to 10. So we set `ExtentSizeLimit.XMax` to the difference, 20, and activate this setting by `ExtentSizeLimit.UseXMax = true`. Now, we can no longer zoom out beyond the original _x_ extent. Similarly, we should restrict the extent of the _y_ axis to 2. 

In the same way, we could prevent to zoom _into_ the chart beyond an x extent of, say, 0.01. For this we would set the properties `ExentSizeLimit.XMin` and `ExtentSizeLimit.UseXMin` accordingly. 

#### Horizontal and vertical zooming

Why are there two parameters that control the amount of zooming, **`ZoomFactor`** and **`ZoomRatio`**? This is because the tool can be used for non-proportional zooming: the _x_ zoom factor is given by `ZoomFactor` alone while the _y_ zoom factor is determined by the product `ZoomFactor*ZoomRatio`. As long as `ZoomRatio=1`, zooming occurs isotropically in all directions. When you set `ZoomFactor=1` and `ZoomRatio=1.1` we leave the _x_ direction unchanged, but zoom only along the _y_ direction. Or, if we set `ZoomRatio = 1/ZoomFactor` zooming occurs only horizontally along the _x_ axis. 

Hey -- these would be nice features for our program! When we look at the keyboard the `⇧ Shift`, `Ctrl` and `Alt` keys form some kind of coordinate system: `Ctrl` is the origin, `⇧ Shift` is the _y_ direction, `Alt` is the _x_ direction. So we could assign the `⇧ Shift` key to vertical zooming, the `Alt` key to horizontal zooming, and "no key" to isotropic zooming -- that's easy to remember. 

How can we implement this feature? Add two more ZoomMouseWheelTools to the form. The first one will do the vertical zoom, so name it `ChartToolset1ZoomMousewheelTool_vert`, select `ssShift` in the `Shift` property, and set `ZoomRatio = 1.1` (keep `ZoomFactor` at 1). The other tool will be for the horizontal zoom, name it `ChartToolset1ZoomMousewheelTool_hor`, set `Shift` to `ssAlt`, and set `ZoomFactor = 1.1` and `ZoomRatio = 0.90909090909` (which is approximately equal to 1/1.1). 

When you run the program you may notice that zooming stops working after you have pressed the `Alt` key. This is caused by the fact that the `Alt` key plays a special role for menu key handling. To reactivate the program you have to click into the chart. Or select another activation key for the horizontal zoom tool. 

## Panning by the mouse-wheel

Let's move on the panning. Why don't we use the mousewheel also for panning? Here, panning can go only in one direction -- either _x_ or _y_. So let us add two `TPanMouseWheelTool`s and replace numbers at the end of their names by "_vert" and "_hor". The tool for vertical panning should be activated by the `⇧ Shift` key again, the tool for horizontal panning by the `Alt` key. 

There is a property `WheelUpDirection` which defaults to `pdUp`. This means that scrolling of the mouse-wheel is translated to a vertical panning direction. This setting is not correct for the horizontal panning tool, select `pdRight` or `pdLeft` instead. 

Run the program. When you rotate the mouse wheel with `⇧ Shift` or `Alt` down the chart zooms, but does not pan. What's wrong? 

The reason is that we are using the same `Shift` settings for the zooming and panning tools. Well, we could use, for example, the `Ctrl` key and assign it additionally to the panning tools. Then vertical panning, for example, would occur by pressing `Ctrl`+`⇧ Shift`, vertical zooming would occur with `⇧ Shift` alone. 

Let's go another way here to demonstrate usage of the `Enabled` property. Add a toolbar to the form with two buttons to activate either zooming or panning. Rename the first button to `ZoomToolbutton` and set `Down` to `true`. Rename the second button to `PanToolbutton`. For both buttons, set the following properties: 

  * `Grouped = true`
  * `Style = tbsCheck`



and assign their `OnClick` to the following event handler: 
    
    
    procedure TForm1.ZoomPanToolbuttonClick(Sender: TObject);
    begin
      ChartToolset1ZoomMouseWheelTool_iso.Enabled := ZoomToolbutton.Down;
      ChartToolset1ZoomMouseWheelTool_vert.Enabled := ZoomToolbutton.Down;
      ChartToolset1ZoomMouseWheelTool_hor.Enabled := ZoomToolbutton.Down;
    
      ChartToolset1PanMouseWheelTool_vert.Enabled := PanToolbutton.Down;
      ChartToolset1PanMouseWheelTool_hor.Enabled := PanToolbutton.Down;
    end;
    

This enables either the zooming or the panning tools, depending on which toolbutton is down. To synchronize the chart tools' `Enabled` with the buttons' `Down` you should disable both panning tools (or call `ZoomPanToolbuttonClick(nil)` in the `OnCreate` event of the form). 

Some observations when you run the program: 

  * It's a matter of taste, but maybe you feel that the vertical zoom direction is **opposite** to the direction the mouse wheel is rotated. You can change this by setting the `WheelUpDirection` of the vertical panning tool to `pdDown`.
  * **Empty chart regions without data** can become visible again. This is because the `ExtentSizeLimit` checks only the width and/or height of the extent. To take care of this the PanMousewheelTool has a set of properties `LimitToExtent`. Activate all options to disallow horizontal and vertical panning beyond the original extent with data.
  * Maybe you want to change the **speed** of panning. This can be done by adapting the `Step` property.
  * You may also notice that it is quite cumbersome to restore the original extent of the chart. It would be fine to have a button _"Reset"_ in the toolbar which undoes any zooming and panning operations. That's easy: add a third button to the toolbar, name it `RestoreToolbutton`, set its `Caption` to _"Reset"_ , and assign its `OnClick` event to the following procedure:


    
    
    procedure TForm1.ResetToolButtonClick(Sender: TObject);
    begin
      Chart1.ZoomFull;
    end;
    

The `ZoomFull` procedure restores the chart extent to its original value. Here's what our program looks like now: 

[![ZoomPan04a.png](https://wiki.freepascal.org/images/1/18/ZoomPan04a.png)](</File:ZoomPan04a.png>)

## Reading data from the series

You can add more zooming and panning tools to the toolset to get a practical user interface. But we will stop here with the extent tools and move on to the data tools. 

### Using a TDataPointClickTool

Our task is to read the x,y coordinates of the series underneath the mouse cursor. A neat tool to achieve this is the `TDatapointClickTool`. Add it to the toolset. Set `Shift` to `ssLeft` to activate it by a left button click. And add a `TStatusbar` to the form where we will display the requested information. 

Whenever a click occurs on (or sufficiently near) a data point the relevant data are stored in the tool. For example, there is a `Series` property which identifies onto which series the click occurred. Similarly, there is a property `PointIndex` which tells the index of the hit point within its series. The tool provides an `OnPointClick` event which fires when a point is clicked and from where we can query the requested information. We can use the following event handler to extract the information for displaying in the status bar: 
    
    
    procedure TForm1.ChartToolset1DataPointClickTool1PointClick(
      ATool: TChartTool; APoint: TPoint);
    var
      x, y: Double;
    begin
      with ATool as TDatapointClickTool do 
        if (Series is TLineSeries) then 
          with TLineSeries(Series) do begin
            x := GetXValue(PointIndex);
            y := GetYValue(PointIndex);
            Statusbar1.SimpleText := Format('%s: x = %f, y = %f', [Title, x, y]);
          end
        else
          Statusbar1.SimpleText := '';  
    end;
    

The parameter `ATool` passed to the event is rather general, thus we have to type-cast it to `TDatapointClickTool` to get access to the mentioned properties. Similarly, the `Series` is of a very basic type as well, another type-cast is needed. Be careful with type-casts, make sure that the class types are correct. Therefore, we check the types by means of `is`. 

When you run the program and click on a data point you will see its coordinates along with the title of its series in the status bar. 

[![ZoomPan05.png](https://wiki.freepascal.org/images/4/49/ZoomPan05.png)](</File:ZoomPan05.png>)

### Showing permanent labels

Unfortunately, there is no indication where exactly the click occurred. It would be better if we could display a label above the clicked point to show the requested information. 

Here we can take advantage of the fact that the series in our chart get their data from `TListChartSource`s. They have no special components on the form, because this is the default behavior, and each `TChartSeries` (from which `TLineSeries` inherits) has a non-visual built-in listsource which can be accessed by a (public) property `ListSource`. 

Why is this important? Because the items stored in a listsource provide extra storage for an additional text: 
    
    
    type
      TChartDataItem = object
        X, Y: Double;
        Text: String;
        // ... 
      end;
    

To modify the `Text` assigned to a data point at a given `index` you can call 
    
    
    ListSource.Item[index]^.Text := 'some text';
    

or, in later versions of Lazarus, there is a `SetText` method for abbreviation: 
    
    
    ListSource.SetText(index, 'some text');
    

(You can learn more about list sources from [ the tutorial on them](<TAChart_Tutorial__ListChartSource,_Logarithmic_Axis,_Fitting.md> "TAChart Tutorial: ListChartSource, Logarithmic Axis, Fitting").) 

Since we did not use this feature all `Text` entries are empty. But when we click onto a data point, we can transfer the coordinate info into the `Text` member of the ChartDataItem by adding the indicated line to our `OnPointClick` event handler of the datapoint click tool (BTW, we don't need the status bar any longer, so delete it and remove the corresponding code from the event handler): 
    
    
    procedure TForm1.ChartToolset1DataPointClickTool1PointClick(
      ATool: TChartTool; APoint: TPoint);
    var
      x, y: Double;
    begin
      with ATool as TDatapointClickTool do
        if (Series <> nil) then
          with (Series as TLineSeries) do begin
            x := GetXValue(PointIndex);
            y := GetYValue(PointIndex);
            { --- next line removed --- }
    //        Statusbar1.SimpleText := Format('%s: x = %f, y = %f', [Title, x, y]);
            { --- next line added --- }
            ListSource.Item[PointIndex]^.Text := Format('x = %f'#13#10'y = %f', [x,y]);
            ParentChart.Repaint; 
            // in newer Lazarus versions you can use (which already contains the Repaint):
            // ListSource.SetText(PointIndex, Format('x = %f'#13#10'y = %f', [x,y]));
          end;
    end;
    

Now we have to make sure that the labels are displayed. For this purpose, the `TChartSeries` which is an ancestor of `TLineSeries` has a property `Marks`. In the sub-properties you find the option `Style` which is set to `smsNone` by default, meaning that no labels are displayed. You see in the object inspector that there is a variety of information than can be displayed in the marks, but you'll need here the option `smsLabel` which shows the text of the ChartDataItems. Another modification may be useful: Set `LinkPen.Color` to some darker color, otherwise the connecting line between label and data point will not be visible on the white background. You may also want to play with some visual enhancements like `CalloutAngle` or `Shape`. 

You have to do the same changes for all three series in the chart. 

When you run the project and click on the series you get nice permanent labels. Please note that the markers stay in place even if you change the viewport by zooming or panning. 

[![ZoomPan06.png](https://wiki.freepascal.org/images/c/c6/ZoomPan06.png)](</File:ZoomPan06.png>)

### Removing permanent labels

Once in a while you may find that you have clicked at the wrong point and you may want to remove a label. 

The basic idea for this feature is as follows: add another `TDatapointClickTool`, assign it to the right mouse button and, in the `OnDatapointClick` event handler, we remove the text from the clicked ChartDataItem. Try doing it yourself -- you should now know all the necessary steps. 

## Related tutorials

  * [TAChart Tutorial: Getting started](<TAChart_Tutorial__Getting_started.md> "TAChart Tutorial: Getting started"): explains how the chart used here is created.
  * [TAChart Tutorial: ListChartSource, Logarithmic Axis, Fitting](<TAChart_Tutorial__ListChartSource,_Logarithmic_Axis,_Fitting.md> "TAChart Tutorial: ListChartSource, Logarithmic Axis, Fitting"): discusses usage of series `Marks`.
  * [TAChart Tutorial: ColorMapSeries, Zooming](<TAChart_Tutorial__ColorMapSeries,_Zooming.md> "TAChart Tutorial: ColorMapSeries, Zooming"): demonstrates other aspects of zooming and panning, like using the ZoomDragTool, PanDragTool and setting up a zoom history.



## Source code

### project.lpr
    
    
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
    

### unit1.pas
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, TAGraph, TASeries, TATools, Forms, Controls,
      Graphics, Dialogs, ComCtrls, types;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Chart1: TChart;
        ChartToolset1: TChartToolset;
        ChartToolset1DataPointClickTool1: TDataPointClickTool;
        ChartToolset1DataPointClickTool2: TDataPointClickTool;
        ChartToolset1PanMouseWheelTool_Hor: TPanMouseWheelTool;
        ChartToolset1PanMouseWheelTool_Vert: TPanMouseWheelTool;
        ChartToolset1ZoomMouseWheelTool_iso: TZoomMouseWheelTool;
        ChartToolset1ZoomMouseWheelTool_Hor: TZoomMouseWheelTool;
        ChartToolset1ZoomMouseWheelTool_Vert: TZoomMouseWheelTool;
        SinSeries: TLineSeries;
        CosSeries: TLineSeries;
        SinCosSeries: TLineSeries;
        ToolBar1: TToolBar;
        ResetToolButton: TToolButton;
        ZoomToolbutton: TToolButton;
        PanToolbutton: TToolButton;
        procedure ChartToolset1DataPointClickTool1PointClick(
          ATool: TChartTool; APoint: TPoint);
        procedure ChartToolset1DataPointClickTool2PointClick(
          ATool: TChartTool; APoint: TPoint);
        procedure FormCreate(Sender: TObject);
        procedure ResetToolButtonClick(Sender: TObject);
        procedure ZoomPanToolbuttonClick(Sender: TObject);
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
    
    procedure TForm1.ResetToolButtonClick(Sender: TObject);
    begin
      Chart1.ZoomFull;
    end;
    
    procedure TForm1.ChartToolset1DataPointClickTool1PointClick(
      ATool: TChartTool; APoint: TPoint);
    var
      x,y: Double;
    begin
      with ATool as TDatapointClickTool do
        if (Series <> nil) then
          with (Series as TLineSeries) do begin
            x := GetXValue(PointIndex);
            y := GetYValue(PointIndex);
    //        Statusbar1.SimpleText := Format('%s: x = %f, y = %f', [Title, x, y]);
            ListSource.Item[PointIndex]^.Text := Format('x = %f'#13#10'y = %f', [x,y]);
            ParentChart.Repaint;
          end;
    end;
    
    procedure TForm1.ChartToolset1DataPointClickTool2PointClick(
      ATool: TChartTool; APoint: TPoint);
    begin
      with ATool as TDatapointClickTool do
        if (Series <> nil) then
          with (Series as TLineSeries) do begin
            ListSource.Item[PointIndex]^.Text := '';
            ParentChart.Repaint;
          end;  
    end;
    
    procedure TForm1.ZoomPanToolbuttonClick(Sender: TObject);
    begin
      ChartToolset1ZoomMouseWheelTool_iso.Enabled := ZoomToolbutton.Down;
      ChartToolset1ZoomMouseWheelTool_vert.Enabled := ZoomToolbutton.Down;
      ChartToolset1ZoomMouseWheelTool_hor.Enabled := ZoomToolbutton.Down;
    
      ChartToolset1PanMouseWheelTool_vert.Enabled := PanToolbutton.Down;
      ChartToolset1PanMouseWheelTool_hor.Enabled := PanToolbutton.Down;
    end;
    
    end.
    

### unit1.lfm
    
    
    object Form1: TForm1
      Left = 554
      Height = 284
      Top = 341
      Width = 347
      Caption = 'Form1'
      ClientHeight = 284
      ClientWidth = 347
      OnCreate = FormCreate
      LCLVersion = '1.1'
      object Chart1: TChart
        Left = 0
        Height = 258
        Top = 26
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
        ExtentSizeLimit.UseXMax = True
        ExtentSizeLimit.UseXMin = True
        ExtentSizeLimit.UseYMax = True
        ExtentSizeLimit.XMax = 20
        ExtentSizeLimit.XMin = 0.01
        ExtentSizeLimit.YMax = 2
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
        Toolset = ChartToolset1
        Align = alClient
        ParentColor = False
        object SinSeries: TLineSeries
          Marks.Format = '%2:s'
          Marks.LinkPen.Color = clGray
          Marks.Style = smsLabel
          Title = 'y=sin(x)'
          LinePen.Color = clRed
        end
        object CosSeries: TLineSeries
          Marks.Format = '%2:s'
          Marks.LinkPen.Color = clGray
          Marks.Style = smsLabel
          Title = 'y=cos(x)'
          LinePen.Color = clBlue
        end
        object SinCosSeries: TLineSeries
          Marks.Format = '%2:s'
          Marks.LinkPen.Color = clGray
          Marks.Style = smsLabel
          Title = 'y=sin(x)*cos(x)'
          LinePen.Color = clGreen
        end
      end
      object ToolBar1: TToolBar
        Left = 0
        Height = 26
        Top = 0
        Width = 347
        Caption = 'ToolBar1'
        EdgeBorders = [ebBottom]
        ShowCaptions = True
        TabOrder = 1
        object ZoomToolbutton: TToolButton
          Left = 1
          Top = 0
          Caption = 'Zoom'
          Down = True
          Grouped = True
          OnClick = ZoomPanToolbuttonClick
          Style = tbsCheck
        end
        object PanToolbutton: TToolButton
          Left = 41
          Top = 0
          Caption = 'Pan'
          Grouped = True
          OnClick = ZoomPanToolbuttonClick
          Style = tbsCheck
        end
        object ResetToolButton: TToolButton
          Left = 69
          Top = 0
          Caption = 'Reset'
          OnClick = ResetToolButtonClick
        end
      end
      object ChartToolset1: TChartToolset
        left = 153
        top = 66
        object ChartToolset1ZoomMouseWheelTool_iso: TZoomMouseWheelTool
          ZoomFactor = 1.1
        end
        object ChartToolset1ZoomMouseWheelTool_Hor: TZoomMouseWheelTool
          Shift = [ssAlt]
          ZoomFactor = 1.1
          ZoomRatio = 0.90909090909091
        end
        object ChartToolset1ZoomMouseWheelTool_Vert: TZoomMouseWheelTool
          Shift = [ssShift]
          ZoomRatio = 1.1
        end
        object ChartToolset1PanMouseWheelTool_Hor: TPanMouseWheelTool
          Shift = [ssAlt]
          LimitToExtent = [pdLeft, pdUp, pdRight, pdDown]
          WheelUpDirection = pdLeft
        end
        object ChartToolset1PanMouseWheelTool_Vert: TPanMouseWheelTool
          Shift = [ssShift]
          LimitToExtent = [pdLeft, pdUp, pdRight, pdDown]
          WheelUpDirection = pdDown
        end
        object ChartToolset1DataPointClickTool1: TDataPointClickTool
          Shift = [ssLeft]
          OnPointClick = ChartToolset1DataPointClickTool1PointClick
        end
        object ChartToolset1DataPointClickTool2: TDataPointClickTool
          Shift = [ssRight]
          OnPointClick = ChartToolset1DataPointClickTool2PointClick
        end
      end
    end

---

_Source: [https://wiki.freepascal.org/TAChart_Tutorial%3A_Chart_Tools](https://web.archive.org/web/20230203110813/https://wiki.freepascal.org/TAChart_Tutorial%3A_Chart_Tools)_
