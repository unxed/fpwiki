# SQLite

│ **[English (en)](<../en/SQLite.md> "SQLite")** │  **[español (es)](</SQLite/es> "SQLite/es")** │  **[français (fr)](</SQLite/fr> "SQLite/fr")** │  **[日本語 (ja)](</SQLite/ja> "SQLite/ja")** │  **[polski (pl)](</SQLite/pl> "SQLite/pl")** │  **русский (ru)** │  **[中文（中国大陆）‎ (zh_CN)](</SQLite/zh_CN> "SQLite/zh CN")** │    
****  
  
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

  * 1 Поддержка SQLite и FPC/Lazarus
    * 1.1 Прямой доступ к SQLite
    * 1.2 Встроенный SQLDB
      * 1.2.1 Поддержка Spatialite
      * 1.2.2 Поддержка шифрования SQLite
      * 1.2.3 sqlite3backup
    * 1.3 Zeos
    * 1.4 SQLitePass
    * 1.5 TSQLite3Dataset и TSQLiteDataset
  * 2 Использование компонентов SQLdb со SQLite
    * 2.1 Creating a Database
    * 2.2 Создание определяемых пользователем порядка сортировки
    * 2.3 Создание пользовательских функций (UDF)
    * 2.4 SQLite3 и даты
      * 2.4.1 Значения по умолчанию для местного времени вместо UTC
    * 2.5 Устранение неполадок SQLDB и SQLite
    * 2.6 Vacuum и другие операции, которые необходимо выполнять вне транзакции
  * 3 Использование TSQLite3Dataset
    * 3.1 Требования
    * 3.2 Как использовать (базовое использование)
      * 3.2.1 Пример Master/detail
    * 3.3 Примечания
  * 4 See also



## Поддержка SQLite и FPC/Lazarus

SQLite - это встроенная (не серверная) однопользовательская база данных, которую можно использовать в приложениях FPC и Lazarus. Для доступа к SQLite из программ FPC/Lazarus можно использовать различные драйверы. Все драйверы нуждаются в библиотеке SQLite/dll в исполняемом каталоге (который может быть каталогом вашего проекта или, например, (projectdir)/lib/architecture/ в зависимости от настроек вашего проекта Lazarus) (и распространяется вместе с вашим исполняемым файлом) для работы. 

