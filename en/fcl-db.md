# fcl-db

│ **English (en)** │  **[français (fr)](</fcl-db/fr> "fcl-db/fr")** │  **[polski (pl)](</fcl-db/pl> "fcl-db/pl")** │    
****

The package **fcl-db** contains most of FPC's higher level database system, plus table drivers for some popular systems. (<LAZDIR>/fpc/3.0.0/source/packages/fcl-db) 

## Known issues/shortcomings

  * Master - detail relations are not complete; however you can implement it using [MasterDetail](<MasterDetail.md> "MasterDetail")
  * Calculated field support is not complete
  * No binary data transfer using parameters (everything is converted to ASCII) in most drivers.
  * There are some floating point issues, with precision and scale parameters only minimally supported in some drivers (amongst others mysql)
  * Most character encoding issues are solved fairly ad hoc. There is no way to set the fundamental encodings manually: 
    1. the encoding of the connection
    2. the encoding of the components' internal storage
    3. the encoding of GUI components
    4. (optionally, encoding of exports, or fileformats)
  * Many driver dependent issues in datetime types and timezone support.
  * Between the drivers, Firebird is used the most, then Mysql, SQLite, PostgreSQL and ODBC. Finally comes Oracle which is mostly still at a proof of concept level. The Microsoft SQL Server and Sybase ASE drivers are a recent addition to FPC (2.6.1 and higher) and Lazarus.
  * Before FPC 2.6: no stored procedure resultset >1 row



Most of these are being worked on, and the status changes on a regular basis, so be sure to do your own testing and source inspection, since this will be most certainly out of date. 

A [testsuite](<Databases.md> "Databases") exists (test/testresult-db) and is expanded when new bugs and functionality occur. 

## Units

