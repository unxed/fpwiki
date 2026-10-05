# GridPrinter

[Template:GridPrinter](</index.php?title=Template:GridPrinter&action=edit&redlink=1> "Template:GridPrinter \(page does not exist\)")

## Contents

  * 1 About
  * 2 Download and Installation
    * 2.1 Release version
    * 2.2 Development version
  * 3 Getting Started
    * 3.1 How can I print a StringGrid?
    * 3.2 How can I see a preview of the printout before the printer starts to waste paper?
    * 3.3 I have several printers and want to make sure that the print-out goes to the right one
    * 3.4 Can individual cell formatting by the OnPrepareCanvas event be applied to the printout?
    * 3.5 Can I print a DBGrid?
    * 3.6 How can I print a page number?
  * 4 Documentation
    * 4.1 TGridPrinter
      * 4.1.1 Public methods
      * 4.1.2 Public properties
      * 4.1.3 Published properties and events
        * 4.1.3.1 TGridPrnHeaderFooter
        * 4.1.3.2 TGridPrnMargins
    * 4.2 TGridPrintPreviewDialog
      * 4.2.1 Public methods
      * 4.2.2 Published properties
        * 4.2.2.1 TGridPrintPreviewFormParams
      * 4.2.3 Functionality of the preview form
    * 4.3 GridPrinter Actions
      * 4.3.1 TGridPrinterAction
      * 4.3.2 TGridPrintPreviewAction



## About

