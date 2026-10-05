# Zeos tutorial

│ **[Deutsch (de)](</Zeos_tutorial/de> "Zeos tutorial/de")** │  **[English (en)](<../en/Zeos_tutorial.md> "Zeos tutorial")** │  **[español (es)](</Zeos_tutorial/es> "Zeos tutorial/es")** │  **[français (fr)](</Zeos_tutorial/fr> "Zeos tutorial/fr")** │  **[português (pt)](</Zeos_tutorial/pt> "Zeos tutorial/pt")** │  **русский (ru)** │  **[中文（中国大陆）‎ (zh_CN)](</Zeos_tutorial/zh_CN> "Zeos tutorial/zh CN")** │    
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

  * 1 Обзор
  * 2 Получение Zeos
    * 2.1 SVN
      * 2.1.1 Windows
      * 2.1.2 Linux/BSD
    * 2.2 ZIP
  * 3 Установка компонентов
  * 4 Немного о компонентах
    * 4.1 TZConnection
      * 4.1.1 AutoCommit
      * 4.1.2 TransactIsolationLevel (TIL)
      * 4.1.3 Protocol
      * 4.1.4 ReadOnly
      * 4.1.5 Properties
    * 4.2 TZQuery и TZUpdateSQL
      * 4.2.1 Transaction и UpdateTransaction
  * 5 Делаем свое первое приложение Zeos
  * 6 Possible Bugs and Issues
  * 7 See also



# Обзор

