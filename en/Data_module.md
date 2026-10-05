# Data module

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
  
A **data module** is a database specific kind of pascal unit. Much like a [TForm](<TForm.md> "TForm") you can drop components on it, as long as they are non-visible. 

Typically a data module might contain a [TSQLConnector](<TSQLConnector.md> "TSQLConnector") (or a specialized one) and a [TSQLTransaction](<TSQLTransaction.md> "TSQLTransaction"). 

A new data module can be created using [File|New...](<IDE_Window__New_Item.md> "IDE Window: New Item")

[![datamodule.png](https://wiki.freepascal.org/images/4/4b/datamodule.png)](</File:datamodule.png>)

The datamodule above contains three database related items: 

  * [TSQLTransaction](<TSQLTransaction.md> "TSQLTransaction")
  * [TSQLConnector](<TSQLConnector.md> "TSQLConnector")
  * [TSQLScript](<TSQLScript.md> "TSQLScript")



SQLScript1 sets its transaction to SQLTransaction1 and its DataBase to SQLConnector1

---

_Source: [https://wiki.freepascal.org/Data_module](https://web.archive.org/web/20250116152259/https://wiki.freepascal.org/Data_module)_
