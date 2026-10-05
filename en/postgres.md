# postgres

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


    [Advantage](<Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<mssqlconn.md> "mssqlconn") \- Postgres \- [Interbase](<Firebird.md> "Firebird") \- [Firebird](<Firebird.md> "Firebird") \- [Oracle](<Oracle.md> "Oracle") \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- [dBASE](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<MS_Access.md> "MS Access") \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
## Contents

  * 1 Overview
  * 2 Direct access to PostgreSQL
  * 3 Zeos
  * 4 SQLDB
    * 4.1 Example
    * 4.2 Event monitoring
    * 4.3 Installation and errors
      * 4.3.1 Windows 64 bit driver
      * 4.3.2 Error: "Can not load PostgreSQL client library "libpq.dll""
      * 4.3.3 Problems clearing parameters
  * 5 How To
    * 5.1 Use keyword RETURNING for values
      * 5.1.1 Get Last Inserted ID
      * 5.1.2 Get multiple fields and expressions
  * 6 PostgreSQL package: the low level units
    * 6.1 Requirements
    * 6.2 Installation
  * 7 See also



## Overview

You can use Free Pascal/Lazarus to access a PostgreSQL database server. If you are looking for information on the _postgres_ package in FPC, please see [postgres#PostgreSQL_package:_the_low_level_units](<postgres.md> "postgres") below. 

Advantages of PostgreSQL: 

  * It is very widely used and available
  * Very stable and has a complete feature set
  * Liberal license (no costs) in comparison with MySQL



Disadvantage of PostgreSQL: 

  * Some hosters may not offer PostgreSQL)
  * No embedded version



Win64: please see warning [here](<Windows_Programming_Tips.md> "Windows Programming Tips") on not using certain FPC/Lazarus Win64 versions. 

## Direct access to PostgreSQL

You can connect Lazarus with PostgreSQL by using PostgreSQL Data Access Components (PgDAC). It is a library of components that provides native connectivity to PostgreSQL from Lazarus (and Free Pascal) on Windows, Mac OS X, iOS, Android, Linux, and FreeBSD for both 32-bit and 64-bit platforms. PgDAC is designed to help programmers develop really lightweight, faster and cleaner PostgreSQL database applications without deploying any additional libraries. 

