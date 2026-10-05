# mysql

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


    [Advantage](<Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<mssqlconn.md> "mssqlconn") \- [Postgres](<postgres.md> "postgres") \- [Interbase](<Firebird.md> "Firebird") \- [Firebird](<Firebird.md> "Firebird") \- [Oracle](<Oracle.md> "Oracle") \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- [dBASE](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<MS_Access.md> "MS Access") \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
## Contents

  * 1 Overview
  * 2 MySQL licensing
  * 3 SQLDB
  * 4 SQLDB tutorials and example code
    * 4.1 Get MySQL working in Linux or Windows
    * 4.2 Get MySQL working for FPC in text mode
    * 4.3 Connecting to MySQL from a Lazarus Application
      * 4.3.1 Download example source code
    * 4.4 Lazarus, MySQL and UTF-8
    * 4.5 Simple MySQL Demo Using the TMySQL5xConnection Component
    * 4.6 Code-driven sample
    * 4.7 RAD/Databound controls
  * 5 Zeos
  * 6 Pascal Data Objects (PDO)
  * 7 MySQL package: the low level units
    * 7.1 Provided units and programs
    * 7.2 Installation
  * 8 See also



## Overview

You can use Free Pascal/Lazarus to access a MySQL database server. Also you can use MySQL Data access components (MyDAC) as a Lazarus component to connect Lazarus and MySQL. [Lazarus components MyDAC](<https://www.devart.com/mydac/download.html>) are free to download. 

Win64: please see warning [here](<Windows_Programming_Tips.md> "Windows Programming Tips") on not using certain FPC/Lazarus Win64 versions. 

Advantages of MySQL: 

  * It is very widely used and available
  * Though older versions had a deserved reputation of not being true RDBMSes, newer versions support ACID properties if properly set up (with the right storage backend)



Disadvantages of MySQL: 

  * The MySQL maintainers break binary compatibility between client library versions. This means that an FPC/Lazarus translation needs to be made for each new version, which slows things down.
  * The license may be restrictive to some users (e.g. in commercial deployments). There are compatible, possibly less restrictive alternatives like MariaDB.



A lot of Lazarus/FPC users prefer [Firebird](<Firebird.md> "Firebird") or [PostgreSQL](<postgresql.md> "postgresql") databases for this reason. 

## MySQL licensing

Before a significant MySQL deployment, don't forget to read its [license](<http://www.mysql.com/about/legal/licensing/index.html>). 

## SQLDB

Lazarus 1.2 (with FPC 2.6.2), supports 

  * MySQL 4.0 client library
  * MySQL 4.1 client library
  * MySQL 5.0 client library
  * MySQL 5.1 client library
  * MySQL 5.5 client library (supported since Lazarus 1.0.8)
  * MySQL 5.6 client library (supported since Lazarus <fill in>)



[![sqldbcomponents.png](https://wiki.freepascal.org/images/8/82/sqldbcomponents.png)](</File:sqldbcomponents.png>)

Newer versions of FPC/Lazarus may support newer MySQL client libraries. 

Make sure you are using the correct connection component for your **client** library version. So if you have the client libraries installed for MySQL 4.1 then you have to use TMySQL41Connection component, **even if the server is running** version 4.0 or 5.0. The reason for this is that MySQL client libraries often break compatibility of their API so each version needs a different Pascal driver. 

On *nix systems, the SQLDB code may look for the plain version of the library without version number suffixes. There are several ways you can deal with this: 

  * (on many Linuxes) installing the -dev version of the library
  * symlinking your specific library name to the one FPC is looking for (rather hackish)
  * use [TSQLDBLibraryLoader](<TSQLDBLibraryLoader.md> "TSQLDBLibraryLoader") to specify the library name before loading the connection



## SQLDB tutorials and example code

_BigChimp September 2012: note: a lot of this is duplicate code and could be consolidated. I propose to move as much as possible to the GUI-oriented SQLDB Tutorial1 and create a separate page FPC database tutorial to create a text mode sample_

### Get MySQL working in Linux or Windows

Follow the instructions in the MySQL User Manual. Make sure that the mysqld daemon runs reliably, and that all potential users (including root, mysql, yourself and anybody else that may need it) have as many privileges as they need, from as many hosts as may be needed (such as 'localhost', the local host's name, and other hosts on your network) as far as is consistent with security. It is preferable that all users including root have passwords. Test the action of the database system using the examples given in the manual, and check that all users really do have reliable access. 

### Get MySQL working for FPC in text mode

There is a directory with an example program in $(fpc**src** dir)/packages/base/mysql/. You can find the fpc source directory in Lazarus: Tools -> Options -> Files -> FPC source directory. Possible paths for the mysql directory are /usr/share/fpcsrc/packages/base/mysql/ (rpm install) or C:\lazarus\fpcsrc\packages\base\mysql\ (windows). This directory also contains the units mysql.pp, mysql_com.pp and mysql_version.pp. Before running the test script, you need to create a database called testdb: do this by logging into the mysql monitor (as root with full privileges) and issuing the following SQL statement 
    
    
    CREATE DATABASE testdb;
    

then make sure that all relevant users have appropriate access privileges to it 
    
    
    GRANT ALL ON testdb TO johnny-user IDENTIFIED BY 'johnnyspassword';
    

There is a script called mkdb which you should now try to run: 
    
    
    sh ./mkdb
    

This will probably fail, as the system will not allow an anonymous user to access the database. So change the script using an editor so that the line invoking mysql reads: 
    
    
    mysql -u root -p  ${1-testdb} << EOF >/dev/null
    

and try running it again, entering your password when prompted. With luck you might have managed to create the test database: test it (while logged in to the mysql monitor) by issuing the mysql statement 
    
    
    select * from FPdev;
    

You should see a table listing the ID, username and email address of some of the FPC developers. 

Now try to run the test program testdb.pp (this may need to be compiled, and will almost certainly fail on the first attempt!!). 

I found that the program could not connect to mysql for several reasons: 

  * My system (SuSE Linux v9.0) installs mysql v4.0.15, not the version3 for which the package was designed.


  * The program needs to have user names and passwords to get access to the database.


  * The compiler needs to know where to find the mysql libraries (IF YOU HAVEN'T INSTALLED THE MYSQL DEVELOPMENT LIBRARIES, DO SO NOW!)



I created a copy of testdb.pp called trydb.pp, rather than editing the original - this means that the original files still get fixed in subsequent CVS updates. I also copied the files found in the subdirectory mysql/ver40/ into the main mysql/ subdirectory, renaming them mysql_v4.pp, mysql_com_v4.pp and mysql_version_v4.pp, being sure to rename the units within each file correspondingly. I changed the uses statement in trydb.pp to 
    
    
    uses mysql_v4
    

and the statement in mysql_v4.pp to 
    
    
    uses mysql_com_v4
    

I added a line to /etc/fpc.cfg to point to my libraries: 
    
    
    -Fl/lib;/usr/lib
    

The following step might not be necessary if the devel-libraries are installed as the links will be created for you, but it never hurts to check. I had to find the real name of the mysqlclint library in the /usr/lib directory and in my case I had to issue the shell command: 
    
    
    ln -s libmysqlclient.so.12.0.0 lmysqlclient
    

to make a symbolic link allowing FPC to find the library. For good measure I also created the link 
    
    
    ln -s libmysqlclient.so.12.0.0 mysqlclient
    

and placed similar links in various other directories: not strictly necessary, but just in case ...! Some users might need to add the following link: 
    
    
    ln -s libmysqlclient.so.12.0.0 libmysqlclient.so
    

I modified trydb.pp to include user details, initially by adding host, user and password as constants: 
    
    
    const
      host : Pchar= 'localhost';
      user : Pchar= 'myusername';
      passwd: Pchar = 'mypassword';
    

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** This section looks extremely outdated. If you are still on MySQL 4 perhaps it is time to upgrade

I also found that I couldn't connect to mysql using the mysql_connect() call, but had to use mysql_real_connect() which has many more parameters. To complicate things further, the number of parameters seems to have changed between version3 (where there are seven) and version4 (where there are eight). Before using mysql_real_connect I had to use mysql_init() which is not found in the original mysql.pp but is found in mysql_v4.pp. 

So the code for connection to the database is now: 
    
    
    { a few extra variables}
    var
      alloc : PMYSQL;
      
    {main program fragment}
      
    begin
      if paramcount=1 then
      begin
        Dummy:=Paramstr(1)+#0;
        DataBase:=@Dummy[1];
      end;
      
      Writeln('Allocating Space...');
      alloc := mysql_init(PMYSQL(@qmysql));
      Write('Connecting to MySQL...');
      sock := mysql_real_connect(alloc, host, user, passwd, database, 0, nil, 0);
      if sock=Nil then
      begin
        Writeln(stderr,'Couldn''t connect to MySQL.');
        Writeln(stderr, 'Error was: ', mysql_error(@qmysql));
        halt(1);
      end;
      Writeln('Done.');
      Writeln('Connection data:');
     {$ifdef Unix}
      writeln('Mysql_port      : ',mysql_port);
      writeln('Mysql_unix_port : ',mysql_unix_port);
     {$endif}
      writeln('Host info       : ',mysql_get_host_info(sock));
      writeln('Server info     : ',mysql_stat(sock));
      writeln('Client info     : ',mysql_get_client_info);
      
      Writeln('Selecting Database ',DataBase,'...');
      if mysql_select_db(sock, DataBase) < 0 then
      begin
        Writeln(stderr,'Couldn''t select database ',Database);
        Writeln(stderr,mysql_error(sock));
        halt(1);
      end;
     {... as original contents of testdb.pp}
    

Now - ready to start compiling trydb.pp? 
    
    
     fpc trydb
    

success! Now run it: 
    
    
     ./trydb
    

whoopee! I got the listing of the FPC developers! 

A few extra refinements: make the entry of user details and the mysql commands interactive, using variables rather than constants, and allow several SQL commands to be entered, until we issue the quit command: see the [full program listing](</index.php?title=Lazarus_Database_Tutorial/TryDbpp&action=edit&redlink=1> "Lazarus Database Tutorial/TryDbpp \(page does not exist\)"), where user details are entered from the console, and the program goes into a loop where SQL commands are entered from the console (without the terminal semicolon) and the responses are printed out, until 'quit' is entered from the keyboard. 

See [Sample Console Listing](<Lazarus_Database_Tutorial/SampleListing.md> "Lazarus Database Tutorial/SampleListing"). 

### Connecting to MySQL from a Lazarus Application

This tutorial shows how to connect Lazarus to the MySQL database, and execute simple queries, using only the basic Lazarus components; it uses no Data Aware components, but illustrates the principles of interfacing with the database. 

Create a new project in Lazarus: 
    
    
    Project -> New Project -> Application
    

A new automatically generated Form will appear. 

Enlarge the form to fill about half of the screen, then re-name the form and its caption to 'TryMySQL'. 

From the Standard Component tab place three Edit Boxes on the upper left side of the Form, and immediately above each box place a label. Change the names and captions to 'Host' (and HostLLabel,HostEdit), 'UserName' (and UserLabel, UserEdit) and 'Password' (with PasswdLabel and PasswdEdit). Alternatively you could use LabelledEdit components from the Additional tab. 

Select the Passwd Edit box and find the PasswordChar property: change this to * or some other character, so that when you type in a password the characters do not appear on your screen but are echoed by a series of *s. Make sure that the Text property of each edit box is blank. 

Now place another Edit box and label at the top of the right side of your form. Change the label to 'Enter SQL Command' and name it CommandEdit. 

Place three Buttons on the form: two on the left under the Edit boxes, and one on the right under the command box. 

Label the buttons on the left 'Connect to Database' (ConnectButton)and 'Exit' (ExitButton) and the one on the right 'Send Query' (QueryButton). 

Place a large Memo Box labelled and named 'Results' (ResultMemo) on the lower right, to fill most of the available space. Find its ScrollBars property and select ssAutoBoth so that scroll bars appear automatically if text fills the space. Make the WordWrap property True. 

Place a Status Bar (from the Common Controls tab) at the bottom of the Form, and make its SimpleText property 'TryMySQL'. 

A screenshot of the Form can be seen here: [Mysql Example Screenshot](<http://lazarus-ccr.sourceforge.net/kbdata/trymysqldb.png>)

Now we need to write some event handlers. 

The three Edit boxes on the left are for entry of hostname, username and password. When these have been entered satisfactorily, the Connect Button is clicked. The OnCLick event handler for this button is based on part of the text-mode FPC program above. 

The responses from the database should be converted into strings and displayed in the Memo box. Text mode Pascal write and writeln statements are capable of performing a lot of type conversion 'on the fly', but the use of a memo box for text output requires explicit conversion of data types to the correct string: so Pchar variables have to be converted to strings using StrPas, and integers have to be converted with IntToStr. 

Strings are displayed in the Memo box using: 
    
    
    procedure ShowString(S : string);
    (* display a string in a Memo box *)
    begin
      trymysqlForm1.ResultsMemo.Lines.Add(S)
    end;
    

The ConnectButton event handler thus becomes: 
    
    
    procedure TtrymysqlForm1.ConnectButtonClick(Sender: TObject);
    (* Connect to MySQL using user data from Text entry boxes on Main Form *)
    var strg: string;
      
    begin
      dummy1 :=  trymysqlForm1.HostEdit.text+#0;
      host := @dummy1[1];
      dummy2 := trymysqlForm1.UserEdit.text+#0;
      user := @dummy2[1] ;
      dummy3 := trymysqlForm1.PasswdEdit.text+#0;
      passwd := @dummy3[1] ;
    
      alloc := mysql_init(PMYSQL(@qmysql));
      sock :=  mysql_real_connect(alloc, host, user, passwd, database, 0, nil, 0);
      if sock=Nil then
        begin
          strg :='Couldn''t connect to MySQL.'; showstring (strg);
          Strg :='Error was: '+ StrPas(mysql_error(@qmysql)); showstring (strg);
        end
        else
        begin
          trymysqlForm1.statusBar1.simpletext := 'Connected to MySQL';
          strg := 'Now choosing database : ' + database; showstring (strg);
     {$ifdef Unix}
          strg :='Mysql_port      : '+ IntToStr(mysql_port); showstring (strg);
          strg :='Mysql_unix_port : ' + StrPas(mysql_unix_port); showstring (strg);
     {$endif}
          Strg :='Host info       : ' + StrPas(mysql_get_host_info(sock));
          showstring (strg);
          Strg :='Server info     : ' + StrPas(mysql_stat(sock)); showstring (strg);
          Strg :='Client info     : ' + Strpas(mysql_get_client_info);  showstring (strg);
      
          trymysqlForm1.statusbar1.simpletext := 'Selecting Database ' + DataBase +'...';
          if mysql_select_db(sock,DataBase) < 0 then
          begin
            strg :='Couldn''t select database '+ Database; ShowString (strg);
            Strg := mysql_error(sock); ShowString (strg);
          end
        end;
    end;
    

The Text Box on the right allows entry of a SQL statement, without a terminal semicolon; when you are satisfied with its content or syntax, the SendQuery button is pressed, and the query is processed, with results being written in the ResultsMemo box. 

The SendQuery event handler is again based on the FPC text-mode version, except that once again explicit type-conversion has to be done before strings are displayed in the box. 

A difference from the text-mode FPC program is that if an error condition is detected, the program does not halt and MySQL is not closed; instead, control is returned to the main form and an opportunity is given to correct the entry before the command is re-submitted. The application finally exits (with closure of MySQL) when the Exit Button is clicked. 

The code for SendQuery follows: 
    
    
    procedure TtrymysqlForm1.QueryButtonClick(Sender: TObject);
    var
      dumquery, strg: string;
    begin
      dumquery := TrymysqlForm1.CommandEdit.text;
      dumquery := dumquery+#0;
      query := @dumquery[1];
      trymysqlForm1.statusbar1.simpletext := 'Executing query : '+ dumQuery +'...';
      strg := 'Executing query : ' + dumQuery; showstring (strg);
      if (mysql_query(sock,Query) < 0) then
      begin
        Strg :='Query failed '+ StrPas(mysql_error(sock)); showstring (strg);
      end
      else
      begin
        recbuf := mysql_store_result(sock);
        if RecBuf=Nil then
        begin
          Strg :='Query returned nil result.'; showstring (strg);
        end
        else
        begin
          strg :='Number of records returned  : ' + IntToStr(mysql_num_rows (recbuf));
          Showstring (strg);
          Strg :='Number of fields per record : ' + IntToStr(mysql_num_fields(recbuf));
          showstring (strg);
          rowbuf := mysql_fetch_row(recbuf);
          while (rowbuf <>nil) do
          begin
            Strg :='(Id: '+ rowbuf[0]+', Name: ' + rowbuf[1]+ ', Email : ' +
            rowbuf[2] +')';
            showstring(strg);
            rowbuf := mysql_fetch_row(recbuf);
          end;
        end;
      end;
    end;
    

Save your Project, and press Run -> Run 

#### Download example source code

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Current versions of Lazarus/FPC require committing all MySQL transactions. This was not necessary in earlier versions. The code download likely will not work until it is updated.

A full listing of the program is available here [Sample Source Code](<http://sourceforge.net/projects/lazarus-ccr/files/Demos%20and%20examples/MySql%20demo%20using%20SQLdb%20components%208%20April%202005/mysql_demo_20050408.tar.gz/download>)

### Lazarus, MySQL and UTF-8

_The following may be required for other codepages/character sets as well_

UTF-8 Unicode is a convenient multibyte character set encoding, that allows working with multilingual texts without requiring WideStrings. It is supported both by Lazarus SQLdb components and by MySQL since version 4.1 by choosing the appropriate character set. 

However, simply setting this encoding as default for 

  * your tables and
  * the MySQL connection component (e.g. `TMySQL51Connection.CharSet:='UTF8';`)



will result in incorrect storage and retrieval of UTF-8 strings: any accented/international character will show up as question mark (?). Apparently, the reason for this is that MySQL client library is compiled to expect Latin1 character set by default. 

In order to enable proper communication between Lazarus, MySQL client library and MySQL server, additional two queries need to be executed **each time** a connection to the database is established: 
    
    
    SET CHARACTER SET `utf8`
    

and 
    
    
    SET NAMES 'utf8'
    

The first query will ensure your application receives strings in correct encoding, and the second tells MySQL not to convert strings it receives from your application. 

### Simple MySQL Demo Using the TMySQL5xConnection Component

Here is code that functions as a quick demo to get up and running simply. As with all SQLDB components, make sure the database client library is in the correct place: 

  * on Windows: the DLL, e.g. libmysql.dll is put in the project output directory (where the executable is generated). Alternatively, you could also place it in your Windows/system32 directory
  * on Linux/OSX: install the mysql client library in your path (e.g. using your distribution's package manager)



When distributing your application, make sure the proper MySQL client library is present on your user's computer. As this can be a problem, perhaps using a different database engine may make more sense. 

### Code-driven sample

The example below uses code to fill your controls with data. You can also use data-bound controls, which might be quicker/easier. See the example below it or [SQLdb_Tutorial1](<SQLdb_Tutorial1.md> "SQLdb Tutorial1") for this. 

Place three edit boxes, a memo box and a few buttons on the form. You need to add mysqlXXconn and sqldb to the uses statement. 

In this example, the MySQL DBMS has a user 'root' with no password, and a database 'test1' with table 'tPerson' which has three fields: 'personid' (int), 'surname' (varchar(40)) and 'dob' (datetime). Also, some test data was inserted. 

The button btnTest must be clicked first as it creates the connection with the DBMS. Note the line that applies updates - without this the changed or new data will not be written back to the DB though they will be in memory and can be viewed using btnFirst and btnNext. 
    
    
    unit unt_db;
    // Example based on:
    // http://www.lazarus.freepascal.org/index.php?name=PNphpBB2&file=viewtopic&t=5761
    // from tpglemur on that forum
    {$mode objfpc}{$H+}
    interface
    uses
      Classes, SysUtils, LResources, Forms, Controls, Graphics, Dialogs,
      mysql50conn, sqldb, StdCtrls;
    type
      { TForm1 }
      TForm1 = class(TForm)
        btnTest: TButton;
        btnNext: TButton;
        btnFirst: TButton;
        btnNew: TButton;
        edtPersonID: TEdit;
        edtSurname: TEdit;
        edtDOB: TEdit;
        Memo1: TMemo;
        procedure btnFirstClick(Sender: TObject);
        procedure btnNewClick(Sender: TObject);
        procedure btnNextClick(Sender: TObject);
        procedure btnTestClick(Sender: TObject);
      private
        { private declarations }
        conn : TMySQL50Connection;
        query : TSQLQuery;
        transaction : TSQLTransaction;
        procedure Display;
      public
        { public declarations }
      end;
    var
      Form1: TForm1;
    implementation
    { TForm1 }
    procedure TForm1.btnTestClick(Sender: TObject);
    var
      S: String;
    begin
      conn := TMySQL50Connection.Create(nil);
      query := TSQLQuery.Create(nil);
      transaction := TSQLTransaction.Create(nil);
      try
        try
          // Adjust to your own database server, username and password:
          conn.HostName := '127.0.0.1';
          conn.UserName := 'root';
          conn.Password := '';
          // If you use a different database name, adjust here:
          conn.DatabaseName := 'test1';
          conn.Connected := True;
          conn.Transaction := transaction;
          query.DataBase := conn;
          //query.ParseSQL := true; //line not needed - this is the default anyway
          //query.ReadOnly := false; //line not needed - this is the default anyway
          query.SQL.Text := 'select * from tperson';
          query.Open;
    
          query.Last;
          S := IntToStr(query.RecordCount) + #13#10;
          query.First;
    
          while not query.EOF do
          begin
            S := S + query.FieldByName('surname').AsString + #13#10;
            query.Next;
          end;
        finally
          //query.Free;
          //conn.Free;
        end;
      except
        on E: Exception do
          ShowMessage(E.message);
      end;
      Memo1.Text:= S;
    end;
    
    procedure TForm1.Display;
    begin
      edtPersonID.Text := query.FieldByName('personid').AsString;
      edtSurname.Text := query.FieldByName('surname').AsString;
      edtDOB.Text := query.FieldByName('dob').AsString;
    end;
    
    procedure TForm1.btnFirstClick(Sender: TObject);
    begin
      query.First;
      Display;
    end;
    
    procedure TForm1.btnNewClick(Sender: TObject);
    begin
      query.Append;
      query.FieldValues['personid'] := edtPersonID.Text;
      query.FieldValues['surname'] := edtSurname.Text;
      query.FieldValues['dob'] := edtDOB.Text;
      query.Post;  
      query.ApplyUpdates; //to apply update
      transaction.Commit; //Needed since FPC 2.6.4; todo: somebody should check if this is enough
    end;
    
    procedure TForm1.btnNextClick(Sender: TObject);
    begin
      query.Next;
      Display;
    end;
    
    initialization
      {$I unt_db.lrs}
    end.
    

### RAD/Databound controls

Please see [SQLdb_Tutorial1](<SQLdb_Tutorial1.md> "SQLdb Tutorial1"), [SQLdb_Tutorial2](<SQLdb_Tutorial2.md> "SQLdb Tutorial2") and further tutorials. 

## Zeos

See [ZeosDBO](<ZeosDBO.md> "ZeosDBO")

## Pascal Data Objects (PDO)

Pascal Data Objects is an alternative data access layer that seems to support: 

  * MySQL 4.0 clients
  * MySQL 4.1 clients
  * MySQL 5.0 clients
  * (and also) Firebird 1.5 and 2.0



Functions introduced with MySQL 4.1 and 5.0 like prepared statements, binding, and stored procedures are supported. PDO is inspired by PHP Data Objects. All the code and documentation necessary to use this API is available on sourceforge: 

[[1]](<http://pdo.sourceforge.net>)

## MySQL package: the low level units

As with all databases, the SQLDB code depends on a lower level mysql specific unit that wraps around the mysql driver library (.so/.dll). Normally, you would use the higher-level SQLDB code as it allows you to code more quickly, easily switch databases etc. 

Using this is very easy, all you need to do is compile some units, and use these units in your program. You need to specify the location in the filesystem of the MySQL client Library (libmysqlclient on Linux) when compiling, and that is it. 

### Provided units and programs

The packages provides 3 units, of which normally only the first is needed: 

  * _mysql_ the main mysql unit.
  * _mysql <version>_ (e.g. _mysql50_) provides access to the specific mysql library for that version. Note: the client library version is unrelated to the version of the server in use - except that you want to make sure these versions are compatible.
  * _mysql <version>com_ contains some internal routines of MySQL (presumably a translation of mysql_com.h); it should normally not be used unless you want access to some internal types.



Example programs can be found in the <fpc>\packages\mysql\examples directory. 

### Installation

The mysql interface is distributed with the Free Pascal packages, and come with the compiler distribution: Normally no action should be taken to work with MySQL. 

In case you want to modify and compile the units yourself, the mysql sources are in the packages directory: _packages/mysql_

This directory contains the units, a test program and a makefile. cd to the directory and type 
    
    
    make
    

This should compile the units. If compilation was succesful, you can install with 
    
    
    make install
    

You can then test the program by running 
    
    
    make test
    

This will: 

  * Run a script to create a table in a database, and fill it with some data. (the mysql program should be in your PATH for this) . By default, the used database is testdb.
  * Run the testprogram testdb
  * Run a shell script again to remove the created table.



You will see a lot of messages on your screen, giving you feedback and results. If something went wrong, make will inform you of this. 

Go back to [Packages List](<Package_List.md> "Package List")

## See also

  * [Lazarus DB Faq](<Lazarus_DB_Faq.md> "Lazarus DB Faq") \- More about database programming
  * [SQLdb Tutorial1](<SQLdb_Tutorial1.md> "SQLdb Tutorial1") \- SQLDB tutorial series that takes you through creating a working database grid GUI application
  * [Lazarus Database Overview](<Lazarus_Database_Overview.md> "Lazarus Database Overview") \- Getting Lazarus working with various database

---

_Source: [https://wiki.freepascal.org/MySQL](https://web.archive.org/web/20220818203429/https://wiki.freepascal.org/MySQL)_
