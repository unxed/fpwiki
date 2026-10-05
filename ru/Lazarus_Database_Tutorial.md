# Lazarus Database Tutorial

│ **[Deutsch (de)](</Lazarus_Database_Tutorial/de> "Lazarus Database Tutorial/de")** │  **[English (en)](<../en/Lazarus_Database_Tutorial.md> "Lazarus Database Tutorial")** │  **[español (es)](</Lazarus_Database_Tutorial/es> "Lazarus Database Tutorial/es")** │  **[français (fr)](</Lazarus_Database_Tutorial/fr> "Lazarus Database Tutorial/fr")** │  **[Bahasa Indonesia (id)](</Lazarus_Database_Tutorial/id> "Lazarus Database Tutorial/id")** │  **[italiano (it)](</Lazarus_Database_Tutorial/it> "Lazarus Database Tutorial/it")** │  **[日本語 (ja)](</Lazarus_Database_Tutorial/ja> "Lazarus Database Tutorial/ja")** │  **[Nederlands (nl)](</Lazarus_Database_Tutorial/nl> "Lazarus Database Tutorial/nl")** │  **[português (pt)](</Lazarus_Database_Tutorial/pt> "Lazarus Database Tutorial/pt")** │  **русский (ru)** │  **[中文（中国大陆） (zh_CN)](</Lazarus_Database_Tutorial/zh_CN> "Lazarus Database Tutorial/zh CN")** │  **[中文（臺灣） (zh_TW)](</Lazarus_Database_Tutorial/zh_TW> "Lazarus Database Tutorial/zh TW")** │    
****

## Contents

  * 1 Обзор
  * 2 Lazarus и MySQL
    * 2.1 Получение работающего MySQL в Linux или Windows
    * 2.2 Работа с MySQL в текстовом режиме
    * 2.3 Подключение к MySQL из приложения Lazarus
  * 3 Lazarus и PostgreSQL
  * 4 Lazarus и SQLite
    * 4.1 Введение
    * 4.2 Требования
    * 4.3 Как работать (Основы)
    * 4.4 Примечания
  * 5 Lazarus и MSSQL
  * 6 Lazarus и Firebird / Interbase
    * 6.1 FBLib Firebird Library
  * 7 Lazarus и dBase
    * 7.1 Поиск и отображение набора данных
  * 8 Ссылки, связанные с этой статьей
  * 9 Ответственность и изменения



## Обзор

Это руководство по работе с различными базами данных в Lazarus. Lazarus поддерживает множество баз данных с помощью внешних пакетов, разработчик должен установить соответствующий пакет для каждой из них. Вы можете получить доступ к базе данных путем написания соответствующего кода или используя компоненты в форме. Компоненты доступа к данным представляют поля и подключаются путем установки свойства DataSource, которое представляет таблицу и подключается к компонентам баз данных, например TPSQLDatabase, TSQLiteDataSet путем установки свойства DataSet. Компоненты доступа к данным располагаются на вкладке "Data Controls". Источники данных и компоненты доступа к базам данных располагаются на вкладке "Data Access". 

## Lazarus и MySQL

### Получение работающего MySQL в Linux или Windows

Следуйте инструкциям в Руководстве пользователя MySQL. Убедитесь в том, что демон mysqld работает надежно, и в том, что все потенциальные пользователи (в том числе root, mysql, вы сами и все остальные, кому это может понадобиться) имеют столько привилегий, сколько им нужно (настолько, насколько это согласуется с безопасностью). Желательно, чтобы все пользователи, включая root, имели пароли. Проверьте действия в системе базы данных, используя примеры, приведенные в руководстве, и убедитесь, что все пользователи действительно имеют надежный доступ. 

### Работа с MySQL в текстовом режиме

В каталоге $(fpcsrcdir)/packages/base/mysql/ лежит пример программы. Исходники fpc в Lazarus найти можно через меню Окружение –> Настройки окружения –> Закладка Файлы –> Каталог исходного кода FPC. Возможные пути для mysql каталога /usr/share/fpcsrc/packages/base/mysql/ (rpm install), или C:\lazarus\fpcsrc\packages\base\mysql\ (windows). Этот каталог содержит также модули mysql.pp, mysql_com.pp и mysql_version.pp. Прежде чем запустить тестовый скрипт, нужно создать базу данных с именем testdb: сделайте это, войдя в mysql монитор (в качестве root с полным набором прав), и выполните следующую команду SQL 
    
    
    CREATE DATABASE testdb;
    

