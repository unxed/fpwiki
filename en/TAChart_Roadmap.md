# TAChart Roadmap

Below is a list of items that would be good to have in [TAChart](<TAChart.md> "TAChart"). Some of them are planned for implementation by the current maintainer, others will require patches from interested contributors. 

## Contents

  * 1 Editors
  * 2 Axises/Transformations
  * 3 Series
  * 4 Legend
  * 5 Tools
  * 6 Optimization
  * 7 Sources
  * 8 Labels
  * 9 Presentation
  * 10 Drawers
  * 11 Chart
  * 12 Demos
  * 13 Other
  * 14 Internal



#### Editors

  * TChartExtent editor
  * series index list editor
  * ListSource.DataPoints editor: intergate with YCount
  * marks.format/style editor
  * margins editor
  * subcomponent editor 
    * add shortcuts, use actions
    * convert to grid/checklist, show enabled/active checkboxes



#### Axises/Transformations

  * finish multiaxis 
    * auto-scaling to percent range
    * partial-linear/local zoom transform (?)
    * inverted as transformation (?)
    * logBase 0 => e ?
  * non-decartes axises + XY transform (ternary, polar...)
  * various axis marks steps -- logarithmic etc.
  * fixed start/step interval source (?)
  * use Intervals.Count in custom source
  * merge Intervals of axis and source (?) 
    * advanced axis step controls -- targetStepSize(Pixels/Graph/Axis)...
  * test/fix inverted axis
  * axis.autoscale as a shortcut(?)
  * axis.DateTimeFormat(?)
  * axis.Depth
  * walls
  * YYYMMDD -> datetime transform(?)
  * LineRange(?)
  * arrows: fill color, pixel-symmetrical
  * fixed size/synchronize axis size between linked charts
  * "axis breaks" -- transformation?
  * Cartesian Distortion: <http://bost.ocks.org/mike/fisheye/#cartesian>



#### Series

  * 3-d series: ~~bubble, box-and-whiskers(?), OHLC(?),~~ func
  * ~~fix 3-d pie series with exploded slices~~
  * bar: ~~pyramid, code, cylinder, prism,~~ number of faces
  * bug: all-negative area has no zero plane
  * top-down and right-to-left series
  * Z axis, XZ and YZ grids
  * polar axis
  * ~~polar series: labels, brush, pointers~~
  * TConstantLine.Source -- use X only
  * 3-d surface
  * color map: 
    * isolines & labels,
    * gradients,
    * data interpolation, auto-scale colors (thresholds in percent)
    * better design-time display,
    * transparency threshold (?) / ColorSource.Y as opacity (?)
  * ~~range bar series~~
  * Gantt series (bar.starty, bar.startx, bar.endx)
  * stacked~~/donut~~ pie series (?)
  * ~~test/fix rotated stacked series~~
  * subcharts
  * GIS
  * func series: discontinuity pointers
  * slopegraphs, sparklines (?)
  * ~~arrow series~~
  * use Hare-Niemer integer compensation to make sure that percents always add up to 100% (??)
  * pie chart slice grouping: MinSliceSize (?)
  * ternary charts (?)
  * measurement series
  * support infinity in more series
  * ~~polygon series~~



#### Legend

  * ColumnPerGroup
  * legend line per stack level
  * legendpanel.autosize -> scrollable with scrollbox
  * different widths for columns
  * arbitrary position
  * ~~support clicks on series items~~
  * support FontSeriesColor



#### Tools

  * ~~test/fix many charts per toolset~~
  * ~~remove old reticule~~
  * xorpen/xortext in all drawers
  * label editor/notes
  * ~~axis tools~~
  * ~~legend click tool~~
  * ~~title/footer click tool~~



#### Optimization

  * caches: graphpoints, imagepoints, layers/barriers
  * image cache for tools



#### Sources

  * OO (incompatible?)
  * YList in Marks.Format
  * calculated sources: optimization, sliding median, user-defined filter
  * derivative: ~~Savitzky-Golay~~ , non-equidistant X
  * bug: db-source does not auto-refresh after dataset changes (?)
  * ~~support NaN in all series: func, ...~~
  * TColorSource (?)
  * more animations



#### Labels

  * fix labels with NaNs
  * margins
  * more frame shapes: rhumb, hexagon, octagon... 
    * control shape corner size
  * fix/improve pie series label positioning 
    * "sideband" positioning
    * special label attachments: "center", "side center", "nearest point"
  * auto-position labels: two-level, generic, GA
  * free labels (notes)
  * control label attachment (top, bottom, center, ...)
  * Styles stored false
  * measure non-symmetrical labels correctly



#### Presentation

  * embellishments: gradient, shadow, glow(?) 
    * color->embellishment mapping(?)
  * Generic renderer (?): OpenGL, SVG, true 3-d
  * TCanvas-independent API: TChartPen, TChartBrush, TChartFont
  * panes (??)
  * styles: apply to more series, usecolor, perstack/perpoint 
    * embelishments from styles ?



#### Drawers

  * transparency in all drawers (print, wmf, ...)
  * monochrome in all drawers
  * putImage in all drawers
  * extract PrepareSimplePen



#### Chart

  * DepthX/DepthY ...or... CameraVector
  * navigation: "axis scrollbar", CurrentExtentPen, limit navscrollbar to the extent
  * measuring units: pixels, graph, percent



#### Demos

  * crosshair page in tools demo
  * zerolevel (?)
  * ~~from tutorials~~



#### Other

  * warnings (incompatible axes in group, ...)
  * more documentation
  * more tests
  * better icons
  * lazreport integration
  * more tutorials
  * wizards
  * shapes
  * LazLogger
  * better teechart compatibility



#### Internal

  * AddSubcomponent => SetParentComponent
  * extract TSubcomponentClassRegistry
  * ~~LazFreeType in nogui (or even OpenGL)~~
  * bresenham in TesselateRadialPie
  * advanced notification

---

_Source: [https://wiki.freepascal.org/TAChart_Roadmap](https://web.archive.org/web/20250121212828/https://wiki.freepascal.org/TAChart_Roadmap)_
