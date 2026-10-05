# MasterDetail

## Contents

  * 1 Overview
  * 2 SQLDB implementation
    * 2.1 Adding detail records with the right foreign key
    * 2.2 Using detail parameters that do not filter using master data
    * 2.3 Detail bookmarks
  * 3 **Countries and cities example**
  * 4 Delete all Detail records if Master record is deleted
  * 5 One more example
  * 6 See also



## Overview

In relational databases the master-detail concept is also called one-to-many or 1:N. It means that one record in a table/dataset can be related to many records in another table/dataset. It is a model that uses data from one dataset to filter data in another dataset. The dataset that is used to filter the data is called Master and the filtered dataset is called Detail. 

For example, countries have many cities. If you select a country and want to see all its cities, that is a good job for Master/Detail. The countries table could be master, and cities table could be detail. 

The filtering works because the detail table contains an identifier from the master table, usually the master table's primary key. So this key exists in both the master and detail table. In the detail table it is called a foreign key. 

Classical examples of master/detail relations are: 

  * customers with orders
  * orders with ordered articles



In our example below we'll work with a customer table and an order table. 

## SQLDB implementation

You can use the **datasource** property in the detail dataset to link to the master dataset. 

The approach below applies to both Lazarus and FPC code that uses the built in [SQLdb Package](<SQLdb_Package.md> "SQLdb Package") database layer. It may also apply to other database layers (e.g. Zeos); please check with their documentation. 

Example from the EMPLOYEE database (as used in [SQLdb Tutorial0](<SQLdb_Tutorial0.md> "SQLdb Tutorial0") and other tutorials): 

  * a CUSTOMER table with an integer primary key CUST_NO and other fields
  * a SALES table with a CUST_NO integer field that is a foreign key linking to the CUST_NO field in the CUSTOMER table



On the form: 

  * use one connection, 1 transaction, but 2 queries, 2 data sources
  * a master query called qryCustomers selects from the CUSTOMER table
  * a detail query called qrySales that selects from the SALES table



In the detail query qrySales: 

  * set the database property as usual
  * set the datasource property to point to the master datasource
  * in the query SQL, use a WHERE query to limit the select; use the detail fieldname and limit it to a parameter with the name of the field in the master table


    
    
    SELECT * from SALES WHERE SALES.CUST_NO=:CUST_NO
    

In this case we use the SALES.CUST_NO field that happens to have the same field name as the master field CUST_NO, but that need not be the case. As the master datasource keeps track of the current record for the master query qryCustomers, FPC can see the CUST_NO parameter as a reference to the current value of the CUST_NO field in the master/qryCustomers query. 

