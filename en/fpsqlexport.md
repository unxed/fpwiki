# TSQLExporter

**TSQLExporter** [![tsqlexporter.png](https://wiki.freepascal.org/images/e/e7/tsqlexporter.png)](</File:tsqlexporter.png>) is a database export component for use with a [TDataSet](<TDataSet.md> "TDataSet"). It may be found on the [Data Export tab](<Data_Export_tab.md> "Data Export tab") of the [Component Palette](<Component_Palette.md> "Component Palette") To export the contents of a TDataSet (for instance the result of a [TSQLQuery](<TSQLQuery.md> "TSQLQuery") correctly setup to link with a [TSQLConnector](<TSQLConnector.md> "TSQLConnector") and [TSQLTransaction](<TSQLTransaction.md> "TSQLTransaction")) also `FormatSettings.TableName` need be provided. 

In the example below a a [TSynEdit](<TSynEdit.md> "TSynEdit") is filled with exported contents of table `mytable`. 
    
    
    type
      myForm = class(TForm)
        btnExport: TButton;
        myConnector: TSQLConnector;
        myExporter: TSQLExporter;
        myQuery: TSQLQuery;
        myTransaction: TSQLTransaction;
        myScriptText: TSynEdit;
        mySyntax: TSynSQLSyn;
        procedure btnExportClick(Sender: TObject);
      private
        procedure ExportTable( const tblnam: String );
      end;
    
    implementation
    
    
    procedure myForm.btnExportClick(Sender: TObject);
    begin
      ExportTable( 'mytable' );
    end;
    
    
    procedure myForm.ExportTable( const tblnam: string );
    var
      ts: TStringStream;
    begin
      try
        ts := TStringStream.Create( '' );
        myTransaction.Database := myConnector;
        myQuery.Database := myConnector;
        myExporter.DataSet := myQuery;
    
        myQuery.SQL.Text := 'SELECT * FROM ' + tblnam;
        myQuery.Open();
    
        with myExporter.FormatSettings do begin
          TableName :=      tblnam;                 // exporter uses tablename in insert statements
          DateFormat :=     'YYYY-MM-DD';           // SQL date format
          DateTimeFormat := 'YYYY-MM-DD hh:mm:ss';  // 
        end;
    
        myExporter.ExportToStream( ts );
        myScriptText.Text := ts.DataString;  //show formatted script text
    
        myQuery.Close();
      finally
        ts.Free();
      end;
    end;

---

_Source: [https://wiki.freepascal.org/fpsqlexport](https://web.archive.org/web/20190825201921/https://wiki.freepascal.org/fpsqlexport)_
