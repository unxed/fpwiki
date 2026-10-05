# SQLdb Programming Reference

│ **English (en)** │  **[español (es)](</SQLdb_Programming_Reference/es> "SQLdb Programming Reference/es")** │  **[français (fr)](</SQLdb_Programming_Reference/fr> "SQLdb Programming Reference/fr")** │  **[日本語 (ja)](</SQLdb_Programming_Reference/ja> "SQLdb Programming Reference/ja")** │  **[中文（中国大陆） (zh_CN)](</SQLdb_Programming_Reference/zh_CN> "SQLdb Programming Reference/zh CN")** │    
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
  * SQLdb Programming Reference

Databases  


    [Advantage](<Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<mssqlconn.md> "mssqlconn") \- [Postgres](<postgres.md> "postgres") \- [Interbase](<Firebird.md> "Firebird") \- [Firebird](<Firebird.md> "Firebird") \- [Oracle](<Oracle.md> "Oracle") \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- [dBASE](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<MS_Access.md> "MS Access") \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
## Contents

  * 1 Documentation
  * 2 Class Structure
    * 2.1 Notes
  * 3 Interaction
    * 3.1 TConnection
    * 3.2 TSQLTransaction
    * 3.3 TSQLQuery
    * 3.4 DataSource
    * 3.5 Databound controls such as DBGrid
  * 4 Data modules



## Documentation

Please see the official documentation at [SQLDB documentation](<http://www.freepascal.org/docs-html/fcl/sqldb/index.html>). 

This article attempts to give some more detail about SQLDb; however the official documentation is authorative. 

## Class Structure

The following diagram attempts to show the hierarchy and required links of the MAIN components involved in SQLdb. It is certainly not exhaustive, nor does it use any "proper" diagram structure, so please don't try to read too much into it. I hope it will make it easier to work out which bits of the source code you need to look at to really work out what is happening. 

[![Laz SqlDB components.png](https://wiki.freepascal.org/images/1/17/Laz_SqlDB_components.png)](</File:Laz_SqlDB_components.png>)

#### Notes

  * The link from TDatabase to TTransaction is Transactions, and is a list, implying many transactions are possible for the one database. However, a new link is defined from TSQLConnection to TSQLTransaction which is Transaction - a single transaction per database. This does not actually hide the previous link, but only the new link is published, and it is probably inadvisable to use the ancestor's link.
  * Some of the inherited links need to be typecast to the new types to be useful. You can't call SQLQuery.Transaction.Commit, as Commit is only defined in TSQLTransaction. Call SQLTransaction.Commit, or "(SQLQuery.Transaction as TSQLTransaction).Commit"



## Interaction

### TConnection

Documentation: [TSQLConnection documentation](<http://www.freepascal.org/docs-html/fcl/sqldb/tsqlconnection.html>)

A [TConnection](</index.php?title=TConnection&action=edit&redlink=1> "TConnection \(page does not exist\)") represents a connection to an SQL database. In daily use, you will use the descendent for a specific database (e.g. TIBConnection for Interbase/Firebird), but it is possible to use TConnection if you are trying to write database factory/database independent applications (note: it's probably more advisable to use [TSQLConnector](<http://www.freepascal.org/docs-html/fcl/sqldb/tsqlconnector.html>)). In this object, you specify connection-related items such as hostname, username and password. Finally, you can connect or disconnect (using the .Active or .Connected property) 

Most database allow muliple concurrent connections from the same program/user. 

### TSQLTransaction

Documentation: [TSQLTransaction](<http://www.freepascal.org/docs-html/fcl/sqldb/tsqltransaction.html>)

This object represents a transaction on the database. In practice, at least one transaction needs to be active for a database, even if you only use it for reading data. When using a single transaction, set the TConnection.Transaction property to the transaction to set the default transaction for the database; the corresponding TSQLTransaction.Database property should then automatically point to the connection. 

Setting a [TSQLTransaction](<TSQLTransaction.md> "TSQLTransaction") to .Active/calling .StartTransaction starts a transaction; calling .Commit or .RollBack commits (saves) or rolls back (forgets/aborts) the transaction. You should surround your database transactions with these unless you use .Autocommit or CommitRetaining. 

### TSQLQuery

Documentation: [TSQLQuery documentation](<http://www.freepascal.org/docs-html/fcl/sqldb/tsqlquery.html>)

See [Working With TSQLQuery](<Working_With_TSQLQuery.md> "Working With TSQLQuery") for more details. 

[TSQLQuery](<TSQLQuery.md> "TSQLQuery") is an object that embodies a dataset from a connection/transaction pair using its SQL property to determines what data is retrieved from the database into the dataset. 

When working with it, you therefore need to at least specify the transaction, connection amd SQL properties. The TSQLQuery is an important part in the chain that links databound controls to the database. As said, the SQL property determines what SELECT query is run against the database to get data. FPC will try to determine what corresponding SQL INSERT, UPDATE and DELETE statements should be used in order to process user/program generated data changes. If necessary, the programmer can fine tune this and manually specify the InsertSQL, UpdateSQL and DeleteSQL properties. 

### DataSource

A [TDataSource](<TDataSource.md> "TDataSource") object keeps track of where in a dataset (such as TSQLQuery) data bound components are. The programmer should specify the corresponding TSQLQuery object for this to work. 

### Databound controls such as DBGrid

These controls must be linked to a DataSource. They will often have properties that indicate what fields in the DataSource they show. 

## Data modules

[Data modules](<Data_module.md> "Data module") can be used to store non-visual components such as **T*Connection** , **TSQLTransaction** , **TSQLQuery** etc. Data modules also let you share components between forms. 

See [SQLdb Tutorial4](<SQLdb_Tutorial4.md> "SQLdb Tutorial4").

---

_Source: [https://wiki.freepascal.org/SQLdb_Programming_Reference](https://web.archive.org/web/20250219115646/https://wiki.freepascal.org/SQLdb_Programming_Reference)_
