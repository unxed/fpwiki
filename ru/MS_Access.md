# MS Access

│ **[English (en)](<../en/MS_Access.md> "MS Access")** │  **[español (es)](</MS_Access/es> "MS Access/es")** │  **[français (fr)](</MS_Access/fr> "MS Access/fr")** │  **русский (ru)** │    
******Этот перевод скопирован со страницы[User:QLKforges/ru](</User:QLKforges/ru> "User:QLKforges/ru")**

Эта страница объясняет, как использовать базу данных Microsoft Access с использованием [ODBC](<../en/ODBCConn.md> "ODBCConn") и FPC в SQLdb. 

## Contents

  * 1 Специфика MS Access
    * 1.1 UsePrimaryKeyAsKey
    * 1.2 Get just-inserted autonumber primary key
    * 1.3 ODBC Драйвер
      * 1.3.1 Windows
      * 1.3.2 Unix/Linux
  * 2 File-based DSN Instructions
    * 2.1 Set up the File DSN
      * 2.1.1 File DSN contents
    * 2.2 Configure your project to use file-based DSN
  * 3 System/User DSN
  * 4 DSN-less connection
  * 5 Example code
    * 5.1 Lazarus: data bound controls on forms
    * 5.2 Create a database programmatically
  * 6 Example database
  * 7 See also



## Специфика MS Access

### UsePrimaryKeyAsKey

В свойствах **TSQLQuery** , возможно, потребуется установить UsePrimaryKeyAsKey: False 

### Get just-inserted autonumber primary key

After inserting data, Access 2000 and higher supports getting the just-inserted autonumber primary key using this query: 
    
    
    SELECT @@IDENTITY
    

### ODBC Драйвер

#### Windows

Есть 2 различных [ ODBC](<../en/ODBCConn.md> "ODBCConn") драйвера для Microsoft Access: 

  * Старый драйвер, который можно использовать только формат файла .mdb, называется он "Microsoft Access Driver (*.mdb)". В многих версиях Windows (начиная с Windows 2000, вплоть до Vista); Для более старых версий Windows нужно загрузить 32-х битные компоненты такие, как MDAC или Jet.
  * Новый драйвер, который может получить доступ как .mdb и .accdb файлам, называется "Microsoft Access Driver (*.mdb, * .accdb)". Доступен как в 32-х битной так, и 64-х битной "Microsoft Access Database Engine 2010 Redistributable";.



Как обычно, при использовании 32 бит Lazarus / FPC, использовать 32 водителей немного ODBC. При использовании 64-битной Lazarus / FPC, использовать 64 водителей немного ODBC. **Редактируется**

#### Unix/Linux

