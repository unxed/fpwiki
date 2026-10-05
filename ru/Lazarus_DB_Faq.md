# Lazarus DB Faq

│ **[Deutsch (de)](</Lazarus_DB_Faq/de> "Lazarus DB Faq/de")** │  **[English (en)](<../en/Lazarus_DB_Faq.md> "Lazarus DB Faq")** │  **[español (es)](</Lazarus_DB_Faq/es> "Lazarus DB Faq/es")** │  **[français (fr)](</Lazarus_DB_Faq/fr> "Lazarus DB Faq/fr")** │  **[italiano (it)](</Lazarus_DB_Faq/it> "Lazarus DB Faq/it")** │  **[日本語 (ja)](</Lazarus_DB_Faq/ja> "Lazarus DB Faq/ja")** │  **[português (pt)](</Lazarus_DB_Faq/pt> "Lazarus DB Faq/pt")** │  **русский (ru)** │  **[slovenčina (sk)](</Lazarus_DB_Faq/sk> "Lazarus DB Faq/sk")** │  **[中文（中国大陆）‎ (zh_CN)](</Lazarus_DB_Faq/zh_CN> "Lazarus DB Faq/zh CN")** │    
****

## Contents

  * 1 Общее
    * 1.1 Где я могу найти больше ЧАсто задаваемых ВОпросов (ЧАВО)?
    * 1.2 Где я могу найти компоненты для баз данных?
    * 1.3 А есть ли другие компоненты?
  * 2 Interbase / Firebird
    * 2.1 Как я могу подключиться из Linux к Windows серверу
  * 3 MySQL
    * 3.1 Какая версия MySQL поддерживается?
      * 3.1.1 Pascal Data Objects
  * 4 PostgreSQL



## Общее

Этот FAQ поможет вам в программировании баз данных с помощью Lazarus. 

### Где я могу найти больше ЧАсто задаваемых ВОпросов (ЧАВО)?

Смотрите на официальном сайте www.lazarus.freepascal.org. Там есть еще один FAQ. [Здесь](<../en/Lazarus_Faq.md> "Lazarus Faq") вы найдете страницу, которая показана как копия FAQ с www.lazarus.freepascal.org. 

### Где я могу найти компоненты для баз данных?

В настоящее время компоненты SQLdb являются частью Lazarus, вам необходимо только установить эти компоненты. Если вы посмотрите в каталоге [$LazarusDir]/components, вы найдете подкаталог SQLdb. Установите sqldblaz.lpk и вы сможете подключаться к серверам MySQL, Interbase / Firebird и Postgres, а так же работать с базами SQLite. Смотрите [сюда](<../en/Install_Packages.md> "Install Packages") для помощи по установке пакетов. 

### А есть ли другие компоненты?

Да. 

  * [ZEOS компоненты](<http://zeoslib.sourceforge.net/index.php>) портированы в Lazarus. Вы можете загрузить ZEOSDBO_Rework с [ZEOS cvs](<http://sourceforge.net/cvs/?group_id=35994>). Смотри также обучение по [Zeos](<../en/Zeos_tutorial.md> "Zeos tutorial").
  * [Pascal Data Objects](<https://trac.synsport.com:8000/index.php/pdo/wiki>) работает одинаково для Delphi и FPC 2.0 и выше (MySQL и Firebird).
  * [FIBL](<http://sourceforge.net/projects/fibl>) Сергея Смирнова. Являются портом FreeIB by Greg Deatz. Работают с Firebird.
  * [Unified IB](<http://www.progdigy.com/?page_id=5>) работают с Firebird.



Смотрите также [wiki.lazarus.freepascal.org/Databases](<http://wiki.lazarus.freepascal.org/Databases>). 

## Interbase / Firebird

### Как я могу подключиться из Linux к Windows серверу

Ввод правильного пути к базе данных к Windows серверу из Linux клиента бывает не совсем ясен. 

Предположим, у вас установлен firebird в F:\Program Files\firebird\\. База данных employee.fdb установлена в каталоге F:\Program Files\firebird\examples\ . IP-адрес компьютера 192.168.2.100. 

Вам нужно ввести следующее имя базы данных в databasename 
    
    
    192.168.2.100:F:\Program Files\firebird\examples\employee.fdb
    

в ваш IBConnection, который соединит с необходимой базой данных. 

**Примечание:** Более правильный способ - при создании базы данных, внести алиас этой БД в файл aliases.conf, например: 
    
    
    db1 = F:\Program Files\firebird\employee.fdb
    

в этом случае строка подключения упрощается: 
    
    
    192.168.2.100:db1
    

Примечание: вам нет надобности указывать значение в свойстве hostname! 

## MySQL

### Какая версия MySQL поддерживается?

В настоящее время поддерживается компонентами SQLdb версии MySQL 4.0, MySQL 4.1 и MySQL 5.0. Удостоверьтесь, что вы используете верный компонент соединения для клиентских библиотек. Например, если у вас клиентские библиотеки установлены для MySQL 4.1, вы должны использовать компонент TMySQL41Connection, даже если сервер версии 4.0 или 5.0. 

#### Pascal Data Objects

Является альтернативой. Функции, предоставляемые MySQL 4.1 и 5.0, такие как подготавливающие состояние, связывание, и хранимые процедуры поддерживаются API базой данных, вызывают Pascal Data Objects, которые входят в PHP Data Objects. Весь код и документация, необходимые для использования нового API, доступны на Trac wiki: 

<https://trac.synsport.com:8000/index.php/pdo/wiki> (логин/пароль - guest/guest) 

## PostgreSQL

Вы можете подключиться к PostgreSQL db при помощи компонента TPQConnection. Этот компонент использует PostgreSQL библиотеку соединения libpq [[1]](<http://www.postgresql.org/docs/8.0/interactive/libpq.html>), так что вы можете легко подключиться к PostgreSQL db, однако, компонент не поддерживает следующие параметры подключения: 

  * Port
  * ClientEncoding



Может также есть другие недокументированные параметры подключения. 

\--[Jrmarino](</index.php?title=User:Jrmarino&action=edit&redlink=1> "User:Jrmarino \(page does not exist\)") 11:28, 13 Mar 2006 (CET)

---

_Source: [https://wiki.freepascal.org/Lazarus_DB_Faq/ru](https://web.archive.org/web/20230331064546/https://wiki.freepascal.org/Lazarus_DB_Faq/ru)_
