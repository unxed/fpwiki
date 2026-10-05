# Lazarus Database Overview

│ **English (en)** │  **[русский (ru)](<../ru/Lazarus_Database_Overview.md>)** │

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

  * Overview
  * [0 - Database set-up](<SQLdb_Tutorial0.md> "SQLdb Tutorial0")
  * [1 - Getting started](<SQLdb_Tutorial1.md> "SQLdb Tutorial1")
  * [2 - Editing](<SQLdb_Tutorial2.md> "SQLdb Tutorial2")
  * [3 - Queries](<SQLdb_Tutorial3.md> "SQLdb Tutorial3")
  * [4 - Data modules](<SQLdb_Tutorial4.md> "SQLdb Tutorial4")
  * [SQLdb Programming Reference](<SQLdb_Programming_Reference.md> "SQLdb Programming Reference")

Databases  


    [Advantage](<Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<mssqlconn.md> "mssqlconn") \- [Postgres](<postgres.md> "postgres") \- [Interbase](<Firebird.md> "Firebird") \- [Firebird](<Firebird.md> "Firebird") \- [Oracle](<Oracle.md> "Oracle") \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- [dBASE](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<MS_Access.md> "MS Access") \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
## Contents

  * 1 Overview
  * 2 Lazarus and Interbase / Firebird
  * 3 Lazarus and MySQL
  * 4 Lazarus and MSSQL/Sybase
  * 5 Lazarus and ODBC
    * 5.1 Microsoft Access
  * 6 Lazarus and Oracle
  * 7 Lazarus and PostgreSQL
  * 8 Lazarus and SQLite
  * 9 Lazarus and Firebird/Interbase
  * 10 Lazarus and dBase
  * 11 Lazarus and Paradox
  * 12 TSdfDataset and TFixedDataset
  * 13 Lazarus and Advantage Database Server
  * 14 See also
  * 15 External links



## Overview

This article is an overview of which databases can work with Lazarus. 

Lazarus supports several databases out of the box (using e.g. the SQLDB framework), however the developer must install the required packages (client libraries) for each one. 

You can access the database through code or by dropping components on a form. The data-aware components represent fields and are connected by setting the DataSource property to point to a [TDataSource](<TDataSource.md> "TDataSource"). The Datasource represents a table and is connected to the database components (examples: _[TPSQLDatabase](</index.php?title=TPSQLDatabase&action=edit&redlink=1> "TPSQLDatabase \(page does not exist\)")_ , _[TSQLiteDataSet](</index.php?title=TSQLiteDataSet&action=edit&redlink=1> "TSQLiteDataSet \(page does not exist\)")_) by setting the DataSet property. The data-aware components are located on the [Data Controls tab](<Data_Controls_tab.md> "Data Controls tab"). The Datasource and the database controls are located on the "Data Access" tab. 

See the tutorials for Lazarus/FPC built in database access, suitable for Firebird, MySQL, SQLite, PostgreSQL etc: 

  * [SQLdb Tutorial0](<SQLdb_Tutorial0.md> "SQLdb Tutorial0")
  * [SQLdb Tutorial1](<SQLdb_Tutorial1.md> "SQLdb Tutorial1")
  * [SQLdb Tutorial2](<SQLdb_Tutorial2.md> "SQLdb Tutorial2")
  * [SQLdb Tutorial3](<SQLdb_Tutorial3.md> "SQLdb Tutorial3")
  * [SQLdb Tutorial4](<SQLdb_Tutorial4.md> "SQLdb Tutorial4")



## Lazarus and Interbase / Firebird

  * Firebird is very well supported out of the box by FPC/Lazarus (using SQLDB); please see [Firebird](<Firebird.md> "Firebird") for details.
  * [Other Firebird libraries](<Other_Firebird_libraries.md> "Other Firebird libraries") has a list of alternative access libraries (e.g. PDO, Zeos, FBlib)



