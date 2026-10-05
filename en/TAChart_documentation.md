# TAChart documentation

│ **English (en)** │  **[русский (ru)](<../ru/TAChart_documentation.md> "TAChart documentation/ru")** │  **[українська (uk)](</TAChart_documentation/uk> "TAChart documentation/uk")** │  **[中文（中国大陆） (zh_CN)](</TAChart_documentation/zh_CN> "TAChart documentation/zh CN")** │    
****

## Contents

  * 1 Overview
  * 2 Series
    * 2.1 Constant line series
    * 2.2 Basic series
      * 2.2.1 Line series
        * 2.2.1.1 Fast lines
        * 2.2.1.2 Point series
      * 2.2.2 Bar series
      * 2.2.3 Area series
      * 2.2.4 Polygon series
    * 2.3 Multi-value series
      * 2.3.1 Bubble series
      * 2.3.2 Box-and-whiskers series
      * 2.3.3 Open-high-low-close series
      * 2.3.4 Field series
      * 2.3.5 TStateSeries
    * 2.4 Radial series
      * 2.4.1 Pie series
      * 2.4.2 Polar series
    * 2.5 User-drawn series
    * 2.6 Functional series
      * 2.6.1 Function series
      * 2.6.2 Expression series
      * 2.6.3 B-spline series
      * 2.6.4 Cubic spline series
      * 2.6.5 Fit series
      * 2.6.6 Color map series
      * 2.6.7 Expression color map series
  * 3 Sources
    * 3.1 Sorted sources
    * 3.2 Multi-valued sources
    * 3.3 Skipping source items
    * 3.4 Error bars
    * 3.5 List source
    * 3.6 Random source
    * 3.7 User-defined source
    * 3.8 Database source
    * 3.9 Calculated source
    * 3.10 Interval source
    * 3.11 Date-time interval source
    * 3.12 Source optimization notes
  * 4 Coordinates and axes
    * 4.1 Axis transformations
      * 4.1.1 Linear and logarithmic transformation
      * 4.1.2 Auto-scaling transformation
      * 4.1.3 Cumulative normal distribution transformation
      * 4.1.4 User-defined transformation
    * 4.2 Date and time axes
    * 4.3 Axis ranges
    * 4.4 Axis intervals
    * 4.5 Axis position
    * 4.6 Axis titles
    * 4.7 Axis LabelSize
  * 5 Extents and margins
    * 5.1 Extents
    * 5.2 Extent limits
    * 5.3 Linked extents
    * 5.4 Margins
    * 5.5 Optimization notes
    * 5.6 Graphical explanation
  * 6 Tools
    * 6.1 Keyboard handling in tools
    * 6.2 Drawing mode
    * 6.3 Extent tools
      * 6.3.1 Zoom drag tool
      * 6.3.2 Zoom click tool
      * 6.3.3 Zoom mouse-wheel tool
      * 6.3.4 Pan drag tool
      * 6.3.5 Pan click tool
      * 6.3.6 Pan mouse wheel tool
    * 6.4 Data tools
      * 6.4.1 Data point drag tool
      * 6.4.2 Data point click tool
      * 6.4.3 Data point hint tool
      * 6.4.4 Data point crosshair tool
      * 6.4.5 Data point distance tool
    * 6.5 Chart element tools
      * 6.5.1 Axis click tool
      * 6.5.2 Title/footer click tool
      * 6.5.3 Legend click tool
      * 6.5.4 Datapoint marks click tool
    * 6.6 User defined tool
  * 7 Decorative elements
    * 7.1 Title and footer
      * 7.1.1 HTML
    * 7.2 Legend
      * 7.2.1 Legend item text
      * 7.2.2 User-defined legend items
    * 7.3 Arrows
    * 7.4 Transparency
    * 7.5 ChartStyles
  * 8 Marks
    * 8.1 Mark labels
      * 8.1.1 Multi-line marks
      * 8.1.2 HTML codes in marks
    * 8.2 Mark positions and attachment
    * 8.3 Rotation of marks
  * 9 Drawers
    * 9.1 TCanvas drawer
    * 9.2 TFPCanvas drawer
    * 9.3 SVG drawer
    * 9.4 OpenGL drawer
    * 9.5 Printer drawer
    * 9.6 AggPas drawer
    * 9.7 BGRABitmap drawer
    * 9.8 FPVectorial drawer
    * 9.9 WMF drawer
  * 10 Navigation
    * 10.1 Scroll bars
    * 10.2 Navigation panel
    * 10.3 Live View
  * 11 Additional components
    * 11.1 Legend panel
    * 11.2 Chart listbox
    * 11.3 Chart image list
    * 11.4 Chart combobox
  * 12 TeeChart compatibility
  * 13 Technical details
    * 13.1 Drawing order
    * 13.2 Coding style
      * 13.2.1 Spaces
      * 13.2.2 Lines
      * 13.2.3 Indentation and blocks
      * 13.2.4 Comments
      * 13.2.5 Names
      * 13.2.6 Variable declarations
      * 13.2.7 Classes and methods
      * 13.2.8 Hints and warnings
      * 13.2.9 Control flow



## Overview