You can download this [Lazarus component](<https://www.devart.com/pgdac/download.html>) for free. 

## Zeos

Zeos supports PostgreSQL; please see [ZeosDBO](<ZeosDBO.md> "ZeosDBO")

## SQLDB

FPC/Lazarus supports PostgreSQL out of the box with a PostgreSQL connection component/class. If you are using FPC only or want to manually add PostgreSQL support, add _pqconnection_ to your _uses_ clause. Otherwise, Lazarus provides a component: 

[![sqldbcomponents.png](https://wiki.freepascal.org/images/8/82/sqldbcomponents.png)](</File:sqldbcomponents.png>)

**Note:** The libpq C client contains some memory leaks (at least up till version 9.3 of Postgres) when a library is repeatedly loaded/unloaded. SQLDB loads the library when the first connection is made, and unloads it when the last connection closes. This means that whenever the last connection is closed, a small memory leak is created. To prevent this from happening (and speed up the application), you can load the library once at the start of the process with the InitialisePostgres3 call. 

The CharSet property is used for client encoding. 

The TPQConnection component does not directly support a Port property, but one can pass the port into the component via the Params parameter: 
    
    
    PQConnection.Params.Add('port=' + VariableContainingPort);
    

Also other PostgreSQL specific connection parameters can be specified using the Params property: 
    
    
    PQConnection.Params.Add('application_name=''yourappname''')
    

For all supported connection parameters see: [Connection Parameter Key Words](<https://www.postgresql.org/docs/current/static/libpq-connect.html#LIBPQ-PARAMKEYWORDS>)

### Example

See [SQLdb_Tutorial1](<SQLdb_Tutorial1.md> "SQLdb Tutorial1") for a tutorial on creating a GUI database-enabled program that is written for PostgreSQL/SQLDB, as well as SQLite/SQLDB, Firebird/SQLDB, basically any RDBMS SQLDB supports). 

### Event monitoring

If you have FPC2.6.2+ and a recent version of Lazarus, you can use the **TPQTEventMonitor** component to monitor events coming from PostgreSQL. 

It is a thin wrapper around FPC **PQEventMonitor** ; please see the FPC pqeventstest.pp example programs for details. 

### Installation and errors

As with all sqldb units, you need to add your driver libraries (**all** required PostgreSQL .dll/.manifest files) 

  * to a directory in the (library search) path (e.g. c:\windows\system32 for Windows)
  * or (Windows) to the program output directory (e.g. lib/something/ in your project directory, and the project directory



#### Windows 64 bit driver

If you are developing 64 bit applications, you must use a 64 bit DLL. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** FPC 2.6.0 (therefore Lazarus 1.0.4 or lower) does not yet support PostgreSQL on Windows 64 bit (unless you patch and recompile it).

A Windows 64 driver is fairly hard to find but can be downloaded here: [[1]](<http://www.enterprisedb.com/products-services-training/pgdownload#windows>). The driver library can be installed in c:\windows\system32; 32 bit driver libraries can be installed in the confusingly named c:\windows\syswow64 

#### Error: "Can not load PostgreSQL client library "libpq.dll""

The program cannot find your PostgreSQL driver files. 

See above on instructions where to install the libraries. 

A good example that demonstrates how to include drive DLL files when connecting Lazarus with PostgreSQL under Windows is [easyDB](<http://paginas.fe.up.pt/~asousa/wiki/doku.php?id=proj:lazarus_projects#easy_db>). 

On Linux/Unix/OSX: make sure the PostgreSQL libraries are in your library search path, e.g.: 

  * On Linux add the path to the libpq.so file to the libraries section in your /etc/fpc.cfg file. For example : -Fl/usr/local/pgsql/lib
  * It may be necessary to create a symbolic link from a specific library version to a general library name: 
        
        ln -s /usr/lib/pqsql.so.5 /usr/lib/pqsql.so
        

. Alternatively, install the postgresql client -dev package using your distribution's package manager



#### Problems clearing parameters

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Verify this: is .paramtype really necessary?

At least in FPC <= 2.6.2: if you .Clear a parameter (i.e. set it to NULL), PostgreSQL may have difficulty recognizing the parameter type. 

In that case, explicitly specify the type, e.g.: 
    
    
    FWriteQuery.Params.ParamByName('LONGITUDE').ParamType:=ptInput; //required for postgresql
    FWriteQuery.Params.ParamByName('LONGITUDE').Clear
    

## How To

### Use keyword RETURNING for values

#### Get Last Inserted ID

With PostGres there is no need to run a second query to get the last inserted ID. 

Use INSERT RETURNING and read the value: 
    
    
    var 
      ID: Integer
    ..
    Query.SQL.Text:= 'INSERT INTO myschema.films(film_name)' +
                     'VALUES(:film_name) RETURNING film_id;';
    Query.Open;
    ID:= Query.FieldByName('film_id').AsInteger;
    

#### Get multiple fields and expressions

RETURNING in PostgreSQL is more flexible than most database engines. It works with INSERT, UPDATE, and DELETE statements and can be any list of fields, constants, or expressions that would be found in a SELECT list. 
    
    
    INSERT INTO films (film_name) VALUES ('val') RETURNING film_id;  -- Returns id's for newly created rows.
    INSERT INTO films (film_name) VALUES ('val') RETURNING film_id, kind;  -- Returns id and kind fields in newly created rows.
    INSERT INTO films (film_name) VALUES ('val') RETURNING *; -- Returns all fields in newly created rows.
         
    UPDATE films SET kind = 'Dramatic' WHERE kind = 'Drama' RETURNING film_id; -- Returns id's of updated rows.
    UPDATE films SET kind = 'Dramatic' WHERE kind = 'Drama' RETURNING film_id, film_name; -- Returns id and film names of updated rows.
    UPDATE films SET kind = 'Dramatic' WHERE kind = 'Drama' RETURNING *; -- Returns all fields of updated rows.
         
    DELETE FROM films RETURNING film_id; --Returns id's of deleted rows.
    DELETE FROM films RETURNING film_id, film_name; --Returns id's and film names of deleted rows.
    DELETE FROM films RETURNING *;  -- Returns all fields of deleted rows.
    

RETURNING using fields, constants, and expressions: 
    
    
    INSERT INTO films (film_name) VALUES ('val') RETURNING id, 'a' AS a, id*2 AS doubled_id, CASE WHEN id > 100 THEN 'a' ELSE 'b' END AS foo;
    

## PostgreSQL package: the low level units

As with all databases, the SQLDB code depends on a lower level PostgreSQL specific unit that wraps around the PostgreSQL driver library (.so/.dll/.dylib). Normally, you would use the higher-level SQLDB code as it allows you to code more quickly, easily switch databases etc. 

Using this is very easy, all you need to do is compile some units, and use these units in your program. You need to specify the place of the PostgreSQL client Library (libpq) when compiling, and that is it. 

The main unit is called **postgres** , normally this is the only unit you must include in your uses clause. 

### Requirements

You need at least version 0.99.5 of Free Pascal (basically any version of FPC except extremely old ones). The headers are translated from PostgreSQL version 6.3.1. 

### Installation

The postgres unit comes with the Free Pascal packages, and is distributed together with the compiler. This contains a directory postgres with the units, a test program and a makefile. cd to the directory and edit the Makefile to set the variables for your system. You must provide only 1 thing: 

  1. The directory where the libpq library resides, usually /usr/local/pgsql/lib



Typing 
    
    
    make
    

Should compile the units and the program. If compilation was succesfull, you can install with 
    
    
    make install
    

(Remember to set the directory where the units should be installed.) 

You can then test the program by running 
    
    
    make test
    

This will: 

  * Run the test program testpg. It is a straightforward pascal translation of the example program in the PostGreSQL programmers' guide.
  * Run a script to create a table in a database, and fill it with some data. (the psql program should be in your PATH for this) . By default, the used database is testdb.
  * Run the testprogram testemail
  * Run a shell script again to remove the created table.



You will see a lot of messages on your screen, giving you feedback and results. If something went wrong, make will inform you of this. 

Go back to [Packages List](<Package_List.md> "Package List")

## See also

  * [TPSQL](<TPSQL.md> "TPSQL") \- A different set of PostgreSQL dataset/database components
  * [Lazarus DB Faq](<Lazarus_DB_Faq.md> "Lazarus DB Faq") \- More about database programming
  * [Lazarus Database Tutorial](<Lazarus_Database_Tutorial.md> "Lazarus Database Tutorial") \- Getting Lazarus working with various databases

---

_Source: [https://wiki.freepascal.org/postgres](https://web.archive.org/web/20240516115505/https://wiki.freepascal.org/postgres)_
