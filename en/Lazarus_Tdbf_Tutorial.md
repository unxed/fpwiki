# Lazarus Tdbf Tutorial

│ **[Deutsch (de)](</Lazarus_Tdbf_Tutorial/de> "Lazarus Tdbf Tutorial/de")** │  **English (en)** │  **[español (es)](</Lazarus_Tdbf_Tutorial/es> "Lazarus Tdbf Tutorial/es")** │  **[français (fr)](</Lazarus_Tdbf_Tutorial/fr> "Lazarus Tdbf Tutorial/fr")** │  **[português (pt)](</Lazarus_Tdbf_Tutorial/pt> "Lazarus Tdbf Tutorial/pt")** │  **[русский (ru)](<../ru/Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial/ru")** │  **[中文（中国大陆） (zh_CN)](</Lazarus_Tdbf_Tutorial/zh_CN> "Lazarus Tdbf Tutorial/zh CN")** │    
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


    [Advantage](<Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<mssqlconn.md> "mssqlconn") \- [Postgres](<postgres.md> "postgres") \- [Interbase](<Firebird.md> "Firebird") \- [Firebird](<Firebird.md> "Firebird") \- [Oracle](<Oracle.md> "Oracle") \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- dBASE \- [MS Access](<MS_Access.md> "MS Access") \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
## Contents

  * 1 Overview
    * 1.1 Documentation
    * 1.2 Alternatives and known problems
      * 1.2.1 Limitations
    * 1.3 No need for installation
    * 1.4 What the TDbf package provides
    * 1.5 What to put in your uses section
  * 2 How to create a new database table
    * 2.1 Setting the path
    * 2.2 Choosing a TableLevel
    * 2.3 Adding fields
    * 2.4 Adding fields at designtime
    * 2.5 Creating the table
  * 3 How to add indexes to a table
  * 4 Expressions
    * 4.1 Index/filter on strings
    * 4.2 Index 100 character string limit
    * 4.3 Index/filter on TDateTimes
    * 4.4 Index/filter operators/functions
  * 5 Code example: creating a table/DBF file
  * 6 External Index Files
  * 7 How to link the TDbf component to data-aware components
  * 8 Master table relations
    * 8.1 Searching and Displaying a data set by using filters
  * 9 Filters
    * 9.1 Expression filters
    * 9.2 OnFilterRecords
      * 9.2.1 Filter on null values
  * 10 Packing and rebuilding the tables
  * 11 Encryption
    * 11.1 Operating system level encryption
    * 11.2 Encrypt the DBF file
    * 11.3 Encrypt field data
  * 12 Code pages and string encoding issues
  * 13 Third party software
  * 14 See also



## Overview

This tutorial is about basic database development using the original [TDbf](<http://tdbf.sf.net>) component (by Micha Nelissen) with Lazarus which is included in the standard FreePascal distribution. 

In Lazarus, the DbfLaz component makes use of the FPC TDbf code. 

### Documentation

For the original TDbf documentation PDF go to the [SourceForge](<https://sourceforge.net/project/showfiles.php?group_id=34085&package_id=26371>) site. It may be useful to keep that pdf alongside this document while reading. 

This wiki article serves as additional documentation for new features since the original documentation. 

### Alternatives and known problems

Alternatives for embedded database use include Firebird embedded (advantage: easily scalable to client/server), SQLite (advantage: easy deployment), or ZMSQL (using CSV files; advantage: only Pascal code required). 

Current issues: please see the bugs in the bug tracker. 

#### Limitations

  * TDBF sets database encoding based on OS language settings (you can choose your own by setting LanguageID; see documentation). Moving a DB to another computer that does not support the encoding may lead to a read-only database. _Note: investigate whether this applies to Tablelevel 7 (Visual DBase) and 25/30 (Foxpro/Visual Foxpro)_
  * None of the DBF formats used support Unicode encoding (whether UTF-8, UTF-16 or UTF-32). There may be a workaround using binary fields, but then you lose sorting abilities etc.
  * Indexing a DBase file with [ixCaseInsensitive] currently does not work.
  * No support yet for referential integrity in file formats that support it (DBase VII, Visual FoxPro).
  * No support (now or ever) for internally encrypted .dbf files: the DbaseIV encryption mechanism is quite weak anyway. Please use one of the encryption methods mentioned below.



FPC 2.6.x and lower: 

  * On ARM processors, the TDBF code is known not to work due to bus alignment issues. This is being addressed in the FPC development/trunk version.
  * Choosing the FoxPro tablelevel (25) will produce files that cannot be read by Visual Foxpro drivers. This has been fixed in the FPC development/trunk version.



### No need for installation

The DbfLaz package is installed by default in both FPC and Lazarus. The DBFLaz package uses the TDbf and associated units in the Free Pascal Free Component Library. In other words, no need to install anything if you have a fairly recent version of Lazarus. 

### What the TDbf package provides

The TDbf package provides access to dBase and FoxPro database tables for Lazarus (and others). It allows for reading, writing and creation of dBase III+, dBase IV, Visual dBase VII and FoxPro tables. It does all of this without the need for additional libraries or database engines. Simply drop the TDbf on your form and you have instant access to a cross-platform database environment. The TDbf works in both Windows and Linux using Lazarus. 

### What to put in your uses section

If you use the visual DBF components in Lazarus, Lazarus will put Dbf in your uses clauses. If you use FPC (console mode) only, you can put it there yourself. Handy other units are db for DataSet support and Dbf_Common for things such as field type definitions. 

So you'd get: 
    
    
    uses Dbf, db, Dbf_Common;
    

## How to create a new database table

There is a "Database Desktop" application for Lazarus (open the project in your Lazarus directory/tools/LazDataDesktop). You could create a new data dictionary, then create code to set up DBF files based on the dictionary. As of now (July 2011), this process is still a bit convoluted. 

Another option is to use OpenOffice or LibreOffice Calc and save your spreadsheet as a .dbf file. 

You may also (on windows - works with wine too) try this application: <http://www.dirfile.com/cdbf_explorer.htm>

In any event, we shall illustrate creating a new database in code below. 

### Setting the path

It's a good idea to give your application's database its own directory. This simplifies making backups of the data. 

There are two ways to set the path: 

  * you can set the full path using the FilePathFull property
  * you can set a path relative to the current application path with FilePath.



For instance, setting "FilePath" at runtime to "data/" would use a data subdirectory just below the executable file. Setting the "FilePathFull" property to "/var/data/" would place all dbfs, index files etc in that exact folder, whatever the application location is. 

An example: 
    
    
    MyDbf.FilePathFull := '/location/to/my/data'; //Directory where all .dbf files will be stored
    MyDbf.TableName := 'customers.dbf'; // note: is the .dbf really required?
    

### Choosing a TableLevel

By default, the TDbf components will create dBase IV tables. While this is very compatible, there are features you may wish to use that are not supported. E.g. to support auto-incrementing fields, you must use something newer. The table types are: 

**Table level** | ***Base product compatibility** | **Auto-increment field support** | **Field length limit** | **Remarks**  
---|---|---|---|---  
Level 3 | dBase III+ | no | 10 characters [[1]](<http://www.clicketyclick.dk/databases/xbase/format/dbf.html#DBF_STRUCT>) | Quite obsolete but very simple format. Handy for in-memory usage.   
Level 4 | dBase IV | no | 10 characters [[2]](<http://www.clicketyclick.dk/databases/xbase/format/dbf.html#DBF_STRUCT>) | Used to be very popular. Basis for GIS shape files (.shp)   
Level 7 | Visual dBase VII | yes | 32 characters [[3]](<http://www.dbase.com/KnowledgeBase/int/db7_file_fmt.htm>) |   
Level 25 | FoxPro | yes | 10 characters [[4]](<http://www.dynamic.com/v7/what-are-limitations-foxpro-full-isam-file>) [[5]](<http://www.foxite.com/archives/foxpro-field-names-0000066805.htm>) | FoxPro; compatible with FoxPro for DOS and Visual FoxPro. Note: implementation in FPC 2.6.2 produces files that cannot be read by Visual FoxPro drivers!   
Level 30 | Visual FoxPro | yes | Normally 10 characters [[6]](<http://www.dynamic.com/v7/what-are-limitations-foxpro-full-isam-file>) [[7]](<http://www.foxite.com/archives/foxpro-field-names-0000066805.htm>) Not supported in FPC: when using a DBC (Database container) file: 27 characters  | Not yet in FPC 2.6.2; only in current development FPC version   
  
See bug [[8]](<http://bugs.freepascal.org/view.php?id=20134>) for more details on field length limitations. 

You choose a table type by setting the TableLevel property appropriately: 
    
    
    MyDbf.TableLevel := 7; //Visual dBase 7
    

### Adding fields

Creating fields for your new table at runtime pretty much follows the old Delphi standard. An example: 
    
    
    With MyDbf.FieldDefs do begin
      Add('Id', ftAutoInc, 0, True); //Autoincrement field called Id
      Add('Name', ftString, 80, True); //80 character string field called Name
    End;
    

**Note** the parameter 'Required' is not implemented therefore regardless of how it is set it will be 'False'. 

List of field types - bold types are currently supported by the DBF code. See [Database_field_type](<Database_field_type.md> "Database field type") for a explanation of field types: 

  * ftUnknown
  * **ftString**
  * **ftSmallInt**
  * **ftInteger**
  * **ftWord**
  * **ftBoolean**
  * **ftFloat**
  * **ftCurrency** (only supported in TableLevel 25)
  * **ftBCD** (only supported in TableLevel 25)
  * **ftDate**
  * ftTime
  * **ftDateTime** : probably the recommended one for storing ftDate or ftDateTime values. At least on FoxPro, seems to store milliseconds as well.
  * **ftBytes** (only supported in TableLevel 25)
  * ftVarBytes
  * **ftAutoInc** (only supported in TableLevel 7 or 30)
  * **ftBlob**
  * **ftMemo**
  * ftGraphic
  * ftFmtMemo
  * ftParadoxOle
  * **ftDBaseOle**
  * ftTypedBinary
  * ftCursor
  * **ftFixedChar**
  * **ftWideString**
  * **ftLargeInt**
  * ftADT
  * ftArray
  * ftReference
  * ftDataSet
  * ftOraBlob
  * ftOraClob
  * ftVariant
  * ftInterface
  * ftIDispatch
  * ftGuid
  * ftTimeStamp
  * ftFMTBcd



### Adding fields at designtime

Use property `FieldDefs` to create fields at designtime - this replaces above call to `FieldDefs.Add`. However, it is important to note that the property `StoreDefs` must be set to true in order to store the definitions in the lfm file and to have them available when the table is created later at runtime. 

### Creating the table

Once you have defined the fields you wish to use in your new table, you can go ahead and create it with: 
    
    
    MyDbf.CreateTable;
    

## How to add indexes to a table

If your database is larger than a few records, chances are you will want to have indexes defined to make searching faster. To change the index structure of a table, we need to have exclusive access to the table - which we would have while creating it anyway. 
    
    
    MyDbf.Exclusive := True;
    MyDbf.Open;
    

Now, we just have to add the index - or two, as in this example: 
    
    
    MyDbf.AddIndex('custid', 'Id', [ixPrimary, ixUnique]);
    MyDbf.AddIndex('custname','Name', [ixCaseInsensitive]);
    MyDbf.Close;
    

## Expressions

Both indexes and filters can use expressions according to the [TDbf documentation](<http://sourceforge.net/projects/tdbf/files/TDbf%20documentation/TDbf%20documentation%201.1/>). 

For more details on filters, see below. 

### Index/filter on strings

Strings needs to be surrounded/delimited by double quotes (") or single quotes ('). 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** In FPC versions lower than 3.0, using the delimited quotes in your filter string does not work (see the example below). See FPC 3.0 New Features page: [FPC_New_Features_3.0#TDBF.2C_bufdataset_.28and_descendents_such_as_TSQLQuery.29_allow_escaped_delimiters_in_string_expression_filter](<FPC_New_Features_3.md> "FPC New Features 3.0").

  

    
    
    // We have a field called CUSTOMER
    // let's delimit with double quotes
    // (of course, double up single ' in string to make FPC happy)
    MyDBF.Filter:='CUSTOMER = "Felipe''s Bank" ';
    // another tricky one, let's delimit with single quotes. Of course,
    // we'll need to double these up for FPC to interpret these correctly
    MyDBF.Filter:='CUSTOMER = ''Jim "Mighty" McLanahan'' ';
    
    // If we have both a ' and a " in our string, we're in trouble.
    // the example below only works on FPC 2.7.1+
    MyDBF.Filter:='CUSTOMER = "Felipe''s ""Mighty"" Bank" ';
    
    MyDBF.Filtered:=true;
    

### Index 100 character string limit

Index expressions that return a string may be maximum 100 characters long; use SUBSTR to cut off extra characters, e.g.: 
    
    
    MyDbf.AddIndex('SOMEFANCYINDEX', 
      'DTOS(DATEFIELD)+SUBSTR(LONGFIELD,1,10)+SUBSTR(LONGFIELD2,1,20)', []);
    

### Index/filter on TDateTimes

To set an index on a date/time field, use an expression index and the DTOS function: 
    
    
    MyDbf.AddIndex('PUBLISHDATEIDX', 'DTOS(PUBLISHDATE)', []);
    

Filtering is similar - note: apparently you need to surround the string value you are comparing against with double quotes. See below for details on the DTOS function; basically you need to format the date as YYYYMMDD: 
    
    
    // we have a field named MODIFIED here:
    MyDBF.Filter:='DTOS(MODIFIED) > "20051231"';
    
    // if you want to compare datetime, that can be done:
    // everything later than 2 January 2006, 4:34PM
    MyDBF.Filter:='DTOS(MODIFIED) > "20060102 163400"';
    
    MyDBF.Filtered:=true;
    

For example, if you want to filter a tdbf on a date field using a value selected from a DBlookupComboBox 
    
    
    var 
    mydate: String;
    begin
      mydate := formatdatetime('yyyymmdd', strtodate(DBLookupComboBox.Text));
      Filter := 'DTOS(DATE) = ' + AnsiQuotedSTR(mydate,'"'); 
    end;
    

### Index/filter operators/functions

Supported operators (from the documentation mentioned above): 

**Operator/function** | **Description**  
---|---  
+ | Concatenate two strings, or add two numbers   
* | Multiply two numbers   
/ | Divide two numbers   
= | Compare two strings, numbers or booleans for equality   
<> | Compare two strings, numbers or booleans for inequality   
< | True if the first argument is smaller than second argument   
<= | True if the first argument is smaller than or equal to the second argument   
> | True if the first argument is greater than the second argument   
>= | True if the first argument is greater than or equal to the second argument   
NOT | Negate the boolean argument to the right   
AND | Returns true if both the first and second boolean argument are true   
OR | Returs true if the first or second boolean argument is true   
STR(num[,size,precision]) | Converts a number num to a string with optional size and precision num: number to convert to string size: the total number of characters to output precision: the number of digits to the right of the decimal point   
DTOS(date) | Converts date to a string with representation YYYYMMDD   
SUBSTR(str,index,count) | Extracts a substring from string str from position index and length count   
UPPER(str) | Returns the uppercase equivalent of string str   
LOWER(str) | Returns the lowercase equivalent of string str   
  
## Code example: creating a table/DBF file

The following sample creates a new table "customers" in code. This of course only needs done once, and after that you just open the table, instad of creating it. ;-) 
    
    
    {$MODE OBJFPC}{$H+}
    Program DatabaseTest;
    { We need the following units be in the USES clause: }
    uses Dbf, db, Dbf_Common, SysUtils;  
                                     
    { The Dbf is put there e.g. when you drop a TDbf component on a form...   }
    { but you will need db for the DataSet object and Dbf_Common              }
    { for things such as the field type definitions                           }
    { Finally, use SysUtils for ForceDirectories.                             }
    var
      MyDbf: TDbf;
    begin
      MyDbf := TDbf.Create(nil);
      try
        { make sure the data directory exists - otherwise this may fail (e.g. on Windows XP): }
        ForceDirectories('data');
        { use relative path to "data" directory }
        MyDbf.FilePath := 'data' + DirectorySeparator; 
        { we want to use Visual dBase VII compatible tables }
        MyDbf.TableLevel := 7;
        MyDbf.Exclusive := True;
        MyDbf.TableName := 'customers.dbf';
        With MyDbf.FieldDefs do begin
          Add('Id', ftAutoInc, 0, True);
          Add('Name', ftString, 80, True);
        End;
        MyDbf.CreateTable;
        MyDbf.Open;
        MyDbf.AddIndex('custid', 'Id', [ixPrimary, ixUnique]);
        { add a secondary index }
        MyDbf.AddIndex('custname','Name', [ixCaseInsensitive]);
        MyDbf.Close;
      finally
        MyDbf.Free;
      end;
    end.
    

## External Index Files

The TDbf component also supports storing secondary indexes in a separate file. This might be helpful if the database is expected to be very large. Secondary index files are created almost identically to normal indexes, but with the addition of the '.ndx' file extension: 
    
    
    MyDbf.AddIndex('custname.ndx','Name', [ixCaseInsensitive]);
    

  
Each time the TDbf is opened, the index file must be loaded: 
    
    
    MyDbf.OpenIndexFile('custname.ndx');
    

  
And indexes must be referenced including the extension: 
    
    
    MyDbf.IndexName := 'custname.ndx';
    

  
Index files can be packed (i.e. entries pointing to deleted data rows are removed) separately using: 
    
    
    MyDbf.CompactIndexFile('custname.ndx');
    

## How to link the TDbf component to data-aware components

The above examples show how to create a new database table in code. Using that table is even more simple. 

Data aware components in Lazarus (such as the TDbEdit control) link to a TDataSource component using their "DataSource" and "DataField" properties. The TDataSource component handles communication between the database engine and the data aware components. A TDataSource then links to the TDbf component using it's "DataSet" property. The connection looks like this: 
    
    
    TDbEdit-------
                 |
    TDbEdit------|-->TDataSource-->TDbf
                 |
    TDbNavigator--
    

  
Be sure to set the FilePath (or FilePathFulll), TableLevel, and TableName properties of your TDbf component before calling 
    
    
    TDbf.Active := True;
    

  


## Master table relations

Real power in database programming begins when you have multiple tables that reference each other. While TDbf does not yet support full referential integrity (_so what is missing?_), it does support a master / detail relationship between TDbf's. 

When there are two tables related, for instance: 
    
    
    [customers]
    Id       <----|
    Name          |
    Phone         |
    Address       |
                  |  The CustID in invoices references a customer primary  field
    [invoices]    |
    Id            |
    Amount        |
    CustID   -----|  * This field indexed as "idxcustid"
    

  
If you wanted to display all invoices for a given customer, the detail table (invoices) can stay in sync with the master table (customers) automatically. 

On the invoices TDbf component set the following: 
    
    
    InvDbf.IndexName := 'idxcustid'; // our field that will match the customers table ID
    InvDbf.MasterSource := dsCustomers; // datasource that is linked to the customers TDbf
    InvDbf.MasterFields := 'Id'; // field on the customers table we are matching against our index
    

### Searching and Displaying a data set by using filters

[Simon Batty](</index.php?title=User:Svbfieldview.net&action=edit&redlink=1> "User:Svbfieldview.net \(page does not exist\)")

In this example I wanted to search a database of books for all the titles an author has listed and then display the list in a memo box 
    
    
    Dbf1.FilePathFull := '/home/somelocatio/database_location/'; // path to the database directory
    Dbf1.TableName := 'books.dbase';                             // database file (including extension)
    DbF1.Open;
    memo1.Clear;                                                 // clear the memo box
     
    Dbf1.FilterOptions := [foCaseInsensitive];                   // set FilterOptions before Filter !
    Dbf1.Filter := 'AUTHOR=' + QuotedStr('anauthor');            // AUTHOR is the field name containing the authors
                                                                 // Note that we need to surround the value with double quotes: "James Joyce"
     
    Dbf1.Filtered := true;       // This selects the filtered set
    Dbf1.First;                  // moves the the first filtered data
    while not dbf1.EOF do        // prints the titles that match the author to the memo box
    begin
      memo1.Append(Dbf1.FieldByName('TITLE').AsString);          // TITLE is the field name for titles
      dbf1.next;                                                 // use .next here NOT .findnext!
    end;
    Dbf1.Close;
    

Note that you can use Ddf1.FindFirst to get the first record in the filtered set, then use Dbf1.Next to move though the data. I found that using Dbf1.FindNext just causes the program to hang. 

## Filters

There are two ways to use filters: 

  * use filter expressions
  * write an event handler for OnFilterRecord; if you want to show the record, set Accept to true.



In both cases, set the Filtered property to true to enable filtering. 

Example: imagine you have a database that identifies the products by a code, for example, B019, and you need to filter only those products that are of this code so that a report is generated with LazReport. Let's assume your DBF object is called TDBF1 and the field with product codes is called "product_no". 

### Expression filters

Implement filtering using a filter expression like this: 
    
    
    TDBF1.Filter := 'product_code = "B019"'; //string type field; string constants should be quoted by ' or "
    TDBF1.Filtered := true;
    
    
    
    TDBF1.Filter := 'product_code = "B*"'; //partial match filter; filters all products which code begins with B
    TDBF1.Filtered := true;
    

Filter on multiple fields: 
    
    
    // Needs to be tested. If it works, please remove this line.
    MyDBF.Filter:='LASTNAME;FIRSTNAME';
    

Adapted from: forum post [[9]](<http://forum.lazarus.freepascal.org/index.php/topic,23090.msg137401.html#msg137401>)

Here is an example of using a DBLookupCombobox value in a filter statement (where NAME is the field from the DBf): 
    
    
    Filter := 'NAME=' + QuotedStr(DBLookupcomboBox1.Text);
    

Here are two examples of using two comboboxes in a compound (i.e. 2 fields) filter statement (where NAME and TYPE are field names from a DBf): 

In this example you are going to "and" the two values. 
    
    
    // unfortunately, this will fail whenever the user types a ' in TypeComboBox.Text...
    Filter := 'NAME=' + QuotedStr(DBLookupcomboBox1.Text) + ' and TYPE=' + QuotedStr(TypeComboBox.Text);
    Filter := Format('NAME="%s" and TYPE="%s"',[DBLookupcomboBox1.Text, TypeComboBox.Text]);
    

An example of using a combobox to provide the field name for the filter and comparing that to the value in lookup combobox: 
    
    
    Filter := FieldComboBox.Text + '=' + QuotedSTR(DBLookupcomboBox1.Text);
    

In this example, the FieldCombobox is programmatically populated with the names of the fields from the DBf: 
    
    
    // fill the combobox with Field Names
    for i:=0 to Dbf.FieldCount-1 do
      FieldComboBox.Items.Add(Dbf.Fields[i].FieldName);
    

Please see the section #Expressions above for the list of supported expressions as well as more examples. 

### OnFilterRecords

To use this method, you need to write a function. 

In your main code, write something like: 
    
    
    OnFilterRecord := @TestOnFilterProc; //instruct tdbf to lead all records past your OnFilterProductCode procedure
    Filtered := True; //actually enable filtering
    

Then write your event handler: 
    
    
    procedure TTestCursorDBBasics.TestOnFilterProc(DataSet: TDataSet; var Accept: Boolean);
    begin
      // Filter out everything except product_no=B019
      Accept := Copy (Trim (Dataset.FieldByName ('product_no'). AsString),1,4) = 'B019';
    end;
    

All records that give a false value for Accept are filtered out i.e. are not displayed in the report. 

#### Filter on null values

You can use this method to show only empty (null) values, something you cannot do with the `Filter` property. An example - assuming you have a field called orderdate: 
    
    
    procedure TTestCursorDBBasics.TestOnFilterProc(DataSet: TDataSet; var Accept: Boolean);
    begin
      // Show only empty order dates
      Accept := Dataset.FieldByName('orderdate').IsNull;
    end;
    

## Packing and rebuilding the tables

When a record is deleted, it's not truly removed from the physical table. Periodically you must "pack" a table to recover that lost space. This should be done with exclusive mode set. See above for remarks on packing external index files. 
    
    
    MyDbf.Exclusive := True;
    MyDbf.Open;
    MyDbf.PackTable;
    // let's also rebuild all the indexes
    MyDbf.RegenerateIndexes;
    MyDbf.Close;
    MyDbf.Exclusive := False;
    

  


## Encryption

Apparently at least DBase IV and 5 support some kind of encryption (there is an encryption flag in the file structure), but it may well be that the password/encryption key is in the DBF itself (see [[10]](<http://www.antionline.com/archive/index.php/t-236215.html>)), making it insecure. 

There are other options if you want to encrypt your data. See here [discussion on DBase encryption](<http://www.lazarus.freepascal.org/index.php/topic,15422.msg83016.htm%7Cforum>)

### Operating system level encryption

You can use an encrypted filesystem such as TrueCrypt (containers) to encrypt the directory where the database files get stored. 

### Encrypt the DBF file

You can use streams (e.g. the Blowfish encyprtion stream code supplied with Lazarus/FPC) to encrypt the data (all tables etc) by storing the unencrypted file e.g. in memory, then writing that to disk: 

  1. Open your file in a TFileStream.
  2. Decrypt TFileStream to a new stream, YourDecryptedStream
  3. Tdbf.UserStream:= YourDecryptedStream; //Note: this will not encrypt .dbt/memo files! Use **UserMemoStream** for this (in FPC development/trunk since May 2013 only)
  4. Tdbf.Storage:=stoMemory;
  5. Tdbf.Open;



When finished 

  1. Tdbf.Close
  2. Encrypt YourDecryptedStream to TFileStream
  3. Free stream(s)



Detailed code can be found in the forum thread mentioned above. People implementing this are welcome to adapt this article with working code. 

### Encrypt field data

You could also choose to encrypt field data by implementing the OnGetText and OnSetText events to encrypt/decrypt the field contents. 

## Code pages and string encoding issues

The dbf format is very old and thus can only handle strings with single-byte characters. Since Lazarus uses UTF-8 encoded strings, there must be a conversion between both formats, otherwise the dbf strings are not displayed correctly in the LCL controls. 

For this purpose the `TDbf` component fires an event `OnTranslate` for string fields. The event provides a boolean parameter `ToOEM` telling whether the conversion must go to a single-byte codepage or to UTF-8. Typical code is as follows: 
    
    
    uses
      LConvEncoding;
    
    function TForm1.DbfTranslate(Dbf: TDbf; Src, Dest: PChar; ToOEM: Boolean): Integer;
    var
      s: String;
      cp: String;
    begin
      cp := 'cp' + IntToStr(Dbf.CodePage);
      if ToOEM then
        s := ConvertEncoding(Src, 'utf8', cp)
      else
        s := ConvertEncoding(Src, cp, 'utf8');
      StrCopy(Dest, PChar(s));
      Result := StrLen(Dest);
    end;
    

After opening the dataset all string fields to be converted must be defined by setting their property `Transliterate` to `true`. 
    
    
    // Open dbf file
    Dbf1.Open;
    
    // Make sure that strings are converted to and back from UTF8.
    Dbf1.OnTranslate := @DbfTranslate;
    for i := 0 to Dbf1.Fields.Count-1 do
      if Dbf1.Fields[i] is TStringField then
        TStringField(Dbf1.Fields[i]).Transliterate := true;
    

When a dbf file is newly created its codepage can be set by using the `LanguageID` property. This is a combination of locale specific information (for sorting) and the code page. The unit `dbf_lang` contains a list of allowed values, e.g. 
    
    
    const
      FoxLangId_ENU_437       = $01; // DOS USA
      FoxLangId_Intl_850      = $02; // DOS multilingual
      FoxLangId_Windows_1252  = $03; // Windows ANSI
      FoxLangId_Mac_10000     = $04; // Standard Macintosh
      DbfLangId_DAN_865       = $08;
      DbfLangId_NLD_437       = $09;
      DbfLangId_NLD_850       = $0A;
      DbfLangId_FIN_437       = $0B; 
      // plus many more...
    

The constants beginning with "Fox" are for the FoxPro TableLevels, those beginning with "Dbf" are for the dBase TableLevels. 

When strings are written to their fields directly, bypassing the LCL, then the encoding must be converted explicitly. The `TDbf` property `CodePage` returns the used codepage as defined by the LanguageID: 
    
    
    uses
      LConvEncoding;
    ...
    Dbf1.Edit;
    Dbf1.FieldByName('some_string_field').AsString := ConvertEncoding('äöü', 'utf8', 'cp' + IntToStr(Dbf1.CodePage));
    Dbf1.Post;
    

## Third party software

  * [MyDBFStudio](<https://github.com/wp-xyz/MyDBFStudio>) is an open source admin tool to manage your dbase table. It is entirely written in Lazarus IDE and based on TDbf. (The original web site is accessible only via [Wayback Machine](<https://web.archive.org/web/20181018052343/http://mydbfstudio.altervista.org/>)).
  * [[11]](<https://vlad-karpov.narod.ru/Components.html#VK%20DBF>) are Delphi components for DBF files. 
    * Vlad Karpov's open source VKDBF: [[[12]](<https://sourceforge.net/projects/vkdbf/?source=directory>)]
  * [fpSpreadsheet](<FPSpreadsheet.md>) has a way to process dbf files and save them to Excel.



## See also

  * [TDbf](<TDbf.md> "TDbf")
  * [fpdbfexport](<fpdbfexport.md> "fpdbfexport") Exports any kind of dataset to DBF files
  * See the dbftool example included in FPC 2.7.1+: creating, using DBF files and exporting data using db export (also downloadable via <https://gitlab.com/freepascal.org/fpc/source/-/tree/main/packages/fcl-db/examples>)
  * [Contacts Database](<Contacts_Database.md> "Contacts Database") Another example database
  * [Example: TDbf (creating table and indexes, selecting of index)](<Example__TDbf_\(creating_table_and_indexes,_selecting_of_index\).md> "Example: TDbf \(creating table and indexes, selecting of index\)")

---

_Source: [https://wiki.freepascal.org/Lazarus_Tdbf_Tutorial](https://web.archive.org/web/20250113133520/https://wiki.freepascal.org/Lazarus_Tdbf_Tutorial)_
