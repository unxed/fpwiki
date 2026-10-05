# Firebird

│ **English (en)** │  **[français (fr)](</Firebird/fr> "Firebird/fr")** │  **[русский (ru)](<../ru/Firebird.md> "Firebird/ru")** │    
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


    [Advantage](<Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<mssqlconn.md> "mssqlconn") \- [Postgres](<postgres.md> "postgres") \- Interbase \- Firebird \- [Oracle](<Oracle.md> "Oracle") \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- [dBASE](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<MS_Access.md> "MS Access") \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
This is a guide about using **Firebird** database in Lazarus/FPC, using Firebird with SQLDB (the FPC/Lazarus built-in database library). Other access methods are described in [Other Firebird libraries](<Other_Firebird_libraries.md> "Other Firebird libraries"). 

## Contents

  * 1 Overview
  * 2 Documentation
  * 3 Client/server and embedded installation
    * 3.1 Windows
    * 3.2 Unix/Linux/macOS
  * 4 Connection examples
  * 5 Troubleshooting client/server access issues
  * 6 Monitoring Events
  * 7 Creating objects programmatically
  * 8 Database Administration
  * 9 Common problems and solutions
    * 9.1 Attempted update of read-only column / COMPUTED BY fields
    * 9.2 Bigint: lost precision
    * 9.3 Boolean data types
    * 9.4 INSERT INTO...RETURNING problems/Cursor is not open =
    * 9.5 Locate does not seem to work
  * 10 Advanced transactions
    * 10.1 Transaction isolation levels
    * 10.2 Access mode
    * 10.3 Lock resolution
    * 10.4 Table reservation
    * 10.5 Record versions
    * 10.6 Various options
    * 10.7 Firebird and ISO transactions
    * 10.8 Common combinations
      * 10.8.1 Batch/bulk insert
      * 10.8.2 Read only transaction
  * 11 Links and more information
    * 11.1 Lazarus Firebird samples
    * 11.2 Tools
    * 11.3 Firebird



## Overview

Firebird is an open source, free database server that has been in use and developed for decades (it developed out of the Interbase 6 database that was open sourced by Borland). It includes rich support for SQL statements (e.g. INSERT...RETURNING), stored procedures, triggers, etc. You can write compiled UDFs (User-Defined Functions) libraries for the server in FreePascal if you want to extend the already extensive function list of Firebird. 

The database requires very little manual DBA work once it is set up, making it ideal for small business use or embedded use. It can grow to terabyte scale given proper tuning, although PostgreSQL may be a better choice for such large environments. 

Firebird offers both embedded (file-based) and client-server database - usable without having to change a single line of code in FPC/Lazarus. If used as an embedded database, it offers a richer SQL support than [SQLite](<SQLite.md> "SQLite") as well as seamless migration to a client-server database, although SQLite is a quite capable embedded database itself. 

The latest stable version, Firebird 2.5, runs on Windows (32- and 64-bit), various Linux versions (32- and 64-bit), Solaris (Sparc and Intel), HP-UX (PA-Risc) and OSX. 

It is being ported to Android at the moment; it is not available on Windows CE/Windows Mobile. 

Support for Firebird in FPC's SQLDB is quite good, comparable to the level of [PostgreSQL](<postgres.md> "postgres") support. 

## Documentation

