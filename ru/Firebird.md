# Firebird

│ **[English (en)](<../en/Firebird.md>)** │  **русский (ru)** │

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
  
Это руководство по использованию базы данных **Firebird** в Lazarus/FPC, с использованием Firebird с SQLDB (встроенная библиотека базы данных FPC/Lazarus). Другие методы доступа описаны в разделе [Другие библиотеки Firebird](<../en/Other_Firebird_libraries.md> "Other Firebird libraries"). 

## Contents

  * 1 Обзор
  * 2 Документация
  * 3 Установка режимов клиент/сервер и встроенного режима сервера
    * 3.1 Windows
    * 3.2 Unix/Linux/macOS
  * 4 Примеры подключения
  * 5 Устранение проблем с доступом client/server
  * 6 Мониторинг событий
  * 7 Программное создание объектов
  * 8 Администрирование базы данных
  * 9 Общие проблемы и решения
    * 9.1 Попытка обновления доступного только для чтения столбца / полей COMPUTED BY
    * 9.2 Bigint: потеря точности
    * 9.3 Логические типы данных
    * 9.4 Проблемы INSERT INTO...RETURNING/Не открытый курсор
    * 9.5 Locate, кажется, не работает
  * 10 Подробнее о транзакциях
    * 10.1 Уровни изоляции транзакций
    * 10.2 Режим доступа
    * 10.3 Обработка конфликтов блокировки
    * 10.4 Резервирование таблиц
    * 10.5 Версионность записей
    * 10.6 Различные варианты
    * 10.7 Firebird и транзакции по стандарту ANSI/ISO SQL
    * 10.8 Общие комбинации
      * 10.8.1 Пакетная/массовая вставка
      * 10.8.2 Транзакции только для чтения
  * 11 Ссылки и дополнительная информация
    * 11.1 Примеры Lazarus Firebird
    * 11.2 Инструменты администрирования
    * 11.3 Firebird



## Обзор

Firebird - это бесплатный сервер баз данных с открытым исходным кодом, который использовался и разрабатывался десятилетиями (он разрабатывался на основе базы данных Interbase 6, исходные коды которой были открыты компанией Borland). Он включает в себя расширенную поддержку операторов SQL (например, INSERT...RETURNING), хранимых процедур, триггеров и т.д. Вы можете написать скомпилированные библиотеки UDF (библиотеки пользовательских функций) для сервера в FreePascal, если вы хотите расширить и без того обширный список функций Firebird. 

База данных требует очень мало ручной работы администратора баз данных после того, как она настроена, что делает ее идеальной для использования в малом бизнесе или для встроенного использования. При правильной настройке он может вырасти до терабайтного масштаба, хотя PostgreSQL может быть лучшим выбором для таких больших сред. 

Firebird предлагает как встроенную (основанную на файлах), так и клиент-серверную базу данных - которую можно использовать без необходимости изменять одну строку кода в FPC/Lazarus. При использовании в качестве встроенной базы данных она предлагает более богатую поддержку SQL, чем [SQLite](<../en/SQLite.md> "SQLite"), а также плавную миграцию в базу данных клиент-сервер, хотя SQLite сама по себе является вполне встроенной базой данных. 

Последняя стабильная версия, Firebird 2.5 ([прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): на момент перевода статьи - это версия Firebird 3.0.4), работает в Windows (32- и 64-разрядная версия), различных версиях Linux (32- и 64-разрядная версия), Solaris (Sparc и Intel), HP-UX (PA-Risc) и macOS. 

В настоящее время сервер портируется на Android; он пока недоступен в Windows CE/Windows Mobile. 

Поддержка Firebird в SQLDB компилятора довольно хорошая, сравнимая с уровнем поддержки [PostgreSQL](<../en/postgres.md> "postgres"). 

## Документация

