# SQLdb Tutorial4

│ **English (en)** │  **[français (fr)](</SQLdb_Tutorial4/fr> "SQLdb Tutorial4/fr")** │  **[日本語 (ja)](</SQLdb_Tutorial4/ja> "SQLdb Tutorial4/ja")** │    
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
  * 4 - Data modules
  * [SQLdb Programming Reference](<SQLdb_Programming_Reference.md> "SQLdb Programming Reference")

Databases  


    [Advantage](<Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<mssqlconn.md> "mssqlconn") \- [Postgres](<postgres.md> "postgres") \- [Interbase](<Firebird.md> "Firebird") \- [Firebird](<Firebird.md> "Firebird") \- [Oracle](<Oracle.md> "Oracle") \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- [dBASE](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<MS_Access.md> "MS Access") \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
## Contents

  * 1 Introduction
  * 2 Why use datamodules?
  * 3 Getting started
  * 4 Everyone has their own way
  * 5 More uses of data modules
    * 5.1 Additional components
    * 5.2 Debugging



## Introduction

This tutorial is an attempt to demonstrate the use of [Lazarus Data Modules](<Data_module.md> "Data module") to isolate the data access components of a project from the program logic associated with the access. Such isolation makes program maintenance and debugging easier. 

The tutorial was developed using Windows 7 and SQLite3 as the database, with Lazarus 1.0.8 with FPC 2.6.2; however it should work with earlier versions. Similarly, other DBMS and Operating Systems should require minimal change, if any. 

## Why use datamodules?

Simple - after following the Lazarus Tutorials: 

  * [SQLdb Tutorial0](<SQLdb_Tutorial0.md> "SQLdb Tutorial0")
  * [SQLdb Tutorial1](<SQLdb_Tutorial1.md> "SQLdb Tutorial1")
  * [SQLdb Tutorial2](<SQLdb_Tutorial2.md> "SQLdb Tutorial2")
  * [SQLdb Tutorial3](<SQLdb_Tutorial3.md> "SQLdb Tutorial3")



developing a 'real' application becomes harder. 'Form1' grows to an exponential size handling the different Events and Database Queries. 

Isolating each table access into a single datamodule makes debugging and maintenance so much easier. An application may have any number of datamodules - a small application with just one or 2 tables can probably suffice with just one Datamodule - a larger application could probably benefit from having a data module for each table or view. 

The sample shown here uses just 2 tables with simple queries, but it can be expanded to include more options with each table. 

## Getting started

In the Lazarus IDE, create a new Application and then click File --> New --> Data Module 

[![newDM.jpg](https://wiki.freepascal.org/images/4/44/newDM.jpg)](</File:newDM.jpg>)

You will be presented with a window as if you selected 'New Form': 

[![dm1.jpg](https://wiki.freepascal.org/images/3/3e/dm1.jpg)](</File:dm1.jpg>)

The difference is this window/form will only accept non-visual components. In fact, look at your Component Palette: it has been greatly reduced to only allow selection of ONLY non-visual components. 

  


## Everyone has their own way

Your use of data modules will vary to suit your own needs. but as an example, I have at least 2 data modules: 

Unit: DataModule1 On this module I 'drop' a _T*Connection_ and a _TSQLTransaction_. 

[![conn.jpg](https://wiki.freepascal.org/images/d/dc/conn.jpg)](</File:conn.jpg>)

DataModule1 is used as the connection for all queries. 

I then create a DataModuleN for each table or view. 

[![dm2.jpg](https://wiki.freepascal.org/images/8/89/dm2.jpg)](</File:dm2.jpg>)

Each DataModuleN will need to have the DataModule1 unit (unit2 in this example) added to the USES clause to connect to the database. 

From here everything is the same as stated in [SQLdb Tutorial1](<SQLdb_Tutorial1.md> "SQLdb Tutorial1"). The components are connected in the same way and access is identical. 

## More uses of data modules

### Additional components

In this example, DataModule1 had nothing more than a Connection and Transaction, but in a 'real' application, this container would typically also hold global non-visual components to be used by the application. 

For example, Load and Save INI settings, _TOpenDialog_ , _TSaveDialog_ , etc. The concept here is to isolate data access from the business logic of an application. A change in data source for any application is never a minimal task, but having the datasources isolated will make the change much easier. 

### Debugging

Debugging a program is also a difficult task. By separating data access and business logic, the code to be viewed is halved. Data access and business logic can be tested separately to at least halve the problem. 

The importance of the _DataModule_ will become even more obvious when developing other applications using the same database and tables. The data module can of course be reused in the new application.

---

_Source: [https://wiki.freepascal.org/SQLdb_Tutorial4](https://web.archive.org/web/20250331230054/https://wiki.freepascal.org/SQLdb_Tutorial4)_
