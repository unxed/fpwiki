# MySQLDatabases

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


    [Advantage](<Advantage_Database_Server.md> "Advantage Database Server") \- MySQL \- [MSSQL](<mssqlconn.md> "mssqlconn") \- [Postgres](<postgres.md> "postgres") \- [Interbase](<Firebird.md> "Firebird") \- [Firebird](<Firebird.md> "Firebird") \- [Oracle](<Oracle.md> "Oracle") \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- [dBASE](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<MS_Access.md> "MS Access") \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
## Contents

  * 1 Introduction
  * 2 Available Components
    * 2.1 SQLdb Components
  * 3 Explanation of the used components
    * 3.1 TMySQLConnection
    * 3.2 TSQLTransaction
    * 3.3 TSQLQuery
    * 3.4 TDataSource
    * 3.5 TDBGrid
  * 4 Our program
    * 4.1 The basics
    * 4.2 The main form
    * 4.3 The code
      * 4.3.1 Connect to a server
      * 4.3.2 Selecting a database
      * 4.3.3 Fields in a table
      * 4.3.4 Showing the data
  * 5 Sources
  * 6 See also



## Introduction

This page will explain how to connect to a [MySQL](<mysql.md> "mysql") server using visual components. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This page has been written a long time ago and may be out of date. Also, most of the concepts described here are not MySQL specific but apply to all SQLDB databases. Therefore, following the SQLdb Tutorial series mentioned below may be easier

Note: see also these tutorials that teach data-bound controls, parameterized queries, database independent programming etc: 

  * [SQLdb Tutorial1](<SQLdb_Tutorial1.md> "SQLdb Tutorial1")
  * [SQLdb Tutorial2](<SQLdb_Tutorial2.md> "SQLdb Tutorial2")
  * [SQLdb Tutorial3](<SQLdb_Tutorial3.md> "SQLdb Tutorial3")



They are written for all databases that support sqldb, including MySQL. 

