# Comparing TAChart with Delphi's TeeChart Standard

## Contents

  * 1 Introduction
  * 2 Shared TeeChart classes
    * 2.1 TChartPen
    * 2.2 TTeeFont
    * 2.3 TTeeGradient
    * 2.4 TTeeShadow and TTeeEmboss
  * 3 TeeChart's TChart
    * 3.1 Public methods
    * 3.2 Properties
    * 3.3 Events
  * 4 TeeChart's TChartTitle
    * 4.1 Public methods
    * 4.2 Properties
  * 5 TeeChart's TChartLegend
    * 5.1 Public methods
    * 5.2 Properties
      * 5.2.1 TLegendSymbol
  * 6 TeeChart's TChartAxis
    * 6.1 Public methods
    * 6.2 Properties
      * 6.2.1 TChartAxisTitle properties
  * 7 TeeChart's TLineSeries
    * 7.1 Public methods
    * 7.2 Properties
      * 7.2.1 TChartValueList
      * 7.2.2 TSeriesMarks
      * 7.2.3 TSeriesPointer
    * 7.3 Events
  * 8 TeeChart's TBarSeries
    * 8.1 Public methods
    * 8.2 Properties
  * 9 TeeChart's THorizBarSeries



## Introduction

This page contains a brief comparison of properties and methods between (trunk version of) Lazarus [TAChart](<TAChart.md> "TAChart") package and TeeChart Standard package which comes with Delphi XE2. 

It can be used as a guide when porting applications from Delphi, as well as a source of inspiration for future TAChart development. 