## Lazarus and MySQL

  * Please see [mysql](<mysql.md> "mysql") for details on various access methods, which include:


  1. Built-in [SQLdb](<SQLdb_Package.md> "SQLdb Package") support
  2. PDO
  3. [Zeos](<ZeosDBO.md> "ZeosDBO")
  4. [MySQL data access Lazarus components](<https://www.devart.com/mydac/>)



## Lazarus and MSSQL/Sybase

You can connect to Microsoft SQL Server databases using 

  1. [SQL Server data access Lazarus components](<https://www.devart.com/sdac/>).They are working on Windows and macOS. Free to download.
  2. The built-in **SQLdb** connectors **TMSSQLConnection** and **TSybaseConnection** (since Lazarus 1.0.8/FPC 2.6.2): see [mssqlconn](<mssqlconn.md> "mssqlconn").
  3. **Zeos** component **TZConnection** (latest CVS, see links to Zeos elsewhere on this page) 
     1. On Windows you can choose between native library **ntwdblib.dll** (protocol **mssql**) or FreeTDS libraries (protocol **FreeTDS_MsSQL-nnnn**) where nnnn is one of four variants depending on the server version. For Delphi (not Lazarus) there is also another Zeos protocol **ado** for MSSQL 2005 or later. Using protocols mssql or ado generates code not platform independient.
     2. On Linux the only way is with FreeTDS protocols and libraries (you should use **libsybdb.so**).
  4. **ODBC** (MSSQL and Sybase ASE) with SQLdb **TODBCConnection** (consider using **TMSSQLConnection** and **TSybaseConnection** instead) 
     1. See also [[1]](<ODBCConn.md>)
     2. On Windows it uses native ODBC Microsoft libraries (like sqlsrv32.dll for MSSQL 2000)
     3. On Linux it uses unixODBC + FreeTDS (packages unixodbc or iodbc, and tdsodbc). Since 2012 there is also a Microsoft SQL Server ODBC Driver 1.0 for Linux which is a binary product (no open source) and provides native connectivity, but was released only for 64 bits and only for RedHat.



## Lazarus and ODBC

ODBC is a general database connection standard which is available on Linux, Windows and macOS. You will need an ODBC driver from your database vendor and set up an ODBC "data source" (also known as DSN). You can use the SQLDB components ([TODBCConnection](<TODBCConnection.md> "TODBCConnection")) to connect to an ODBC data soruce. See [ODBCConn](<ODBCConn.md> "ODBCConn") for more details and examples. 

### Microsoft Access

You can use the ODBC driver on Windows as well as Linux to access Access databases; see [MS Access](<MS_Access.md> "MS Access")

## Lazarus and Oracle

  * See [Oracle](<Oracle.md> "Oracle"). Access methods include:


  1. Built-in SQLDB support
  2. Zeos
  3. [Oracle data access Lazarus component](<https://www.devart.com/odac/>)



## Lazarus and PostgreSQL

  * PostgreSQL is very well supported out of the box by FPC/Lazarus
  * Please see [postgres](<postgres.md> "postgres") for details on various access methods, which include:


  1. Built-in SQLdb support. Use component **TPQConnection** from the [SQLdb tab](<SQLdb_tab.md> "SQLdb tab") of the [Component Palette](<Component_Palette.md> "Component Palette")
  2. [Zeos](<Zeos.md> "Zeos"). Use component **TZConnection** with protocol 'postgresql' from palette **Zeos Access**
  3. [PostgreSQL data access Lazarus component](<https://www.devart.com/pgdac/>)



## Lazarus and SQLite

SQLite is an embedded database; the database code can be distributed as a library (.dll/.so/.dylib) with your application to make it self-contained (comparable to Firebird embedded). SQLite is quite popular due to its relative simplicity, speed, small size and cross-platform support. 

Please see the [SQLite](<SQLite.md> "SQLite") page for details on various access methods, which include: 

  1. Built-in SQLDb support. Use component **TSQLite3Connection** from palette **SQLdb**
  2. Zeos
  3. SQLitePass
  4. TSQLite3Dataset
  5. [SQLite data access Lazarus components](<https://www.devart.com/litedac/>)



## Lazarus and Firebird/Interbase

InterBase (and FireBird) Data Access Components (IBDAC) is a library of components that provides native connectivity to InterBase, Firebird and Yaffil from Lazarus (and Free Pascal) on Windows, macOS, iOS, Android, Linux, and FreeBSD for both 32-bit and 64-bit platforms. IBDAC-based applications connect to the server directly using the InterBase client. IBDAC is designed to help programmers develop faster and cleaner InterBase database applications. 

IBDAC is a complete replacement for standard InterBase connectivity solutions. It presents an efficient alternative to InterBase Express Components, the Borland Database Engine (BDE), and the standard dbExpress driver for access to InterBase. 

[Firebird data access components for Lazarus](<https://www.devart.com/ibdac/download.html>) are free to download. 

## Lazarus and dBase

FPC includes a simple database component that is derived from the Delphi TTable component called "TDbf" [TDbf Website](<http://tdbf.sourceforge.net/>)). It supports various DBase and Foxpro formats. 

**TDbf** does not accept SQL commands but you can use the dataset methods etc and you can also use regular databound controls such as the DBGrid. 

It doesn't require any sort of runtime database engine. However it's not the best option for large database applications. 

See the [TDbf Tutorial page](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") for the tutorial as well as documentation. 

You can use e.g. OpenOffice/LibreOffice Base to visually create/edit dbf files, or create DBFs in code using [TDbf](<TDbf.md> "TDbf"). 

## Lazarus and Paradox

Paradox was the default format for database files in old versions of Delphi. The concept is similar to DBase files/DBFs, where the "database" is a folder, and each table is a file inside that folder. Also, each index is a file too. To access this files from Lazarus we have these options: 

  * **[TParadox](<TParadox.md> "TParadox")** : Install package "lazparadox 0.0" included in the standard distribution. When you install this package, you will see a new component labeled "PDX" in the "Data Access" palette. This component is not standalone, it uses a "native" library, namely the [pdxlib library](<http://pxlib.sourceforge.net>) which is available for Linux and Windows. For example, to install in Debian, you could get **pxlib1** from package manager. In Windows you need the pxlib.dll file.


  * **[TPdx](</index.php?title=TPdx&action=edit&redlink=1> "TPdx \(page does not exist\)")** : Paradox DataSet for Lazarus and Delphi from [this site](<http://tpdx.sourceforge.net/>). This component is standalone (pure object pascal), not requiring any external library, but it can only read (not write) Paradox files. The package to install is "paradoxlaz.lpk" and the component should appear in the "Data Access" palette with PDX label (but orange colour).


  * **[TParadoxDataSet](<TParadoxDataSet.md> "TParadoxDataSet")** : is a [TDataSet](<TDataSet.md> "TDataSet") that can only read Paradox Files up to Version 7. The approach is similar to the TPdx component, the package to install is "lazparadox.lpk" and the component should also appear in the "Data Access" palette.



## TSdfDataset and TFixedDataset

[TSdfDataSet](<TSdfDataSet.md> "TSdfDataSet") and [TFixedFormatDataSet](<TFixedFormatDataSet.md> "TFixedFormatDataSet") are two simple [TDataSet](<TDataSet.md> "TDataSet") descandants which offer a very simple textual storage format. These datasets are very convenient for small databases, because they are fully implemented as an Object Pascal unit, and thus require no external libraries. Also, their textual format allows them to be easily viewed/edited with a text editor. 

See [CSV](<CSV.md> "CSV") for example code. 

## Lazarus and Advantage Database Server

  * Please see [Advantage Database Server](<Advantage_Database_Server.md> "Advantage Database Server") for details on using Advantage Database Server



## See also

(Sorted alphabetically) 

  * [Database Portal](<Portal_Databases.md> "Portal:Databases")
  * [Databases](<Databases.md> "Databases")
  * [Database_field_type](<Database_field_type.md> "Database field type")
  * [How to write in-memory database applications in Lazarus/FPC](<How_to_write_in-memory_database_applications_in_Lazarus/FPC.md> "How to write in-memory database applications in Lazarus/FPC")
  * [Lazarus DB Faq](<Lazarus_DB_Faq.md> "Lazarus DB Faq")
  * [Lazarus Tdbf Tutorial](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial")
  * [Multi-tier options with FPC](<multi-tier_options_with_fpc.md> "multi-tier options with fpc")
  * [SQLdb Tutorial1](<SQLdb_Tutorial1.md> "SQLdb Tutorial1")
  * [SqlDBHowto](<SqlDBHowto.md> "SqlDBHowto")
  * [tiOPF](<tiOPF.md> "tiOPF") \- a free and open source Object Persistence Framework.
  * [Zeos tutorial](<Zeos_tutorial.md> "Zeos tutorial")



## External links

  * [Pascal Data Objects](<http://pdo.sourceforge.net>) \- a database API that worked for both FPC and Delphi and utilises native MySQL libraries for version 4.1 and 5.0 and Firebird SQL 1.5, and 2.0. It's inspired by PHP's PDO class.
  * [Zeos+SQLite Tutorial](<http://lazaruszeos.blogspot.com>) \- Good tutorial using screenshots and screencasts it explain how to use SQLite and Zeos, spanish (google translate does a good work in translating it to english)

---

_Source: [https://wiki.freepascal.org/Lazarus_Database_Overview](https://web.archive.org/web/20250407173417/https://wiki.freepascal.org/Lazarus_Database_Overview)_
