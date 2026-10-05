# Oracle

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


    [Advantage](<Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<mssqlconn.md> "mssqlconn") \- [Postgres](<postgres.md> "postgres") \- [Interbase](<Firebird.md> "Firebird") \- [Firebird](<Firebird.md> "Firebird") \- Oracle \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- [dBASE](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<MS_Access.md> "MS Access") \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
## Contents

  * 1 Low level Oracle server interface
  * 2 Direct access to Oracle
  * 3 OOP access to Oracle
  * 4 Troubleshooting
    * 4.1 Client and server character sets
    * 4.2 ORA-00911 : invalid character



## Low level Oracle server interface

The low level Oracle server interface exists of one unit, **oraoci** , which is a straight translation of the Oracle interface header files. 

There are 2 example programs: 

  * **oraclew** contains some utility routines for the oracle interface, for easier management of result sets. Needs the classes unit from the FCL.
  * **test01** a simple test program to demonstrate the interface.



## Direct access to Oracle

You can directly connect Lazarus and Oracle by using Oracle Data Access Components (ODAC). It is a library of components that provides native connectivity to Oracle from Lazarus (and Free Pascal) on Windows, Mac OS X, iOS, Android, Linux, and FreeBSD for both 32-bit and 64-bit platforms. The ODAC library is designed to help programmers develop faster and more native Oracle database applications. 

This [Lazarus component](<https://www.devart.com/odac/download.html>) is free to download. 

## OOP access to Oracle

Built over the low-level interface, the SQLDB framework supplied with FPC supports accessing Oracle (using TOracleConnection); see also [Lazarus_Database_Overview#Lazarus_and_Oracle](<Lazarus_Database_Overview.md> "Lazarus Database Overview")

Lazarus also has a component: [TOracleConnection](<TOracleConnection.md> "TOracleConnection")

  * Hostname: as with other sqldb connectors, use hostname or IP address. Leave empty if you use a TNSNAMES.ORA net service name in DatabaseName
  * Username/password: same as with other sqldb connectors
  * DatabaseName: 
    * instance/SID of the Oracle server you want to connect to **or**
    * net service name in a TNSNAMES.ORA file



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Released FPC/Lazarus x64 versions on Windows do not include an Oracle connector. If you enable it and test successfully, please submit a patch with the changes so it can be included

## Troubleshooting

### Client and server character sets

To get info about what character set/NLS settings are active, you can run the following program. 

Please don't forget to fill out correct server name, username, password and database name before compiling. 
    
    
    program oracharset;
    
    { Shows client and server character set/NLS info}
    { PLEASE EDIT PASSWORDS ETC BELOW. }
    
    {$mode objfpc}{$H+}
    
    uses {$IFDEF UNIX} {$IFDEF UseCThreads}
      cthreads, {$ENDIF} {$ENDIF}
      Classes,
      SysUtils,
      sqldb,
      oracleconnection;
    
    var
      Col: integer;
      Conn: TOracleConnection;
      Tran: TSQLTransaction;
      Q: TSQLQuery;
    begin
      Conn := TOracleConnection.Create(nil);
      Tran := TSQLTransaction.Create(nil);
      Q := TSQLQuery.Create(nil);
      try
        // * EDIT IDENTIFYING INFO AS NEEDED*
        Conn.HostName := '';
        Conn.UserName := 'system';
        Conn.Password := '';
        Conn.DatabaseName := 'XE';
        // *END IDENTIFIYING INFO*
        Conn.Transaction := Tran;
        Q.DataBase := Conn;
        Conn.Open;
        Tran.Active := true;
    
        writeln('Server character set info:');
        Q.SQL.Text := 'SELECT value$ FROM sys.props$ WHERE name like ''NLS_%'' ';
        Q.Open;
        Q.First;
        while not (Q.EOF) do
        begin
          writeln('*****************');
          for Col := 0 to Q.Fields.Count - 1 do
          begin
            try
              writeln(Q.Fields[Col].DisplayLabel + ':');
              writeln(Q.Fields[Col].AsString);
            except
              writeln('Error retrieving field ', Col);
            end;
          end;
          Q.Next;
        end;
        Q.Close;
    
        writeln('');
        writeln('Client character set info:');
        Q.SQL.Text := 'SELECT * FROM NLS_SESSION_PARAMETERS ';
        Q.Open;
        Q.First;
        while not (Q.EOF) do
        begin
          writeln('*****************');
          for Col := 0 to Q.Fields.Count - 1 do
          begin
            try
              writeln(Q.Fields[Col].DisplayLabel + ':');
              writeln(Q.Fields[Col].AsString);
            except
              writeln('Error retrieving field ', Col);
            end;
          end;
          Q.Next;
        end;
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
    

### ORA-00911 : invalid character

If you see this error message, you might want to try 

  * removing a trailing semicolon - ; - if you have it in a SELECT statement
  * adding a trailing semicolon (e.g. in CALL or EXECUTE statements)



See this thread which applies to .Net but may apply to SQLDB as well: [[1]](<http://social.msdn.microsoft.com/Forums/en/adodotnetdataproviders/thread/58a27505-a3fb-4cb1-9063-3946b3f26acd>)

Go back to [Packages List](</index.php?title=Packages_List&action=edit&redlink=1> "Packages List \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/Oracle](https://web.archive.org/web/20250404133906/https://wiki.freepascal.org/Oracle)_