Официальная документация включена в FPC 2.6.2+: [Документация SQLDB для IBConnection](<http://www.freepascal.org/docs-html/fcl/ibconnection/index.html>)

## Установка режимов клиент/сервер и встроенного режима сервера

Firebird может работать в режиме клиент/сервер и в режиме встроенного сервера. 

_Client/Server_ означает, что у вас где-то есть физический сервер Firebird: либо на локальной машине, либо на другой машине, доступной по вашей сети. Соединения с сервером проходят через TCP/IP; при указании соединения имя хоста содержит имя или IP-адрес. Серверу Firebird для "общения" с вашими компонентами доступа необходима _fbclient.dll/fbclient.so.nnn_ (_вместе с его файлами поддержки_). 

_Embedded Firebird_ означает, что ваше приложение для доступа к базе данных Firebird на локальной машине загружает единственную библиотеку, которая является и сервером, и клиентской библиотекой Firebird "в одном флаконе". При указании строки соединения имя хоста всегда пусто (не указывается). Серверу Firebird для "общения" с вашими компонентами доступа необходима _fbembed.dll/fbembed.so_ (_вместе с его файлами поддержки_). См. [вики-страницу на Firebird embedded](<../en/Firebird_embedded.md> "Firebird embedded") для более подробной информации. 

Обратите внимание, что _fbembed.dll_ может использоваться как для клиент-серверного, так и для встроенного использования, поэтому установка только этой dll может быть разумной задачей. 

* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): необходимо уточнить, что выше описанное справедливо для версий FB 1.5, FB 2.0, FB 2.1 и FB 2.5. Начиная с версии FB 3.0, режим используемого сервера зависит от строки подключения, а клиентская библиотека на все случаи жизни одна - все та же _fbclient.dll/libfbclient.so.3.0.n_ (где n - номер билда версии FB). 

* * *

### Windows

Win64: пожалуйста, см. предупреждение [здесь](<../en/Windows_Programming_Tips.md> "Windows Programming Tips") о нежелательном использовании некоторых версий FPC/Lazarus Win64. 

Для Windows: (это относится ко всем драйверам базы данных SQLDB) необходимо иметь _fbclient.dll_ (или _fbembed.dll'_ ) и его поддержку dll, установленных в: 

  * каталог проекта **и** каталог вывода исполняемого файла/каталог приложения (например, lib /что-то еще в каталоге вашего проекта)
  * или каталог в вашем PATH (не в системном каталоге)
  * Если вы хотите использовать системный каталог, пожалуйста, используйте официальный установщик и поставьте галочку «скопировать fbclient в системный каталог»



Как и для всех библиотек DLL (базы данных), разрядность библиотеки DLL должна соответствовать вашему приложению: используйте 32-разрядную библиотеку для 32-разрядной скомпилированной программы и 64-разрядную библиотеку для 64-разрядной программы. 

### Unix/Linux/macOS

В Linux/macOS/FreeBSD должна быть установлена ​​клиентская библиотека Firebird (например, вашим менеджером пакетов; установите обычный пакет и пакет -dev), или они должны быть прописаны в путях поиска библиотеки. 

FPC ищет наиболее распространенные имена библиотек (например, libfbclient.so.2.5, libgds.so и libfbembed.so.2.5; пожалуйста, проверьте ibase60.inc, если ваша версия отличается). При желании можно указать имя библиотеки. Есть 2 способа сделать это: 

  * использовать компонент [TSQLDBLibraryLoader](<../en/TSQLDBLibraryLoader.md> "TSQLDBLibraryLoader") из sqldblib (FPC 2.7.1). Работает для всех компонентов коннектора SQLDB.
  * вызвать функцию 
        
        function InitialiseIBase60(Const LibraryName : AnsiString) : integer;
        

с правильным именем библиотеки (для этого может потребоваться использовать модуль ibase60dyn).



## Примеры подключения

1\. Пример для client/server: 
    
    
    Hostname: 192.168.1.1
    * База данных находится на сервере с IP-адресом 192.168.1.1.
    DatabaseName: /interdata/example.fdb  
    * Имя файла базы данных - "example.fdb" в каталоге /interdata сервера (машина с IP-адресом 192.168.1.1).
    Username: SYSDBA
    Password: masterkey
    

2\. Другой пример client/server: 
    
    
    Hostname: dbhost
    * База данных находится на сервере с именем хоста dbhost
    DatabaseName: F:\Program Files\firebird\examples\employee.fdb  
    * Имя файла базы данных - "employee.fdb" в каталоге Program Files\firebird\examples на диске F: dbhost.
    Username: SYSDBA
    Password: masterkey
    

3\. Пример для embedded-режима: 
    
    
    Hostname: <пустая строка>
    * Оставляя имя хоста пустым, выбирается embedded-режим.
    DatabaseName: test.fdb
    * Файл базы данных "test.fdb" находится в каталоге запускаемого приложения (убедитесь, что fbembed.dll находится в каталоге исполняемого файла приложения)
    Username: SYSDBA
    * Для встроенного сервера вы должны указать имя пользователя ...
    Password: <пустая строка> ([прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): вообще-то, это может быть любая строка, в т.ч. и пустая)
    * ... но не важно, какой пароль вы укажете.
    

* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): выше приведенные примеры подразумевают, что используется порт по умолчанию 3050. Если номер порта отличается от стандартного (например, пусть будет 3051), то для каждого из выше приведенных примеров строка подключения (например, к утилите _isql_) соответственно будет выглядеть следующим образом: 
    
    
    SQL>connect '192.168.1.1/3051:/interdata/example.fdb' -user 'SYSDBA' -password 'masterkey';
    
    
    
    SQL>connect 'dbhost/3051:F:\Program Files\firebird\examples\employee.fdb' -user 'SYSDBA' -password 'masterkey';
    
    
    
    SQL>connect 'test.fdb' -user 'SYSDBA' -password '';
    

  
