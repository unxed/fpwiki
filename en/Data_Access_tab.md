# Data Access tab

│ **English (en)** │  **[français (fr)](</Data_Access_tab/fr> "Data Access tab/fr")** │  **[русский (ru)](<../ru/Data_Access_tab.md> "Data Access tab/ru")** │    
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


    [Advantage](<Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<mssqlconn.md> "mssqlconn") \- [Postgres](<postgres.md> "postgres") \- [Interbase](<Firebird.md> "Firebird") \- [Firebird](<Firebird.md> "Firebird") \- [Oracle](<Oracle.md> "Oracle") \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- [dBASE](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<MS_Access.md> "MS Access") \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
The **Data Access tab** of the [Component Palette](<Component_Palette.md> "Component Palette") contains non-visible database-dataset related components. 

[![Component Palette Data Access.png](https://wiki.freepascal.org/images/2/25/Component_Palette_Data_Access.png)](</File:Component_Palette_Data_Access.png>)

Icon | Component | Description | Online Docs   
---|---|---|---  
[![tdatasource.png](https://wiki.freepascal.org/images/d/d0/tdatasource.png)](</File:tdatasource.png>) | [TDataSource](<TDataSource.md> "TDataSource") | Connects [TDataSet](<TDataSet.md> "TDataSet") (database contents) and [data controls](<Data_Controls_tab.md> "Data Controls tab") |   
[![tbufdataset.png](https://wiki.freepascal.org/images/6/64/tbufdataset.png)](</File:tbufdataset.png>) | [TBufDataset](<TBufDataset.md> "TBufDataset") | Buffered in-memory [TDataSet](<TDataSet.md> "TDataSet") |   
[![tmemdataset.png](https://wiki.freepascal.org/images/4/41/tmemdataset.png)](</File:tmemdataset.png>) | [TMemDataset](<TMemDataset.md> "TMemDataset") | Light-weight alternative to [TBufDataset](<TBufDataset.md> "TBufDataset") |   
[![tsdfdataset.png](https://wiki.freepascal.org/images/3/37/tsdfdataset.png)](</File:tsdfdataset.png>) | [TSdfDataSet](<TSdfDataSet.md> "TSdfDataSet") | Accesses delimited text files as a [TDataSet](<TDataSet.md> "TDataSet")  
[![tcsvdataset.png](https://wiki.freepascal.org/images/d/d6/tcsvdataset.png)](</File:tcsvdataset.png>) | [TCSVDataSet](<TCSVDataSet.md> "TCSVDataSet") | Accesses CSV text files as a [TDataSet](<TDataSet.md> "TDataSet"). Similar to TSdfDataset, but conforms to RFC4180 CSV |   
[![tfixedformatdataset.png](https://wiki.freepascal.org/images/d/d4/tfixedformatdataset.png)](</File:tfixedformatdataset.png>) | [TFixedFormatDataSet](<TFixedFormatDataSet.md> "TFixedFormatDataSet") | Accesses text files with fixed format records as a [TDataSet](<TDataSet.md> "TDataSet") |   
[![tdbf.png](https://wiki.freepascal.org/images/b/bb/tdbf.png)](</File:tdbf.png>) | [TDbf](<TDbf.md> "TDbf") | Accesses dBASE data files |   
[![tparadox.png](https://wiki.freepascal.org/images/f/f4/tparadox.png)](</File:tparadox.png>) | [TParadox](<TParadox.md> "TParadox") | Accesses PARADOX data files |   
[![tsqlite3dataset.png](https://wiki.freepascal.org/images/8/8a/tsqlite3dataset.png)](</File:tsqlite3dataset.png>) | [TSqlite3](<SQLite.md> "SQLite") | Connects to SQLITE3 and its data files.   
  
  


[Component Palette](<Component_Palette.md> "Component Palette")  
---  
[Standard](<Standard_tab.md> "Standard tab") \- [Additional](<Additional_tab.md> "Additional tab") \- [Common Controls](<Common_Controls_tab.md> "Common Controls tab") \- [Dialogs](<Dialogs_tab.md> "Dialogs tab") \- [Data Controls](<Data_Controls_tab.md> "Data Controls tab") \- Data Access \- [System](<System_tab.md> "System tab") \- [Misc](<Misc_tab.md> "Misc tab") \- [LazControls](<LazControls_tab.md> "LazControls tab") \- [RTTI](<RTTI_tab.md> "RTTI tab") \- [SQLdb](<SQLdb_tab.md> "SQLdb tab") \- [Pascal Script](<Pascal_Script_tab.md> "Pascal Script tab") \- [SynEdit](<SynEdit_tab.md> "SynEdit tab") \- [Chart](<Chart_tab.md> "Chart tab") \- [IPro](<IPro_tab.md> "IPro tab")

---

_Source: [https://wiki.freepascal.org/Data_Access_tab](https://web.archive.org/web/20240121085629/https://wiki.freepascal.org/Data_Access_tab)_