See also [TAChart_documentation#TeeChart_compatibility](<TAChart_documentation.md> "TAChart documentation"). 

A note on the structure of this document: Because it is intended to help porting applications from TeeChart to TAChart the basis of this comparison will be TeeChart. We will list the TeeChart methods and properties, give a short description based on the help file or own experiments and experience, and discuss the differences to TAChart and how they can be resolved. If a feature is not available in TAChart we will often use short statements like "not available" without giving reference to TAChart. 

## Shared TeeChart classes

In this section, we compare classes used by TeeChart at several places in various other classes. 

#### TChartPen

[![TeeChart TChartPen properties.png](https://wiki.freepascal.org/images/e/e9/TeeChart_TChartPen_properties.png)](</File:TeeChart_TChartPen_properties.png>)

In addition to `TChartPen`, TeeChart uses also descendants with slightly different default settings, like 

  * `TAxisGridPen` with `Color = clGray` and a new properties `DrawEvery` and `ZPosition`
  * `TChartAxisPen` with `Width = 2`
  * `TChartHiddenPen` with `Visible = false`
  * `TWhitePen` with `Color = clWhite`



Here are the properties of `TChartPen`: 

  * `Color`: Like TAChart
  * `EndStyle`: Like TAChart's `EndCap`
  * `Mode`: Like TAChart
  * `SmallDots`: Overrides `Style` for drawing a dotted line with narrow dots at a distance given by `SmallSpace`.
  * `Style`: Like TAChart
  * `Visible`: Not availabe in TAChart, `Visible=false` can be replaced by `Style = clClear`.
  * `Width`: Like TAChart



#### TTeeFont

[![TeeChart TTeeFont properties.png](https://wiki.freepascal.org/images/b/b9/TeeChart_TTeeFont_properties.png)](</File:TeeChart_TTeeFont_properties.png>)

  * `Charset`: Standard TFont property
  * `Color`: Standard TFont property
  * `Depth`: ???, not available in TAChart.
  * `Emboss`: Upward-directed shadow, see `TTeeEmboss`. Not available.
  * `Gradient`: Draws characters with gradient, see `TTeeGradient`. No gradient support in TAChart.
  * `Height`: Standard TFont property.
  * `InterCharSize: Integer`: Defines the width of the character-to-character spacing. Not available.
  * `Name`: Standard TFont property.
  * `Orientation`: Standard TFont property.
  * `Outline`: Pen used to draw a border around font. To be used with big font sizes (for example 30). Not supported.
  * `Picture`: Defines a picture to be filled into the font region. Not supported.
  * `Pitch`: Standard TFont property.
  * `Quality`: Standard TFont property.
  * `Shadow`: Shadow drawn underneath the characters. See `TTeeShadow`. Not supported.
  * `Size`: Standard TFont property.
  * `Style`: Standard TFont property.



#### TTeeGradient

[![TeeChart TTeeGradient properties.png](https://wiki.freepascal.org/images/6/64/TeeChart_TTeeGradient_properties.png)](</File:TeeChart_TTeeGradient_properties.png>)

TAChart does not support gradients at the time of this writing. Here is a reference of the properties that are published for TeeChart: 

  * `Angle`: Rotation angle of gradient, in degrees.
  * `Balance`: Determines the preference of use between gradient start and end colors.
  * `Direction = (gdTopBottom, gdBottomTop, gdLeftRight, gdRightLeft, gdFromCenter, gdFromTopLeft, gdFromBottomLeft, gdRadial, gdDiagonalUp, gdDiagonalDown)`: Gradient direction and type.
  * `EndColor, MidColor, StartColor`: The gradient fill depends three colors: `StartColor`, `MidColor` and `EndColor`. For a 2-color gradient, set `MidColor=clNone`. `MidColor` seems to be ignored for non-rectangular gradients.
  * `RadialX, RadialY`: Offset in pixels to displace the gradient radial center.
  * `SubGradient`: Applies a secondary (superimposed) gradient to the gradient area. Has the same properties as `TTeeGradient` except for another `SubGradient`.
  * `Visible`: Turns gradient on/off.



#### TTeeShadow and TTeeEmboss

[![TeeChart TTeeShadow properties.png](https://wiki.freepascal.org/images/8/80/TeeChart_TTeeShadow_properties.png)](</File:TeeChart_TTeeShadow_properties.png>)

TTeeShadow and TTeeEmboss have the same properties. The shadow of TTeeShadow points down-right, that of TTeeEmboss up-left. 

TAChart does not support shadows. Here is a reference of the published properties of TeeChart: 

  * `Clip`: Don't know what the clipping boundary is...
  * `Color`: Shadow color
  * `HorizSize, VertSize`: Shadow size in horizontal and vertical directions.
  * `Smooth, SmoothBlur`: Activates/deactivates a soft shadow
  * `Transparency`: Percentage of how transparent the shadow is.
  * `Visible`: Turns shadow on and off.



## TeeChart's TChart

### Public methods

to be written ... 

### Properties

[![TeeChart object explorer 1.png](https://wiki.freepascal.org/images/6/67/TeeChart_object_explorer_1.png)](</File:TeeChart_object_explorer_1.png>)

  * `Align`: Standard VCL/LCL property.
  * `AlignWithMargins`: VCL property, missing in LCL
  * `AllowPanning`: Controls built-in panning support (panning directions, turn panning off). TAChart has an equally-named property which only turns built-in panning on/off; further panning control is possible by linking a TChartToolset to the chart.
  * `Anchors`: Standard VCL/LCL property.
  * `Animations`: Don't know, a click on the ellipsis button opens the series editor, maybe a feature of TeeChart Professional. Property missing in TAChart.
  * `AutoSize`: VCL/LCL property of some classes, don't know its effect on the chart, no entry in help file. Not used in TAChart.
  * `AxisBehind: Boolean`, default `true`: In 3D view, draws axis in the back wall, behind the series. If false, axes are drawn at the front wall, before the series. Property missing in TAChart, but effect can be achieved by TChartAxis.ZPosition.
  * `AxisVisible: Boolean`, default `true`: Turns on/off axis title, marks, ticks, and grid of all axes. The same as with TAChart.
  * `BackImage`: Allows to add a background image (see also other "BackImage*" properties). No image support in TAChart.
  * `BackImageInside: Boolean`, default `false`: If `true` the `BackImage` is drawn in the rectangle defined by the axes or frame, otherwise it is a background image of the entire chart. No image support in TAChart.
  * `BackImageMode`: Defines the way the image is drawn/repeated on the background, options (pbmStretch, pbmTile, pbmCenter, pbmCustom). No image support in TAChart.
  * `BackImageTransp: Boolean`, default `false`: If true the transparency is that of a TBitmap, i.e., the transparent color is taken from the left-bottom pixel of the image. If `BackImage` is an icon, `BackImageTransp` is `true` and cannot be changed. No image support in TAChart.
  * `BackWall`: See separate chapter on `TChartBackWall`. Missing in TAChart.
  * `BevelInner`, `BevelOuter`, `BevelWidth`: Bevel parameters like those of a `TPanel` \-- TeeChart's TChart inherits from TCustomPanel. Missing in TAChart.
  * `Border`: A frame around the entire chart. See `TChartPen`, `Visible = false` by default. Not available.
  * `BorderRound`: If greater than 0 the `Border` is drawn as a `RoundRect` with this radius. Not available.
  * `BorderWidth`: Seems to have the same effect as the LCL's `BorderSpacing.Around`
  * `BottomAxis`: See `TChartAxis`.
  * `BottomWall`: See separate chapter on `TChartBottomWall`. Missing in TAChart.
  * `Chart3DPercent`: Ratio between chart dimensions and chart depth `Chart.View3D` is true. Similar to TAChart's property `Depth` of chart and some series.
  * `ClipPoints: boolean`, default `true`: Toggles the drawing of series points within chart boundaries, preventing other chart regions from being overwritten. Not available in TAChart, works as always `true`.
  * `Color`: Background color of entire chart, the same as with TAChart.
  * `Constraints`: Standard VCL / LCL property.
  * `Cursor`: Standard VCL / LCL property.
  * `CustomAxes`: List of axes. Similar to TAChart's `AxisList` which, however, contains also the default axes.
  * `CustomHint`: ???, not available in TAChart
  * `DepthAxis`: Characteristic of the bottom-right axis in 3D chart, see separate chapter on TChartDepthAxis. TAChart does not have a depth axis in 3D charts.
  * `DepthTopAxis`: Characteristics of the left-top axis in 3D chart, see separate chapter on TChartDepthAxis. Missing in TAChart.
  * `DockSite`: Enables Docking, not supported by LCL and TAChart.
  * `DragCursor, DragMode`: Drag and Drop support, standard VCL/LCL property.
  * `Emboss`: See `TTeeEmboss`. Some kind of upward shadow of entire chart background, not available.
  * `Enabled`: Not in help file, at least, turns off zooming and panning.
  * `Foot`: See `TChartTitle`. Like in TAChart.
  * `Frame`: See TChartPen. Like in TAChart.
  * `Gradient`: See `TTeeGradient`. Draws a gradient behind the entire chart. No gradient support in TAChart.
  * `Height`: Standard VCL/LCL property.
  * `HelpContext, HelpKeyword, HelpType`: Standard VCL property.
  * `Hint`: Standard VCL/LCL property.
  * `Left`: Standard VCL/LCL property.
  * `LeftAxis`: See `TChartAxis`.
  * `LeftWall`: See separate chapter on `TChartLeftWall`, describes left wall of 3D charts. Missing in TAChart.
  * `Legend`: See `TChartLegend`, describes legend parameters, like in TAChart.
  * `LiveBindings`: New VCL properties introduced in Delphi XE2. Not supported by LCL and TAChart.
  * `Locked`: ???, missing in TAChart
  * `MarginBottom, MarginLeft, MarginRight, MarginTop`: Margin between "Border" and Axis, corresponds to TAChart's `MarginExternal`.
  * `MarginUnits = (muPercent, muPixels)`: According to the help file: "When MarginUnits is muPercent, chart margins are calculated as percentage of total chart width and height. Percent margins are useful to obtain the same margin proportions, no matter the chart size." TAChart supports margin only in pixels.
  * `Margins`: Standard VCL property, in combination with `AlignWithMargins`. Like LCL's `BorderSpacing`.
  * `Monochrome`: If `true`, turns off colors and renders chart in grayscale. Not supported.
  * `Name`: Standard VCL/LCL property
  * `Pages`: Accesses pages for a multi-page chart. Not supported.
  * `Panning`: Properties for built-in panning support. Not available in TAChart, default panning by right mouse button, parameters, can be changed by means of chart tools.
  * `ParentColor`: Standard VCL/LCL property.
  * `ParentCustomHint`: ???, missing in TAChart.
  * `ParentShowHint`: Standard VCL/LCL property.
  * `PopupMenu`: Standard VCL/LCL property.
  * `PrintProportional`: No direct correspondence, in TAChart printing can be done as described [here](<TAChart_documentation.md> "TAChart documentation").
  * `RightAxis`: See `TChartAxis`.
  * `RightWall`: See separate chapter on `TChartLeftWall`, describes left wall of 3D charts. Not available.
  * `ScrollMouseButton`: Sets the button that will trigger scrolling when pressed. TAChart does not have built-in scrolling support, effect can be achieved by chart tools.
  * `SeriesGroups`: According to the help file: "Chart Series can be grouped. Series groups can optionally be displayed at the chart editor. The main purpose of groups is to have an easy way to show or hide multiple series, that is, show or hide individual groups of series." Not available in TAChart.
  * `SeriesList`: Grayed out. In TAChart accessible as property `Series`.
  * `Shadow`: See `TTeeShadow`. No shadow support in TAChart.
  * `ShowHint`: Standard VCL/LCL property.
  * `SubFoot`: See `TChartTitle`. TeeChart allows for a second footer, TAChart does not.
  * `SubTitle`: See `TChartTitle`. TeeChart allows for a second title, TAChart does not.
  * `TabOrder, TabStop, Tag`: Standard VCL/LCL properties.
  * `Title`: See `TChartTitle`. Similar to TAChart.
  * `Tools`: Property is disabled in TeeChart Standard. Chart tools are an essential part of TAChart.
  * `Top`: Standard VCL/LCL property.
  * `TopAxis`: See `TChartAxis`. Parameters for second x axis above the chart. Property not available in TAChart, but can be accessed by `AxisList` in object tree.
  * `Touch`: Touch [gestures](</index.php?title=gestures&action=edit&redlink=1> "gestures \(page does not exist\)") are not supported by Lazarus.
  * `UseDockmanger`: Docking is not supported by Lazarus.
  * `View3D, View3DOptions, View3DWalls`: Details on 3D charting. In TAChart, 3D effect can only be achieved by `Depth` properties of chart and some series.
  * `View3DOptions`: Not supported.
  * `View3DWalls`: Draw left and bottom "walls" to simulate 3D effect. Not available.
  * `Visible`: Standard VCL/LCL property.
  * `Width`: Standard VCL/LCL property.
  * `Zoom`: Parameters to define the built-in zooming. Property not available in TAChart, but effect can be achieved by chart tools.



### Events

to be written ... 

## TeeChart's TChartTitle

### Public methods

to be written ... 

### Properties

[![TeeChartTitle object explorer.png](https://wiki.freepascal.org/images/7/7f/TeeChartTitle_object_explorer.png)](</File:TeeChartTitle_object_explorer.png>)

  * `AdjustFrame`: Controls if chart title and foot frames will be resized to full chart dimensions or to the title text width. Not available.
  * `Alignment`: The same as with TAChart.
  * `Bevel, BevelWidth`: Defines a bevel around the Title. Not available.
  * `Brush`: Corresponds to the Brush object that is used to fill the shape interior. Usage is a bit confusing, because there is also a separate `Color` property, and within the Brush there are `BackColor` and `Color` properties again. `Brush.Style=bsDiagCross` uses `Title.Brush.Color` as background, `Title.Color` as foreground color. What is `Brush.BackColor` for?
  * `Color`: Background color of the title. In TAChart, use `Brush.Color`.
  * `CustomPosition: boolean`: If `true`, title can be positioned by `Left` and `Top` properties. Not supported.
  * `Emboss`: See `TTeeEmboss`. Some kind of upward-directed shadow. Not supported.
  * `Font`: See `TTeeFont`. Similar to TAChart
  * `Frame: TChartPen`: A frame around the title, see `TChartPen`. Very similar to TAChart.
  * `Gradient`: Allows to fill the title background with a gradient, see `TTeeGradient`. No gradient support in TAChart.
  * `Left, Top`: Distance of title from chart edge, used if `CustomPosition=true`. Not available.
  * `Picture`: Background picture for the title. No picture support in TAChart.
  * `Shadow`: Shadow parameters. See `TTeeShadow`. No shadow support in TAChart.
  * `ShapeStyle = fosEllipse, fosRectangle, fosRoundRectangle`: Title shape, like TAChart's `Shape` which has more options.
  * `Text`: Like TAChart
  * `TextFormat = (ttfNormal, ttfHTML)`: Allows HTML formatting in the title. Available in Lazarus v1.8 or newer as enumeration (tfNormal, tfHTML).
  * `Transparency = 0..100`: Defines how much of the background is visible through the title (as percentage). Not available in TAChart.
  * `Transparent: boolean`: If true only the text is visible, all decorating elements are turned off. Missing in TAChart.
  * `Visible`: Show/hides title, like in TAChart.



## TeeChart's TChartLegend

### Public methods

to be written ... 

### Properties

[![TeeChart TChartLegend properties.png](https://wiki.freepascal.org/images/d/d7/TeeChart_TChartLegend_properties.png)](</File:TeeChart_TChartLegend_properties.png>)

  * `Alignment`: Defines location of legend, `laLeft, laRight, laTop, laBottom`. Top- and bottom-aligned legends are automatically drawn as row. The same property in TAChart has more options. Top- and bottom-aligned legends are still column-oriented, multicolumn legends can be achieved by property `ColumnCount`, and column-to-row ordering can be changed by `ItemFillOrder`.
  * `Bevel, BevelWidth`: Define the bevel around the legend, like that of a panel. Not available in TAChart.
  * `Brush, Color`: Defines parameters how to fill the background of the legend. TAChart has a `BackgroundBrush` property which behaves slightly different. The background color, e.g., is set by `BackgroundBrush.Color`, while TeeChart uses the property `Color`.
  * `Checkboxes`: Enables/disabled display of checkboxes or radiobuttons for each legend item. Not available in TAChart, but `TChartListbox` can be a replacement.
  * `CheckboxesStyle = (cbsCheck, cbsRadio)`: Not available in TAChart, but the legend can be replaced by a `TChartListbox` where you can select the property `CheckStyle`.
  * `ColorWidth`: Defines percentage of width of color rectangle associated with each legend item. Like `SymbolWidth` in TAChart which is in pixels, though.
  * `CurrentPage: Boolean`: When chart is divided into several pages this property, if `true`, displays only the items for the current page. No page support in TAChart.
  * `CustomPosition, Left, LeftPercent, Top, TopPercent, PositionUnits`: If `CustomPosition = true` legend positioning can be fine-tuned by `Left` and `Top` properties (if `PositionUnits = muPixels`), or by `LeftPercent` and `TopPercent` (if `PositionUnits = muPercent`). Not available.
  * `DividingLines`: Sets the pen attributes of optional lines that divide individual legend items. Corresponds to `GridHorizontal` and `GridVertical` in TAChart.
  * `Emboss: TTeeEmboss`: See `TTeeEmboss`. Parameters for some kind of upward shadow. Absent in TAChart.
  * `FirstValue: Integer`: Defines the index of the first value to be displayed in the legend, can be used to display a specific sub-set of series or points in the legend. Not available in TAChart, but series have a `Legend` property which controls if or how the series is shown in the legend.
  * `Font`: See `TTeeFont`. Similar to TAChart.
  * `FontSeriesColor: Boolean`: If `true` the legend item text is drawn in the same color as the series. Not directly available in TAChart, but effect can be achieved by handling the `OnDraw` event of the series.Legend.
  * `Frame`: See TChartPen. Similar to TAChart.
  * `Gradient`: See `TTeeGradient`. No background gradient support in TAChart.
  * `HorizMargin, VertMargin`: Specifies the number of screen pixels between legend and chart rectangle. In TAChart, use properties `MarginX` and/or `MarginY`.
  * `Inverted`: Legend items are drawn in the order first to last, or last to first. Available in TAChart since version 1.7. In addition to `Inverted`, the order of series in the legend is defined by the series' property `Legend.Order` (this does not work if the `Legend.Multiplicity` of the series is `lmStyle`).
  * `LegendStyle = (lsAuto, lsSeries, lsValues, lsLastValues, lsSeriesGroups)`: `lsSeries` draws series titles (the same as `lsAuto`, `lsValues` draws values of first visible series, `lsLastValues` draws the last value of each active series, `lsSeriesGroups`: ???. In TAChart, drawing of series values or series titles can be activated per series by the series property `Legend.Multiplicity`.
  * `MaxNumRows`: 0 means "all rows". Not available in TAChart, but series may be excluded from the legend by setting their `Legend.Visible` to `false`.
  * `Picture`: Background picture of the legend. No picture support in TAChart.
  * `ResizeChart`: Determines whether or not the legend reduces the chart rectangle to prevent overlap between legend and chart rectangles. In TAChart, use `UseSidebar`.
  * `RoundSize: Integer`: Corner radius for rounded rectangle frame (see property `ShapeStyle`). Not available in TAChart.
  * `Shadow`: See `TTeeShadow`. No shadow support in TAChart
  * `ShapeStyle = (fosRectangle, fosRoundRectangle, fosEllipse)`: Shape of the legend. TAChart supports only rectangular shapes for the legend.
  * `Symbol`: Parameters that define how the series symbol is displayed in the legend, see `TLegendSymbol`.
  * `TextStyle = (ltsPlain, ltsLeftValue, ltsRightValue, ltsLeftPercent, ltsRightPercent, ltsXValue, ltsValue, ltsPercent, ltsXAndValue, ltsXAndPercent)`: `ltsPlain` displays point label, `ltsLeftValue` displays point value plus point label, etc, similar to TAChart's series property `Marks.Style`. But TAChart has no independent style of legend and series marks. However, the event handler for the series' `Legend.OnCreate` can be used to define the legend text in any way.
  * `TextSymbolGap`: Distance between text and symbol. Not available in TAChart.
  * `Title`: Text box to show at legend's top. In TAChart, effect can be achieved by means of `Legend.GroupTitles`.
  * `TopPos`: Specifies the Legend's top (or left) position as a percentage of the total chart height (or width). When the `Alignment` property is `laLeft` or `laRight` (vertical legend), the `TopPos` property increments the top legend position by this percentage of the total chart height. For `laTop` or `laBottom` legend alignments (horizontal legend), it increments the legend's left position. Not available in TAChart.
  * `Transparency`: Sets percent transparency of the legend. Not available.
  * `Transparent`: Turns on/off decorating elements liks frame, shadow etc. Property not supported in TAChart, but effect can be achieved by handling elements individually.
  * `Visible`: The same as with TAChart.



#### TLegendSymbol

[![TeeChart TLegendSymbol properties.png](https://wiki.freepascal.org/images/2/2b/TeeChart_TLegendSymbol_properties.png)](</File:TeeChart_TLegendSymbol_properties.png>)

  * `Continuous`: effect unclear. Not available in TAChart.
  * `DefaultPen`: if `DefaultPen = true` the pen of the series is used to draw the line in the symbol (e.g. line segments for TLineSeries, border of bars for TBarSeries, etc). Otherwise the lines are draw as specified by `Pen`. `DefaultPen` is not available in TAChart. LineSeries work as if `DefaultPen = true`, others as if `DefaultPen = false`, the pen parameters are in `SymbolFrame`.
  * `Emboss`: See TTeeEmboss. Not supported in TAChart.
  * `Gradient`: See TTeeGradient. No gradient support in TAChart.
  * `Position = (spLeft, spRight)`: Position of the symbol with respect to the text. Not available.
  * `Shadow`: SeeTTeeShadow: No shadow support.
  * `Squared`: if `true` the `Width` parameter is ignored and the symbol is drawn in a square defined by the lineheight of the legend items. Not available.
  * `Visible`: Shows/hides legend symbols in legend. Not available.
  * `Width, WidthUnits = (lcsPixels, lcsPercent)`: Width of the legend symbol. `SymbolWidth` in TAChart, in Pixels.



## TeeChart's TChartAxis

### Public methods

to be written ... 

### Properties

[![TeeChart TChartAxis properties.png](https://wiki.freepascal.org/images/e/e2/TeeChart_TChartAxis_properties.png)](</File:TeeChart_TChartAxis_properties.png>)

  * `Automatic, AutomaticMaximum, AutomaticMinimum`: Automatic range finding, or if off, use `Maximum` and/or `Minimum` for axis ends. In TAChart, use `Range`.
  * `Axis`: Defines how the axis line is drawn. Its class `TChartAxisPen` has the same properties as `TChartPen`. In TAChart, use `AxisPen`.
  * `AxisValuesFormat`: Format mask for `FormatFloat` that defines the format of the axis labels (default: `#,##0.###`). In TAChart, use `Marks.Format`, default: `%0:.9g`.
  * `DateTimeFormat`: Defines how date/time values are formatted. No direct equivalent in TAChart. Effect can be created by `TDateTimeChartSource` or `TListChartSource`.
  * `EndPosition, StartPosition`: Defines the percentage position of chart width or height where the axis begins and ends. Left and Top are 0,0 positions. No 1:1 equivalent in TAChart, but effect can be achieved by `Range`.
  * `ExactDateTime: boolean`: Useful when placing date/time ticks e.g. at the first of each month such that the tick distance is the number of days per month. Missing in TAChart. Can be achieved with some coding using a `TListChartSource`.
  * `Grid`: Parameters to draw the axis grid. See `TChartPen` for details. TAChart's `Grid` is equivalent, the subproperty `DrawEvery:integer` which allows to skip every other grid line is not directly supported, by may be emulated by using `Axis.Marks.Stripes`.
  * `GridCentered: boolean`: Draws axis grid line between label positions. Not available. You can add a minor axis (`Minors` for the intermediate labels and hide the main labels to get the same effect.
  * `Horizontal: Boolean`: Equivalent to TAChart's `Alignment`.
  * `Increment: Double`: Distance between axis labels. If 0 the distance is determined automatically. In TAChart automatic label positioning is controlled by property `Intervals`.
  * `Inverted`: If `true>` the axis runs into reverse direction. Same as with TAChart.
  * `Labels`: Turns axis labels on and off. In TAChart: `Marks.Visible`
  * `LabelsAlign`: If `alDefault` labels of vertical axes are aligned towards the axis line (left axis: right-aligned), if `alOpposite` they are aligned away from the axis line (left axis: left-aligned). Not supported.
  * `LabelsAlternate`: If true labels for horizontal are offset vertically, one near axis, the next one a line deeper. Not available.
  * `LabelsAngle`: Rotation angle of labels in degrees. Like TAChart's `Marks.LabelFont.Orientation` which is in tenths of degrees, though.
  * `LabelsBehind`: ???
  * `LabelsExponent`: Controls axis labels in exponent format: if true Exponent is in super-script font, else in scientific notation. Not available in TAChart.
  * `LabelsFont`: Font of labels. See `TTeeFont`. Like TAChart's `Mark.LabelFont` which has less subproperties, though.
  * `LabelsMultiLine`: Allows multi-line labels. No direct correspondence in TAChart, but effect can be achieved by event `OnMarkToText`.
  * `LabelsOnAxis`: Controls if labels at axis minimum and maximum positions will be shown or not. Not available in TAChart, effect can be partially achieved using `Marks.Range` property.
  * `LabelsSeparation`: Specifies the percent amount of minimum distance between labels. Zero value causes overlapping labels. Not directly available in TAChart, but effect can be partially achieved by `Intervals` and `OverlapPolicy` settings.
  * `LabelsSize`: Size occupied by labels, default 0 means automatic calculation. If not correct, axis title may overlap with labels. In Lazarus since 1.2.8, in older versions no equivalent, always automatic.
  * `LabelsStyle` = (`talAuto, talNone, talPointValue, talText, talValue`): Like TAChart's `Marks.Style`. No correspondence for `talAuto`, though, which tries to guess the value displayed from the type of axis, e.g. if series has XLabels, then `talText` will be used.
  * `Logarithmic`: Enables logarithmic axis scaling. In TAChart no direct correspondence, but effect can be achieved by means of AxisTransformations and `Axis.Intervals`.
  * `LogarithmicBase`: Base for logarithmic scaling. In TAChart, property is found in the transformation `TLogarithmAxisTransform` needed to achieve logarithmic scaling.
  * `MaximumOffset, MinimumOffset`: Pixels that will be left as a margin at axis maximum or minimum positions. In TAChart: `TChart.Margins.Right` or `Margins.Left`, `MarginsForMarks = true` for automatic calculation.
  * `MaximumRound, MinimumRound`: Rounds axis maximum or minimum to the next integer. Not available in TAChart.
  * `MinorGrid`: Parameters to define minor grid. Use property `Minors[n].Grid`.
  * `MinorTickCount`: Number of minor ticks between two major ticks. Use `Intervals` property of `Minors[n]`.
  * `MinorTickLength`: Length of minor ticks. In TAChart, the property is `Axis.Minors[n].TickLength`; there is also an `TickInnerLength` which TeeChart does not have.
  * `MinorTicks`: Parameter to define minor ticks. Use property `Minors[n]`.
  * `OtherSide`: If `true` axis is drawn on the opposite side of the chart. In TAChart, change the `Alignment` of the axis correspondingly.
  * `PartnerAxis`: ???
  * `PositionPercent`: Percentage of chart width or height the axis is moved into the drawing area. In TAChart, use `Position` and `PositionUnits=cuPercent`.
  * `RoundFirstLabel`: Rounds the first label to a "nice" number (default: `true`). In TAChart, label positioning is determined by `Intervals` subproperties.
  * `TickInnerLength`: Like with TAChart
  * `TickOnLabelsOnly`: From the help file: "... sets the axis ticks and axis grid to be drawn only to coincide at labels. Otherwise they will be drawn at all axis increment positions. When the axis LabelsSeparation property is greater than 0 (default 10), axis increases the increment property to avoid axis label overlap." Seems not be available in TAChart.
  * `Ticks, TicksInner`: Determines drawing of the outer and inner ticks (Color, EndStyle, Mode, Style, Visible, Width, etc). In TAChart only `TickColor`.
  * `Title`: Defines the axis title - see `TChartAxisTitle`. TAChart has the same property but with less sub-properties; the main difference is that TAChart's `Visible` defaults to `false`.
  * `TitleSize`: Distance between title and chart border (?). 0 --> automatic determination. Use `Margin` property in TAChart.
  * `UsePartnerAxis`: No effect in TeeChart Standard. No equivalent in TAChart.



[![TeeChart TChartAxisTitle properties.png](https://wiki.freepascal.org/images/0/04/TeeChart_TChartAxisTitle_properties.png)](</File:TeeChart_TChartAxisTitle_properties.png>)

  * `Visible`: Like in TAChart.
  * `ZPosition`: Like in TAChart.



#### TChartAxisTitle properties

Here is a description of properties that have not yet been explained in `TChartTitle`. 

  * `Angle`: Rotation angle of title, in degrees. In TAChart, use `Title.LabelFont.Orientation` which is in tenths of degrees.
  * `Caption`: Text to be displayed as axis title. Same as in TAChart.
  * `Pen`: Defines parameters that draw the frame around the title, see `TChartPen`. In TAChart, use `Frame`.
  * `RoundSize`: Corner radius of rounded rectangle if `ShapeStyle = fosRoundRectangle`. Not available.
  * `Visible`: Toggles the axis title. Same as in TAChart where the title is off by default.



## TeeChart's TLineSeries

### Public methods

### Properties

[![TeeChart TLineSeries properties.png](https://wiki.freepascal.org/images/9/93/TeeChart_TLineSeries_properties.png)](</File:TeeChart_TLineSeries_properties.png>)

  * `Active`: Shows/hides series, like in TAChart.
  * `Brush`: Used in 3d view to paint the band of the line series. Not available in TAChart.
  * `ClickableLine`: Determines if series accepts mouse clicks over the line segment drawn between points. Not available.
  * `ColorEachLine`: If `ColorEachPoint` is `true`, determines if the connecting lines of a lineseries will also be displayed using a different color for each line segment. In TAChart, this corresponds to the setting `ceLineAfter` or `cePointAndLineAfter` of property `ColorEach`; in addition there are additonal options to colorize the preceding line segment.
  * `ColorEachPoint`: If `false`, all points will be drawn using the `SeriesColor` property. If `true`, each series point will be colored with its corresponding point color. In TAChart, assign colors to each `ChartDataItem` by calling `ListSource.SetColor`. Further control is given by the property `ColorEach` which can be set to colorize the data point only and/or the neighboring line segments (see also Delphi's `ColorEachLine`).
  * `ColorSource`: Used only when connecting a series to a database dataset or datasource. Must be a valid database field name. Not supported.
  * `Cursor`: Shown when the mouse moves sufficiently close to the series. Not suppored in TAChart.
  * `Dark3D`: In 3d view, draws band of lineseries darker and brighter depending on orientation to light source. Not supported.
  * `DataSource`: A TChartSeries can be optionally connected to a "point data provider" (or DataSource) which may be another series, TDataset derived class, or "TTeeSeriesSource". Concept similar to TAChart's chart sources.
  * `Depth`: In 3d view, depth of the band drawn. Like in TAChart.
  * `Emboss`: Upward-directed shadow. See `TTeeEmboss`. Not supported in TAChart.
  * `Gradient`: Gradient drawn across depth of the 3d band, see `TTeeGradient`. Not supported.
  * `HorizAxis = (aTopAxis, aBottomAxis, aBothHorizAxis, aCustomHorizAxis)`: Identifies which horizontal axis is used. Similar to TAChart's `AxisIndexX`.
  * `InvertedStairs`: Alter the direction of the stairs. In TAChart, like `LineType = ltStepYX`.
  * `LegendTitle`: Title to be used in legend, different from default `Title`. Not available in TAChart which normally uses `Title` only, but legend text can be altered from `Title` by event `Legend.OnCreate`.
  * `LineBrush`: Defines the brush style used to fill lineseries band in 3d view. Identical to `Brush.Style`. Not supported in TAChart.
  * `LineHeight`: Thickness of the band drawn in 3d view. Not supported.
  * `LinePen`: Pen parameters used to draw the line series in 2d view. See TChartPen. Like TAChart.
  * `Marks`: Defines all necessary properties to draw a mark near to each series point, see `TSeriesMarks`. Similar to TAChart (which has less options).
  * `Name`: Standard VCL/LCL property
  * `Outline`: Determines an additional pen used to display a "border" or "outline" around a lineseries. Not available.
  * `ParentChart`: Chart to which the series belongs. In TAChart, readonly and public.
  * `PercentFormat`: Standard formatting string specifier used to draw the series marks in percent style. In TAChart, use `Marks.Format`.
  * `Pointer`: Parameters to draw data points, see `TSeriesPointer`. Similar to TAChart which has less options.
  * `SeriesColor`: Color of the series, like in TAChart.
  * `Shadow`: See `TTeeShadow`. No shadow support in TAChart.
  * `ShowInEditor`: Shows/hides series in chart editor. Not available.
  * `ShowInLegend`: Shows/hides series in legend. Use `Legend.Visible` in TAChart.
  * `Stacked = (cssNone, cssOverlap, cssStack, cssStack100)`: Stacking of series. In TAChart, use multi-valued source.
  * `Stairs: boolean`: Connects data paints by line segments (`false`) or stairs (`true`). In TAChart is is controlled by `LineType=ltFromPrevious`, or `LineType = ltStepXY or ltStepYX`, respectively.
  * `Tag`: Standard VCL/LCL property
  * `Title`: Text to identify the series in the legend. Same with TAChart.
  * `Transparency`: Percent transparency of the series. The same in TAChart, but values are between 0 and 255.
  * `TreatNulls`: Not available, but TAChart works like `tnSkip`: 
    * `tnDontPaint`: Line from non-null to null point is displayed, line from null to non-null is not displayed
    * `tnSkip`: Lines between a non-null point and the null point and the next non-null point are not displayed.
    * `tnIgnore`: Treats all points as non-nulls.
  * `ValueFormat`: Standard formatting string specifier used to draw the series Marks.
  * `VertAxis = (aLeftAxis, aRightAxis, aBothVertAxis, aCustomVertAxis)`: Identifies which vertical axis is used. Similar to TAChart's `AxisIndexY`.
  * `XLabelsSource`: Name of a DataSet field that contains the point labels. In TAChart, a DBChartSource is needed.
  * `XValues, YValues`: Provides information on storage of x and y values. In TAChart, use `Source` property for this purpose.



#### TChartValueList

[![TeeChart TChartValueList properties.png](https://wiki.freepascal.org/images/2/2b/TeeChart_TChartValueList_properties.png)](</File:TeeChart_TChartValueList_properties.png>)

TAChart does not provide a TChartValueList property. Here is the reference what this class contains in TeeChart. 

  * `DateTime`: tells TeeChart that data values are numbers (`false`) or date/time values (`true`). Not available in TAChart. Automatic presentatation of date/time values as axis labels can be achieved by assigning a TDateTimeIntervalChartSource to the axis; date/time series mark texts can be obtained by means of the `OnGetMark` event of the series.
  * `Name`: Name of the series, needed when series is used as `ValueSource` of another series. Not needed in TAChart.
  * `Order = (loNone, loAscending, loDescending)`: Sorting order of the data. In TAChart, sorting is achieved by the chart source, in case of the `TListChartSource` commonly used for `TLineSeries` this is the property `Sorted`.
  * `ValueSource`: If series is connected to a database then this is the name of a dataset field. If series is connected to another chart, then this is the name of the connected chart. TAChart does support database-sensitive chartsources, but sharing data with other charts is possible only by linking their chart sources.



#### TSeriesMarks

[![TeeChart TSeriesMarks properties.png](https://wiki.freepascal.org/images/b/be/TeeChart_TSeriesMarks_properties.png)](</File:TeeChart_TSeriesMarks_properties.png>)

  * `Angle`: Rotation angle of marks, in degrees. In TAChart, use `Marks.LabelFont.Orientation` (which is in tenths of degrees).
  * `Arrow`: Parameters for the connecting line between mark and data point. Property name is misleading since an arrow head is not drawn by default (only after setting `Callout.ArrowHead` to a value different from `ahNone`). This property is called `LinkPen`, parameters for an arrow head are available under `Arrow`.
  * `ArrowLength`: Length of the connecting line between mark and data point. Use `Distance` in TAChart.
  * `BackColor`: Background color of the mark. In TAChart, use `LabelBrush.Color`.
  * `Bevel, BevelWidth`: Draws a bevel around the mark (like TPanel). Not available.
  * `Brush`: Parameters to draw the background of the mark. In TAChart, use `LabelBrush` which has less options, though.
  * `CallOut`: Parameters to refine the connection line between mark and data point; you can also set the arrow head here. In TAChart, callout-type of connection can be defined by CallOutAngle which is in tenths of degrees; the arrow head is set by property `Arrow`.
  * `Clip`: Clips mark labels at the edge of the chart areas. Called `Clipped` in TAChart.
  * `Color`: Seems to be the same as `BackColor`. `LinkBrush.Color` in TAChart.
  * `DrawEvery: Integer`: Allows to skip a number of marks to avoid overlap. Not available.
  * `Emboss`: Upward-directed shadow. See `TTeeEmboss`. Not supported.
  * `Font`: Font of the marks text. In TAChart `LabelFont`.
  * `Frame`: Parameters for drawing the frame around the mark, see `TChartPen`. The same as with TAChart.
  * `Gradient`: See `TTeeGradient`. No gradient support in TAChart.
  * `Margins`: Distance between text and frame. The same as with TAChart.
  * `MultiLine`: Enable/disable multilined marks. Not available in TAChart, but works like `true` if the mark text contains `LineEnding`.
  * `Picture`: No support for background picture in TAChart.
  * `Shadow`: See `TTeeShadow`. No shadow support in TAChart.
  * `ShapeStyle = (fosEllipse, fosRectangle, fosRoundRectangle)`: Like TAChart's `Shape` which has more options.
  * `Style = (smsLabel, smsLabelPercent, smsLabelPercentTotal, smsLabelValue, smsLegend, smsPercent, smsPercentRelative, smsPercentTotal, smsPointIndex, smsSeriesTitle, smsValue, smsXValue, smsXY)`: Like TAChart's `Style` with slightly different options.
  * `Symbol`: Adds the series symbol to the mark. Not available.
  * `TextAlign`: Alignment of text within frame rectangle. Is called `Alignment` in TAChart.
  * `Transparency`: Percentage of transparency to see the background underneath the marks. Not available.
  * `Transparent`: Toggles visual enhancements like frame, shadow, emboss. Not available, same effect can be achieved by toggling elements separately.
  * `Visible`: Turns marks on/off. The same as in TAChart.



#### TSeriesPointer

[![TeeChart TSeriesPointer properties.png](https://wiki.freepascal.org/images/9/95/TeeChart_TSeriesPointer_properties.png)](</File:TeeChart_TSeriesPointer_properties.png>)

  * `Brush`: Parameters, how the interior of the symbol is filled. Like in TAChart.
  * `Dark3D`: Shades faces of the extruded shape (if `Draw3D=true`) as if it were illuminated by a light source. Not available.
  * `Depth, Draw3D`: In 3d view, allows to extrude symbols into depth (as specified by `Depth`); only available of `Style = psRectangle`. Not available in TAChart.
  * `Emboss`: Upward-directed shadow, see `TTeeEmboss`. Not supported.
  * `Gradient`: See `TTeeGradient`. No gradient support in TAChart
  * `HorizSize, VertSize`: Size of the data symbol, like in TAChart.
  * `InflateMargins`: Automatically increases the extent of the chart such that the symbols at the edge are not clipped. Not available in TAChart, but can be achieved manually by increasing the chart's `Margins`.
  * `Pen`: Parameters used to draw the outline of the symbol, see `TChartPen`. Like in TAChart.
  * `Shadow`: See `TTeeShadow`. No shadow support in TAChart
  * `Style = (psRectangle, psCircle, psTriangle, psDownTriangle, psCross, psDiagCross, psStar, psDiamond, psSmallDot, psNothing, psLeftTriangle, psRightTriangle, psHexagon)`: Geometrical shape used for the pointer symbol, like in TAChart with slightly different options.
  * `Transparency`: Percentage of transparency which allows to see the background underneath the symbols. Not supported, transparency of the entire series is controlled by the corresponding property of the series.
  * `Visible`: Shows/hides the point symbols. In TAChart, use the property `ShowPoints` of the series.



### Events

## TeeChart's TBarSeries

### Public methods

### Properties

[![TeeChart TBarSeries properties.png](https://wiki.freepascal.org/images/9/94/TeeChart_TBarSeries_properties.png)](</File:TeeChart_TBarSeries_properties.png>)

Only those properties will be discussed here which were not mentioned in TLineSeries. 

  * `AutoBarSize:`: Determines if the series bars calculate their width automatically based on the minimum and maximum bar values. `False` by default, meaning that the bar size is calculated using the number of bar points instead of bar values. Not available.
  * `AutoMarkPosition:`: Controls if marks will be repositioned if there's the possibility of mark overlapping. Marks are displaced to the top of the bars to minimize the overlapping effect of marks with long text or big font sizes. When `false`, no checking is performed and all marks are drawn at their `Mark.ArrowLength` distance to the top of the bar. Does not work perfectly. Not available.
  * `BarBrush`: Defines how the bars are filled. When `BarBrush.Style` is different from `bsSolid`, the `SeriesColor` is the bar _background_ color. Like TAChart, except that `BarBrush.Color` is the same as `SeriesColor`; this is the bar _foreground_ color if `Style <> bsSolid`; there is no way to change the background color.
  * `BarPen`: Pen used to draw the bar border, see `TChartPen`. Like TAChart. The `Visible` property of TeeChart is not supported, but can be emulated by setting `Style = psClear`.
  * `BarRound = (brNone, brAtValue, btBothEnds)`: Determines which end of the bar is rounded if `BarStyle = brRoundRectangle`. Not available.
  * `BarStyle = (bsRectangle, bsRoundRectangle, bsPyramid, bsInvPyramid, bsCilinder, bsEllipse, bsArrow, bsRectGradient, bsCone, bsBevel, bsSlantCube, bsDiamond, bsInvArrow, bsInvCone, bsCustom)`: Shape used to draw bars. `bsSlantCube` is mostly useful only in 3D where it corresponds to `bsRectangle`, but is rotated around the vertical axis. Not available, TAChart only draws rectangles.
  * `BarWidth` (runtime and read-only): Indicates bar width in pixels (in case of `THorizBarSeries` the corresponding property is `BarHeight`). In TAChart use the function `GetBarWidth(BarIndex:Integer)`.
  * `BarWidthPercent`: Determines the bar width expressed as percentage of the space available per bar (`BarWidthPercent = 100`: joined bars; `BarWidthPercent = 50`: bar width equal to spacing). Similar to TAChart which, however, allows to control bar width also by `BarWidthStyle`.
  * `BevelSize`: Pixels of bar bevel, used only when `BarStyle` property is `bsBevel` (only in 2D and 3D orthogonal display modes). Not available in TAChart.
  * `CustomBarWidth`: Determines the fixed bar width in pixels. Not supported.
  * `DepthPercent`: Defines the percentage the bars will need in the _z_ direction (into the screen); for 3D only. Not available.
  * `Gradient, GradientRelative`: Gradient parameters for `BarStyle = bsRectGradient`, see `TTeeGradient`. If `GradientRelative = true` the gradient of the longest bar is applied to all other bars, if `false` all bars show the full gradient independent of their length. No gradient support in TAChart.
  * `MarksLocation = (mlStart, mlCenter, mlEnd), MarksOnBar`: if `MarksOnBar = true`, `MarksLocation` controls the position of the marks at the bottom, center, top of the bars (inside). `YOrigin` is ignored for marks position in case of `mlStart`. If `MarksOnBar = false`, marks are always above the top. TAChart has a similar property `MarkPositions` which behaves slightly different. `mlCenter` is not supported.
  * `MultiBar = (mbNone, mbSide, mbStacked, mbStacked100, mbSideAll, mbSelfStack)`: If there are more than one `TBarSeries` in the same chart, the property determines if they will be drawn side-by-side, back-to-front or stacked. `mbSideAll` means that all bars of the first group are drawn next to each other, then those of the second group, etc. `mbSelfStack` is effectively like `mbSideAll` except that the bars are stacked. Property not available in TAChart, but some multi-bar effects (`mbSide, mbStacked, mbStacked100`) can be achieved by multi-valued series and `TChartStyles`.
  * `OffsetPercent`: Indicates the bar displacement in percentage of bar size. Can be used to create "overlayed" bar charts. In TAChart: `BarOffsetPercent`.
  * `SideMargins: boolean`: Controls if first and last displayed bar will be separated from the chart rectangle. Property not supported in TAChart, but you can use `Chart.Margins` to get the same effect.
  * `StackGroup: Integer`: Index which groups series when applying the `MultiBar` property. In TAChart a multi-bar chart is created from a single series using a multi-valued source and `TChartStyles`, instead of multiple series grouped by `StackGroup`.
  * `TickLines`: Pen used to display lines in front of bars at the height of each y axis grid line. See `TChartPen`. Not available in TAChart.
  * `UseYOrigin, YOrigin`: `YOrigin` determines the axis value used as a common bottom for all bars. `UseYOrigin` must be `true` (the default) to use `YOrigin`, otherwise `YOrigin` is effectively 0. Bars with a value bigger than `YOrigin` are drawn in one direction and bars with a lower value are drawn in the opposite direction. In TAChart: use `ZeroLevel` instead of `YOrigin`; `UseYOrigin` is not available, but works like `true`.



## TeeChart's THorizBarSeries

TAChart does not support this series type. But it can be easily derived from TBarSeries by interchanging its properties `AxisIndexX` and `AxisIndexY`.

---

_Source: [https://wiki.freepascal.org/Comparing_TAChart_with_Delphi%27s_TeeChart_Standard](https://web.archive.org/web/20250122190457/https://wiki.freepascal.org/Comparing_TAChart_with_Delphi%27s_TeeChart_Standard)_
