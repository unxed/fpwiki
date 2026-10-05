# fpjsondataset

│ **English (en)** │  **[polski (pl)](</fpjsondataset/pl> "fpjsondataset/pl")** │   
  
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


    [Advantage](<Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<mssqlconn.md> "mssqlconn") \- [Postgres](<postgres.md> "postgres") \- [Interbase](<Firebird.md> "Firebird") \- [Firebird](<Firebird.md> "Firebird") \- [Oracle](<Oracle.md> "Oracle") \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- [dBASE](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<MS_Access.md> "MS Access") \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
**fpjsondataset** is an fcl-db unit containing a dataset class for accessing JSON data as simple table structures. 

  


## TJSONDataSet

TJSONDataSet is a dataset class for using JSON arrays for data handling. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** TJSONDataSet is included in Free Pascal but not visible in Lazarus' component palette.

  


### Example

  1. Create a new application project
  2. Place a DB Grid on the form (DBGrid1)
  3. Place a DataSource on the form (DataSource1)
  4. Connect DBGrid1.Datasource with DataSource1
  5. Create a JSON dataset via code


    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, DBCtrls, DBGrids, DB,
      fpjson, fpjsondataset;
    
    type
    
      TForm1 = class(TForm)
        DataSource1: TDataSource;
        DBGrid1: TDBGrid;
        procedure FormCreate(Sender: TObject);
      private
        JSONDataSet: TJSONDataSet;
      end;
    
    var
      Form1: TForm1;
    
    const
      // this is the sample JSON data used in the demo
      JSON_STRING = '['
        +'{"Author ID":"409-56-7008","Last Name":"Bennet","First Name":"Abraham","Active": False},'
        +'{"Author ID":"213-46-8915","Last Name":"Green","First Name":"Marjorie","Active": True}'
        +']';
    
    implementation
    
    {$R *.lfm}
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      data: TJSONArray;
    begin
      // parse the JSON string
      data := GetJSON(JSON_STRING) as TJSONArray;
      // create a new TJSONDataSet
      JSONDataSet := TJSONDataSet.Create(Self);
      // you'll need to create the FieldDefs manually
      // ftString requires a length specified
      JSONDataSet.FieldDefs.Add('Author ID', ftString, 11, True);
      JSONDataSet.FieldDefs.Add('Last Name', ftString, 40);
      JSONDataSet.FieldDefs.Add('First Name', ftString, 20);
      JSONDataSet.FieldDefs.Add('Active', ftBoolean);
      // set OwnsData to True, and the JSON data will be destroyed when the dataset is destroyed
      JSONDataSet.OwnsData := True;
      // the JSON data is an array of objects
      JSONDataSet.RowType := rtJSONObject;
      // assign the JSON data to the dataset
      JSONDataSet.Rows := data;
      // activate the dataset
      JSONDataSet.Active := True;
      // assign the dataset to the datasource
      DataSource1.DataSet := JSONDataSet;
    end;
    
    end.

---

_Source: [https://wiki.freepascal.org/fpjsondataset](https://web.archive.org/web/20250122112637/https://wiki.freepascal.org/fpjsondataset)_
