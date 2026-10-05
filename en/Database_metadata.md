# Database metadata

│ **English (en)** │  **[polski (pl)](</Database_metadata/pl> "Database metadata/pl")** │ 

Database metadata is data about a database. In this article, we also cover information about the database connection/server/client. 

## Contents

  * 1 SQLDB
    * 1.1 Server & client information
      * 1.1.1 Firebird/Interbase specifics
    * 1.2 Schema information
      * 1.2.1 Proposal for extension/uniformization
      * 1.2.2 Lazarus TSQLQuery metadata tool
    * 1.3 Data dictionary schema information
  * 2 Database specific information
  * 3 See also



## SQLDB

### Server & client information

In FPC development versions as of November 2012, you can use _GetConnectionInfo_. See [GetConnectionInfo documentation](<http://www.freepascal.org/docs-html/fcl/sqldb/tsqlconnection.getconnectioninfo.html>) Clarification: 

  * _citServerVersion_ : the server version is returned in the form of a string formatted as a number (e.g. 0205 representing 2.5)



if the T*Connection in question does not support (part of) this, the functions will return empty values. 

#### Firebird/Interbase specifics

  * _citServerVersion_ returns major and minor numbers only for Firebird (e.g. 0205 for version 2.5.1); it returns 3 numbers for Interbase
  * _citClientVersion_ is not supported and probably will never be supported for Firebird on Linux/*Nix due to missing functionality



In addition to the functions above, as of December 2012, the Firebird connector provides the _GetODS_ function that shows the ODS major version of the database you are connected to. This ODS version determines what features are available for use - as you can e.g. use a database created with Firebird 2.1 with a Firebird 2.5 server, but the features will be limited to 2.1 functionality. 

### Schema information

Also known as a database's DDL (Data Definition Language), a schema is the representation of the tables, views, procedures and other objects that make up a database. 

See [Retrieving Schema Information documentation](<http://www.freepascal.org/docs-html/fcl/sqldb/retrievingschemainformation.html>)

_GetSchemaInfoSQL_ is not implemented equally for every connector; the Firebird connector currently has most of the functionality implemented; the others (e.g. MS SQL, Sybase, Oracle) have less functionality. Patches are welcome. 

#### Proposal for extension/uniformization

**Note: there were discussions/a proposal on how to harmonize and extend metadata retrieval for SQLDB on the FPC mailing list. Please update the below with all results of that discussion:**

Suggest converging to SQL standard INFORMATION_SCHEMA column naming in existing GetSchemaInfoSQL. So for: 

  * stTables use INFORMATION_SCHEMA.TABLES with column naming:


    
    
       TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME, TABLE_TYPE ('BASE TABLE', 'VIEW', 'GLOBAL TEMPORARY', 'LOCAL TEMPORARY'), ...
    

(for databases which do not implement INFORMATION_SCHEMA views leave queries to system tables but use above mentioned column naming) 

  * stColumns use INFORMATION_SCHEMA.COLUMNS with column naming:


    
    
       TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME, ORDINAL_POSITION, COLUMN_DEFAULT, IS_NULLABLE, DATA_TYPE, CHARACTER_MAXIMUM_LENGTH, CHARACTER_OCTET_LENGTH, NUMERIC_PRECISION, NUMERIC_SCALE, ..., CHARACTER_SET_NAME, COLLATION_NAME, DOMAIN_NAME, ..., IS_IDENTITY, IS_GENERATED
    

  * stProcedures use INFORMATION_SCHEMA.ROUTINES with column naming:


    
    
       SPECIFIC_CATALOG, SPECIFIC_SCHEMA, SPECIFIC_NAME, ROUTINE_CATALOG, ROUTINE_SCHEMA, ROUTINE_NAME, ROUTINE_TYPE ('PROCEDURE', 'FUNCTION',...), ...
    

Note: On other side Delphi uses slightly different column naming, described in [The_Structure_of_Metadata_Datasets](<http://docwiki.embarcadero.com/RADStudio/XE3/en/The_Structure_of_Metadata_Datasets>) But tests with Delphi XE MySQL and Interbase DBX drivers shows results which do not match this specification (f.e. for stTables these columns are returned: CatalogName, SchemaName, TableName, TableType (without "_") and without RECNO column) and for **ADO OpenSchema** method these columns are returned: 
    
    
    siTables (adSchemaTables):	TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME, TABLE_TYPE(widestring)
    siColumns (adSchemaColumns):	TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME, ORDINAL_POSITION, COLUMN_DEFAULT, IS_NULLABLE, DATA_TYPE, CHARACTER_MAXIMUM_LENGTH, CHARACTER_OCTET_LENGTH, NUMERIC_PRECISION, NUMERIC_SCALE, ..., CHARACTER_SET_NAME, COLLATION_NAME, DOMAIN_NAME, ..., IS_COMPUTED
    siIndexes (adSchemaIndexes):	TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME, INDEX_CATALOG, INDEX_SCHEMA, INDEX_NAME, PRIMARY_KEY(boolean), UNIQUE(boolean), CLUSTERED(boolean), TYPE(word), FILL_FACTOR, ..., ORDINAL_POSITION, COLUMN_NAME, ... (one row for every column in index)
    siProcedures (adSchemaProcedures):	PROCEDURE_CATALOG, PROCEDURE_SCHEMA, PROCEDURE_NAME, PROCEDURE_TYPE(Smallint), ...
    siSchemata (adSchemaSchemata):	CATALOG_NAME, SCHEMA_NAME, SCHEMA_OWNER, ...
    

#### Lazarus TSQLQuery metadata tool

_GetSchemaInfoSQL_ is used in Lazarus: in design mode, if you use the Object Inspector to edit the SQL property of a _TSQLQuery_ where the database connection is connected, you can not only edit the text of the SQL query in the SQL Code tab, but also retrieve information in the Metadata tab by filling in some details and pressing on the Play/Go key: 

[![MetaDataEditor.png](https://wiki.freepascal.org/images/5/5a/MetaDataEditor.png)](</File:MetaDataEditor.png>)

### Data dictionary schema information

There is a separate FPC data dictionary package that has support for schema information. This package is used in the [lazdatadesktop](<lazdatadesktop.md> "lazdatadesktop") project supplied with Lazarus. 

## Database specific information

Many databases have either 

  * system tables/views (which you can select from) or
  * stored procedures



that show information about database objects as well as server version etc. 

Examples are the @@VERSION variables in MS SQL Server and Sybase servers. 

If your database connection library does not support other means of getting this information, you can use this (and submit a patch/improvement request on the relevant bug tracker for incorporation into SQLDB/Zeos etc) 

## See also

  * [Working With TSQLQuery](<Working_With_TSQLQuery.md> "Working With TSQLQuery") More information about queries

---

_Source: [https://wiki.freepascal.org/Database_metadata](https://web.archive.org/web/20240516115419/https://wiki.freepascal.org/Database_metadata)_