Затем убедитесь, что все соответствующие пользователи имеют соответствующие права доступа к этой базе данных 
    
    
    GRANT ALL ON testdb TO johnny-user IDENTIFIED BY 'johnnyspassword'; 
    

Существует скрипт, называемый mkdb, попробуйте его запустить: 
    
    
    sh ./mkdb
    

Скорее всего, из этого ничего не выйдет, потому что система не позволит анонимному пользователю получить доступ к базе данных. Измените скрипт с помощью текстового редактора таким образом, чтобы строка запуска mysql выглядела так: 
    
    
    mysql -u root -p  ${1-testdb} << EOF >/dev/null
    

и запустите его еще раз, когда появится запрос – введите пароль. Возможно, вам удалось создать тестовую базу данных, проверьте ее, войдя в mysql монитор и введя следующую команду 
    
    
    select * from FPdev;
    

Вы увидите таблицу с ID, именами пользователя и email-адресами некоторых разработчиков FPC. 

Теперь попробуйте запустить тестовую программу testdb.pp (возможно, придется ее скомпилировать и весьма вероятно, что с первой попытки это не удастся!!). 

Я обнаружил, что программа не может соединиться с mysql по нескольким причинам: 

  * Моя система (SuSE Linux v9.0) содержит mysql v4.0.15, а не версию 3, для которой был разработан пакет доступа к mysql.
  * Программе требуется имя пользователя и пароль для доступа к базе данных.
  * Компилятору хочется знать, где можно найти библиотеки mysql (ЕСЛИ ВЫ ДО СИХ ПОР НЕ УСТАНОВИЛИ БИБЛИОТЕКИ РАЗРАБОТЧИКА MYSQL, СДЕЛАЙТЕ ЭТО ПРЯМО СЕЙЧАС!)



Я создал копию testdb.pp с именем trydb.pp, не редактируя оригинал – это означает, что исходные файлы еще получат исправления в последующих обновлениях CVS. Также я скопировал файлы в каталоге mysql/ver40/ в основной каталог mysql/, переименовав их в mysql_v4.pp, mysql_com_v4.pp и mysql_version_v4.pp, будучи уверенным в переименовании каждого модуля соответственно. Я изменил uses в trydb.pp на 
    
    
    uses mysql_v4
    

и в mysql_v4.pp на 
    
    
    uses mysql_com_v4
    

Я добавил строку в /etc/fpc.cfg, показывающую на мои библиотеки: 
    
    
    -Fl/lib;/usr/lib
    

В следующем шаге, возможно, не будет необходимости, если библиотеки разработчика установлены в качестве ссылок, но проверить не помешает. Мне пришлось найти подлинное имя из библиотеки mysqlclint в /usr/lib каталоге, и в моем случае мне пришлось выдать команду: 
    
    
    ln -s libmysqlclient.so.12.0.0 lmysqlclient
    

чтобы создать символическую ссылку, позволяющую FPC найти библиотеку. Также я создал ссылку 
    
    
    ln -s libmysqlclient.so.12.0.0 mysqlclient
    

и поместил аналогичные ссылки в других каталогах: не то, чтобы очень уж необходимо, но на всякий случай. Некоторым пользователям, возможно, придется добавить следующую ссылку 
    
    
    ln -s libmysqlclient.so.12.0.0 libmysqlclient.so
    

Я изменил trydb.pp – включил реквизиты пользователя, стартовый хост, имя пользователя и пароль как константы: 
    
    
    const
     host : Pchar= 'localhost';
     user : Pchar= 'myusername';
     passwd: Pchar = 'mypassword';
    

Я также обнаружил, что не могу соединиться с mysql, используя вызов mysql_connect() и мне приходится использовать вызов функции mysql_real_connect(), которая имеет гораздо больше параметров. Дальнейшее осложняется тем, что количество параметров изменилось между версией 3 (их было семь) и версией 4 (стало восемь). Перед использованием mysql_real_connect мне пришлось использовать mysql_init(), который не нашелся в оригинале mysql.pp но имеется в mysql_v4.pp. 

