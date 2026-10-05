# TPSQL

│ **English (en)** │

**TPSQL** is a modified-LGPL [postgres](<postgres.md> "postgres") database package for Lazarus. It defines two components, TPSQLDatabase and TPSQLDataset, allowing applications to connect to PostgreSQL database servers over TCP/IP networks. 

The download contains the component Pascal files, the Lazarus package file and resource files and the modified-LGPL license text files. 

This component was designed for applications running on Linux and Win32 platforms. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** TPSQL is not the same as [TPSQLConnection](<postgres.md> "postgres"), the PostgreSQL connector that is part of the SQLDB database components.

## Contents

  * 1 Author
  * 2 License
  * 3 Download
  * 4 Change Log
  * 5 Dependencies / System Requirements
  * 6 Installation
  * 7 Usage



### Author

[Antonio d'Avino](</index.php?title=User:Blacktony&action=edit&redlink=1> "User:Blacktony \(page does not exist\)")

### License

Modified LGPL (read COPYING.modifiedLGPL and COPYING.LGPL included in package). 

### Download

The latest stable release can be found on the [Lazarus CCR Files page](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=98986>) or on author's web pages <http://infoconsult.homelinux.net>. 

### Change Log

  * Version 0.4.0 2005/06/01
  * Version 0.4.1 2005/06/02 
    * ClientEncoding property added to TPSQLDatabase class.
  * Version 0.4.2 2005/06/03 
    * Some changes to destroy method of TPSQLDatabase/TPSQLDataset for avoiding exception when closing IDE/project with components in Active/Connected status.
    * Base path in archived files changed from 'usr/share/lazarus/components/psql' to 'psql'
  * Version 0.4.6 2005/06/06 
    * Executing queries that doesn't return data column now raises an exception.
    * commandQuery function added (see Usage section).
    * beginTransaction/commitTransaction/rollbackTransaction support added (see Usage section).
    * More ClientEncoding entities added.



### Dependencies / System Requirements

  * Lazarus 0.9.6 (FPC 1.9.8)



Status: 

    Stable

Issues: 

    Tested on Windows (Win2K) and Linux (Mdk 10.1). Database server: PostgreSQL 8.0.3 (running on both platforms).

### Installation

  * In the lazarus/components directory, untar (unzip) the files from psql-laz-package-<version>.tar.gz file. The psql folder will be created.
  * Open lazarus
  * Open the package psql_laz.lpk with Component/Open package file (.lpk)
  * (Click on Compile only if you don't want to install the component into the IDE)
  * Click on Install and answer 'Yes' when you are asked about Lazarus rebuilding. A new tab named 'PSQL' will be created in the components palette.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** **Important for Win32 users.** In case you experience some difficults in compiling the Pascal files in the package, you may need to add the following path: ` <some-drive>:<some-path>\lazarus\lcl\units\i386-win32 ` (ie: `c:\programs\lazarus\lcl\units\i386-win32`) to the 'Other unit files ...' in 'Paths' tab of 'Compiler Options' of the lazarus package manager window.

### Usage

Drop a TPSQLDatabase component on a form, for any different PostgreSQL database your application needs to connect to. Mandatory property of this component you have to set are: 

  * DatabaseName : The name of the PostgreSQL database that your application needs to connect to.
  * HostName : the IP address or the URL of the PC hosting the PostgreSQL server.
  * UserName : The name of an user having permission to access to host/database.



Optionally you may need to set Password property for correct connection to server. Then, to activate the connection, you neet to set the Connected property to 'True'. An exception will be raised if connection failed (wrong parameters, user with no permission to access the host/database, network errors or PostgreSQL server not active). Set this property to 'False' for closing connection. 

**Note about the ClientEncoding property.** The ClientEncoding property allows user to set a character set for the client application different from the one defined for the database. The PostgreSQL server make a 'translation' between the two sets. However a "ClientEncoding change failed" exception may be the result of a ClientEncoding property change. This may be due to a database character set incompatible with the clientencoding you selected. IE, the char set 'LATIN9' is not compatible with a 'WIN1250'. You select the default char set for the whole database cluster when you create it with the command 'initdb', using the -E option: IE. 'initdb -E UNICODE'. Also, you can define a different character set for any database you create (different from the default one of the whole db cluster), also with the -E option: IE. 'createdb -E UNICODE Test' or 
    
    
    CREATE DATABASE TEST WITH ENCODING 'UNICODE';
    

(note you must quote the character set name in the 'CREATE DATABASE' command). I suggest to set UNICODE character set as the default for your db cluster (or for the database you create), because it is compatible with the whole set of charset available for the client, except the MULE_INTERNAL. 

Now you may drop a TPSQLDataset component on the form for any table connection you need. The main property to set on this component type is the Database one. A dropdown menu allows you to select one of any [TDatabase](</index.php?title=TDatabase&action=edit&redlink=1> "TDatabase \(page does not exist\)") descendant component present on form (of course, you must select a TPSQLDatabase component). 

You also need to provide a valid SQL statement in SQL property. Now, you are able to open the TPSQLDataset, setting to 'True' the Active property. Several exceptions are provided for signaling failing conditions. Please, refer to TDataSet documentation for infos and examples about using the TPSQLDataset component in order to access to SQL retrieved data rows as well as conneting to Data Controls components. However, procedures and functions added to parent class are explained here: 

function commandQuery( query: String ): ShortInt
    Use this function for submit queries that doesn't returns data columns, as update queries (ie. update, insert, delete etc.). Current dataset is not affected, however it will reflect changes made by execution of commandQuery itself, if it has some influence on current dataset.Function returns 0 if succeded, -1 if failed. No exceptions are provided.
procedure beginTransaction()
    Starts an SQL transaction session. Changes to table may be submitted using the commandQuery function.
procedure commitTransaction()
    Ends an SQL transaction session, committing changes submitted starting from last beginTransaction execution.
procedure rollbackTransaction()
    Ends an SQL transaction session, aborting changes.

---

_Source: [https://wiki.freepascal.org/TPSQL](https://web.archive.org/web/20241104221401/https://wiki.freepascal.org/TPSQL)_
