# FPReport Usage

│ **English (en)** │    
****

On this page we'll dissect how to create a simple report in code. As a starting point, we take the dataset demo report from the Free Pascal demo application. 

## Contents

  * 1 Owners & Parent-Child structure
  * 2 Creating the report
    * 2.1 Providing data to the report
    * 2.2 Adding a page
    * 2.3 Adding bands to a page
    * 2.4 The loop data
    * 2.5 Expressions
      * 2.5.1 Builtin Variables
      * 2.5.2 Builtin Expressions
    * 2.6 Image Support
  * 3 Running the report
  * 4 Rendering (or exporting) the report
  * 5 Complete Code Example
    * 5.1 Requirements
    * 5.2 Source Code
  * 6 Loading and saving a report from/to file
    * 6.1 Manually loading and saving
    * 6.2 The easy way: Using TFPJSONReport
    * 6.3 Data management
  * 7 Back to main FPReport page



## Owners & Parent-Child structure

All report elements (pages, bands, printable elements) are TComponent descendents. They are organised in a parent-child relation: 

  * The pages are children of the report
  * The bands are children of a page
  * The printable elements are children of a band.



The owner (as in TComponent) of the various elements is not important for the report structure, but if the owner is the natural parent of a newly created component, then the parent will be set automatically. 

## Creating the report

Most fpReport related classes are defined in a single unit, the **fpreport** unit. So make sure you add that to your uses clause. 
    
    
    uses
      SysUtils
      ,fpreport           // for most FPReport classes
      ,dbf                // for TDBF (database) access
      ,fpreportdb         // for the TFPReportDatasetData class
      ,fpttf              // for access to gTTFontCache singleton
      {$IFDEF ExportPDF}
      ,fpreportpdfexport  // for access to the PDF Report exporter class
      {$ENDIF}
      {$IFDEF ExportAggPas}
      ,fpreport_export_aggpas // for access to fpGUI's fpReport AggPas exporter class
      {$ENDIF}
      {$IFDEF ExportFPImage}
      ,fpreportfpimageexport  // for access to the FPImage exporter class
      {$ENDIF}
      ;
    

Everything starts with a report component: 
    
    
    PaperManager.RegisterStandardSizes; // register paper types for fpReport
    rpt := TFPReport.Create(nil); // if your Application object is a TComponent decendant, use Self instead of nil.
    rpt.Author := 'Graeme Geldenhuys';
    rpt.Title := 'FPReport Demo 8 - Datasets';
    

The **Author** and **Title** properties can be used in expressions in the report. 

### Providing data to the report

Every report needs a data loop to be able to render itself. The data loop is a descendent of **TFPReportData** , there exist several pre-defined data loops. 

One of the possible data loops is based on a dataset, **TFPReportDatasetData** \- defined in the **fpreportdb** unit, so remember to add that to your uses clause. It has a **Dataset** property which must be set to the dataset that provides the data to the report. The loop will run over all records in the dataset, and all fields in the dataset will be available for use in expressions used in the report. 

The following code creates a data loop component, and assigns a **TDBF** dataset to it. Note that the TDBF class is defined in the **dbf** unit. 
    
    
    var
      lDataSet: TDBF;
      lReportData: TFPReportDatasetData;
    begin
      lReportData := TFPReportDatasetData.Create(nil);  // Same as previous code example. Use Self
      lDataSet := TDBF.Create(nil);                     // or Nil, depending on your Application object.
      lDataSet.TableName := 'test.dbf';
      lReportData.DataSet := lDataSet;
    

The **lReportData** component can then be used in the report structure. 

### Adding a page

Every report needs at least one page. 

The following page is owned by the report, so it will be added automatically to the pages of the report. Once created, a page size must be set (a set of standard sizes is available), this is done through the PageSize.PaperName property. If the name is known to the global paper manager factory, then the sizes will be set automatically from the name. 
    
    
    p := TFPReportPage.Create(rpt);
    p.Orientation := poPortrait;
    p.PageSize.PaperName := 'A4';
    

After the page name was set, the margins can be set. The measurements are in millimeter: 
    
    
    { page margins }
    p.Margins.Left := 30;
    p.Margins.Top := 20;
    p.Margins.Right := 30;
    p.Margins.Bottom := 20;
    

