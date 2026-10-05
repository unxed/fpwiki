# CsvDocument

│ **English (en)** │  **[русский (ru)](<../ru/CsvDocument.md> "CsvDocument/ru")** │    
****

## Contents

  * 1 About
  * 2 Author
  * 3 Contributors
  * 4 License
  * 5 Dependencies
  * 6 Availability/Download
  * 7 Change Log
    * 7.1 Version 0.1 (2010-01-22)
    * 7.2 Version 0.2 (2010-05-31)
    * 7.3 Version 0.3 (2011-01-14)
    * 7.4 Version 0.4 (2011-05-10)
    * 7.5 Version 0.5 (2014-10-25)
    * 7.6 Since move to FCL (2015-04-04)
  * 8 Installation
  * 9 Usage examples
    * 9.1 Extracting a specific cell from a CSV document
    * 9.2 Loading a csv document into a StringGrid
  * 10 Contact
  * 11 See Also



### About

CsvDocument library is a unit contaning set of classes for [CSV files](<http://en.wikipedia.org/wiki/Comma-separated_values>) handling. The library was created to exchange data with OpenOffice Calc / MS Office Excel using CSV as intermediate format. 

Its main characteristics are: 

  * Random read/write access to CSV fields based on object model (TCSVDocument class). This requires preloading CSV file content into memory but allows to edit any field at any moment.
  * Serial access (field-by-field) parsing of CSV files using TCSVParser class. It helps to read CSV file content directly without preloading the whole file into memory.
  * Field-by-field building of CSV files using TCSVBuilder class. This helps to write CSV content directly to file and thus avoid using in-memory buffer. Implemented in version 0.4 and above.
  * CSV implementation is compatible with that of OpenOffice Calc / MS Office Excel. This means CSV files saved from Calc/Excel can be edited using CsvDocument library and vice versa.
  * Both UTF-8 encoding and Windows-xxx codepages can be used with the CsvDocument library. The library uses _string_ type for all string operations and will not do any encoding conversion for you. Keep in mind though that Excel does not support CSV files in UTF-8.
  * Support for line breaks embedded into CSV fields. It was one of the reasons to reinvent the wheel. OO Calc supports this feature as well, but MS Excel does not.
  * StringGrid-like field access with TCsvDocument.Cells[ACol, ARow]. Field access is safe: i.e. when you try to access non-existing field you do not get "Index out of bounds" exception, but get an empty string result. To distingush between empty and non-existing fields there are methods and properties like HasRow/HasCell and RowCount/ColCount.
  * Support for search in single row/column using IndexOfRow(AString, ACol) / IndexOfCol(AString, ARow). Implemented in version 0.3 and above.
  * No additional limits on field length, number of rows/fields, etc. other than performance, memory usage, _string_ type limit of 2 Gb and signed 32 bit integer value limit.
  * Compliance with RFC 4180 (<http://tools.ietf.org/html/rfc4180#section-2>) using default settings, as of version 0.4 and above.
  * Compliance with unofficial CSV specification (<http://www.creativyst.com/Doc/Articles/CSV/CSV01.htm#FileFormat>), as of version 0.3 and above. Requires setting IgnoreOuterWhitespace to True in version 0.4 and above.



### Author

Vladimir Zhirov 

### Contributors

Luiz Americo Pereira Camara 

Mattias Gaertner 

Reinier Olislagers 

### License

[Modified](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL.txt>)[LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL.txt>) (same as FPC RTL and Lazarus LCL). 

### Dependencies

The library is a single Pascal source file that only depends on FPC RTL and FCL. 

### Availability/Download

Since FPC 3.0, CSVDocument belongs to the FCL (folder _fcl-base/src_ , units _csvreadwrite.pp_ and _csvdocument.pp_) where it is actively maintained. 

An older version is available at Lazarus-CCR and kept for compatibility, but not maintained any more: <http://sourceforge.net/projects/lazarus-ccr/files/CsvDocument/CsvDocument%200.5/csvdocument-0.5.zip/download>

### Change Log

#### Version 0.1 (2010-01-22)

  * initial release.



#### Version 0.2 (2010-05-31)

  * fixed bug in line ending conversion.
  * fixed compilation with range checking on.
  * exposed QuoteCSVString function to simplify CSV generation without TCsvDocument class.
  * minor code cleanup.



#### Version 0.3 (2011-01-14)

  * CsvParser API changed: callback methods were replaced by ResetParser/ParseNextCell methods to allow using the library in pure functional programs. See example of new API usage in TCSVDocument.LoadFromStream.
  * CsvDocument API changed: TrimEmptyCells were renamed to RemoveTrailingEmptyCells.
  * support for trimming leading and trailing whitespace in fields. New TrimWhitespace property added. **Warning** : TrimWhitespace is enabled by default.
  * support for search in single row/column using IndexOfRow(AString, ACol) / IndexOfCol(AString, ARow).
  * performance improvements.
  * code cleanup.
  * added simple test suite.



#### Version 0.4 (2011-05-10)

  * replaced QuoteCSVString function with TCSVBuilder class. See example of its usage in TCSVDocument.SaveToStream.
  * renamed TrimWhitespace to IgnoreOuterWhitespace (to be consistent with newly introduced QuoteOuterWhitespace property).
  * support for equal column count throughout the document (required by RFC 4180). See new EqualColCountPerRow property.
  * changed default settings to RFC 4180 compliant ones (comma as Delimiter, double quote as QuoteChar, CRLF as line ending, ignoring outer whitespace off, equal column count on).
  * implemented conversion of line endings embedded into fields when parsing and building a CSV file. It helps to prevent mixed line endings in output file when using OS-specific line endings and working with the same file on multiple OSes.
  * moved CSV format settings to TCSVHandler class (an ancestor of TCSVParser, TCSVBuilder and TCSVDocument).
  * updated test suite for RFC 4180 compliance.
  * fixed minor annoyances in demo application.
  * included optimized ChangeLineEndings function by Mattias Gaertner.
  * speed optimizations, demo improvements and Lazarus package by Luiz Americo Pereira Camara.



#### Version 0.5 (2014-10-25)

  * fixed bug in TrimEmptyCells method, it was removing empty cells in the middle of a row (patch by Reinier Olislagers)
  * implemented MaxColCount property: the maximum number of columns found parsing a CSV file (patch by Reinier Olislagers)
  * commented procedures/functions/properties (based on patch by Reinier Olislagers)
  * fixes in demo application (updating view after loading CSV document, separate project files for Lazarus 0.9.30)



#### Since move to FCL (2015-04-04)

  * Please see the GIT commit notes in the FPC repository



### Installation

No "real" installation needed. Simply include unit `CsvDocument` and/or `csvreadwrite` in the `uses` section. 

### Usage examples

#### Extracting a specific cell from a CSV document
    
    
    uses 
      csvdocument;
    
    function ReadCSVCell(AFileName: String; ARow, ACol: Integer): String;
    var 
      CSVDoc:TCSVDocument;
    begin
      CSVDoc := TCSVDocument.Create;
      try
        CSVDoc.Delimiter := ';';
        CSVDoc.LoadFromFile(AFileName);
        Result := CSVDoc.Cells[ACol, ARow];
      finally
        CSVDoc.Free;
      end;
    end;
    

#### Loading a csv document into a StringGrid

This sample code uses `TCSVParser` from the `csvreadwrite` unit to load a CSV/TSV/semicolon-separated file into a stringgrid, similar to _StringGrid.LoadFromCSVFile_ but it supports all file format variations that `CSVDocument` supports. 

The advantage of using `TCSVParser` versus `TCSVDocument` is that you can e.g. parse only the first 20 rows and load them into the grid, which saves a lot of memory if dealing with large CSV files. 
    
    
    unit stringgridutil;
    { Use csvreadwrite to import csv data into stringgrid. Offers more robust support for CSV file variations than TStringGrid.LoadFromCSVFile }
    
    {$mode objfpc}{$H+} 
    interface
    
    uses
      Classes, SysUtils, Grids, csvreadwrite, LazFileUtils;
      // replace "csvreadwrite" by "csvdocument" if you use the out-dated version from CCR.
    
    procedure LoadGridFromCSVFile(Grid: TStringGrid;AFilename: string;
      ADelimiter:Char=','; WithHeader:boolean=true; AddRows:boolean=true);
    // Loads (quasi)CSV document in AFilename into Grid, using ADelimiter as
    // delimiter. If WithHeader is true, it won't import the first row.
    // If AddRows is true, it will add rows if the existing grid is too short.
    
    implementation
    
    procedure LoadGridFromCSVFile(Grid: TStringGrid;AFilename: string;
      ADelimiter:Char=','; WithHeader:boolean=true;AddRows:boolean=true);
    const
      DefaultRowCount=10; //Number of rows to show by default
    var
      FileStream: TFileStream;
      Parser: TCSVParser;
      RowOffset: integer;
    begin
      Grid.BeginUpdate;
      // Reset the grid:
      Grid.Clear;
      Grid.RowCount:=DefaultRowCount;
      Grid.ColCount:=6; //Vaguely sensible
      if not(FileExistsUTF8(AFileName)) then exit;
    
      Parser:=TCSVParser.Create;
      FileStream := TFileStream.Create(AFilename, fmOpenRead+fmShareDenyWrite);
      try
        Parser.Delimiter:=ADelimiter;
        Parser.SetSource(FileStream);
    
        // If the grid has fixed rows, those will not receive data, so we need to
        // calculate the offset
        RowOffset:=Grid.FixedRows;
        // However, if we have a header row in our CSV data, we need to
        // discount that
        if WithHeader then RowOffset:=RowOffset-1;
    
        while Parser.ParseNextCell do
        begin
          // Stop if we've filled all existing rows. Todo: check for fixed grids etc, but not relevant for our case
          if AddRows=false then
            if Parser.CurrentRow+1>Grid.RowCount then break; //VisibleRowCount doesn't seem to work.
    
          // Widen grid if necessary. Slimming the grid will come after import done.
          if Grid.Columns.Enabled then
          begin
            if Grid.Columns.VisibleCount<Parser.CurrentCol+1 then Grid.Columns.Add;
          end
          else
          begin
            if Grid.ColCount<Parser.CurrentCol+1 then Grid.ColCount:=Parser.CurrentCol+1;
          end;
    
          // If header data found, and a fixed row is available, set the caption
          if (WithHeader) and
            (Parser.CurrentRow=0) and
            (Parser.CurrentRow<Grid.FixedRows-1) then
          begin
            // Assign header data to the first fixed row in the grid:
            Grid.Columns[Parser.CurrentCol].Title.Caption:=Parser.CurrentCellText;
          end;
    
          // Actual data import into grid cell, minding fixed rows and header
          if Grid.RowCount<Parser.CurrentRow+1 then
            Grid.RowCount:=Parser.CurrentRow+1;
          Grid.Cells[Parser.CurrentCol,Parser.CurrentRow+RowOffset]:=Parser.CurrentCellText;
        end;
    
        // Now we know the widest row in the import, we can snip the grid's
        // columns if necessary.
        if Grid.Columns.Enabled then
        begin
          while Grid.Columns.VisibleCount>Parser.MaxColCount do
          begin
            Grid.Columns.Delete(Grid.Columns.Count-1);
          end;
        end
        else
        begin
          if Grid.ColCount>Parser.MaxColCount then
            Grid.ColCount:=Parser.MaxColCount;
        end;
    
      finally
        Parser.Free;
        FileStream.Free;
        Grid.EndUpdate;
      end;
    end;
    end.
    

### Contact

Please send bug reports and patches to the FPC gitlab bugtracker (<https://gitlab.com/freepascal.org/fpc/source/-/issues>). 

### See Also

  * [CSV](<CSV.md> "CSV")
  * [FPSpreadsheet](<FPSpreadsheet.md> "FPSpreadsheet")
  * [JCSV (Jans CSV Components)](<JCSV_\(Jans_CSV_Components\).md> "JCSV \(Jans CSV Components\)")

---

_Source: [https://wiki.freepascal.org/CsvDocument](https://web.archive.org/web/20250114060241/https://wiki.freepascal.org/CsvDocument)_
