# FPSpreadsheet: Examples

## Contents

  * 1 Requirements to create an FPSpreadsheet application
    * 1.1 Units
  * 2 Sample projects in the fpspreadsheet installation folder
  * 3 Code examples
    * 3.1 Opening an existing spreadsheet
    * 3.2 Writing a spreadsheet to file based on extension
    * 3.3 Converting between two spreadsheet formats
    * 3.4 Iterating through all worksheets
    * 3.5 Iterating through cells
    * 3.6 Excel 5 example
    * 3.7 Reading and writing of CSV files
    * 3.8 Converting a database to a spreadsheet
    * 3.9 Converting a large database table to a spreadsheet using virtual mode



## Requirements to create an FPSpreadsheet application

To create a project which uses the fpspreadsheet library, add the **fpspreadsheet_pkg** package to it's Lazarus project, or add the base directory of fpspreadsheet to you compiler options if using another IDE. 

### Units

The entire FPSpreadsheet package consists of several units. A spreadsheet application typically "uses" the following units: 

  * _fpspreadsheet_ : implements **TsWorkbook** and **TsWorksheet** and the **basic file reading/writing methods**
  * _fpstypes_ : declares most of the **data types** and **constants** used throughout the package. **Note:** in older versions these declarations were contained in fpspreadsheet.pas.
  * the unit(s) implementing the reader/writer for a given **file format** , e.g. _xlsbiff8_ for binary Excel files. If the application will be able to handle all formats "use" the unit _fpsallformats_.



The next units are required only occasionally: 

  * _fpsutils_ : a collection of **utility functions** that are occasionally needed (e.g. conversion of col/row indexes to Excel-like cell address string).
  * _fpscell_ : this unit is required if you use **direct cell formatting** (e.g. `cell^.BackgroundColor := scYellow`) instead of calling the corresponding worksheet method (`MyWorksheet.WriteBackgroundColor(cell, scYellow)`, in this example).
  * _fpsnumformat_ : collects all utility functions related to **number formats**.



All other units are probably not needed at the application level. In case of the **visual spreadsheet controls** , the needed units usually are inserted at design-time automatically. 

  


## Sample projects in the fpspreadsheet installation folder

A bunch of sample projects accompanies the FPSpreadsheet installation. They can be found in the folder "examples". Here is a brief description of these sample projects 

  * **db_import_export** is an example showing how to export a large database table to a spreadsheet file using virtual mode or **TFPSExport**. It also shows importing the spreadsheet into a database using virtual mode.
  * **fpsspeedtest** compares the effect of file format and various reading/writing parameters on the speed of writing and reading very large spreadsheet files. Again, please run the write test first which create the test files used for the read test.
  * Folder **read_write** : 
    * **excel2demo** contains command-line programs for writing and reading Excel 2.x xls files. Please run the write demo before the read demo so the required spreadsheet file is generated.
    * **excel5demo** , like excel2demo, but for Excel 5 xls files.
    * **excel8demo** , like excel2demo, but for Excel 97-2003 xls files.
    * **csvdemo** , like excel2demo, but for CSV files.
    * **htmldemo** , like excel2demo, but for HTML file (currently writing only).
    * **ooxmldemo** , like excel2demo, but for the new Excel xlsx files.
    * **opendocdemo** , like excel2demo, but for OpenOffice/LibreOffice ods files
    * **wikitabledemo** , like excel2demo, but for wiki table files. Note that the write example currently writes a format that the read example cannot understand.
  * **other** : simple commandline programs showing various aspects of the fpspreadsheet package. Have a look at _readme.txt_ for more details.
  * Folder **visual** : 
    * **fpschart** shows the application of the [TsWorksheetChartSource](<TsWorksheetChartSource.md> "TsWorksheetChartSource") and TsWorkbookChartSource and the interaction with the [TAChart](<TAChart.md> "TAChart") plotting package.
    * **fpsctrls** and **fpsctrls_no_install** create a GUI spreadsheet application with a minimum amount of written code; the latter demo is good for testing because it does not require installation of the FPSpreadsheet packages. Step-by-step instructions on how _fpsctrls_ is made can be found in the [FSpreadsheet tutorial](<FPSpreadsheet_tutorial__Writing_a_mini_spreadsheet_application.md> "FPSpreadsheet tutorial: Writing a mini spreadsheet application").
    * **fpsgrid** and **fpsgrid_no_install** show the basic application of the [TsWorksheetGrid](<TsWorksheetGrid.md> "TsWorksheetGrid") without using the TsWorkbookSource component; the latter demo does not require installation of any FPSpreadsheet package.
    * **wikitablemaker** is a small application for creation of code to be used for tables on wiki pages. Type the data into a [TsWorksheetGrid](<TsWorksheetGrid.md> "TsWorksheetGrid") (or load an existing spreadsheet file), go to page "Code" to see the generated wiki code, click "Copy to clipboard" and paste the code into the wiki page.
  * **spready** is an extended application of the the entire library showing spreadsheet files with formatting, editing of cells, etc. Since it is a stand-alone application it has been moved to the folder _applications/spready' of the_ Lazarus Components and Code Library _._



  


