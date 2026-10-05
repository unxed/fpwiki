# TParadoxDataSet

│ **English (en)** │  **[français (fr)](</TParadoxDataSet/fr> "TParadoxDataSet/fr")** │  **[português (pt)](</TParadoxDataSet/pt> "TParadoxDataSet/pt")** │    
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
  
![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Paradox uses the **BDE** (Borland Database Engine) that is unmaintained and unsupported since around 2000.

[![tparadoxdataset 150.png](https://wiki.freepascal.org/images/a/a7/tparadoxdataset_150.png)](</File:tparadoxdataset_150.png>) **TParadoxDataSet** is a `[TDataSet](<TDataSet.md> "TDataSet")` that can read (_**but not write!**_) Paradox files up to Version 7. 

Its main characteristics are : 

  * Data-aware implementation, i.e. the component is a descendant of TDataset, and you can use the standard data-aware Lazarus components to display the table contents.
  * All table levels up to version 7
  * No external DLL needed (unlike the [TParadox](<TParadox.md> "TParadox") component that comes with Lazarus). Works in 32- and 64-bit operating systems.
  * Now supports BLOB fields, character encoding, filtering and bookmarks.



The motivation for having the component read-only is to provide a way to read old Paradox files for converting to newer database formats. 

## Contents

  * 1 Author/License
  * 2 Download
  * 3 Bug reporting / Feature Request
  * 4 SVN
  * 5 Change Log
  * 6 Installation
  * 7 Usage
  * 8 See also



### Author/License

  * Author: [Christian Ulrich](</User:Christian> "User:Christian")
  * License: [Mozilla Public License 1.1](<https://opensource.org/licenses/MPL-1.1>), or [LGPL](<http://www.opensource.org/licenses/lgpl-license.php>)



### Download

The latest stable release (v0.2) can be found at 

  * [Lazarus CCR](<https://sourceforge.net/projects/lazarus-ccr/files/Paradox%20DataSet/tparadoxdataset-v0.2.zip/download>)



### Bug reporting / Feature Request

Please report bugs to the [Lazarus Bugtracker](<http://www.freepascal.org/mantis/set_project.php?project_id=3>) in Category Packages 

### SVN

You can get the latest source via SVN: 
    
    
    svn checkout https://svn.code.sf.net/p/lazarus-ccr/svn/components/tparadoxdataset
    

### Change Log

  * 31 January 2007 - Initial release
  * May 2019 - Update to current FPC/Lazarus versions, 64-bit, BLOB support, filtering, bookmarks, character encoding



Status: Beta 

### Installation

  * If needed, unzip the files from the zip file to any directory.
  * In Lazarus, open the package .lpk file with "Package" > "Open package file (.lpk)". In v0.2 there are two packages; both have the same content, but **lazparadox.lpk** has a naming conflict with the [Paradox](<TParadox.md> "TParadox") package contained in the Lazarus distribution; therefore it is recommended to install **lazparadoxpkg.lpk** instead.
  * If you don't want to install the component into the IDE, click on "Compile".
  * If you want to install the component into the IDE, click on "Use" > "Install". Confirm the prompt to rebuild the IDE. When Lazarus restarts you'll find the component `TParadoxDataset` on palette "Data Access".



### Usage

  * Drop the `TParadoxDataset` component on the form.
  * Specify the name of the table file (extension ".db") in property `TableName`
  * Set `Active` to `true` in order to open the table. When, for example, a DBGrid is linked to the dataset the table is immediately displayed.
  * The **encoding** of text fields used in the file is displayed in the read-only property `InputEncoding`. It is automatically converted to the encoding given in property `TargetEncoding` which defaults to "UTF8". In the case that text fields must be encoded differently specify the codepage name here, e.g. "CP1252" for Western Europe code page (use the (case-insensitive) names given in the LazUtils unit _lconvencoding_).
  * **Filtering** : Set `Filtered` to `false`. Then define the filter conditon in property `Filter` or use the `OnFilterRecord` event. Finally start the filtering action by setting `Filtered` to `true`.
  * **Bookmarks** : Store the currently active record as a bookmark by setting the variable `bookmark` (which is of type `TBookmark` to `ParadoxDataset1.GetBookmark`. For returning to this record, call `ParadoxDataset1.GotoBookmark(boomark)`. Since the component uses the `RecNo` as bookmarks it is not required to call `FreeBookmark`.



## See also

  * [TParadox](<TParadox.md> "TParadox")

---

_Source: [https://wiki.freepascal.org/TParadoxDataSet](https://web.archive.org/web/20250409070956/https://wiki.freepascal.org/TParadoxDataSet)_
