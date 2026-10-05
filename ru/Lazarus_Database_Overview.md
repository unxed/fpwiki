# Lazarus Database Overview

│ **[English (en)](<../en/Lazarus_Database_Overview.md>)** │  **русский (ru)** │

---  
[**Databases portal**](<../en/Portal_Databases.md> "Portal:Databases")  
References: 

  * [General info](<../en/Databases.md> "Databases")
  * [Libraries](<../en/Database_libraries.md> "Database libraries")
  * [Field types](<../en/Database_field_type.md> "Database field type")
  * [Controls](<../en/Data_Controls_tab.md> "Data Controls tab")
  * [FAQ](<../en/Lazarus_DB_Faq.md> "Lazarus DB Faq")
  * [SQL how-to](<../en/SqlDBHowto.md> "SqlDBHowto")
  * [Working With TSQLQuery](<../en/Working_With_TSQLQuery.md> "Working With TSQLQuery")
  * [In-memory database applications](<../en/How_to_write_in-memory_database_applications_in_Lazarus/FPC.md> "How to write in-memory database applications in Lazarus/FPC")

Tutorials/practical articles: 

  * [Overview](<../en/Lazarus_Database_Overview.md> "Lazarus Database Overview")
  * [0 - Database set-up](<../en/SQLdb_Tutorial0.md> "SQLdb Tutorial0")
  * [1 - Getting started](<../en/SQLdb_Tutorial1.md> "SQLdb Tutorial1")
  * [2 - Editing](<../en/SQLdb_Tutorial2.md> "SQLdb Tutorial2")
  * [3 - Queries](<../en/SQLdb_Tutorial3.md> "SQLdb Tutorial3")
  * [4 - Data modules](<../en/SQLdb_Tutorial4.md> "SQLdb Tutorial4")
  * [SQLdb Programming Reference](<../en/SQLdb_Programming_Reference.md> "SQLdb Programming Reference")

Databases  


    [Advantage](<../en/Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<../en/MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<../en/mssqlconn.md> "mssqlconn") \- [Postgres](<../en/postgres.md> "postgres") \- [Interbase](<../en/Firebird.md> "Firebird") \- [Firebird](<../en/Firebird.md> "Firebird") \- [Oracle](<../en/Oracle.md> "Oracle") \- [ODBC](<../en/ODBCConn.md> "ODBCConn") \- [Paradox](<../en/TParadox.md> "TParadox") \- [SQLite](<../en/SQLite.md> "SQLite") \- [dBASE](<../en/Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<../en/MS_Access.md> "MS Access") \- [Zeos](<../en/Zeos_tutorial.md> "Zeos tutorial")  
  
## Contents

  * 1 Обзор
  * 2 Lazarus и Interbase / Firebird
  * 3 Lazarus и MySQL
  * 4 Lazarus и MSSQL/Sybase
  * 5 Lazarus и ODBC
    * 5.1 Microsoft Access
  * 6 Lazarus и Oracle
  * 7 Lazarus и PostgreSQL
  * 8 Lazarus и SQLite
  * 9 Lazarus и Firebird/Interbase
  * 10 Lazarus и dBase
  * 11 Lazarus и Paradox
  * 12 TSdfDataset и TFixedDataset
  * 13 Lazarus и Advantage Database Server
  * 14 См. также
  * 15 Внешние ссылки



## Обзор

Эта статья представляет собой обзор баз данных, которые могут работать с Lazarus. 

Lazarus поддерживает несколько баз данных "из коробки" (используя, например, фреймворк SQLDB), однако разработчик должен установить необходимые пакеты (клиентские библиотеки) для каждой из них дополнительно. 

Вы можете получить доступ к базе данных посредством кода или путем добавления компонентов на форму. Data-aware компоненты предоставляют поля и подсоединяются путем установки свойства DataSource (Источник данных) для указания на [TDataSource](<../en/TDataSource.md> "TDataSource"). Datasource представляет собой таблицу и подключается к компонентам базы данных (примеры: _[TPSQLDatabase](</index.php?title=TPSQLDatabase&action=edit&redlink=1> "TPSQLDatabase \(page does not exist\)")_ , _[TSQLiteDataSet](</index.php?title=TSQLiteDataSet&action=edit&redlink=1> "TSQLiteDataSet \(page does not exist\)")_) путем установки свойства DataSet. Компоненты, учитывающие данные, расположены на [вкладке Data Controls](<Data_Controls_tab.md> "Data Controls tab/ru"). Источник данных и элементы управления базой данных расположены на [вкладке Data Access](<Data_Access_tab.md> "Data Access tab/ru"). 

