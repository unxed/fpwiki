# TECGrid

## Contents

  * 1 Overview
    * 1.1 License
    * 1.2 Author
    * 1.3 Download and Install
  * 2 Options
    * 2.1 Property Options
    * 2.2 Other Grid Properties
    * 2.3 Property Column.Option
    * 2.4 Property Column.Title
    * 2.5 Other Column Properties
  * 3 Using the Component
    * 3.1 Mouse
    * 3.2 Keyboard
    * 3.3 Code
  * 4 Workflow
    * 4.1 Displaying data
    * 4.2 Editing data
    * 4.3 Order of columns



## Overview

**TECGrid** [![ecgrid.png](https://wiki.freepascal.org/images/4/41/ecgrid.png)](</File:ecgrid.png>) is a lightweight grid component. It does not contain any strings, it has no [DataSource](<TDataSource.md> "TDataSource") connection and its is not possible to custom-draw each cell. Therefore it's not alternative to [TStringGrid](<TStringGrid.md> "TStringGrid"), [TDBGrid](<TDBGrid.md> "TDBGrid") nor [TDrawGrid](<TDrawGrid.md> "TDrawGrid"). It is designed to show content of any data structure and all displayed strings are taken from OnGetHeaderText and OnGetDataCellText events. It features bi-directional mode, fixed columns, fixed rows, column moving, column sizing, various fixed-cell styles and many others, described below. 

[![tecgrid.png](https://wiki.freepascal.org/images/7/79/tecgrid.png)](</File:tecgrid.png>)

TECGrid is part of [Eye-Candy Controls](<Eye-Candy_Controls.md> "Eye-Candy Controls") (shortly ECControls or EC-Controls) set of visual controls written for Lazarus. Their design is based on Themes, therefore its look is very native everywhere, no matter what widgetset you use. 

TECGrid is installed to the tab EC-C on the Lazarus component palette. 

Each release is announced on Lazarus Forum in section Third Party Announcements. There are always attached files `README.txt` (list of all known issues) and `CHANGELOG.txt` (list of all changes from previous release). 

### License

GNU Lesser General Public License 2.0 with linking exception (a.k.a. Modified LGPL). File ectabctrl.pas contains license header. Also, files `COPYING.modifiedLGPL.txt` and `COPYING.LGPL.txt` are bundled to each archive. 

### Author

This component is written by Blaazen. Copyright notice and real name is mentioned in the header of the unit. You can contact author on Lazarus Forum (nickname: Blaazen) in any thread about EC-Controls. If you are logged in to forum, you can get author's e-mail or send him private message. 

### Download and Install

See [Eye-Candy Controls#Install](<Eye-Candy_Controls.md> "Eye-Candy Controls")

## Options

### Property Options

Option | Description   
---|---  
egoAlwaysShowEditor | Editor is shown always; Grid and Column must not be egoReadOnly/ecoReadOnly   
egoColMoving | Columns can be moved by mouse; 1) Except fixed columns 2) Columns can be always moved by code   
egoColSizing | Columns can be resized by mouse; Column must be ecoSizing   
egoDottedGrid | Grid line is dotted   
egoEnlargeAlways | Column is enlarged even if Editor is not opened   
egoHeaderPushedLook | Column header has pushed look when clicked   
egoHilightCol | Whole column is highlighted   
egoHilightRow | Whole row is highlighted   
egoHorizontalLines | Show horizontal lines of grid   
egoReadOnly | Grid is ReadOnly, it never opens Editor   
egoScrollKeepVisible | Selection remains visible while scrolling (scrollbars/mouse wheel)   
egoSortArrow | Sorting arrow in title is visible; 1) Column must be ecoSorting 2) Grid must have FixedRows>=1   
egoTabs | Tab key goes through cells instead of other components   
egoUseOrder | Array OrderedCols is defined; flag excfOrder for Load/SaveColumnsToXML is available   
egoVerticalLines | Show vertical lines of grid   
egoWheelScrollsGrid | Mouse wheel scrolls the grid, selection does not change (i.e. it rolls away)   
  
### Other Grid Properties

AlternateColor | Color of even data rows   
---|---  
AlternateTint | Tint of even data rows in percents, 0 = normal color, 100 = AlternateColor   
Columns | Collection of columns   
FixedCols | Number of fixed columns   
FixedRowHeight | Height of fixed (header) rows   
FixedRows | Number of fixed (header) rows   
GridLineColor | Color of grid lines (data area only)   
GridLineWidth | Width of grid lines (data area only)   
Images | ImageList for column headers; see Column.Title.ImageIndex property   
RowHeight | Height of data rows   
Style | Style of cells of fixed rows and/or fixed columns   
  
### Property Column.Option

Option | Description   
---|---  
ecoCellToHint | Content of data cell is shown as a Hint (tooltip)   
ecoEnlargePixels | Property EnlargeWidth means pixels, otherwise means percents   
ecoReadOnly | Column is ReadOnly (never shows editor)   
ecoSizing | Column can be resized by mouse; Grid must be egoColSizing   
ecoSorting | Sorting arrow in title is visible; Grid must be egoSortArrow and must have FixedRows>=1   
ecoVisible | Column is visible   
  
### Property Column.Title

Option | Description   
---|---  
Alignment | Text alignment of the column header; bi-directional mode is respected   
FontOptions | Font differences of the column header to Grid.Font   
ImageIndex | Image displayed in the column header; taken from Grid.Images   
Text | Caption of the column header; the only text that is not taken from events   
  
### Other Column Properties

Option | Description   
---|---  
Alignment | Text alignment of the column; bi-directional mode is respected   
Color | Background color of the column   
ColorTint | Tint of background color in percents, 0 = Grid.Color/Grid.AlternateColor, 100 = Column.Color   
EnlargeWidth | Increment to the column width; value means pixels or percents, depend on option ecoEnlargePixels   
FontOptions | Font differences to Grid.Font   
MaxWidth | Maximal width of the resizable column   
MinWidth | Minimal width of the resizable column   
  
## Using the Component

### Mouse

Left button

Click on a data-cell always selects it and possibly opens editor (depends on options egoAlwaysShowEditor, egoReadOnly, ecoReadOnly and implementation of OnSelectEditor event). 

Click on a cell of fixed columns changes row, which in turn changes selection and possibly opens editor (see above). 

Click on a cell of fixed rows changes SortIndex if option ecoSorting of Column is set. It happens even if option egoSortArrow is not set (arrow is invisibe). 

Middle button

Does nothing. 

Right button

Opens context menu. Note that Grid, Column and Column.Title can have different context menu. 

Dragging and Resizing

Dragging is realized by left mouse button. Dragging moves column on the new position (egoColMoving must be set). Fixed columns cannot be moved. Data columns cannot be moved in front of fixed columns. Shape of the mouse cursor shows if the column can be moved to the new position or not. 

Resizing is realized by left mouse button. Mouse must be pushed on the right edge of the column header (or the left edge in right-to-left BiDiMode) - when mouse cursor changes its shape. Column width can be resized between MinWidth and MaxWidth. Both Options egoColSizing and ecoSizing must be set. 

Mouse Wheel

Wheel moves selection up/down. 

Wheel + CTRL key (exactly ssModifier) scrolls grid. 

Option egoWheelScrollsGrid reverses this behaviour. 

### Keyboard

(Grid must be focused). 

Arrow keys change selection. 

Arrow keys + CTRL key (exactly ssModifier) scroll grid. 

Home/End keys skip to the first/last data column. 

Home/End keys + CTRL key (exactly ssModifier) skip to the first/last data row. 

PageUp/PageDown keys skip up/down by the number of visible rows. 

PageUp/PageDown keys + CTRL key (exactly ssModifier) scroll up/down by the number of visible rows. 

If option egoTabs is set, Tab key skips to the next data cell. If selection reaches the last column, it skips to the first column and the next row. Shift + Tab skips in the opposite direction. 

Enter, Space, F2, BackSpace and Delete keys open Editor (Grid/Column must not be egoReadOnly/ecoReadOnly). 

Keys 0..9 and A..Z also open Editor. 

CTRL+C copies content of the selection to the clipboard. 

### Code
    
    
    procedure BeginUpdate; override;
    

Reduces number of calculations and repaints when multiple properties change. 
    
    
    function CellRect(ACol, ARow: Integer): TRect;
    

Returns rectangle of the specified cell. 
    
    
    function CellRectEditor(ACol, ADataRow: Integer): TRect;
    

Returns corrected rectangle of the specified cell, it takes care of the GridLineWidth. Recommended for opening Editor. 
    
    
    procedure EndUpdate; override;
    

Each call of BeginUpdate; must be followed by EndUpdate. All changes will be processed. 
    
    
    function IsCellFullyVisible(ACol, ARow: Integer): Boolean;
    

Returns True if specified cell is fully visible. 
    
    
    function IsCellVisible(ACol, ARow: Integer): Boolean;
    

Returns True if specified cell is at least partially visible. 
    
    
    function IsColFullyVisible(ACol: Integer): Boolean;
    

Returns True if AColl is fully visible. 
    
    
    function IsColVisible(ACol: Integer): Boolean;
    

Returns True if ACol is at least partially visible. 
    
    
    function IsRowFullyVisible(ARow: Integer): Boolean;
    

Returns True if ARow is fully visible. 
    
    
    function IsRowVisible(ARow: Integer): Boolean;
    

Returns True if ARow is at least partially visible. 
    
    
    procedure LoadColumnsFromXML(AColumnsNode: TDOMNode; AXMLFlags: TXMLColFlags = cXMLColFlagsAll); overload;
    procedure LoadColumnsFromXML(AFileName: string; AColumnsNode: DOMString; AXMLFlags: TXMLColFlags = cXMLColFlagsAll); overload;
    

Loads parameters of columns. Option egoUseOrder must be set for using the Order properties. 
    
    
    function MakeCellFullyVisible(ACol, ARow: Integer; AForceColVisible: Boolean): Boolean;
    

Makes specified cell visible (without selecting it). If column is invisible, it can be forced to show by AForce parameter. 
    
    
    procedure MouseToCell(AMouse: TPoint; out ACol, ARow: Integer);
    

Returns indexes of a cell placed on the coordinates AMouse. 
    
    
    procedure SaveColumnsToXML(AXMLDoc: TXMLDocument; AColumnsNode: TDOMNode; AXMLFlags: TXMLColFlags = cXMLColFlagsAll); overload;
    procedure SaveColumnsToXML(AFileName: string; AColumnsNode: DOMString; AXMLFlags: TXMLColFlags = cXMLColFlagsAll); overload;
    

Saves parameters of columns. 
    
    
    procedure SaveToCSVFile(AFileName: string; ADelimiter: Char = ',';  AHeaders: Boolean = True; AVisibleColsOnly: Boolean = False);
    

Saves content of the grid to the text file. 
    
    
    procedure SelectCell(ACol, ARow: Integer; ASelectEditor: TEditorSelection; AForce: Boolean = False);
    

Changes selection to the cell specified by ACol and ARow. If column is invisible, it can be forced to show by AForce parameter. ASelectEditor specifies whether Editor will be opened (esNative depends on the egoAlwaysShowEditor). 
    
    
    procedure UpdateCell(ACol, ARow: Integer);
    

Updates single cell. 
    
    
    procedure UpdateColumn(ACol: Integer; AHeader: Boolean = False; AData: Boolean = True);
    

Updates cells in column. 
    
    
    procedure UpdateRow(ARow: Integer; AFixedCols: Boolean = False; AData: Boolean = True);
    

Updates visible cells in row. 
    
    
    procedure UpdateRowCount;
    

Call if you need to change number of data rows immediately. That's because RowCount is taken from OnGetDataRowCount event. 

## Workflow

ECGrid itself does not contain any data (except column titles) and does not provide any built-in editor. All columns must be defined, setting ColCount (as it is with TStringGrid and TDrawGrid) is not enough, this property is read-only here. 

### Displaying data

Lets assume that data are stored in any general structure, for example TFPGObjectList. For displaying all desired data is necessary: 

  * Create corresponding number of columns
  * Implement OnGetDataCellText event for each cell (for fixed columns too)
  * Implement OnGetDataRowCount event. Implementation of this event will return TFPGObjectList.Count (ADataRowCount parameter)



Note: It is also possible to implement only one OnGetDataCellText event and assign it to all columns. This event has parameter AColumn, so it is possible to use `**case** AColumn.Index **of**` construction. 

### Editing data

  * Create corresponding editors
  * Implement OnSelectEditor event
  * Implement OnChange or OnEditingDone events of the editors



Editing: 

  * Select cell and open editor (TEdit, TSpinEdit etc.). Example of implementation of the OnSelectEditor event:


    
    
    procedure TForm1.SelectEditor(Sender: TObject; ACol, ARow: Integer; var AEditor: TWinControl; AKey: Word);
    begin
      SpinEdit1.Value:={ fill appropriate value here };
      SpinEdit1.BoundsRect:=TECGrid(Sender).CellRectEditor(ACol, ARow);
      AEditor:=SpinEdit1;
    end;
    

  * Change value of the editor (text, number)
  * Editor triggers OnChange or OnEditingDone event
  * Event will change the data
  * Call Grid.Invalidate to repaint, new data will appear



Note: If number of items in list is changed (some items was added or deleted), it is necessary to call Grid.UpdateRows to display changes. 

### Order of columns

**Important:** Option egoUseOrder must be enabled for proper use of Orders. Do not change egoUseOrder during lifetime of the component. 

There are properties to get the relation of Indexes and Orders. 

Property 
    
    
    Columns[i].Order;
    

stores initial order of each column (column with Index=i in this case), while property 
    
    
    ColOrd[i];
    

returns Index of column with Order=i. 
    
    
    CellOrd[i, j];
    

is the same as 
    
    
    Cell[ColOrd[i], j];.
    

Example

Lets assume that grid has three columns and their are movable (option egoColMoving is enabled). 

Index  | 0 | 1 | 2   
---|---|---|---  
Title  | Name | Category | Count   
Order  | 0 | 1 | 2   
  
If the first column (Name) is edited, grid will trigger event OnSelectEditor with parameter ACol=0. 

Now assume that user will move the first column to the third position. 

Index  | 0 | 1 | 2   
---|---|---|---  
Title  | Category | Count | Name   
Order  | 1 | 2 | 0   
  
If the same column is edited again, grid will trigger event OnSelectEditor, but parameter ACol=2 now, which may be difficult to maintain. 

Even more, user wants to save defined order of columns when session is over and load it back in the next session (for example restart of an application). 

For this use case, there are properties ColOrd and CellOrd. 

If we use: 
    
    
    procedure TForm1.SelectEditor(Sender: TObject; ACol, ARow: Integer; var AEditor: TWinControl; AKey: Word);
    begin
      case ACol of
        0: { set editor for column Name };
        1: { set editor for column Category };
        2: { set editor for column Count };
      end;
    end;
    

This code works well as long as the columns are in their initial order. 

But we need to select the right editors for all columns, regardless of their new poitions. We must modify the code: 
    
    
    procedure TForm1.SelectEditor(Sender: TObject; ACol, ARow: Integer; var AEditor: TWinControl; AKey: Word);
    begin
      case ColOrd[ACol] of
        0: { set editor for column Name };
        1: { set editor for column Category };
        2: { set editor for column Count };
      end;
    end;         
    

  
There are methods LoadColumnsFromXML and SaveColumnsToXML which can store and refresh previous order of columns, but also their width and visibility (property Width and option ecoVisible). 

  
**Note:** When option egoUseOrder is disabled it reduces some calculations. Switch this property off if you extensively add and delete columns and/or if you disabled moving of columns (egoColMoving).

---

_Source: [https://wiki.freepascal.org/TECGrid](https://web.archive.org/web/20220101000000/https://wiki.freepascal.org/TECGrid)_