(In the below table the subdirectory is listed as "submodule", so one can see easily to which subsystem the unit belongs. 

Unit | submodule | comment   
---|---|---  
[browseds](</index.php?title=browseds&action=edit&redlink=1> "browseds \(page does not exist\)") | sqlite |   
[bufdataset](</index.php?title=bufdataset&action=edit&redlink=1> "bufdataset \(page does not exist\)") | base | In memory dataset. More capable than memds. See [TBufDataSet](<How_to_write_in-memory_database_applications_in_Lazarus/FPC.md> "How to write in-memory database applications in Lazarus/FPC") for some usage examples   
[bufdataset_parser](</index.php?title=bufdataset_parser&action=edit&redlink=1> "bufdataset parser \(page does not exist\)") | base |   
[concurrencyds](</index.php?title=concurrencyds&action=edit&redlink=1> "concurrencyds \(page does not exist\)") | sqlite |   
[createds](</index.php?title=createds&action=edit&redlink=1> "createds \(page does not exist\)") | sqlite |   
[customsqliteds](</index.php?title=customsqliteds&action=edit&redlink=1> "customsqliteds \(page does not exist\)") | sqlite |   
[db](</index.php?title=db&action=edit&redlink=1> "db \(page does not exist\)") | base |   
[dbcoll](</index.php?title=dbcoll&action=edit&redlink=1> "dbcoll \(page does not exist\)") | base |   
[dbconst](</index.php?title=dbconst&action=edit&redlink=1> "dbconst \(page does not exist\)") | base |   
[dbf](</index.php?title=dbf&action=edit&redlink=1> "dbf \(page does not exist\)") | dbase | TDBF components for DBase/FoxPro/Visual Foxpro tables (upstream code: Sourceforge TDBF project). See also [Lazarus Tdbf Tutorial](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial")  
[dbf_avl](</index.php?title=dbf_avl&action=edit&redlink=1> "dbf avl \(page does not exist\)") | dbase |   
[dbf_collate](</index.php?title=dbf_collate&action=edit&redlink=1> "dbf collate \(page does not exist\)") | dbase |   
[dbf_common](</index.php?title=dbf_common&action=edit&redlink=1> "dbf common \(page does not exist\)") | dbase |   
[dbf_cursor](</index.php?title=dbf_cursor&action=edit&redlink=1> "dbf cursor \(page does not exist\)") | dbase |   
[dbf_dbffile](</index.php?title=dbf_dbffile&action=edit&redlink=1> "dbf dbffile \(page does not exist\)") | dbase |   
[dbf_fields](</index.php?title=dbf_fields&action=edit&redlink=1> "dbf fields \(page does not exist\)") | dbase |   
[dbf_idxcur](</index.php?title=dbf_idxcur&action=edit&redlink=1> "dbf idxcur \(page does not exist\)") | dbase |   
[dbf_idxfile](</index.php?title=dbf_idxfile&action=edit&redlink=1> "dbf idxfile \(page does not exist\)") | dbase |   
[dbf_lang](</index.php?title=dbf_lang&action=edit&redlink=1> "dbf lang \(page does not exist\)") | dbase |   
[dbf_memo](</index.php?title=dbf_memo&action=edit&redlink=1> "dbf memo \(page does not exist\)") | dbase |   
[dbf_parser](</index.php?title=dbf_parser&action=edit&redlink=1> "dbf parser \(page does not exist\)") | dbase |   
[dbf_pgcfile](</index.php?title=dbf_pgcfile&action=edit&redlink=1> "dbf pgcfile \(page does not exist\)") | dbase |   
[dbf_pgfile](</index.php?title=dbf_pgfile&action=edit&redlink=1> "dbf pgfile \(page does not exist\)") | dbase |   
[dbf_prscore](</index.php?title=dbf_prscore&action=edit&redlink=1> "dbf prscore \(page does not exist\)") | dbase |   
[dbf_prsdef](</index.php?title=dbf_prsdef&action=edit&redlink=1> "dbf prsdef \(page does not exist\)") | dbase |   
[dbf_prssupp](</index.php?title=dbf_prssupp&action=edit&redlink=1> "dbf prssupp \(page does not exist\)") | dbase |   
[dbf_reg](</index.php?title=dbf_reg&action=edit&redlink=1> "dbf reg \(page does not exist\)") | dbase |   
[dbf_str](</index.php?title=dbf_str&action=edit&redlink=1> "dbf str \(page does not exist\)") | dbase |   
[dbf_str_es](</index.php?title=dbf_str_es&action=edit&redlink=1> "dbf str es \(page does not exist\)") | dbase |   
[dbf_str_fr](</index.php?title=dbf_str_fr&action=edit&redlink=1> "dbf str fr \(page does not exist\)") | dbase |   
[dbf_str_ita](</index.php?title=dbf_str_ita&action=edit&redlink=1> "dbf str ita \(page does not exist\)") | dbase |   
[dbf_str_nl](</index.php?title=dbf_str_nl&action=edit&redlink=1> "dbf str nl \(page does not exist\)") | dbase |   
[dbf_str_pl](</index.php?title=dbf_str_pl&action=edit&redlink=1> "dbf str pl \(page does not exist\)") | dbase |   
[dbf_str_pt](</index.php?title=dbf_str_pt&action=edit&redlink=1> "dbf str pt \(page does not exist\)") | dbase |   
[dbf_str_ru](</index.php?title=dbf_str_ru&action=edit&redlink=1> "dbf str ru \(page does not exist\)") | dbase |   
[dbf_wtil](</index.php?title=dbf_wtil&action=edit&redlink=1> "dbf wtil \(page does not exist\)") | dbase |   
[dblib](<mssqlconn.md> "mssqlconn") |  | Wrapper around FreeTDS; required for the mssqlconn SQLDB driver for Microsoft SQL Server and Sybase ASE   
[dbwhtml](</index.php?title=dbwhtml&action=edit&redlink=1> "dbwhtml \(page does not exist\)") | base |   
[fillds](</index.php?title=fillds&action=edit&redlink=1> "fillds \(page does not exist\)") | sqlite |   
[fpcgcreatedbf](</index.php?title=fpcgcreatedbf&action=edit&redlink=1> "fpcgcreatedbf \(page does not exist\)") | codegen |   
[fpcgdbcoll](</index.php?title=fpcgdbcoll&action=edit&redlink=1> "fpcgdbcoll \(page does not exist\)") | codegen |   
[fpcgsqlconst](</index.php?title=fpcgsqlconst&action=edit&redlink=1> "fpcgsqlconst \(page does not exist\)") | codegen |   
[fpcgtiopf](</index.php?title=fpcgtiopf&action=edit&redlink=1> "fpcgtiopf \(page does not exist\)") | codegen |   
[fpcsvexport](</index.php?title=fpcsvexport&action=edit&redlink=1> "fpcsvexport \(page does not exist\)") | export |   
[fpdatadict](</index.php?title=fpdatadict&action=edit&redlink=1> "fpdatadict \(page does not exist\)") | datadict |   
[fpdbexport](<fpDBExport.md> "fpDBExport") | export | See the dbftool example included in FPC 2.7.1+: creating, using DBF files and exporting data using db export. Also used in the Lazarus db export component.   
[fpdbfexport](<fpdbfexport.md> "fpdbfexport") | export |   
[fpddcodegen](</index.php?title=fpddcodegen&action=edit&redlink=1> "fpddcodegen \(page does not exist\)") | codegen |   
[fpdddbf](</index.php?title=fpdddbf&action=edit&redlink=1> "fpdddbf \(page does not exist\)") | datadict |   
[fpdddiff](</index.php?title=fpdddiff&action=edit&redlink=1> "fpdddiff \(page does not exist\)") | datadict |   
[fpddfb](</index.php?title=fpddfb&action=edit&redlink=1> "fpddfb \(page does not exist\)") | datadict |   
[fpddmysql40](</index.php?title=fpddmysql40&action=edit&redlink=1> "fpddmysql40 \(page does not exist\)") | datadict |   
[fpddmysql41](</index.php?title=fpddmysql41&action=edit&redlink=1> "fpddmysql41 \(page does not exist\)") | datadict |   
[fpddmysql50](</index.php?title=fpddmysql50&action=edit&redlink=1> "fpddmysql50 \(page does not exist\)") | datadict |   
[fpddodbc](</index.php?title=fpddodbc&action=edit&redlink=1> "fpddodbc \(page does not exist\)") | datadict |   
[fpddoracle](</index.php?title=fpddoracle&action=edit&redlink=1> "fpddoracle \(page does not exist\)") | datadict |   
[fpddpopcode](</index.php?title=fpddpopcode&action=edit&redlink=1> "fpddpopcode \(page does not exist\)") | codegen |   
[fpddpq](</index.php?title=fpddpq&action=edit&redlink=1> "fpddpq \(page does not exist\)") | datadict |   
[fpddregstd](</index.php?title=fpddregstd&action=edit&redlink=1> "fpddregstd \(page does not exist\)") | datadict |   
[fpddsqldb](</index.php?title=fpddsqldb&action=edit&redlink=1> "fpddsqldb \(page does not exist\)") | datadict |   
[fpddsqlite3](</index.php?title=fpddsqlite3&action=edit&redlink=1> "fpddsqlite3 \(page does not exist\)") | datadict |   
[fpfixedexport](</index.php?title=fpfixedexport&action=edit&redlink=1> "fpfixedexport \(page does not exist\)") | export | Dataset export to fixed width text format   
[fprtfexport](</index.php?title=fprtfexport&action=edit&redlink=1> "fprtfexport \(page does not exist\)") | export | Dataset export to RTF format   
[fpsimplejsonexport](</index.php?title=fpsimplejsonexport&action=edit&redlink=1> "fpsimplejsonexport \(page does not exist\)") | export | Dataset export to JSON format   
[fpsimplexmlexport](</index.php?title=fpsimplexmlexport&action=edit&redlink=1> "fpsimplexmlexport \(page does not exist\)") | export | Dataset export to ASCII encoded XML   
[fpsqlexport](<fpsqlexport.md> "fpsqlexport") | export | Dataset export to SQL Insert/Update statements   
[fpstdexports](</index.php?title=fpstdexports&action=edit&redlink=1> "fpstdexports \(page does not exist\)") | export |   
[fptexexport](</index.php?title=fptexexport&action=edit&redlink=1> "fptexexport \(page does not exist\)") | export | Dataset export to Latex format   
[fpXMLXSDExport](<fpXMLXSDExport.md> "fpXMLXSDExport") | export | Dataset export to various XML formats: Access, ADO.Net, Excel, Delphi ClientDataset   
[ibconnection](</index.php?title=ibconnection&action=edit&redlink=1> "ibconnection \(page does not exist\)") | sqldb/interbase |   
[memds](</index.php?title=memds&action=edit&redlink=1> "memds \(page does not exist\)") | memds | In memory dataset. Not as capable as bufdataset. See [How_to_write_in-memory_database_applications_in_Lazarus](</index.php?title=How_to_write_in-memory_database_applications_in_Lazarus&action=edit&redlink=1> "How to write in-memory database applications in Lazarus \(page does not exist\)") for some usage examples   
[mssqlconn](<mssqlconn.md> "mssqlconn") | sqldb/mssqlconn | Microsoft SQL Server and Sybase ASE drivers, introduced in FPC 2.6.1. Requires dblib.   
[mysql40conn](<mysql.md> "mysql") | sqldb/mysql | Connector for MySQL server using MySQL 4.0 client library   
[mysql41conn](<mysql.md> "mysql") | sqldb/mysql | Connector for MySQL server using MySQL 4.1 client library   
[mysql4conn](<mysql.md> "mysql") | sqldb/mysql | Connector for MySQL server using MySQL 4?? client library   
[mysql50conn](<mysql.md> "mysql") | sqldb/mysql | Connector for MySQL server using MySQL 5.0 client library   
[mysql51conn](<mysql.md> "mysql") | sqldb/mysql | Connector for MySQL server using MySQL 5.1 client library   
[mysql55conn](<mysql.md> "mysql") | sqldb/mysql | Connector for MySQL server using MySQL 5.5 client library   
[odbcconn](<ODBCConn.md> "ODBCConn") | sqldb/odbc | Connector for ODBC databases (e.g. Microsoft Access, DB2)   
[oracleconnection](</index.php?title=oracleconnection&action=edit&redlink=1> "oracleconnection \(page does not exist\)") | sqldb/oracle | Connector for Oracle (XE) databases   
[pqconnection](<postgresql.md> "postgresql") | sqldb/postgres | Connector for PostgreSQL databases   
[sqlite3conn](<SQLite.md> "SQLite") | sqldb/sqlite |   
[paradox](</index.php?title=paradox&action=edit&redlink=1> "paradox \(page does not exist\)") | paradox |   
[sdfdata](</index.php?title=sdfdata&action=edit&redlink=1> "sdfdata \(page does not exist\)") | sdf | CSV dataset support   
[sqldb](</index.php?title=sqldb&action=edit&redlink=1> "sqldb \(page does not exist\)") | sqldb |   
[sqlite3ds](</index.php?title=sqlite3ds&action=edit&redlink=1> "sqlite3ds \(page does not exist\)") | sqlite |   
[sqliteds](</index.php?title=sqliteds&action=edit&redlink=1> "sqliteds \(page does not exist\)") | sqlite |   
[sqlscript](</index.php?title=sqlscript&action=edit&redlink=1> "sqlscript \(page does not exist\)") | base | SQL scripting component that lets you run multiple SQL statement as a batch. Has support for Firebird SET TERM. See example in Lazarus examples/database/tsqlscript and [Firebird#Creating_objects_programmatically](<Firebird.md> "Firebird")  
[tdbf_l](</index.php?title=tdbf_l&action=edit&redlink=1> "tdbf l \(page does not exist\)") | dbase |   
[testcp](</index.php?title=testcp&action=edit&redlink=1> "testcp \(page does not exist\)") | memds |   
[testdbf](</index.php?title=testdbf&action=edit&redlink=1> "testdbf \(page does not exist\)") | dbase |   
[testds](</index.php?title=testds&action=edit&redlink=1> "testds \(page does not exist\)") | sqlite |   
[testfix](</index.php?title=testfix&action=edit&redlink=1> "testfix \(page does not exist\)") | sdf |   
[testld](</index.php?title=testld&action=edit&redlink=1> "testld \(page does not exist\)") | memds |   
[testopen](</index.php?title=testopen&action=edit&redlink=1> "testopen \(page does not exist\)") | memds |   
[testpop](</index.php?title=testpop&action=edit&redlink=1> "testpop \(page does not exist\)") | memds |   
[testsdf](</index.php?title=testsdf&action=edit&redlink=1> "testsdf \(page does not exist\)") | sdf |   
[testsqldb](</index.php?title=testsqldb&action=edit&redlink=1> "testsqldb \(page does not exist\)") | sqldb |   
[xmldatapacketreader](</index.php?title=xmldatapacketreader&action=edit&redlink=1> "xmldatapacketreader \(page does not exist\)") | base |   
[fpjsondataset](<fpjsondataset.md> "fpjsondataset") | json | Dataset for JSON data   
  
## See also

  * [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/fcl-db](https://web.archive.org/web/20230401112337/https://wiki.freepascal.org/fcl-db)_