См. руководства встроенных в Lazarus/FPC компонентов доступа к базам данных, подходящих для Firebird, MySQL, SQLite, PostgreSQL и т.д.: 

  * [SQLdb Tutorial0](<../en/SQLdb_Tutorial0.md> "SQLdb Tutorial0")
  * [SQLdb Tutorial1](<../en/SQLdb_Tutorial1.md> "SQLdb Tutorial1")
  * [SQLdb Tutorial2](<../en/SQLdb_Tutorial2.md> "SQLdb Tutorial2")
  * [SQLdb Tutorial3](<../en/SQLdb_Tutorial3.md> "SQLdb Tutorial3")
  * [SQLdb Tutorial4](<../en/SQLdb_Tutorial4.md> "SQLdb Tutorial4")



## Lazarus и Interbase / Firebird

  * Firebird очень хорошо поддерживается "из коробки" FPC/Lazarus (с использованием SQLDB); пожалуйста, смотрите [Firebird](<Firebird.md> "Firebird/ru") для получения подробностей.
  * [Другие библиотеки Firebird](<../en/Other_Firebird_libraries.md> "Other Firebird libraries") имеют список альтернативных библиотек доступа (например, PDO, Zeos, FBlib)



## Lazarus и MySQL

  * Пожалуйста, см. раздел [mysql](<../en/mysql.md> "mysql") для получения подробностей по деталям различных методов доступа, которые включают:


  1. Встроенную поддержку [SQLdb](<../en/SQLdb_Package.md> "SQLdb Package")
  2. PDO
  3. [Zeos](<../en/ZeosDBO.md> "ZeosDBO")
  4. [MySQL data access Lazarus components](<https://www.деварт.com/mydac/>)



## Lazarus и MSSQL/Sybase

Вы можете подключиться к базам данных Microsoft SQL Server, используя 

  1. [SQL Server data access Lazarus components](<https://www.деварт.com/sdac/>). Они работают на Windows и macOS. Бесплатны для загрузки.
  2. Встроенные **SQLdb** компоненты подключения к БД **TMSSQLConnection** и **TSybaseConnection** (начиная с Lazarus 1.0.8/FPC 2.6.2): см. [Mssqlconn](</index.php?title=Mssqlconn&action=edit&redlink=1> "Mssqlconn \(page does not exist\)").
  3. **Zeos** компонент **TZConnection** (последний CVS, см. ссылки на Zeos в других местах на этой странице) 
     1. На Windows вы можете выбрать между собственной библиотекой **ntwdblib.dll** (протокол **mssql**) или библиотеками FreeTDS (протокол **FreeTDS_MsSQL-nnnn**), где nnnn - один из четырех вариантов в зависимости от версии сервера. Для Delphi (не для Lazarus) существует также другой протокол Zeos **ado** для MSSQL 2005 или более поздней версии. При использовании протоколов mssql или ado генерируется платформонезависимый код.
     2. На Linux единственный способ - использовать протоколы и библиотеки FreeTDS (вы должны использовать **libsybdb.so**).
  4. **ODBC** (MSSQL и Sybase ASE) со SQLdb **TODBCConnection** (рассмотрите использование **TMSSQLConnection** и **TSybaseConnection** в качестве альтернативы) 
     1. См. также [Connecting to Microsoft SQL Server](<../en/ODBCConn.md> "ODBCConn")
     2. На Windows он использует собственные библиотеки ODBC Microsoft (например, sqlsrv32.dll для MSSQL 2000)
     3. На Linux он использует unixODBC + FreeTDS (пакеты unixodbc или iodbc, и tdsodbc). С 2012 года существует также драйвер ODBC для Microsoft SQL Server 1.0 для Linux, который является бинарным продуктом (без открытого исходного кода) и обеспечивает собственное подключение, но выпущен только для х64 и только для RedHat.



## Lazarus и ODBC

ODBC - это общий стандарт подключения к базе данных, который доступен в Linux, Windows и OSX. Вам потребуется драйвер ODBC от поставщика базы данных и надстройка ODBC "data source" (также известный как DSN). Вы можете использовать компоненты SQLDB ([TODBCConnection](<../en/TODBCConnection.md> "TODBCConnection")) для подключения к источнику данных ODBC. Смотрите [ODBCConn](<../en/ODBCConn.md> "ODBCConn") для получения более подробной информации и примеров. 

### Microsoft Access

Вы можете использовать драйвер ODBC в Windows и Linux для доступа к базам данных Access; см. [MS Access](<../en/MS_Access.md> "MS Access")

## Lazarus и Oracle

  * См. [Oracle](<../en/Oracle.md> "Oracle"). Методы доступа включают в себя:


  1. Встроенную поддержку SQLDB
  2. Zeos
  3. [Oracle data access Lazarus component](<https://www.dev_art.com/odac/>)



* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): девартовский сайт почему-то внесен список спам-фильтра. Поэтому в выше приведенной ссылке удалите знак подчеркивания из dev_art, чтобы перейти на оф.сайт ODAC 

## Lazarus и PostgreSQL

  * PostgreSQL очень хорошо поддерживается FPC/Lazarus "из коробки"
  * Пожалуйста, см. [postgres](<../en/postgres.md> "postgres") для получения деталей о различных методах доступа, которые включают:


  1. Встроенная поддержка SQLdb. Используйте компонент **TPQConnection** со вкладки [SQLdb](<../en/SQLdb_tab.md> "SQLdb tab") из [ палитры компонент](<../en/Component_Palette.md> "Component Palette")
  2. [Zeos](<../en/Zeos.md> "Zeos"). Используйте компонент **TZConnection** с протоколом 'postgresql' из палитры **Zeos Access**
  3. [PostgreSQL data access Lazarus component](<https://www.dev_art.com/pgdac/>)



* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): девартовский сайт почему-то внесен список спам-фильтра. Поэтому в выше приведенной ссылке удалите знак подчеркивания из dev_art, чтобы перейти на оф.сайт ODAC 

## Lazarus и SQLite

SQLite - это встроенная база данных; код базы данных может распространяться как библиотека (.dll/.so/.dylib) вместе с вашим приложением, чтобы сделать его автономным (сравнимым со встроенным Firebird). SQLite довольно популярен благодаря своей относительной простоте, скорости, небольшому размеру и кроссплатформенной поддержке. 

Пожалуйста, см. страницу [SQLite](<../en/SQLite.md> "SQLite") для получения подробной информации о различных методах доступа, которые включают: 

  1. Встроенную поддержку SQLDb. Используйте компонент **TSQLite3Connection** из палитры **SQLdb**
  2. Zeos
  3. SQLitePass
  4. TSQLite3Dataset
  5. [SQLite data access Lazarus components](<https://www.dev_art.com/litedac/>)



* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): девартовский сайт почему-то внесен список спам-фильтра. Поэтому в выше приведенной ссылке удалите знак подчеркивания из dev_art, чтобы перейти на оф.сайт ODAC 

## Lazarus и Firebird/Interbase

Компоненты доступа к данным InterBase (и FireBird) (IBDAC) - это библиотека компонентов, которая обеспечивает нативное подключение к InterBase, Firebird и Yaffil из Lazarus (и Free Pascal) на Windows, macOS, iOS, Android, Linux и FreeBSD для 32-битных и 64-битных платформ. Приложения на основе IBDAC подключаются к серверу напрямую, используя клиента InterBase. IBDAC призван помочь программистам разрабатывать более быстрые и чистые приложения для баз данных InterBase. 

  
IBDAC является полной заменой стандартных решений InterBase для подключения. Он представляет собой эффективную альтернативу InterBase Express Components, Borland Database Engine (BDE) и стандартному драйверу dbExpress для доступа к InterBase. 

[Firebird data access components for Lazarus](<https://www.dev_art.com/ibdac/download.html>) бесплатны для загрузки 

* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): девартовский сайт почему-то внесен список спам-фильтра. Поэтому в выше приведенной ссылке удалите знак подчеркивания из dev_art, чтобы перейти на оф.сайт ODAC 

## Lazarus и dBase

FPC включает в себя простой компонент базы данных, производный от компонента Delphi TTable, который называется «TDbf» [TDbf Website](<http://tdbf.sourceforge.net/>)). Он поддерживает различные форматы DBase и Foxpro. 

**TDbf** не принимает команды SQL, но вы можете использовать методы набора данных и т.д., и вы также можете использовать обычные элементы управления с привязкой к данным, такие как DBGrid. 

Это не требует какого-либо вида движка базы данных во время выполнения. Однако это не лучший вариант для больших приложений баз данных. 

См. страницу [TDbf Tutorial](<../en/Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") как для учебника, так и для документации. 

Вы можете использовать, например, OpenOffice/LibreOffice Base для визуального создания/редактирования dbf-файлов или создания DBF-файлов в коде с использованием [TDbf](<../en/TDbf.md> "TDbf"). 

## Lazarus и Paradox

Paradox был форматом по умолчанию для файлов базы данных в старых версиях Delphi. Концепция аналогична файлам DBase/DBF, где «база данных» - это папка, а каждая таблица - это файл внутри этой папки. Кроме того, каждый индекс тоже является файлом. Для доступа к этим файлам из Lazarus у нас есть следующие опции: 

  * **[TParadox](<../en/TParadox.md> "TParadox")** : Установите пакет "lazparadox 0.0", входящий в стандартную поставку. Когда вы установите этот пакет, вы увидите новый компонент с надписью "PDX" в палитре "Data Access". Этот компонент не является автономным, он использует «родную» библиотеку, а именно [pdxlib library](<http://pxlib.sourceforge.net>), которая доступна для Linux и Windows. Например, для установки в Debian вы можете получить **pxlib1** из менеджера пакетов. В Windows вам нужен файл pxlib.dll.


  * **[TPdx](</index.php?title=TPdx&action=edit&redlink=1> "TPdx \(page does not exist\)")** : Paradox DataSet для Lazarus и Delphi с [этого сайта](<http://tpdx.sourceforge.net/>). Этот компонент является автономным (чистый объектный паскаль), не требует никакой внешней библиотеки, но может только читать (не записывать) файлы Paradox. Пакет для установки - "paradoxlaz.lpk", и компонент должен появиться в палитре "Data Access" с меткой PDX (но оранжевого цвета).


  * **[TParadoxDataSet](<../en/TParadoxDataSet.md> "TParadoxDataSet")** : является [TDataSet](<../en/TDataSet.md> "TDataSet"), который может только читать файлы Paradox до версии 7. Подход аналогичен компоненту TPdx, пакет для установки - "lazparadox.lpk", и компонент также должен отображаться в палитре "Data Access".



## TSdfDataset и TFixedDataset

[TSdfDataSet](<TSdfDataSet.md> "TSdfDataSet/ru") и [TFixedFormatDataSet](<TFixedFormatDataSet.md> "TFixedFormatDataSet/ru") \- это два простых наследника [TDataSet](<../en/TDataSet.md> "TDataSet"), которые предлагают очень простой текстовый формат хранения. Эти наборы данных очень удобны для небольших баз данных, поскольку они полностью реализованы как модуль Object Pascal и, следовательно, не требуют внешних библиотек. Кроме того, их текстовый формат позволяет их легко просматриваться/редактироваться с помощью текстового редактора. 

См. [CSV](<../en/CSV.md> "CSV") для примера кода. 

## Lazarus и Advantage Database Server

  * Пожалуйста, см. [Advantage Database Server](<../en/Advantage_Database_Server.md> "Advantage Database Server") для получения подробностей по использованию Advantage Database Server



## См. также

(Отсортировано по алфавиту) 

  * [Database Portal](<../en/Portal_Databases.md> "Portal:Databases")
  * [Databases](<../en/Databases.md> "Databases")
  * [Database_field_type](<../en/Database_field_type.md> "Database field type")
  * [Как писать приложения базы данных в памяти в Lazarus/FPC](<How_to_write_in-memory_database_applications_in_Lazarus/FPC.md> "How to write in-memory database applications in Lazarus/FPC/ru")
  * [Lazarus DB Faq](<../en/Lazarus_DB_Faq.md> "Lazarus DB Faq")
  * [Lazarus Tdbf Tutorial](<../en/Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial")
  * [Multi-tier options with FPC|многоуровневые опции с fpc](<../en/multi-tier_options_with_fpc.md> "multi-tier options with fpc")
  * [SQLdb Tutorial1](<../en/SQLdb_Tutorial1.md> "SQLdb Tutorial1")
  * [SqlDBHowto](<../en/SqlDBHowto.md> "SqlDBHowto")
  * [tiOPF](<../en/tiOPF.md> "tiOPF") \- бесплатный и с открытым исходным кодом Object Persistence Framework.
  * [Zeos tutorial](<../en/Zeos_tutorial.md> "Zeos tutorial")



## Внешние ссылки

  * [Pascal Data Objects](<http://pdo.sourceforge.net>) \- API базы данных, который работал как для FPC, так и для Delphi, и использует собственные библиотеки MySQL для версий 4.1 и 5.0 и Firebird SQL 1.5 и 2.0. Навеяно PHP-классом PDO.
  * [Zeos+SQLite Tutorial](<http://lazaruszeos.blogspot.com>) \- хороший учебник, использующий скриншоты и скринкасты, он объясняет, как использовать SQLite и Zeos, по-испански (Google Translate хорошо справляется с переводом на английский)

---

_Source: [https://wiki.freepascal.org/Lazarus_Database_Overview/ru](https://web.archive.org/web/20250517004709/https://wiki.freepascal.org/Lazarus_Database_Overview/ru)_