Ну вот и код для подключения к базе данных: 
    
    
    { a few extra variables}
    var
      alloc : PMYSQL;
    
    {main program fragment}
    
    begin
     if paramcount=1 then
     begin
       Dummy:=Paramstr(1)+#0;
       DataBase:=@Dummy[1];
     end;
    
     Writeln ('Allocating Space...');
     alloc := mysql_init(PMYSQL(@qmysql));
     Write ('Connecting to MySQL...');
     sock :=  mysql_real_connect(alloc, host, user, passwd, database, 0, nil, 0);
     if sock=Nil then
     begin
       Writeln (stderr,'Couldnt connect to MySQL.');
       Writeln (stderr, 'Error was: ', mysql_error(@qmysql));
       halt(1);
     end;
     Writeln ('Done.');
     Writeln ('Connection data:');
    {$ifdef Unix}
     writeln ('Mysql_port      : ',mysql_port);
     writeln ('Mysql_unix_port : ',mysql_unix_port);
    {$endif}
     writeln ('Host info       : ',mysql_get_host_info(sock));
     writeln ('Server info     : ',mysql_stat(sock));
     writeln ('Client info     : ',mysql_get_client_info);
    
     Writeln ('Selecting Database ',DataBase,'...');
     if mysql_select_db(sock,DataBase) < 0 then
     begin
       Writeln (stderr,'Couldnt select database ',Database);
       Writeln (stderr,mysql_error(sock));
       halt (1);
     end;
     {... as original contents of testdb.pp}
    

Теперь готовы к компиляции trydb.pp? 
    
    
    fpc trydb
    

Успешно! Запускаем: 
    
    
    ./trydb
    