## Code examples

### Opening an existing spreadsheet

To open a spreadsheet while specifying a particular format to use use `ReadFromFile` with two parameters: 
    
    
    MyWorkbook.ReadFromFile(AFileName, sfExcel5);
    

It is also possible to call `ReadFromFile` with only one parameter, the filename. Then the workbook will use the extension to auto-detect the file format. In case of the ambiguous extension _.xls_ (Excel 2-8) it will simply try various possibilities until one works. Although typical fingerprint byte patterns are checked now it is still possible that an exception will be raised for each incorrect format if run from the IDE at designtime; this does not occur at runtime. 
    
    
    MyWorkbook.ReadFromFile(AFileName);
    

### Writing a spreadsheet to file based on extension

Similar to the `ReadFromFile` routine, there is also a `WriteToFile` procedure to determine the spreadsheet's type based on the filename suffix. It uses the `GetFormatFromFileName` routine in the previous section's code, so the actual code is simple. However, it will always write files with a given extension using the latest format that uses that extension (e.g. Excel _.xls_ files will be written as `sfExcel8`), so if you want to write them in an earlier format, you have to use the base routine. 

As above, this code patches the _fpspreadsheet.pas_ unit. 
    
    
    procedure TsWorkbook.WriteToFile(const AFileName: string; const AOverwriteExisting: Boolean = False); overload;
    var SheetType: TsSpreadsheetFormat;
    begin
      if getFormatFromFileName(AFileName, SheetType) then
        WriteToFile(AFileName, SheetType, AOverwriteExisting)
      else raise Exception.Create(Format(
        '[TsWorkbook.WriteToFile] Attempted to save a spreadsheet by extension, but the extension %s is invalid.', [ExtractFileExt(AFileName)]));
    end;
    

### Converting between two spreadsheet formats
    
    
    program ods2xls;
     
    {$mode delphi}{$H+}
     
    uses
      Classes, SysUtils, 
      fpstypes, fpspreadsheet, fpsallformats, fpspreadsheet_pkg;
     
    const 
      INPUT_FORMAT = sfOpenDocument;
      OUTPUT_FORMAT = sfExcel8;
     
    var
      MyWorkbook: TsWorkbook;
      MyDir: string;
    begin
      // Initialization
      MyDir := ExtractFilePath(ParamStr(0));
     
      // Convert the spreadsheet
      MyWorkbook := TsWorkbook.Create;
      try
        MyWorkbook.ReadFromFile(MyDir + 'test.ods', INPUT_FORMAT);
        MyWorkbook.WriteToFile(MyDir + 'test.xls', OUTPUT_FORMAT);
      finally
        MyWorkbook.Free;
      end;
    end.
    

### Iterating through all worksheets
    
    
    var
      MyWorkbook: TsWorkbook;
      MyWorksheet: TsWorksheet;
      i: Integer;
    begin
      // Here load MyWorkbook from a file or build it
     
      for i := 0 to MyWorkbook.GetWorksheetCount() - 1 do
      begin
        MyWorksheet := MyWorkbook.GetWorksheetByIndex(i);
        // Do something with MyWorksheet
      end;
    

### Iterating through cells

