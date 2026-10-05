# TsWorksheetGrid

│ **English (en)** │  **[русский (ru)](<../ru/TsWorksheetGrid.md> "TsWorksheetGrid/ru")** │    
****

## Contents

  * 1 Overview
  * 2 Programming interface
    * 2.1 Access to cell values
      * 2.1.1 Writing of cell values
      * 2.1.2 Reading of cell values
      * 2.1.3 Comparing cell values
    * 2.2 Formulas in cells
    * 2.3 Cell formatting
    * 2.4 Cell comments
    * 2.5 Hyperlinks in cells
    * 2.6 Merged cells
    * 2.7 Column widths and row heights
    * 2.8 Worksheet and cell protection
    * 2.9 New grid properties
    * 2.10 Recommended property values
  * 3 See also



## Overview

`TsWorksheetGrid` is a specialized grid component which interfaces to a `TsWorksheet` from the [FPSpreadsheet](<FPSpreadsheet.md> "FPSpreadsheet") library and displays spreadsheet data files like in a conventional [`TStringGrid`](<Grids_Reference_Page.md> "Grids Reference Page"). 

[![fpsgrid.png](https://wiki.freepascal.org/images/0/02/fpsgrid.png)](</File:fpsgrid.png>)

## Programming interface

`TsWorksheetGrid` inherits from TCustomGrid and behaves much like a standard [`TStringGrid`](<Grids_Reference_Page.md> "Grids Reference Page"). The main difference is that the grid itself does not store data, but data are stored in a `TWorksheet` of [FPSpreadsheet](<FPSpreadsheet.md> "FPSpreadsheet"). The worksheet can be accessed by using the property `Worksheet` of the grid. Similarly, `Workbook` allows to access the entire workbook to which the worksheet belongs. 

A variety of properties are available to access cells, their values and attributes, in a StringGrid-like way, such as writing a cell value by means of `Grid.Cells[ACol, ARow] := 1.234`. The cells are identified in these properties by means of the cell's column and row indexes. Please note that the indexes are passed in the order "column first / row last", and they include the indexes occupied by the fixed cells, i.e. the top/left data cell has the indexes col=1 and row=1. This is different from fpspreadsheat where the indexes always start at 0 and are passed to functions in the opposite order "row first / column last". 

### Access to cell values

The property `Cells[ACol, ARow]` provides access to the data in a cell given by its column and row indexes. This is similar to `TStringGrid`. Unlike `TStringGrid` which works with strings only, `TsWorksheetGrid`, however, accepts numbers, dates, booleans, and formulas as well. For this reason, the data type of `Cells` is a `variant`. 

#### Writing of cell values

Use this code to write data to a cell in a `TsWorksheetGrid` named `Grid` for simplicity: 
    
    
      // Write a string
      Grid.Cells[1, 1] := 'This is a string';  
      // Write a string with embedded HTML codes
      Grid.Cells[1, 2] := 'Chemical formula of <b>water</b>: H<sub>2</sub>O';
      // Write a number
      Grid.Cells[1, 3] := 1.2345;
      // Write a date
      Grid.Cells[1, 4] := EncodeDate(2016, 1, 18);
      // Write a formula
      Grid.Cells[1, 5] := '=A3+2';
      // etc.
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** A text assigned to a cell can contain embedded simple HTML codes to change the font for particular words or characters.

#### Reading of cell values

In the same way, cell values can be read into variables of decidated types: 
    
    
    var
      str: String;
      x: Double;
    begin
      str := Grid.Cells[1, 1];  // Variable "str" will contain the value "This is a string"
      x := Grid.Cells[1, 2];    // x will be 1.2345
      str := Grid.Cells[1, 2];  // Although the cell contains a number it is converted to the string "1.2345"
    end;
    

#### Comparing cell values

Since the Cells property is a variant it is a bit more difficult to compare string values. Use the string-to-variant conversion function `VarToStr()` provided by unit `variants`. 
    
    
      // This straightforward comparison will fail:
      // if Grid.Cells[1,1] = 'This is a string' then ...
    
      // Use this instead:
      if VarToStr(Grid.Cells[1,1]) = 'This is a string' then ...
    

Numerical values often can be compared without an explicit conversion: 
    
    
      if Grid.Cells[1,2] = 1.2345 then ...
    

### Formulas in cells

Since the `TsWorksheetGreed` works on top of a spreadsheet all formulas supported by [FPSpreadsheet](<FPSpreadsheet.md> "FPSpreadsheet") can be entered. See [this document](<FPSpreadsheet__List_of_formulas.md> "FPSpreadsheet: List of formulas") for a complete list of supported formulas. 

The formula string must begin with the character "=". Cell references must follow Excel's "A1" notation, i.e. column index must be characters "A".."Z" , and row index is a 1-based integer. If there are more than 26 columns then two (or three) characters can be used. 

In case of a cell range, the coordinates of the top-left and bottom-right corners of the cell rectangle must be separated by a colon, e.g. '=SUM(A1:C10)' calculates the sum of the values in the cell rectangle extending from A1 to C10. 

Formulas referring to cells in other sheets can be used by following the Excel syntax: specify the sheet name followed by a "!" in front of the cell addresses, e.g. '=Sheet3!A1' or '=SUM(Sheet3:Sheet4!A1:C10)'. 

Note that formulas are not automatically calculated by default. In order to activate automatic calculation of formulas set the grid property `AutoCalc` to true, or set the option `boAutoCalc` of the workbook used by the grid. "Automatic calculation" means that all formulas in the entire worksheet are recalculated whenever the content of any cell changes. Therefore, it is not required that the cells addressed by the formula already have values when the formula is entered. 
    
    
    // Enable automatic calculation of formulas
    Grid.AutoCalc := true;        
    // or: Grid.Workbook.Options := Grid.Workbook.Options + [boAutoCalc];
    
    // Enter cells
    Grid.Cells[1,1] := 1;         // this is cell A1
    Grid.Cells[1,2] := 2;         // this is cell A2
    
    // Enter formula
    Grid.Cells[1,3] := '=A1+A2';  // Calculates the sum of the values in A1 and A2 
    
    // Another formula in Cell B3
    Grid.Cells[2,3] := '=B1*B2';  // It does not matter that the cells B1 and B2 do not exist, yet.
    
    // Enter cells needed by the formula
    Grid.Cells[2,1] := '=A3';     // Use the result of the previous formula
    Grid.Cells[2,2] := 10;
    

### Cell formatting

Cell attributes can be attached to each cell in a similar way as the cell values. There is a set of grid properties representing each attribute: 

  * `BackgroundColor[ACol, ARow: Integer]: TsColor` \- specifies the background color of the cell. `TsColor` is an integer containing the rgb components of the color. Unit `fpsTypes` provides a list of constants for a large number of predefined colors; the colors defined by the standard unit `Graphics` can be used as well (except for the system color values).
  * `CellBorder[ACol, ARow: Integer]: TsCellBorders` \- specifies which cell edges will be decorated by a border. Use the set values `[cbEast, cbWest, cbNorth, cbSouth]` for the right, left, top and border edges, respectively.
  * `CellBorderStyle[ACol, ARow: Integer; ABorder: TsCellBorder]: TsCellBorderStyle` \- specifies the style to be used for the cell border line given in the parameter `ABorder`. The `TsCellBorderStyle` is a record containing information on the line style and line color. Note that the set `CellBorder[ACol, ARow]` must contain the element `ABorder` in order to activate this border line.
  * `CellFont[ACol, ARow: Integer]: TFont` \- describes the font used when painting the cell text. Elements of the font can be changed separately by these properties: 
    * `CellFontColor[ACol, ARow: Integer]: TsColor` \- identifies the text color. See "BackgroundColor" above for a description of the TsColor type.
    * `CellFontName[ACol, ARow: Integer]: String` \- is the name of the font.
    * `CellFontSize[ACol, ARow: Integer]: Single` \- is the point size of the font (1 pt = 1/72 inch).
    * `CellFontStyle[ACol, ARow: Integer]: TsFontStyles` \- is a set containing elements for using a bold, italic, underlined, or striked-out font.
  * `HorAlignment[ACol, ARow: Integer]: TsHorAlignment` \- allows to modify the horizontal alignment of the cell text (`haLeft`, `haCenter`, or `haRight`).
  * `NumberFormat[ACol, ARow: Integer]: String` \- is an Excel-compatible number format string, e.g. `'0.000'` for displaying a number value with 3 decimal places. The number format is important if numbers are to be displayed as date or time values.
  * `TextRotation[ACol, ARow: Integer]: TsTextRotation` \- must be used for rotating the text within the cell. The type `TsTextRotation` provides `trHorizontal`, `rt90DegreeClockwiseRotation`, `rt90DegreeCounterClockwiseRotation`, and `rtStacked`.
  * `VertAlignment[ACol, ARow: Integer]: TsVertAlignment` \- allows to modify the horizontal alignment of the cell text (`vaTop`, `vaCenter`, or `vaBottom`).
  * `Wordwrap[ACol, ARow: Integer]: Boolean` \- activates word-wrapping of text which is longer than the width of a cell (or the height if rotated text is used).



These properties can also be accessed for a range of cells specified by the indexes of the left column, top row, right column and bottom row of the cell block. Since these properties are related to several cells they are spelled in plural form (with appended "s"): 

  * `BackgroundColors[ALeft, ATop, ARight, ABottom: Integer]: TsColor`
  * `CellBorders[ALeft, ATop, ARight, ABottom: Integer]: TsCellBorders`
  * `CellBorderStyles[ALeft, ATop, ARight, ABottom: Integer; ABorder: TsCellBorder]: TsCellBorderStyle`
  * `CellFonts[ALeft, ATop, ARight, ABottom: Integer]: TFont`
  * `CellFontColors[ALeft, ATop, ARight, ABottom: Integer]: TsColor`
  * `CellFontNames[ALeft, ATop, ARight, ABottom: Integer]: String`
  * `CellFontStyles[ALeft, ATop, ARight, ABottom: Integer]: TsFontStyles`
  * `CellFontSizes[ALeft, ATop, ARight, ABottom: Integer]: Single`
  * `HorAlignments[ALeft, ATop, ARight, ABottom: Integer]: TsHorAlignment`
  * `NumberFormats[ALeft, ATop, ARight, ABottom: Integer]: String`
  * `TextRotations[ALeft, ATop, ARight, ABottom: Integer]: TsTextRotation`
  * `VertAlignments[ALeft, ATop, ARight, ABottom: Integer]: TsVertAlignment`
  * `Wordwraps[ALeft, ATop, ARight, ABottom: Integer]: Boolean`



If properties to be read do not have identical values within the cell block then a neutral or default value is returned. 

  
**Example**

This example adds a formula for today's date to cell A1, formats the cells to display the number as a date and selects a white, italic font on lightgray background. A red dotted border is drawn around the cell. 
    
    
    const
      RED_DOTTED_BORDER: TsBorderStyle = (LineStyle: lsDotted; Color: scRed);
    
    // Set cell content
    Grid.Cells[1,1] := '=TODAY()';
    
    // Format as date
    Grid.Numberformat[1,1] := 'yyyy/mm/dd';
    
    // Select background color
    Grid.BackgroundColor[1,1] := scSilver;
    
    // Select format 
    Grid.FontColor[1,1] := scWhite;
    Grid.FontStyle[1,1] := [fssItalic];
    
    // Activate cell borders 
    Grid.Border[1,1] := [cbEast, cbWest, cbNorth, cbSouth];
    
    // Determine how cell borders will be drawn
    Grid.BorderStyle[1,1, cbEast] := RED_DOTTED_BORDER;
    Grid.BorderStyle[1,1, cbWest] := RED_DOTTED_BORDER;
    Grid.BorderStyle[1,1, cbNorth] := RED_DOTTED_BORDER;
    Grid.BorderStyle[1,1, cbSouth] := RED_DOTTED_BORDER;
    

Defining cell borders for a larger cell block by using these properties is a bit cumbersome because different styles may have to be used for the corder, border and inner cells. To simplyfy this task, the grid provides a method `ShowCellBorders` which gets the coordinates of the cell block and the styles of the left, top, right, bottom outer, and horizontal and vertical inner border lines. Use the constant `NO_CELL_BORDER` of no border line should be drawn at the specific location: 
    
    
    const
      THICK_CELL_BORDER: TsCellBorderStyle = (LineStyle: lsThick; Color: clBlack);
      DOTTED_CELL_BORDER: TsCellBorderStyle = (LineStyle: lsDotted; Color: clSilver);
    
    // Draw a thick border around the block A1:C3, no inner border
    Grid.ShowCellBorders(1,1, 3,3,      // left, top, right, bottom coordinates of cell block
      THICK_CELL_BORDER, THICK_CELL_BORDER, THICK_CELL_BORDER, THICK_CELL_BORDER,   // outer border styles
      NO_CELL_BORDER, NO_CELL_BORDER                                                // inner border styles
    );
    
    // Draw a thick border around the block A1:C3, dotted gray horizontal inner lines
    Grid.SetCellBorders(1,1, 3,3, 
      THICK_CELL_BORDER, THICK_CELL_BORDER, THICK_CELL_BORDER, THICK_CELL_BORDER, 
      DOTTED_CELL_BORDER, NO_CELL_BORDER
    );
    

### Cell comments

Comments can be added to each cell by using the grid's property `CellComment[ACol, ARow: Integer]: String`. Cells containing a comment are marked with a red triangle in the upper right corner of a cell. If the mouse is moved into a cell with a comment a popup window appears to display the comment. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** For the popup window to show up it is required to add the flag `goCellHints` to the grid's `Options`, and the standard grid property `ShowHint` must be true. Otherwise the popup windows will not appear.

**Example:**
    
    
    Grid.Cells[1,1] := '=pi()';
    Grid.CellComment[1,1] := 'The number pi is needed to calculate the area and circumference of a circle.';
    

### Hyperlinks in cells

Cells with attached hyperlinks allow the user to navigate to other cells or other documents by clicking on the cell. Hyperlinks can be accessed by using the property `Hyperlink[ACol, ARow: Integer: String`. The hyperlink string contains the hyperlink target and an optional tooltip text which is separated by means of a bar character ("|"). Internal targets are already handled by the grid, but for navitation to the hyperlink an event handler for `OnClickHyperlink` must be provided. To distinguish normal cell clicks from hyperlink clicks the mouse must be held down for fractions of a second before the hyperlink is executed. 

**Example:**
    
    
    // Example for adding an external hyperlink
    Grid.Cells[1,1] := 'Lazarus';
    Grid.Hyperlink[1,1] := 'www.lazarus-ide.org|Open Lazarus web site';
    
    // Example for adding an internal hyperlink
    Grid.Cells[2,2] := 'Summary';
    Grid.Hyperlink[2,2] := '#Sheet2!B10|Go to the summary starting at cell B10 of sheet2';
    
    // Example for an OnClickHyperlink event handler needed for external hyperlinks
    uses
      ..., uriparser;
    
    procedure TForm1.GridOnClickHyperlink(Sender: TObject; const AHyperlink: TsHyperlink);
    begin
    var
      uri: TUri;
    begin
      uri := ParseURI(AHyperlink.Target);
      case Lowercase(uri.Protocol) of
        'http', 'https', 'ftp', 'mailto', 'file':
          OpenUrl(AHyperlink.Target);
        else
          ShowMessage('Hyperlink ' + AHyperlink.Target + ' clicked');
      end;
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Follow the instructions above to show the tooltip text as a popup window. 

### Merged cells

A rectangular group of cells can be merged to a single block. The content of the top/left cell of this block is displayed across all the combined cells, the content of the other cells is ignored. 

Use the method `MergeCells` to perform this operation. As parameters, it requires the left, top, right and bottom coordinates of the cell range to be merged. They also can be combined into a a `TRect` record. The method `Unmerge` splits a previously merged block again in the individual cells, here the coordinates of a single cell from the merged range is sufficient. 
    
    
      // Example for merging
      Grid.MergeCells(1,1, 3,1);          // Combine the first 3 cells of the first row
      Grid.Cells[1,1] := 'Summary';       // Write the text "Summary" across the 3 cells
      Grid.HorAlignment[1,1] := haCenter; // and center it.
    
      // Example for unmerging
      Grid.UnmergeCells(1,1);             // Splits the merged block (1,1..3,1)
    

### Column widths and row heights

Column widths and row heights can be changed for all cells by setting the `DefaultColWidth` and `DefaultRowHeight`. This works also in design mode. In addition, the widths and heights of particular columns and rows can be modified by the properties `ColWidths[ACol]` and `RowHeights[ARow]`. These values must be given in pixels. 
    
    
      Grid.DefaultColWidth := 80;  // This is the width of all columns 
      Grid.ColWidths[10] := 10;    // except for column #10 which is only 10 pixels wide.
    
      Grid.DefaultRowHeight := 20; // All rows are 20 pixels high
      Grid.RowHeights[2] := 4;     // Row #2 serves as a spacer and is only 4 pixels high.
    

### Worksheet and cell protection

The worksheet grid supports protection at the worksheet and cell level. This way, for example, an entire worksheet can be protected from changes by the user except for some specific cells. For this purpose you must follow the same inverted logic used also by Excel and LibreOffice Calc: Protect the entire worksheet, and then remove the protection from the writeable cells. In the following code snipped editing is allowed only in cells A1 and C2:C5: 
    
    
      // Protect the worksheet
      Grid.Worksheet.Protection := DEFAULT_SHEET_PROTECTION;
      Grid.Worksheet.Protect(Checkbox1.Checked);
    
      // Unprotect the writeable cells A1 and C2:C5
      Grid.CellProtection[1, 1] := []; 
      Grid.CellProtections[3, 2, 3, 5] := [];
    

For further details on all protection options see the main [FPSpreadsheet documentation](<FPSpreadsheet.md> "FPSpreadsheet"). 

### New grid properties

In addition to the properties inherited from its ancestors `TCustomDrawGrid` and `TCustomGrid`, the `TsWorksheetGrid` introduces the following new **published properties** : 

  * **AllowDragAndDrop** (boolean): Cells can be dragged to a new location if this option is active. Move the mouse cursor to the border of the cells to be dragged until the mouse cursor becomes a four-sided arrow, and then begin dragging. If the dragged cell is referenced by a formula the formula is NOT updated - this behavior is different from Excel but agrees with Open/LibreOffice Calc.
  * **AutoCalc** (boolean): Formulas in the grid are automatically recalculated whenever cell content changes.
  * **AutoExpand** (set of `aeData, aeNavigation, aeDefault`): 
    * If the option `aeData` is contained in the set `AutoExpand` then the grid automatically expands if cells are written outside the predefined range.
    * If `AutoExpand` contains the option `aeNavigation` then the user can _navigate_ outside the predefined cell range; new rows and columns are automatically added to the grid (but not to the underlying worksheet). If a file is loaded into the grid then grid dimensions are automatically expanded to the range needed for the worksheet.
    * The option `aeDefault` comes into play if a grid is smaller than the DEFAULT_COL_COUNT and DEFAULT_ROW_COUNT default values. If this option is included the grid is automatically expanded to this default size. On the other hand, if ony a given number of rows and columns should be contained in the grid the option `aeDefault` must be **removed before** setting `RowCount`/`ColCount`.
  * **EditorLineMode** determines whether the grid's cell editor supports only single lines (`elmSingleLine`) or multiple lines (`elmMultiLine`). In the latter case, you can press CTRL+ENTER in order to begin a new line during editing a cell.
  * **FrozenBorderPen** : if the grid has frozen panes (see `FrozenCols` and `FrozenRows`) a separating line is drawn out the edge of the last frozen row and column. The property `FrozenBorderPen` determines how this line is drawn. Set `FrozenBorderPen.Style = psClear` to hide these lines.
  * **FrozenCols** and **FrozenRows** (integer): determines the number of non-scrolling columns (rows) at the left (top) of the grid. Technically these are custom-drawn fixed columns (rows) of the ancestor. Note that the user cannot navigate or edit cells within this range.
  * **ReadFormulas** (boolean): Reads formulas from the input files. Since fpspreadsheet does not support all formulas available in the Office spreadsheet applications there is a chance that reading of a file may crash due to formulas; in this case, reading of formulas can be disabled.
  * **SelectionPen** (TPen): determines how the border of the selected cell is painted. By default, the selected cell is outlined by a 3-pixel-wide black line.
  * **ShowGridLines** (boolean): allows to turn off the grid lines
  * **ShowHeaders** (boolean): can be used to turn off the column and row headers ('A', 'B', 'C', ..., '1', '2', '3'). The property **DisplayFixedColRow** has the same effect, but is deprecated now.
  * **TextOverflow** (boolean): If this property is on then long text content is allowed to extend into adjacent cells if these are empty. Note that numerical data cells are rounded such that the cell does not overflow.
  * **WorkbookSource** (TsWorkbookSource): links to the workbook source which provides the data. If empty, the grid uses an internal workbook source.



**Events**

  * **OnClickHyperlink** : This event fires whenever the user clicks a cell with an embedded hyperlink. Since clicking a cell normally would bring a cell into edit mode it is necessary to hold the mouse key down for about half a second to trigger the hyperlink event.



### Recommended property values

In order to set up the grid to behave similar to the well-known Office applications we recommend the following grid property settings. It should be emphasized, though, that differences in usage do exist and cannot be removed without a major re-write of the inherited grid infrastructure: 

  * `AutoAdvance = aaDown`: The ENTER key advances the selected cell to the next lower cell.
  * `AutoCalc = true`: Automatically calculate formulas
  * `AutoEdit = true`: For editing a cell, just begin typing. Alternatively you can begin edit mode by pressing F2.
  * `AutoExpand = [aeData, aeNavigation, aeDefault]`: Don't restrict usage of the grid to the predefined grid dimensions, for an Excel-like user-interface with an "infinite" worksheet.
  * `EditorLineMode = elmMultiLine`: Activate cell editor supporting multiple lines during editing.
  * `MouseWheelOption = mwGrid`: The mouse wheel scrolls the grid, not the selected cell.
  * `Options`: add these flags to the standard options inherited from TCustomGrid: 
    * `goColSizing`: the user can change a column width by dragging the vertical separating line between two column header cells
    * `goRowSizing`: the user can change a row height by dragging the horizontal separating line between two row header cells
    * `goDblClickAutosize`: a double-click on a separating line between two column or row header cells resizes the column width or row height to its optimum value.
    * `goEditing`: puts the grid into edit mode (same as `AutoEdit`)
    * `goThumbTracking`: immediate scrolling of the grid while the scrollbar is dragged with the mouse (if this option is off scrolling occurs at the moment when the mouse button is released).
  * `TextOverflow = true`: allow long cell text flow into empty adjacent cells



## See also

  * [FPSpreadsheet tutorial: Writing a mini spreadsheet application](<FPSpreadsheet_tutorial__Writing_a_mini_spreadsheet_application.md> "FPSpreadsheet tutorial: Writing a mini spreadsheet application")
  * [API documentation of FPSpreadsheet](<FPSpreadsheet.md> "FPSpreadsheet")
  * [RPN formulas in FPSpreadsheet](<RPN_Formulas_in_FPSpreadsheet.md> "RPN Formulas in FPSpreadsheet")
  * [FPSpreadsheet: List of formulas](<FPSpreadsheet__List_of_formulas.md> "FPSpreadsheet: List of formulas")
  * [Grids reference page](<Grids_Reference_Page.md> "Grids Reference Page")

---

_Source: [https://wiki.freepascal.org/TsWorksheetGrid](https://web.archive.org/web/20250418093103/https://wiki.freepascal.org/TsWorksheetGrid)_
