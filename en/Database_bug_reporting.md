# Database bug reporting

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
  
## Contents

  * 1 Sorts of bugs
  * 2 Reporting bugs
  * 3 Bug reporting template
    * 3.1 Lazarus
    * 3.2 Free Pascal
  * 4 See also



## Sorts of bugs

Bugs regarding databases can lie in three areas: 

  * The GUI controls in Lazarus (DBGrid etc)
  * The FPC database code (T*Connection, T*Query etc)
  * The underlying infrastructure: database drivers/dlls, operating system, network, database server



Lazarus builds on the FPC code, which builds on the infrastructure, so problems that show up in Lazarus GUI programs can be caused by bugs from any category. 

## Reporting bugs

When reporting bugs, it is very useful (and often essential) to have a compilable test program that shows the problem. See [Tips on writing bug reports](<Tips_on_writing_bug_reports.md> "Tips on writing bug reports") for reasons why. 

In case of database bugs, that means that you need to show how to create a database as well so others can repeat the test. 

To make this easier, you can write your bug report for common test/demonstration databases such as: 

  * Firebird's employee.fdb
  * Microsoft SQL Server's AdventureWorks



If you don't, please add the DDL (the schema definition) and sample data SQL INSERT statements (or code that performs this). 

## Bug reporting template

It's annoying to have to write long test programs when you just want to report a bug. However, it's also annoying and time-consuming for developers to have to write a program just to test if behaviour is incorrect. 

A possible solution: use a template program and adapt that to your bug. 

### Lazarus

For Lazarus, you can use one of the demo database applications in your examples directory (e.g. examples\database\sqldbtutorial3) and adapt that. Don't forget to specify how to create the required database (include the needed CREATE TABLE etc SQL) - or specify a well-known sample database as mentioned above. 

### Free Pascal

A template program can be found below. Please adjust where marked. Once you've tested the program in your own environment, **don't forget to remove confidential passwords** etc. 
    
    
    program dberror;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes, sysutils,
      sqldb,
      ibconnection {*REPLACE WITH RELEVANT CONNECTION LIBRARY*};
    
    {
    * ADD THE SQL NEEDED TO CREATE THE DATABASE HERE *
    also known as the DDL/schema for the database.
    For Firebird you can just say to use the Employee sample database.
    For MSSQL you can just say to use the Adventureworks sample database.
    }
    
    var
      Conn: TibConnection; {*REPLACE WITH RELEVANT CONNECTION *}
      Tran: TSQLTransaction;
      Q: TSQLQuery;
    begin
      Conn:=TIBConnection.create(nil); {*REPLACE WITH RELEVANT CONNECTION *}
      Tran:=TSQLTransaction.create(nil);
      Q:=TSQLQuery.Create(nil);
      try
        // *REMOVE IDENTIFYING INFO AND EDIT AS NEEDED*
        Conn.HostName:='127.0.0.1';
        Conn.UserName:='SYSDBA';
        Conn.Password:='masterkey';
        Conn.DatabaseName:='employee';
        // *END IDENTIFIYING INFO*
        Conn.Transaction:=Tran;
        Q.DataBase:=Conn;
        Conn.Open;
        // *EXAMPLE BUG TESTING CODE, REPLACE WITH YOUR OWN*
        Q.SQL.Text:='select now() as RightNow ';
        Q.Open;
        Q.Last; //force recordcount update
        writeln('recordcount: '+inttostr(q.RecordCount));
        Q.Close;
        // *END EXAMPLE BUG TESTING CODE*
        Conn.Close;
      finally
        Q.Free;
        Tran.Free;
        Conn.Free;
      end;
      writeln('Program complete. Press a key to continue.');
      readln;
    end.
    

Of course, if you don't want to use this template but want to directly write a test case for the [db test framework](<Databases.md> "Databases"), that's very welcome. 

## See also

  * [How do I create a bug report](<How_do_I_create_a_bug_report.md> "How do I create a bug report") General information on how to report a bug for Lazarus/FPC
  * [Tips on writing bug reports](<Tips_on_writing_bug_reports.md> "Tips on writing bug reports") Explains e.g. why test programs in bug reports are so important

---

_Source: [https://wiki.freepascal.org/Database_bug_reporting](https://web.archive.org/web/20250121225606/https://wiki.freepascal.org/Database_bug_reporting)_