The [mdbtools project](<http://sourceforge.net/projects/mdbtools/>) offers limited support for MS Accesss. It includes an ODBC driver which might be used. Try installing packages like mdbtools mdbtools-dev mdbtools-gmdb 

At least on Debian, the ODBC driver name is "MDBTools". 

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

TOBDCConnection Properties: FileDSN: the path+filename to the DSN file saved earlier, e.g. c:\mylazarus\project1\myFile.dsn Username: admin (or whatever username you need if you are using Access security) _this parameter may not be needed if no MS Access security is used_ Do not put any other entries in the properties. 

  


## System/User DSN

As explained in the [ODBC](<../en/ODBCConn.md> "ODBCConn") article, you can also use system or user DSNs, where connection settings are defined in the ODBC control panel instead of stored in a file. 

The TODBCConnection should have DatabaseName: <name_of_your_DSN>

## DSN-less connection

As explained in the [ODBC](<../en/ODBCConn.md> "ODBCConn") article, you can also create a DSN-less connection to your Access database where you can specify all connection parameters in code, something like: 
    
    
      //Notice we're using the new MS Access driver, we could probably use the old one too:
      conn.Driver:='Microsoft Access Driver (*.mdb, *.accdb)';
      conn.Params.Add('DBQ=c:\somedirectory\test.mdb');
      ... add whatever parameters you want/need...
    

  


## Example code

### Lazarus: data bound controls on forms

Working Source code sample:(Updated2_with_add_delete_update) <http://www.mediafire.com/file/ne1jx3zpnwzefq3/msaccesstest2.zip>
    
    
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

Using either ODBC driver, you can programmatically create Microsoft Access databases. (Note: error handling has not been tested; please update page if you tested it) 

Example program: 
    
    
    program CreateAccessDb;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes, sysutils,
      Windows;
    
    Const
       ODBC_ADD_DSN=1;
       ODBC_CONFIG_DSN=2;
       ODBC_REMOVE_DSN=3;
       ODBC_ADD_SYS_DSN=4;
       ODBC_CONFIG_SYS_DSN=5;
       ODBC_REMOVE_SYS_DSN=6;
       ODBC_REMOVE_DEFAULT_DSN=7;
    function SQLConfigDataSource(hwndParent: Integer; fRequest: Integer;
      lpszDriverString: PChar; lpszAttributes: PChar): Integer; stdcall; external 'odbccp32.dll';
    function SQLInstallerError(iError: integer; pfErrorCode: PInteger; lpszErrorMsg: string; cbErrorMsgMax: integer; pcbErrorMsg: PInteger): integer; stdcall; external 'odbccp32.dll';
    
    function CreateAccessDatabase(DatabaseFile: string): boolean;
    var
      DBPChar: PChar;
      Driver: PChar;
      ErrorCode, ResizeErrorMessage: integer;
      ErrorMessage: PChar;
      retCode: integer;
    begin
    	driver := 'Microsoft Access Driver (*.mdb, *.accdb)';
      { With this driver,
      CREATE_DB/CREATE_DBV12 will create an .accdb format database;
      CREATE_DBV4 will create an mdb
      http://stackoverflow.com/questions/9205633/how-do-i-specify-the-odbc-access-driver-format-when-creating-the-database
      }
      DBPChar:=PChar('CREATE_DBV4="'+DatabaseFile+'"');
      retCode := SQLConfigDataSource(Hwnd(nil), ODBC_ADD_DSN, Driver, DBPChar);
      if retCode<>0 then
      begin
        //try alternate driver
        Driver := 'Microsoft Access Driver (*.mdb)';
        DBPChar:=PChar('CREATE_DB="'+DatabaseFile+'"');
        retCode := SQLConfigDataSource(Hwnd(nil), ODBC_ADD_DSN, Driver, DBPChar);
      end;
      if retCode=0 then
      begin
        result:=true;
      end
      else
      begin
        result:=false;
        ErrorCode:=0;
        ResizeErrorMessage:=0;
        // todo: verify how the DLL is called - use pointers?; has not been tested.
        GetMem(ErrorMessage,512);
        try
          SQLInstallerError(1, @ErrorCode, ErrorMessage, SizeOf(ErrorMessage), @ResizeErrorMessage);
        finally
          FreeMem(ErrorMessage);
        end;
        raise Exception.CreateFmt('Error creating Access database: %s', [ErrorMessage]);
      end;
    end;
    
    begin
      writeln('Result: ');
      write(CreateAccessDatabase('d:\cop\t\bla.mdb'));
    end.
    

## Example database

Microsoft Access ships with the Northwind example database. 

If you only have the Access Runtime installed, you can use this alternative database from the Mondial project as a test database: [[1]](<http://databases.about.com/od/sampleaccessdatabases/a/Microsoft-Access-Sample-Database-Countries-Cities-And-Provinces.htm>)

## See also

  * [ODBCConn](<../en/ODBCConn.md> "ODBCConn") The ODBC connector this article uses

---

_Source: [https://wiki.freepascal.org/MS_Access/ru](https://web.archive.org/web/20231203203629/https://wiki.freepascal.org/MS_Access/ru)_
