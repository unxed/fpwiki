# odbc

## ODBC : Universal database access.

This package contains one unit **odbcsql** which contains the header file translations of the ODBC system. It works on the windows ODBC implementation and works with UnixODBC as well - it was tested on both. 

The test program **testodbc** accesses a MS-Access database and runs a query on it. 

The FCL contains a unit fpodbc which contains an OOP wrapper around the ODBC calls, which makes ODBC programming considerably easier and more pascal-like. 

Also, there is an [SQLDB ODBC driver](<ODBCConn.md> "ODBCConn") so you can use recordsets, visual database components in Lazarus, etc. 

Go to back [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/odbc](https://web.archive.org/web/20210228100740/https://wiki.freepascal.org/odbc)_
