# SQLdb tab

│ **English (en)** │  **[français (fr)](</SQLdb_tab/fr> "SQLdb tab/fr")** │  **[日本語 (ja)](</SQLdb_tab/ja> "SQLdb tab/ja")** │  **[русский (ru)](<../ru/SQLdb_tab.md> "SQLdb tab/ru")** │    
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
  
The **SQLdb tab** of the [Component Palette](<Component_Palette.md> "Component Palette") contains non-visible components for access to "large" database systems. 

[![Component Palette SQLdb.png](https://wiki.freepascal.org/images/4/40/Component_Palette_SQLdb.png)](</File:Component_Palette_SQLdb.png>)

Icon | Component | Description   
---|---|---  
[![tsqlquery.png](https://wiki.freepascal.org/images/b/be/tsqlquery.png)](</File:tsqlquery.png>) | [TSQLQuery](<TSQLQuery.md> "TSQLQuery") | Needed to read/write data from/to a [DataSet](</index.php?title=DataSet&action=edit&redlink=1> "DataSet \(page does not exist\)") using SQL commands   
[![tsqltransaction.png](https://wiki.freepascal.org/images/f/fd/tsqltransaction.png)](</File:tsqltransaction.png>) | [TSQLTransaction](<TSQLTransaction.md> "TSQLTransaction") | Defines how changes are transferred to the databasse   
[![tsqlscript.png](https://wiki.freepascal.org/images/5/52/tsqlscript.png)](</File:tsqlscript.png>) | [TSQLScript](<TSQLScript.md> "TSQLScript") | Scripting for execution of multiple SQL commands   
[![tsqlconnector.png](https://wiki.freepascal.org/images/5/52/tsqlconnector.png)](</File:tsqlconnector.png>) | [TSQLConnector](<TSQLConnector.md> "TSQLConnector") | General connection component; the database engine is defined by property `ConnectorType`  
[![tmssqlconnection.png](https://wiki.freepascal.org/images/1/1c/tmssqlconnection.png)](</File:tmssqlconnection.png>) | [TMSSQLConnection](<TMSSQLConnection.md> "TMSSQLConnection") | Connection component to **MS SQL** database engine   
[![tsybaseconnection.png](https://wiki.freepascal.org/images/1/12/tsybaseconnection.png)](</File:tsybaseconnection.png>) | [TSybaseConnection](<TSybaseConnection.md> "TSybaseConnection") | Connection component to **Sybase** database engine   
[![tpqconnection.png](https://wiki.freepascal.org/images/b/b3/tpqconnection.png)](</File:tpqconnection.png>) | [TPQConnection](<TPQConnection.md> "TPQConnection") | Connection component to **Postgres** SQL server   
[![tpqteventmonitor.png](https://wiki.freepascal.org/images/e/e9/tpqteventmonitor.png)](</File:tpqteventmonitor.png>) | [TPQTEventMonitor](</index.php?title=TPQTEventMonitor&action=edit&redlink=1> "TPQTEventMonitor \(page does not exist\)") | Monitors events sent from the Postgres SQL database server   
[![toracleconnection.png](https://wiki.freepascal.org/images/8/8d/toracleconnection.png)](</File:toracleconnection.png>) | [TOracleConnection](<TOracleConnection.md> "TOracleConnection") | Connection component to **Oracle** database server   
[![todbcconnection.png](https://wiki.freepascal.org/images/5/52/todbcconnection.png)](</File:todbcconnection.png>) | [TODBCConnection](<TODBCConnection.md> "TODBCConnection") | Connection component to an **ODBC** (Open Database Connectivity) data source   
[![tmysql40connection.png](https://wiki.freepascal.org/images/1/16/tmysql40connection.png)](</File:tmysql40connection.png>) | [TMySQL40Connection](<TMySQL40Connection.md> "TMySQL40Connection") | Connection component to **MySQL 4.0**  
[![tmysql41connection.png](https://wiki.freepascal.org/images/b/b6/tmysql41connection.png)](</File:tmysql41connection.png>) | [TMySQL41Connection](<TMySQL41Connection.md> "TMySQL41Connection") | Connection component to **MySQL 4.1**  
[![tmysql50connection.png](https://wiki.freepascal.org/images/4/48/tmysql50connection.png)](</File:tmysql50connection.png>) | [TMySQL50Connection](<TMySQL50Connection.md> "TMySQL50Connection") | Connection component to **MySQL 5.0**  
[![tmysql51connection.png](https://wiki.freepascal.org/images/f/f8/tmysql51connection.png)](</File:tmysql51connection.png>) | [TMySQL51Connection](<TMySQL51Connection.md> "TMySQL51Connection") | Connection component to **MySQL 5.1**  
[![tmysql55connection.png](https://wiki.freepascal.org/images/6/6b/tmysql55connection.png)](</File:tmysql55connection.png>) | [TMySQL55Connection](<TMySQL55Connection.md> "TMySQL55Connection") | Connection component to **MySQL 5.5**  
[![tmysql56connection.png](https://wiki.freepascal.org/images/7/76/tmysql56connection.png)](</File:tmysql56connection.png>) | [TMySQL56Connection](<TMySQL56Connection.md> "TMySQL56Connection") | Connection component to **MySQL 5.6**  
[![tmysql57connection.png](https://wiki.freepascal.org/images/9/92/tmysql57connection.png)](</File:tmysql57connection.png>) | [TMySQL57Connection](<TMySQL57Connection.md> "TMySQL57Connection") | Connection component to **MySQL 5.7**  
[![tsqlite3connection.png](https://wiki.freepascal.org/images/e/e4/tsqlite3connection.png)](</File:tsqlite3connection.png>) | [TSQLite3Connection](<TSQLite3Connection.md> "TSQLite3Connection") | Connection component for **SQLite3** database files.   
[![tibconnection.png](https://wiki.freepascal.org/images/5/58/tibconnection.png)](</File:tibconnection.png>) | [TIBConnection](<TIBConnection.md> "TIBConnection") | Connection component to **InterBase/[Firebird](<Firebird.md> "Firebird")**  
[![tfbadmin.png](https://wiki.freepascal.org/images/f/f8/tfbadmin.png)](</File:tfbadmin.png>) | [TFBAdmin](<TFBAdmin.md> "TFBAdmin") | Component for Interbase/Firebird administration tasks   
[![tfbeventmonitor.png](https://wiki.freepascal.org/images/9/9b/tfbeventmonitor.png)](</File:tfbeventmonitor.png>) | [TFBEventMonitor](<TFBEventMonitor.md> "TFBEventMonitor") | Monitors events sent from the Interbase/[Firebird](<Firebird.md> "Firebird") server   
[![tsqldblibraryloader.png](https://wiki.freepascal.org/images/f/f9/tsqldblibraryloader.png)](</File:tsqldblibraryloader.png>) | [TSQLDBLibraryLoader](<TSQLDBLibraryLoader.md> "TSQLDBLibraryLoader") | Specifies the names and locations of SQLDB database libraries   
[Component Palette](<Component_Palette.md> "Component Palette")  
---  
[Standard/ja](</Standard_tab/ja> "Standard tab/ja") \- [Additional/ja](</Additional_tab/ja> "Additional tab/ja") \- [Common Controls/ja](</index.php?title=Common_Controls_tab/ja&action=edit&redlink=1> "Common Controls tab/ja \(page does not exist\)") \- [Dialogs/ja](</index.php?title=Dialogs_tab/ja&action=edit&redlink=1> "Dialogs tab/ja \(page does not exist\)") \- [Data Controls/ja](</Data_Controls_tab/ja> "Data Controls tab/ja") \- [Data Access/ja](</Data_Access_tab/ja> "Data Access tab/ja") \- [System](<System_tab.md> "System tab") \- [Misc](<Misc_tab.md> "Misc tab") \- [LazControls](<LazControls_tab.md> "LazControls tab") \- [RTTI](<RTTI_tab.md> "RTTI tab") \- SQLdb \- [Pascal Script](<Pascal_Script_tab.md> "Pascal Script tab") \- [SynEdit](<SynEdit_tab.md> "SynEdit tab") \- [Chart](<Chart_tab.md> "Chart tab") \- [IPro](<IPro_tab.md> "IPro tab")

---

_Source: [https://wiki.freepascal.org/SQLdb_tab](https://web.archive.org/web/20250323133731/https://wiki.freepascal.org/SQLdb_tab)_