Official documentation is included in FPC 2.6.2+: [SQLDB documentation for IBConnection](<http://www.freepascal.org/docs-html/fcl/ibconnection/index.html>)

## Client/server and embedded installation

Firebird can run in client/server and embedded mode. 

_Client/Server_ means that you have a physical Firebird server somewhere: either on your local machine or another machine reachable over your network. Connections to the server go through TCP/IP; when specifying the connection, the hostname contains a name or IP address. The Firebird DLL you need for this is _fbclient.dll_ (_along with its support files_). 

_Embedded Firebird_ means that your application loads the Firebird DLLs to access a Firebird database on the local machine. When specifying the connection, the hostname is always empty. The Firebird DLL you need for this is _fbembed.dll_ (_along with its support files_). See [the wiki page on Firebird embedded](<Firebird_embedded.md> "Firebird embedded") for more details. 

Note that _fbembed.dll_ can be used both for client/server and embedded use, so installing only this dll may be a smart thing to do. 

### Windows

Win64: please see warning [here](<Windows_Programming_Tips.md> "Windows Programming Tips") on not using certain FPC/Lazarus Win64 versions. 

On Windows: (this applies to all SQLDB database drivers) you must have **fbclient.dll** (or **fbembed.dll**) and its support dlls installed in: 

  * the project directory **and** the executable output directory/the application directory (e.g. lib/something under your project directory)
  * or a directory in your PATH (not your system directory)
  * If you want to use the system directory, please use the official installer and tick "copy fbclient to system directory"



As with all (database) DLLs, the bitness of the DLL must match your application: use a 32 bit library for a 32 bit compiled program and a 64 bit library for a 64 bit program. 

### Unix/Linux/macOS

On FreeBSD/Linux/macOS, the Firebird client library should be installed (e.g. by your package manager; install the regular package and the -dev package), or they should be placed in the library search path. 

FPC searches for the most common library names (e.g. libfbclient.so.2.5, libgds.so, and libfbembed.so.2.5; please check ibase60.inc if your version is different). If you want to, you can explicitly specify the library name. There are 2 ways for this: 

  * use the [TSQLDBLibraryLoader](<TSQLDBLibraryLoader.md> "TSQLDBLibraryLoader") component from sqldblib (FPC 2.7.1). Works for all SQLDB connector components.
  * call 
        
        function InitialiseIBase60(Const LibraryName : AnsiString) : integer;
        

with the correct library name (you may need to use unit ibase60dyn for this).



## Connection examples

Example for client/server: 
    
    
    Hostname: 192.168.1.1
    * The database is on the server with IP address 192.168.1.1. 
    DatabaseName: /interdata/example.fdb  
    * The name of the database file is "example.fdb" in the /interdata directory of the server (the machine with IP address 192.168.1.1).
    Username: SYSDBA
    Password: masterkey
    

Another example for client/server: 
    
    
    Hostname: dbhost
    * The database is on the server with the host name dbhost
    DatabaseName: F:\Program Files\firebird\examples\employee.fdb  
    * The name of the database file is "employee.fdb" in the Program Files\firebird\examples directory on the F: drive of dbhost.
    Username: SYSDBA
    Password: masterkey
    

An embedded example: 
    
    
    Hostname: <empty string>
    * Leaving the hostname empty selects embedded use.
    DatabaseName: test.fdb
    * The database file is "test.fdb" in the directory where the application runs (make sure fbembed.dll is in the application executable directory)
    Username: SYSDBA
    * On embedded, you do have to specify a username...
    Password: <empty string>
    * ... but it doesn't matter what password you give.
    

## Troubleshooting client/server access issues

Make sure you started the Interbase/Firebird server on the server IP/hostname you specified. You can test connectivity by telnetting to the machine. Firebird usually listens on port 3050: 
    
    
    telnet 192.168.1.1 3050
    

You should see something, maybe just a blank screen, but you can type something. This means you can send data to the Firebird database. 

For further information, please see the Firebird documentation. 

## Monitoring Events

FPC/Lazarus comes with a component to monitor events coming from Firebird databases; see [TFBEventMonitor](<TFBEventMonitor.md> "TFBEventMonitor"). 

## Creating objects programmatically

While you can use tools such as Flamerobin to create tables etc, you can also create these programmatically/dynamically, which could be handy when you want your programs to update existing database schemas to a new schema. 

You can use e.g. TSQLQuery.ExecSQL to perform this task: 
    
    
    Query.ExecSQL('CREATE TABLE TEST(ID INTEGER NOT NULL, TESTNAME VARCHAR(800))');
    // You need to commit the transaction after DDL before any DML - SELECT, INSERT etc statements.
    // Otherwise the SQL won't see the created objects
    

To do this, use the TSQLScript object - see [TSQLScript](<TSQLScript.md> "TSQLScript"). 

## Database Administration

FPC/Lazarus has a component for database adminstration; see [TFBAdmin](<TFBAdmin.md> "TFBAdmin")

## Common problems and solutions

Sometimes using Firebird in Lazarus seems to be tricky. Please find solutions below. 

### Attempted update of read-only column / COMPUTED BY fields

If you have COMPUTED BY fields (server-side calculated fields) in your Firebird table, SQLDB will not pick up that these are read only fields (for performance reasons). 

In this case, auto-generated INSERTSQL,UPDATESQL statements can lead to error messages like "attempted update of read-only column". The solution is to manually specify that the field in question may not be updated after setting the TSQLQuery's SQL property, something like: 
    
    
    // Disable updating this field or searching for changed values as user cannot change it
    sqlquery1.fieldbyname('full_name').ProviderFlags:=[];
    

### Bigint: lost precision

If you use the bigint datatype (64 bit signed integer) in Firebird, please use .AsLargeInt instead of .AsInteger for parameters: 
    
    
    // Assuming ID is bigint here
    sqlquery1.sql.text := 'insert into ADDRESS (ID) values (:ID)';
    // Use this:
    sqlquery1.params.parambyname('ID').aslargeint := <some qword or 64 bit integer variable>;
    // Do not use this:
    //sqlquery1.params.parambyname('ID').asinteger := <some qword or 64 bit integer variable>;
    ...
    

... otherwise you might get errors like duplicate PK (primary key) - if using the bigint as a primary key. 

### Boolean data types

Firebird versions below 3.0 do not support boolean data types. 

At least on FPC trunk (2.7.1) this datatype can be emulated: 

Use a DOMAIN that uses a SMALLINT type (other integer types may work as well - please test and adjust text): 
    
    
    CREATE DOMAIN "BOOLEAN"
     AS SMALLINT
     CHECK (VALUE IS NULL OR VALUE IN (-1,0,1))
     /* -1 used for compatibility with FPC SQLDB; 1 is used by many other data access layers */
    ;
    

  


Let your field/column use this domain type e.g. 
    
    
    CREATE TABLE MYTABLE
    (
    ...
      MYBOOLEANCOLUMN "BOOLEAN",
    );
    

  * Now you can use .AsBoolean for assigning field values etc



**To do: verify this works with Lazarus grids etc as well.**

### INSERT INTO...RETURNING problems/Cursor is not open =

If you try to select SQL (e.g. Query.Open) with SQL like this: 
    
    
    INSERT INTO PEOPLE (NICKNAME) VALUES ('Superman') RETURNING ID
    

and get something like this error: 
    
    
    Database error:  : Fetch :
     -Dynamic SQL Error
     -SQL error code = -504
     -Invalid cursor reference
     -Cursor is not open(error code: 335544569)
    

while running FPC 2.6.0 (which is supplied with Lazarus 1.0) or lower, then you probably ran into an FPC SQLDB parser bug. 

SQLDB thinks the statement you're running is a normal INSERT statement, which doesn't return data. Obviously, it should return data. Newer FPC code has fixes for this. 

If you're using generators/sequences for your primary keys (like many do), a workaround is to first get the next sequence number: 
    
    
    SELECT NEXT VALUE FOR GEN_PEOPLEID FROM RDB$DATABASE /* If your generator name is GEN_PEOPLEID */
    

then use that to do a regular INSERT. see [FAQ entry](<http://www.firebirdfaq.org/faq111/%7CFirebird>)

### Locate does not seem to work

Source: [Issue ##21988](<https://bugs.freepascal.org/view.php?id=#21988>)

When running locate on UTF8 (or presumably other multibyte character sets) CHAR fields, locate may not find your record. 

The problem is related mostly to UTF8 charset used and how Firebird reports column length. In case of UTF8 Firebird reports the maximum column length (in bytes) as 4*"character length". So if you have a column defined as char(8), Firebird reports 4*8=32. 

Values are right-padded to this length 32. When locating say '57200001' there is no match because the field actually stores '57200001 ........................' (with trailing spaces represented by dots here). 

Workaround: rewrite your select query: 
    
    
    SELECT substring(THEFIELD from 1 for 8) AS THEFIELD ...
    
    
    
    or
    
    
    
    SELECT cast(THEFIELD as varchar(8)) as THEFIELD ...
    

or use VARCHAR fields. 

Note: this problem may occur for other databases as well depending on their reporting of field length. 

## Advanced transactions

Sources for this information/further reading: 

  * [Understanding Firebird Transactions](<http://www.ibphoenix.com/resources/documents/how_to/doc_400>) very detailed article
  * [Transactions in Firebird](<https://ib-aid.com/en/transactions-in-firebird-acid-isolation-levels-deadlocks-and-update-conflicts-resolution/>) another very detailed article
  * Interbase 6 API Guide (valid for Firebird+Interbase), page 63 and further
  * [Overview of transaction settings in Interbase](<http://conferences.embarcadero.com/article/32280>)
  * [Detailed explanation of parameters in Firebird](<http://tech.groups.yahoo.com/group/firebird-support/message/58653>)
  * [Overview of settings in Firebird](<http://fhasovic.blogspot.com/2005/02/transaction-isolation-levels-in.html>)
  * [Transaction isolation levels in FIBPlus](<http://www.devrace.com/en/fibplus/articles/3292.php>)
  * README.set_transaction.txt in Firebird 2.5 documentation folder.



### Transaction isolation levels

If you want to, you can change the transaction isolation levels by adding a line in the transaction's Parameters property: 

  * `isc_tpb_read_committed`: you see all changes committed by other transactions
  * `isc_tpb_concurrency`: also called Snapshot: you see database as it was when the transaction started. Has more overhead than isc_tpb_read_committed. Better than ANSI Serializable because it has no phantom reads.
  * `isc_tpb_consistency`: also called Table Stability: stable, serializable view of data, but locks tables. Unlikely you will need this



Example: 
    
    
    SQLTransaction1.Params.text:='isc_tpb_read_committed';
    

You can also add additional parameters that have an effect on the transaction (taken from the ibconnection.pp source file and [[1]](<http://conferences.embarcadero.com/article/32280>)): 

### Access mode

This allow reads only or read/write 

  * `isc_tpb_read`: read permission
  * `isc_tpb_write`: read+write permission



### Lock resolution

  * `isc_tpb_nowait`: if another transaction is editing the record then don't wait
  * `isc_tpb_wait`: if another transaction is editing the record then wait for it to finish. Can mitigate "live locks" in heavy contention ([[2]](<http://tech.groups.yahoo.com/group/firebird-support/message/58653>)). See below for timeout value.



### Table reservation

Deals with locking entire tables. 

  * `isc_tpb_shared`: first specify this, then either lock_read or lock_write for one or more tables. Shared read or write mode for tables.
  * `isc_tpb_protected`: first specify this, then either lock_read or lock_write for one or more tables. Lock on tables; can allow deadlock-free operation at the cost of delayed transactions
  * `isc_tpb_lock_read`: Set a read lock. Specify which table to lock, e.g. isc_tpb_lock_read=CUSTOMERS
  * `isc_tpb_lock_write`: Set a read/write lock. Specify which table to lock, e.g. isc_tpb_lock_read=CUSTOMERS



Combinations: 

  * Shared, lock_write: write transactions with concurrency or read committed isolation



can update the table. All transactions can read the table 

  * Shared, lock_read: any transaction can read or update
  * Protected, lock_write: Other transactions cannot update the table. Only concurrency and



read committed transactions can read the table 

  * Protected, lock_read: Other transactions cannot update the table. Any transaction can



read the table 

### Record versions

This setting is apparently only relevant for isc_tpb_read_committed isolation mode for records being modified by other transactions: 

  * `isc_tpb_no_rec_version`: only newest record version is read. Can be useful for batch/bulk insert operations together with isc_tpb_read_committed)
  * `isc_tpb_rec_version`: the latest committed version is read, even when the other transaction has other uncommitted changes **note: verify this**. More overhead than isc_tpb_no_rec_version



### Various options

For completeness, some more options appearing in the Firebird/Interbase FPC code. You will likely only ever use isc_tpb_no_auto_undo to speed up batch inserts/edits. 

  * isc_tpb_exclusive (apparently translates to protected in Firebird, see [[3]](<http://tech.groups.yahoo.com/group/firebird-support/message/58653>))
  * isc_tpb_verb_time (Related to deferred constraints, which could execute at verb time or commit time. Firebird: not implemented, always use verb time)
  * isc_tpb_commit_time (Related to deferred constraints, which could execute at verb time or commit time. Firebird: not implemented, always use verb time)
  * isc_tpb_ignore_limbo (ignores the records created by transactions in limbo. Limbo transactions are failing two-phase commits in multi-database transactions. Unlikely that you will need this feature)
  * isc_tpb_autocommit (autocommit this transaction: every statement is a separate transaction. Probably specifically for JayBird JDBC driver.)
  * isc_tpb_restart_requests (apparently looks for requests in the connection which had been active in another transaction, unwinds them, and restarts them under the new transaction.)
  * isc_tpb_no_auto_undo (disable transaction-level undo log, handy for getting max throughput when performing a batch update. Has no effect when only reading data.)
  * isc_tpb_lock_timeout (specify number of seconds to wait for lock release, if you use isc_tpb_wait. If this value is reached without lock release, an error is reported.)



### Firebird and ISO transactions

Firebird transaction do not map 1 to 1 to ISO/ANSI transaction levels. An approximation is: 

  * ISO Read Committed=READ COMMITTED+RECORD_VERSION
  * ISO Read Committed=READ COMMITTED+NO RECORD_VERSION
  * ISO Repeatable Read=SNAPSHOT (also known as CONCURRENCY)
  * ISO Serializable=SNAPSHOT TABLE STABILITY (also known as CONSISTENCY)



### Common combinations

Default is (probably, will have to check) read committed. 

#### Batch/bulk insert

isc_tpb_read_committed and isc_tpb_no_rec_version could be a good combination: it allows other transactions to function while the batch is going on. 

#### Read only transaction

If you want to only have read access to the database, you can do so by setting these transaction parameters: 

  * `isc_tpb_read`
  * `isc_tpb_read_committed`
  * `isc_tpb_rec_version`
  * `nowait`



This combination will not block garbage collection, which is a good thing. Source: [[4]](<http://tech.groups.yahoo.com/group/firebird-support/message/118748>)

Note: the [Firebird FAQ](<http://www.firebirdfaq.org/faq164/>) indicates you will need write access to the database file even if you only read from it, unless you set the database read-only flag (e.g. using gfix). 

## Links and more information

The list below shows links to more information on Firebird and related tools. 

  * [Using Firebird embedded with FPC/Lazarus](<Firebird_embedded.md> "Firebird embedded")



### Lazarus Firebird samples

  * Sample Lazarus/Firebird application source code [[5]](<http://lazarus.freepascal.org/index.php/topic,13940.msg73617.html#msg73617>)
  * Firebird/SQLDB tutorials: 
    * [SQLdb Tutorial0](<SQLdb_Tutorial0.md> "SQLdb Tutorial0")
    * [SQLdb Tutorial1](<SQLdb_Tutorial1.md> "SQLdb Tutorial1")
    * [SQLdb Tutorial2](<SQLdb_Tutorial2.md> "SQLdb Tutorial2")
    * [SQLdb Tutorial3](<SQLdb_Tutorial3.md> "SQLdb Tutorial3")
    * [SQLdb Tutorial4](<SQLdb_Tutorial4.md> "SQLdb Tutorial4")


  * PP4S tutorials; cover Firebird installation on Windows as well: 
    * [Planning a Database](<http://www.pp4s.co.uk/main/tu-db-plan.html>)
    * [Installing Firebird](<http://www.pp4s.co.uk/main/tu-db-installingfirebird01.html>)
    * [Creating a Firebird database](<http://www.pp4s.co.uk/main/tu-db-firebird-create.html>)
    * [Editing a Firebird database](<http://www.pp4s.co.uk/main/tu-db-firebird-demo1.html>)
    * [Searching a Firebird database](<http://www.pp4s.co.uk/main/tu-db-firebird-demo2.html>)
    * [Creating and printing a report](<http://www.pp4s.co.uk/main/tu-db-firebird-demo3.html>)
    * [Creating and using stored procedures](<http://www.pp4s.co.uk/main/tu-db-firebird-demo4.html>)



### Tools

  * FlameRobin [Flamerobin site](<http://www.flamerobin.org/>) Open source GUI tool to manage Firebird, available for Linux, Windows and Mac OSX. Highly recommended.
  * Turbobird [Turbobird site](<https://github.com/motaz/turbobird>) Open source GUI tool to manage Firebird. Written with Lazarus using TIBConnection
  * ibconsole : Tool to manage Firebird an Interbase Databases with a GUI, available for Windows and Linux
  * Lazarus Data Desktop - included in the Lazarus repository (found in the 'tools/lazdatadesktop/' directory)
  * [LazSQLX](<http://lazsqlx.wordpress.com/>) Multi-database open source database management tool. Written with Lazarus using both SQLDB and Zeos components. Includes support for Firebird.
  * tiSQLEditor - included in the "Support Apps" directory of the [tiOPF](<tiOPF.md> "tiOPF") repository. It is a tool used to write SQL with some code completion support, can run scripts, execute upgrade scripts for applications from one build to a later build, has various handy copy and paste functions for Object Pascal applications, has many features that are useful to tiOPF (creates Object Pascal code templates from query results, for tiOPF's visitor classes), export query results to CSV etc.



### Firebird

  * The Firebird RDBMS [Firebird site](<http://firebirdsql.org/>). This site also contains a lot of documentation on Firebird.
  * Firebird FAQ [[6]](<http://firebirdfaq.org/>). Handy site that shows e.g. differences with other RDBMS.
  * New site that shows how to use the latest tools and LibreOffice, FPC to be added soon [Firebird](<http://www.controlpascal.com/firebird.htm>)

---

_Source: [https://wiki.freepascal.org/Firebird](https://web.archive.org/web/20250317192736/https://wiki.freepascal.org/Firebird)_