Начиная с версии FB 3 появилась возможность указывать другие протоколы коннекта: 

  * **INET** -протокол (для TCP/IP) в виде строки `<host>[/<port>]:<database file path or alias>`



Тогда строка подключения (например, к утилите _isql_) для выше указанных примеров соответственно будет выглядеть так: 
    
    
    SQL>connect 'inet://192.168.1.1:3051:/interdata/example.fdb' -user 'SYSDBA' -password 'masterkey';
    
    
    
    SQL>connect 'inet://dbhost:3051:F:\Program Files\firebird\examples\employee.fdb' -user 'SYSDBA' -password 'masterkey';
    
    
    
    SQL>connect 'inet://test.fdb' -user 'SYSDBA' -password '';
    

INET-протокол поддерживает _URL-стиль_ (для адресов IPv4 и IPv6) строки подключения вы виде шаблона `[<protocol>://[<host>[:<port>]]]/<database file path or alias>`. Тогда в строке подключения часть строки заменяется с _inet://_ на _inet4://_ или _inet6://_ соответственно. Например: 
    
    
    SQL>connect 'inet6://192.168.1.1:3051:/interdata/example.fdb' -user 'SYSDBA' -password 'masterkey';
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** по умолчанию Firebird прослушивает нулевой адрес IPv6 (: :) и принимает все входящие соединения, будь то IPv4 или IPv6. Если эти адреса имеют ненулевое значение, то в строке подключения они обрамляются квадратными скобками, чтобы устранить двусмысленность при использовании двоеточия в качестве разделителя

. 

Например, если `IPv6 = 2014:1234::5`, то строка подключения примет вид: 
    
    
    SQL>connect '[2014:1234::5]://192.168.1.1:3051:/interdata/example.fdb' -user 'SYSDBA' -password 'masterkey';
    

и далее по аналогии... 

  * протокол NetBEUI, или **WNET** (именованные каналы) - **только для Windows!** \- в виде строки `\\<host>[@<port>]\<database file path or alias>`.
  * протокол **XNET** для локальных подключений с использованием общей памяти.



Последние два протокола имеют строку подключения, аналогичную INET, только вместо _inet://_ подставляются _wnet://_ или _xnet://_ соответственно. 