If you want to use additional parameters in your query that should _not_ be linked to the master query, make sure their `Bound` property is true before opening the master dataset. See [doc:fcl/db/tparam.bound.html](<http://lazarus-ccr.sourceforge.net/docs/fcl/db/tparam.bound.html> "doc:fcl/db/tparam.bound.html")

Make sure that the master query is open before the detail query so it can look up fields. 

### Adding detail records with the right foreign key

Although detail records scroll with master records, additional code is needed. 

When adding new detail records, the SALES.CUST_NO field is still NULL unless we fill it. So we need to set up an AfterInsert event handler for qrySales: 
    
    
    procedure TForm1.qrySalesAfterInsert(DataSet: TDataSet);
    begin
      DataSet.FieldByName('CUST_NO').AsInteger := qryCustomers.FieldByName('CUST_NO').AsInteger;
    end;
    

### Using detail parameters that do not filter using master data

By default, the values of all parameters in the detail dataset will be provided by the master dataset. Suppose you have another parameter in the detail dataset that you want to filter on independently: 

Master Query: Same as above 

Detail query: 
    
    
    SELECT * FROM SALES WHERE SALES.CUST_NO=:CUST_NO AND OVERDUE=:CREDITOVERDUE
    

The solution is to tell SQLDB that the CREDITOVERDUE parameter should be "bound", using one of these methods: 

  1. Set a value for the parameter in the object inspector.
  2. Mark the parameter as bound (to a value) before opening the master dataset: `qryDetail.Params.ParamByName('CREDITOVERDUE').Bound:=True;` See [Bound property documentation](<http://lazarus-ccr.sourceforge.net/docs/fcl/db/tparam.bound.html> "doc:fcl/db/tparam.bound.html")
  3. Set the parameter value before opening the master dataset:



`qryDetail.Params.ParamByName('CREDITOVERDUE').AsBoolean:=true;`

### Detail bookmarks

Note: in TBufDataset and bufdataset descendents (such as SQLQuery), detail datasets are reloaded whenever the master dataset active record moves; therefore existing detail bookmarks, even if valid, will no longer point to the same detail record after moving. 

  


* * *

## **Countries and cities example**

This tutorial was made using SQLite but the principles are the same for other databases also. 

In case you have no admin program for SQLite, I suggest SQLite Studio. <http://sqlitestudio.pl>

  * First create a new SQLite database and save it as database.db3.



(If you use another database system, just create your tables as you are used to and use the appropriate components for the example). 

  
**Now lets make two tables.**

The "countries" table: 
    
    
    COUNTRY_ID (Primary key, integer, autoincrement)
    COUNTRY_NAME (VARCHAR(50), not null)
    

**Add two countries to the table:**

  * Sweden
  * Norway



Because COUNTRY_ID is autoincrement, each country is assigned an ID automatically. 

**Then make the "cities" table:**
    
    
    CITY_ID (Primary key, integer, autoincrement)
    CITY_NAME (VARCHAR(50), not null)
    COUNTRY_ID (integer, not null) (foreign key)
    

**Add three cities to this table:**

  * Stockholm with COUNTRY_ID 1
  * Gothenburg with COUNTRY_ID 1
  * Oslo with COUNTRY_ID 2



Since Sweden was added first in the empty table, it will have COUNTRY_ID 1 and Norway will have with COUNTRY_ID 2. For every city you add, you must supply its COUNTRY_ID because thats what links the tables together and make the master/detail concept possible. 

  


  * Create a new project in Lazarus and save it as MasterDetail
  * Drop a TSQLite3Connection on the form. (SQLdb tab)
  * In the DatabaseName property for TSQLite3Connection enter full path and filename for the SQLite Database file you just made.
  * Drop a TSQLTransaction on the form. (SQLdb tab)
  * Set its Database property to: SQLite3Connection1
  * Return to TSQLite3Connection and set its Transaction property to SQLTransaction1
  * Set TSQLite3Connection.Connected to true



**You should now have a connection to the database and can proceed to the next step.**

  * Drop a TSQLQuery on the form. (SQLdb tab)
  * Set its Database property to: SQLite3Connection1
  * Set its Transaction property to: SQLTransaction1
  * In the SQL statement, enter:


    
    
    select * from countries
    

  * Drop a TDataSource on the form. (Data Access tab)
  * Set its DataSet to: SQLQuery1


  * Drop a TDBGrid on the form (Data Controls tab)
  * Set its DataSource property to: DataSource1


  * Go to SQLQuery1 and set Active to true.



**The content of the countries table (master) should now be visible in DBGrid1.**

  * Now proceed with the cities table (detail)
  * Drop a second TSQLQuery on the form. (SQLdb tab)
  * Set its Database property to: SQLite3Connection1
  * Set its Transaction property to: SQLTransaction1
  * In the SQL statement, enter:


    
    
    select *
    from cities
    where cities.COUNTRY_ID = :COUNTRY_ID
    

The colon sign for :COUNTRY_ID just means that this is a variable parameter and that the value comes from somewhere else. It takes the value from the Master DataSource. So in our example the :COUNTRY_ID value is supplied by DataSource1, which is our Master DataSource. (The parameter name is not arbitrary: it is the relevant field name in the Master Table) 

  * Set its Datasource property to Datasource1.



This does not yet allow us to add cities; to do this: 

  * Please use the information under #Adding detail records with the right foreign key above.



  
**Continue with the Detail view:**

  * Drop a second TDataSource on the form.
  * Set its DataSet property to Query2 (detail query)
  * Drop a second DBGrid on the form (Detail grid)
  * Set its DataSource to DataSource2 (Detail DataSource)



  
**Now get the data flowing into the grids. Set all these to active/connected:**

  * DBConnection
  * Transaction
  * Query1
  * Query2



  
Now there should be detail data in the DBGrid2 

  
A thing to remember is that (as above) the Detail Query's DataSource must be set to the Master DataSource. (DataSource1 in our example) 

  


**If you still didn't understand, look at this wiring scheme:**

Master Query.DataSource := None 

Master DataSource.Dataset := Master Query 

Master DBGrid.DataSource := Master DataSource 

Detail Query.DataSource := Master DataSource 

Detail DataSource.Dataset := Detail Query 

Detail DBGrid.DataSource := Detail DataSource 

## Delete all Detail records if Master record is deleted

This is easy to do with Cascading Deletes. Once again, this is for SQLite, but other systems work similarly. 

  * First turn on foreign keys for SQLite because it's off by default. Do this by adding FOREIGN_KEYS=ON to the Params property of TSQLite3Connection.


  * Then make sure the detail table is created so the foreign key (country_id) references the master table. In this case it is the cities table:


    
    
      try
        { Master table }
        Conn.ExecuteDirect('CREATE TABLE IF NOT EXISTS countries (' +
                            ' country_id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, ' +
                            ' country_name VARCHAR(30) ' +
                            ')');
        { Detail table }
        Conn.ExecuteDirect('CREATE TABLE IF NOT EXISTS cities (' +
                            ' city_id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, ' +
                            ' city_name VARCHAR(30), ' +
                            ' country_id INTEGER REFERENCES countries ON DELETE CASCADE' +
                            ')');
        { Sample data }
        Conn.ExecuteDirect('INSERT INTO countries(country_name) VALUES(''Sweden'')');
        Conn.ExecuteDirect('INSERT INTO cities(city_name, country_id) VALUES(''Stockholm'', 1)');
        Conn.ExecuteDirect('INSERT INTO cities(city_name, country_id) VALUES(''Malmo'', 1)');
    
        Conn.ExecuteDirect('INSERT INTO countries(country_name) VALUES(''Norway'')');
        Conn.ExecuteDirect('INSERT INTO cities(city_name, country_id) VALUES(''Oslo'', 2)');
        Conn.ExecuteDirect('INSERT INTO cities(city_name, country_id) VALUES(''Bergen'', 2)');
    
        Conn.Transaction.Commit;
        Conn.Close;
      except
        on E:Exception do
        begin
          TX.Rollback;
          ShowMessage( E.Message );
        end;
      end;
    

Thats it. From now on, all detail records are deleted automatically whenever the master record is deleted. 

  
Note: DON'T try to turn on foreign keys this way: 
    
    
    Conn.ExecuteDirect('PRAGMA foreign_keys = ON');
    

TSQLite3Connection starts a transaction before the first query is sent to the database, even in case of a ExecuteDirect. It is not possible to enable or disable foreign key constraints in the middle of a multi-statement transaction (when SQLite is not in autocommit mode). Attempting to do so does not return an error; it simply has no effect. Source: Ludob/SQLite documentation. 

## One more example

There is a complete running example with source code on this forum topic: 

How to Append a new line and save it in DBGrid? 

<http://forum.lazarus.freepascal.org/index.php/topic,42088.msg293305.html#msg293305>

## See also

  * <http://www.freepascal.org/docs-html/fcl/sqldb/usingparams.html> Documentation on using parameters, including implementation for master/detail
  * <http://docwiki.embarcadero.com/RADStudio/XE3/en/Establishing_Master-detail_Relationships_Using_Parameters> The documentation for the equivalent Delphi feature
  * <http://forum.codecall.net/topic/62683-master-detail-datasets/>
  * [Lazarus Tdbf Tutorial#Master table relations](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") Example of using master/detail with the TDBF components
  * [SQLdb Tutorial0](<SQLdb_Tutorial0.md> "SQLdb Tutorial0") \- Set up instructions for sample data for the subsequent database tutorials. Has master/detail tables

---

_Source: [https://wiki.freepascal.org/MasterDetail](https://web.archive.org/web/20241004013408/https://wiki.freepascal.org/MasterDetail)_
