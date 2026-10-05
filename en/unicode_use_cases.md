# unicode use cases

│ **English (en)** │

## Contents

  * 1 Introduction
    * 1.1 Cases
      * 1.1.1 Sqlite library requires filename to be encoded as UTF-8
      * 1.1.2 Firebird database path
      * 1.1.3 ODBC support for SQL_WVARCHAR string fields
    * 1.2 See Also



# Introduction

Currently, there's a lot of interest in the implementation of full Unicode support into fpc. This page is destined to describe situations where the developers face problems when dealing with Unicode characters/strings. In order to keep the information useful the description should be as detailed as possible and provide real code/examples when available. 

## Cases

### Sqlite library requires filename to be encoded as UTF-8

The sqlite3 wrapper class provided by FPC (TSqlite3Dataset) stores the FileName property into a String type (ansistring) and uses it to open the database through a sqlite function (sqlite3_open) that expects an UTF-8 encoded string. This works fine as long the string is UTF-8 encoded or has only ASCII characters. The problem is that the encoding varies according to the situation. LCL and *nix RTL return UTF-8, Win32 RTL returns the current locale encoding. Some workarounds were tried: 

  * _Call UTF8Encode inside FileName property method setter_  
This will work when the string is not UTF-8. When the string is already encoded in UTF-8, UTF8Encode will corrupt the string. Since there's no clean way to guess encoding, this option is not feasible.


  * _Call UTF8Encode or not in the source string, before setting the FileName property_  
This will handle the "strings coming from LCL" case, since that is always UTF-8. But using a string returned by a RTL function like GetAppConfigDir can lead to problems, e.g., in win32 systems with accented characters in the returned path, it will be necessary to call UTF8Encode while this is not necessary and dangerous in *nix systems.



So, in this case, AFAIK, there's no way to write a cross platform solution without using defines. 

### Firebird database path

When passing a connection string through the sqldb components to a Firebird database server, Firebird expects the path to the database to be encoded in the filesystem encoding of the server. So connecting from e.g. Windows to a typical Linux server (which has the UTF8 character set) would require converting the path to UTF8. 

Note that this is a weakness in the Firebird design itself (you have to know the Firebird server db filesystem encoding, which is slightly ridiculous) but this limitation did not appear when everybody spoke ASCII ;) 

  *     *       * to do: find mantis bug id where this problem was described ***



### ODBC support for SQL_WVARCHAR string fields

See [[1]](<http://bugs.freepascal.org/view.php?id=22095>) Note: that bug has a patch attached to support the *W functions; it has not been applied. 

## See Also

  * [FPC Unicode support](<FPC_Unicode_support.md> "FPC Unicode support")
  * [LCL Unicode Support](<LCL_Unicode_Support.md> "LCL Unicode Support")

---

_Source: [https://wiki.freepascal.org/unicode_use_cases](https://web.archive.org/web/20250219061834/https://wiki.freepascal.org/unicode_use_cases)_
