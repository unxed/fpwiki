# TsWorksheetChartSource

│ **English (en)** │  **[français (fr)](</TsWorksheetChartSource/fr> "TsWorksheetChartSource/fr")** │    
****

## Contents

  * 1 Overview
  * 2 Usage
  * 3 Example
  * 4 See also



## Overview

TsWorksheetChartSource is a charting component which is available in the source code of the [FPSpreadsheet](<FPSpreadsheet.md> "FPSpreadsheet") library. It is a chart data source component designed to work with the TChart component from the [TAChartLazarusPkg package](<TAChart.md> "TAChart"), which usually comes pre-installed with Lazarus. 

## Usage

At the moment TsWorksheetChartSource has the following properties to control its behavior: 
    
    
      published
        property PointsNumber: Integer read FPointsNumber write SetPointsNumber default 0;
        property XFirstCellCol: Integer read FXFirstCellCol write SetXFirstCellCol default 0;
        property XFirstCellRow: Integer read FXFirstCellRow write SetXFirstCellRow default 0;
        property YFirstCellCol: Integer read FYFirstCellCol write SetYFirstCellCol default 0;
        property YFirstCellRow: Integer read FYFirstCellRow write SetYFirstCellRow default 0;
        property XSelectionDirection: TsSelectionDirection read FXSelectionDirection write SetXSelectionDirection;
        property YSelectionDirection: TsSelectionDirection read FYSelectionDirection write SetYSelectionDirection;
      end;
    

The Chart will display an amount of PointsNumber points, being that on the coordinates of the X-Axis will be collected from the worksheet starting in the position (XFirstCellRow, XFirstCellCol). Subsequent X-Axis values will be taken from the cells directly to the right of the first cell, if XSelectionDirection has value fpsHorizontalSelection, or below the first cell, if XSelectionDirection has value fpsVerticalSelection. The values will be taken in sequence so if another layout of the data exists, such as each second cell or going up, it can be created by copying the relevant data to a new TsWorksheet. 

The same is valid for the Y-Axis value, but applies to the properties YFirstCellCol, YFirstCellRow and YSelectionDirection. The property PointsNumber is valid for both axis. 

Be careful that if a value could not be read from the worksheet it's value will be considered zero. 

The values for Worksheet coordinates use the same values as the FPSpreadsheet API, that is, they are zero-based. So, for example, to select the cell B2 as the starting position for X-Axis values the following values should be used: XFirstCellCol = 1 and XFirstCellRow = 1 

## Example

A simple example is present in the fpspreadsheet svn repository, located at fpspreadsheet/examples/fpchart/fpchart.lpi 

Please see the image below to see how the components are connected at design time. The TChart component can be edited by double-clicking. Then use the menus of the editor to add a new line series and connect this line series to our WorksheetChartSource by setting its Source property. 

[![Fpschart design.png](https://wiki.freepascal.org/images/5/5e/Fpschart_design.png)](</File:Fpschart_design.png>)

To load the data from the grid and into the chart, a button was created with the following code. Notice that we can't just use any Grid, it has to be an FPSpreadsheet Grid, which has the class TsWorksheetGrid: 
    
    
    type
      
      { TFPSChartForm }
    
      TFPSChartForm = class(TForm)
        btnCreateGraphic: TButton;
        MyChart: TChart;
        FPSChartSource: TsWorksheetChartSource;
        MyChartLineSeries: TLineSeries;
        WorksheetGrid: TsWorksheetGrid;
        procedure btnCreateGraphicClick(Sender: TObject);
      private
        { private declarations }
      public
        { public declarations }
      end; 
    
    ...
    
    procedure TFPSChartForm.btnCreateGraphicClick(Sender: TObject);
    begin
      FPSChartSource.LoadFromWorksheetGrid(WorksheetGrid);
    end;
    

And finally we can see how this works nicely in a running application in the picture below: 

[![Fpschart run.png](https://wiki.freepascal.org/images/2/2f/Fpschart_run.png)](</File:Fpschart_run.png>)

## See also

  * [FPSpreadsheet](<FPSpreadsheet.md> "FPSpreadsheet")

---

_Source: [https://wiki.freepascal.org/TsWorksheetChartSource](https://web.archive.org/web/20250418102002/https://wiki.freepascal.org/TsWorksheetChartSource)_
