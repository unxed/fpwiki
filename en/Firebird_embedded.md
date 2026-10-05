# Firebird embedded

│ **English (en)** │  **[русский (ru)](<../ru/Firebird_embedded.md>)** │

## Contents

  * 1 What is Firebird?
    * 1.1 Advantages
    * 1.2 Disadvantages
  * 2 Installation
    * 2.1 Windows
    * 2.2 Linux
    * 2.3 macOS
    * 2.4 FreeBSD
  * 3 Connecting to Firebird servers
  * 4 Use
    * 4.1 Creating databases
      * 4.1.1 Using Firebird ISQL
      * 4.1.2 Programmatically creating databases
      * 4.1.3 Creating objects like tables, views, triggers...
  * 5 Running FPC database test suite
    * 5.1 Telling the framework which database to use
    * 5.2 Creating the test database
      * 5.2.1 Manually creating a database
      * 5.2.2 Programmatically creating a database
    * 5.3 Running the test suite
  * 6 Windows x64 and FPC pre-March 2013
  * 7 External references



# What is Firebird?

Firebird embedded is a special version of the [Firebird](<Firebird.md> "Firebird") database server. It lets you run your programs without a dedicated database server. Instead, you add a library (DLL/.so/.dylib) to your application) and let your application access your Firebird .fdb database file using that library. 

This is similar to the way programs like Microsoft Access and LibreOffice Base can work. 

## Advantages

  * runs on many architectures (Linux, macOS, Windows, FreeBSD, Solaris)
  * no separate server setup necessary, reducing complexity
  * you can easily switch from embedded to full client/server - without changing a line of code (except your connection string). This is probably a nice advantage compared to sqlite.



## Disadvantages

  * only single user access
  * need to distribute some libraries along with your program when distributing it (unless you can statically link them)



# Installation

On Windows, Firebird embedded and regular Firebird client libraries are different files. Firebird embedded can always serve as a regular client, making it a logical choice for inclusion in your installation. 

## Windows