The first idea is to use a simple **for-to** loop: 
    
    
    var
      MyWorksheet: TsWorksheet;
      col, row: Cardinal;
      cell: PCell;
    begin
      for row:=0 to MyWorksheet.GetLastRowIndex do
        for col := 0 to MyWorksheet.GetLastColIndex do
        begin
          cell := MyWorksheet.FindCell(row, col);
          WriteLn(MyWorksheet.ReadAsUTF8Text(cell));
        end;
    end;
    

`FindCell` initiates a search for a cell independently of the previously found cell. Cells, however, are are organized interally in a sorted tree structure, and each cell "knows" its previous and next neighbors. Moreover, `FindCell` wastes time on searching for non-existing cells in case of sparsely-occupied worksheets. In general, it is more efficient to use the **for-in** syntax which takes advantage of the internal tree structure by means of special **enumerators**. Note that there are also dedicated enumerators for searching along rows, columns or in cell ranges: 
    
    
    var
      MyWorksheet: TsWorksheet;
      cell: PCell;
    begin
      // Search in all cells
      for cell in Myworksheet.Cells do
        WriteLn(MyWorksheet.ReadAsText(cell));
    
      // Search in column 0 only
      for cell in MyWorksheet.Cells.GetColEnumerator(0) do
        WriteLn(MyWorksheet.ReadAsText(cell));
    
      // Search in row 2 only
      for cell in MyWorksheet.Cells.GetRowEnumerator(2) do
        WriteLn(MyWorksheet.ReadAsText(cell));
    
      // Search in range A1:C2 only (rows 0..1, columns 0..2)
      for cell in MyWorksheet.Cells.GetRangeEnumerator(0, 0, 1, 2) do
        WriteLn(MyWorksheet.ReadAsText(cell));
    end;
    

### Excel 5 example

Note: at least with fpspreadsheet from trunk (development version), this example requires (at least) Lazarus _avglvltree.pas, lazutf8.pas, asiancodepagefunctions.inc, asiancodepages.inc_ and _lconvencoding.pas_ (in the _$(LazarusDir)\components\lazutils\_ directory) 
    
    
    {
    excel5demo.dpr
    
    Demonstrates how to write an Excel 5.x file using the fpspreadsheet library
    
    You can change the output format by changing the OUTPUT_FORMAT constant
    
    AUTHORS: Felipe Monteiro de Carvalho
    }
    program excel5demo;
    
    {$mode delphi}{$H+}
    
    uses
      Classes, SysUtils, fpstypes, fpspreadsheet, fpsallformats, laz_fpspreadsheet;
    
    const 
      OUTPUT_FORMAT = sfExcel5;
    
    var
      MyWorkbook: TsWorkbook;
      MyWorksheet: TsWorksheet;
      MyFormula: TsRPNFormula;
      MyDir: string;
    begin
      // Initialization
      MyDir := ExtractFilePath(ParamStr(0));
    
      // Create the spreadsheet
      MyWorkbook := TsWorkbook.Create;
      try
        MyWorksheet := MyWorkbook.AddWorksheet('My Worksheet');
    
        // Write some number cells
        MyWorksheet.WriteNumber(0, 0, 1.0);
        MyWorksheet.WriteNumber(0, 1, 2.0);
        MyWorksheet.WriteNumber(0, 2, 3.0);
        MyWorksheet.WriteNumber(0, 3, 4.0);
    
        // Write the formula E1 = A1 + B1
        MyWorksheet.WriteFormula(0, 4, 'A1+B1');
    
        // Creates a new worksheet
        MyWorksheet := MyWorkbook.AddWorksheet('My Worksheet 2');
    
        // Write some string cells
        MyWorksheet.WriteText(0, 0, 'First');
        MyWorksheet.WriteText(0, 1, 'Second');
        MyWorksheet.WriteText(0, 2, 'Third');
        MyWorksheet.WriteText(0, 3, 'Fourth');
    
        // Save the spreadsheet to a file
        MyWorkbook.WriteToFile(MyDir + 'test' + STR_EXCEL_EXTENSION, OUTPUT_FORMAT);
      finally
        MyWorkbook.Free;
      end;
    end.
    

