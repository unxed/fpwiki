# CSV

│ **English (en)** │  **[русский (ru)](<../ru/CSV.md> "CSV/ru")** │    
****

## Contents

  * 1 Overview
  * 2 CSV and SDF
  * 3 Implementations
    * 3.1 CsvDocument
    * 3.2 DelimitedText
      * 3.2.1 SDF format
    * 3.3 SDFDataset
      * 3.3.1 TSdfDataset and TFixedDataset sample files
      * 3.3.2 Using the datasets: example code
    * 3.4 Data Export
    * 3.5 Jan's CSV components
    * 3.6 ZMSQL



## Overview

CSV means Comma-Separated Values, and is a popular file format that is unfortunately not completely standardized. 

It is a text based file format with 

  * data fields separated by commas (or, in variants, other characters like tabs and semicolons)
  * there may or not be a header line that lists the field names
  * field data containing delimiters may be prohibited or enclosed by quotes (most commonly the double quote character)
  * line endings (#13 and/or #10) may or may not be allowed in field data



[RFC4180](<http://www.rfc-editor.org/rfc/rfc4180.txt>) tries to codify and standardize existing practice; it makes sense to conform to this standard when writing CSV data (and accept all RFC4180 data when reading). 

A different, Creativyst/Excel, CSV pseudo-format specification can be found [here](<http://www.creativyst.com/Doc/Articles/CSV/CSV01.htm#FileFormat>). 

A sample CSV snippet: 
    
    
    FirstName,Surname,DOB,Remarks
    Jim,Weston,19560818,"Also known as ""The Butcher"""
    Alice,Cooper,19760312,""
    

Apparently a header line is used here, as is quoting using double quotes. 

Spreadsheet packages such as Microsoft Excel and OpenOffice/LibreOffice Calc are able to export to and import from this format. However, as Microsoft Excel may interpret some fields such as date fields differently depending on a user's OS locale, it may pay to find alternative ways of transferring data (e.g. using the [FPSpreadsheet](<FPSpreadsheet.md> "FPSpreadsheet") code). 

## CSV and SDF

Delphi (and Free Pascal) have a very similar (but not identical) format, SDF. See [SDF](<SDF.md> "SDF") for more details. 

## Implementations

### CsvDocument

[CsvDocument](<CsvDocument.md> "CsvDocument") is a robust implementation of both the RFC 4180 CSV format and the alternative Creativyst/Excel CSV format. It offers both line-based and document-based access. Recommended for use with FPC/Lazarus. See [CsvDocument](<CsvDocument.md> "CsvDocument"). 

### DelimitedText

TStringList offers the DelimitedText property. This parses a line of text out into separate fields. Note, however, that DelimitedText is supposed to be in SDF, a Delphi-specific format that is very much like CSV, but does not conform with RFC4180 completely. 

Tips: when reading CSV data, set the _StrictDelimiter_ property to true. 

When writing out CSV data, set _StrictDelimiter_ to false and output the _DelimitedText_ property. One oddity is that e.g. tab characters are removed when writing out data using _StrictDelimiter:=false_

#### SDF format

See [SDF format](<SDF.md> "SDF")

### SDFDataset

Free Pascal offers the [TSdfDataSet](<TSdfDataSet.md> "TSdfDataSet"), which ~~stores data in SDF format.~~ Note: FPC 2.6.x and earlier store sdfdataset data in a format that is not completely CSV or completely SDF compatible. 

As indicated, [SDF](<SDF.md> "SDF") differs from CSV. Depending on the flavour of CSV, this format may be close enough to what a reading application expects to function. If you need an RFC4180 compliant CSV format, use [TCSVDataSet](<TCSVDataSet.md> "TCSVDataSet") instead. 

#### TSdfDataset and TFixedDataset sample files

Sample SDF file 

    Below is a sample database for TSdfDataset. Note that the first line has the names of the fields and that we are using commas as separators:
    
    
    ID,NAMEEN,NAMEPT,HEIGHT,WIDTH,PINS,DRAWINGCODE
    1,resistor,resistor,1,1,1,LINE
    2,capacitor,capacitor,1,1,1,LINE
    3,transistor npn,transistor npn
    

  * Sample TFixedDataset file



    Each record occupies a fixed amount of space, and if the field is smaller, spaces should be used to fill the remaining size.
    
    
    Name = 15 chars; Surname = 15 chars; e_mail = 20 chars;
    

Piet Pompies piet@pompies.net 

#### Using the datasets: example code

Sometimes it is useful to create the dataset and work with it completely in code, and the following code will do exactly this. Note some peculiarities of TSdfDataset/TFixedDataset: 

  * The lines in the database can have a maximum size of about 300. A fix is being researched.
  * It is necessary to add the field definitions. Some datasets are able to fill this information alone from the database file.
  * One should set FirstLineAsSchema to true, to indicate that the first line includes the field names and positions.
  * The Delimiter property holds the separator for the fields. It will not be possible to use this char in strings in the database. Similarly it will currently not be possible to have line endings in the database because they mark the change between records. It's possible to overcome this by substituting the needed comma or line ending with another rarely used char, like #. When showing the data on screen all # chars could be converted to line endings and the inverse when storing data back to the database. The ReplaceString routine is useful here.


    
    
    uses sdfdata, db;
    
    constructor TComponentsDatabase.Create;
    var
      FDataset: TSdfDataset;
    begin
      inherited Create;
    
      FDataset := TSdfDataset.Create(nil);
      FDataset.FileName := vConfigurations.ComponentsDBFile;
      FDataset.FileMustExist := false; //don't require existing csv file
    
      // Not necessary with TSdfDataset:
      //  FDataset.TableName := STR_DB_COMPONENTS_TABLE;
      //  FDataset.PrimaryKey := STR_DB_COMPONENTS_ID;
    
      // Adds field definitions
      FDataset.FieldDefs.Add('ID', ftString);
      //schema addition is needed so that sdfdataset writes out the csv header correctly:
      FDataset.Schema.Add('ID'); 
      FDataset.FieldDefs.Add('NAMEEN', ftString);
      FDataset.Schema.Add('NAMEEN'); 
      FDataset.FieldDefs.Add('NAMEPT', ftString);
      FDataset.Schema.Add('NAMEPT'); 
      FDataset.FieldDefs.Add('HEIGHT', ftString);
      FDataset.Schema.Add('HEIGHT'); 
      FDataset.FieldDefs.Add('WIDTH', ftString);
      FDataset.Schema.Add('WIDTH'); 
      FDataset.FieldDefs.Add('PINS', ftString);
      FDataset.Schema.Add('PINS'); 
      FDataset.FieldDefs.Add('DRAWINGCODE', ftString);
      FDataset.Schema.Add('DRAWINGCODE'); 
    
      // Necessary for TSdfDataset
      FDataset.Delimiter := ',';
      FDataset.FirstLineAsSchema := True;
    
      FDataset.Active := True;
    
      // Sets the initial record
      CurrentRecNo := 1;
      FDataset.First;
    end;
    

When using [TSdfDataSet](<TSdfDataSet.md> "TSdfDataSet") directly be aware that RecNo, although it is implemented, does not work as a way to move through the dataset whether reading or writing records. The standard navigation routines like First, Next, Prior and Last work as expected, so you need to use them rather than RecNo. 

If you are used to using absolute record numbers to navigate around a database you can implement your own version of RecNo. Declare a global longint variable called CurrentRecNo which will hold the current RecNo value. Remember that this variable will have the same convention as RecNo, so the first record has number 1 (it is not zero-based). After activating the database initialize the database to the first record with TSdfDataset.First and set CurrentRecNo := 1. 

Example code: 
    
    
    {@@
      Moves to the desired record using TDataset.Next and TDataset.Prior
      This avoids using TDataset.RecNo which doesn't navigate reliably in any dataset.
    
      @param AID Indicates the record number. The first record has number 1
    }
    procedure TComponentsDatabase.GoToRec(AID: Integer);
    begin
      // We are before the desired record, move forward
      if CurrentRecNo < AID then
      begin
        while (not FDataset.EOF) and (CurrentRecNo < AID) do
        begin
          FDataset.Next;
          FDataset.CursorPosChanged;
          Inc(CurrentRecNo);
        end;
      end
      // We are after the desired record, move back
      else if CurrentRecNo > AID  then
      begin
        while (CurrentRecNo >= 1) and (CurrentRecNo > AID) do
        begin
          FDataset.Prior;
          FDataset.CursorPosChanged;
          Dec(CurrentRecNo);
        end;
      end;
    end;
    

### Data Export

Free Pascal/Lazarus database export functionality (e.g. TCSVExporter on the Data Export tab - requires installation of the [lazdbexport](<lazdbexport.md> "lazdbexport") package) offers CSV export functionality for [datasets](<TDataSet.md> "TDataSet"). 

### Jan's CSV components

See [JCSV (Jans CSV Components)](<JCSV_\(Jans_CSV_Components\).md> "JCSV \(Jans CSV Components\)"). 

### ZMSQL

[ZMSQL](<ZMSQL.md> "ZMSQL") stores data in semicolon-delimited files (using SDF?).

---

_Source: [https://wiki.freepascal.org/CSV](https://web.archive.org/web/20240803130231/https://wiki.freepascal.org/CSV)_