У-у-у-е-е-е-е! Я попал в список разработчиков FPC! Несколько дополнительных уточнений: сделать ввод реквизитов пользователя и mysql команд интерактивным, с помощью переменных, а не констант, и позволить вводить SQL команды, пока не подадим команду выхода: см. полный листинг программы [см. полный листинг программы](<http://wiki.lazarus.freepascal.org/Lazarus_Database_Tutorial/TryDbpp>), в которой реквизиты пользователя вводятся из консоли, и программа входит в цикл, где SQL команды вводятся в консоли (без закрывающей запятой), и результаты выводятся до тех пор, пока с клавиатуры не будет введена команда "quit". Листинг консольного вывода [здесь](<http://wiki.lazarus.freepascal.org/Lazarus_Database_Tutorial/SampleListingSee>)

### Подключение к MySQL из приложения Lazarus

Данное руководство показывает, как Lazarus подключается к базе данных MySQL и выполняет простые запросы, используя основные компоненты Lazarus. Использования компонент отображения данных не будет, покажем только принцип взаимодействия с базами данных. 

Создайте новый проект в Lazarus: 
    
    
    Project -> New Project -> Application
    

Автоматически сгенерируется новая форма.. 

Растяните форму на пол-экрана и переименуйте ее и ее заголовок в 'TryMySQL'. 

Расположите на форме сверху слева три компонента TEdit с вкладки Standard и над каждым из них поместите компонент TLabel. Задайте им заголовки 'Host' (и имена HostLLabel, HostEdit), 'UserName' (UserLabel, UserEdit) и 'Password' (PasswdLabel и PasswdEdit). Как вариант вы можете использовать компонент TLabelledEdit с закладки Additional. 

Найдите свойство PasswordChar у компонента PasswdEdit, измените его на * или любой другой символ, это приведет к тому, что когда вы вводите пароль, то вместо пароля будет отображаться именно этот символ (несколько символов). Убедитесь, что свойство Text у этого компонента не содержит текста. 

Теперь расположите еще один компонент Tedit и TLabel в правом верхнем углу формы. Измените заголовок на 'Enter SQL Command' и переименуйте компонент в CommandEdit. 

Поместите на форму три кнопки, две слева под компонентами TEdit и одну справа под компонентом для команд. 

Измените свойства Label (Name) левых кнопок на 'Connect to Database' (ConnectButton) и 'Exit' (ExitButton) и правой - 'Send Query' (QueryButton). 

Расположите большой TMemo с заголовком 'Results' (ResultMemo) слева внизу так, чтобы он заполнил все оставшееся место. Найдите свойство ScrollBars и установите значение ssAutoBoth, чтобы компонент мог прокручивать текст и вертикально и горизонтально. Свойство WordWrap установите в True. 

Поместите на форму статусную строку (TStatusBar с закладки Common Controls) внизу формы и впишите в свойство SimpleText строку 'TryMySQL'. 

[Вот что должно получится](<http://lazarus-ccr.sourceforge.net/kbdata/trymysqldb.png>)

Теперь нам потребуется написать несколько обработчиков. 

У нас на форме слева три компонента TEdit для ввода имени хоста, пользователя и пароля. Как только в них будут введены необходимые данные, нужно нажать на кнопку Connect. Обработчик OnClick для этой кнопки частично базируется на модуле для FPC, который мы привели выше. 

Результаты запросов к базе данных не могут быть сразу выведены с помощью операторов Pascal write или writeln, сначала их необходимо преобразовать в строки и вывести в компонент TMemo. 

Хотя операторы Pascal write и writeln и приспособлены для преобразования типов «на лету», использование компонента TMemo для вывода текста требует прямого преобразования типов в строковую форму, так переменные типа Pchar должны быть преобразованы в строки путем использования StrPas, и числа должны конвертироваться функцией IntToStr. 

Добавим строки в TMemo используя 
    
    
    procedure ShowString (S : string);
    (* display a string in a Memo box *)
    begin
          trymysqlForm1.ResultsMemo.Lines.Add (S)
    end;
    

Таким образом, обработчик ConnectButton становится следующим: 
    
    
    procedure TtrymysqlForm1.ConnectButtonClick(Sender: TObject);
    (* Connect to MySQL using user data from Text entry boxes on Main Form *)
    var strg: string;
    
    begin
    
     dummy1 :=  trymysqlForm1.HostEdit.text+#0;
     host := @dummy1[1];
     dummy2 := trymysqlForm1.UserEdit.text+#0;
     user := @dummy2[1] ;
     dummy3 := trymysqlForm1.PasswdEdit.text+#0;
     passwd := @dummy3[1] ;
     alloc := mysql_init(PMYSQL(@qmysql));
     sock :=  mysql_real_connect(alloc, host, user, passwd, database, 0, nil, 0);
     if sock=Nil then
       begin
         strg :='Couldnt connect to MySQL.'; showstring (strg);
         Strg :='Error was: '+ StrPas(mysql_error(@qmysql)); showstring (strg);
      end
       else
       begin
         trymysqlForm1.statusBar1.simpletext := 'Connected to MySQL';
         strg := 'Now choosing database : ' + database; showstring (strg);
    {$ifdef Unix}
         strg :='Mysql_port      : '+ IntToStr(mysql_port); showstring (strg);
         strg :='Mysql_unix_port : ' + StrPas(mysql_unix_port); showstring (strg);
    {$endif}
         Strg :='Host info       : ' + StrPas(mysql_get_host_info(sock));
         showstring (strg);
         Strg :='Server info     : ' + StrPas(mysql_stat(sock)); showstring (strg);
         Strg :='Client info     : ' + Strpas(mysql_get_client_info);  showstring (strg);
     
         trymysqlForm1.statusbar1.simpletext := 'Selecting Database ' + DataBase +'...';
     if mysql_select_db(sock,DataBase) < 0 then
     begin
       strg :='Couldnt select database '+ Database; ShowString (strg);
       Strg := mysql_error(sock); ShowString (strg);
     end
     end;
    end;
    

Поле для ввода справа позволяет вводить команды SQL без завершающей точки-с-запятой; как только вы удовлетворитесь синтаксисом - жмите кнопку SendQuery - запрос выполнится и результат будет отображен в компоненте ResultsMemo. 

Обработчик SendQuery опять же базируется на версии FPC, за исключением использования преобразования типов для последующего отображения строк. 

Разница между программой на FPC и этой программой в том, что в случае возникновения ошибки программа на Free Pascal завершит свою работу и сессия MySQL останется открытой, в то же время во второй программе контроль вернется в главную форму и можно повторить попытку. В конечном итоге приложение нормально завершает свою работу (закрывая при этом сессию MySQL) при нажатии кнопки Exit. 

Ниже приведен код обработчика кнопки SendQuery: 
    
    
    procedure TtrymysqlForm1.QueryButtonClick(Sender: TObject);
    var
     dumquery, strg: string;
    begin
        dumquery := TrymysqlForm1.CommandEdit.text;
        dumquery := dumquery+#0;
        query := @dumquery[1];
        trymysqlForm1.statusbar1.simpletext := 'Executing query : '+ dumQuery +'...';
        strg := 'Executing query : ' + dumQuery; showstring (strg);
        if (mysql_query(sock,Query) < 0) then
        begin
          Strg :='Query failed '+ StrPas(mysql_error(sock)); showstring (strg);
        end
        else
        begin
          recbuf := mysql_store_result(sock);
          if RecBuf=Nil then
          begin
            Strg :='Query returned nil result.'; showstring (strg);
          end
          else
          begin
            strg :='Number of records returned  : ' + IntToStr(mysql_num_rows (recbuf));
            Showstring (strg);
            Strg :='Number of fields per record : ' + IntToStr(mysql_num_fields(recbuf));
            showstring (strg);
            rowbuf := mysql_fetch_row(recbuf);
            while (rowbuf <>nil) do
            begin
                 Strg :='(Id: '+ rowbuf[0]+', Name: ' + rowbuf[1]+ ', Email : ' +
                  rowbuf[2] +')';
                 showstring (strg);
                 rowbuf := mysql_fetch_row(recbuf);
            end;
          end;
        end;
    end;
    

Сохраните свой проект и жмите Запуск -> Запуск 

Полный листинг программы доступен [тут](<http://lazarus-ccr.sourceforge.net/kbdata/mysqldemo.tar.gz>)

## Lazarus и PostgreSQL

Очень краткое руководство по подключению Lazarus 0.9.12 (или более поздней версии) к базе данных PostgreSQL, локально или удаленно, используя компонент TPQConnection. 

  * После корректной установки, выполните следующие шаги: 
    * Поместите на форму компоненты PQConnection, SQLQuery, SQLTransaction с вкладки SQLdb
    * Поместите компонент DataSource с вкладки DataAccess
    * Поместите DBGrid с вкладки DataControls


  * В компоненте PQConnection заполните свойства: 
    * transaction – ссылкой на объект SQLTransaction
    * DatabaseName
    * HostName
    * UserName + password



Проверьте, что SQLTransaction автоматически привязался к PQConnection 

  * В компоненте SQLQuery заполните свойства: 
    * transaction соответствующим объектом
    * database
    * SQL (что-то вроде 'select * from anytable')



В компоненте DataSource установите свойство DataSet который должен указываить на объект SQLQuery В компоненте DBGrid установите в свойстве datasource указатель на объект DataSource Включайте и активируйте все. DBGrid должен показать данные прямо во время разработки. TDBText и TDBEdit работают, но у меня только отображают данные. Для внесения изменений в базу данных я вызываю запрос вставки для базы напрямую, используя следующий код: 
    
    
    try
      sql:= 'UPDATE table SET setting=1';
      PQDataBase.Connected:=True;
      PQDataBase.ExecuteDirect('Begin Work;');
      PQDataBase.ExecuteDirect(sql);
      PQDataBase.ExecuteDirect('Commit Work;');
      PQDataBase.Connected:=False;
    except
      on E : EDatabaseError do
        MemoLog.Append('DB ERROR:'+sql+chr(13)+chr(10)+E.ClassName+chr(13)+chr(10)+E.Message);
      on E : Exception do
        MemoLog.Append('ERROR:'+sql+chr(13)+chr(10)+E.ClassName+chr(13)+chr(10)+E.Message);
    end;
    

Примечания: Проверено в Windows, Lazarus 0.9.12 + PgSQL 8.3.1 Некоторые тесты в linux, Lazarus 0.9.12 и PgSQL 8.0.x 

Установка и ошибки: В тестируемой версии Lazarus .12, поля типа "text" и "numeric" имеют ошибки Я без проблем использовал строки фиксированной длины, int и float8 Иногда перезапуск Lazarus позволяет обойти тупые ошибки... После каких-то ошибок транзакция может остаться активной и требуется ручное ее закрытие Изменения, сделанные в Lazarus, конечно же не видны до тех пор, пока транзакция не завершена (commit) Встроенный отладчик немного глюкавый (по крайней мере в windows) – иногда внешний (запущенный помимо IDE) может помочь найти ошибки. В Линукс некоторые сообщения об ошибках выводятся в консоль – запускайте вашу программу в командной строке, иногда можно получить немного полезной отладочной информации. Ошибка: "Can not load Postgresql client. Is it installed (libpq.so) ?" Добавьте путь для поиска libpq* из инсталляции PostgreSQL. В Linux добавьте путь к файлу libpq.so в секцию libraries в файле /etc/fpc.cfg file. Например: -Fl/usr/local/pgsql/lib В windows, добавьте пути к библиотекам в переменную Path или в папку проекта В windows я копировал все DLLs из папки C:\Program Files\PostgreSQL\8.1\bin в папку, которая была в переменной PATH Или добавьте папку postgres\bin в path. 

## Lazarus и SQLite

Luiz Américo 

Посетите страничку [sqlite4fpc](<http://sqlite4fpc.yolasite.com/>), чтобы найти описание API и дополнительную документацию. 

TSqliteDataset и TSqlite3Dataset сейчас входят в стандартную поставку Lazarus. 

### Введение

TSqliteDataset и TSqlite3Dataset являются потомками TDataset, предоставляющими доступ к базам данных SQLite версий 2.8.x и 3.x.x соответственно. Ниже перечислены основные достоинства и недостатки: 

Достоинства: 

  * Гибкость: программист может выбирать - использовать или нет язык SQL, что позволяет ему работать с простым макетом таблицы или с комплексным макетом любой сложности, насколько это позволяет SQL/sqlite
  * Автоматическое обновление данных: нет необходимости обновлять данные вручную, используюя команды SQL - один метод берет на себя все заботы
  * Скорость: данные кэшируются в оперативной памяти, что позволяет быстрее просматривать данные
  * Не требуется установка/настройка сервера: просто поставляйте программу вместе с библиотекой sqlite



Недостатки: 

  * Требуется дополнительный файл (библиотека sqlite).



### Требования

  * Для баз данных sqlite2: 
    * fpc 2.0.0 или выше
    * Lazarus 0.9.10 или выше
    * runtime-библиотека sqlite 2.8.15 или выше (можно взять с [www.sqlite.org](<http://www.sqlite.org>))


  * Для баз данных sqlite3: 
    * fpc 2.0.2 или выше
    * Lazarus 0.9.11 (svn revision 8443 или выше)
    * runtime-библиотека sqlite 3.2.1 или выше (можно взять с [www.sqlite.org](<http://www.sqlite.org>))



Прежде чем начать проект lazarus, убедитесь что: 

  * библиотека sqlite доступна по пути PATH или находится в каталоге программы
  * под Linux, используйте cmem первым модулем в предложении uses главной программы (модуля проекта) 
    * В Debian, Ubuntu и других Debian-like дистрибутивах (также относится к OpenSuse), в целях сборки Lazarus IDE вы должны установить пакеты libsqlite-dev/libsqlite3-dev, не только sqlite/sqlite3.



### Как работать (Основы)

Установите пакет из каталога /components/sqlite (смотрите инструкцию) (Пакет → Настройка установленных пакетов → установить sqlite3laz) 

Во время разработки установите следующие свойства: 

  * FileName: путь к файлу базы данных [обязательно]
  * TableName: название таблицы используемой в sql выражении [обязательно]
  * Sql: SQL select выражение [необязательно]



**Создание таблицы (Dataset)**

Сделайте двойной щелчок мышкой по иконке компонента или используйте пункт 'Create Table' в выпадающем меню, которое появляется после щелчка правой кнопкой мыши по компоненту. Перед Вами появится окно редактора таблицы. 

Ниже даны все типы данных поддерживаемые TSqliteDataset и TSqlite3Dataset: 

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

После создания таблицы, откройте базу данных методом Open. Если свойство SQL не установлено, то будут выбраны все записи из всех полей, что эквивалентно следующему SQL выражению: 
    
    
    SQL:='Select * from TABLENAME'; 
    

**Закрепление изменений в файле базы данных**

Используйте метод ApplyUpdates. При этом dataset должно содержать минимум одно поле которое требуется для Первичного Ключа (значения поля должны быть UNIQUE и не NULL) Это возможно достичь двумя путями: 

  * Установите свойство PrimaryKey, дав ему имя поля, которое будет Первичным Ключом
  * Добавьте поле типа AutoInc (Это поле часто используется в TSqliteDataSet автоматически как Первичный Ключ)



Если одно из двух действий выполнено, вызывайте 
    
    
    ApplyUpdates;
    

PS1: Если выполнены оба действия, то для ApplyUpdates используется поле установленное в свойстве PrimaryKey. 

PS2: Установка поля в PrimaryKey которое не является Первичным Ключём может привести к повреждению данных при вызове ApplyUpdates. Так же убедитесь, что используемое поле содержит уникальные и не Null значения перед тем как использовать это. 

### Примечания

  * Хотя это было проверено с таблицей в 10000 записей и работало прекрасное, однако TSqliteDataset хранит все данные в памяти, так что не забудьте извлекать только необходимые данные (это в первую очередь касается Memo Полей)
  * Один и тот же файл данных (свойство Filename) может содержать несколько таблиц/наборов данных
  * Несколько наборов данных (различные комбинации полей) могут быть созданы, используя одну и ту же таблицу одновременно
  * Возможно фильтровать данные используя оператор WHERE в sql выражении, закрывая и вновь открывая набор данных (или вызывая метод RefetchData). Но в этом случае расположение и число полей должны оставаться одинаковыми
  * Также возможно применение сложных операторов SQL, используя псевдонимы, объединения, представления из нескольких таблиц (помните, что они должны постоянно находится в том же файле данных), но в этом случае ApplyUpdates не будет работать. Если кто-то хочет использовать сложные запросы и применить обновления файла данных, сообщите мне по почте и я дам некоторые советы, как это сделать
  * Устанавить имя файла при несозданном sqlite2.x файле данных и если после открыть TSqliteDataset, то это возможно, но у некоторых полей не будут обнаружены правильный тип поля. Они будут обработаны как строковые поля.



Примеры для sqlite можно найти в каталоге fpc/fcl/db/sqlite 

Luiz Américo pascalive(at)bol(dot)com(dot)br 

## Lazarus и MSSQL

На сегодняшний день с MSSQL можно работь через: 

  * встроенный драйвер SQLDB (последний Lazarus/FPC 2.6.1+ из исходников, недоступен как stable download): TMSSQLConnection (MS SQL) и TSybaseConnection (Sybase ASE)
  * Zeos (свежий CVS, см. ссылки на Zeos на данной странице)
  * ODBC (MS SQL и Sybase ASE)



Пожалуйста, обратитесь к справке FPC и Lazarus, чтобы подробнее узнать про SQLDB для Sybase ASE и MS SQL Server. 

## Lazarus и Firebird / Interbase

Посмотрите [Установка пакетов](<http://wiki.lazarus.freepascal.org/Install_Packages>). На этой странице расположен небольшой пример для начинающих, как соединиться с сервером FireBird или InterBase. 

### FBLib Firebird Library

[FBLib](<http://fblib.altervista.org/>) \- это OpenSource библитотека для непосредственного доступа к серверу из Borland Delphi / Kylix, Freepascal и Lazarus. 

Текущая реализация включает: 

  * Непосредственный доступ к Firebird 1.0.x, 1.5.x и 2.0 Classic или SuperServer
  * Кроссплатформенность [Win32,Gnu/Linux,FreeBSD)
  * Автоматический выбор клиентской библиотеки 'fbclient' или 'gds32'
  * Запросы с параметрами
  * Поддержка SQL диалектов 1 и 3
  * LGPL лицензия
  * Получение метаданных
  * Простой синтаксический анализатор скриптов
  * Добавляется только 100-150 KБ к экзешнику
  * Поддержка BLOB-полей
  * Выгрузка данных в HTML, SQL-скрипт
  * Сервисные функции (backup,restore,gfix...)
  * Обработчик событий



Вы можете загрузить документацию с сайта [FBLib's website](<http://fblib.altervista.org/>). 

## Lazarus и dBase

[Tony Maro](</User:Tonymaro> "User:Tonymaro")

Вы можете также посетить [TDbf Страница руководства](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial/ru"). 

FPC содержит простой компонент базы данных под названием "TDbf", который функционально подобен компоненту TTable в Delphi ([вебсайт TDbf](<http://tdbf.sourceforge.net/>)), поддерживающий все основные функции по работе с файлами dBase. Этот компонент не устанавливается по умолчанию, поэтому Вы должны сами установить пакет из каталога "lazarus/components/tdbf" и пересобрать Lazarus. Он появится рядом с компонентом TDatasource на вкладке "Data Access". 

Компонент TDbf имеет преимущество перед другими компонентами баз данных, т.к. не требует никаких внешних библиотек, однако это не самая лучшая идея для больших приложений баз данных. 

Компонент очень удобен в применении. Просто поместите TDbf на свою форму, установите путь к каталогу, в котором будут Ваши файлы базы данных, задайте название таблицы, и свяжите его с Вашим компонентом TDatasource. 

Реальная программа требует немного большего усилия, как бы то ни было. Если таблица ещё не существует, то Вы должны будете её создать программно до открытия. **Примечание** : Версия OpenOffice 2.0x и выше содержит OpenOffice Base, которое может создать dbf файлы более дружественным способом. 

Попытка открыть несуществующую таблицу вызовет ошибку. Таблицы могут быть созданы программно через компонент после того, как во время выполнения будут заданы путь и название таблицы. 

Например, чтобы создать таблицу с названием "dvds", чтобы сохранить Вашу dvd коллекцию, Вы поместите компонент на свою форму, установите во время выполнения путь к каталогу с базой и установите название таблицы в "dvds". Получающийся файл будет назван "dvds.dbf". В свой код вставьте такие строки: 
    
    
        Dbf1.FilePathFull := '/path/to/my/database';
        Dbf1.TableName := 'dvds';
        With Dbf1.FieldDefs do begin
            Add('Name', ftString, 80, True);
            Add('Description', ftMemo, 0, False);
            Add('Rating', ftString, 5, False);
        end;
        Dbf1.CreateTable;
    

Когда этот код будет выполнен, cоздасться таблица коллекции DVD. После этого данные можно будет отображать в компонентах просмотра данных, связав их через TDatasource с этим компонентом. 

Добавление индексов является немного отличным от типичного TTable. Это должно быть сделано после того, как база данных открыта. Этот же метод используется и для того, чтобы перестроить индексы. Например: 
    
    
        Dbf1.Exclusive := True;
        Dbf1.Open;
        Dbf1.AddIndex('dvdsname','Name',[ixPrimary, ixUnique, ixCaseInsensitive]);
        Dbf1.AddIndex('rating.ndx', 'Rating', [ixCaseInsensitive]);
        Dbf1.Close;
    

Первичный индекс будет файлом с названием "dvdsname.mdx", а второй будет файл с названием "rating.ndx". Однако в базе данных с множеством таблиц, Вы должны позаботиться об уникальности имени файлов индексов. 

**Примечание**. Для версий dBase, которые поддерживают несколько индексов в одном файле, расширение в имени индекса указывать не надо, только имя индекса. Тогда все индексы будут созданы в одном файле с названием таким же, как и имя таблицы, но с расширением ".mdx". 

Для того, чтобы указать конкретную версию таблицы, нужно задать свойство TDbf.TableLevel. 

### Поиск и отображение набора данных

[Simon Batty](</index.php?title=User:Svbfieldview.net&action=edit&redlink=1> "User:Svbfieldview.net \(page does not exist\)")

В этом примере я хочу найти в базе данных книг все названия, у которых есть автор и затем отобразить список в мемобоксе: 
    
    
    Dbf1.FilePathFull := '/home/somelocatio/database_location/';   // путь к базе данных
      Dbf1.TableName := 'books.dbase';                     // файл таблицы, включая расширение
      DbF1.Open;
      memo1.Clear;                                           // очистка мемобокса
    
       Dbf1.FilterOptions := [foCaseInsensitive];
       Df1.Filter := 'AU=' + QuotedStr('anauthor'); // AU это поле, где содержится имя автора
    
        Dbf1.Filtered := true;   // Включаем фильтрование
        Dbf1.First;                   // Переходим в начало отфильтрованных данных
      while not dbf1.EOF do           //  В цикле проходим весь полученый набор данных
         begin
           memo1.Append(Dbf1.FieldByName('TI').AsString); //TI это поле названий
           dbf1.next;                                // Переход на следующую запись
         end;
    
    Dbf1.Close;
    

Заметьте, что Вы можете использовать Ddf1.findfirst, чтобы перейти на первую запись в отфильтрованном наборе, затем используйте Dbf1.next, чтобы перемещаться по данным. Я нашел, что использование Dbf1.Findnext подвешивает программуь. 

## Ссылки, связанные с этой статьей

  * [Создание приложений для баз данных с помощью Lazarus используя модуль DB](<../en/Databases.md> "Databases")  

  * [Создание приложений для баз данных MySQL](<../en/MySQLDatabases.md> "MySQLDatabases")  

  * [Создание приложений баз данных с использованием компонентов Zeos](<../en/Zeos_tutorial.md> "Zeos tutorial")
  * [Pascal Data Objects](<http://pdo.sourceforge.net>) API для доступа к базам данных программами FPC и Delphi, и использующие родную библиотеку MySQL версий 4.1 и 5.0, и Firebird SQL 1.5 и 2.0. Сделано на подобие класса PDO в PHP.



## Ответственность и изменения

Эта страница сформирована из epikwiki [version](<http://lazarus-ccr.sourceforge.net/index.php?wiki=LazarusDatabase>).

---

_Source: [https://wiki.freepascal.org/Lazarus_Database_Tutorial/ru](https://web.archive.org/web/20250124195423/https://wiki.freepascal.org/Lazarus_Database_Tutorial/ru)_