### Reading and writing of CSV files

CSV files (CSV = comma-separated values) are plain text files without metadata. Therefore, additional information for correct reading of writing from/to a worksheet is required. The global record `CSVParams` makes available fundamental settings for this purpose: 
    
    
    type
      CSVParams: TsCSVParams = record    // W = writing, R = reading, RW = reading/writing
        SheetIndex: Integer;             // W: Index of the sheet to be written
        LineEnding: TsCSVLineEnding;     // W: Specification for line ending to be written
        Delimiter: Char;                 // RW: Column delimiter
        QuoteChar: Char;                 // RW: Character for quoting texts
        Encoding: String;                // RW: Encoding of file
        DetectContentType: Boolean;      // R: try to convert strings to content types
        NumberFormat: String;            // W: if empty write numbers like in sheet, otherwise use this format
        AutoDetectNumberFormat: Boolean; // R: automatically detects decimal/thousand separator used in numbers
        TrueText: String;                // RW: String for boolean TRUE
        FalseText: String;               // RW: String for boolean FALSE
        FormatSettings: TFormatSettings; // RW: add'l parameters for conversion
      end;
    

This record contains fields which are evaluated for reading only, writing only, or for both - see the attached comments. 

A common situation is to read a file using a number decimal separator which is different from the system's decimal separator: Suppose you are on a European system in which the decimal separator is a comma, but the csv file to be read originates from a machine which uses a decimal point. And suppose also, that the file contains tab characters as column separator instead of the default comma. In this case, simply set the `CSVParams.FormatSettings.DecimalSeparator` to `'.'`, and the `CSVParams.Delimiter` to `#9` (TAB character) before reading the file: 
    
    
    uses
      fpstypes, fpspreadsheet, fpscsv;
    var
      MyWorkbook: TsWorkbook;
    begin
      CSVParams.FormatSettings.DecimalSeparator := '.';
      CSVParams.Delimiter := #9;
      MyWorkbook := TsWorkbook.Create;
      try
        MyWorkbook.ReadFromFile('machine-data.csv', sfCSV);
      finally
        MyWorkbook.Free;
      end;
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If you want to give the user the possibility to interactively modify the `CSVParams` record have a look at the unit _scsvparamsform.pas_ of the _spready_ demo which provides a ready-to-use dialog.

### Converting a database to a spreadsheet

The easiest solution is to use the DatasetExport component. 

If you need to have more control over the process, use something like: 
    
    
    program db5xls;
     
    {$mode delphi}{$H+}
     
    uses
      Classes, SysUtils, 
      // add database units
      fpstypes, fpspreadsheet, fpsallformats;
     
    const OUTPUT_FORMAT = sfExcel5;
     
    var
      MyWorkbook: TsWorkbook;
      MyWorksheet: TsWorksheet;
      MyDatabase: TSdfDataset;
      MyDir: string;
      i, j: Integer;
    begin
      // Initialization
      MyDir := ExtractFilePath(ParamStr(0));
     
      // Open the database
      MyDatabase := TSdfDataset.Create;
      MyDatabase.Filename := 'test.dat';
      // Add table description here
      MyDatabase.Active := True;
    
      // Create the spreadsheet
      MyWorkbook := TsWorkbook.Create;
      MyWorksheet := MyWorkbook.AddWorksheet('My Worksheet');
     
      // Write the field names
      for i := 0 to MyDatabase.Fields.Count - 1 do
        MyWorksheet.WriteText(0, i, MyDatabase.Fields[i].FieldName);
    
      // Write all cells to the worksheet
      MyDatabase.First;
      j := 0;
      while not MyDatabase.EOF do
      begin
        for i := 0 to MyDatabase.Fields.Count - 1 do
          MyWorksheet.WriteText(j + 1, i, MyDatabase.Fields[i].AsString);
    
        MyDatabase.Next;
        Inc(j);
      end;
    
      // Close the database
      MyDatabase.Active := False;
      MyDatabase.Free;
    
      // Save the spreadsheet to a file
      MyWorkbook.WriteToFile(MyDir + 'test' + STR_EXCEL_EXTENSION, OUTPUT_FORMAT);
      MyWorkbook.Free;
    end.
    

