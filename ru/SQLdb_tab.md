# SQLdb tab

│ **[English (en)](<../en/SQLdb_tab.md> "SQLdb tab")** │  **[français (fr)](</SQLdb_tab/fr> "SQLdb tab/fr")** │  **русский (ru)** │    
****  
  
---  
[**Databases portal**](<../en/Portal_Databases.md> "Portal:Databases")  
References: 

  * [General info](<../en/Databases.md> "Databases")
  * [Libraries](<../en/Database_libraries.md> "Database libraries")
  * [Field types](<../en/Database_field_type.md> "Database field type")
  * [Controls](<../en/Data_Controls_tab.md> "Data Controls tab")
  * [FAQ](<../en/Lazarus_DB_Faq.md> "Lazarus DB Faq")
  * [SQL how-to](<../en/SqlDBHowto.md> "SqlDBHowto")
  * [Working With TSQLQuery](<../en/Working_With_TSQLQuery.md> "Working With TSQLQuery")
  * [In-memory database applications](<../en/How_to_write_in-memory_database_applications_in_Lazarus/FPC.md> "How to write in-memory database applications in Lazarus/FPC")

Tutorials/practical articles: 

  * [Overview](<../en/Lazarus_Database_Overview.md> "Lazarus Database Overview")
  * [0 - Database set-up](<../en/SQLdb_Tutorial0.md> "SQLdb Tutorial0")
  * [1 - Getting started](<../en/SQLdb_Tutorial1.md> "SQLdb Tutorial1")
  * [2 - Editing](<../en/SQLdb_Tutorial2.md> "SQLdb Tutorial2")
  * [3 - Queries](<../en/SQLdb_Tutorial3.md> "SQLdb Tutorial3")
  * [4 - Data modules](<../en/SQLdb_Tutorial4.md> "SQLdb Tutorial4")
  * [SQLdb Programming Reference](<../en/SQLdb_Programming_Reference.md> "SQLdb Programming Reference")

Databases  


    [Advantage](<../en/Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<../en/MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<../en/mssqlconn.md> "mssqlconn") \- [Postgres](<../en/postgres.md> "postgres") \- [Interbase](<../en/Firebird.md> "Firebird") \- [Firebird](<../en/Firebird.md> "Firebird") \- [Oracle](<../en/Oracle.md> "Oracle") \- [ODBC](<../en/ODBCConn.md> "ODBCConn") \- [Paradox](<../en/TParadox.md> "TParadox") \- [SQLite](<../en/SQLite.md> "SQLite") \- [dBASE](<../en/Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<../en/MS_Access.md> "MS Access") \- [Zeos](<../en/Zeos_tutorial.md> "Zeos tutorial")  
  
Вкладка **SQLdb** [палитры компонентов](<Component_Palette.md> "Component Palette/ru") содержит невизуальные компоненты, предназначенные для подключения к базам данных и использования с [TDataSet](<TDataSet.md> "TDataSet/ru") и [TDataSource](<TDataSource.md> "TDataSource/ru"). 

