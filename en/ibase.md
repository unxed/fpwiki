# ibase

## Contents

  * 1 Free Pascal interface to Interbase/Firebird
    * 1.1 Overview
  * 2 Requirements
  * 3 Installation



## Free Pascal interface to Interbase/Firebird

### Overview

You can use Free Pascal to access an [Interbase](</index.php?title=Interbase&action=edit&redlink=1> "Interbase \(page does not exist\)") or [Firebird](<Firebird.md> "Firebird") database server from Linux and Windows. 

For more information on Firebird, see their [home page](<http://www.firebirdsql.org/>). For more info on Interbase, see their [home page](<http://www.interbase.com/>)

Interfacing is very easy; all you need to do is compile some units, and use these units in your program. You need to specify the place of the Interbase client Library (libgds) when compiling, and that is it. the provided units take care of the rest. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** There is a higher level, object oriented SQLDB wrapper around this unit called [TIBConnection](<TIBConnection.md> "TIBConnection") that is more convenient to use. See [the Firebird article](<Firebird.md> "Firebird") for more details

## Requirements

You need at least version 0.99.11 of Free Pascal. The headers are translated for Interbase versions 4 and 6, or Firebird 1.0. 

## Installation

The ibase unit is distributed as part of the Free Pascal packages, and is distributed with the compiler. 

This file contains a directory base/ibase with the units, a test program and a make file. The following units exist: 

  * **ibase40** the Interbase version 4 or 5 interface header translation. The interface is largely the same as for version 6.
  * **ibase60** the Interbase version 6 interface header translation. .



There is also a test program available. 

Typing 
    
    
    make
    

Should compile the units and the program. If compilation was succesfull, you can install with 
    
    
    make install
    

(Remember to set the directory where the units should be installed.) 

You can then test the program by running 
    
    
    make test
    

This will: 

  * Run a script to create a table in a database, and fill it with some data. (the isql program should be in your PATH for this) . By default, the used database is testdb.
  * Run the testprogram testdb
  * Run a shell script again to remove the created table.



You will see a lot of messages on your screen, giving you feedback and results. If something went wrong, make will inform you of this. 

Go to back [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/ibase](https://web.archive.org/web/20240227025153/https://wiki.freepascal.org/ibase)_
