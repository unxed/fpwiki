# Advantage Database Server

│ **English (en)** │

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


    Advantage \- [MySQL](<MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<mssqlconn.md> "mssqlconn") \- [Postgres](<postgres.md> "postgres") \- [Interbase](<Firebird.md> "Firebird") \- [Firebird](<Firebird.md> "Firebird") \- [Oracle](<Oracle.md> "Oracle") \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- [dBASE](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<MS_Access.md> "MS Access") \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
## Contents

  * 1 Overview
  * 2 Installing the Advantage TDataSet
    * 2.1 Windows
    * 2.2 Linux
  * 3 Installing the Advantage Package
  * 4 A simple Advantage project



## Overview

This brief tutorial is intended to get you started using the Advantage [TDataSet](<TDataSet.md> "TDataSet") descendent to access tables hosted by the **Advantage Database Server**. 

## Installing the Advantage TDataSet

### Windows

If installing the Advantage TDataSet on Windows, you will need to download and install the Advantage Delphi Components (version 10.1 or greater). The install media can be obtained from the Advantage web site [here.](<http://www.advantagedatabase.com>)

### Linux

If installing the Advantage TDataSet on Linux, you will need to complete a two-part installation. First, download and install the Advantage Client Engine for Linux (version 10.1). Second, download and extract the Linux TDataSet source tarball. Both downloads are available at the Advantage Developer Zone at <http://devzone.advantagedatabase.com>. (The Advantage Client Engine download is located in the "Product Download" section of the site, and the Linux TDataSet source is available in the Delphi Applications section of the CodeCentral page of the Advantage Developer Zone.) 

## Installing the Advantage Package

Once the TDataSet Descendant is installed, you'll want to move on to installing the package into Lazarus. 

  1. From Lazarus, click on "Package", then "Open Package File (.lpk)..." and browse to the adsl.lpk file in the TDataSet installation directory (or in the directory you extracted the TDataSet source).
  2. In the package window, click the "Compile" button.
  3. Upon successful compilation, click the "Install" button, and select "Yes" to rebuild Lazarus.
  4. The Lazarus IDE should successfully compile and re-start with the Advantage components installed.



## A simple Advantage project

Start a new project to start working with Advantage data. 

  1. Drop a TAdsConnection object from the Advantage tab of the palette onto your form. (The TAdsConnection object is the left-most object on the Advantage tab.)
  2. In the Object Inspector, click the drop-down for the AliasName property, and select "ADTDemoData". 
     * Alternately, you can select the ConnectPath property and enter (or browse to) the path to the Help\ADS_Data directory beneath your TDataSet (or acesdk on Linux) install directory.
  3. Expand the AdsServerTypes property, and change the "stADS_LOCAL" property to True. 
     * If you have an Advantage Database server configured and running, you may set "stADS_REMOTE" to true instead.
  4. Drop a TAdsTable object on the form. (The TAdsTable object is immediately to the right of the TAdsConnection object on the tab, and it looks like a red table.)
  5. Set the AdsConnection Property for AdsTable1 to AdsConnection1 (the connection object you just dropped in step 1.)
  6. Scroll down to the "TableName" property, and use the drop-down box to select the biolife.adt table.
  7. Drop a [TDataSource](<TDataSource.md> "TDataSource") component on the form, and set its DataSet property to "AdsTable1".
  8. Drop a [TDBGrid](<TDBGrid.md> "TDBGrid") component on the form, and set its "DataSource" property to "Datasource1".
  9. At this point, you should be able to select the AdsTable1 object and set its "Active" property to "True".
  10. Finally, run the project. (Note that you may need to save the project before running it.)



Since the Advantage components descend from the [TDataSet](<TDataSet.md> "TDataSet"), you can use these components with any data-aware component that supports the TDataSet. This extremely simple example should demonstrate how simple it is to get started with Advantage.

---

_Source: [https://wiki.freepascal.org/Advantage_Database_Server](https://web.archive.org/web/20250421234236/https://wiki.freepascal.org/Advantage_Database_Server)_
