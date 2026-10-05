# Lazarus DB Faq

│ **English (en)** │  **[русский (ru)](<../ru/Lazarus_DB_Faq.md>)** │

---  
[**Databases portal**](<Portal_Databases.md> "Portal:Databases")  
References: 

  * [General info](<Databases.md> "Databases")
  * [Libraries](<Database_libraries.md> "Database libraries")
  * [Field types](<Database_field_type.md> "Database field type")
  * [Controls](<Data_Controls_tab.md> "Data Controls tab")
  * FAQ
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
  
This is a list of Frequently Asked Questions (FAQ) regarding database programming with Lazarus. 

## Contents

  * 1 Where can I find more information?
  * 2 Where can I find database components?
  * 3 Supported databases
  * 4 Known issues
  * 5 Are there other components?
  * 6 Lazarus and FPC documentation
  * 7 Lazarus documentation



### Where can I find more information?

Databases: 

  * See [Databases](<Databases.md> "Databases") and the articles about using databases/SQLQuery.



### Where can I find database components?

At the moment the SQLdb components are part of FPC and Lazarus. They are installed by default in all more or less recent Lazarus versions. 

Manual installation: if you look in the [$LazarusDir]/components you will see a subdirectory SQLdb. Install the sqldblaz.lpk and you will be able to connect to MySQL, Interbase / Firebird, Postgres, MS SQL and Sybase ASE (if you have FPC 2.6.1+), Oracle servers. See [Install Packages](<Install_Packages.md> "Install Packages") for help on installing packages. 

### Supported databases

  * See [Lazarus Database Overview](<Lazarus_Database_Overview.md> "Lazarus Database Overview") for a list of what databases are supported by SQLDB.



### Known issues

  * See [fcl-db#Known issues/shortcomings](<fcl-db.md> "fcl-db")



### Are there other components?

  * See [Lazarus Database Overview](<Lazarus_Database_Overview.md> "Lazarus Database Overview") for a list of what databases work with what components.



### Lazarus and FPC documentation

The Lazarus visual database controls use FPC database code. Please see [SQLDB documentation](<http://www.freepascal.org/docs-html/fcl/sqldb/index.html>) for more information. 

Background info on SQLDB: [SqlDBHowto](<SqlDBHowto.md> "SqlDBHowto")

More info on TSQLQuery: [Working With TSQLQuery](<Working_With_TSQLQuery.md> "Working With TSQLQuery")

### Lazarus documentation

  * Some information on the interaction between the various FPC and Lazarus components: [SQLdb Programming Reference](<SQLdb_Programming_Reference.md> "SQLdb Programming Reference")

---

_Source: [https://wiki.freepascal.org/Lazarus_DB_Faq](https://web.archive.org/web/20250512113811/https://wiki.freepascal.org/Lazarus_DB_Faq)_