A page can have a font. This font is then used as the default for all the bands on the page. Likewise, all elements on a band will use the font of the band by default. Note that the font name is the PostScript name of a TTF font file. 
    
    
    p.Font.Name := 'LiberationSans';
    

When the report must be rendered, the layouting engine will run the data loop of the page, and repeat the page as often as is needed to fit the data. 

Therefor, a page must have a data loop associated with it. 
    
    
    p.Data := lReportData;
    

### Adding bands to a page

Now, the page must be filled with some content. This means adding several bands to the report page. 

A report title will be printed once, at the start of the report: 
    
    
    TitleBand := TFPReportTitleBand.Create(p);
    TitleBand.Layout.Height := 40;
    

The width of the band must not be set, it is calculated automatically from the page width, the number of columns and the margins of the page. 

A band by itself is not very useful, it serves only as a placeholder for some printable elements. 

For the title page, we'll add a simple static text as the page title. All text (dynamic or static) must be added using a **TFPReportMemo** component: 
    
    
      Memo := TFPReportMemo.Create(TitleBand);
      Memo.Layout.Left := 5;
      Memo.Layout.Top := 0;
      Memo.Layout.Width := 140;
      Memo.Layout.Height := 15;
    

The layout of the memo is relative to the band, and determines where the memo will be positioned. 

The text of the memo, and the internal formatting can be specified using the **Text** and **TextAlignment** properties: 
    
    
      Memo.Text := 'Dataset Demo';
      Memo.TextAlignment.Vertical := tlCenter;
      Memo.TextAlignment.Horizontal := taCentered;
    

Finally, the font and font size can be set: 
    
    
      Memo.UseParentFont := False;
      Memo.Font.Color := TFPReportColor($000080);
      Memo.Font.Size := 24;
    

The color is a RRGGBB value (Red/Green/Blue). Alpha channel support is not yet available. Several pre-defined values are defined in the FPReport unit. 

### The loop data

The report title is printed only once, but normally a report will have a band that is printed for each record in the report loop. For this, a data band (**TFPReportDataBand**) must be added to the report: 
    
    
    DataBand := TFPReportDataBand.Create(p);
    DataBand.Layout.Height := 30;
    DataBand.Data:= lReportData;
    

This band will be repeated for every record in the data loop. 

As noted above, any text must be printed with a memo. This is also true for data from the data loop. The following memo will print the name field from the dataset, prepended with the literal text **"Name: "**
    
    
      Memo := TFPReportMemo.Create(DataBand);
      Memo.Layout.Left := 30;
      Memo.Layout.Top := 0;
      Memo.Layout.Width := 50;
      Memo.Layout.Height := 5;
      Memo.Text := 'Name: [name]';
    

### Expressions

Dynamic text is obtained by intermixing static text and expressions. 

The expressions are anything that is between square brackets **[]**. 

  
The **TFPExpressionParser** expression engine is used to calculate the data. 

That means that Expressions are much like Pascal expressions. An expression: 

  * Is typed (string, integer, float, datetime)
  * can contain calculations on these types.
  * can contain report variables
  * can use data fields from the report data
  * Can use any of the pre-defined functions available from fpexprpars.



#### Builtin Variables

This variables are builtin in fpreport and are always defined 

  * **[TODAY]**
  * **[AUTHOR]**
  * **[TITLE]**



#### Builtin Expressions

This expression are builtin in fpreport and are always defined 

  * **[RecNo]**
  * **[PageNo]** Actual pagenumber
  * **[PageCount]** Maximum pages (Hint: need TwoPass activated to work)
  * **[ColNo]** Actual coloumn
  * **[PageNoPerDesignerPage]**
  * **[InRepeatedGroupHeader]**
  * **[InIntermediateGroupFooter]**
  * **[IsOverflowed]**
  * **[IsGroupDetailPrinted]**
  * **[FieldIsNull]**



### Image Support

If the dataset contains a blob field with an image, then this can also be printed: 
    
    
      Image := TFPReportImage.Create(DataBand);
      Image.Layout.Top := 0;
      Image.Layout.Left := 10;
      Image.Layout.Height := 20;
      Image.Layout.Width := 14.8;
      Image.FieldName := 'Photo';
      Image.Stretched := True;
    

It is sufficient to set the FieldName property to the name of the field containing the image data. 

