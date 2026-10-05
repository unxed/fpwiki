# MS Access

│ **English (en)** │  **[español (es)](</MS_Access/es> "MS Access/es")** │  **[français (fr)](</MS_Access/fr> "MS Access/fr")** │  **[日本語 (ja)](</MS_Access/ja> "MS Access/ja")** │  **[русский (ru)](<../ru/MS_Access.md> "MS Access/ru")** │    
****  
  
---  
[**Databases portal**](<Portal_Databases.md> "Portal:Databases")  
References: 

  * [General info](<Databases.md> "Databases")
  * [Libraries](<Database_libraries.md> "Database libraries")
  * [Field types](<Database_field_type.md> "Database field type")
  * [Controls](<Data_Controls_tab.md> "Data Controls tab")
  * [FAQ](<Lazarus_DB_Faq.md> "Lazarus DB Faq")
  * [SQL how-to](<SqlDBHowto.md> "SqlDBHowto")
  * [Working With TSQLQuery](<Working_With_TSQLQuery.md> "Working With TSQLQuery")
  * [In-memory database applications](<How_to_write_in-memory_database_applications_in_Lazarus/FPC.md> "How to write in-memory database applications in Lazarus/FPC")

Tutorials/practical articles: 

  * [Overview](<Lazarus_Database_Overview.md> "Lazarus Database Overview")
  * [0 - Database set-up](<SQLdb_Tutorial0.md> "SQLdb Tutorial0")
  * [1 - Getting started](<SQLdb_Tutorial1.md> "SQLdb Tutorial1")
  * [2 - Editing](<SQLdb_Tutorial2.md> "SQLdb Tutorial2")
  * [3 - Queries](<SQLdb_Tutorial3.md> "SQLdb Tutorial3")
  * [4 - Data modules](<SQLdb_Tutorial4.md> "SQLdb Tutorial4")
  * [SQLdb Programming Reference](<SQLdb_Programming_Reference.md> "SQLdb Programming Reference")