If needed, see also [mysql#SQLDB_tutorials_and_example_code](<mysql.md> "mysql") for yet more tutorials (that are also written a long time ago. 

## Available Components

### SQLdb Components

In any even vaguely recent version of Lazarus, the SQLDB components are installed by default. 

[![sqldbcomponents.png](https://wiki.freepascal.org/images/8/82/sqldbcomponents.png)](</File:sqldbcomponents.png>)

On the SQLDB tab you will find: 

  * Various connectors, including [TMySQL40Connection](<TMySQL40Connection.md> "TMySQL40Connection")..[TMySQL56Connection](<TMySQL56Connection.md> "TMySQL56Connection") (or perhaps even newer versions) and the most versatile of all [TSQLConnector](<TSQLConnector.md> "TSQLConnector") that may load any of the mysql/oracle/postgres/mssql/interbase/firebird/odbc drivers.
  * [TSQLQuery](<TSQLQuery.md> "TSQLQuery")



If the SQLDB tab is missing, have a look at [Install Packages](<Install_Packages.md> "Install Packages") for an "Install Howto". 

## Explanation of the used components

### TMySQLConnection

The TMySQLConnection is used to store parameters to connect to the database server. It enables you to set the host to connect to, and the userid and password to use in the connection. Another property of the TMySQLConnection is used to indicate the database you want to use. The 'LoginPrompt' is not functional yet, so make sure that next to the HostName and DatabaseName the UserName and Password properties have values as well before you try to open the connection. Be sure to use a TSQLTransaction as well and connect it to your MySQLConnection by setting the Transaction property, or you will not be able to open a SQLQuery. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** As indicated above, there are various MySQLConnection components that differ in version number. The version number **must** match the version number of the client library you use to connect to your server. So if you are running a MySQL 5.1 server but use a MySQL 5.0 client, use the TMySQL50Connection.

You should check MySQL documentation to make sure the combination between client version and server version is supported - e.g. you may well have problems connecting to a 4.0 server using a 5.x client. 

In all cases put a copy of libmysql.dll _and any other required files/dlls_

  * in your Lazarus directory and the same directory as your project files or
  * in the Windows system directory (if you don't want to keep copying files). Note that on 64 bit Windows you have to put the 32 bit library in SysWOW64, while 64 bit libraries go into System32.



### TSQLTransaction

A [TSQLTransaction](<TSQLTransaction.md> "TSQLTransaction") is needed for some internal housekeeping. A SQLTransaction is automatically activated when you open a dataset using it. Closing a connection also deactivates the related transaction and closes all datasets using it. 

### TSQLQuery

[TSQLQuery](<TSQLQuery.md> "TSQLQuery") is used to execute SQLstatements on the server. You can retrieve data by setting the SQL to some SELECT statement and call the Open method. Or you can manipulate data by issuing some an INSERT, DELETE or UPDATE statement. In the latter case you should not use the Open method but the ExecSQL method. 

### TDataSource

A [TDataSource](<TDataSource.md> "TDataSource") provides the connection between the visible data aware components like DBEdit, DBGrid and a dataset. It makes the data available for the data aware components to display. A datasource can only be connected to a single dataset at a time but there can be several data aware components connected. 

### TDBGrid

A [TDBGrid](<TDBGrid.md> "TDBGrid") can be used to present the data retrieved by a Dataset. The DBGrid needs a datasource to connect to a dataset. When the dataset is opened, the DBgrid will automatically be populated with the data. 

## Our program

### The basics

We will try to make a program based on the one made here (in Dutch) which is based on the [original (in English)](<Lazarus_Database_Tutorial.md> "Lazarus Database Tutorial") by [Chris](</User:Kirkpatc> "User:Kirkpatc"). 

### The main form

We will use the same main screen and build all functionality from scratch :) As you will see there is a lot less to take care of, because the components really take away all the hard stuff! So lets start by making a screen that looks like this. 
    
    
    [![Trymysql.png](https://wiki.freepascal.org/images/2/28/Trymysql.png)](</File:Trymysql.png>)
    

From the SQLdb-tab place a [TMySQL56Connection](<TMySQL56Connection.md> "TMySQL56Connection") [![tmysql56connection.png](https://wiki.freepascal.org/images/7/76/tmysql56connection.png)](</File:tmysql56connection.png>) (or other mysql-client version) a [TSQLTransaction](<TSQLTransaction.md> "TSQLTransaction") [![tsqltransaction.png](https://wiki.freepascal.org/images/f/fd/tsqltransaction.png)](</File:tsqltransaction.png>) and a [TSQLQuery](<TSQLQuery.md> "TSQLQuery") [![tsqlquery.png](https://wiki.freepascal.org/images/b/be/tsqlquery.png)](</File:tsqlquery.png>) on this form. Don't change the default names given to this components. Except for the connection component. To make this article the same for all versions of MySQL, name your TMySQL##Connection component: `MySQLConnection1`. We have to link these components together so they can do their job. So the following properties have to be set: 

**Component** | **Property** | **Value**  
---|---|---  
MySQLConnection1 | Transaction | SQLTransaction1   
SQLTransaction1 | Database | MySQLConnection1   
SQLQuery1 | Transaction | SQLTransaction1   
SQLQuery1 | Database | MySQLConnection1   
  
The Transaction-property of SQLQuery1 will automatically be set if you have set the Transaction property of MySQLConnection1 first. When you set this, you will notice that SQLTransaction1.Database has been set to MySQLConnection1. 

As said earlier: Make sure you are using the correct Connection component for your version of MySQL server. 

### The code

As you can see in the screen dump the only buttons available on start of the program are "Connect to server" and "Exit". For the other buttons to work we need more information so these are disabled. We could decide to disable "Connect to Server" as well until the information for the host, username and password has been given. I decided against this because our user might think: "Nothing seems possible, so let's hit exit." :) 

Before I start giving you any code I would like to stress that there should be more exception handling in the code. Critical sections should be placed in 
    
    
    try ... finally
    

or 
    
    
    try ... except
    

constructions. 

#### Connect to a server

The first thing we have to do is get connected to our server. As when connecting we don't know what databases are available on the server we will ask for a list of databases on connecting. However there is one catch, to make the connection we have to enter a valid DatabaseName in the properties of the MySQLConnection. You will see in the code that I am using the "mysql" database. This database is used by mysql for some housekeeping so it will always be there. 
    
    
    procedure TFormTryMySQL.ConnectButtonClick(Sender: TObject);
    begin
      // Check if we have an active connection. If so, let's close it.
      if MySQLConnection1.Connected then CloseConnection(Sender);
      // Set the connection parameters.
      MySQLConnection1.HostName := HostEdit.Text;
      MySQLConnection1.UserName := UserEdit.Text;
      MySQLConnection1.Password := PasswdEdit.Text;
      MySQLConnection1.DatabaseName := 'mysql'; // MySQL is allways there!
      ShowString('Opening a connection to server: ' + HostEdit.Text);
      MySQLConnection1.Open;
      // First lets get a list of available databases.
      if MySQLConnection1.Connected then begin
        ShowString('Connected to server: ' + HostEdit.Text);
        ShowString('Retrieving list of available databases.');
        SQLQuery1.SQL.Text := 'show databases';
        SQLQuery1.Open;
        while not SQLQuery1.EOF do begin
          DatabaseComboBox.Items.Add(SQLQuery1.Fields[0].AsString);
          SQLQuery1.Next;
        end;
        SQLQuery1.Close;
        ShowString('List of databases received!');
      end;
    end;
    

The first thing we do is check to see if we are connected to a server, if we are then we call a private method "CloseConnection". In this method some more housekeeping is done. like disabling buttons and clearing comboboxes and listboxes. Then we set the necessary parameters to connect to server. 

    _Throughout our program you may see calls to ShowString. This method adds a line to the memo on our form which acts like a kind of log._

With the parameters set, we can connect to the server. This is done by calling 
    
    
    MySQLConnection1.Open;
    

In a proper application one would place this in an exception handling construct to present a friendly message to the user if the connection failed. When we are connected we want to get a list of databases from the server. To get data from the server a TSQLQuery is used. The SQL property is used to store the SQL-statement send to the server. MySQL knows the "SHOW DATABASES" command to get the list of databases. So after we have set the SQL-text, we call 
    
    
    SQLQuery1.Open;
    

On MySQL5 set this to correct error with SQL syntax: 
    
    
    SQLQuery1.ParseSQL := False; 
    SQLQuery1.ReadOnly := True;
    

The result set of a SQLQuery can be examined through the fields property. As you can see we iterate through the records by calling 
    
    
    SQLQuery1.Next;
    

When we have added all available databases to our combobox, we close the SQLQuery again. 

#### Selecting a database

If the user selects a database in the DatabaseComboBox we enable the "Select Database" button. In the OnClick event of this button we set the DatabaseName of MySQLConnection1, and request a list of tables. The last statement of this procedure enables the "Open Query" Button, so the user can enter a query in the "Command" Editbox and have it send to the server. 
    
    
    procedure TFormTryMySQL.SelectDBButtonClick(Sender: TObject);
    begin
      // A database has been selected so lets get the tables in it.
      CloseConnection(Sender);
      if DatabaseComboBox.ItemIndex <> -1 then begin
        with DatabaseComboBox do
          MySQLConnection1.DatabaseName := Items[ItemIndex];
        ShowString('Retreiving list of tables');
        SQLQuery1.SQL.Text := 'show tables';
        SQLQuery1.Open;
        while not SQLQuery1.EOF do begin
          TableComboBox.Items.Add(SQLQuery1.Fields[0].AsString);
          SQLQuery1.Next;
        end;
        SQLQuery1.Close;
        ShowString('List of tables received');
      end;
      OpenQueryButton.Enabled := True;
    end;
    

MySQL has a special command to get a list of tables, comparable to getting the list of databases, "show tables". The result of this query is handled in the same way as the list of databases and all the tables are added to the TableComboBox. You might wonder why we do not open the connection again before opening the query? Well, this is done automatically (if necessary) when we activate the SQLQuery. 

#### Fields in a table

In MySQL you can again use a form of "SHOW" to get the fields in a table. In this case "SHOW COLUMNS FROM <tablename>". If the user picks a table from the TableComboBox the OnChangeEvent of this ComboBox is triggered which fills the FieldListbox. 
    
    
    procedure TFormTryMySQL.TableComboBoxChange(Sender: TObject);
    begin
      FieldListBox.Clear;
      SQLQuery1.SQL.Text := 'show columns from ' + TableComboBox.Text;
      SQLQuery1.Open;
      while not SQLQuery1.EOF do begin
        FieldListBox.Items.Add(SQLQuery1.Fields[0].AsString);
        SQLQuery1.Next;
      end;
      SQLQuery1.Close;
    end;
    

As well as the names of the fields, the result set contains information on the type of field, if the field is a key, if nulls are allowed and some more. 

#### Showing the data

Well as we said we would use components to get connected to the database, lets use some components to show the data as well. We will use a second form to show a grid with the data requested by the user. This form will be shown when the user typed a SQL command in the "Command" editbox and afterwards clicks the "Open Query" button. This is the OnClick event: 
    
    
    procedure TFormTryMySQL.OpenQueryButtonClick(Sender: TObject);
    begin
      ShowQueryForm := TShowQueryForm.Create(self);
      ShowQueryForm.Datasource1.DataSet := SQLQuery1;
      SQLQuery1.SQL.Text := CommandEdit.Text;
      SQLQuery1.Open;
      ShowQueryForm.ShowModal;
      ShowQueryForm.Free;
      SQLQuery1.Close;
    end;
    

The ShowQueryForm looks like this: 

[![Mysqlshow.png](https://wiki.freepascal.org/images/4/47/Mysqlshow.png)](</File:Mysqlshow.png>)

and contains a 

[TPanel](<TPanel.md> "TPanel") | Align | alBottom   
---|---|---  
[TDataSource](<TDataSource.md> "TDataSource") |  |   
[TDBGrid](<TDBGrid.md> "TDBGrid") | Align | alClient   
| DataSource | DataSource1   
[TButton](<TButton.md> "TButton") | Caption | Close   
  
The button is placed on the panel. What happens in the "Open Query" OnClick is this. First we create an instance of TShowQueryForm. Secondly we set the DataSet property of the DataSource to our SQLQuery1. Then we set the SQLQuery SQL command to what the user entered in the "Command" editbox and open it. Then the ShowQueryForm is shown modally, this means that it will have the focus of our application until it is closed. When it is closed, we "free" it and close SQLQuery1 again. 

The Form can be further enhanced by inserting a method for modifying the content of the [TDataSet](<TDataSet.md> "TDataSet") and ultimately the DataBase. A full version can be downloaded from <http://digitus.itk.ppke.hu/~janma/lazarus/MySql5Test.tar.gz> (with thanks to Arwen and JZombi from the Lazarus MySQL Forum) but the relevant details are as follows: 

Add a Button at the bottom of the ShowQuery Form named AddButton and with Caption 'Add'. Create a method for AddButtonClick like this: 
    
    
    procedure TShowQueryForm.AddButtonClick(Sender: TObject);
    begin
      DataSource1.DataSet.Append;
    end;
    

Change the code for OpenQueryButtonClick to allow for updates to the database when we finish with the Query Form. 
    
    
    procedure TFormTryMySQL.OpenQueryButtonClick(Sender: TObject);
    begin
      ShowQueryForm := TShowQueryForm.Create(nil);
      try
        ShowQueryForm.DataSource1.DataSet := SQLQuery1;
        // We will write in the database, so let's set ReadOnly to false, 
        // and for that we need to set ParseSQL true
        SQLQuery1.ParseSQL:=true;
        SQLQuery1.ReadOnly:=false;
        SQLQuery1.SQL.Text := CommandEdit.Text;
        SQLQuery1.Open;
        ShowQueryForm.ShowModal;
      finally
        // set up update mode, and update database
        SQLQuery1.UpdateMode:=upWhereChanged;
        SQLQuery1.ApplyUpdates;
        SQLTransaction1.Commit;
        ShowQueryForm.Free;
        SQLQuery1.Close;
        // set read-only and parsesql back to default
        SQLQuery1.ParseSQL:=false;
        SQLQuery1.ReadOnly:=true;
      end;
    end;
    

We can now add records to the Database. If we add a TDBNavigator to the ShowQuery Form we can move around the Data Grid more easily, and edit records; the database gets updated with our changes each time we close the ShowQuery Form, and we can test this by opening it up again to inspect the database. 

If you want to be able to DELETE or otherwise modify records on the original database (ie make sure changes you make to the local dataset get committed back to the database) then you need to have one column in your database table that is a primary key autoincremented, as the 'delete' method requires to be able to generate a 'where' clause when writing instructions back to the database, to identify the records selected for deletion. So use the following code in your MySQL client (it can all be typed on one line, but line breaks have been added for clarity): 
    
    
    ALTER TABLE TRESTRIG 
    ADD COLUMN AUTOID INT 
    PRIMARY KEY AUTO_INCREMENT;
    

and then you will find that the Delete button on the navigator works. 

## Sources

The sources for this project can be downloaded [here](<http://prdownloads.sourceforge.net/lazarus-ccr/mysql_demo_20050408.tar.gz?download>) For more demo projects see [sourceforge](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=148359>)

## See also

  * [mysql](<mysql.md> "mysql")

---

_Source: [https://wiki.freepascal.org/MySQLDatabases](https://web.archive.org/web/20240906071746/https://wiki.freepascal.org/MySQLDatabases)_