Более подробную информацию по нововведениям FB 3, в том числе и по протоколам коннекта можно посмотреть [здесь](<http://www.ibase.ru/files/firebird/Firebird_v3.0.3.ReleaseNotes.pdf>). 

* * *

## Устранение проблем с доступом client/server

Убедитесь, что вы запустили сервер Interbase/Firebird на указанном вами IP-адресе/имени хоста сервера. Вы можете проверить подключение, подключившись к компьютеру через telnet. Firebird обычно слушает порт 3050: 
    
    
    telnet 192.168.1.1 3050
    

Вы должны увидеть что-то, может быть, просто пустой экран, но вы можете что-то напечатать. Это означает, что вы можете отправлять данные в базу данных Firebird. 

Для получения дополнительной информации см. документацию Firebird. 

## Мониторинг событий

FPC/Lazarus поставляется с компонентом для мониторинга событий, поступающих из баз данных Firebird; см. [TFBEventMonitor](<../en/TFBEventMonitor.md> "TFBEventMonitor"). 

## Программное создание объектов

Хотя для создания таблиц и т.д. можно использовать такие инструменты, как Flamerobin ([прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): для windows, особенно русскоязычной, предпочтительнее использовать IBExpert), вы также можете создавать объекты БД программно/динамически, что может быть удобно, если вы хотите, чтобы ваши программы обновляли существующие схемы базы данных до новой схемы. 

Вы можете использовать, например, TSQLQuery.ExecSQL для выполнения этой задачи: 
    
    
    Query.ExecSQL('CREATE TABLE TEST(ID INTEGER NOT NULL, TESTNAME VARCHAR(800))');
    // Вам необходимо закомиттить транзакцию после DDL перед любыми инструкциями DML - SELECT, INSERT и т. Д.
    //В противном случае SQL не увидит созданные объекты
    

Используйте для этих целей объект TSQLScript - см. [TSQLScript](<../en/TSQLScript.md> "TSQLScript"). 

## Администрирование базы данных

У FPC/Lazarus есть компонент для администрирования базы данных; см. [TFBAdmin](<../en/TFBAdmin.md> "TFBAdmin")

## Общие проблемы и решения

Иногда использование Firebird в Lazarus кажется сложным. Пожалуйста, поищите решения ниже по тексту. 

### Попытка обновления доступного только для чтения столбца / полей COMPUTED BY

Если в вашей таблице Firebird есть поля COMPUTED BY (вычисляемые поля на стороне сервера), SQLDB не будет брать в расчет, что это поля только для чтения (из соображений производительности). 

В этом случае автоматически сгенерированные операторы INSERTSQL, UPDATESQL могут привести к сообщениям об ошибках, таких как "attempted update of read-only column" (попытка обновления столбца только для чтения).Решение состоит в том, чтобы вручную указать, что данное поле не может быть обновлено после установки свойства SQL в TSQLQuery, что-то вроде: 
    
    
    // Отключаем обновление этого поля или поиск измененных значений, поскольку пользователь не может изменить его
    sqlquery1.fieldbyname('full_name').ProviderFlags:=[];
    

### Bigint: потеря точности

Если вы используете тип данных bigint (64-битное целое число со знаком) в Firebird, пожалуйста, используйте .AsLargeInt вместо .AsInteger для параметров: 
    
    
    // Предполагая, что идентификатор здесь bigint
    sqlquery1.sql.text := 'insert into ADDRESS (ID) values (:ID)';
    // делаем так:
    sqlquery1.params.parambyname('ID').aslargeint := <любая qword- или 64-bit integer-переменная>;
    // Не делайте так:
    //sqlquery1.params.parambyname('ID').asinteger := <любая qword- или 64-bit integer-переменная>;
    ...
    

... в противном случае вы можете получить такие ошибки, как дубликат PK (первичного ключа) - при использовании bigint в качестве первичного ключа. 

### Логические типы данных

Версии Firebird ниже 3.0 не поддерживают логические типы данных. 

По крайней мере, в транке FPC (2.7.1) этот тип данных можно эмулировать: 

Используйте домен, который использует тип SMALLINT (другие целочисленные типы также могут работать - пожалуйста, протестируйте и настройте текст): 
    
    
    CREATE DOMAIN "BOOLEAN"
     AS SMALLINT
     CHECK (VALUE IS NULL OR VALUE IN (-1,0,1))
     /* -1 используется для совместимости с SQLDB FPC; 1 используется многими другими слоями доступа к данным */
    ;
    

  
Позвольте вашему полю/столбцу использовать этот тип домена, например 
    
    
    CREATE TABLE MYTABLE
    (
    ...
      MYBOOLEANCOLUMN "BOOLEAN",
    );
    

  * Теперь вы можете использовать .AsBoolean для присвоения значений полей и т.д.



**To do: убедитесь, что это работает с гридами Lazarus и т. д..**

### Проблемы INSERT INTO...RETURNING/Не открытый курсор

Если вы попытаетесь сделать выборку SQL (например, Query.Open) в следущем SQL-запросе: 
    
    
    INSERT INTO PEOPLE (NICKNAME) VALUES ('Superman') RETURNING ID
    

и получаете что-то вроде этой ошибки: 
    
    
    Database error:  : Fetch :
     -Dynamic SQL Error
     -SQL error code = -504
     -Invalid cursor reference
     -Cursor is not open(error code: 335544569)
    

во время работы FPC 2.6.0 (которая поставляется с Lazarus 1.0) или ниже, вы, вероятно, столкнулись с ошибкой синтаксического парсера FPC SQLDB. 

SQLDB считает, что выполняемый вами оператор является обычным оператором INSERT, который не возвращает данные. Очевидно, он должен возвращать данные. В более новом FPC для этого кода уже внесены исправления. 

Если вы используете генераторы/последовательности для своих первичных ключей (как это делают многие), в качестве обходного пути вы должны сначала получить следующий порядковый номер: 
    
    
    SELECT NEXT VALUE FOR GEN_PEOPLEID FROM RDB$DATABASE /* Если имя вашего генератора GEN_PEOPLEID */
    

затем используйте это, чтобы сделать обычный INSERT. см. [FAQ entry](<http://www.firebirdfaq.org/faq111/%7CFirebird>)

* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): вообще-то, мягко говоря, немного странный совет. Гораздо проще и безопаснее создать в БД триггер BeforeInsert примерно следующего содержания: 
    
    
    /* Trigger: TBL_PEOPLE_BI */
    CREATE OR ALTER TRIGGER TBL_PEOPLE_BI FOR TBL_PEOPLE
    ACTIVE BEFORE INSERT POSITION 0
    AS
    BEGIN
      /* генерируем новое значение поля, если вставляемое является NULL-значением */
      IF (NEW.ID IS NULL) THEN 
        NEW.ID = GEN_ID(GEN_PEOPLEID,1);
    END
    ^
    

чтобы переложить заботу о генерации PK на плечи сервера, передавая с клиента NULL-значение генератора (или не передавая его вообще, что будет восприниматься сервером, как NULL). 

* * *

### Locate, кажется, не работает

Источник: [Issue ##21988](<https://bugs.freepascal.org/view.php?id=#21988>)

При запуске locate в CHAR-полях с UTF8 (или, возможно, других многобайтовых символов), locate может не найти вашу запись. 

Проблема в основном связана с используемой кодировкой UTF8 и тем, как Firebird сообщает о длине столбца. В случае UTF8 Firebird сообщает о максимальной длине столбца (в байтах) как 4 * «размер символа». Поэтому, если у вас есть столбец, определенный как char(8), Firebird сообщает 4*8=32. 

Значения "добиваются" пробелами справа до этой длины 32. При поиске, скажем, «57200001», совпадений нет, потому что в поле фактически хранится «57200001 ........................» (с конечными пробелами, представленными здесь точками ). 

Обходной путь: перепишите ваш select-запрос: 
    
    
    SELECT substring(THEFIELD from 1 for 8) AS THEFIELD ...
    

или 
    
    
    SELECT cast(THEFIELD as varchar(8)) as THEFIELD ...
    

или используйте VARCHAR-тип полей. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** эта проблема может возникать и для других баз данных, в зависимости от их возвращаемой длины.

## Подробнее о транзакциях

Источники для этой информации / дальнейшего чтения: 

  * [Понимание транзакций Firebird](<http://www.ibphoenix.com/resources/documents/how_to/doc_400>) \- очень детальная статья (англ.)
  * [Транзакции в Firebird](<https://ib-aid.com/en/transactions-in-firebird-acid-isolation-levels-deadlocks-and-update-conflicts-resolution/>) другая очень детальная статья (англ.) ([прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): русскоязычные аналоги от тех же авторов [Жизненный цикл транзакций](<http://www.ibase.ru/utl/>) и [Транзакции в InterBase и Firebird](<http://www.ibase.ru/ibtrans/>))
  * Руководство по Interbase 6 API (справедливо для Firebird+Interbase), стр. 63 и далее
  * [Обзор настроек транзакций в Interbase](<http://conferences.embarcadero.com/article/32280>) (англ.)
  * [Подробное объяснение параметров в Firebird](<http://tech.groups.yahoo.com/group/firebird-support/message/58653>) (англ.)
  * [Обзор настроек в Firebird](<http://fhasovic.blogspot.com/2005/02/transaction-isolation-levels-in.html>) (англ.)
  * [Уровни изоляции транзакции в FIBPlus](<http://delphi-help.ru/index.php?option=com_k2&view=item&id=309:rabota-s-tranzaktsiyami-i-ih-ispolzovanie-v-fibplus-chast-2&Itemid=55>) ([прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): ссылки на devrace'овские статьи, как и сам оригинальный сайт, увы, давно мертвы, остались лишь сохраненные статьи на сторонних ресурсах)
  * README.set_transaction.txt в папке с документацией Firebird 2.5.



### Уровни изоляции транзакций

Если вы хотите, вы можете изменить уровни изоляции транзакции, добавив строку в свойстве Parameters транзакции: 

  * `isc_tpb_read_committed`: вы видите все изменения, закомиченные другими транзакциями
  * `isc_tpb_concurrency`: также называется Снапшотом (моментальным снимком): вы видите базу данных такой, какой она была при запуске транзакции. Имеет больше накладных расходов, чем isc_tpb_read_committed. Лучше, чем ANSI Serializable, потому что не имеет фантомных чтений.
  * `isc_tpb_consistency`: также называется Table Stability: стабильное, сериализуемое представление данных, но блокирует таблицы. Вряд ли вам это понадобится.



Пример: 
    
    
    SQLTransaction1.Params.text:='isc_tpb_read_committed';
    

Вы также можете добавить дополнительные параметры, которые влияют на транзакцию (взятые из исходного файла ibconnection.pp и [[1]](<http://conferences.embarcadero.com/article/32280>)): 

### Режим доступа

Этот параметр позволяет или только чтение, или чтение/запись 

  * `isc_tpb_read`: разрешение на чтение
  * `isc_tpb_write`: разрешение на чтение+запись



### Обработка конфликтов блокировки

  * `isc_tpb_nowait`: текущая транзакция откатывается с сообщением о конфликте, если запись все еще редактируется другой транзакцией
  * `isc_tpb_wait`: если запись все еще редактируется другой транзакцией, текущая транзакция дожидается завершения работы той, первой транзакции. Может смягчить «долгоживущие блокировки» в условиях сильной конкуренции ([[2]](<http://tech.groups.yahoo.com/group/firebird-support/message/58653>)). См. ниже описание параметра timeout value.



### Резервирование таблиц

Соглашение по блокировкам таблиц в целом. 

  * `isc_tpb_shared`: сначала указывается этот параметр, затем либо lock_read, либо lock_write для одной или нескольких таблиц. Разделяемый режим чтения или записи для таблиц.
  * `isc_tpb_protected`: сначала указывается этот параметр, затем либо lock_read, либо lock_write для одной или нескольких таблиц. Блокировка на таблицы; может разрешить операции без блокировок за счет отложенных транзакций.
  * `isc_tpb_lock_read`: задает блокировку чтения. Указывается, какая таблица блокируется, например, isc_tpb_lock_read=CUSTOMERS
  * `isc_tpb_lock_write`: задает блокировку чтения/записи. Указывается, какая таблица блокируется, например, isc_tpb_lock_read=CUSTOMERS



Комбинации: 

  * Shared, lock_write: пишущая транзакция с уровнем изоляции concurrency или read committed может обновлять таблицу. Читать таблицу может любая транзакция ([ прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): только в режиме READ ONLY (read)).
  * Shared, lock_read: читать и обновлять таблицу может любая транзакция.
  * Protected, lock_write: Другие транзакции не могут обновлять таблицу. Читать таблицу могут только транзакции concurrency или read committed.
  * Protected, lock_read: Другие транзакции не могут обновлять таблицу, но могут ее читать.



### Версионность записей

Этот параметр, по-видимому, относится только к режиму изоляции isc_tpb_read_committed для записей, изменяемых другими транзакциями: 

  * `isc_tpb_no_rec_version`: читается только самая новая версия записи. Может быть полезно для операций пакетной/массовой вставки вместе с isc_tpb_read_committed.
  * `isc_tpb_rec_version`: читается последняя подтвержденная версия записи, даже если в другой транзакции есть другие незакомиченные изменения. Больше накладных расходов, чем isc_tpb_no_rec_version.



### Различные варианты

Для полноты, в коде Firebird/Interbase FPC появилось еще несколько опций. Скорее всего, вы будете использовать isc_tpb_no_auto_undo только для ускорения вставки/редактирования пакетов. 

  * isc_tpb_exclusive (видимо переводится как защищенный в Firebird, см. [[3]](<http://tech.groups.yahoo.com/group/firebird-support/message/58653>))
  * isc_tpb_verb_time (Относится к отложенным ограничениям, которые могут выполняться в verb time или во время коммита. Firebird: не реализовано, всегда используйте verb time)
  * isc_tpb_commit_time (Относится к отложенным ограничениям, которые могут выполняться в verb time или в commit time. Firebird: не реализовано, всегда используйте verb time)
  * isc_tpb_ignore_limbo (игнорирует записи, созданные limbo-транзакциями. limbo-транзакции - это незавершенные транзакции в двухфазном коммите при работе с несколькими базами данных. Вряд ли вам понадобится эта функция)
  * isc_tpb_autocommit (автокоммит транзакции: каждый оператор является отдельной транзакцией. Вероятно, специально для драйвера JDB JayBird.)
  * isc_tpb_restart_requests (очевидно, ищет запросы в соединении, которые были активны в другой транзакции, прибивает их и перезапускает в новой транзакции.)
  * isc_tpb_no_auto_undo (отключает журнал отмен на уровне транзакций, что удобно для получения максимальной пропускной способности при выполнении пакетного обновления. Не имеет эффекта при данных только для чтения.)
  * isc_tpb_lock_timeout (укажите количество секунд ожидания для снятия блокировки, если вы используете isc_tpb_wait. Если это значение достигнуто без снятия блокировки, выдается сообщение об ошибке.)



### Firebird и транзакции по стандарту ANSI/ISO SQL

Транзакции Firebird не соответствуют 1 в 1 уровню транзакции стандарта ISO/ANSI. Приблизительно так: 

  * ISO Read Committed=READ COMMITTED+RECORD_VERSION
  * ISO Read Committed=READ COMMITTED+NO RECORD_VERSION
  * ISO Repeatable Read=SNAPSHOT (также известная как CONCURRENCY)
  * ISO Serializable=SNAPSHOT TABLE STABILITY (также известная как CONSISTENCY)



### Общие комбинации

По умолчанию (возможно, придется проверить) read committed. 

#### Пакетная/массовая вставка

isc_tpb_read_committed и isc_tpb_no_rec_version могут быть хорошей комбинацией: она позволяет другим транзакциям функционировать во время выполнения пакетной вставки. 

#### Транзакции только для чтения

Если вы хотите иметь доступ только для чтения к базе данных, вы можете сделать это, установив следующие параметры транзакции: 

  * `isc_tpb_read`
  * `isc_tpb_read_committed`
  * `isc_tpb_rec_version`
  * `nowait`



Эта комбинация не будет блокировать сборку мусора, что само по себе хорошо. Источник: [[4]](<http://tech.groups.yahoo.com/group/firebird-support/message/118748>)

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** [Firebird FAQ](<http://www.firebirdfaq.org/faq164/>) указывает, что вам понадобится доступ с правом записи в файл базы данных, даже если вы только читаете из него, если вы не установите флаг базы данных read-only [только для чтения] (например, с помощью утилиты gfix).

## Ссылки и дополнительная информация

В списке ниже приведены ссылки на дополнительную информацию о Firebird и связанных с ней инструментах. 

  * [Использование Firebird embedded с приложениями FPC/Lazarus](<Firebird_embedded.md> "Firebird embedded/ru")



### Примеры Lazarus Firebird

  * Примеры исходного кода приложения Lazarus/Firebird [[5]](<http://lazarus.freepascal.org/index.php/topic,13940.msg73617.html#msg73617>)
  * Учебники Firebird/SQLDB: 
    * [SQLdb Tutorial0](<../en/SQLdb_Tutorial0.md> "SQLdb Tutorial0")
    * [SQLdb Tutorial1](<../en/SQLdb_Tutorial1.md> "SQLdb Tutorial1")
    * [SQLdb Tutorial2](<../en/SQLdb_Tutorial2.md> "SQLdb Tutorial2")
    * [SQLdb Tutorial3](<../en/SQLdb_Tutorial3.md> "SQLdb Tutorial3")
    * [SQLdb Tutorial4](<../en/SQLdb_Tutorial4.md> "SQLdb Tutorial4")


  * Учебники по PP4S; включает установку Firebird на Windows, а также 
    * [Планирование базы данных (англ.)](<http://www.pp4s.co.uk/main/tu-db-plan.html>)
    * [Установка Firebird (англ.)](<http://www.pp4s.co.uk/main/tu-db-installingfirebird01.html>)
    * [Создание базы данных Firebird (англ.)](<http://www.pp4s.co.uk/main/tu-db-firebird-create.html>)
    * [Редактирование базы данных Firebird (англ.)](<http://www.pp4s.co.uk/main/tu-db-firebird-demo1.html>)
    * [Поиск в базе данных Firebird (англ.)](<http://www.pp4s.co.uk/main/tu-db-firebird-demo2.html>)
    * [Создание и печать отчетов (англ.)](<http://www.pp4s.co.uk/main/tu-db-firebird-demo3.html>)
    * [Создание и использование хранимых процедур (англ.)](<http://www.pp4s.co.uk/main/tu-db-firebird-demo4.html>)



### Инструменты администрирования

  * [FlameRobin](<http://www.flamerobin.org/>) \- GUI инструмент с открытым исходным кодом для управления Firebird, доступный для Linux, Windows и macOS. Настоятельно рекомендуется.
  * [Turbobird](<https://github.com/motaz/turbobird>) \- GUI инструмент с открытым исходным кодом для управления Firebird. Написано с помощью Lazarus с использованием TIBConnection.
  * ibconsole : инструмент для управления Firebird и базами данных Interbase с графическим интерфейсом, доступен для Windows и Linux.
  * Lazarus Data Desktop - входит в репозиторий Lazarus (находится в каталоге 'tools/lazdatadesktop/').
  * [LazSQLX](<http://lazsqlx.wordpress.com/>)\- многофункциональный инструмент управления базами данных с открытым исходным кодом. Написан на Lazarus с использованием компонентов как SQLDB, так и Zeos. Включает поддержку Firebird.
  * tiSQLEditor - включен в каталог «Support Apps» репозитория [tiOPF](<../en/tiOPF.md> "tiOPF"). Это инструмент, используемый для написания SQL с некоторой поддержкой автозавершения кода, может запускать сценарии, выполнять сценарии обновления для приложений от одной сборки до более поздней сборки, имеет различные удобные функции копирования и вставки для приложений Object Pascal, имеет много функций, которые полезны для tiOPF (создает шаблоны кода Object Pascal из результатов запросов, для классов посетителей tiOPF), экспортирует результаты запросов в CSV и т.д.
  * [Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): [IBExpert](<http://www.ibexpert.net/ibe/>) \- ИМХО, лучший GUI-инструмент для администрирования БД Firebird под Windows. Постоянно обновляемый, с активным [русскоязычным коммунити](<https://www.sql.ru/forum/ibexpert>), бесплатен для пользователей Windows с кодовой страницей _Win1251_ (в основном, жителей exUSSR). В комплекте SQL-редактор с подсветкой синтаксиса и автозавершением слов, редактор скриптов, построитель запросов, поддержка IBEScript, менеждер прав и пользователей, печать и экспорт данных в различные форматы и проч.проч. Скачать можно по прямой ссылке [здесь](<http://www.ibexpert.com/rus/ibe_sfx.exe>) в виде самораспаковывающегося архива. Документация представлена [здесь](<http://www.ibexpert.net/ibe/index.php?n=Doc.Doc>).



### Firebird

  * РСУБД [Firebird](<http://firebirdsql.org/>). Этот сайт также содержит много документации по Firebird.
  * Firebird [FAQ](<http://firebirdfaq.org/>). Удобный сайт, который показывает, например, отличия от других РСУБД.
  * [Новый сайт](<http://www.controlpascal.com/firebird.htm>), который показывает, как использовать новейшие инструменты и LibreOffice, FPC будет добавлен в ближайшее время.

---

_Source: [https://wiki.freepascal.org/Firebird/ru](https://web.archive.org/web/20250219062224/https://wiki.freepascal.org/Firebird/ru)_