[![Component Palette SQLdb.png](https://wiki.freepascal.org/images/4/40/Component_Palette_SQLdb.png)](</File:Component_Palette_SQLdb.png>)

Значок | Компонент | Описание   
---|---|---  
[![tsqlquery.png](https://wiki.freepascal.org/images/b/be/tsqlquery.png)](</File:tsqlquery.png>) | [TSQLQuery](<TSQLQuery.md> "TSQLQuery/ru") | SQL query   
[![tsqltransaction.png](https://wiki.freepascal.org/images/f/fd/tsqltransaction.png)](</File:tsqltransaction.png>) | [TSQLTransaction](<TSQLTransaction.md> "TSQLTransaction/ru") | Transaction   
[![tsqlscript.png](https://wiki.freepascal.org/images/5/52/tsqlscript.png)](</File:tsqlscript.png>) | [TSQLScript](</index.php?title=TSQLScript/ru&action=edit&redlink=1> "TSQLScript/ru \(page does not exist\)") | Scripting   
[![tsqlconnector.png](https://wiki.freepascal.org/images/5/52/tsqlconnector.png)](</File:tsqlconnector.png>) | [TSQLConnector](<TSQLConnector.md> "TSQLConnector/ru") | generic connector   
[![tmssqlconnection.png](https://wiki.freepascal.org/images/1/1c/tmssqlconnection.png)](</File:tmssqlconnection.png>) | [TMSSQLConnection](<TMSSQLConnection.md> "TMSSQLConnection/ru") | MS SQL   
[![tsybaseconnection.png](https://wiki.freepascal.org/images/1/12/tsybaseconnection.png)](</File:tsybaseconnection.png>) | [TSybaseConnection](<TSybaseConnection.md> "TSybaseConnection/ru") | Sybase   
[![tpqconnection.png](https://wiki.freepascal.org/images/b/b3/tpqconnection.png)](</File:tpqconnection.png>) | [TPQConnection](<TPQConnection.md> "TPQConnection/ru") | Postgres   
[![tpqteventmonitor.png](https://wiki.freepascal.org/images/e/e9/tpqteventmonitor.png)](</File:tpqteventmonitor.png>) | [TPQTEventMonitor](</index.php?title=TPQTEventMonitor/ru&action=edit&redlink=1> "TPQTEventMonitor/ru \(page does not exist\)") | Postgres event monitor   
[![toracleconnection.png](https://wiki.freepascal.org/images/8/8d/toracleconnection.png)](</File:toracleconnection.png>) | [TOracleConnection](<TOracleConnection.md> "TOracleConnection/ru") | Oracle   
[![todbcconnection.png](https://wiki.freepascal.org/images/5/52/todbcconnection.png)](</File:todbcconnection.png>) | [TODBCConnection](<TODBCConnection.md> "TODBCConnection/ru") | ODBC   
[![tmysql40connection.png](https://wiki.freepascal.org/images/1/16/tmysql40connection.png)](</File:tmysql40connection.png>) | [TMySQL40Connection](<TMySQL40Connection.md> "TMySQL40Connection/ru") | MySQL 4.0   
[![tmysql41connection.png](https://wiki.freepascal.org/images/b/b6/tmysql41connection.png)](</File:tmysql41connection.png>) | [TMySQL41Connection](<TMySQL41Connection.md> "TMySQL41Connection/ru") | MySQL 4.1   
[![tmysql50connection.png](https://wiki.freepascal.org/images/4/48/tmysql50connection.png)](</File:tmysql50connection.png>) | [TMySQL50Connection](<TMySQL50Connection.md> "TMySQL50Connection/ru") | MySQL 5.0   
[![tmysql51connection.png](https://wiki.freepascal.org/images/f/f8/tmysql51connection.png)](</File:tmysql51connection.png>) | [TMySQL51Connection](<TMySQL51Connection.md> "TMySQL51Connection/ru") | MySQL 5.1   
[![tmysql55connection.png](https://wiki.freepascal.org/images/6/6b/tmysql55connection.png)](</File:tmysql55connection.png>) | [TMySQL55Connection](<TMySQL55Connection.md> "TMySQL55Connection/ru") | MySQL 5.5   
[![tmysql56connection.png](https://wiki.freepascal.org/images/7/76/tmysql56connection.png)](</File:tmysql56connection.png>) | [TMySQL56Connection](<TMySQL56Connection.md> "TMySQL56Connection/ru") | MySQL 5.6   
[![tsqlite3connection.png](https://wiki.freepascal.org/images/e/e4/tsqlite3connection.png)](</File:tsqlite3connection.png>) | [TSQLite3Connection](</index.php?title=TSQLite3Connection/ru&action=edit&redlink=1> "TSQLite3Connection/ru \(page does not exist\)") | SQLite   
[![tibconnection.png](https://wiki.freepascal.org/images/5/58/tibconnection.png)](</File:tibconnection.png>) | [TIBConnection](<TIBConnection.md> "TIBConnection/ru") | InterBase/[Firebird](<Firebird.md> "Firebird/ru")  
[![tfbadmin.png](https://wiki.freepascal.org/images/f/f8/tfbadmin.png)](</File:tfbadmin.png>) | [TFBAdmin](</index.php?title=TFBAdmin/ru&action=edit&redlink=1> "TFBAdmin/ru \(page does not exist\)") | Firebird admin   
[![tfbeventmonitor.png](https://wiki.freepascal.org/images/9/9b/tfbeventmonitor.png)](</File:tfbeventmonitor.png>) | [TFBEventMonitor](<TFBEventMonitor.md> "TFBEventMonitor/ru") | Firebird Event monitor   
[![tsqldblibraryloader.png](https://wiki.freepascal.org/images/f/f9/tsqldblibraryloader.png)](</File:tsqldblibraryloader.png>) | [TSQLDBLibraryLoader](</index.php?title=TSQLDBLibraryLoader/ru&action=edit&redlink=1> "TSQLDBLibraryLoader/ru \(page does not exist\)") | Library loader   
[Палитра компонентов](<Component_Palette.md> "Component Palette/ru")  
---  
[Standard](<Standard_tab.md> "Standard tab/ru") \- [Additional](<Additional_tab.md> "Additional tab/ru") \- [Common Controls](<Common_Controls_tab.md> "Common Controls tab/ru") \- [Dialogs](<Dialogs_tab.md> "Dialogs tab/ru") \- [Data Controls](<Data_Controls_tab.md> "Data Controls tab/ru") \- [Data Access](<Data_Access_tab.md> "Data Access tab/ru") \- [System](<System_tab.md> "System tab/ru") \- [Misc](<Misc_tab.md> "Misc tab/ru") \- [LazControls](<LazControls_tab.md> "LazControls tab/ru") \- [RTTI](<RTTI_tab.md> "RTTI tab/ru") \- SQLdb \- [Pascal Script](<Pascal_Script_tab.md> "Pascal Script tab/ru") \- [SynEdit](<SynEdit_tab.md> "SynEdit tab/ru") \- [Chart](<Chart_tab.md> "Chart tab/ru") \- [IPro](<IPro_tab.md> "IPro tab/ru")

---

_Source: [https://wiki.freepascal.org/SQLdb_tab/ru](https://web.archive.org/web/20230602163416/https://wiki.freepascal.org/SQLdb_tab/ru)_