### Converting a large database table to a spreadsheet using virtual mode

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The example program in _examples\db_import_export_ shows a demonstration of using virtual mode to export datasets to spreadsheet files.

We want to write a large database table to a spreadsheet file. The first row of the spreadsheet is to show the field names in bold type face and with a gray background. 

Normally, FPSpreadsheet would load the entire representation of the spreadsheet into memory, so we'll use virtual mode to minimize memory usage. 
    
    
    type
      TDataProvider = class;
    
    var
      MyWorkbook: TsWorkbook;
      MyWorksheet: TsWorksheet;
      MyDatabase: TSdfDataset;
      MyDir: string;
      MyHeaderTemplateCell: PCell;
      DataProvider: TDataProvider;
    
    // Implement TDataProvider here - see below...
    
    begin
      // Initialization
      MyDir := ExtractFilePath(ParamStr(0));
     
      // Open the database
      MyDatabase := TSdfDataset.Create;
      try
        MyDatabase.Filename := 'test.dat';
        // Add table description here
        MyDatabase.Active := True;
    
        // Create the spreadsheet
        MyWorkbook := TsWorkbook.Create;
        try
          MyWorksheet := MyWorkbook.AddWorksheet('My Worksheet');
    
          // Create the template cell for the header line, we want the 
          // header in bold type-face and gray background color
          // The template cell can be anywhere in the workbook, let's just select cell A1.
          MyWorksheet.WriteFontStyle(0, 0, [fssBold]);
          MyWorksheet.WriteBackgroundColor(0, 0, scGray);
          // We'll need this cell again and again, so let's save the pointer to it in a variable
          MyHeaderTemplateCell := MyWorksheet.Find(0, 0);
     
          // Enter virtual mode
          MyWorkbook.Options := MyWorkbook.Options + [boVirtualMode];
    
          // Define number of columns - we want a column for each field
          MyWorksheet.VirtualColCount := MyDatabase.FieldCount;
    
          // Define number of rows - we want every record, plus 1 row for the title row
          MyWorksheet.VirtualRowCount := MyDatabase.RecordCount + 1;
    
          // Link the event handler which passes data from database to spreadsheet writer
          MyWorksheet.OnWriteCellData := @DataProvider.WriteCellData;
    
          // Write all cells to an Excel8 file
          // The data to be written are specified in the OnWriteCellData event handler.
          MyWorkbook.WriteToFile(MyDir + 'test.xls', sfExcel8);
    
        finally
          // Clean-up
          MyWorkbook.Free;
        end;
    
      finally
        // Close the database & clean-up
        MyDatabase.Active := False;
        MyDatabase.Free;
      end;
    
    end.
    

What is left is to write the event handler for `OnWriteCellData`. For the command-line program above we setup a particular data provider class (in a gui program the event handler can also be a method of any form): 
    
    
    type
      TDataProvider = class
        procedure WriteCellData(Sender: TsWorksheet; ARow, ACol: Cardinal; var AValue: variant; var AStyleCell: PCell);
      end;
    
    procedure TDataProvider.WriteCellData(Sender: TsWorksheet; ARow, ACol: Cardinal; AValue: variant; var: AStyleCell: PCell);
    begin
      // Let's handle the header row first:
      if ARow = 0 then begin
        // The value to be written to the spreadsheet is the field name.
        AValue := MyDatabase.Fields[ACol].FieldName;
        // Formatting is defined in the HeaderTemplateCell.
        AStyleCell := MyHeaderTemplateCell;
        // Move to first record
        MyDatabase.First;
      end else begin
        // The value to be written to the spreadsheet is the record value in the field corresponding to the column.
        // No special requirements on formatting --> leave AStyleCell at its default (nil).
        AValue := MyDatabase.Fields[ACol].AsVariant;
        // Advance database cursor if last field of record has been written
        if ACol = MyDatabase.FieldCount-1 then MyDatabase.Next;
      end;
    end;

---

_Source: [https://wiki.freepascal.org/FPSpreadsheet%3A_Examples](https://web.archive.org/web/20250418110154/https://wiki.freepascal.org/FPSpreadsheet%3A_Examples)_