Это руководство посвящено получению, установке и использованию [Zeoslib](<https://www.firmos.at/zeos>) с [Lazarus](<../en/Glossary.md> "Glossary") и [FPC](<../en/Glossary.md> "Glossary"). 

# Получение Zeos

Zeos недавно был перенесен на [Lazarus](<../en/Glossary.md> "Glossary"), и пока нет выпусков, которые официально поддерживают его, но вы можете легко получить его из SVN, если выполните следующие действия: 

## SVN

### Windows

  * скачайте клиент SVN [TortoiseSVN](<http://tortoisesvn.tigris.org>) и установите
  * см. [Начало работы с TortoiseSVN](<http://tortoisesvn.net/docs/release/TortoiseSVN_en/help-onepage.html#tsvn-dug-general>)
  * сделайте Checkout из проводника Windows: <http://svn.code.sf.net/p/zeoslib/code-0/trunk>



### Linux/BSD

  * [FreeBSD](<../en/Portal_FreeBSD.md> "Portal:FreeBSD") поставляется с предустановленным **svnlite** (клиент svn)
  * получите клиент SVN (esvn, kdesvn и т.д.)
  * создайте каталог zeosdbo, перейдите в этот каталог и выполните
  * svn checkout <http://svn.code.sf.net/p/zeoslib/code-0/trunk>



## ZIP

Вы можете скачать последнюю версию в виде ZIP-файла с sourceforge.net: 

  * <http://sourceforge.net/projects/zeoslib/>



# Установка компонентов

Это сложная часть, поэтому вам следует проявить немного терпения и внимательно прочитать эту часть. 

  * Убедитесь, что у вас последний [снапшот Lazarus](<http://www.de.freepascal.org/lazarus/>) и версия FPC 2.0.3 не ранее 6 марта 2006г.
  * Запустите один экземпляр Lazarus.


  1. Используйте **Components/Open Package File(.lpk)** из главного меню.
  2. Перейдите в **zeosdbo_rework\packages\lazarus\** и откройте **zcomponent.lpk**
  3. Нажмите только **[Compile]** в том случае, если вы не хотите устанавливать компоненты в IDE
  4. Нажмите **[Install]**
  5. Вас спросят, хотите ли вы перекомпилировать Lazarus.


  * Ответьте **[Yes]** на этот раз.
  * Дождитесь завершения компиляции, после этого Lazarus должен перезапуститься.
  * Если все в порядке, теперь вы должны увидеть вкладку **[Zeos Access]** на палитре компонентов.



* * *

[Прим.перев](</User:Zoltanleo> "User:Zoltanleo"): на момент перевода статьи (май 2021г) установка немного изменилась: 

  * качаете исходный код отсюда <https://svn.code.sf.net/p/zeoslib/code-0/trunk>
  * открываете файл zeos\packages\lazarus\zcomponentdesign.lpk
  * жмете последовательно **[Compile]** и **[Install]**
  * пересобираете Лазарус



* * *

[![Zeos Components.png](https://wiki.freepascal.org/images/2/25/Zeos_Components.png)](</File:Zeos_Components.png>)

Если вы получаете сообщение об ошибке "Cannot find unit ZClasses"(Не удается найти модуль ZClasses) или что-то подобное, вам необходимо внимательно проверить регистр имен файлов в исходном дистрибутиве Zeos. 

  * Даже если случаи полностью совпадают, автоматически сгенерированный исходный файл пакета может сгенерировать неправильное имя случая в разделе uses (Lazarus 0.9.18), то есть:


    
    
    { This file was automatically created by Lazarus. Do not edit!
      This source is only used to compile and install the package.
    }
    unit Zcore; 
    interface
    uses
      Zclasses, Zcollections, Zcompatibility, Zexprparser, Zexprtoken, Zexpression, 
      Zfunctions, Zmatchpattern, Zmessages, Zsysutils, Ztokenizer, Zvariables, 
      Zvariant; 
    implementation
    end.
    

  * Обратите внимание, что Lazarus переименовал модуль Z**C** lasses на Z**c** lasses, что привело к конфликту имен. Предположительно это ошибка в Lazarus, а не в пакетах Zeos. Один из способов обойти это - переименовать все исходные файлы zeos в нижний регистр. Просмотрите каждый подкаталог в src/ и выполните эту команду в окне bash:


    
    
     rename -v 'y/A-Z/a-z/' *
    

  * Затем в Lazarus повторно откройте пакет (.lpk) и исправьте регистры имен файлов, нажав "More..."/"Fix Files Case"(Еще .../Исправить регистр файлов).
  * Теперь пакет должен скомпилироваться.



# Немного о компонентах

[ Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): позволил себе сделать краткое описание компонентов из набора на основе [официальной документации](<https://sourceforge.net/projects/zeoslib/files/documentation/ZeosDocumentationCollection-2017-03-20.pdf/download>). 

## TZConnection

Компонент TZConnection представляет собой комбинацию компонента, подобного BDE TDatabase, и компонента, который обрабатывает транзакцию. Транзакция запускается библиотекой ZEOS всякий раз, когда открывается соединение (метод Connect TZConnection) с базой данных. Это приводит к тому, что каждый доступ к базе данных выполняется автоматически в контексте выполняющейся транзакции. 

### AutoCommit

Так называемый режим AutoCommit о умолчанию всегда включен (установлен в «True»). Это также стандартное поведение соответствующего компонента BDE. Если AutoCommit активен, то каждое изменение оператора SQL будет подтверждаться в базе данных с помощью COMMIT после его успешного выполнения. 

Если вы хотите управлять транзакцией сами при помощи последовательных StartTransaction..Commit/Rollback, то AutoCommit должен быть отключен (установлен в «False»). В рамках этой явной транзакции можно последовательно выполнить несколько операторов SQL, которые вносят изменения в базу данных. При вызове метода Commit все изменения, сделанные в этой явной транзакции, подтверждаются. Вызов метода Rollback сбрасывает эти изменения. 

### TransactIsolationLevel (TIL)

Компонент TZConnection предоставляет четыре полезных и предопределенных уровня изоляции транзакций (TIL): 

  * **tiRepeatableRead** : он соответствует TIL ”SNAPSHOT”, который является стандартом серверов Firebird. Это комбинация параметров транзакции concurrency и nowait. Создается снимок текущей базы данных. На других пользователей влияют (ограничивают) только в том случае, если две транзакции работают с одной записью одновременно. Если при доступе к данным возникнут конфликты, будет возвращено сообщение об ошибке. Изменения в других транзакциях не будут замечены. Этот TIL широко охватывает требования стандарта SQL (SERIALIZABLE).


  * **TiReadCommitted** : соответствует TIL "READ COMMITTED". Это комбинация параметров транзакции "read_committed", "rec_version" и "nowait'. Этот TIL распознает все изменения в других транзакциях, которые были подтверждены COMMIT. Параметр "rec_version" отвечает за поведение, при котором будут учитываться самые последние значения, зафиксированные другими пользователями. Параметр "nowait" отвечает за поведение, при котором нет ожидания освобождения заблокированной записи (т.е. транзакция откатывается, если запись редактируется другим пользователем, а потому является заблокированной). Таким образом, сервер более нагружен, чем в TIL tiRepeatableRead, потому что он должен выполнять все обновления, чтобы получать эти значения снова и снова.


  * **TiSerializable** : соответствует TIL "SNAPSHOT TABLE STABILITY'. Она используется для получения монопольного доступа к набору данных. Реализуемый параметром транзакции «согласованность», она предотвращает доступ «внешней» транзакции к записанным данным. Только транзакция, которая записала данные, может получить к ним доступ. Это предотвращает также многопользовательский доступ к записанным данным. Поскольку этот TIL очень ограничивает доступ к записанным данным, его следует применять с большой осторожностью.


  * **TiNone** : TIL не используется для изоляции транзакции. TIL tiReadUncommitted не поддерживается Firebird. Если используется этот TIL, будет вызвана ошибка, и транзакция не будет изолирована (как при использовании tiNone).



### Protocol

Свойство определяет, какой протокол коннекта (напр, ado, firebird, sqlite и т.д.) будет использоваться и, следовательно, к какому серверу SQL будет осуществляться доступ. Благодаря этому, вам не нужно устанавливать специальные компоненты для каждой базы данных, к которой вы хотите получить доступ, как это было в версии 5.x и ранее. Компоненты будут установлены один раз. Вы только выбираете протокол для поддерживаемого SQL-сервера, к которому хотите получить доступ, и все готово. 

### ReadOnly

Соединение с базой данных, поддерживаемое объектом TZConnection, по умолчанию настроено только для чтения (ReadOnly = True). Это означает, что доступ для записи в подключенную базу данных запрещен. Чтобы получить доступ для записи в базу данных, вы должны установить для ReadOnly значение False. 

### Properties

Редактор множества настроек, каждую из которых можно задать либо путем выбора в диалоговом окне 

[![zeos zconnection properties.png](https://wiki.freepascal.org/images/f/ff/zeos_zconnection_properties.png)](</File:zeos_zconnection_properties.png>)

либо определением этого параметра в коде, например: 
    
    
    ZConection.Properties.Add ('lc\_ctype=ISO8859\_1');
    //или
    ZConnection.Properties.Add ('Codepage=ISO8859\_1');
    

## TZQuery и TZUpdateSQL

Компонент для чтения и изменения набора данных. Для чтения необходимо заполнить свойство SQL, затем открыть набор данных, например: 
    
    
    ZQuery.SQL.Text:= 'select * from MyTable';
    ZQuery.Active:= True;//или if not ZQuery.Active then ZQuery.Open;
    

Для модификации набора данных необходимо использовать TZUpdateSQL, где должны быть заполнены свойства ModifySQL, InsertSQl, DeleteSQL и RefreshSQL для изменения, вставки, удаления и обновления данных соответственно. При этом оба компонента должны быть связаны следующим образом: 
    
    
    ZQuery.UpdateObject:= ZUpdateSQL;
    

а для выполнения модификации данных необходимо вызывать ZQuery.ExecSQL (вместо ZQuery.Open). 

Генерацию модифицирующих запросов можно выполнять в design-time при помощи встроенного редактора. Для этого необходимо в design-time связать между собой ZConnection, ZTransaction, ZQuery и ZUpdateSQL. Затем выполнить подключение к базе данных, сделать активным ZQuery, выделить ZUpdateSQL, ПКМ вызвать контекстное меню и выбрать пункт "UpdateSQL editor..." 

[![zeos zupdatesql editor1.png](https://wiki.freepascal.org/images/b/b9/zeos_zupdatesql_editor1.png)](</File:zeos_zupdatesql_editor1.png>)

Затем последовательно выделяем первичный ключ в поле "Key Fields" и жмем "Select Primary Keys", потом в поле "Update Fields" выделяем поля, которые мы хотим модифицировать 

[![zeos zupdatesql editor2.png](https://wiki.freepascal.org/images/3/31/zeos_zupdatesql_editor2.png)](</File:zeos_zupdatesql_editor2.png>)

Нажав кнопку "Generate SQL", получает автоматически сгенерированные запросы 

[![zeos zupdatesql editor3.png](https://wiki.freepascal.org/images/c/cb/zeos_zupdatesql_editor3.png)](</File:zeos_zupdatesql_editor3.png>)

### Transaction и UpdateTransaction

Поля, где указываются транзакции для чтения (ZQuery.Open) и записи (ZQuery.ExecSQL) соответственно. Иногда читающей и пишущей транзакцией может быть одна транзакция (зависит от типа сервера). 

# Делаем свое первое приложение Zeos

  * Бросьте на форму **ZConnection**. 
    * Задайте свои User, Password, Host, Port и Protocol (и любые другие параметры, если необходимо).
    * Установите Connected в True.


  * Бросьте на форму **ZQuery** (не перепутайте с ZReadOnlyQuery). 
    * Задайте свойству Connection значение ZConnection.
    * Задайте для свойства Sql что-то вроде **SELECT * FROM MyTable**
    * Установите Active в True.


  * Бросьте на форму **DataSource** с вкладки **[Data Access]**. 
    * Задайте свойству DataSet ваш ZQuery.


  * Бросьте на форму **DBGrid** с вкладки **[Data Controls]**. 
    * Задайте свойству Datasource ваш DataSource.
    * Если все в порядке, вы должны увидеть записи из своей таблицы..



# Possible Bugs and Issues

  * I have noticed that sometimes when building Lazarus it cannot find some Zeos files, as a quick workaround try this: 
    * Use **Components/Package Graph** from the main menu.
    * Open the **ZComponent** package.
    * Right Click on the **Files** item in the list.
    * Choose **[Recompile all required]**.
    * When asked "Re-Compile this and all required packages?" answer **[Yes]**.
    * Recompile Lazarus normally (with packages).  
  




# See also

  * [Forum for ZeosLib](<http://zeoslib.sourceforge.net/index.php>)
  * [ZeosDBO](<../en/ZeosDBO.md> "ZeosDBO")
  * [Учебник Lazarus/Zeos/Firebird (Windows)](<https://lazarus.intern.es/tutorial_firebird_lazarus_zeos_2.html>) Немец./частично на Англ [download site](<https://lazarus.intern.es/download_tutorials_lazarus_zeos_firebird.html>)
  * [Документация по работе с компонентами (англ)](<https://sourceforge.net/projects/zeoslib/files/documentation/ZeosDocumentationCollection-2017-03-20.pdf/download>)

---

_Source: [https://wiki.freepascal.org/Zeos_tutorial/ru](https://web.archive.org/web/20240701000000/https://wiki.freepascal.org/Zeos_tutorial/ru)_