[![GridPrinter](https://wiki.freepascal.org/images/0/08/GridPrinter_v0.2.0.png)](</File:GridPrinter_v0.2.0.png> "GridPrinter")

**`TGridPrinter`** is a component to simplify printing of string grids or other descendants of TCustomGrid. It is bundled together with a ready-made print preview dialog, **`TGridPrintPreviewDialog`** , as well as standard actions to trigger printing or to show the preview dialog without writing a single line of code. 

Author: Werner Pamler. 

License: Modified LGPL-2 (with linking exception, like Lazarus LCL). 

## Download and Installation

### Release version

The package is made available for installation by the Online-Package-Manager. Additionally, a zip file with the most recent release version can be found at [Lazarus CCR at SourceForge](<https://sourceforge.net/projects/lazarus-ccr/files/GridPrinter/>); unzip the file into any folder; load the file _gridprinterpkg.lpk_ into Lazarus and click _Use_ > _Install_. 

### Development version

The current development version is hosted at [Lazarus CCR](<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/gridprinter/>). Use SVN to check it out, or download the zipped snapshot from this page. Install by loading the _gridprinterpkg.lpk_ package file into Lazarus and clicking _Use_ > _Install_. 

## Getting Started

#### How can I print a StringGrid?

Nothing easier when you have the GridPrinter package... 

  * Drop a `TGridPrinter` component on the form.
  * Link its `Grid` property to the StringGrid that you want to print.
  * In the `OnClick` handler of the button or menu item that is supposed to start the print-out, call the `Print` method of the `GridPrinter`. That's all.


    
    
    procedure TForm1.PrintButtonClick(Sender: TObject);
    begin
      GridPrinter1.Print;
    end;
    

#### How can I see a preview of the printout before the printer starts to waste paper?

  * Drop also a `TGridPrintPreviewDialog` on the form.
  * Links its `GridPrinter` property to the GridPrinter instance that you had added in the previous exercise.
  * In the `OnClick` handler of the button or menu item that is supposed to show the preview, call the `Execute` method of the `GridPrintPreviewDialog`.
  * In the preview dialog you can scroll through the pages, zoom to various levels, change page orientation, adjust the page margins by dragging with the mouse, or add a header and/or footer. Of course, you can also print here, too.


    
    
    procedure TForm1.PrintPreviewButtonClick(Sender: TObject);
    begin
      GridPrintPreviewDialog1.Execute;
    end;
    

#### I have several printers and want to make sure that the print-out goes to the right one

`TGridPrinter` has a property `ShowPrintDialog` which controls whether a print dialog should be displayed before printing starts. The kind of dialog is determined by the following options: 

  * `gpdNone`: no print dialog
  * `gpdPageSetup`: shows a `TPageSetupDialog`. Here you can select the paper size and set page orientation as well as page margins.
  * `gpdPrintDialog`: shows a `TPrintDialog` where you can select the printer, the number of copies and the range of printed pages.
  * `gpdPrinterSetup`: shows `TPrinterSetupDialog`. Here you can select the printer, the paper size and the page orientation.



#### Can individual cell formatting by the OnPrepareCanvas event be applied to the printout?

Yes. The `TGridPrinter` fires an `OnPrepareCanvas` event, too. Both even can share the same event handler. You only should be careful to use the correct `Canvas` when handling the event: When the `Sender` of the event is the grid, you must refer to the grid's `Canvas`, and when the `Sender` is the `GridPrinter` you must use that `Canvas` exposed by the `GridPrinter`; during printing this is the printer's `Canvas`, and when the preview is painted it is the `Canvas` of a temporary `TBitmap`. 

Let's assume that your `OnPrepareCanvas` handler is supposed to paint the column title in bold type face. This could be done by the following code. The procedure must be assigned to the `OnPrepareCanvas` of both grid and `GridPrinter`. 
    
    
    procedure TForm1.PrepareCanvasHandler(Sender: TObject; ACol, ARow: Integer; AState: TGridDrawState);
    var
      lCanvas: TCanvas;
    begin
      if Sender = StringGrid1 then
        lCanvas := StringGrid1.Canvas
      else
      if Sender = GridPrinter1 then
        lCanvas := GridPrinter1.Canvas
      else
        raise Exception.Create('Unknown sender of OnPrepareCanvas.');
    
      if ARow < StringGrid1.FixedRows then
        lCanvas.Font.Style := [fsBold];
    end;
    

#### Can I print a DBGrid?

Yes, but it requires a bit more work than just connecting it to the `Grid` property of the `GridPrinter`. A `DBGrid` displays data from a `TDataset`, but it keeps only a small number of records. But you usually want to print the entire dataset, rather than just the section loaded into the `DBGrid`. 

At first we must tell the `GridPrinter` how many rows it will have to print. This can be done by providing a handler for the event `OnGetRowCount` in which we pass the dataset's `RecordCount` value to the provided parameter. Note that the title row must be included here. Note also that the `RecordCount` property often is only correct after the dataset pointer has been moved to the last record and back to the first record. 
    
    
    procedure TForm1.GridPrinter1GetRowCount(Sender: TObject; AGrid: TCustomGrid;
      var ARowCount: Integer);
    var
      dbGrid: TDBGrid;
    begin
      dbGrid := AGrid as TDBGrid;
      dbGrid.Datasource.Dataset.Last;
      dbGrid.Datasource.Dataset.First;
      ARowCount := dbGrid.Datasource.Dataset.RecordCount + 1;  // we must 1 for the header row
    end;
    

In order to provide the cell texts for the GridPrinter we must write a handler for the `OnGetCellText` event which is fired whenever the GridPrinter needs any cell text. We will return to this in a minute. We must first clarify the process flow. The easiest way to get the cell texts is by iterating over the entire dataset. Provide a handler for the `OnBeforePrint` event which moves the dataset to its first record: 
    
    
    procedure TForm1.GridPrinter1BeforePrint(Sender: TObject);
    begin
      DBGrid1.DataSource.Dataset.First;
    end;
    

Another event, `OnNewLine` fires when the `GridPrinter` has finished printing a line and wants to proceed with the next one. This means we must advance the dataset to its next record. However, if a printed line extends over several pages we must be able to return to this record for completing its printout on the next page. Generally this must be handled by setting bookmarks - please see the example _dbgrid2_ in the `GridPrinter` installation folder for details. For some flat-file databases such as _dBase_ , _BufDataset_ etc. there is a simpler way by using the `RecNo` property of the dataset. This is a running number for each record starting at 1 with the first record. So, in the `OnNewLine` event we simply set the dataset's `RecNo` to the row parameter passed to the event: 
    
    
    procedure TForm1.GridPrinter1NewLine(Sender: TObject; AGrid: TCustomGrid;
      ARow: Integer);
    var
      dbGrid: TDBGrid;
    begin
      dbGrid := AGrid as TDBGrid;
      BufDataset1.RecNo := ARow;  // RecNo starts at 1. ARow starts at 1, too, since we display the header row   
    end;
    

Then, within each row, the `GridPrinter` proceeds from grid column to grid column. In order to extract the cell text, we determine the column `col` from the `ACol` parameter of the `OnGetCellText`event and thus have access to the dataset field in that column (`col.Field`). Finally, we can query the cell text by calling the field's `AsString` method. Special handling is required for the very first row (`ARow = 0`) which corresponds to the grid titles: 
    
    
    procedure TForm1.GridPrinter1GetCellText(Sender: TObject; AGrid: TCustomGrid;
      ACol, ARow: Integer; var AText: String);
    var
      dbGrid: TDBGrid;
      col: TColumn;
      colOffs: Integer;
    begin
      AText := '';
    
      dbGrid := AGrid as TDBGrid;
    
      if (dgIndicator in dbGrid.Options) then
        colOffs := 1
      else
        colOffs := 0;
    
      if ACol < colOffs then
        exit;
    
      col := dbGrid.Columns[ACol - colOffs];
    
      if (ARow = 0) then
        AText := col.FieldName
      else
        AText := col.Field.AsString;
    end;
    

As already mentioned, there are `DBGrid` sample projects in the `GridPrinter` installation to demonstrate the steps for `DBGrid` printing. 

#### How can I print a page number?

You can print a page number at the top or bottom of every page by activating the header or footer of the `GridPrinter`. For this you select the `Header` (or `Footer` property and switch is `Visible` property to `true`. Then you enter the header (or footer) text in the property `Text`. In order to print the page number use for it the symbol `$PAGE` which is replaced by the number of the currently printed page. Note that the header/footer text consists of a left-aligned, centered and right-aligned part separated by a `'|'` character. So, if you want the page number to be centered the `Text` should be `'|$PAGE|'`, and if you want to include the total page count you could use '|$PAGE of $PAGECOUNT|'. Learn about the other symbols in the documentation section below. 

## Documentation

### TGridPrinter

#### Public methods

  * `function CreatePreviewBitmap(APageNo, APercentage: Integer): TBitmap` \-- Creates the preview bitmap for the printout of the specified page. The bitmap is scaled by the given percentage relative to the original pager size (100).
  * `function GetCellText(ACol, ARow: Integer): String` \-- Returns the text to be printed for the grid cell at the given row and column indices.
  * `procedure Print` \-- Prints the grid. **Main method of the component**.
  * `procedure ScaleToPages(NumHor, NumVert: Integer)` \-- Scales the printout such that it fits on `NumHor` pages horizontally and `NumVert` pages vertically. When `NumHorPages` is 0 (or smaller) only the height is scaled according to `NumVert`. Similarly when `NumVert` is 0 (or smaller).
  * `function ScaleX(AValue: Integer): Integer` \-- General scaling function for conversion of horizontal screen pixels to pixels on the printer or the preview bitmap.
  * `function ScaleY(AValue: Integer): Integer` \-- General scaling function for conversion of vertical screen pixels to pixels on the printer or the preview bitmap.
  * `procedure UpdatePreview` \-- Fires the event `OnUpdatePreview` if the `GridPrinter` is in preview mode.



#### Public properties

  * `Canvas: TCanvas` (read-only) -- Canvas currently used by the `GridPrinter`. This is either the printer canvas or the canvas of the preview bitmap sent to the `TGridPrintPreview` component.
  * `ColCount: Integer` (read-only) -- Number of columns to be printed (including fixed columns).
  * `ColWidth[AIndex: Integer]: Double` (read-only) -- Width of the specified column, given in pixels of the current output device (printer or preview bitmap).
  * `FooterMargin: Integer` (read-only) -- Distance of the footer bottom from the page bottom, in pixels of the current output device.
  * `HeaderMargin: Integer` (read-only) -- Distance of the header top from the page top, in pixels of the current output device.
  * `PageHeight: Integer` (read-only) -- Height of the page, in pixels of the current output device.
  * `PageWidth: Integer` (read-only) -- Width of the page, in pixels of the current output device.
  * `PageRect: TRect` (read-only) -- Rectangle on the page, in pixels of the current output device, available for printing the grid. In other words: size of the page with margins subtracted.
  * `PixelsPerInchX: Integer` (read-only) -- Pixel density of the current output device, in horizontal direction.
  * `PixelsPerInchY: Integer` (read-only) -- Pixel density of the current output device, in vertical direction.
  * `Padding: Integer` (read-only) -- Distance, in pixels of the current output device, between cell text and cell border.
  * `PageCount: Integer` (read-only) -- Total number of pages to be printed.
  * `PrintPageNumber: Integer` (read-only) -- Number of the page which is currently printed. Pages numbers begin with `1`.
  * `PrintScaleToNumHorPages: Integer` \-- The `PrintScalingFactor` is adjusted such that this number of pages fits onto a single sheet of paper horizontally.
  * `PrintScaleToNumVertPages: Integer` \-- The `PrintScalingFactor` is adjusted such that this number of pages fits onto a single sheet of paper vertically.
  * `PrintScalingMode: TGridPrnScalingMode` \-- Determines how the print output is scaled. 
    * `smManual` \-- The current value of the `PrintScalingFactor` is applied.
    * `smFitToWidth` \-- The `PrintScalingFactor` is adjusted so that the number of pages given by `PrintScaleToNumVertPages` fit onto a single page vertically.
    * `smsmFitToHeight` \-- The `PrintScalingFactor` is adjusted so that the number of pages given by `PrintScaleToNumHorPages` fit onto a single page horizontally.
    * `smFitAll` \-- The `PrintScalingFactor` is adjusted so that the entire grid fits onto a single page.
  * `RowCount: Integer` (read-only) -- Number of rows to be printed (including fixed rows).
  * `RowHeight[AIndex: Integer]: Double` (read-only) -- Height of the specified row, given in pixels of the current output device.



#### Published properties and events

**Properties**

  * `Grid: TCustomGrid` \-- Links to the grid to be printed. Ideally, this should be a `TStringGrid`, but other descendants of `TCustomGrid` can be printed as well when the appropriate event handlers are provided.
  * `BorderLineColor: TColor` \-- Line color of the outer border of the printed grid.
  * `BorderLineWidth: Double` \-- Line width of the outer border of the printed grid, in millimeters. If not positive, the scaled value of the grid's `GridLineWidth` is used.
  * `FileName: String` \-- Has no functional use inside the `GridPrinter`. But indicates the name of the file which is printed, if applicable. The filename is displayed in the header or footer of the printout/preview where it is used to replace the symbols `$FULL_FILENAME`, `$FILENAME`, `$PATH` in the header/footer `Text`.
  * `FixedLineColor: TColor` \-- Line color of the dividing line between fixed and normal cells in the printout/preview.
  * `FixedLineWidth: Double` \-- Line width of the dividing line between fixed and normal cells in the printout/preview, in millimeters. If not positive, the scaled value of the grid's `GridLineWidth` is used.
  * `Footer: TGridPrnHeaderFooter` \-- Parameters for printing a footer at the bottom of each page. See TGridPrnHeaderFooter.
  * `FromPage: Integer` \-- Defines the first page to be printed. Any value less than 1 is interpreted as "first page".
  * `GridLineColor: TColor` \-- Color of the inner grid lines.
  * `GridLineWidth: Double` \-- Line width of the inner grid lines, in millimeters. If not positive, the scaled value of the grid's `GridLineWidth` is used.
  * `Header: TGridPrnHeaderFooter` \-- Parameters for printing a header at the top of each page. See TGridPrnHeaderFooter
  * `Margins: TGridPrnMargins` \-- Defines page and header/footer margins, in millimeters. See TGridPrnMargins.
  * `Monochrome: Boolean` \-- Prints the grid only with black color.
  * `Options: TGridPrnOptions` \-- Options for fine-tuning the printout: 
    * `gpoCenterHor` \-- Centers the printed table on the page horizontally.
    * `gpoCenterVert` \-- Centers the printed table on the page vertically.
    * `gpoHorGridLines` \-- Overrides the corresponding grid option and can be used to show/hide horizontal grid lines between the printed rows.
    * `gpoVertGridLines` \-- Overrides the corresponding grid option and can be used to show/hide vertical grid lines between the printed columns.
    * `gpoFixedHorGridLines` \-- Can be used to show/hide horizontal grid lines between the fixed cells.
    * `gpoFixedVertGridLines` \-- Can be used to show/hide vertical grid lines between the fixed cells.
    * `gpoHeaderBorderLines` \-- Can be used to show/hide dividing lines between the fixed and normal cells.
    * `gpoOuterBorderLines` \-- Can be used to show/hide outer lines around the grid.
  * `Orientation: TPrinterOrientation` \-- Defines the page orientation: `poPortrait`, `poLandscape`, `poReverseLandscape`, `poReversePortrait` (declared in the _Printers_ unit).
  * `PrintOrder: TGridPrnOrder` \-- Large grids are wrapped into several pages. This property defines whether rows (`poRowsFirst`) or columns (`poColsFirst`) will be completed first.
  * `PrintScaleFactor: Double` \-- Scaling factor for the printout. Default: `1.0`. Call the method `ScaleToPages()` if you want to adjust the `PrintScaleFactor` such that the entire printout fits on a given number of pages.
  * `ShowPrintDialog: TGridPrnDialog` \-- Determines whether one of the Lazarus print dialogs should be shown before printing begins: `gpdNone`, `gpdPageSetup`, `gpdPrintDialog`, `gpdPrinterSetup`.
  * `ToPage: Integer` \-- Defines the last page to be printed. Any value larger than the real page count prints to the last page.



**Events**

  * `OnAfterPrint` \-- Fires when printing is finished.
  * `OnBeforePrint` \-- Fires before printing begins. The printer has not yet received its `BeginDoc` command.
  * `OnGetCellText` \-- Whenever the `GridPrinter` needs to know the text in a specific cell it sends this event. The text to be printed can be passed in a parameter. This way non-standard grid which have different cell technology can be printed as well.
  * `OnGetRowCount` \-- In this event the application can tell the `GridPrinter` the number of rows to be printed. This is useful for printing a `DBGrid` which holds only a small portion of all rows of a dataset.
  * `OnGetColCount` \-- Similar to `OnGetRowCount`, just for columns, rather than rows.
  * `OnLinePrinted` \-- Fires when the `GridPrinter` has finished printing a specific line.
  * `OnNewLine` \-- Fires when the `GridPrinter` starts printing a new line of cells.
  * `OnNewPage` \-- Fires when the `GridPrinter` begins a new page.
  * `OnPrepareCanvas` \-- Similar to the equally-named event of the grid. When the canvas for printing a cell has been prepared the user has the opportunity here to override the selection of colors, fonts, alignments etc. The handler can be shared between grid and `GridPrinter`, it only must be guaranteed that the correct canvas is used in the code.
  * `OnPrintCell` \-- When this event has a handler it completely replaces the built-in code for printing a cell. Can be used for printing very specific grids.
  * `OnUpdatePreview` \-- When the `GridPrinter` runs in preview mode the application can be notified, for example, when some parameter has been changed and the preview can repaint itself.



##### TGridPrnHeaderFooter

This is the class of the header and footer lines which can be printed at the top and bottom of each page. 

**Published properties**

  * `Font: TFont` \-- Font used for printing the header/footer.
  * `LineColor: TColor` \-- Color of the dividing line printed below the header/above the footer.
  * `LineWidth: Double` \-- Line width of the dividing line printed below the header/above the footer, in millimeters.
  * `SectionSeparator: String` \-- The header/footer text can consist of a left-aligned, centered, and a right-aligned part, all specified together in the `Text`property. Defines the separator character or string between these three sections, default: `'|'`.
  * `ShowLine: Boolean` \-- Shows/hides the dividing line printed below the header/above the footer.
  * `Text: String` \-- Contains the text to be displayed in the header/footer. It can consist of up to three sections, separated by the `SectionSeparator`, which are printed at the left, in the center or at the right end of the header/footer. Moreover, the `Text` can contain the following symbols which are replaced during printing by the current values: 
    * `$DATE` \-- Replaced by current date.
    * `$PAGECOUNT` \-- Total number of pages to be printed.
    * `$PAGE` \-- Number of the currently printed page. First page number is `1`.
    * `$FULL_FILENAME` \-- Is replaced by the full path of the `FileName` property of the `GridPrinter`.
    * `$FILENAME` \-- Is replaced by the `FileName` property of the `GridPrinter` after removal of the file path.
    * `$PATH` \-- Is replaced by the path to the `FileName` property of the `GridPrinter`.
    * `$TIME` \-- Replaced by current time.
  * `Visible: Boolean` \-- Shows/hides the header/footer.



**Public properties**

  * `ProcessedText[AIndex: TGridPrnHeaderFooterSection]: String` where `TGridPrnHeaderFooterSection = (hfsLeft, hfsCenter, hfsRight)` \-- returns the header/footer text in the specified section after replacement of the symbols.
  * `SectionText[AIndex: TGridPrnHeaderFooterSection]: String` where `TGridPrnHeaderFooterSection = (hfsLeft, hfsCenter, hfsRight)` \-- returns the specified section `Text`.



##### TGridPrnMargins

This class collects the parameters for the page margins: 

**Published properties**

  * `Left: Double` \-- Left margin, in millimeters.
  * `Top: Double` \-- Top margin, in millimeters.
  * `Right: Double` \-- Right margin, in millimeters.
  * `Bottom: Double` \-- Bottom margin, in millimeters.
  * `Header: Double` \-- Margin to the top of the header, in millimeters. Note that it is not checked that the header does not overlap with the grid.
  * `Footer: Double` \-- Margin to the bottom of the footer, in millimeters. Note that it is not checked that the footer does not overlap with the grid.



### TGridPrintPreviewDialog

In unit _GridPrnPreviewDlg_. 

#### Public methods

  * `procedure Execute` \-- Displays the preview form **(Main method of the component)**.



#### Published properties

  * `FormParams: TGridPrintPreviewFormParams` \-- Parameters which define size and position of the preview form. See TGridPrintPreviewFormParams.
  * `GridPrinter: TGridPrinter` \-- Links to the `TGridPrinter` instance which actually performs the printing.
  * `Options: TGridPrintPreviewOptions` \-- Controls the options the user has to modify the preview (declared in unit _GridPrnPreviewForm_ : 
    * `ppoNavigationBtns` \-- Enables/disables page navigation in the preview.
    * `ppoNavigationEdit` \-- Shows/hides the edit control in the preview toolbar with which the user can navigate to a specific page of the preview.
    * `ppoZoomBtns` \-- Enables/disables zooming in the preview.
    * `ppoPageOrientationBtns` \-- Enables/disables selection of page orientation.
    * `ppoMarginsBtn` \-- Enables/disables page margin selection in the preview.
    * `ppoHeaderFooterBtn` \-- Enables/disables the possibility to set up the header and footer in a dialog.
    * `ppoPrintOrderBtns` \-- Enables/disables the possibility to define the order of pages when the entire print-out does not fit onto a single page of paper.
    * `ppoCenterBtns` \-- Shows/hides the buttons to center the grid on the printed page.
    * `ppoScalePrinterBtn` \-- Shows/hides the button to scale the printer output manually or to force a given count of pages onto a single sheet.
    * `ppoPageSetupBtn` \-- Shows/hides a toolbar button with a dropdown menu containing options to control the page layout (page orientation, margin selection, etc).
    * `ppoPageNumberInfo` \-- Shows/hides the display of page number and total page count in the preview form.
    * `ppoZoomLevelInfo` \-- Shows/hides the display of the preview zoom level (percentage) in the preview form.
  * `Zoom: Integer` \-- Scaling factor, in percent, applied when the preview bitmap is displayed in the preview form. Default: `100`.
  * `ZoomMode: TGridPrintPreviewZoomMode` \- Determines how the preview bitmap is scaled by means of the `Zoom` scaling factor: 
    * `zmCustom` \-- The current value of the `Zoom` factor is applied directly.
    * `zmFitWidth` \-- The `Zoom` factor is adjusted so that the page fits horizontally into the preview window.
    * `zmFitHeight` \-- The `Zoom` factor is adjusted such that the page fits vertically into the preview window.



##### TGridPrintPreviewFormParams

  * `Left: Integer` \-- Position of the left edge of the preview form on the screen, in screen pixels.
  * `Top: Integer` \-- Position of the top edge of the preview form on the screen, in screen pixels.
  * `Width: Integer` \-- Width of the preview form, in screen pixels.
  * `Height: Integer` \-- Height of the preview form, in screen pixels.
  * `Position: TPosition` \-- Standard LCL form position options overriding the other parameters in the `GridPrintPreviewFormParams`, e.g. `poMainFormCenter` or `poScreenCenter` to center the form over the mainform or on the screen, respectively.



#### Functionality of the preview form

The preview dialog contains a toolbar with a variety of buttons. By means of the `Options` of the `GridPrintPreviewDialog` it is possible to hide those buttons which are not needed. 

Here is a description of what the buttons can be used for: 

  * [![gridprn print.png](https://wiki.freepascal.org/images/2/21/gridprn_print.png)](</File:gridprn_print.png>) Sends the grid to the printer, as displayed by the preview.
  * [![gridprn firstpage.png](https://wiki.freepascal.org/images/9/9c/gridprn_firstpage.png)](</File:gridprn_firstpage.png>) Displays the first page of the preview.
  * [![gridprn prevpage.png](https://wiki.freepascal.org/images/a/ab/gridprn_prevpage.png)](</File:gridprn_prevpage.png>) Displays the previous page of the preview.
  * [![gridprn nextpage.png](https://wiki.freepascal.org/images/3/39/gridprn_nextpage.png)](</File:gridprn_nextpage.png>) Displays the next page of the preview.
  * [![gridprn lastpage.png](https://wiki.freepascal.org/images/a/a5/gridprn_lastpage.png)](</File:gridprn_lastpage.png>) Displays the last page of the preview.
  * [![gridprn zoomin.png](https://wiki.freepascal.org/images/4/41/gridprn_zoomin.png)](</File:gridprn_zoomin.png>) Enlarges the preview ("zoom in").
  * [![gridprn zoomout.png](https://wiki.freepascal.org/images/b/bc/gridprn_zoomout.png)](</File:gridprn_zoomout.png>) Reduces the preview ("zoom out").
  * [![gridprn zoom100.png](https://wiki.freepascal.org/images/8/8e/gridprn_zoom100.png)](</File:gridprn_zoom100.png>) Original size of the preview page (100%).
  * [![gridprn zoomtowidth.png](https://wiki.freepascal.org/images/a/a4/gridprn_zoomtowidth.png)](</File:gridprn_zoomtowidth.png>) Scales the preview image so that it fills the width of the preview window.
  * [![gridprn zoomtoheight.png](https://wiki.freepascal.org/images/6/6e/gridprn_zoomtoheight.png)](</File:gridprn_zoomtoheight.png>) Scales the preview image so that it fills the height of the preview window.
  * [![gridprn portrait.png](https://wiki.freepascal.org/images/0/09/gridprn_portrait.png)](</File:gridprn_portrait.png>) Switches the page to portrait orientation.
  * [![gridprn landscape.png](https://wiki.freepascal.org/images/c/c1/gridprn_landscape.png)](</File:gridprn_landscape.png>) Switches the page to landscape orientation.
  * [![gridprn headerfooter.png](https://wiki.freepascal.org/images/f/f7/gridprn_headerfooter.png)](</File:gridprn_headerfooter.png>) Shows a dialog to set up the header and/or footer of the printout.
  * [![gridprn pagemargins.png](https://wiki.freepascal.org/images/a/a0/gridprn_pagemargins.png)](</File:gridprn_pagemargins.png>) Displays the page margins as lines. Drag these lines to adjust the margins.
  * [![gridprn rowsfirst.png](https://wiki.freepascal.org/images/8/82/gridprn_rowsfirst.png)](</File:gridprn_rowsfirst.png>) In case of large grids the rows are printed first before advancing to the next block of rows.
  * [![gridprn colsfirst.png](https://wiki.freepascal.org/images/7/70/gridprn_colsfirst.png)](</File:gridprn_colsfirst.png>) In case of large grids the columns are printed first before advancing to the next block of columns.
  * [![gridprn centerhor.png](https://wiki.freepascal.org/images/9/9b/gridprn_centerhor.png)](</File:gridprn_centerhor.png>) Centers the grid on the printed page horizontally.
  * [![gridprn centervert.png](https://wiki.freepascal.org/images/8/81/gridprn_centervert.png)](</File:gridprn_centervert.png>) Centers the grid on the printed page vertically.
  * [![gridprn scaleprinter.png](https://wiki.freepascal.org/images/1/14/gridprn_scaleprinter.png)](</File:gridprn_scaleprinter.png>) Opens a dialog in which a scale factor for the printout can be defined. This way, for example, a large grid can be "squeezed" onto a single page.
  * [![gridprn pagesetup.png](https://wiki.freepascal.org/images/5/5b/gridprn_pagesetup.png)](</File:gridprn_pagesetup.png>) Opens a dropdown menu with a collection of the commandsabove. This is useful when not all buttons are shown on the toolbar.



The icons displayed on these buttons are taken from the Lazarus general-purpose image collection provided by [Roland Hahn](<https://www.rhsoft.de/>). 

### GridPrinter Actions

Unit _GridPrnActions_ implements two standard actions for further simplification of usage 

#### TGridPrinterAction

When its property `GridPrinter` is linked to a `TGridPrinter` instance and the action is linked to the `Action` property of a control a click on the control executes the `Print` method of the `GridPrinter` without any additional code. 

#### TGridPrintPreviewAction

When its property `PreviewDialog` is linked to a `TGridPrintPreviewDialog` instance and the action is linked to the `Action` property of a control a click on the control runs the `Execute` method of the `GridPrintPreviewDialog` without any additional code.

---

_Source: [https://wiki.freepascal.org/GridPrinter](https://web.archive.org/web/20240920204138/https://wiki.freepascal.org/GridPrinter)_
