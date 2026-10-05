# mssqlconn

│ **English (en)** │  **[español (es)](</mssqlconn/es> "mssqlconn/es")** │  **[français (fr)](</mssqlconn/fr> "mssqlconn/fr")** │  **[polski (pl)](</mssqlconn/pl> "mssqlconn/pl")** │    
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


    [Advantage](<Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<MySQLDatabases.md> "MySQLDatabases") \- MSSQL \- [Postgres](<postgres.md> "postgres") \- [Interbase](<Firebird.md> "Firebird") \- [Firebird](<Firebird.md> "Firebird") \- [Oracle](<Oracle.md> "Oracle") \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- [dBASE](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<MS_Access.md> "MS Access") \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
**mssqlconn** is an fcl-db unit containing connector code for [TMSSQLConnection](<TMSSQLConnection.md> "TMSSQLConnection") (MS SQL Server) and [TSybaseConnection](<TSybaseConnection.md> "TSybaseConnection") (Sybase ASE). 

Lazarus uses mssqlconn to present an MS SQL connector and Sybase ASE connector. 

## Contents

  * 1 DBLib
  * 2 Installation
  * 3 Documentation
  * 4 Limitations and tips
    * 4.1 Output parameters of stored procedures
  * 5 Error messages
    * 5.1 Error 20009 : Unable to connect: Adaptive Server is unavailable or does not exist
    * 5.2 Error 20019 : Attempt to initiate a new Adaptive Server operation with results pending
    * 5.3 Error 20047 : DBPROCESS is dead or not enabled
    * 5.4 EINOutError/Can not load DB-Lib client library "dblib.dll". Check your installation
  * 6 Examples
    * 6.1 Creating a database



## DBLib

mssqlconn uses the dblib low-level unit which requires the [FreeTDS](<http://www.freetds.org>) library (.dll/.dylib/.so) to connect to the server. 

## Installation

The only thing you need is a FreeTDS dll/dylib/so for your platform. 

  * On Linux, download the freetds package which provides **libsybdb.so** (e.g. in Debian/Raspbian this is provided by package **libsybdb5**) and the related development package using your package manager. You might also need to add a symbolic link libsybdb.so to libsybdb.so.5 
    * Ubuntu 20.04 

    `sudo apt install libsybdb5`
    `cd /usr/lib/x86_64-linux-gnu`
    `sudo ln -s libsybdb.so.5.1.0 libsybdb.so`
  * On Windows, you can download a recent 32 or 64 bit version of the FreeTDS library **dblib.dll** here: <ftp://ftp.freepascal.org/fpc/contrib/windows/> or <http://downloads.freepascal.org/fpc/contrib/windows/>



Advanced use: by modifying FPC file **dblib.pas** SQLDB could use the "native" library **ntwdblib.dll** instead of the default FreeTDS library **dblib.dll** (in this case FPC needs to be recompiled). 

## Documentation

Official documentation: [FPC documentation on mssqlconn](<http://www.freepascal.org/docs-html/fcl/mssqlconn/index.html>)

## Limitations and tips

  * Sybase port: the default port for Sybase ASE is 5000. As FreeTDS/mssqlconn uses 1433 by default, you may want to add :5000 to the hostname proprerty automatically or after feedback from the end user.



As the documentation specifies: 

  * you might need to tweak some parameters for BLOB support.
  * MS SQL multiple resultsets (MARS) are not supported.



Change the drivername and/or position 

  * Put dblib as first unit in your .lpr and then call dblib.initialisedblib('/position/to/libsybdb.so')



### Output parameters of stored procedures

Also, **TMSSQLConnection** and **TSybaseConnection** do not support handling of return status and output parameters of stored procedures. For more informations see: <http://www.freetds.org/faq.html#ms.output.parameters> As a workaround you can use something like: 
    
    
    with SQLQuery1 do begin
      SQL.Text := 'declare @param int; declare @ret int;' +
                  'exec @ret=MyStoredProc 1, @param OUTPUT;' +
                  'select @ret as return_status, @param as out_param';
      Open;
    end;
    

Some more tips: 

  * You might want to do an YourConnection.ExecuteDirect('SET ANSI_NULL_DFLT_ON ON'); if you want to automatically allow NULL values for columns when creating tables. See MS documentation: <http://msdn.microsoft.com/en-us/library/ms174979.aspx>
  * You might want to similarly set SET ANSI_PADDING to ON to enable better compatibiity with sqldb text comparison functions. See MS documentation: <http://msdn.microsoft.com/en-us/library/ms187403.aspx>



## Error messages

### Error 20009 : Unable to connect: Adaptive Server is unavailable or does not exist

  * Check if TCP/IP protocol is enabled on SQL Server side using "SQL Server Configuration Manager" - "SQL Server Network Configuration"
  * Check if TCP port 1433 is opened
  * If you connect to specific instance of SQL Server (this is also case of SQL Server Express), check if SQL Server Browser Service is running on server (service listens on UDP port 1434)



### Error 20019 : Attempt to initiate a new Adaptive Server operation with results pending

  * Set TSQLQuery.PacketRecords property to -1
  * For more information see: <http://www.freetds.org/faq.html#pending>



### Error 20047 : DBPROCESS is dead or not enabled

  * SQL Server periodically verifies (by default every 30 seconds) if idle TCP/IP connection is still intact by sending a keep alive packet to its peer. If the remote system is still reachable and functioning, a acknowledge packet is sent back. If not local TCP will reset connection.
  * Adjust SQL Server Configuration Manager -> SQL Server Network Configuration -> Protocols -> TCP/IP : "Keep Alive"



### EINOutError/Can not load DB-Lib client library "dblib.dll". Check your installation

  * Make sure you have all required dlls installed (e.g. **dblib.dll** , **libiconv2.dll**)
  * Make sure you have all required C/C++ runtime libraries installed that **dblib.dll** depends on
  * Make sure the bitness (32 or 64 bit) of the dblib library and your compiled program match



## Examples

### Creating a database

The example below creates a database on an MS SQL Server. For this to work, the connection's parameter AutoCommit must be set to on. For normal subsequent connection you can switch it off again and use regular **StartTransaction** and **Commit**. 
    
    
    // Set up a form with a button called DBButton, 
    // textbox called DatabaseNameEdit,
    // '''MSSQLConnection''' called FConn
    // ***make sure the Autocommit parameter is on for the create db part before connectiong:
    //FConn.Params.Add('AutoCommit=true');
    procedure TForm1.CreateDBButtonClick(Sender: TObject);
    var
      CurrentDB: string;
    begin
      if DatabaseNameEdit.Text = '' then
      begin
        showmessage('Empty database name. Please specify database name first.');
        exit;
      end;
    
      CurrentDB := FConn.DatabaseName;
      try
        // For FPC 2.6.1+
        FConn.ExecuteDirect('CREATE DATABASE ' + DatabaseNameEdit.Text);
        {
        // This works with FPC 2.7.1, not on 2.6.1
        FConn.CreateDB;
        }
      except
        on E: Exception do
        begin
          showmessage('Exception running CreateDB: ' + E.Message);
        end;
      end;
    end;

---

_Source: [https://wiki.freepascal.org/mssqlconn](https://web.archive.org/web/20241207110807/https://wiki.freepascal.org/mssqlconn)_