Это также может быть необходимо включить в ваш каталог Lazarus IDE. См. [ветку форума](<https://forum.lazarus.freepascal.org/index.php?topic=46118.msg327940#msg327940>) и особенно внимательно прочитайте про TSQLite3Dataset и TSQLiteDataset [ниже](<SQLite.md> "SQLite/ru")

В большинстве дистрибутивов Linux по умолчанию установлен sqlite3 (например, libsqlite3.so.0), но для дистрибутивов Ubuntu, по крайней мере, также требуется соответствующий пакет Dev. Оба должны быть установлены через системный менеджер пакетов и помечены как зависимость, а не распространяться вместе с вашим приложением. 

Win64: см. предупреждение [ здесь](<../en/Windows_Programming_Tips.md> "Windows Programming Tips") о неиспользовании определенных версий FPC/Lazarus Win64. 

### Прямой доступ к SQLite

Вы можете использовать простой способ подключения SQLite к Lazarus. Компоненты называются LiteDAC. Компоненты доступа к данным SQLite (LiteDAC) - это библиотека компонентов, которая обеспечивает встроенное подключение к SQLite из Lazarus (и Free Pascal) в Windows, macOS, iOS, Android, Linux и FreeBSD как для 32-разрядных, так и для 64-разрядных платформ. LiteDAC предназначен для программистов, которые могут разрабатывать действительно кроссплатформенные настольные и мобильные приложения баз данных SQLite без необходимости развертывания каких-либо дополнительных библиотек. 

Вы можете скачать пробную версию этого коммерческого продукта по адресу _https://www.devart.com/litedac/download.html (LiteDAC 4.4 for Lazarus (FreePascal)) 

### Встроенный SQLDB

FPC/Lazarus предлагает встроенные компоненты SQLDB, которые включают поддержку баз данных SQLite ([TSQLite3Connection](<../en/TSQLite3Connection.md> "TSQLite3Connection")) с [вкладки SQLdb](<../en/SQLdb_tab.md> "SQLdb tab") в [Палитре компонентов](<../en/Component_Palette.md> "Component Palette"), которые позволяют, например, создавать графические интерфейсы пользователя с такими компонентами базы данных, как [TDBGrid](<../en/TDBGrid.md> "TDBGrid"). Преимущество использования SQLDB заключается в том, что довольно легко перейти на другую базу данных, такую ​​как Firebird или PostgreSQL, без особых изменений в программе. Подробнее см. ниже. 

#### Поддержка Spatialite

Spatialite - это ГИС-расширения SQLite, которые можно использовать из SQLDB. См. [Spatialite](<../en/Spatialite.md> "Spatialite"). 

#### Поддержка шифрования SQLite

В последних версиях FPC (реализованных в марте 2012 г.) SQLDB включал поддержку некоторых расширенных версий SQLite3, которые шифруют файл базы данных SQLite с использованием алгоритма AES. Используйте свойство пароля, чтобы установить ключ шифрования. 

Примеры: 

  * [SQLCipher](<http://sqlcipher.net/>): Открытый исходный код, например Бинарные файлы Windows не бесплатно (их нужно скомпилировать самостоятельно)
  * [System.Data.SQLite](<http://system.data.sqlite.org/>): с открытым исходным кодом, доступны двоичные файлы Windows (32, 64, CE), загрузите, например, один из предварительно скомпилированных двоичных файлов и переименуйте SQLite.Interop.dll в sqlite3.dll (если вы используете статически связанные файлы, вероятно, вам нужно переименовать System.Data.SQLite.DLL в sqlite3.dll)
  * [wxSQLite3](<http://wxcode.sourceforge.net/docs/wxsqlite3/>): открытый исходный код, доступны некоторые бинарные файлы для Linux (напр: <https://launchpad.net/ubuntu/oneiric/+package/libwxsqlite3-2.8-0>)
  * [sqleet](<https://github.com/resilar/sqleet>): открытый исходный код, нет зависимостей от других библиотек, легко собирается при помощи GCC в исполняемые файлы и файлы библиотек, кроссплаформенный. Доступны механизмы шифрования как в интерактивном режиме, так и посредством sqleet API.



#### sqlite3backup

sqlite3backup - это модуль, поставляемый с FPC (не в Lazarus, но может использоваться программно), который обеспечивает функции резервного копирования/восстановления для SQLite3. Он использует SQLDB sqlite3conn. 

### Zeos

[Zeos](<https://sourceforge.net/projects/zeoslib/>)

### SQLitePass

Компоненты [SQLitePass](<http://source.online.free.fr/>). Последнее обновление кода в 2010. Последняя активность на форуме в 2011. 

### TSQLite3Dataset и TSQLiteDataset

Существуют также отдельные пакеты TSQLiteDataset (модуль sqlite) и TSQLite3Dataset (модуль sqlite3ds); см. ниже описание того, как их использовать. Посетите [домашнюю страницу sqlite4fpc](<http://sqlite4fpc.yolasite.com/>), чтобы найти справочник по API и другие руководства. 

TSqliteDataset и TSqlite3Dataset являются потомками [TDataSet](<../en/TDataSet.md> "TDataSet"), которые обращаются к базам данных sqlite 2.8.x и 3.x.x соответственно. Для новых проектов вы, вероятно, будете использовать TSQlite3Dataset, поскольку текущая версия SQLite 3.x. 

Ниже приведен список основных преимуществ и недостатков по сравнению с другими драйверами/методами доступа FPC/Lazarus SQLite: 

Преимущества: 

  * Гибкость: программисты могут выбирать, использовать или не использовать язык SQL, что позволяет им работать с простыми макетами таблиц или любым сложным макетом, который позволяет SQL/sqlite.



Недостатки: 

  * Переход на другие базы данных сложнее, чем при использовании компонентов SQLDB или Zeos



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Учитывая вышеизложенное, многие пользователи будут использовать SQLDB или Zeos из-за преимуществ, если им не нужен низкоуровневый доступ к библиотеке SQLite.

## Использование компонентов SQLdb со SQLite

Эти инструкции сосредоточены на особенностях SQLDB (TSQLite3Connection) для SQLite. Для общего обзора ознакомьтесь с [SqlDB: Howto](<../en/SqlDBHowto.md> "SqlDBHowto"), где есть полезная информация о компонентах SQLdb. 

См. [ Учебник1 по SQLdb](<../en/SQLdb_Tutorial1.md> "SQLdb Tutorial1") для обучения создания программы с поддержкой базы данных с графическим интерфейсом пользователя, которая написана для SQLite/SQLDB (а также для Firebird/SQLDB, PostgreSQL/SQLDB, в основном любой СУБД, поддерживаемой SQLDB). 

Мы будем использовать комбинацию из трех компонентов из вкладки Lazarus SQLdb: TSQLite3Connection, TSQLTransaction и TSQLQuery. TSQLQuery действует как наш TDataset; в простейшем случае это просто одна из наших таблиц. Для простоты: убедитесь, что у вас уже есть существующий файл базы данных SQLite, и вам не нужно создавать новый сейчас. TSQLite3Connection можно найти в модуле sqlite3conn, если вы хотите объявить его самостоятельно или работаете в FreePascal. 

Эти три компонента связаны друг с другом как обычно: в TSQLQuery установите свойства Database и Transaction, в TSQLTransaction установите свойство Database. В компонентах Transaction и Connection особо нечего делать, большинство интересного будет сделано в TSQLQuery. Настройте компоненты следующим образом: 

**TSQLite3Connection** : 

  * DatabaseName: установите для этого свойства имя файла (абсолютный путь!) вашего файла SQLite. К сожалению, вы не можете просто использовать относительный путь, который работает без изменений во время разработки и во время выполнения (_это все еще верно? Разве вы не можете просто скопировать файл db в сценарий оболочки после сборки или создать символическую ссылку на него?_). Вы должны убедиться, что при запуске приложения правильный путь к файлу всегда устанавливается программно, независимо от того, что он содержал во время разработки.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Чтобы установить полный путь к библиотеке (если вы поместите свою sqlite dll/so/dylib в место, где ОС ее не найдет, например каталог приложения в Linux/OSX), вы можете установить «SQLiteLibraryName» свойство (ДО того, как будет установлено какое-либо соединение, например, в событии OnCreate главной формы)

, например: 
    
    
    SQLiteLibraryName:='./sqlite3.so';
    

**TSQLQuery** : 

  * SQL: задайте для него простой запрос выбора для одной из ваших таблиц. Например, если у вас есть таблица 'foo' и вы хотите, чтобы этот набор данных представлял эту таблицу, просто используйте следующие: 
        
        SELECT * FROM foo
        



  * Active: установите для него значение True в среде IDE, чтобы проверить, все ли настроено правильно. Это также автоматически активирует транзакцию и объекты подключения. Если вы получаете сообщение об ошибке, то либо DatabaseName соединения неверно, либо запрос SQL неверен. Позже, когда мы закончим добавление полей (см. ниже), снова установим их все в неактивные состояния, мы не хотим, чтобы среда IDE блокировала базу данных SQLite (для одного пользователя!) при тестировании приложения.


  * _Наверное, не нужно для правильной работы - нужно будет проверить (June 2012)_ Теперь мы можем добавить поля в наш TSQLQuery. Пока компоненты все еще активны, щелкните правой кнопкой мыши и «отредактируйте поля ...». Нажмите кнопку «+» и добавьте поля. В нем будут перечислены все поля, возвращенные вашим SQL-запросом. Добавьте все поля, которые вам понадобятся, вы также можете добавить сюда поля поиска; в этом случае просто убедитесь, что вы уже определили все необходимые поля в других наборах данных, прежде чем начинать добавлять поля поиска, которые ссылаются на них. Если в вашей таблице много столбцов, и они вам не нужны, вы можете просто не указывать их, а также сделать свой SQL более конкретным.


  * В вашем коде вам нужно вызвать SQLQuery.ApplyUpdates и SQLTransaction.Commit, события TSQLQuery.AfterPost и AfterInsert - хорошее место для этого при использовании его с элементами управления с учетом данных, но, конечно, вы также можете отложить эти вызовы на более позднее время. Если вы не вызовете их, база данных не будет обновлена.


  * "Database is locked" (База данных заблокирована): IDE может по-прежнему блокировать базу данных (SQLite - это база данных для одного пользователя), вы, вероятно, забыли установить компоненты в неактивное состояние и снова отключить их после того, как вы закончили определение всех полей ваших объектов TSQLQuery. Используйте событие OnCreate формы, чтобы задать путь и активировать объекты только во время выполнения. Большинство вещей, которые вы устанавливаете в TSQLQuery из среды IDE, не требуют (а некоторые даже не позволяют), чтобы они были активными во время разработки, единственным исключением является определение полей, в которых он хочет прочитать дизайн таблицы, поэтому неактивность во время разработки должно быть нормальным состоянием.


  * Все ваши таблицы должны иметь первичный ключ, и вы должны убедиться, что соответствующее поле имеет pfInKey и ничего больше в его PoviderFlags (эти флаги управляют тем, как и где используется поле при автоматическом построении запросов на обновление и удаление).


  * Если вы используете lookup-поля 
    * убедитесь, что ProviderFlags для lookup-поля полностью пуст, чтобы он не пытался использовать свое имя в запросе на обновление. Само поле поиска не является полем данных, оно действует только на значение другого поля, соответствующего ключевого поля, и только это ключевое поле будет позже использоваться в запросах на обновление. Вы можете установить ключевое поле как скрытое, потому что обычно вы не хотите видеть его в своей DBGrid, но его необходимо определить.
    * LookupCache должен иметь значение True. На момент написания этой статьи по какой-то причине в -lookupполе ничего не отображается (но все равно работает), и, как ни странно, происходит полная противоположность при работе с TSQLite3Dataset или другими компонентами TXXXDataset, здесь должно быть установлено значение False. Я еще не уверен, является ли это предполагаемым поведением или ошибкой.


  * Обычно для простых таблиц вам не нужно устанавливать какие-либо свойства InsertSQL, UpdateSQL и DeleteSQL, просто оставьте их пустыми. Если у вас правильно установлены ProviderFlags всех ваших полей, он сможет создать необходимый SQL на лету. Подробнее об InsertSQL, UpdateSQL и DeleteSQL см. [Работа с TSQLQuery](<../en/Working_With_TSQLQuery.md> "Working With TSQLQuery").



После того, как все вышеперечисленное будет настроено правильно, вы сможете использовать TSQLQuery, как и любой другой TDataset, либо программно манипулируя его данными, либо помещая TDatasouce в форму, подключая его к TSQLQuery, а затем используя контуры данных, такие как TDBGrid и т.п. 

### Creating a Database

Метод [TSQLite3Connection.CreateDB](<http://www.freepascal.org/docs-html/fcl/sqldb/tsqlconnection.createdb.html>), унаследованный от родительского класса, на самом деле ничего не делает; чтобы создать базу данных, если файл еще не существует, вам просто нужно записать данные таблицы, как в следующем примере: 

(Код, извлеченный из примера sqlite_encryption_pragma, который поставляется с Lazarus 1.3 и новее) 
    
    
    var
      newFile : Boolean;
    begin
    
      SQLite3Connection1.Close; // Убедитесь, что соединение закрыто при запуске
    
      try
      //Поскольку мы делаем эту базу данных впервые,
      // проверяем, существует ли уже файл    
      newFile := not FileExists(SQLite3Connection1.DatabaseName);
    
        if newFile then
        begin
          // Создаем базу данных и таблицы
          try
            SQLite3Connection1.Open;
            SQLTransaction1.Active := true;
    
            // Здесь мы настраиваем таблицу с именем "DATA" в новой базе данных.
            SQLite3Connection1.ExecuteDirect('CREATE TABLE "DATA"('+
                        ' "id" Integer NOT NULL PRIMARY KEY AUTOINCREMENT,'+
                        ' "Current_Time" DateTime NOT NULL,'+
                        ' "User_Name" Char(128) NOT NULL,'+
                        ' "Info" Char(128) NOT NULL);');
    
            // Создание индекса на основе идентификатора в таблице "DATA"
            SQLite3Connection1.ExecuteDirect('CREATE UNIQUE INDEX "Data_id_idx" ON "DATA"( "id" );');
    
            SQLTransaction1.Commit;
    
            ShowMessage('База данных успешно создана.');
          except
            ShowMessage('Невозможно создать новую базу данных');
          end;
        end;
      except
        ShowMessage('Невозможно проверить, существует ли файл базы данных');
      end;
     end;
    

### Создание определяемых пользователем порядка сортировки
    
    
    // utf8-callback функция сравнения с учетом регистра
    function UTF8xCompare(user: pointer; len1: longint; data1: pointer; len2: longint; data2: pointer): longint; cdecl;
    var S1, S2: AnsiString;
    begin
      SetString(S1, data1, len1);
      SetString(S2, data2, len2);
      Result := UnicodeCompareStr(UTF8Decode(S1), UTF8Decode(S2));
    end;
    
    // utf8-callback функция сравнения без учета регистра
    function UTF8xCompare_CI(user: pointer; len1: longint; data1: pointer; len2: longint; data2: pointer): longint; cdecl;
    var S1, S2: AnsiString;
    begin
      SetString(S1, data1, len1);
      SetString(S2, data2, len2);
      Result := UnicodeCompareText(UTF8Decode(S1), UTF8Decode(S2));
    end;
    
    // регистрируем порядок сортировки с помощью SQLite3 API (требуется модуль sqlite3dyn):
    sqlite3_create_collation(SQLite3.Handle, 'UTF8_CI', SQLITE_UTF8, nil, @UTF8xCompare_CI);
    // или используем метод TSQLite3Connection:
    CreateCollation('UTF8_CI',1,nil,@UTF8xCompare_CI);  
    
    // теперь мы можем использовать порядок сортировки без учета регистра в SQL, например:
    // SELECT * FORM table1 WHERE column1 COLLATE UTF8_CI = 'á'
    
    // но это не работает для оператора LIKE
    // чтобы поддерживать также оператор LIKE, мы должны перегрузить функцию LIKE по умолчанию, используя sqlite3_create_function ()
    // http://www.sqlite.org/lang_corefunc.html#like
    

### Создание пользовательских функций (UDF)
    
    
    // пример перегрузки функции LOWER() по умолчанию на функцию, предоставляемую пользователем
    // чтобы запустить эту демку, вы должны добавить модули sqlite3dyn и ctypes в ваше предложение uses
    // и добавить константу 'SQLITE_DETERMINISTIC' со значением $800
    
    procedure UTF8xLower(ctx: psqlite3_context; N: cint; V: ppsqlite3_value); cdecl;
    var S: AnsiString;
    begin
      SetString(S, sqlite3_value_text(V[0]), sqlite3_value_bytes(V[0]));
      S := UTF8Encode(AnsiLowerCase(UTF8Decode(S)));
      sqlite3_result_text(ctx, PAnsiChar(S), Length(S), sqlite3_destructor_type(SQLITE_TRANSIENT));
    end;
    
    // регистрируем функцию LOWER() с помощью SQLite3 API (требуется модуль sqlite3dyn):
    sqlite3_create_function(SQLite3.Handle, 'lower', 1, SQLITE_UTF8 or SQLITE_DETERMINISTIC, nil, @UTF8xLower, nil, nil);
    

### SQLite3 и даты

  * SQLite 3 не хранит даты как специальное значение DateTime. Он может хранить их в виде строк, чисел двойной точности или целых чисел - см. [Date and Time Datatype](<http://www.sqlite.org/datatype3.html#datetime>).
  * В строках разделителем даты является «-» в соответствии со стандартом SQL/ISO 8601. Таким образом, если вы выполняете INSERT с использованием встроенной функции DATE, она сохранит его как что-то вроде «ГГГГ-ММ-ДД».
  * Чтение значения DateTime может вызвать проблемы для DataSet, если они хранятся в виде строк: квалификатор .AsDateTime может останавливаться на "строковой дате" SQLite, но это можно преодолеть, используя что-то вроде `strftime(_%d/%m/%Y_ ,recdate) AS sqlite3recdate` в вашем операторе SQL SELECT, который заставляет SQLite3 возвращать запись даты в указанном формате (строка формата %d/%m/%d соответствует вашему региональному формату даты, который будет пониматься как .AsDateTime) **== > Пожалуйста, откройте отчет об ошибке с примером приложения, демонстрирующим проблему, если это так**
  * При сравнении дат, хранящихся в виде строк (используя, например, функцию BETWEEN), помните, что сравнение всегда будет **строковым** сравнением и, следовательно, будет зависеть от того, как вы сохранили значение даты.



#### Значения по умолчанию для местного времени вместо UTC

CURRENT_TIME, CURRENT_DATE и CURRENT_TIMESTAMP возвращают текущую дату и/или время в формате UTC. Для местной даты и/или времени мы можем использовать: 
    
    
     DEFAULT (datetime('now','localtime')) for datetime values formated YYYY-MM-DD HH:MM:SS
     DEFAULT (date('now','localtime')) for date value formated YYYY-MM-DD
     DEFAULT (time('now','localtime')) for time value formated HH:MM:SS
    

### Устранение неполадок SQLDB и SQLite

  * Имейте в виду, что для поддержки работы времени разработки (поля и т.д.) Lazarus также должен найти sqlite3.dll ([прим.перев](</User:Zoltanleo> "User:Zoltanleo"): положите этот файл в корень IDE рядом с экзешником. Обратите внимание, что разрядность библиотеки должна соответствовать разрядности Лазаруса).
  * То же самое и с именем файла базы данных. Всегда используйте абсолютный путь, если вы используете компоненты для извлечения, например, имена полей во время разработки. В противном случае IDE создаст в своем каталоге пустой файл. В случае возникновения проблем проверьте, не содержит ли каталог lazarus/ нулевую байтовую копию файла базы данных.
  * Если у вас есть отношения master/detail, вам необходимо обновлять главный набор данных после каждой вставки, чтобы получить значение для поля внешнего ключа подчиненного набора данных. Это можно сделать в событии AfterPost основного набора данных, вызвав одну из следующих перегруженных процедур:


    
    
    interface
        procedure RefreshADatasetAfterInsert(pDataSet: TSQLQuery);overload;
        procedure RefreshADatasetAfterInsert(pDataSet: TSQLQuery; pKeyField: string);overload;  
     
    implementation
     
    procedure RefreshADatasetAfterInsert(pDataSet: TSQLQuery; pKeyField: string);
    // Эта процедура обновляет набор данных и помещает курсор на последнюю запись
    // Используется, если Dataset не гарантирует сортировку набора данных по первичному ключу автоинкремента
    var
      vLastID: Integer;
      vUpdateStatus : TUpdateStatus;
    begin
      vUpdateStatus := pDataset.UpdateStatus;
      //Получаем последний вставленный ID в базе данных 
      pDataset.ApplyUpdates;
      vLastID:=(pDataSet.DataBase as TSQLite3Connection).GetInsertID;
      //Теперь возвращаемся к соответствующей строке
      if vUpdateStatus = usInserted then begin
        pDataset.Refresh;
        //Обновляемся и возвращаемся к соответствующей строке
        pDataset.Locate(pKeyField,vLastID,[]);
      end;
    end;
     
    procedure RefreshADatasetAfterInsert(pDataSet: TSQLQuery);
    //Эта процедура обновляет набор данных и помещает курсор на последнюю запись
    //Используется только в том случае, если DataSet гарантированно отсортирован по первичному ключу с автоинкрементом.
    var
      vLastID: Integer;
      vUpdateStatus : TUpdateStatus;
    begin
      vUpdateStatus := pDataset.UpdateStatus;
      pDataset.ApplyUpdates;
      vLastID:=(pDataSet.DataBase as TSQLite3Connection).GetInsertID;
      if vUpdateStatus = usInserted then begin
        pDataset.Refresh;
        //Опасно!
        pDataSet.Last;
      end;
    end;
    
    procedure TDataModule1.SQLQuery1AfterPost(DataSet: TDataSet);
    begin
      RefreshADatasetAfterInsert(Dataset as TSQLQuery); //Если ваш набор данных отсортирован по первичному ключу
    end;  
    
    procedure TDataModule1.SQLQuery2AfterPost(DataSet: TDataSet);
    begin
      RefreshADatasetAfterInsert(Dataset as TSQLQuery, 'ID'); //если вы не уверены, что набор данных всегда сортируется по первичному ключу
    end;
    

### Vacuum и другие операции, которые необходимо выполнять вне транзакции

Кажется, что SQLDB всегда требует подключения, но некоторые операции, такие как Pragma и Vacuum, должны Быть сделано вне транзакции. Хитрость заключается в том, чтобы завершить транзакцию, выполнить то, что вы должны, и начать транзакцию еще раз (чтобы не запутать sqldb): 
    
    
      // коммит любых ожидающих операций или использование «свежего» соединения sql
      Conn.ExecuteDirect('End Transaction');  // Завершение транзакции, начатой SQLdb
      Conn.ExecuteDirect('Vacuum');
      Conn.ExecuteDirect('Begin Transaction'); //Запуск транзакции для использования SQLdb
    

## Использование TSQLite3Dataset

В этом разделе подробно описано, как использовать компоненты TSQLite2Dataset и TSQLite3Dataset для доступа к базам данных SQlite (от Luiz Américo luizmed(at)oi(dot)com(dot))br 

  


#### Требования

  * Для баз данных sqlite2 (legacy): 
    * FPC 2.0.0 или выше
    * Lazarus 0.9.10 или выше
    * SQLite runtime библиотеки 2.8.15 или выше*


  * Sqlite2 больше не поддерживается, и двоичный файл не может быть найден на сайте sqlite


  * Для баз данных sqlite3: 
    * FPC 2.0.2 или выше
    * Lazarus 0.9.11 (svn revision 8443) или выше
    * sqlite runtime библиотеки 3.2.1 или выше (можно получить с [www.sqlite.org](<http://www.sqlite.org>))



**Перед запуском проекта lazarus убедитесь, что:**

  * библиотека sqlite либо 
    * прописана в системной переменной PATH или
    * находится в каталоге вывода исполняемых файлов и в каталогах Lazarus (или текущего проекта) - этот параметр может работать только в Windows
  * в Linux поместите `cmem` в качестве первого модуля в разделе `uses` основной программы 
    * В Debian, Ubuntu и других подобных Debian дистрибутивах для создания Lazarus IDE необходимо установить пакеты libsqlite-dev/libsqlite3-dev, а не только sqlite/sqlite3 (также относится к OpenSuSe)



#### Как использовать (базовое использование)

Установить пакет, который находится в каталоге /components/sqlite (см. инструкции [здесь](<Install_Packages.md> "Install Packages/ru")) ([прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): на момент перевода статьи это можно сделать через стандартный установщик пакетов Лазаруса из меню Package(Пакеты) --> Install/Uninstall Package (Установка/Удаление пакетов) --> справа в окне Available for installation (Доступные для установки) выбираем sqlite3laz 0.4 --> ниже жмем кнопку Install Selection (Установить выбранное) --> жмем в самом низу кнопку Save and rebuild IDE (Сохранить и пересобрать IDE)). 

Во время разработки установите следующие свойства: 

  * FileName: путь к файлу sqlite [обязательно]
  * TableName: имя таблицы, используемой в операторе sql [обязательно]
  * SQL: оператор выбора SQL [по желанию]
  * SaveOnClose: значение по умолчанию - false, что означает, что изменения не сохраняются. Его можно изменить на true. [по желанию]
  * Active: необходимо установить во время разработки или при запуске программы. [обязательно]



**Создание Table (Dataset)**

Дважды щелкните значок компонента или используйте пункт 'Create Table'(Создать таблицу) во всплывающем меню, которое появляется при нажатии правой кнопки мыши. Будет показан простой редактор таблиц с пояснениями. 

Вот все типы полей, поддерживаемые TSqliteDataset и TSqlite3Dataset: 

  * Integer
  * AutoInc
  * String
  * Memo
  * Bool
  * Float
  * Word
  * DateTime
  * Date
  * Time
  * LargeInt
  * Currency



**Получение данных**

После создания таблицы или с помощью ранее созданной таблицы откройте набор данных с помощью метода Open. Если свойство SQL не было установлено, будут извлечены все записи из всех полей, то же самое можно получить, если вы зададите SQL следующим образом: 
    
    
    SQL := 'Select * from TABLENAME';
    

**Применение изменений к базовому файлу данных**

Чтобы использовать функцию ApplyUpdates, набор данных должен содержать по крайней мере одно поле, которое удовлетворяет требованиям для первичного ключа (значения должны быть UNIQUE, а не NULL) 

Сделать это можно двумя способами: 

  * Установить свойство PrimaryKey на имя поля первичного ключа
  * Добавить поле AutoInc (это проще, поскольку TSqliteDataSet автоматически обрабатывает его как первичный ключ)



Если установлено одно из двух условий, просто вызовите 
    
    
    ApplyUpdates;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Если заданы оба условия, поле, соответствующее PrimaryKey, используется для применения обновлений.

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Установка PrimaryKey в поле, которое не является первичным ключом, приведет к потере данных при вызове ApplyUpdates, поэтому перед его использованием убедитесь, что выбранное поле не содержит значений Null и Unique.

##### Пример Master/detail

Различные примеры отношений master/detail (например, отношения между клиентом и заказами): 

  * общий способ SQLDB для этого с использованием свойства `DataSource`: [MasterDetail](<../en/MasterDetail.md> "MasterDetail")
  * Конкретный пример TSQLite3 с использованием метода `locate`: [пример TSqlite3 Master Detail](<../en/TSqlite3_Master_Detail_Example.md> "TSqlite3 Master Detail Example").



#### Примечания

  * Несмотря на то, что TSqliteDataset был протестирован с 10 000 записей и работал нормально, все данные хранятся ([прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): кешируются) в памяти, поэтому не забывайте извлекать только необходимые данные (особенно с полями Memo).
  * Один и тот же файл данных (свойство Filename) может содержать несколько таблиц/наборов данных.
  * Несколько наборов данных (разные комбинации полей) могут быть созданы с использованием одной и той же таблицы одновременно
  * Можно фильтровать данные с помощью операторов WHERE в sql, закрывая и повторно открывая набор данных (или вызывая метод RefetchData). Но в этом случае порядок и количество полей должны оставаться прежними.
  * Также можно использовать сложные операторы SQL, используя псевдонимы, объединения, представления в нескольких таблицах (помните, что они должны находиться в одном файле данных), но в этом случае ApplyUpdates не будет работать. Если кто-то хочет использовать сложные запросы и применить обновления к файлу данных, напишите мне, и я дам несколько советов, как это сделать.
  * Установка имени файла в файл данных sqlite, не созданный TSqliteDataset, и его открытие разрешено, но в некоторых полях не будет обнаружен правильный тип поля. Они будут рассматриваться как строковые поля.



Общие примеры можно найти в SVN каталоге fpc/fcl-db/src/sqlite. 

## See also

  * [SQLdb_Tutorial1](<../en/SQLdb_Tutorial1.md> "SQLdb Tutorial1") Учебник по Sqlite
  * [SqlDBHowto](<../en/SqlDBHowto.md> "SqlDBHowto") Общая информация о SQLDB
  * [Working With TSQLQuery](<../en/Working_With_TSQLQuery.md> "Working With TSQLQuery") Информация о SQLDB's TSQLQuery
  * [SQLite site](<http://www.sqlite.org>)
  * [TSQLConnection](<https://www.freepascal.org/docs-html/fcl/sqldb/tsqlconnection.html>)

---

_Source: [https://wiki.freepascal.org/SQLite/ru](https://web.archive.org/web/20240516115455/https://wiki.freepascal.org/SQLite/ru)_