[TAChart](<TAChart.md> "TAChart") is a package for drawing graphs, charts and other diagrams. It is [comparable in features](<Comparing_TAChart_with_Delphi's_TeeChart_Standard.md> "Comparing TAChart with Delphi's TeeChart Standard"), but not specifically compatible, with Delphi's TeeChart package. One substantial difference is that some features (e.g. data sources and axis transformations) are implemented via separate components instead of through simple chart properties. This design gives increased flexibility and opportunity for code re-use, but at the cost of a slightly more complex API. 

This document provides a concise but comprehensive overview of TAChart concepts, features and components. For a step-by-step introduction, see [TAChart Tutorial: Getting started](<TAChart_Tutorial__Getting_started.md> "TAChart Tutorial: Getting started") and [other tutorials](<TAChart.md> "TAChart"). 

The documentation describes the TAChart package as found in the latest development version of Lazarus. Some features may not yet be available in the official Lazarus release version. We recommend that you use this latest document version because it has corrected and improved older descriptions in previous versions, as well as the fact that it includes details about the newest features. 

Nevertheless, you can look at the [the old version](<TAChart_documentation.md>) of this document to see the state of the documentation around the time of the Lazarus 1.0 release. 

## Series

[![series hierarchy-1.png](https://wiki.freepascal.org/images/d/d2/series_hierarchy-1.png)](</File:series_hierarchy-1.png>)

Series are the central part of TAChart. Their hierarchy is illustrated in the figure. The series inheriting from TCustomChartSeries represents data taken from a ChartSource in graphical ways, such as lines or bars; the other series get their data in another way. 

### Constant line series

This is the simplest series type, representing an "infinite" vertical or horizontal line. It can be used as a "central axis" in function graphs, or as a draggable marker line. 

Also, by setting `Active`=true, `Pen.Style=psClear` and `UseBounds=true` and an appropriate `AxisIndexX`, it becomes an "axis extender", making sure that a given `Position` will always be included in the axis range. 

### Basic series

Basic series are most often used, and include line, bar and area series. All basic series can be "stacked" by using multi-valued source. Also, all basic series fully support rotation and 3-D drawing. 

#### Line series

`TLineSeries` can be used to draw a given set of points, optionally marking each data point with a symbol and/or connecting the data points with a line. The color of the line series is defined by the `SeriesColor`. 

`ColorEach` gives control over coloring inidividual data points and the adjacent line segments (the data point color is provided by the chart source): 

  * `ceNone` \-- no individual coloring, even if colors are assigend to the chart data item.
  * `cePoint` \-- colorize only the data point symbol, not the line segment.
  * `ceLineAfter` \-- apply the data point color to the line segment following the data point
  * `ceLineBefore` \-- apply the data point color to the segment before the data point
  * `cePointAndLineAfter, cePointAndLineBefore` \-- like before, but colorizes also the data point symbol.



You can get a "stepped" look by setting `LineType` property to `ltStepXY`, `ltStepYX`, `ltStepCenterXY` or `ltStepCenterYX`. 

Using a multi-valued data source `TLineSeries` can handle several y values for each x value. If `Stacked` is set to `true` then the individual y values are plotted in a stacked way, i.e., the second y value is added to the first one, the third y value to the second value etc. Note, however, unlike stacked bar or area series it may not be clear that the data values are stacked, and therefore stacking is usually not recommended for a line series. 

##### Fast lines

Some charting packages include a special "fast line" series to quickly draw line series from extremely large datasets (10000+ points). TAChart, by contrast, contains an optimized fast path inside standard line series code, which achieves comparable drawing speed. A line series will be drawn very fast if all of the following are true: 

  * There are no marks.
  * There are no pointers.
  * `LineType` is not `ltFromOrigin`.



Some operating systems/widgetsets may additionally require that `LinePen.Style=psSolid` and `LinePen.Width=1` and `LinePen.Cosmetic=true`. 

Additional speedups will be available if `Source.Sorted=true`, and if `AxisIndexX` and `AxisIndexY` are `-1` (i.e. if no axis transformations are applied). 

You can measure line speed drawing on your platform with the "line" demo. 

##### Point series

`TLineSeries` can be drawn as a "point series" showing only symbols at the position of the data points and hiding the connecting lines by setting `ShowPoints` to `True` and `LineType` to `ltNone`. Note that the `SeriesColor` applies only to the connecting lines; to specify the color of the points change the `Pointer.Brush.Color` for the fill and/or `Pointer.Pen.Color` for the border line color. The `Pointer` property also gives access to the shape (circle, box, etc) of the symbols (`Style`). 

#### Bar series

`TBarSeries` represents data as a set or bars, extending from `ZeroLevel` to data points. 

You can control bar width with `BarWidthPercent` property. Note that the it is measured relative to the neighboring bars. If the X values are not equidistant, bars will have varying width. To prevent that, set `BarWidthStyle=bwPercentMin`. 

You can draw multiple bar series side-by-side by using `BarOffsetPercent` property. 

Use multiple y values to create stacked bar series (`Stacked = true`). Beginning with Lazarus v2.0 multiple y values can also be drawn in a side-by-side arrangement (`Stacked = false`). 

In addition to rectangular bars the data can be represented by alternative shapes: cylinder, hexagonal prism, pyramid or cone - see property `BarShape` which supports these values: `bsRectangular, bsCylindrical, bsHexPrism, bsPyramid, bsConical`. Furthermore, the event `OnCustomDrawBar` allows to paint the bars in any user-defined manner. 

#### Area series

`TAreaSeries` represents data as a polygon extending downwards from the data points either to a `ZeroLevel` line or towards infinity (if `UseZeroLevel=false`). 

You can get a "stepped" look by setting the `ConnectType` property. 

Like `TBarSeries` and `TLineSeries`, an area series can accept several y values for the same x from a multi-valued data source. These y values are drawn in a stacked way if the property `Stacked` is set to `True` (its default value). Use a ChartStyles component to define the appearance of the individual layers. (Note that in Lazarus versions 1.8.x or older, ChartStyles do not colorize overlapping layers consistently). 

The property `Banded` (available in Lazarus v2.0+), if set to true, suppresses painting of the lowest stack level. This way a band of data values can be highlighted: use a chartsource with two y values and set the first y value to the lower and the second y value to the upper band level. Alternatively, the width of the band can be specified in the second y value if both properties `Banded` and `Stacked` are set to true. 

Multiple-y value support should be applied with caution: Stacking non-positive values may produce plots which are difficult to understand, and unstacked area series (`Stacked = false`) may suffer from data being covered by other y values. Also, the `ZeroLevel` should be included only in the lowest stack level to avoid unusual results. 

A 3D-like presentation of the series can be obtained if `Depth` is set to a positive value, something like 10 or 20. Note that the drawing algorithm is very simple and likely produces incorrect displays when curves cross, or stacking and banding are turned off. 

#### Polygon series

`TPolygonSeries` draws a filled polygon defined by the points of the series. It is intended to be the basis of contour plots or GIS plots (earth maps). The series supports both single and multiple polygons. Single polygons are closed automatically. Multiple Polygons are created by closing a first polygon and simply adding the points of one ore more subsequent polygon(s); each contributing polygon must be closed. Such polygonare are useful to display disconnected "islands" or to display "holes" in an outer shape; in the latter case the points of inner and outer polygons must have opposite orientations. 

### Multi-value series

Multi-value series require a multi-valued data source, and use additional X and/or Y values as extra parameters to draw complex shapes. 

#### Bubble series

`TBubbleSeries` represents data as circles of variable radius centered at data points. This series requires a source with `YCount` of at least 2, and uses the first additional Y value for the bubble size. 

Using the `BubbleRadiusUnits` property, the bubble radius can be mapped on the chart area in the following ways 

  * `bruX`: Bubbles are drawn as circles and the radius units are the same as used on the x axis.
  * `bruY`: like `bruX`, but the bubble radius is measured on the y axis.
  * `bruXY`: Bubbles are drawn as ellipses, the radius is understood to have the x axis units horizontally, and the y axis units vertically.
  * `bruPercentageRadius, bruPercentageArea`: Bubbles are drawn as circles. The data defining the bubble size are interpreted as being relative to the bubble radius (`bruPercentageRadius`) or to the bubble area (`bruPercentage`). The bubble size is scaled such that the radius of the largest bubble has a percentage `BubbleRadiusPercentage` of the plot area size.



Background and border of the bubbles are determined by `BubbleBrush` and `BubblePen`, respectively. But by means of the `OverrideColor` property the color of either interior or edge of each bubble can be changed individually to the color value stored in the chartsource. 

Since v3.99, the `Styles` property can be used to define all brush and pen parameters of bubbles indivudually. 

#### Box-and-whiskers series

`TBoxAndWhiskerSeries` represents data as rectangles with a central line and two T-like shapes protruding in both directions. Although in statistics a [box-and whiskers plot](<http://www.wikipedia.org/wiki/Box_plot> "wikipedia:Box plot") is supposed to be based on specific data quartiles, TAChart does not enforce this, allowing users to draw arbitrary plots. 

With some effort, box-and whiskers series may be used to represent other charts of different meaning but similar appearance, such as [Gantt diagrams](<http://www.wikipedia.org/wiki/Gantt_chart> "wikipedia:Gantt chart"). 

This series requires a source with `YCount` of at least 5, and uses Y values as follows: 

Index | Usage   
---|---  
0 | Lower whisker   
1 | Lower box bound   
2 | Central line   
3 | Upper box bound   
4 | Upper whisker   
  
Since Lazarus v2.3 a more flexible assignment of Y values to the parts of the box/whisker shape is possible by means of the properties `YIndexWhiskerMin`, `YIndexBoxMin`, `YIndexCenter`, `YIndexBoxMax` and `YIndexWhiskerMax`. Special combinations of Y values can be selected easily via the enumerated `YDataLayout` property: 

  * **bwlLegacy** : above assignment running the Y values from the lower to the upper end of the shape (default)
  * **bwlNormal** : here the ordinary Y value has index 0; this makes it easier to reuse the same chart source in other series types. It is followed by the Y indices for whisker min, box min, box max and whisker max. This selection will become the default in later versions.
  * **bwlCustom** : uses the Y index values as defined by the `YIndex*` properties.



#### Open-high-low-close series

`TOpenHighLowCloseSeries` represents data as vertical lines with two ticks, as described [here](<http://www.wikipedia.org/wiki/Open-high-low-close_chart> "wikipedia:Open-high-low-close chart"). 

It usually requires `YCount` of at least 4, and uses Y values as follows: 

Property | Default | Usage   
---|---|---  
YIndexLow | 0 | Lower point of line   
YIndexOpen | 1 | Left-facing tick position   
YIndexClose | 2 | Right-facing tick position   
YIndexHigh | 3 | Upper point of line   
  
Note that although Y values are supposed to be ordered ascending along the table above, the series does not enforce this and will draw any supplied data. 

#### Field series

`TFieldSeries` displays a vector at each data point. The vector is drawn as a line segment with an optional arrow. By default, the line is centered at the x,y coordinates stored in the chart source, its direction is given by additional values stored in the XList and YList of the chart source. When property `VectorCoordKind` is switched to `vckStartend` the line is defined by start and end points directly. In total, this series type requires a source with two x and y values: 

Source data item field | Usage   
---|---  
X | X coordinate of line center (or start point, if `VectorCoordKind = vckStartEnd`)   
Y | Y coordinate of line center (or start point)   
XList[0] | X coordinate of line direction (or end point, if `VectorCoordKind = vckStartEnd`)   
YList[0] | Y coordinate of line direction (or end point)   
  
The method `AddVector(x,y: Double; vectorx, vectory: Double; ALabel: String = ''; AColor: TColor = clTAColor)` helps to assign all values correctly to the internal list source. 

In order to avoid overlapping of vectors call the method `NormalizeVectors(ALength: Double)` to normalize their lengths such that the longest vector has the specified length. 

The arrow parameters are understood to be percentages of the vector length, i.e. the arrow becomes smaller for shorter vectors. 

#### TStateSeries

`TStateSeries` is supposed to display the states of machines or processes in a barseries-like way. Possible machine states are, for example, "production, "repair", "maintenance", "development". Each state is defined by a start and end value (usually: date/time), a description and a color value. In order to accomodate the start and end times, the chart source of the series must provide at least 2 x values. The most convenient way to enter data is by calling the `AddXY` method of the series which here accepts the start/end times, the description and the bar color. The y value provided normally should be the same for all steps of the same machine/process. However, it is also possible to increment the y value for each data point - this results in a staircase-like chart typial of Gantt charts. 

The `BarHeight` property indicates the height of the bars measured in y axis units. 

When a data point has equal start and end times it is assumed that this is a "milestone" in a Gantt chart and is painted as a diamond symbol. 

See the project in _demo/stateseries_ as a demostration example for a state and a Gantt chart. 

### Radial series

Radial series ignore axis transformations. You can see examples of radial series in the "radial" demo. 

#### Pie series

`TPieSeries` draws pie charts. 

For each data point, the pie series interprets the Y value as relative size of the slice, and the X value as the distance of the slice from the center of the pie, relative to the pie radius (only if `Exploded` property is true). 

Beginning at `StartAngle`, the pies run in counter-clockwise direction around the circle. When `AngleRange` is at its default of 360 the pies cover the full circle; a smaller value allows to use the circle only partially. 

The pie radius can either be set manually by the `FixedRadius` property, or be calculated automatically so that the whole series, including all labels, exactly fits the parent chart. 

A ring can be painted instead of a pie slice by giving the property `InnerRadiusPercent` a value greater than 0, but smaller than 100 - this determines the radius of the inner circle as percentage of the total radius. 

The slice colors are determined by either the data items' `Color` field or a hard-coded palette. Slice contours are drawn using the `EdgePen` property. Since v3.99, all brush and pen parameters can be controlled individually for each slice by means of the `Styles` property. 

There are several options for label positioning, controlled by the `MarkPositions` property: 

  * `pmpAround` \-- marks are drawn outside the pie, on the continuation of the radius vector for each slice
  * `pmpInside` \-- marks are drawn inside each slice
  * `pmpLeftRight` \-- marks are drawn directly to the left or to the right of slice



`MarkDistance` defines the distance between the marks starting point and the label. The starting point is defined by `MarkPositionCentered` to be either the pie perimeter (`false`) or center (`true`). `MarkDistancePercent`, if set to `true`, interprets `Marks.Distance` as percentage of the pie radius. Otherwise it is in image units. 

If `RotateLabels` is true, each label is additionally rotated so that (if `LabelFont.Orientation=0`) it is parallel to the radius vector of its slice. 

If the Y value of a data item is set to NaN, the item is skipped. If the X value is set to NaN, the item is not drawn, but the space for it is still reserved. This allows drawing of "partial" pie diagrams. 

Pie charts have limited support for 3-d drawing. The property `Depth` determines the thickness of the pie. `Orientation` supplements the normal view (`poNormal`) with an oblique view of the reclined (`poHorizontal`) and upright (`poVertical`) pie series, the latter two being controlled by property `ViewAngle` in degrees between 0 and 89. Since TAChart does not implement a true hidden line/hidden area algorithm there may be some chance of drawing artifacts for some combinations of parameters, in particular when `AngleRange` is not `360`. A slight change of `StartAngle` and/or `AngleRange` usually helps to fix this issue. 

#### Polar series

`TPolarSeries` represents data as points in [polar coordinates](<http://www.wikipedia.org/wiki/Polar_coordinate_system> "wikipedia:Polar coordinate system"). 

The origin of the polar coordinate system is defined in graph coordinates by `OriginX` and `OriginY` properties. 

For each data point, X value is interpreted as an angle in radians and Y value -- as a distance from the center. 

The TPolarSeries does not support axis transformations. 

Some additional properties: 

  * `CloseCircle`: If true the last point is connected with the first one.
  * `Filled`: If true the polygon of the series points is filled using the current `Brush`. If `CloseCircle` is false the fill goes to the origin.
  * `ShowPoints`: If true then the series pointer is drawn at the position of each data point.



### User-drawn series

Provides `OnDraw` and `OnGetBounds` events to allow arbitrary custom drawing on the TChart. Note that using `TChart.Canvas` directly is highly discouraged and will often not work as expected. 

### Functional series

Functional series are recommended way to draw functional plots as opposed to, for example, pre-calculating function data and using line series. They provide scale-independent controls of smoothness vs drawing speed. 

You can see examples of functional series in "func" demo. 

#### Function series

`TFuncSeries` represents a one-dimensional function defined by the `OnCalculate` event. The function is calculated for each `Step` pixels of the image, so you can use this property to increase either "smoothness" or drawing speed. 

The `Extent` property may be used to set both _x_ and _y_ extents for the purpose of calculating full extent of the chart. While drawing, however, this property is ignored, and the function will be displayed according to the current extent. 

The `DomainExclusions` property allows to exclude some intervals from the function domain. The TFuncSeries correctly draws discontinuity points set by `DomainExclusions`. Currently, `DomainExclusions` can only be set at run-time by calling the `AddRange` or `AddPoint` procedures. Intervals defined by `DomainExclusions` are closed by default, i.e. contain the end points; this means that the function is not calculated at the end points. If the domain exclusion must contain endpoints (as needed for the funtion `y = sqrt(x)`) call `AddRange` with the corresponding element of the set `TIntervalOptions = (ioOpenStart, ioOpenEnd)` as optional third parameter. 

The `DomainExclusions.Epsilon` property controls the distance between endpoint and last drawn point (of course, in case of an open interval, the curve begins immediately at the end point). `Epsilon` can be adjusted in the rare cases that the default value of 1E-6 does not produce correct results. 

#### Expression series

The `TExpressionSeries` is similar to the `TFuncSeries`, but the function is specified as a mathematical expression string such as `'sin(1.23456*x)'` (property `Expression`). The series creates an instance of the [FPExpressionParser](<How_To_Use_TFPExpressionParser.md> "How To Use TFPExpressionParser") to analyze the string and to calculate the function values. In this way functions can be plotted at designtime. 

The property `Params` allows to define parameters for the function term; in the object inspector it opens a collection editor in which every parameter can be specified by its name (as used in the expression) and its value. If, for example, a parameter `f` is defined to have the value `1.23456` then above expression can also be written as `'sin(f*x)'`. 

Similarly to `TFuncSeries`, domain exclusions are supported to define the regions at which the function cannot be calculated. But in agreement with mathematical conventions, `TExpressionSeries` requires in property `Domain` the region in which the function is **defined** (the complement of the domain exclusions). The region is specified by a string such as `'x >= 0'`. If several conditions exist they must be separated by a colon, e.g. `'x < -1; x > 1'`. The colon has function of a logical OR. An interval is defined like `'-1 < x < 1'`. The constant parts of the conditions can be mathematical expressions as well - the following example excludes the point π/2 from calculation of the function values: `'x <> pi/2'`. Decimal numbers must be entered with a dot as decimal separator, no matter what is defined in the language settings of the operating system. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Parameters and domains must be defined before the expression is entered, otherwise the parser will report an error

#### B-spline series

`TBSplineSeries` draws [B-spline](<http://www.wikipedia.org/wiki/B-spline> "wikipedia:B-spline") of given `Degree` using [De Boor's algorithm](<http://www.wikipedia.org/wiki/De_Boor%27s_algorithm> "wikipedia:De Boor's algorithm"). 

Spline segments shorter then `Step` pixels are represented by straight lines. 

#### Cubic spline series

`TCubicSplineSeries` draws the data curve as a [cubic spline](<http://www.wikipedia.org/wiki/Spline_interpolation> "wikipedia:Spline interpolation") using the standard _NumLib_ package of FPC. 

In Lazarus v2.0+, a property `SplineType` allows to select between [natural](<http://www.wikipedia.org/wiki/Spline_interpolation> "wikipedia:Spline interpolation") and [monotone Hermite](<http://www.wikipedia.org/wiki/Monotone_cubic_interpolation> "wikipedia:Monotone cubic interpolation") splines, the latter avoiding overshoot of the interpolation. 

The spline function is calculated for each `Step` pixels of the image, so you can use this property to increase "smoothness" or drawing speed. 

The data source must contain at least two points and have strictly increasing X coordinates. (Older Lazarus versions before v2.0 require at least 4 data points; for less points a polygon is drawn instead of a spline using the `BadDataPen` if the `csoDrawFewPoints` option is set.) 

If X values are unordered and the `csoDrawUnorderedX` option is set, the spline will be drawn ignoring offending points using `BadDataPen`. 

The options `csoExtrapolateLeft` and `csoExtrapolateRight` enable natural extrapolation to the left and to the right correspondingly. 

#### Fit series

`TFitSeries` performs [least squares fitting](<http://www.wikipedia.org/wiki/Least_squares> "wikipedia:Least squares") using calculation routines contained in the standard [Numlib](<numlib.md> "numlib") package from the FPC. The main fitting unit is _TAFitLib_ which is self-contained and can be applied also without TAChart. 

The fitting function is selected via the `FitEquation` property: 

  * `fePolynomial`: _y_ = _b_ 0 \+ _b_ 1 _x_ \+ _b_ 2 _x_ 2 \+ … + _b_ n _x_ n, where the value of _n_ is controlled by `ParamCount` property (`ParamCount = n + 1`).
  * `feLinear`: _y_ = _a_ \+ _bx_
  * `feExp`: _y_ = _a_ * e _bx_
  * `fePower`: _y_ = _a_ * _x_ b
  * `feCustom`: _y_ = _b_ 0X0(_x_) + _b_ 1X1(_x_) + _b_ 2X2(_x_) + … + _b_ nXn(_x_) where the value of _n_ is controlled by the `ParamCount` property (`ParamCount = n + 1`), and Xi(_x_), _i_ = 0.._n_ , are "basis functions" which are defined by the method `SetFitBasisFunc(AIndex: TFitFuncIndex; AFitFunc: TFitFunc; AFitFuncName: String)`. 
    * `AIndex` \- index _i_ of the basis function, `1..MaxInt`.
    * `AFitFuncName` is a text representation of the function to be used for the default legend.
    * `AFitFunc` \- address of the basis function Xi(_x_) which has the signature `function(x: ArbFloat; Param: Integer): ArbFloat` with `ArbFloat` being the general floating-point data type in FPC's [Numlib](<numlib.md> "numlib") (unit _typ_). Some basis functions are readily available in unit _TAFitLib_ :


    
    
    function FitBaseFunc_Const(x: ArbFloat; Param: Integer): ArbFloat;   // constant term
    function FitBaseFunc_Linear(x: ArbFloat; Param: Integer): ArbFloat;  // linear term (x)
    function FitBaseFunc_Square(x: ArbFloat; Param: Integer): ArbFloat;  // square term (x^2)
    function FitBaseFunc_Cube(x: ArbFloat; Param: Integer): ArbFloat;    // cubic term (x^3)
    function FitBaseFunc_Poly(x: ArbFloat; Param: Integer): ArbFloat;    // power term (x^Param)
    function FitBaseFunc_Sin(x: ArbFloat; Param: Integer): ArbFloat;     // sin(x*Param) 
    function FitBaseFunc_Cos(x: ArbFloat; Param: Integer): ArbFloat;     // cos(x*Param)
    

By default, the parameters, `b0, b1, ..., a, b` are varied by the fitting process such that the mean square deviation between data and fitted curves is minimized. However, it is also possible to hold one or more fitting parameters constant, e.g. in order to force a fitted straight line to run through the origin. For this purpose, the values of the constant parameters must be specified in the ';' or '|' delimited list of string values of property `FixedParams`. The order of values must match the order of the fitting parameters. Variable parameters can be left empty or replaced by a non-numerical string. 

Example: `FixedParams='var;2'` (or shorter: `';2'`) means that the first fitting parameter is variable, but the second one is held constant a the value 2; if there are more than 2 parameters, the others are considered to be variable, too. 

The fitting range is usually defined by the series extent, but can be manually set via the `FitRange` property. The fitting function is drawn over the entire axis unless `DrawFitRangeOnly = true`. 

When fitting is complete the `ErrCode` of the series is set to indicate whether the calculation was successful or not: 

  * `fitOK`: successful
  * `fitDimError`: calculation was aborted because the dimensions of the data arrays (x, y, y error bars) do not match.
  * `fitMoreParamsThanValues`: calculation failed because there are more fitting parameters than data values.
  * `fitNoFitParams`:calculation aborted because no fit parameters are specified. This could happen incidentally if all fitting parameter are forced to be fixed in the `FixedParams` property.
  * `fitSingular`: the design matrix of the fit problem is found to be (nearly) singular.
  * `fitNoBaseFunction`: Not enough fit basis functions are specified for the case of `FitEquation=feCustom`.



The method `ErrorMsg` creates a text for an error message from the current `ErrCode`. 

The event `OnFitComplete` is called after the fitting equation is found, but before the drawing starts. This is a good place to query the results of the fitting procedure: 

  * `Param[AIndex]` is the best-fit value for the parameter with the specified index, `AIndex = 0..ParamCount-1`.
  * `ParamError[AIndex]` tells the uncertainty ("standard error" in statistical terms) of the corresponding fitting parameter.
  * The procedure `GetConfidenceLimits(AIndex: Integer; out ALower, AUpper: Double)` returns the range between `ALower` and `AUpper` within which the corresponding best-fit parameter is expected to be found at the probability specified by `ConfidenceLevel` (default: `0.95`, i.e. 95%).
  * `Param_tValue[AIndex]` returns the value of the [t statistic](<http://www.wikipedia.org/wiki/T-statistic> "wikipedia:T-statistic") for the corresponding fit parameter. This is the ratio of value divided by standard error. For an accurate parameter, the t value should be much larger than 1.
  * `Param_pValue[AIndex]` is the probability that any t value as high as `Param_tValue` occurs just by chance for the given degrees of freedom. In order to "trust" the fitted value with a certainty given by `ConfidenceLevel`, the p value should be smaller than `1 - ConfidenceLevel`, by default 0.05.
  * `FitStatistics` informs about other statistical parameters helpful to verify the validity of the fitted model or for further statistical calculations. These data are accessible as properties or functions of this class: 
    * `N` (integer): the number of data points used for fitting. Note that this can be less than the data points provided by the chart source since it may contain NaN values which are skipped.
    * `M` (integer): the number of fitting parameters. Normally this is the value of `ParamCount`, but if certain parameters are forced to be constant (`FixedParams`) their count is subtracted.
    * `DOF` (integer): "degrees of freedom" - is the difference `N - M`, an important parameter in the statistical analysis of the fit results.
    * `SST`: "total sum of squares" - this is the sum of the squares of the term `(y[i] - ymean)/dy[i]` over all data points. Here, `y[i]` is the y value of a data point at index `i`, `dy[i]` the size of its error bar (standard deviation) - or 1 if not available -, and `ymean` is the average value of `y[i]/dy[i]`. The quantity identifies the total variation of data points around their mean value.
    * `SSE`: "error sum of squares" - the variation of the data points around the fitted curve, i.e. sum of `((y[i] - ycalc(x[i])/dy[i])^2)`; here `ycalc(x[i])` is the calculated value of the fitted curve at the point `x[i]`.
    * `SSR`: "regression sum of squares" - the variation of the fitted curve alone, i.e. sum of `((ycalc(x[i]) - ymean)/dy[i])^2`.
    * `xBar`: average value of the x coordinates of all used data points.
    * `SSx`: variation of the x coordinates of the data points, i.e. sum of `(x[i] - xbar)^2`.
    * `VarCovar[i, j: Integer]: Double`: is an element of the variance-covariance matrix for the fit parameters at the given indexes.
    * `R2`: "coefficient of determination". This is the ratio `SSR/SSE` and describes how much of the variation in the data is explained by the fitted curve. The value is 1 for a perfect fit and 0 for absolutely no agreement.
    * `AdjR2`: was introduced to compensate for the effect that `R2` usually increases by adding more and more fit parameters.
    * `Chi2`: another word for `SSE`. In essence, this is the statistical quantity which is minimized during the fitting procedure. The value given is the minimum value found.
    * `ReducedChi2`: is `Chi2` (or: `SSE`) normalized to the degrees of freedom, i.e. variation of the data around the fitted curve per degree of freedom. Should be "small" for a good fit.
    * `ResidualStdError`: is the overall standard error of the fit. Should be "small".
    * `pValue`: (fractional) probability that the residual scatter of the data points around the fitted curve is by chance. Should be high, in practice a few thenths, although sometimes values as low as 0.001 are still accepted. If no error bars are included in the fit, sometimes p turns out to be either exactly 0 or 1 - such values should be ignored.
  * The procedures `GetLowerConfidenceInterval(const Ax: Double; out AY: Double)`, `GetUpperConfidenceInterval`, `GetLowerPredictionInterval` and `GetUpperPredictionInterval` can be hooked into the `OnCalculate` event of a `TFuncSeries` in order to plot the limits of the lower and upper confidence and prediction intervals around the fitted curve. These limits define areas around the fitted curve within which the overall fitted curve (confidence interval) or individual data points (prediction interval) can be expected based on the probability given by `ConfidenceLevel`.
  * Note: all these statistical data assume that the residual errors follow a normal distribution and that the error bars have been estimated appropriately.



The `EquationText` method returns an `IEquationText` interface, which allows to construct a string representing the fit equation. You can override names of _x_ and _y_ variables, numeric format, and, finally, obtain a string from `Get` function. Note that a transformation from IEquationText to String is defined, so usually there is no need to call `Get` explicitly. 

`Legend.Format` property of fit series supports additional format argument ('%2:s') pointing to the default equation text. 

After a change to `TFitSeries` parameters, the equation will be recalculated on the next drawing of the series. To recalculate it immediately, call `ExecFit` procedure. Use `State` property to check the validity of current equation. 

#### Color map series

`TColorMapSeries` represents a 2-dimensional function defined by the `OnCalculate` event as a field of pixels, with color depending on function value. 

The series is drawn as a set of rectangles of size `StepX` by `StepY` pixels. The function is called once for each rectangle. 

Color values are defined by one of the built-in palettes (property `BuiltinPalette = cmpHot, cmpCold, cmpRainbow, cmpMonochrome`). The range of values mapped to the colors of the palette must be specified in `BuiltinPaletteMax` and `BuiltinPaletteMin`, otherwise the results are not predictable. 

Alternatively, an external ChartSource can be assigned to property `ColorSource`, which must be sorted. Each data point of the chart source is interpreted as having its X value correspond to the Color value. If the actual value falls between color levels, it can be either linearly interpolated (if `Interpolate` is `true`) or rounded down to the nearest level. 

When `Legend.Multiplicity=lmPoint`, the color map series will display the color levels in the legend. 

#### Expression color map series

is similar to the color map series with the main difference that the 2-dimensional function is not defined by an event, but by a mathematical expression, such as `x^2+y^2`, like in the TExpressionSeries, but now with two variables `x` and `y`. The variables can be renamed by means of the properties `VariableNameX` and `VariableNameY`. Similar to `TExpressionSeries` it is also possible to introduce parameters (property `Params`). Domains, however, are not supported, the function must be defined within the entire visible x/y range. 

## Sources

[![chartsource hierarchy.png](https://wiki.freepascal.org/images/6/61/chartsource_hierarchy.png)](</File:chartsource_hierarchy.png>)

Data can get into a chart from various sources. 

They are implemented as a set of components derived from `TCustomChartSource`. 

To assign a source to a series, you can set the `Source` property. If the property is left unassigned, the series will use its own built-in list source. Methods like `AddXY` are delegated to the current series source. Note that the list source is the only editable source, so after you assign, for example, a random chart source to the series, a call to `AddXY` will raise an exception. 

Each data item has the following fields: `X`, `Y`, `XList`, `YList`, `Color`, `Text`. 

The element `Text` can provide a text for each y value. In this case, the individual texts must be separated by the `LabelSeparator` defined by the source (by default, `'|'`). The number of texts must match the number of y values. 

### Sorted sources

If it is known that X values of the source are ascending, some additional optimizations like binary search become possible. So all sources have an `IsSorted` property which helps determine that. 

In Lazarus 2.1+, the `TListChartSource` can be sorted according to several criteria: 

  * `SortBy` = `sbX`, `sbY`, `sbText`, `sbColor`, `sbCustom`: sorting by X or Y value, Text, Color or in a user-defined way, respectively. In the latter case a handler for the event `OnCompare` must be provided.
  * `SortIndex`: the index of the X or Y value used for sorting.
  * `SortDir` = `sdAscending`, `sdDescending`: sorting in ascending or descending order.



When a source is sorted by anything other than x only the drawing order of the related series is affected, and it may be possible that nothing changes in the chart. A visual effect is seen for 

  * `TPieSeries` because the data point order determines to order of the pies around the circle
  * `TBubbleSeries` when bubbles overlap. Use the combination `SortBy=sbY`, `SortIndex=1` and `SortDir=sdDescending` to avoid covered bubbles because large bubbles are drawn first and small bubbles last.
  * all series inherited from `TBasicPointSeries` when the `XCount` of the associated chart source is 0: in this case, the X value of the source is ignored and replaced by the data point index. Since the data point index changes upon sorting the data points will be automatically rearranged in the sorted order - see example _sort_demo_ in the _demo_ folder.



### Multi-valued sources

Sources can contain multiple Y values for each X value. These values are stored in the `YList` field of the source data item. The number of Y values is determined by the `YCount` property. Note that the first Y value is stored in Y field anyway, so `YCount=3` means that values are stored in `Y`, `YList[0]` and `YList[1]`. 

Additional values may be used by various series -- for example, stacked bars or bubble charts. 

Moreover, it is also possible to store several X values for the same data point. This is required by `TFieldSeries`. These additional X values are stored in the `XList` of the source data item. Like with the Y values, the total count of X values is given by the `XCount` property which contains the "ordinary" X value plus the length of the `XList` array. 

### Skipping source items

It is possible to instruct TAChart to skip drawing some source items without removing them from the source. This may be useful for both optimization and user interface reasons. 

To skip the item, assign [NaN](<http://www.freepascal.org/docs-html/rtl/math/nan.html>) to either X value or one of the Y values. The exact effect of skipping depends on the series, but usually setting X to NaN means skipping the entire item, while setting Y or some element of YList means skipping only this Y value, while still drawing others. 

Also note that if you set only one coordinate to NaN, the other will still take part in extent calculation. 

In case of stacked series it is not clear how the missing value should be handled when the following levels are added to it. In Lazarus v2.1+ there is a property `StackedNaN` which can be selected as 

  * `snReplaceByZero` \-- the missing value is considered to be zero; this is the way how Excel handles this case.
  * `snDoNotDraw` \-- skips the entire data point with all stack levels.



You can see examples of skipping items in the "nan" demo. 

### Error bars

`TLineSeries`, `TBSplineSeries`, `TCubicSplineSeries`, `TFitSeries` and `TAreaSeries` can display error bars at each data point. The size of the error bars in both `x` and `y` direction is determined by the parameters in the `XErrorBarData` and `YErrorBarData` properties of the chart source, respectively: 

  * `Kind: TChartErrorBarKind`: defines how the following data are interpreted 
    * `ebkNone`: ignore - no error bars will be diplayed
    * `ebkConst`: all data points have the same error bar given by `ValuePlus` and `ValueMinus`.
    * `ebkPercent`: the values `ValuePlus` and `ValueMinus` are understood as percentage of the data values.
    * `ebkChartSource`: every data point can have an individual error bar. The error bar length is stored in the XList or YList of a multi-valued chart source. The index among all x and y values is specified by `IndexPlus` and `IndexMinus` for positive and negative error bar, respectively.
  * `ValuePlus` and `ValueMinus`: length of the positive and negative error bars, respectively. Depending on the setting of `Kind`, the value is assumed to be constant for all data points (`Kind = ebkConst`, axis units), or a percentage of the data point value (`Kind = ebkPercent`). Both values must be positive numbers with the exception of `ValueMinus = -1` which indicates that the negative error bar should be equal to the positive error bar.
  * `IndexPlus` and `IndexMinus` are used when `Kind` is `ebkChartSource` and indicate the x or y index in a multi-valued chartsource from which the error bar lengths for the individual data points can be obtained. Suppose a chart source contains two x values (`XCount = 2`) and `XErrorBarData.ValuePlus` is `1` then the second x value (stored in `XList[0]`) is used for the positive error bar length. If `IndexMinus` is -1 then the negative error bar length is assumed to be equal to that of the positive error bar without having to provide an explicit value in the chart source.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Usage of multiple y values for other purposes than individual error bars is discouraged in this context because the present implementation can display error bars only for the y value with index 0.

Each series type supporting error bars has additional properties `XErrorBars` and `YErrorBars` which determine how the error bars will be displayed: 

  * `Visible` can be used to turn off error bars even if their size is specified in the chart source.
  * `Width` is the length of the crossbar drawn at the end of the error bar. The default value -1 means that its length will be equal to the corresponding size of the series' `Pointer`.
  * `Pen` defines the pen parameter used for drawing the error bars. Usually set `Pen.Color` to the color of the series.



### List source

`TListChartSource` is a basic chart source, storing chart data inside itself. As such, you can use `Add` and `Delete` functions to change source data. 

The `Item` property returns a pointer directly to the underlying storage, so you can modify item fields directly. However, doing this will not automatically update the chart and will also invalidate some internal state of list source. It is recommended: 

  * To change a single item, use `Set{Color|Text|XValue|YValue|YList}` procedures.
  * To change many items in a time-sensitive code, call `BeginUpdate`, then modify items directly, then call `EndUpdate`.



The source also has `DataPoints` property to allow setting data at design time. This property is a TStringList, with each line representing a data point. Line consists of X, Y, optional YList, Color and Text values separated by | (vertical bar) character. Note that `DataPoints` property is designed primarily for sample and demo code. It is very inefficient, and you should not use it to add data points from the code. 

You can control X value sorting by setting the `Sorted` property. Note when `Sorted` is set to true, list source sorts the data and keeps it sorted after insertion of new points. If inserted points are not sorted, this may result in quadratic running time. You should either set `Sorted` to true only after insertion, or pre-sort your data to avoid this. 

### Random source

`TRandomChartSource` source generates random data in the given range and is intended mostly to use in demos. You can also use it as design-time replacement for your actual data source. This will let you see and change the look of your chart without having to run the application. 

Each random source uses its own independent random number generator to guarantee the stability of its values. 

### User-defined source

This source may be used if you already have your data in memory, but in a format different from the data items used in TAChart. With user-defined source you can access your data directly instead of first moving it all into a list source. In some cases this may improve performance or reduce memory consumption. You can of course also generate, filter or modify data with the user-defined source. 

The number of data items in the source is determined by `PointsNumber` property. Items themselves must be returned by the `OnGetChartDataItem` event handler. You should call `Reset` method to notify the chart about changes in the data. (Other sources detect changes and perform notification automatically). 

Note that if the `Sorted` property is set to true, it is the responsibility of the event handler to provide actually sorted data. 

### Database source

`TDbChartSource` takes data directly from a database. It is contained in a separate unit to avoid introducing a db-aware component dependency into every project using TAChart. 

The following properties contain database field names for data item fields: 

Property | Access method   
---|---  
`FieldX` | AsFloat (or AsDateTime, if option dcsoDateTimeX is set)   
`FieldY` | AsFloat (or AsDateTime, if option dcsoDateTimeY is set)   
`FieldColor` | AsInteger   
`FieldText` | AsString   
  
If `FieldX` property is empty, `RecNo` is used instead. `NULL` values in coordinate fields are translated into NaNs. 

To get a multi-valued source, set the `FieldY` property to a comma-separated list of field names. Note that `YCount` will be set automatically -- trying to set it by hand will raise an exception. 

When the dataset returns date/time fields as strings you should set the property `DateTimeFormat` accordingly (e.g. `'yyyy-mm-dd'`) so that the strings can be converted to a TDateTime variables. 

Please note that iterating through the items of a `TDBChartSource` is very ineffective. Do not attach other DB-aware controls to the same dataset because they will mirror the process. Usually it is recommended to copy the `DBChartSource` to a `ListChartSource` for better performance. Note also that some dataset types do not read all records of a table or query. 

### Calculated source

`TCalculatedChartSource` is the source used for manipulating data taken from the `Origin` source. This source performs transformations in the following order: 

  * Y reordering -- Y values of multi-valued source can be duplicated, removed or exchanged according to `ReorderYList` property, which is a comma-separated list of original Y value indexes. Step skipped if `ReorderYList` is empty.
  * Accumulation -- replaces each item's Y values by a function of the neighboring values.
  * Percentage -- replace each Y value by the percentage of total of all Y values for that item. Useful for drawing "stacked percentage" bar and area charts. Step skipped if the `Percentage` property is false.



Accumulation is controlled by several properties: 

`AccumulationRange` controls number of items to accumulate, counting the current item, so `AccumulationRange = 1` disables accumulation. `AccumulationRange = 0` is interpreted as "infinite" range, i.e. request to accumulate over all the available data. This is mostly useful in conjunction with `camSum` method to produce cumulative sums. 

Note that the actual number of items may be lower for the points near the beginning or end of the source. 

`AccumulationDirection`: 

  * `cadBackward` \-- use previous values from the source,
  * `cadForward` \-- use next values from the source,
  * `cadCenter` \-- use both previous and next values for a total number of up to `2 * AccumulationRange - 1`.



`AccumulationMethod`: 

  * `camNone` \-- skip accumulation step,
  * `camSum` \-- sum of the last `AccumulationRange` items,
  * `camAverage` \-- average the last `AccumulationRange` items,
  * `camDerivative` \-- finite differences derivative, calculated using the last `AccumulationRange` items. Note that the calculation method assumes equidistant X values, and may loose accuracy if the X distance varies substantially.
  * `camSmoothDerivative` \-- smoothed finite differences derivative, more robust against random measurement errors in the data.



### Interval source

`TIntervalChartSource` can supply arbitrarily many points in a given interval, controlled by various properties of the source. This source is the default built-in source for axis marks. If you want to set the same axis interval parameters for several axes, you can assign a single `TIntervalChartSource` component the `Marks.Source` of each of those axes. 

### Date-time interval source

`TDateTimeIntervalChartSource` is similar to the `TIntervalChartSource`, but provides marks formatted as date/time values. This source automatically selects appropriate calendar interval (such as week or hour) depending on the axis scale. 

Note that X values of provided data items are `TDateTime` values, and `Label` values contain formatted date-time strings. If you want to use `TDateTimeIntervalChartSource` as the source of axis marks, you should probably set `Marks.Format` or `Marks.Style` properties to make use of provided labels (e.g. by setting `Marks.Style` to `smsLabel`). 

If the `DateTimeFormat` property is set, it is used to format all labels. Formatting is performed with standard [SysUtils.FormatDateTime](<http://www.freepascal.org/docs-html/rtl/sysutils/formatdatetime.html>) function. If `DateTimeFormat` is empty, format is chosen automatically based on scale. 

TDateTimeIntervalChartSource is defined in the [TAIntervalSources](</index.php?title=TAIntervalSources&action=edit&redlink=1> "TAIntervalSources \(page does not exist\)") unit. 

### Source optimization notes

Primary data source API allows random access. Nevertheless, many sources, in particular random, database and calculated, may exhibit quadratic or worse behavior if actually accessed randomly. TAChart itself takes care to only use sequential access (although it may require several passes). Sources optimize sequential access by using internal state. User code should be careful not to reset this state during chart drawing from event handlers or custom series code. 

A notable exception is the list source, which is guaranteed to provide fast random access. It may be used to cache slow sources with the help of `CopyFrom` procedure. 

Also note that the pointer returned by `GetItem` function may point to the internal buffer which will be overwritten by the next call to `GetItem`. Again the list source does not have this limitation. 

## Coordinates and axes

TAChart uses four coordinate systems: 

  * _Axis coordinates_ (known in some other applications as object coordinates) -- this is the "raw" coordinate values obtained from the data. As the name implies, axis coordinates are interpreted in terms of specific axis -- the same coordinate value may have different meaning depending on the axis it is applied to.
  * _Graph coordinates_ (aka world coordinates) are converted from the axis coordinates using axis transformation, such as logarithmic scale. Graph coordinates are common for all objects in the chart.
  * _Image coordinates_ (aka screen coordinates) are converted from graph coordinates based on the chart viewport. This transformation is always linear and can be influenced by chart tools such as zooming and panning.
  * _Device coordinates_ are usually equal to screen coordinates, but may be adjusted to the drawing back-end to accommodate different physical resolutions (DPI values). See, for example, printer drawer.



You can add or remove an arbitrary number of axes by editing `AxisList` property. By default, chart have two axes: one horizontal and one vertical. They are accessible via `BottomAxis` and `LeftAxis` properties. Note that those properties are aliases to `AxisList[0]` and `AxisList[1]`, so if you remove those default axes, accessing `BottomAxis` and `LeftAxis` will return nil. 

Visually, axis consists of the axis line (drawn by `AxisPen`), grid lines (drawn by `GridPen`), ticks, marks and arrow. 

Each axis is drawn inside its own rectangle, determined by the size of mark labels and ticks. By assigning several axes the same positive `Group` number, you can have them share the same rectangular area. Grouped axes can be used to achieve a "panes" look, when several series are drawn on different portions of the same graph. 

Axes with the same alignment, but different groups, are stacked alongside each other. You can use the `Margin` property to control spacing between such axes. 

### Axis transformations

Axis transformations are grouped in the `TChartAxisTransformations` component. It contains a list of transformations which are applied in the order given. (For example, performing scale before and after logarithm will yield different results). 

For transformations to have an effect, you should: 

  * Make sure that the `Enabled` property is true for all transformations. (Default)
  * Assign transformations component to `Transformations` property of at least one axis.
  * Assign `AxisIndexX` and/or `AxisIndexY` properties of the series to the appropriate axis index.



Note that by default, `AxisIndexX` and `AxisIndexY` have a special value of -1, which means "ignore axis transformations". Also note that if you add or remove axes, the indexes may change. You can rotate the series by assigning _both_ `AxisIndexX` to vertical axis and `AxisIndexY` to the horizontal axis. 

#### Linear and logarithmic transformation

Those are simple arithmetic transformations. 

#### Auto-scaling transformation

To display several independently-scaled series, assign them to two or more axes and apply `TAutoScaleAxisTransform` to each axis. See "axistransf" demo, page "Linear", checkbox "Auto scale". 

By using the `MinValue` and `MaxValue` properties you can control the in graph coordinates of the auto-scaled series. For example, by setting one transformation to a range from 0 to 1, and another to a range from 1 to 2, you will confine all the series using the first transformation to the upper half of the chart, and all the series using the second transformation to the lower half (assuming there are no unassigned series left). 

#### Cumulative normal distribution transformation

Use `TCumulNormDistrAxisTransform` to set [cumulative normal distribution](<http://www.wikipedia.org/wiki/Cumulative_distribution_function> "wikipedia:Cumulative distribution function") as an axis transformation. This may be useful in statistical charting. 

Note that this transformation result is in range from 0 to 1. It is recommended to restrict the axis range accordingly. 

See the "Normal distribution" page of the "axistransf" demo for an example. 

#### User-defined transformation

You can create you own transformation either by inheriting from `TAxisTransform`, or, if you prefer "visual" programming, by using `TUserDefinedAxisTransform`. In either case there are two basic requirements: 

  * `AxisToGraph` and `GraphToAxis` functions should be defined everywhere in data range and inverse of each other (for example, avoid now only dividing, but also multiplying by zero).
  * Functions should be monotonic.



### Date and time axes

Using date/time values for axis marks is a common requirement. The correct way to do this depends on the exact nature of your date/time data: 

  * If the data is actual `TDateTime` values, use it as an X coordinate in points, assign `TDateTimeIntervalChartSource` to the `Marks.Source` of the corresponding axis, and change `Marks.Style` to `smsLabel`. `TDateTimeIntervalChartSource` provides automatic labeling depending on the scale in wide range -- from centuries to milliseconds.
  * If the data is in physical units, but outside `TDateTime` values range, such as astronomical or micro-electronics timings, use it as a normal X coordinate with custom `Marks.Format`.
  * If the data is in calendar units, such as months and years, which is common for financial data, you have several options: 
    * If date units are "equidistant" when interpreted as numbers (for example, simple year numbers), assign the same data source to both series and axis marks, then use custom `Marks.Format`, `Axis.OnMarkToText` event or `Text` field of the data items to format dates per your requirements.
    * If date units are not "equidistant" (for example, numbers in YYYYMM format or even date strings), use surrogate X coordinate (usually, simply a point index) instead and display dates using methods described above.
    * Convert coordinates to `TDateTime` values beforehand, then use `tt` as described above.



### Axis ranges

Axis range is measured in axis units and determines the extent of series attached to this axis. Normally an axis range is equal to the union of all series extents, but may be overridden with the `Range` property. 

Axis marks are displayed inside the axis marks range, which is determined as the intersection of: 

  * Logical extent (converted to axis coordinates)
  * Combined extent of all data series of this axis, if `Marks.AtDataOnly = true`
  * `Marks.Range` property.



### Axis intervals

Axis marks are located along the axis at equal intervals chosen by the chart. The choice of intervals can be influenced via `Intervals` property. This property has a few subproperties, which are applied in the following order: 

  * The `Options` property contains a set of flags controlling usage of other parameters. If a flag is not in the set, the corresponding parameter is ignored. Additionally, the following options play a special role: 
    * `aipGraphCoords`: tries to find the labels based on the graph coordinates rather than on the axis coordinates. This is required for logarithmic axes.
    * `aipInteger`: accepts only integer labels. This feature is useful for barcharts to suppress labels between the bars, or for logarithmic plots to get labels at powers of 10 (together with `aipGraphCoords`). But note that the axis can be left without any labels in unfavourable cases.
  * `NiceSteps` is a string containing a sequence of "step multipliers" -- floating point values in the range from 0 to 1, excluding 0. If this property is applied, the axis step will be a power of ten multiplied by one of the provided values. If several multipliers can be used, the leftmost one will be chosen. Multipliers are separated by the vertical bar (|) character.
  * If `NiceSteps` is ignored or TAChart fails to find appropriate step, the axis range will be divided into equal intervals without regard to the number of decimal digits in the representation of mark values. In this case, it is recommended to reduce the number of visible digits in `Marks.Format`.
  * `MinLength` and `MaxLength` properties set the limits of interval length _in image units_ (usually pixels).
  * The `Count` property is the desired number of axis marks. Among all mark steps passing the previous tests, TChart chooses the one which gives the number of marks nearest to the `Count`. If `Count` property is ignored, or there are several steps with equal number of marks, the longest step is chosen.
  * The `Tolerance` property sets the maximum distance _in image units_ by which the mark may be moved in order to reduce the length of its decimal representation. This helps to avoid a lot of meaningless digits in mark labels when previous steps have for some reason failed to generate "nice" marks. Note that this stage ignores all previous restrictions, so large `Tolerance` values may result in significant distortions. Setting `Tolerance = 1` is often sufficient.



Note that if you set the chart source manually, the `Intervals` property may apply only partially or not apply at all. For example,  list source is unable to guarantee maximum interval length, since it has only a finite number of points. See also interval chart source. 

### Axis position

By default, the axis is aligned to the side of the chart corresponding to the `Alignment` property. You can position an axis differently by setting the `Position` and `PositionUnits` properties. 

The following values of the `PositionUnits` property are accepted: 

  * `cuPercent` \-- percentage of the clipping rectangle.
  * `cuGraph` \-- absolute position in graph coordinates.
  * `cuAxis` \-- absolute position in axis coordinates. In order to apply the correct axis transformation the index of the orthogonal axis (`OrthogonalAxisIndex`) must be specified (new feature in Laz 4.0).
  * `cuPixels` \-- position in screen units relative to the default coordinate.



**Note 1:** If the axis position is changed from default (`Position = 0` and `PositionUnits = cuPercent`), it will be excluded from the margins calculation, so it will appear to overlap the series. 

**Note 2:** The `Alignment` affects not only the position of the axis itself, but also of the corresponding labels. So, if two axes have both `Position = 50` and `PositionUnits = cuPercent`, but one has `Alignment = calLeft` and another `Alignment = calRight`, they will display a common axis line with labels on different sides. 

See "Position" page of the "axis" demo for an example. 

### Axis titles

If the property `TextFormat` is switched to `tfHTML` then HTML entities and tags can be embedded for special formatting of axis titles, such as subscripts/superscripts or greek characters. See HTML for details. 

The property `Wordwrap`, if set to `true`, allows to wrap extra long axis titles into new lines. Note that this feature works only when the title is horizontal for the x axis and vertical for the y axis. Moreover, html-formatted text is not supported for word-wrapping. 

### Axis LabelSize

Since Lazarus v1.4 the chart axes have a new property `LabelSize` which - if different from `0` \- overrides the automatic calculation of the space needed by the axis marks. The `LabelSize`, approximately, defines the pixels between axis line and axis title, independently of the size of the axis labels. This feature can be used to align the axis lines of several charts that are stacked above each other or placed side by side. 

See the "axisalign" demo and [Graphical explanation](<TAChart_documentation.md> "TAChart documentation") for an example. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If the (non-zero) `LabelSize` value is too small then the axis labels may reach into the axis title or may even be truncated at the chart border.

## Extents and margins

### Extents

Chart _extent_ is a rectangle in graph coordinates. 

There are several extents defined by TChart: 

  * _Full extent_ \-- usually determined automatically as the area encompassing all the data from series and axis ranges. Is returned by the `GetFullExtent` function.
  * _Fixed extent_ \-- determined by the `TChart.Extent` property. May override full extent calculation partially or fully.
  * `LogicalExtent` \-- the extent requested by the user to be seen on the chart image. Writing to this property is the official way to change the chart extent by external code. For example, `LogicalExtent := GetFullExtent` is (almost) equivalent to calling the `ZoomFull` procedure.
  * `CurrentExtent` \-- the extent actually displayed to the user. May differ from the `LogicalExtent` due to the need to reserve space for series marks, inner chart margins etc.



The value of the minimum extent cannot be greater than the value of the maximum extent. If an attempt is made to set such values an exception will be generated. To prevent exceptions, the procedure `.Extent.FixTo(aBounds: TDoubleRect)` can be used to set logical extents. The values in `TDoubleRect` are ordered in the following way: _[xmin, ymin, xmax, ymax]_. 

In order to invert the extents, the property `Inverted`' of the (left, bottom, etc) axis shall be set to `True`. 

**Important:** Setting logical extents will work as required only if all the `Margins` of the chart are set to 0 (default value is 4) and `Series.Marks.AutoMargins` to all series in the chart is set to `False` (default is `True`). Otherwise, the requested extent will be enlarged to include also the chart's `Margins` as well as the series' `Marks`. 

### Extent limits

By default, TAChart allows arbitrary extents. However, for both usability and speed reasons it may be desirable to limit extent size. For example, setting the lower bound of the size may disallow extreme zoom levels, while setting the upper bound may force the user to only see a part of the very long series at any given time. 

The extent can be limited with the `ExtentSizeLimit` property. Sub-properties `MinX`, `MinY`, `UseMinX` and `UseMinY` control the lower bounds of the extent, while sub-properties `MaxX`, `MaxY`, `UseMaxX` and `UseMaxY` control the upper bounds. 

Extent limits are expressed in graph units. 

Setting `Proportional = true` will enforce the current extent to have the shape of the plot area. This may be useful for cartographic or some math plots which require the same scale on both axes. 

### Linked extents

Using the `TChartExtentLink` component, you can ensure that the logical extents of several charts enumerated by the `LinkedCharts` property always stay the same. In Laz 2.1+ the component is extended to provide an additional property `AlignSides` which adjusts the `LabelSize` properties of aligned charts such that the corresponding sides of the plot area are aligned, too. (When property `AlignMissingAxes` is `true` (default) dummy axes are created for those sides having no axes so that alignment works for these sides, too). 

This is useful for simulating multi-pane chart layouts. See "panes" demo for an example. 

### Margins

_Margin_ is a distance reserved around the edges of rectangular region. Margins are measured in image units (usually pixels). The chart itself has two kinds of margins: 

  * Internal (`Margins` property) -- applied after axis drawing. Are also influenced by series marks and series themselves.
  * External (`MarginsExternal` property) -- applied before axis drawing. Are also influenced by axis marks and arrows.



Other chart elements, such as legend, title, footer and labels, have margins, too. 

### Optimization notes

Calculation of `CurrentExtent` and actual margins is a non-trivial _iterative_ process (see `TChart.PrepareAxis` code for details). Although usually fast, in complex cases it can require multiple passes through chart sources. 

### Graphical explanation

1 = Chart.MarginsExternal.Left  
2 = Chart.LeftAxis.Margin  
3 = Chart.LeftAxis.Title.Margins.Top   
4 = Chart.LeftAxis.Title.Distance  
5 = Chart.LeftAxis.LabelSize  
6 = Chart.Margins.Left  
7 = Autosized  | [![chart-margins.png](https://wiki.freepascal.org/images/2/20/chart-margins.png)](</File:chart-margins.png>)  
---|---  
  
## Tools

Chart tools define how the chart reacts to various user actions, primarily mouse movements and clicks. You can see examples of tools usage in the "tools" or "barseriestools" demos. 

Tools are grouped in a `TChartToolset` component, which should be assigned to the chart's `Toolset` property. The same toolset can be used in several charts. 

If `Toolset` is unassigned, for compatibility reasons a built-in toolset consisting of zoom drag  and pan drag tools is used; these builtin tools can be turned off individually by setting the chart properties `AllowZoom` and/or `AllowPanning` to `false`, respectively (the latter is available only after Lazarus v2.2+). 

In each user action, tools in the toolset are processed in order, and for each tool: 

  * If `Enabled=false`, the tool is ignored.
  * If `Shift` is not equal to the current shift state, the tool is ignored.
  * Tool is requested to process the action.
  * If the tool signals that the action is handled, processing is stopped, otherwise it continues to the next tool. This way, tools with the same `Shift` value may be differentiated based on special activation conditions. For example, some drag tools may be configured to not activate on a simple click, leaving the click event available for other actions.
  * Finally, if no suitable tool is found, the chart's event handler is called. Note that this means that chart event handlers will only work with all tools disabled. Generally it is recommended to use tools for interactivity, chart events are left mostly for compatibility.



In your application you can create, for example, a toolbar with each button enabling the corresponding tool in the toolset and disabling all others. Alternatively, by assigning different `Shift` values, you can enable several tools at once. 

Some tools publish the `EscapeCancels` property which, if set to `true`, cancels the tool operation if the user pressed the ESC key. 

### Keyboard handling in tools

Besides mouse events, some tools may react on key presses -- for example, crosshair tool with `Shift = [ssCtrl]` will display crosshairs when the CTRL key is pressed, without mouse buttons. Unfortunately, the chart must be focused to receive keyboard events. This means that after the user interacted with other controls on the same form, the chart stops reacting on keyboard-only events. 

To prevent this, you can either call the `Chart.SetFocus` method, or set `Chart.AutoFocus = true`, which will make a chart grab the focus when the mouse moves over it. 

### Drawing mode

Some tools, such as zoom drag or crosshair, display moving shapes over the chart with the mouse movement. 

There are two ways to display those shapes: either simply draw them over the chart, fully redrawing the chart upon each mouse movement, or use `pmXor` pen mode to draw and erase the shape directly from the `MouseMove` event handler. The former method allows to use arbitrary pen color and style, but the latter is much more efficient. Additionally, some widgetsets ignore all drawing outside the `Paint` event, in such a case the latter method will not work at all. 

The display method is controlled by the `DrawingMode` property with the following values: 

  * `tdmXor` \-- use XOR method;
  * `tdmNormal` \-- use full chart redraw;
  * `tdmDefault` \-- use XOR method on widgetsets where it is known to work (Windows and Gtk) and full redraw on others.



### Extent tools

Extent tools modify the chart's logical extent. 

Zooming tools can be animated by setting `AnimationSteps` to a value greater then `1` and specifying an appropriate `AnimationInterval` in milliseconds. 

In the zooming tools, adapt the `LimitToExtent` property to restrict zooming to the chart's full extent on all or some directions. The same property is available also in the panning tools for the corresponding purpose when the viewport is panned. 

The event `OnCalculateNewExtent` allows to fine-tune the extent after zooming or panning before applying to the chart. 

  


#### Zoom drag tool

`TZoomDragTool` allows the user to zoom in by drawing a rectangle with the mouse. The rectangle then becomes the new logical extent. 

Restoration of the zooming to the full extent can be achieved by several actions, controlled by the `RestoreExtentOn` property: 

  * `zreDragTopLeft`, `zreDragTopRight`, `zreDragBottomLeft`, `zreDragBottomRight` \-- dragging in the specified direction,
  * `zreClick` \-- clicking without dragging,
  * `zreDifferentDrag` \-- dragging in a direction different from the one used to zoom in.



By default, `RestoreExtentOn = [zreClick, zreDragTopLeft, zreDragTopRight, zreDragBottomLeft]` which means that the full extent is restored either by clicking or by drawing a rectangle in any direction except from top-left to bottom-right. 

To get a behavior compatible with earlier versions of Delphi's TeeChart, specify `RestoreExtentOn = [zreDragTopLeft, zreDragTopRight, zreDragBottomLeft]`. 

To get a behavior compatible with newer versions of TeeChart, specify `RestoreExtentOn = [zreDifferentDrag]`. 

The enumerated property `RatioLimit` (`zrlNone, zrlProportional, zrlFixedX, zrlFixedY`) lets you restrict zooming to one of the coordinates, or makes the zooming process keep the original proportions. In such a case of restricted zooming the zoom rectangle is drawn in the fixed direction across the entire chart when the property `AdjustSelection` is at its default value `true`; otherwise the zoom rectangle would be drawn between the mouse start and end points, like in the unrestricted case. 

#### Zoom click tool

`TZoomClickTool` allows the user to zoom in or out by clicking on the chart with the mouse. `ZoomFactor` is the scaling multiplier applied by the tool where factors below 1 represent "zoom out", and factors above represent "zoom in". 

`ZoomRatio` allows to create a non-proportional zooming effect by specifiying the ratio of X to Y scale. In other words, the X zoom factor is given by the property `ZoomFactor`, the Y zoom factor is the product `ZoomFactor*ZoomRatio`. 

  * In particular, to **zoom only horizontally** , the Y zoom factor must be 1, i.e. `ZoomFactor*ZoomRatio = 1` or `ZoomRatio = 1/ZoomFactor`. For example, when your `ZoomFactor` for the X axis is 1.1, then `ZoomRatio` must be 1/1.1 = 0.909.
  * Similarly, to **zoom only vertically** , the X factor must be 1, i.e. `ZoomFactor = 1`, and you can use `ZoomRatio` alone to determine the Y zoom factor.
  * And finally, in the normal case, when X and Y should zoom by the same ratio, keep `ZoomRatio` at 1 and determine the overall zoom factor by the `ZoomFactor` alone.



If `FixedPoint` is true, the location of the mouse click is used as a fixed point for zooming, otherwise the chart image center is used instead. 

#### Zoom mouse-wheel tool

`TZoomMouseWheelTool` allows the user to zoom in and out with the mouse wheel. Its properties are identical to the zoom click tool. 

The chart is scaled by `ZoomFactor` when the user scrolls the mouse wheel up, and by `1/ZoomFactor` when the user scrolls the mouse wheel down. 

#### Pan drag tool

`TPanDragTool` allows the user to move the logical extent by dragging the mouse in the directions indicated by the `Directions` property. 

Use the `MinDragRadius` property to distinguish dragging from clicking. 

#### Pan click tool

`TPanClickTool` allows the user to move the logical extent by clicking inside a range of `Margins` pixels from the corresponding border of the chart image. 

The panning offset is determined by the distance from the edge of the chart (the nearer to the edge, the greater). Setting `Interval` in milliseconds will allow to continue panning with the given interval until the mouse button is up. 

#### Pan mouse wheel tool

`TPanMouseWheelTool` allows the user to move the logical extent by scrolling the mouse wheel. 

The extent is moved in `WheelUpDirection` when the wheel is scrolled up and in the opposite direction when the wheel is scrolled down. 

The movement speed is controlled by `Step` property. 

### Data tools

Data tools are linked to specific data series via the `AffectedSeries` property, which is a string of comma-separated series indexes. Note that indexes may change if you add or remove series. Having `AffectedSeries` empty means that the tools operate on every series in the chart. 

When a data tool is activated, it determines the nearest point of the affected series which is located inside of the `GrabRadius` (in pixels). 

Using the set `Targets: TNearestPointTargets` it can be controlled which part of a complex series must be hit: 

  * `nptPoint`: The tool must hit the data point at (x, y).
  * `nptXList`: If the series' source has several x values the tool must hit any of the values in the XList.
  * `nptYList`: If the series' source has several y values the tool must hit any of the values in the YList.
  * `nptCustom`: Meant for special effects depending on the series. In case of a bar series, for example, the function `GetNearestPoint` is overridden to accept hits inside series bars if this option is set.



All options are on by default. For further control, most series have the same set of Targets, named `ToolTargets`. An option must be included in the targets of both tool and series to be active. This way tools can be shared with series reacting differently on each option. See "barseriestools" demo as an example. 

For large series, the efficiency can be improved by using a sorted source, a small `GrabRadius` and a `DistanceMode<>cdmOnlyY `. 

#### Data point drag tool

`TDataPointDragTool` allows the user to change data values by dragging the data point. Requires the series' data source to be a list source. 

You can use the `OnDrag` and `OnDragStart` events to limit drag direction or grab area. 

See "dragdrop" demo for an example. 

#### Data point click tool

`TDataPointClickTool` allows you to assign an `OnPointClick` event handler, which will be called when the user clicks on the data point. 

#### Data point hint tool

`TDataPointHintTool` displays a hint when the user moves the mouse over the data point. The hint is either equal to the data point label (if `UseDefaultHintText=true`) or determined by calling the `OnHint` event handler. In the former case, the `Style` property of the series' `Marks` must be set to something different from `smsNone`. (You can turn off directly displaying all the series marks in the chart by selecting the series' `Marks.Visible = false`). 

By default, this tool displays a separate hint window, independent from the application hint. To use a single hint window per application, you may set `UseApplicationHint=true`. 

Note that the application-level hint does not work in combination with modifier keys and mouse buttons, so the hint is displayed only if no buttons are pressed (i.e. `Shift` property must be empty when `UseApplicationHint=true`). 

#### Data point crosshair tool

`TDataPointCrosshairTool` displays a cross-hair centered on the data point. It replaces the "reticule" found in older versions of TAChart. 

#### Data point distance tool

`TDataPointDistanceTool` allows to measure and display a distance between two points on the chart. 

### Chart element tools

#### Axis click tool

This tool fires an event `OnClick` when the user clicks on an axis or its title. The event has the clicked axis and a set of `TAxisHitTest` elements as parameters: 

  * `ahtTitle`: click on the axis title
  * `ahtLine`: click on the axis line (allowed tolerance defined by the tool's `GrabRadius` or the axis' `TickLength` and `InnerTickLength` whichever is greater).
  * `ahtLabels`: click into the label area of the axis. It is not distinguished whether a label is clicked or the empty space between the labels.
  * `ahtAxisStart`: the click on the line or label area occured in the first quarter of the axis length
  * `ahtAxisCenter`: the click on the line or label area occured in the center part (two quarters) of the axis length
  * `ahtAxisEnd`: the click on the line or label area occured in the last quarter of the axis length.



If you do not want an axis to register clicks on some of these elements you can remove them from the `EnabledHitTests` property of the axis. 

#### Title/footer click tool

An `OnClick` event is created when the user clicks on the title or footer of a chart. The event has the clicked title/footer as parameter. 

#### Legend click tool

Generates an `OnClick` event when a click on the chart's legend has occured. The event has the legend as a parameter. 

Moreover an `OnSeriesClick` event fires when a legend item is clicked and the item is associated with a series; the series is passed to the event handler as a parameter. The series parameter can be nil when empty space or a non-series item (group header) is clicked. Requires Lazarus version 2.3 or newer. 

When both events are assigned, only `OnSeriesClick` is fired. 

#### Datapoint marks click tool

Fires the event `OnPointClick` when the user clicks on the marks of a series. It is useful for crowded charts in which a series with marks is covered by others and is to be moved to the top of the z order so that all marks can be read. Requires Lazarus version 2.3 or newer. 

### User defined tool

To add your own tool, either inherit from `TUserDefinedTool` or use it directly, assigning one or more On{After,Before}{KeyDown,KeyUp,MouseDown,MouseMove,MouseUp,MouseWheelDown,MouseWheelUp} event handlers. 

Call the `Handled` method of the tool to indicate that no further processing of the event should be done. 

## Decorative elements

### Title and footer

Chart title and footer are multi-line texts appearing above and below the chart correspondingly. They support various rotations and alignments, as well as the boolean `WordWrap` property to wrap text wider than the chart into multiple lines. 

Normally the title/footer background tightly encloses the text; it can be expanded to run across the entire width of the chart by setting the `FullWidth` property to `true`. 

#### HTML

If the Lazarus version is 1.8 or newer then html entities and some html tags can be embedded into the title/footer texts for a variety of additional formatting options. This feature is turned on by switching the property `TextFormat` to `tfHTML`. In addition to header/footer texts it is available also for axis captions, axis marks, series marks and legend texts/series titles. 

  * Non-standard charcters can be embedded using the corresponding **html entities** , i.e. as codes beginning with an ampersand (&), a standardized appreviation of the character name (or its hex or decimal unicode value), and ending with a semicolon (;). A list of all html entities can be found at <http://unicode.e-workers.de/entities.php>.   
_Example_ : The text `sin 2&alpha; = 2 sin &alpha; cos &alpha;` is displayed as "sin 2α = 2 sin α cos α"
  * `<sub>...</sub>` displays the enclosed text as **subscript**.   
_Example:_ `H<sub>2</sub>O` is shown in the chart as "H2O"
  * `<sup>...</sup>` displays the enclosed text as **superscript**.   
_Example:_ `cm<sup>2</sup>` is shown in the chart as "cm2"
  * `<b>...</b>` displays the enclosed text with **bold** type face.   
Likewise, the tags <i>...</i>, <u>...</u>, <s>...;</s> can be used to format portions of the text as _italic_ , _underlined_ , and ~~strike-out~~  
 _Example:_ `Plot of <b>x</b> <i>vs.</i> <b>y</b>` is shown in the chart as "Plot of **x** _vs._ **y** "
  * The `<font>...</font>` tags can be used to modify the font of the enclosed text: 
    * **Text color** : The embedded `This text is <font color="red">RED</font>` displays the enclosed text in red color, i.e. like "This text is RED". Specify the color by the most typical color names or by the html hex color codes of rgb values, in the format `#rrggbb` or `#rgb` where r, g, b are hex bytes for the red, green and blue color components. The three-digit codes `#rgb` are equivalent to the six-digit codes `#rrggbb` where both digits of the same component are equal.
    * The **font name** can be specified by the `name` attribute.
    * The **font size** is controlled by the `size` attribute. It can take the values "x-small", "small", "medium", "large", "x-large", "xx-large" for font sizes 7, 10, 12, 14, 18, 24pt, respectively. Alternatively, the point or pixel size can be specified directly if "pt" or "px", respectively, is appended.
    * _Example:_ This code adds a two-line footer in which the 2nd line is in blue, 8-pt, underlined Times New Roman:


    
    
      Chart.Foot.Text.Clear;
      Chart.Foot.Text.Add('Reference:');
      Chart.Font.Text.Add('<font name="Times New Roman" size="8pt" color="blue"><u>www.freepascal.org/</u></font>');
    

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** It is not recommended to switch font name and font size within the same line because TAChart drawers cannot exactly align the base line of characters in different fonts and font sizes

The demo application in folder _demo/html_ of the Lazarus installation heavily makes use of embedded HTML codes. 

### Legend

Chart legend is a table with each item containing an icon and a text line. Grid lines may be controlled by `GridHorizontal` and `GridVertical` properties. 

Legend supports various alignments and can be located inside the chart or on the sidebar. Legend can be displayed in two or more columns by setting `ColumnCount` property. Setting `ColumnCount` to a very large value effectively creates "horizontal" legend. `ColumnCount = 0` automatically adjusts the legend orientation and number of columns to the size of the chart and position of the legend. 

Legend items are generated based on chart series which have both `Active` and `ShowInLegend` set to true. Depending on `Legend.Multiplicity` series can produce a single item or one item per point. 

Legend items can be grouped together under sub-headers. Sub-headers are taken from `GroupNames` property, and each series can use `Legend.GroupIndex` property to indicate its group. 

Legend items are sorted as following: 

  * By group index, items without group (`GroupIndex=-1`) going first to avoid confusion with the last group.
  * By `Order` property, items without explicit order (`Order=-1`) going last.
  * By creation order of series.
  * For multiple items per series, by generation order (for standard series it is the order of points).



Sorted items are then added to the legend table in either by rows or by columns depending on the `ItemFillOrder` property. 

#### Legend item text

The text of a legend item is generated by the corresponding series based on the `Legend.Format` property. This property is used as a first argument for the [`SysUtils.Format`](<http://www.freepascal.org/docs-html/rtl/sysutils/format.html>) function, with the second argument containing following data items: 

  * For per-series multiplicity: 
    * 0: Series `Title`
    * 1: Series `Index`
  * For per-point multiplicity: 
    * Same as mark labels



If the property `TextFormat` is switched to `tfHTML` then HTML entities and tags can be embedded for special formatting of legend texts. See HTML for details. 

#### User-defined legend items

Arbitrary legend items can be generated by overriding `OnCreate` and `OnDraw` events of series `Legend` property. 

Note that user-defined item count is controlled solely by <codettUserItemsCount property and does not depend on `Multiplicity`. Also note that `GroupIndex` and `Order` properties are actually per-item, so you can totally override entire legend from a single (perhaps fictive) series. 

### Arrows

Arrowheads can be drawn at the end of axis lines, constant lines, mark links, etc. It is controlled by `TChartAxis.Arrow`, `TConstantLine.Arrow`, `TChartMarks.Arrow` correspondingly. 

Arrowhead shape is determined by `Width`, `Length` and `BaseLength` properties, for example: 

  * Thin wedge: `BaseLength = 0`
  * Triangle: `BaseLength = Length`
  * Rhombus: `BaseLength = 2 * Length`
  * Inverted Triangle: `BaseLength > 0`, `Length = 0`



This image shows these arrows created for `Length=10` and `Width=5`.[![TAChart Arrows.png](https://wiki.freepascal.org/images/b/be/TAChart_Arrows.png)](</File:TAChart_Arrows.png>)  


### Transparency

`Transparency` property of the series represents a level of transparency from 0 (fully opaque) to 255 (fully transparent -- i.e. invisible). 

**Compatibility note** : FPVectorial and TFPCanvas drawers currently do not support transparency because of limitations of the underlying libraries. Printer and WMF drawers can not support transparency in principle. 

**Optimization note** : Transparency support in the LCL is rudimentary, so current TCanvas drawer implementation may be slow with many transparent series and large charts. To improve efficiency, it is recommended to use as few different transparency levels as possible and to not interleave series with different transparencies. Alternatively, use BGRABitmap as back-end, since it has faster transparency implementation. 

### ChartStyles

[![tchartstyles 200.png](https://wiki.freepascal.org/images/b/b6/tchartstyles_200.png)](</File:tchartstyles_200.png>)

`TChartStyles` can be employed to control the appearance of the "layers" of area, line, polar, b-spline and bar series with multiple y values. There are properties to define the `Brush`, `Pen`, `Pointer`, `Font` and `Text` for each layer. Boolean `UseBrush`, `UsePen`, `UsePointer` and `UseFont` properties are available to revert to the corresponding series settings (if `false`). `Font` is the font of the series labels in that particular layer. `Text` is used as legend text when the `Legend.Multiplicity` of the associated series has been set to `lmStyle`. The event `OnAddStyleToLegend` is meant to exclude a particular series level from the legend if required (set the event argument `ShowInLegend` to `false`). 

ChartStyles can also be used to color the regions between axis ticks in a banded way. 

A special application exists also for `TBarSeries` (which must have a single layer in this case), `TBubbleSeries` and `TPieSeries`: When `Styles` is not `nil` for these series types, the styles define the formatting of individual data points. This way, for example, the fill pattern of pies in a TPieSeries can be varied from slice to slice.   
  


## Marks

Marks annotate certain points of the chart. These points can be defined by a series or an axis. Typically, mark consists of some graphical element (such as an axis tick) and a text label. However, either of these elements can be omitted. 

You can see examples in the "labels" demo. 

### Mark labels

The label text can be enclosed in a box, controlled by `LabelBrush` and `Frame` properties. The text itself is created based on the data source items, with the help of `Format` property. This property is used as a first argument for the [`SysUtils.Format`](<http://www.freepascal.org/docs-html/rtl/sysutils/format.html>) function, with the second argument containing following data items: 

  * 0: Y
  * 1: Y as a percentage of the Y total
  * 2: Text
  * 3: Y total
  * 4: X



Where "Y total" is the sum of all Y values for this source. Note that not all sources supply all the items above. 

Some pre-defined formats can be set via the `Style` property. 

If the source is mutli-valued, `YIndex` property determines which Y value is used. If `YIndex = -1`, a separate label is displayed for each Y value, which is useful for stacked series. The `Text` element of the source's `TChartDataItem` records can contain indidual texts for each Y value level; they must be separated by the `LabelSeparator` defined by the chart source (by default `'|'`). 

The built-in predefined label texts can be overridden by provding a handler for the `OnGetMark`/`OnGetMarkText` events of the series. Note that `OnGetMark` has been deprecated in Laz v3.99 to be replaced by event `OnGetMarkText` which gives access to more parameters. 

The text is rendered using the `LabelFont`. Note in particular that TAChart supports arbitrary font `Orientation`. 

Mark labels are usually included in margins calculation to guarantee that all labels fit in the viewport. This behavior can be turned off by setting `AutoMargins` property to `false`. 

#### Multi-line marks

If the mark text contains [`LineEnding`](<http://www.freepascal.org/docs-html/rtl/system/lineending.html>) character sequence, it is split into several lines. Lines of different length are aligned according to `Alignment` property. 

#### HTML codes in marks

If the property `TextFormat` is switched to `tfHTML` then HTML entities and tags can be embedded for special formatting of marks. See HTML for details. 

### Mark positions and attachment

The mark position relative to the marked point is determined by the marks owner (series or axis). 

Common mark properties include `Distance`, which measures the distance from the origin point to the attachment point in image units, and `Attachment`, which controls whether the attachment point is considered to be in the center or at the edge of the label box. 

Additionally, basic chart series have a property `MarkPositions` specifying the direction of labels' offsets relative to series data points as following: 

  * `lmpOutside` \-- away from the center of the data points cloud; for TBarSeries and TAreaSeries it has a special meaning: away from the ZeroLevel defined for this series.
  * `lmpPositive` \-- positive direction of series' Y axis
  * `lmpNegative` \-- negative direction of series' Y axis
  * `lmpInside` \-- towards the center of the data points cloud; for TBarSeries and TAreaSeries: towards the ZeroLevel



In Lazarus 2.0+, TBarSeries and TAreaSeries have an additional property `MarkPositionCentered` which allows to center the marks origins between data points and zero level. In order to place marks exactly at the center of the bars of a BarSeries or at the center of the drop line of an AreaSeries, the following properties must be changed additionally: `Marks.Distance = 0`, and `Marks.Attachment = maCenter`. 

### Rotation of marks

Marks can be rotated by any angle in a resolution of 1/10 degree by modifying the `Orientation` of their font; it is specified by the 10-fold value in degrees, i.e. the value 450 refers to an angle of 45°. 0° means horizontal orientation, the angle increases in the counter-clockwise direction. 

The rotation normally occurs with respect to the center of the label. Axis and series marks additionally have a property `RotationCenter` to shift the center of rotation to an edge of the text: 

  * `raCenter` \-- default setting, i.e. rotation occurs around the text center
  * `raLeft` \-- the rotation center is at the left edge of the label.
  * `raRight` \-- the rotation center is at the right edge of the text.
  * `raEdge` \-- ignored in case of series marks (will be replaced by raLeft). But in case of axis marks the center of rotation is automatically moved to the text start or end in order to avoid rotation of the label into the chart.



## Drawers

For low-level drawing routines, TAChart uses special set of classes implementing `IChartDrawer` interface. This allows such features as printing charts and exporting them to SVG format. 

These classes are called _drawing back-ends_ or _drawers_ for short. 

### TCanvas drawer

`TCanvasDrawer` is the default drawer used to display chart on TCanvas. This includes screen and various raster image formats. The image produced by this drawer is used as a reference when developing and debugging other back-ends. 

### TFPCanvas drawer

`TFPCanvasDrawer` is similar to TCanvas drawer, but based on `TFPCanvas` (which is a `TCanvas` analog implemented in the FCL instead of the LCL). 

See the "nogui" demo for an example. 

Although `TFPCanvas` and, correspondingly, `TFPCanvasDrawer` have a restricted implementation of some TAChart features, their important advantage is the possibility they offer of compiling an application with the nogui widgetset. This is particularly useful for Web applications, which can then generate raster chart images without any further graphic dependency, or a need to install X/Gtk/Qt on the server. 

### SVG drawer

`TSVGDrawer` produces text stream with the image of the chart in [SVG](<http://www.wikipedia.org/wiki/SVG> "wikipedia:SVG") format. Similarly to TFPCanvas drawer, it is independent of LCL and can be used in Web applications to generate vector charts in nogui widgetset. 

See "save" demo for an example. 

For this drawer, image unit is an SVG canvas unit instead of a pixel. 

Note that due to the nature of SVG, there is no way to measure font dimensions, so they are approximated crudely. This may result in problems like label text not fitting in the mark rectangle, especially in browsers like Firefox that do not support `textLength` attribute. 

### OpenGL drawer

`TOpenGLDrawer` draws chart on the current [OpenGL](<http://www.wikipedia.org/wiki/OpenGL> "wikipedia:OpenGL") context. It is suitable to be used in games and other OpenGL-only applications. 

OpenGL drawer expects, but does not set by itself, an orthogonal projection. See "opengl" demo for an example. 

Note that, like in OpenGL itself, `TOpenGLDrawer` font support is extremely limited. 

### Printer drawer

`TPrinterDrawer` draws chart on the printer canvas. It does not flush the page. 

Although printer canvas is a descendant of `TCanvas`, and so printing can be done using the default drawer, `TPrinterDrawer` does proper re-scaling of image coordinates according to printer vs screen DPI. 

You can use this drawer to export chart to PDF format using one of the available PDF writer products. 

See "print" demo for an example. 

Note that this drawer is located in a separate TAChartPrint package. 

### AggPas drawer

`TAggPasDrawer` draws chart using [AggPas library](<http://www.crossgl.com/aggpas>). 

AggPas offers high-speed antialiased drawing and is included in Lazarus sources. Unfortunately, the library is currently not maintained and there are some limitations in TAChart support. 

Note that this drawer is located in a separate TAChartAggPas package. 

### BGRABitmap drawer

`TBGRADrawer` draws chart using [BGRABitmap](<BGRABitmap.md> "BGRABitmap") library. 

BGRABitmap is recently created and actively developed graphics library, offering, in particular, anti-aliasing and rich selection of gradients. 

Currently BGRABitmap supports all TAChart features, but is somewhat slower then other drawing methods. 

Note that this drawer is located in a separate TAChartBGRA package, which depends on external `bgrabitmappack` package. 

### FPVectorial drawer

`TFPVectorialDrawer` draws chart using [fpvectorial](<fpvectorial.md> "fpvectorial") library. FPVectorial offers exporting to various vector formats, including SVG, PDF, CorelDraw and even instructions for metal cutting machines. 

It currently has some limitations in TAChart support, but is actively developed. 

Note that this drawer is located in a separate `TAChartFPVectorial` package, which depends on `fpvectorialpkg` package. 

### WMF drawer

`TWindowsMetafileDrawer` draws chart into a [Windows Metafile](<http://www.wikipedia.org/wiki/Windows_Metafile> "wikipedia:Windows Metafile"). 

It uses WinAPI directly, and so will only work on Windows. WMF support has been added to fpvectorial which provides a cross-platform alternative to this package. 

## Navigation

Chart _navigation_ consists of two parts: moving logical extent around without changing zoom factor, and visualizing the logical extent's position and size relative to the full extent. 

Moving extent (but not visualizing it) is possible by using panning tools. 

You can see examples in the "navigation" demo. 

### Scroll bars

[![tchartnavscrollbar 200.png](https://wiki.freepascal.org/images/e/ea/tchartnavscrollbar_200.png)](</File:tchartnavscrollbar_200.png>)

`TChartNavScrollBar` is a `TCustomScrollBar` descendant with additional `Chart` property referencing the chart. `TChartNavScrollBar` synchronizes its position with chart extent in both directions. If the logical extent is equal to or larger than the full extent, navigation scroll bar does nothing. 

Setting `AutoPageSize = true` lets `TChartNavScrollBar` pick a page size proportional to the logical extent. 

Note that `Min` and `Max` properties are _not_ changed automatically. It is recommended to set `Min = 0` and `Max` to some fairly large integer value to avoid rounding issues. Also note that `TChartNavScrollBar` does not automatically align or attach itself to a chart, so it can be arbitrarily positioned on the form. 

### Navigation panel

[![tchartnavpanel 200.png](https://wiki.freepascal.org/images/4/4f/tchartnavpanel_200.png)](</File:tchartnavpanel_200.png>)

`TChartNavPanel` component displays logical and full extent of an assigned chart as differently colored rectangles, allowing user to drag the logical extent rectangle if `AllowDragNavigation = true`. 

If `MiniMap = true`, the panel additionally displays the chart series. 

`TChartNavPanel` can have arbitrary size, but it is recommended to keep height to width proportion the same as in the assigned chart. Setting `Proportional = true` will enforce the same proportions even if the above condition is not met, at the cost of some wasted space on the panel. 

### Live View

[![tchartliveview 200.png](https://wiki.freepascal.org/images/4/4a/tchartliveview_200.png)](</File:tchartliveview_200.png>)

Although not a navigation tool directly, `TChartLiveView` is related to navigation. In the case of a long array of incoming data it provides an easy way to display a viewport with only the most recent data while older data are flowing to the left out of the viewport. 

The viewport is updated whenever the full extent of the associated chart changes. However, when property `Active` is set to `false` the viewport is fixed -- this is a setting which allows the user to review older data, or change the extent while still new data are coming at the same time. 

Property `ViewportSize` defines the width of the visible viewport, in graph units. If set to 0, the width of the current `LogicalExtent` is used instead. 

The enumerated property `ExtentY = (lveAuto, lveFull, lveLogical)`, finally, determines the height of the viewport: 

  * `lveAuto` adjusts the height to the visible data range automatically. This works also when the chart contains several y axes. When a `Range` is defined for an axis (i.e. by setting `axis.Range.Min`/`axis.Range.UseMin` and/or `axis.Range.Max`/`axis.Range.UseMax`) the component keeps it as long as series values are within this range.
  * `lveFull` freezes the height to the full extent of the chart.
  * `lveLogical` freezes the height to the logical extent of the chart.



The methods `StoreAxisRange` and `RestoreAxisRange` are used for internal purposes because the component may change the `Range` parameters of an axis. Be careful when calling these methods because they may disrupt operation of the live view if not done properly. 

## Additional components

### Legend panel

`TChartLegendPanel` provides a legend which can be placed anywhere on the form. The legend is taken from the chart with is assigned to the property `Chart` of the LegendPanel. The chart’s own legend should be turned off. 

### Chart listbox

`TChartListbox` is a versatile legend replacement which displays an icon and the series title for each series of the chart attached to the property `Chart` of the listbox. But in addition to the standard legend, it also has checkboxes for each series. Unchecking a checkbox hides the associated series, checking it shows the series again. The listbox listens to changes in the associated chart and updates automatically. 

The property `CheckStyle` can be used to switch the listbox from a checkbox-like behavior (`cbsCheckbox`, an arbitrary combination of series can be checked) to a radiobutton-like behavior (`cbsRadioButton`, only a single series can be checked). Depending on `CheckStyle` either a radiobutton or a checkbox state icon is displayed for each series. 

Events are fired when the checkbox/radiobutton or the series title are clicked (`OnCheckboxClick` or `OnItemClick`, respectively), or when the series icon is double-clicked (`OnSeriesIconDblClick`). The latter event is thought to open a dialog for changing the series color, pointer style, series title etc. 

The listbox items are collected in a different way than for a conventional legend. The populating process ignores the series property `Legend.Visible` which controls whether the series is to be shown in the legend or not. Instead of it, there is an event `OnAddSeries` in which the `var` parameter `Skip` should be set to `true` if the series passed as another argument should be excluded from the listbox. Every series is listed only with a single item, i.e. the series’ `Multiplicity` is ignored as well. Moveover, grouping of items is not supported either. 

The order of listbox items is normally given by the order of series items in the chart's legend and thus depend on the chart's `Legend.Inverted` and the series' `Legend.Order` parameters. Moreover, items can be rearranged independently of the legend by calling the `ExchangeSeries(AIndex1, AIndex2)` method with the listbox indices as parameters. There is also a method `Sort` to sort the listbox by series titles (the sort order can be inverted by setting its optional `Descending` parameter, or `Chart.Legend.Inverted` to `true`. Note that the `Sort` method works only on the series having the default `Legend.Order` (-1). 

### Chart image list

`TChartImageList` collects the icons displayed for each series in the legend and can be used for imagelist-aware controls, such as treeviews, listviews or menus to show icons for each series. The icons are extracted from the chart which is assigned to the `Chart` property of the component. The image list is notified of changes in the chart and updates its images automatically whenever something changes with a series. The event `OnPopulate` is fired if the image list is rebuilt. 

Note that this kind of image list is _volatile_ , i.e. its images are not stored in the form's lfm file. Nevertheless, at runtime, it can be used as a standard image list with other images. Just add these non-series images **before** assigning the chart to the image list. Use the property `FirstSeriesIndex` to get the image index of the first series contained in the image list. Alternatively, find the image index of a specific series by calling the function `ImageIndexOfSeries(series)`. 

In Laz 2.1+ the `TChartImageList` can be used also at designtime. It stores only those images not assigned to a series. 

### Chart combobox

The `TChartCombobox` is an extended combobox. Depending on the property `Mode` (`ccmPointerStyle`, `ccmPenStyle`, `ccmPenWidth`, `ccmBrushStyle`) it is populated with names and corresponding icons for pointer style, pen style, pen width or brush style selection. Therefore, it is useful for gui elements in which the user can modify the appearance of series and other chart objects. 

The combobox is not linked to a particular chart. Use the `OnChange` event to transfer the modified property to the series. 

## TeeChart compatibility

TeeChart is a standard set of charting components used by Delphi. Although TAChart does not have a goal to to be compatible with TeeChart, basic feature set and names are very similar, so simple examples may work in both libraries equivalently. 

More complex features -- in particular, multi-value series, series sources, multiple axes -- are implemented differently from TeeChart. This is by design and will not change. See [comparison page](<Comparing_TAChart_with_Delphi's_TeeChart_Standard.md> "Comparing TAChart with Delphi's TeeChart Standard") for more detailed info. 

To assist porting of TeeChart code to the TAChart, you can use `TAChartTeeChart` unit. It contains class helpers adding or emulating some TeeChart-specific properties and methods. 

Note that it is NOT recommended to use this unit for normal development, since the emulation is only intended to simulate the subset of features implemented in TeeChart, and may not be reliable when used in conjunction with the full set of TAChart features. 

## Technical details

### Drawing order

Chart drawing consists of three stages: 

  1. _Preparation_ : At this stage, various internal data structures are initialized.
  2. _Measurement_ : At this stage, the chart calculates the sizes of all elements, and optimizes them for best presentation. Optimization may require several iterations, so the measurement stage is often the heaviest one both in terms of implementation complexity and running time.
  3. _Drawing_ : At this stage, the actual chart image is displayed.



There also exists an ordering among various chart elements: 

  1. Background (using `TChart.Color` property).
  2. Back-wall (using `TChart.BackColor` property).
  3. Series and axes according to `ZPosition` property. For each series and axis, graphic elements are drawn before marks. Note that this protects marks against hiding by the axis/series they belong to, but not by other axes/series.
  4. Legend.
  5. Tools.



Note that `ZPosition` works for both 2-D and 3-D charts, so you can overlay series and axes in arbitrary order. 

### Coding style

Historically FPC, Lazarus and LCL sources contain a mix of coding styles, with the general rule being "be consistent with the surrounding code". 

However, since TAChart has many fewer contributors, it is feasible to adopt and maintain a consistent style across all TAChart code. If you want to contribute to TAChart, please format your code accordingly. Also remember that any coding style may be violated in certain situations when the reason is good enough, but please explain that reason if you do so. 

#### Spaces

  * No double spaces anywhere.
  * Spaces after: operations, comma, semicolon, assignment, closing parenthesis in expressions (not in function calls).
  * Spaces before: operations, assignment, opening parenthesis in expressions (not in function calls).



#### Lines

  * No double empty lines anywhere.
  * Empty lines between procedures, classes, unit sections. Rare empty lines inside procedure bodies to separate logical blocks.
  * Line length below 80 characters, with rare exceptions.
  * Single statement per line, except _if_ ... _then_ {_exit_ |_break_ |_continue_} and some rare cases of mass assignment.
  * If the line is too long, line breaks may be inserted after at the following symbols, in order of decreasing priority: keywords, opening parenthesis, opening square bracket, semicolon, comma, operation.



#### Indentation and blocks

  * Always two spaces, both for blocks and continuation lines.
  * _begin_ on the same line as the control statement, _end_ aligned with the control statement.
  * _end_ always alone on the line. In particular, write _end else begin_ on two lines.
  * Put _then_ or _do_ on a separate line to separate a complex condition from the statement body:


    
    
     **while**
       long condition or
       another long condition
     **do**
       loop body
    

  * Use _begin_ /_end_ only when necessary (i. e. not for single statements).



#### Comments

  * Put a license header at the beginning of every file.
  * Single-line comments everywhere except the license header and auto-generated class headers in implementation section.
  * Use only full sentences in comments, starting with a capital letter and ending with a full stop.
  * Comments should be placed before commented code, except in rare cases where a comment fits at the end of the same line.
  * Comments should only include information not evident from the source. In particular, choose meaningful procedure and argument names in preference to adding comments describing their usage.



#### Names

  * Constants use ALL_CAPS_WITH_UNDERSCORE, everything else use CamelCase.
  * Local variables start with a lower-case letter, everything else starts with an upper-case letter, even when FPC library disagrees (e.g. ``Math``, not ``math``).
  * Class fields start with 'F', arguments start with 'A', types start with 'T', TAChart units start with 'TA'.



#### Variable declarations

  * Initialize when possible.
  * Local variables after nested procedures unless used by them.
  * Group variables by type.



#### Classes and methods

  * Methods are grouped per-class, methods inside class are sorted alphabetically both in interface and implementation. There should be zero Code Observer warnings about 'Unsorted members'.
  * If there is a need to group methods by topic, use visibility specifiers as topic separators. In particular, group overridden methods separately from the newly introduced ones.
  * Use _strict private_ /_strict protected_ visibility where possible.



#### Hints and warnings

  * Code should compile with zero hints and warnings.
  * Silence any "unused parameter" warning with the ``Unused`` procedure from the TAChartUtils unit, or use the {%H-} Lazarus IDE directive.



#### Control flow

  * Use _exit_ or _raise_ to abort method execution in case of a violated pre-condition.
  * Use enumerators where possible.
  * Use _with_ carefully, and only where its use significantly saves code size and in the minimal possible range.
  * Limit all procedures to 50-60 lines, and use nested procedures liberally.

---

_Source: [https://wiki.freepascal.org/TAChart_documentation](https://web.archive.org/web/20250323021440/https://wiki.freepascal.org/TAChart_documentation)_