A fixed image can also be added, if so desired: 
    
    
      Image := TFPReportImage.Create(TitleBand);
      Image.Layout.Left := 0;
      Image.Layout.Top := 0;
      Image.Layout.Width := 40;
      Image.Layout.Height := 30;
      Image.LoadFromFile('company-logo.png');
      Image.Stretched := True;
    

## Running the report

To layout the report, the RunReport method can be used: 
    
    
    { specify what directories should be used to find TrueType fonts }
    gTTFontCache.SearchPath.Add(cFCLReportDemosLocation + '/fonts/');
    gTTFontCache.BuildFontCache;
    
    rpt.RunReport;
    

This will create the report in memory. 

It is not visible on screen, it is not saved to file. 

## Rendering (or exporting) the report

To actually view the report, it must be rendered or exported. A preview of the report is also an export. 

Various exporters exist: 

  * [PDF](<PDF.md> "PDF") exporter
  * FPImage exporter (no sub-pixel rendering)
  * AggPas exporter (sub-pixel rendering and anti-aliasing). It is recommended to use the [AggPas](<https://github.com/graemeg/fpGUI/tree/develop/src/corelib/render/software>) code included in the [fpGUI](<fpGUI.md> "fpGUI") code repository.
  * HTML exporter
  * LCL canvas exporter, used as the basis for the LCL Preview exporter
  * [fpGUI](<fpGUI.md> "fpGUI") canvas exporter, used as the basis for the fpGUI Preview exporter



Thus, previewing or saving a layouted report is just a matter of creating the correct exporter, and calling the report **RenderReport** method: 
    
    
    RptExporter := TFPReportExportPDF.Create(nil); // as before, use Self or Nil based on Application class
    rpt.RenderReport(RptExporter);
    

## Complete Code Example

### Requirements

This example can be compiled with FPC 2.6.4 or later. 

For this code to compile, you need to specify the following Unit Search Paths: 

  * /data/devel/fpc-3.1.1/src/packages/fcl-report/src/
  * ../../fpgui/src/fpreport
  * ../../fpgui/src/corelib/render/software



The first search path is to the **fcl-report** directory from FPC Trunk. 

The second search path (optional) is to the AggPas exporter as defined in the [fpGUI](<fpGUI.md> "fpGUI") code repository. 

The third search path (optional) is to the AggPas library source code, as found in the fpGUI code repository. 

You also need to make sure the constant **cFCLReportDemosLocation** defined in the source code is pointing to the correct fcl-report/demo/ directory as it is on your own system. 

### Source Code
    
    
    program project1;
    
    {$mode objfpc}{$H+}
    
    { Which exporter do you want to use. Select any one or all. If you enable the AggPas
      exporter, remember to add the extra Unit Search Paths to your project settings. }
    {$define ExportPDF}
    {$define ExportFPImage}
    {$define ExportAggPas}
    
    uses
      SysUtils
      ,fpreport           // for most FPReport classes
      ,dbf                // for TDBF (database) access
      ,fpreportdb         // for the TFPReportDatasetData class
      ,fpttf              // for access to gTTFontCache singleton
      {$IFDEF ExportPDF}
      ,fpreportpdfexport  // for access to the PDF Report exporter class
      {$ENDIF}
      {$IFDEF ExportAggPas}
      ,fpreport_export_aggpas // for access to fpGUI's fpReport AggPas exporter class
      {$ENDIF}
      {$IFDEF ExportFPImage}
      ,fpreportfpimageexport  // for access to the FPImage exporter class
      {$ENDIF}
      ;
    
    const
      cFCLReportDemosLocation = '/data/devel/fpc-3.1.1/src/packages/fcl-report/demos/';
    
    var
      rpt: TFPReport;
      lDataSet: TDBF;
      lReportData: TFPReportDatasetData;
      p: TFPReportPage;
      TitleBand: TFPReportTitleBand;
      Memo: TFPReportMemo;
      DataBand: TFPReportDataBand;
      Image: TFPReportImage;
      RptExporter: TFPReportExporter;
    begin
      RptExporter := nil;
      if not FileExists(cFCLReportDemosLocation + 'test.dbf') then
      begin
        writeln('Unable to find database file <' + cFCLReportDemosLocation + 'test.dbf>');
        writeln('Please run the fcl-report''s dataset demo at least once.');
        writeln('');
        exit;
      end;
    
      // ***** Creating the report *****
      PaperManager.RegisterStandardSizes;
      rpt:=TFPReport.Create(nil);
      rpt.Author := 'Graeme Geldenhuys';
      rpt.Title := 'FPReport Demo 8 - Datasets';
      try
        // ***** Providing data to the report *****
        lReportData := TFPReportDatasetData.Create(nil);
        lDataSet := TDBF.Create(nil);
        lDataSet.TableName := cFCLReportDemosLocation + 'test.dbf';
        lReportData.DataSet := lDataSet;
    
        // ***** Adding a page *****
        p := TFPReportPage.Create(rpt);
        p.Orientation := poPortrait;
        p.PageSize.PaperName := 'A4';
    
        { page margins }
        p.Margins.Left := 30;
        p.Margins.Top := 20;
        p.Margins.Right := 30;
        p.Margins.Bottom := 20;
    
        { page's default font }
        p.Font.Name := 'LiberationSans';  // this is the PostScript name of the TTF font
    
        { assign the data for the page data loop }
        p.Data := lReportData;
    
        // ***** Adding bands to a page *****
        TitleBand := TFPReportTitleBand.Create(p);
        TitleBand.Layout.Height := 40;
    //    TitleBand.Frame.Shape := fsRectangle;
    //    TitleBand.Frame.BackgroundColor := clYellow;
    
        Memo := TFPReportMemo.Create(TitleBand);
        Memo.Layout.Left := 5;
        Memo.Layout.Top := 0;
        Memo.Layout.Width := 140;
        Memo.Layout.Height := 15;
    
        Memo.Text := 'Dataset Demo';
        Memo.TextAlignment.Vertical := tlCenter;
        Memo.TextAlignment.Horizontal := taCentered;
    
        Memo.UseParentFont := False;
        Memo.Font.Color := TFPReportColor($000080);
        Memo.Font.Size := 24;
    
        // ***** The loop data *****
        DataBand := TFPReportDataBand.Create(p);
        DataBand.Layout.Height := 30;
        DataBand.Data:= lReportData;
    //    DataBand.Frame.Shape := fsRectangle;
    //    DataBand.Frame.BackgroundColor := clCream;
    
        Memo := TFPReportMemo.Create(DataBand);
        Memo.Layout.Left := 30;
        Memo.Layout.Top := 0;
        Memo.Layout.Width := 50;
        Memo.Layout.Height := 5;
        Memo.Text := 'Name: [name]';
    
        // ***** Image Support *****
        Image := TFPReportImage.Create(DataBand);
        Image.Layout.Top := 0;
        Image.Layout.Left := 10;
        Image.Layout.Height := 20;
        Image.Layout.Width := 14.8;
        Image.FieldName := 'Photo';
        Image.Stretched := True;
    
        Image := TFPReportImage.Create(TitleBand); // note this one is placed on the TitleBand
        Image.Layout.Left := 0;
        Image.Layout.Top := 0;
        Image.Layout.Width := 40;
        Image.Layout.Height := 30;
        Image.LoadFromFile(cFCLReportDemosLocation + 'pictures/woman01.png');
        Image.Stretched := True;
    
        // ***** Running the report *****
        { specify what directories should be used to find TrueType fonts }
        gTTFontCache.SearchPath.Add(cFCLReportDemosLocation + '/fonts/');
        gTTFontCache.BuildFontCache;
    
        rpt.RunReport;
    
        // ***** Rendering (or exporting) the report *****
        {$IFDEF ExportPDF}
        if Assigned(RptExporter) then
          FreeAndNil(RptExporter);
        RptExporter := TFPReportExportPDF.Create(nil);
        rpt.RenderReport(RptExporter);
        {$ENDIF}
        {$IFDEF ExportAggPas}
        if Assigned(RptExporter) then
          FreeAndNil(RptExporter);
        RptExporter := TFPReportExportAggPas.Create(nil);
        TFPReportExportAggPas(RptExporter).BaseFileName := ApplicationName + '-aggpas-%.2d.png';
        rpt.RenderReport(RptExporter);
        {$ENDIF}
        {$IFDEF ExportFPImage}
        if Assigned(RptExporter) then
          FreeAndNil(RptExporter);
        RptExporter := TFPReportExportfpImage.Create(nil);
        TFPReportExportfpImage(RptExporter).BaseFileName := ApplicationName + '-fpimage-.png';
        rpt.RenderReport(RptExporter);
        {$ENDIF}
    
      finally
        // Freeing all objects we used
        FreeAndNil(RptExporter);
        FreeAndNil(rpt);
        FreeAndNil(lReportData);
        FreeAndNil(lDataset);
      end;
    end.
    

## Loading and saving a report from/to file

As explained in the introductory page [FPReport](<FPReport.md> "FPReport"), the **TFPReport** class by itself does not have a concept of files. It only knows a streamer (**TFPReportStreamer**). FPC ships a JSON streamer **TFPReportJSONStreamer** , there is also an older XML streamer, currently untested). 

### Manually loading and saving

The following code shows how to use a TFPReportJSONStreamer to save a report to file: 
    
    
    Var
      J : TFPReportJSONStreamer;
      F : TFileStream;
      S : TJSONStringType;
    begin
      F:=Nil;
      J:=TFPReportJSONStreamer.Create(Self);
      try
        FReport.WriteElement(J);
        F:=TFileStream.Create('txt2pdf.fpr',fmCreate);
        S:=J.JSON.FormatJSON();
        F.WriteBuffer(S[1],Length(S));
      finally  
        F.Free;
        J.Free;
      end;
    end;
    

And the following code loads it again from file: 
    
    
    procedure TPrintApplication.LoadReportDesign;
    
    Var
      J : TFPReportJSONStreamer;
      F : TFileStream;
      O : TJSONObject;
    
    begin
      J:=Nil;
      F:=TFileStream.Create('txt2pdf.fpr',fmOpenRead);
      try
        O:=GetJSON(F) as TJSONObject;
        J:=TFPReportJSONStreamer.Create(Self);
        J.JSON:=O;
        J.OwnsJSON:=True;
        FReport.ReadElement(J);
      finally
        F.Free;
        J.Free;
      end;
    end;
    

### The easy way: Using TFPJSONReport

The _fpjsonreport_ unit contains the **TFPJSONReport** class. It combines the JSON streamer and TFPReport, and makes it easier to load/save The _fpjsonreport_ unit contains a **TFPJSONReport** descendent which does all the above with some simple methods: 
    
    
    TFPJSONReport = class(TFPReport)
      procedure LoadFromStream(const aStream: TStream);
      procedure SaveToStream(const aStream: TStream);
      Procedure LoadFromJSON(aJSON : TJSONObject); virtual;
      Procedure SavetoJSON(aJSON : TJSONObject); virtual;
      Procedure LoadFromFile(const aFileName : String);
      Procedure SaveToFile(const aFileName : String);
      Property DataManager : TFPCustomReportDataManager;
    end;
    

It also is able to save the JSON Design in a lazarus .lfm file, so if you want to use a report that stores it's design info in a .lfm file, use a **TJSONFPReport** in lazarus. 

### Data management

The **TFPReportDataManager** class can be used to create and manage data sets based on definitions found in the JSON file created by the standalone designer. 

(in fact the standalone designer uses the **TFPReportDataManager** to manage its data). 

When you use **TFPJSONReport** , you can set its **DataManager** property to an instance of **TFPReportDataManager**. 

When loading/saving the data, it will use the data manager to add and/ord interpret the extra information in the report JSON to create data sets. 

If a data set fails to open for some reason (no connection to the database or other things), the **LoadErrors** property (of type **TStrings**) will contain the error messages. 

The data manager uses a factory to create the necessary data sets. For this, it requires handlers to be registered in the executable. The following units with data handlers are available: 

  * **fpreportdatacsv** for CSV support.
  * **fpreportdatadbf** for dbf support.
  * **fpreportdatajson** for JSON data file support.
  * **fpreportdatasqldb** for SQLDB support.



You must make sure to include the units for the data formats that you need in your project file. If you fail to do so, you will get errors saying that an unknown data type is used. 

## Back to main FPReport page

  * [FPReport](<FPReport.md> "FPReport")

---

_Source: [https://wiki.freepascal.org/FPReport_Usage](https://web.archive.org/web/20230529000312/https://wiki.freepascal.org/FPReport_Usage)_
