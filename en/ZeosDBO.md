# ZeosDBO

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

  * 1 About
  * 2 Screenshot
  * 3 Download
    * 3.1 Released Version
    * 3.2 SVN Version
      * 3.2.1 Windows
      * 3.2.2 Linux
      * 3.2.3 Installing the SVN version in Lazarus
  * 4 Bug report/Feature request
  * 5 External links
  * 6 See also



## About

ZeosDBO is a component suite to connect in various types of database (MySQL, Firebird, etc). 

## Screenshot

A screenshot of zeos components in lazarus component palette: 

[![Zeos access.png](https://wiki.freepascal.org/images/2/29/Zeos_access.png)](</File:Zeos_access.png>)

## Download

### Released Version

ZeosDBO can be found here <http://sourceforge.net/projects/zeoslib> .  


### SVN Version

These instructions will give you the trunk version of Zeos. You can also get the testing version; see the Zeos forum for details on differences. 

#### Windows

  * Install [TortoiseSVN](<http://tortoisesvn.tigris.org/>).
  * Create a new directory (example: C:\zeosdbo) to put the files and open this directory in Windows Explorer.
  * Right click inside directory and select **SVN Checkout** (popup menu).
  * Enter **<http://svn.code.sf.net/p/zeoslib/code-0/trunk>** in edit and press OK.



#### Linux

**via esvn:**

  * Install esvn: in terminal type: **$ sudo apt-get install esvn**
  * when finished, create a folder **zeosdbo** in your home dir: **$ mkdir ~/zeosdbo**
  * type **$ esvn** to run it in graphic mode, and so, menu file, select **Checkout**.
  * Enter **<http://svn.code.sf.net/p/zeoslib/code-0/trunk>** in **URL** edit and **~/zeosdbo** in **Local Path**
  * Edit and press **OK**



**via subversion:**

  * first, install subversion: in terminal type: **$ sudo apt-get install subversion**
  * when finished, create a folder **zeosdbo** in your home dir: **$ mkdir ~/zeosdbo**
  * then access this folder **$ cd ~/zeosdbo**
  * type **$ svn co<http://svn.code.sf.net/p/zeoslib/code-0/trunk>**
  * later, if you want to update the repo, do **$ cd ~/zeosdbo/trunk** and **$ svn update**



#### Installing the SVN version in Lazarus

For all platforms, to install the SVN version into your Lazarus environment: 

  * open Lazarus IDE
  * select packages/open package files (.lpk), then navigate to and open the file: packages/lazarus/**zcomponent.lpk**
  * click in the button "Compile" and wait for that process
  * click in the button "Use/install" (it will ask to rebuild the IDE, accept and continue)
  * After rebuilding the IDE, the new components will appear in the last tab "Zeos Access" of the Component Palette



## Bug report/Feature request

You can send bugs at [Zeos Bug Tracker](<https://sourceforge.net/p/zeoslib/tickets/?source=navbar>). 

## External links

  * [Zeos Snapshots](<http://zeosdownloads.firmos.at/downloads/snapshots/>).



## See also

  * [Lazarus DB Faq](<Lazarus_DB_Faq.md> "Lazarus DB Faq") \- More about database programming
  * [Getting Lazarus](<Getting_Lazarus.md> "Getting Lazarus") \- Read them if you're a newbie in SVN...
  * [Zeos tutorial](<Zeos_tutorial.md> "Zeos tutorial")
  * [Tutorial Lazarus/Zeos/Firebird (Windows)](<https://lazarus.intern.es/tutorial_firebird_lazarus_zeos_2.html>) German/Parts in English [download site](<https://lazarus.intern.es/download_tutorials_lazarus_zeos_firebird.html>)

---

_Source: [https://wiki.freepascal.org/ZeosDBO](https://web.archive.org/web/20210125221827/https://wiki.freepascal.org/ZeosDBO)_