Databases  


    [Advantage](<Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<mssqlconn.md> "mssqlconn") \- [Postgres](<postgres.md> "postgres") \- [Interbase](<Firebird.md> "Firebird") \- [Firebird](<Firebird.md> "Firebird") \- [Oracle](<Oracle.md> "Oracle") \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- [dBASE](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- MS Access \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
This page explains how to use a **Microsoft Access** database using [ODBC](<ODBCConn.md> "ODBCConn") and FPC's built-in SQLDB. 

  


## Contents

  * 1 MS Access specifics
    * 1.1 UsePrimaryKeyAsKey
    * 1.2 Get just-inserted autonumber primary key
    * 1.3 ODBC drivers
      * 1.3.1 Windows
      * 1.3.2 Unix/Linux
    * 1.4 Wildcards in SQL queries
  * 2 File-based DSN Instructions
    * 2.1 Set up the File DSN
      * 2.1.1 File DSN contents
    * 2.2 Configure your project to use file-based DSN
  * 3 System/User DSN
  * 4 DSN-less connection
  * 5 Example code
    * 5.1 Lazarus: data bound controls on forms
    * 5.2 Create a database programmatically
    * 5.3 Character set issues
  * 6 Example database
  * 7 See also



## MS Access specifics

### UsePrimaryKeyAsKey

In the **TSQLQuery** properties, you may need to set Set UsePrimaryKeyAsKey: False 

### Get just-inserted autonumber primary key

After inserting data, Access 2000 and higher supports getting the just-inserted autonumber primary key using this query: 
    
    
    SELECT @@IDENTITY
    

### ODBC drivers

#### Windows

There are 2 different [ODBC](<ODBCConn.md> "ODBCConn") drivers for Microsoft Access: 

  * the old driver that can only use .mdb format files, driver name is "Microsoft Access Driver (*.mdb)". Included in many Windows versions (since Windows 2000 up to and including Vista?); downloadable for older Windows versions as part of the MDAC components or Jet components. 32 bit only.
  * the new driver that can access both .mdb and .accdb files, driver name is "Microsoft Access Driver (*.mdb, *.accdb)". Downloadable as the "Microsoft Access Database Engine 2010 Redistributable"; available as 32 and 64 bit.



As usual, when using 32 bit Lazarus/FPC, use 32 bit ODBC drivers. When using 64 bit Lazarus/FPC, use 64 bit ODBC drivers. 

#### Unix/Linux

The [mdbtools project](<https://github.com/mdbtools/mdbtools>) offers limited support for MS Accesss. It includes an ODBC driver which might be used. Try installing packages like mdbtools mdbtools-dev mdbtools-gmdb 

At least on Debian, the ODBC driver name is "MDBTools". 

### Wildcards in SQL queries

Microsoft Access has a different wildcard syntax in SQL queries than other database systems. The following information is taken from a document _"Intermediate Microsoft Jet SQL for Access 2000"_ at [MSDN](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnacc2k/html/acintsql.asp>), but is no longer found at this place (some copies still exist in the web at third-party sites). Tested to be valid. 

Wildcard Character | Description   
---|---  
% (percent sign) | Matches any number of characters and can be used anywhere in the pattern string.   
_ (underscore) | Matches any single character and can be used anywhere in the pattern string.   
[] (square brackets) | Matches any single character within the list that is enclosed within brackets, and can be used anywhere in the pattern string.   
! (exclamation point) | Matches any single character not in the list that is enclosed within the square brackets.   
\- (hyphen) | Matches any one of a range of characters that is enclosed within the square brackets.   
  
Note that 

  * the `WHERE` condition in Access must be put in round parenthesis
  * strings must be single-quoted
  * dates must be formatted like `#yyyy-mm-dd#` (in year-month-day order and enclosed by '#')
  * field and table names with normally forbidden characters must be enclosed in square brackets.



Example: `SELECT * FROM [Birthday Table] WHERE ([Birth date] >= #2000-01-01#)`

## File-based DSN Instructions

A file DSN is simply where the connection settings are written to a file. The reason for having a file DSN is if you want to distribute a data source connection to multiple users on different systems without having to configure a DSN for each system. For instance, I can create a file DSN to a reporting database on my desktop. I can then send the file to my users. My users can save the file DSN to their hard drives and then point their reporting applications at the file DSN. 

If you want to use a file-based DSN with the SQLDB ODBC driver: 

### Set up the File DSN

  * Go to your [Data sources (ODBC)] at the control panel administrative tools.
  * Go to [File DSN] tab menu, Click Add, then Select <microsoft access driver>.
  * Next, browse to your current Lazarus project path to save your .DSN file, because that dsn file will contain the configuration needed to access your database file (.mdb).
  * Click Next, then Finish (you now have created a new .dsn file) of which will be use in your TODBCConnection [FileDSN].



#### File DSN contents

For reference, a file DSN that refers to an MS Access database **may** look something **like** this: 
    
    
    [ODBC]
    DRIVER=Microsoft Access Driver (*.mdb)
    UID=admin
    UserCommitSync=Yes
    Threads=3
    SafeTransactions=0
    PageTimeout=5
    MaxScanRows=8
    MaxBufferSize=2048
    FIL=MS Access
    DriverId=25
    DefaultDir=
    DBQ=YOUR_msaccess.mdb
    

### Configure your project to use file-based DSN

Usage of grids and other data bound controls is the same as for any other SQLDB connector, so that is not covered here. 

As you're using an ODBC connection, you should have a **TODBCConnection** object. 

TOBDCConnection Properties: 

FileDSN: the path+filename to the DSN file saved earlier, e.g. c:\mylazarus\project1\myFile.dsn Username: admin (or whatever username you need if you are using Access security) _this parameter may not be needed if no MS Access security is used_ Do not put any other entries in the properties. 

  


## System/User DSN

As explained in the [ODBC](<ODBCConn.md> "ODBCConn") article, you can also use system or user DSNs, where connection settings are defined in the ODBC control panel instead of stored in a file. 

The TODBCConnection should have DatabaseName: <name_of_your_DSN>

## DSN-less connection

As explained in the [ODBC](<ODBCConn.md> "ODBCConn") article, you can also create a DSN-less connection to your Access database where you can specify all connection parameters in code, something like: 
    
    
      //Notice we're using the new MS Access driver, we could probably use the old one too:
      conn.Driver:='Microsoft Access Driver (*.mdb, *.accdb)';
      conn.Params.Add('DBQ=c:\somedirectory\test.mdb');
      ... add whatever parameters you want/need...
    

Here is an example how to query a table in an Access mdb file using TODBCConnection, TSQLTransaction and TSQLQuery; the mdb file exists in the same folder as the project binary: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      //connection
      ODBCConnection1.Driver := 'Microsoft Access Driver (*.mdb, *.accdb)';
      ODBCConnection1.Params.Add('DBQ='+Application.Location+'test.mdb');      // path to mdb file required
      ODBCConnection1.Params.Add('Locale Identifier=1031');
      ODBCConnection1.Params.Add('ExtendedAnsiSQL=1');
      ODBCConnection1.Params.Add('CHARSET=ansi');
      ODBCConnection1.Connected := True;
      ODBCConnection1.KeepConnection := True;
         
      //transaction
      SQLTransaction1.DataBase := ODBCConnection1;
      SQLTransaction1.Action := caCommit;
      SQLTransaction1.Active := True;
         
      SQLQuery1.DataBase := ODBCConnection1;
      SQLQuery1.UsePrimaryKeyAsKey := False;
      SQLQuery1.SQL.Text := 'select * from Customers';
      SQLQuery1.Open;
    end;
    

## Example code

### Lazarus: data bound controls on forms

Working Source code sample:(Updated2_with_add_delete_update) <http://www.mediafire.com/file/ne1jx3zpnwzefq3/msaccesstest2.zip> _[Dead link - November 2020]_
    
    
    unit Unit1; 
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, odbcconn, sqldb, db, FileUtil, LResources, Forms, Controls,
      Graphics, Dialogs, DBGrids, DbCtrls, StdCtrls, Printers, PrintersDlgs;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        Button2: TButton;
        Button3: TButton;
        Button4: TButton;
        Datasource1: TDatasource;
        DBEdit1: TDBEdit;
        DBEdit2: TDBEdit;
        DBGrid1: TDBGrid;
        ODBCConnection1: TODBCConnection;
        PrintDialog1: TPrintDialog;
        SQLQuery1: TSQLQuery;
        SQLTransaction1: TSQLTransaction;
        procedure Button1Click(Sender: TObject);
        procedure Button2Click(Sender: TObject);
        procedure Button3Click(Sender: TObject);
        procedure Button4Click(Sender: TObject);
        procedure FormCloseQuery(Sender: TObject; var CanClose: boolean);
        procedure FormShow(Sender: TObject);
      private
        { private declarations }
        procedure PrintDbGrid(dbGrid:TdbGrid);
      public
        { public declarations }
      end; 
    
    var
      Form1: TForm1; 
    
    implementation
    
    { TForm1 }
    
    procedure TForm1.FormShow(Sender: TObject);
    begin
      //connection
      ODBCConnection1.FileDSN := ExtractFilePath(Application.ExeName) + 'file.dsn';
      ODBCConnection1.Connected := True;
      ODBCConnection1.KeepConnection := True;
    
      //transaction
      SQLTransaction1.DataBase := ODBCConnection1;
      SQLTransaction1.Action := caCommit;
      SQLTransaction1.Active := True; //not applied to ms access(false)
    
      SQLQuery1.DataBase := ODBCConnection1;
      SQLQuery1.UsePrimaryKeyAsKey := False;
    
      SQLQuery1.SQL.Text := 'select * from table1';
    
      // :id is the parameter for the field name id
      SQLQuery1.deleteSQL.Text := 'delete from table1 where id=:id';
      //:name is the parameter for the field name name
      SQLQuery1.updateSQL.Text := 'update table1 set name=:name where id=:id';
    
      DataSource1.DataSet := SQLQuery1;
      DBGrid1.DataSource := DataSource1;
      DBGrid1.ReadOnly := true;
    
      DBEdit1.DataField := 'id';
      DBEdit1.DataSource := DataSource1;
      DBEdit2.DataField := 'name';
      DBEdit2.DataSource := DataSource1;
    
      SQLQuery1.Open;
    end;
    
    function MulDiv(nNumber, nNumerator, nDenominator: Integer): Integer;
    begin
      Result:=Round(int64(nNumber)*int64(nNumerator)/nDenominator);
    end;
    procedure TForm1.PrintDbGrid(dbGrid:TdbGrid);
    const
      LeftMargin = 0.05;
      TopMargin = 0.05;
      BottomMargin = 0.05;
    var
      i: integer;
      x,y: integer;
    begin
      if PrintDialog1.Execute then
        begin
        Printer.BeginDoc;
        Printer.Canvas.Font.Size := 12;
    
        y := Round(TopMargin*Printer.PageHeight);
        dbGrid.DataSource.DataSet.First;
        while not dbGrid.DataSource.DataSet.Eof do
          begin
          x := Round(LeftMargin*Printer.PageWidth);
          for i := 0 to dbGrid.DataSource.DataSet.FieldCount-1 do
            begin
            printer.Canvas.TextOut(x,y,dbGrid.DataSource.DataSet.Fields[i].AsString);
            x := x + MulDiv(dbGrid.Columns[i].Width,72, dbGrid.Width);
            end;
          dbGrid.DataSource.DataSet.Next;
          y := y + printer.Canvas.TextHeight('A');
          if y > (1-TopMargin-BottomMargin)* Printer.PageHeight then
            begin
            y := Round(TopMargin*Printer.PageHeight);
            Printer.NewPage;
            end;
          end;
        Printer.EndDoc;
        end
        else
        Form1.caption := 'NO PRINTER INSTALLED';
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      PrintDbGrid(DBGrid1);
    end;
    
    procedure TForm1.Button2Click(Sender: TObject);
    begin
    if button2.Caption = 'new' then
    begin
      SQLQuery1.Insert;
      button2.Caption := 'save';
      exit
      end
      else
      begin
        if (dbedit1.Text = '') or (dbedit2.Text = '')
        then
        begin
        SQLQuery1.Cancel;
        end
        else
        begin
        if SQLQuery1.State = dsInsert then
           begin
           SQLQuery1.Post;
           SQLQuery1.ApplyUpdates;
           Form1.caption := 'ADDED';
           end;
        end;
      end;
    button2.Caption := 'new';
    end;
    
    procedure TForm1.Button3Click(Sender: TObject);
    begin
    if SQLQuery1.RecordCount>0 then
      begin
      SQLQuery1.Delete;
      SQLQuery1.ApplyUpdates;
    
      //REMOVED or ADD this 2 lines if there is a problem in your SQLdB
      //THESE 2 lines apparently work around a bug in SQLdB for MS_Access dataBase'
      //**** missing reference to bugtracker item; please create a bugtracker item
      //SQLQuery1.Close;
      //SQLQuery1.Open;
    
    
      Form1.caption := 'DELETED';
      end;
    end;
    
    procedure TForm1.Button4Click(Sender: TObject);
    begin
      if SQLQuery1.RecordCount>0 then
      begin
        SQLQuery1.Edit;
        SQLQuery1.Post;
        Sqlquery1.ApplyUpdates;
        Form1.caption := 'UPDATED';
      end;
    end;
    
    procedure TForm1.FormCloseQuery(Sender: TObject; var CanClose: boolean);
    begin
      SQLQuery1.Close;
      CanClose := True;
    end;
    
    initialization
      {$I unit1.lrs}
    
    end.
    

### Create a database programmatically

Using either ODBC driver, you can programmatically create Microsoft Access databases. The following code is valid for both 32-bit and 64-bit applications on Windows. 

Example code: 
    
    
    uses
      ..., sysutils, odbcsqldyn;
    
    const
       ODBC_ADD_DSN=1;
       { just for reference...
       ODBC_CONFIG_DSN=2;
       ODBC_REMOVE_DSN=3;
       ODBC_ADD_SYS_DSN=4;
       ODBC_CONFIG_SYS_DSN=5;
       ODBC_REMOVE_SYS_DSN=6;
       ODBC_REMOVE_DEFAULT_DSN=7; }
    
    function SQLConfigDataSource(hwndParent: Integer; fRequest: Integer;
      lpszDriverString: PChar; lpszAttributes: PChar): Integer; stdcall;
      external 'odbccp32.dll';
    
    function SQLInstallerError(iError: integer; pfErrorCode: PInteger;
      lpszErrorMsg: String; cbErrorMsgMax: integer; pcbErrorMsg: PInteger): integer; stdcall;
      external 'odbccp32.dll';
    
    procedure CreateAccessDatabase(DatabaseFile: string);
    var
      cmd: String;
      driver: String;
      errCode, errMsgLength: DWord;
      errMsg: String;
      retCode: integer;
      ext: String;
    begin
      ext := Lowercase(ExtractFileExt(DatabaseFile));
    
      driver := 'Microsoft Access Driver (*.mdb, *.accdb)';
      { With this driver,
        CREATE_DBV12 will create an .accdb format database;
        CREATE_DBV4 will create an .mdb format database
        http://stackoverflow.com/questions/9205633/how-do-i-specify-the-odbc-access-driver-format-when-creating-the-database
      }
      if ext = '.mdb' then
        cmd := 'CREATE_DBV4="' + DatabaseFile + '"'
      else if ext = '.accdb' then
        cmd := 'CREATE_DBV12="' + DatabaseFile + '"'
      else
        raise Exception.Create('Invalid database file extension.');
    
      retCode := SQLConfigDataSource(0, ODBC_ADD_DSN, PChar(driver), PChar(cmd));
      if not (retCode in [SQL_SUCCESS, SQL_SUCCESS_WITH_INFO]) then
      begin
        //try alternate driver
        driver := 'Microsoft Access Driver (*.mdb)';
        cmd := 'CREATE_DB="' + DatabaseFile + '"';
        retCode := SQLConfigDataSource(0, ODBC_ADD_DSN, PChar(driver), PChar(cmd));
      end;
    
      if not (retCode in [SQL_SUCCESS, SQL_SUCCESS_WITH_INFO]) then
      begin
        errCode := 0;
        errMsgLength := 0;
        SetLength(errMsg, SQL_MAX_MESSAGE_LENGTH);
        SQLInstallerError(1, @errCode, PChar(errMsg), Length(errMsg), @errMsgLength);
        SetLength(errMsg, errMsgLength);
        raise Exception.CreateFmt('Error creating Access database: %s', [errMsg]);
      end;
    end;
    

### Character set issues

Old Access databases are encoded using an ANSI character set (I don't know about new ones - they probably use wide strings). Therefore, non-ASCII characters are not displayed correctly in a DBGrid or other data-aware controls. This can be fixed by attaching handlers to the OnGetText and OnSetText events of the dataset after opening: 
    
    
    uses
      lconvencoding;
     
    procedure TForm1.SQLQuery1AfterOpen(DataSet: TDataSet);
    var
      i: Integer;  
    begin
      for i:=0 to DataSet.Fields.Count-1 do
      begin
        if DataSet.Fields[i].DataType=ftString then
        begin
          DataSet.Fields[i].OnGetText := @ConvertFromDB;
          DataSet.Fields[i].OnSetText := @ConvertToDB;
        end;
      end;
    end;
     
    procedure TForm1.ConvertFromDB(Sender: TField; var aText: string; DisplayText: Boolean);
    begin
      if not Sender.IsNull then
        aText := WinCPToUTF8(Sender.AsString);  // if encoded in windows default code page
    end;
     
    procedure TForm1.ConvertToDB(Sender: TField; const aText: string);
    begin
      if aText <> '' then 
        Sender.Value := UTF8ToWinCP(aText);
    end;
    

## Example database

Microsoft Access ships with the Northwind example database. 

If you only have the Access Runtime installed, you can use this alternative database from the Mondial project as a test database: [[1]](<http://databases.about.com/od/sampleaccessdatabases/a/Microsoft-Access-Sample-Database-Countries-Cities-And-Provinces.htm>) _[Dead link - November 2020]_

## See also

  * [ODBCConn](<ODBCConn.md> "ODBCConn") The ODBC connector this article uses

---

_Source: [https://wiki.freepascal.org/MS_Access](https://web.archive.org/web/20250121230313/https://wiki.freepascal.org/MS_Access)_