Download and unzip the Firebird embedded kit. Make sure the .dll and .manifest files are either: 

  * in your project directory and executable output directory (where the .exe is generated) or
  * in a directory in your PATH (not your system directory) (useful if you don't want to keep copying dlls)



Note that bitness must match: you must use the 64 bit embedded version if you compile your program for 64 bit, and 32 if you're writing 32 bit programs. Also make sure the the .dlls and .manifest files are placed in your project output (executable) directory, and distribute them with your applications, along with the license files. 

As an indication, the minimum necessary files for Firebird 2.5 are: 

  * fbembed.dll
  * firebird.msg
  * ib_util.dll
  * icudt30.dll
  * icuin30.dll
  * icuuc30.dll
  * IDPLicense.txt
  * IPLicense.txt
  * Microsoft.VC80.CRT.manifest
  * msvcp80.dll
  * msvcr80.dll



## Linux

For Debian, probably similar for Ubuntu: 

Get library files as well as the symlink: e.g. /usr/lib/x86_64-linux-gnu/libfbembed.so linking to /user/lib/x86_64-linux-gnu/libfbembed.so.bla.x.y 
    
    
    aptitude install libfbembed2.5 firebird-dev
    

For at least Firebird 2.5+: Do not specify a username or password as that will force a lookup to security.fdb which may fail if system-wide security.fdb cannot be accessed. Do specify the role RDB$ADMIN on Firebird 2.5+ as that should give you full control over the database file. 

Put these files in your project directory (to do: test if this is all needed): 
    
    
    firebird/libfbembed.so (or perhaps libfbembed.so.2.5)
    firebird.conf
    firebird/security.fdb #perhaps not necessary
    firebird/intl/fbintl
    firebird/udf/fbudf.so
    

For Firebird 3 you will need: 
    
    
    firebird/libfbclient.so
    plugins/libEngine12.so
    

You need to change plugins directory variable. If you cannot do that you need libEngine into its original directory. 

Had set this in firebird.conf (to do: check if this is all needed) 
    
    
    RootDirectory=./firebird #replace with your directory
    DatabaseAccess = Full #probably useful
    ExternalFileAccess = Full #probably useful
    UdfAccess = Full #probably useful
    #disable
    #TempDirectories = /tmp #=> didn't fix /tmp/firebird lock issues
    #TempDirectories = ~/tmp #=> didn't fix /tmp/firebird lock issues
    

2 environment variables need to be set before the application is started. Supposing the application is installed in /home/pascaldev/embed: 
    
    
    # tell dynamic loader where to find embedded lib:
    LD_LIBRARY_PATH=/home/pascaldev/embed/firebird
    # tell server where to find files (messages, config etc)
    FIREBIRD=. #or use absolute path: /home/pascaldev/embed
    

If you hit errors like 
    
    
    EIBDatabaseError/ : DoInternalConnect :
     -Can't access lock files' directory /tmp/firebird
    

you probably have stale lock files lying around that are owned by another user Deleting the /tmp/firebird directory works, but the problem is that it will be recreated under your own local permissions 

See [How to setup Firebird embedded on Linux or Mac?](<http://www.firebirdfaq.org/faq51/>) for hints on working with embedded Firebird on Linux or Apple macOS. Somebody used Firebird embedded with FPC: [Earlier version of Firebird embedded on Linux](<http://www.mwasoftware.co.uk/index.php?option=com_content&task=view&id=105>)

## macOS

  * todo: write this, perhaps adapt [Earlier version of Firebird embedded on Linux](<http://www.mwasoftware.co.uk/index.php?option=com_content&task=view&id=105>) and especially [[1]](<http://paulbeachsblog.blogspot.mx/2012/09/firebird-embedded-on-macosx.html>)



## FreeBSD

  * to do: write this, perhaps adapt [Earlier version of Firebird embedded on Linux](<http://www.mwasoftware.co.uk/index.php?option=com_content&task=view&id=105>)



# Connecting to Firebird servers

As mentioned, Firebird embedded can work like a regular Firebird client. Therefore, using the embedded Firebird library instead of the client/server Firebird library can help make your application more flexible. 

Note: there is a bug at least in Firebird embedded 2.5.3 together with FPC 2.6.4 that crashes when closing a connection to a remote server. This bug has been fixed in FPC trunk. 

# Use

Once you have the proper Firebird embedded libraries in the right path (see above), you need to specify that you are connecting to an embedded instead of a client server environment. Leave the hostname property of the _IBConnection_ object empty. The rest of the properties work like in the client/server version, e.g. the DatabaseName is the full path to your .fdb file - or just the database name if it is in the same directory as fbembed.dll. Additionally, older versions of FPC (FPC 2.6.2+ certainly don't need this) may require that you set the _UseEmbeddedFirebird_ property to true. The property is not present in the SQLDB units, but you need to add _ibase60dyn_ to your uses clause. 

## Creating databases

### Using Firebird ISQL

You can create databases by using the Firebird isql application (e.g. as provided in the regular Firebird (client) package). On Windows, this executable should be in the same directory as your fbembed.dll. Note: isql expects the fbclient library, not fbembed; you can copy and rename fbembed, e.g. into fbclient.dll on Windows, to fix this. Run **isql** , then do: 
    
    
    create database 'employee.fdb' user 'SYSDBA' password 'masterkey' page_size 16384 default character set UTF8;
    exit;
    

Dropping/deleting a database would basically mean deleting the file, but you can also run: **isql**
    
    
    connect 'employee.fdb' user 'sysdba' password 'masterkey';
    drop database;
    commit;
    exit;
    

### Programmatically creating databases

You can also create a database from within your program. The program in the Creating the test database section below shows how you can detect missing databases and create them on the fly. A similar procedure could be called in Lazarus applications when initializing your form (FormShow event). 

### Creating objects like tables, views, triggers...

Creating the objects - as most things - is the same in Firebird and Firebird embedded. Please see [Firebird#Creating_objects_programmatically](<Firebird.md> "Firebird")

# Running FPC database test suite

It can be useful to run the FPC database test framework (in packages\fcl-db\tests) when you are hunting bugs, trying to find out how something works, or improving the database code. 

## Telling the framework which database to use

You will need to edit the db test framework .ini file to tell the test executable that it needs to use your embedded database. See [Databases#Running_FPC_database_tests](<Databases.md> "Databases") for instructions on how to do this. 

## Creating the test database

If you have set up the framework to use Firebird, you will need to prepare the environment for testing. 

Copy over the Firebird embedded libraries and other files for your architecture into the test directory. 

Now create the test database - manually or programmatically 

### Manually creating a database

**isql**
    
    
    create database 'test.fdb' user 'SYSDBA' password 'masterkey' page_size 16384 default character set UTF8;
    exit;
    

### Programmatically creating a database

You can programmatically create a database. The example below creates a local, embedded database, but it can be easily changed to a remote database (adjust hostname and if necessary path): 
    
    
    program firebirdembeddedtest;
    {$mode objfpc}{$H+}
    
    uses 
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF} 
      Classes, SysUtils,
      sqldb, IBConnection {for Firebird};
    
    const
      // Name for the database. Be sure to specify the same name in your database.ini
      DatabaseFile = 'test.fdb'; //Sensible name for a test database, right?
    var
      Fire: TIBConnection;
    
    begin
      //Connection to Firebird database
      // The next line is needed for quite old FPC versions
      // Newer versions will first look for fbembed.dll in the application directory automatically
      //UseEmbeddedFirebird:=true; // Using embedded (and fbembed.dll) or regular client/server (fbclient.dll), requires ibase60dyn in uses
      Fire:=TIBConnection.Create(nil);
      try
        Fire.HostName := ''; //must be empty for embedded Firebird; must be filled for client/server Firebird
        Fire.DatabaseName := DatabaseFile; //(path and) filename
        // Username and password do not matter for authentication, but you do get authorizations in the database
        // based on the name (and optionally role) you give.
        Fire.Username := 'SYSDBA';
        Fire.Password := 'masterkey'; //default password for SYSDBA
        Fire.Charset := 'UTF8'; //Send and receive string data in UTF8 encoding
        Fire.Dialect := 3; //Nobody uses 1 or 2 anymore.
        Fire.Params.Add('PAGE_SIZE=16384'); //I like a large page size (used when creating a database). Useful for larger indexes=>larger possible column sizes
    
        // Find out if there is a database in the application directory.
        // If not, create it. Note: this may fail if you don't have enough permissions.
    
        // If you use client/server, you obviously don't need this part of the code.
        if (FileExists(DatabaseFile)=false) then
        begin
            writeln('File '+DatabaseFile+' does not exist.');
            writeln('Creating a Firebird embedded database...');
            // Create the database as it doesn't exist
            try
              Fire.CreateDB; //Create the database file.
            except
              on E: Exception do
              begin
                writeln('ERROR creating database. Probably problems loading embedded library:');
                writeln('- not all files present');
                writeln('- wrong architecture (e.g. 32 bit instead of 64 bit)');
                writeln('Exception message:');
                writeln(E.ClassName+'/'+E.Message);
              end;
            end;
            Fire.Close;
        end;
      finally
        Fire.Free;
      end; 
    end.
    

## Running the test suite

Finally compile and run the _dbtestframework.pas_ database test program. It should generate status output on the console and XML output in files with test results that can be analysed and sent to developers. 

# Windows x64 and FPC pre-March 2013

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** In versions of FPC below FPC 2.7.1 r23738 (March 2013), FPC x64 on Windows does not work well with the Firebird embedded dll - or other DLLs. The section below contains a workaround if you have an old FPC version

On versions of FPC below 2.7.1 r23738 you can get errors/exceptions from the IBConnection object even if you write correct code (see e.g. [Create database bug report](<http://bugs.freepascal.org/view.php?id=21581>). 

Workarounds: 

  1. move to a development snapshot/FPC version after FPC 2.7.1 r23738; see [[[2]](<http://wiki.lazarus.freepascal.org/User_Changes_3.0#Exception_handling_has_been_changed_to_be_ABI-conformant>)]
  2. if on FPC 2.7.1 lower than r23738, compile FPC with -dTEST_WIN64_SEH in order to use the new Structured Exception Handling (SEH) code on Win64.
  3. use a 32 bit FPC and Lazarus on Windows 64



# External references

  * [How to setup Firebird embedded on Linux or Mac?](<http://www.firebirdfaq.org/faq51/>)
  * [Earlier version of Firebird embedded on Linux](<http://www.mwasoftware.co.uk/index.php?option=com_content&task=view&id=105>)

---

_Source: [https://wiki.freepascal.org/Firebird_embedded](https://web.archive.org/web/20250513072413/https://wiki.freepascal.org/Firebird_embedded)_
