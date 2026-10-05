# fpWeb Tutorial

│ **[English (en)](<../en/fpWeb_Tutorial.md>)** │  **русский (ru)** │

    
     Первоначально основано на учебнике fcl-web (в формате PDF) пользователя форума [Leledumbo](<https://forum.lazarus.freepascal.org/index.php?action=profile;u=7523>).
    

## Contents

  * 1 Введение
  * 2 Архитектура (ПОЖАЛУЙСТА, прочтите)
    * 2.1 Приложение
    * 2.2 Веб-модули
  * 3 Установка
  * 4 Специализированные модули
  * 5 Hello, World!
  * 6 Чтение данных GET и POST
    * 6.1 Чтение GET
    * 6.2 Чтение POST
    * 6.3 Чтение загружаемых файлов
  * 7 Cookies
    * 7.1 Отправка
    * 7.2 Получение
  * 8 Сессии
    * 8.1 Активация
    * 8.2 Управление переменными сессии
    * 8.3 Завершение
  * 9 Маршрутизация
    * 9.1 Старый способ
      * 9.1.1 Использование нескольких модулей
      * 9.1.2 С использованием Actions
        * 9.1.2.1 Поток обработки запросов (request)
        * 9.1.2.2 Добавление действий в веб-модули
        * 9.1.2.3 Действие по умолчанию
    * 9.2 Новый механизм
      * 9.2.1 Синтаксис маршрута
      * 9.2.2 Регистрация маршрута
      * 9.2.3 Пример веб-сервера
  * 10 Использование шаблонов
    * 10.1 На уровне действий
    * 10.2 На уровне модуля
    * 10.3 Использование отдельного шаблона
  * 11 Использольвание html Producer object
  * 12 Советы и хитрости
    * 12.1 Возврат другого кода ответа HTTP
    * 12.2 Перенаправление запроса на другой URL
    * 12.3 Обслуживание статических файлов (встроенный веб-сервер)
    * 12.4 Централизованное управление конфигурацией и модулями
    * 12.5 Элегантное завершение (FastCGI / встроенный веб-сервер)
    * 12.6 Пользовательский обработчик исключений
    * 12.7 Чисто ручное кодирование (дизайнер форм не требуется)
  * 13 См.также



## Введение

fpWeb - это фреймворк веб-приложений, поставляемый FPC в своем дистрибутиве по умолчанию как часть пакета fcl-web. Сам фреймворк построен на основе функций fcl-web. Фреймворк построен с мыслью о RAD, чтобы эффективно использовать компонентность при создании динамического контента. Предоставляется пакет Lazarus, который может использовать фреймворк методом перетаскивания для управления сеансами и создания контента. В этом руководстве мы попытаемся охватить основные функции fpWeb, чтобы с его помощью можно было создать обычное веб-приложение. Обратите внимание, что это руководство не пытается научить вас HTTP-протоколу, HTML, CSS, JavaScript или манипуляциям с базой данных, поскольку владение протоколом и клиентскими языками должно быть предварительным условием работы для каждого программиста веб-приложений, а манипуляции с базой данных не отличаются от десктопной реализации. 

  


## Архитектура (ПОЖАЛУЙСТА, прочтите)

Перед тем, как начать, необходимо знать архитектуру и поток приложения, чтобы избежать путаницы, когда определенные вещи не работают или работают неожиданно. Поэтому, пожалуйста, потратьте немного времени на чтение этого раздела. 

### Приложение

Под приложением здесь понимается протокол, который будет реализовывать ваше приложение. fpWeb с радостью переключится с CGI, FCGI, с модуля Apache на встроенный сервер и т.д., если fcl-web реализует еще что- нибудь в будущем. Каждое приложение реализовано в своем собственном модуле, поэтому для переключения с одного приложения на другое, за исключением модуля Apache, нужно просто изменить соответствующий идентификатор в разделе uses. В настоящее время (по состоянию на 3.0.0 / 3.1.1) это: 

  * fpCGI -> CGI
  * fpFCGI -> FastCGI
  * fpApache (также требуется httpd) -> модуль Apache
  * fpHttpApp -> встроенный сервер
  * microhttpapp -> встроенный сервер с использованием библиотеки GNU libmicrohttp.
  * fphttpsys -> поддержка системой Windows протокола HTTP.



В этом руководстве мы будем использовать встроенный сервер для простоты, потому что вам не придется иметь дело с настройкой виртуального сервера и путаницей со сложным файлом конфигурации и управлением службами. Ваше приложение будет единым переносным бинарным веб-приложением! Другая причина может заключаться в том, что существует больше, чем одно приложение веб-сервера, и каждое имеет свой способ настройки. Было бы излишним пытаться охватить их всех, пока их документация уже выполняет свою работу. Модуль Apache реализован как (динамическая) библиотека, тогда как другие протоколы являются обычным приложением. Каждое приложение может иметь определенные свойства (например, порт), доступные и значимые только для этого приложения. Вот почему, если вы посмотрите на примеры fcl-web, пары .lpi / .lpr для каждого протокола помещаются в свои собственные каталоги, только веб-модули являются общими. 

### Веб-модули

[![fpWeb-overview.png](https://wiki.freepascal.org/images/6/6f/fpWeb-overview.png)](</File:fpWeb-overview.png>)

[](</File:fpWeb-overview.png> "Enlarge")

Приложения fpWeb состоят из веб-модулей, которые фактически создают контент. Веб-модуль может содержать веб-действия, которые могут еще больше разделить функциональность. Например, веб-модуль аутентификации может иметь веб-действия входа и выхода. Хотя модуль about web может вообще не нуждаться в действии и обслуживает только один контент. Веб-модуль интегрирован с fpTemplate, который можно использовать для создания динамического контента из файла шаблона. Это примерно похоже на то, что делает PHP, только разрыв между логикой и представлением скорее вынужденный, чем предполагаемый. Некоторые говорят, что fpTemplate реализует пассивное представление, в то время как PHP по умолчанию реализует шаблон проектирования активного представления. 

## Установка

Пакет fpWeb для Lazarus по умолчанию не установлен (но идет в стандартной поставке). Чтобы включить fpWeb: 

  1. Откройте Lazarus и выберите **Package- >Install/Uninstall Package**
  2. В списке Available (Доступно) для установки найдите **weblaz** и нажмите **Install selection** (Установить выбранное). Нажмите **Save and rebuild IDE**(Сохранить и пересобрать IDE) и подтвердите, нажав **Continue**(Продолжить).
  3. Позвольте IDE пересобраться и перезапуститься. Если все пойдет хорошо, у вас должна появиться вкладка fpWeb на палитре компонентов, как показано ниже:



    

    [![Installed weblaz package](https://wiki.freepascal.org/images/d/d2/fpwebtutorial_inst.png)](</File:fpwebtutorial_inst.png> "Installed weblaz package")

## Специализированные модули

Класс*TFPWebModule* (используемый ниже) - это простой пример модуля fpWEB, который можно использовать для всех видов HTTP-запросов. 

Однако fpWEB поставляется с некоторыми специализированными модулями, которые имеют дополнительную поддержку для специализированных задач: 

  * Класс **TSimpleFileModule** в модуле _fpwebfile.pp_ можно использовать для отправки файлов. Вы указываете ему каталог, и он делает все остальное.
  * Класс **TFPHTMLModule** в модуле _fphtml.pp_ можно использовать для создания HTML.
  * Класс **TProxyWebModule** в модуле _fpwebproxy.pp_ \- это готовый прокси для пересылки.
  * Класс **TFPWebProviderDataModule** в модуле _fpwebdata.pp_ обслуживает данные в формате JSON, которые могут использоваться хранилищами ExtJS.
  * Класс **TSQLDBRestModule** в модуле _sqldbrestmodule.pp_ реализует полноценный сервер REST, поддерживаемый SQLDB. См. дополнительную информацию в [SQLDBRestBridge](<../en/SQLDBRestBridge.md> "SQLDBRestBridge").
  * Класс **TJSONRPCModule** в модуле _webjsonrpc.pp_ реализует службу JSON-RPC.
  * Класс **TExtDirectModule** в модуле _fpextdirect.pp_ реализует вариант Ext.Direct службы JSON-RPC.



## Hello, World!

Создадим простое веб-приложение. Как обычно преподают при изучении программирования: "Hello, World!" будет нашим первым приложением.  
  


1\. Откройте Lazarus и выберите **Project- >New Project**, затем выберите **HTTP server Application**

    [![Создать новое приложение HTTP-сервера](https://wiki.freepascal.org/images/c/c5/fpwebtutorial_create_app.png)](</File:fpwebtutorial_create_app.png> "Создать новое приложение HTTP-сервера")  


2\. Появится другое диалоговое окно для обслуживания статических файлов, выбора порта и многопоточности. Просто используйте порт по умолчанию 8080. 

    
    
    
    You may skip the static files serving (go to tips and tricks section if you want to know it more).
    

(_Вы можете пропустить обслуживание статических файлов (перейдите в раздел советов и рекомендаций, если хотите узнать больше)_) 

    [![Static files, port selection and multithreading options](https://wiki.freepascal.org/images/7/7f/fpwebtutorial_port_selection.PNG)](</File:fpwebtutorial_port_selection.PNG> "Static files, port selection and multithreading options")

  


    **ВАЖНО!:**
    Если вы решите использовать потоки на *nix, не забудьте добавить cthreads в качестве первого модуля в разделе uses .lpr, в противном случае будет создан RTE 232. При запуске с консоли должно отображаться сообщение:
    
    
    
    This binary has no thread support compiled in. Recompile the application with a thread-driver in the program uses clause before other units using thread.
    

(_В этом двоичном файле не скомпилирована поддержка потоков. Перекомпилируйте приложение с драйвером потока в программе using предложение перед другими модулями, использующими поток_)   
  
3\. С 14 января 2017г. (или FPC 3.0.4) вам может потребоваться открыть .lpr и добавить следующую строку в основной текст, если ее еще там нет: 

    
    
    
    Application.LegacyRouting := true;
    

    причина будет объяснена в главе [Маршрутизация](<fpWeb_Tutorial.md> "fpWeb Tutorial/ru").

  
4\. Что бы вы ни выбрали, нажмите "OK", и вы будете представлены в приложении fpWeb с одним модулем по умолчанию.  


5\. Переместите фокус на модуль и перейдите в **Object Inspector**. Не стесняйтесь переименовывать модуль, если хотите.  


6\. Выберите вкладку **Events** и нажмите кнопку справа от второго столбца в строке**OnRequest** , чтобы создать обработчик событий. 

    [![Создание обработчика OnRequest веб-модуля в object inspector](https://wiki.freepascal.org/images/1/1a/fpwebtutorial_on_request.png)](</File:fpwebtutorial_on_request.png> "Создание обработчика OnRequest веб-модуля в object inspector")  


    Вы будете перенаправлены в редактор исходного кода со следующим кодом:
    
    
    
    procedure TFPWebModule1.DataModuleRequest(Sender: TObject; ARequest: TRequest;
    AResponse: TResponse; var Handled: Boolean);
    begin
      |
    end;
    

    Заполните событие:

    
    
    
    procedure TFPWebModule1.DataModuleRequest(Sender: TObject; ARequest: TRequest;
    AResponse: TResponse; var Handled: Boolean);
    begin
      AResponse.Content := 'Hello, World!';
      Handled := true;
    end;
    

  


7\. Теперь запустите приложение (или нажмите F9).  


8\. Откройте браузер и введите: 

    <http://localhost:8080/>   


9\. Вы должны увидеть отображающееся "Hello, World!".  
  


Если это не так, проверьте ниже: 

  * Фреймворк выполняет много операций по обработке исключений, и отладчик IDE может их поймать и прервать работу вашего приложения. Можно добавить большинство исключений в список игнорирования, чтобы вы могли больше сосредоточиться на потоке приложения. Продолжайте пропускать и продолжайте, пока не перестанет появляться диалоговое окно и браузер не покажет результат.


  * **Handled := true** \- это способ, которым мы сообщаем фреймворку, что запрос был обработан. Если вы не установите его (или установите для него значение **false**), вместо этого будет отображаться страница с ошибкой. На данный момент это не влияет на поток запросов, но будет позже. Так что держите это так, пока не придет время, чтобы использовать его с пользой.


  * другой трек: тест без брандмауэра, загруженного в ОЗУ (как приложение, как сервис или демон, или как оба).



## Чтение данных GET и POST

Динамический контент, скорее всего, будет запускаться из пользовательского ввода, либо через формы, предоставляя значения в URL-адресе, и т. Д. Эти данные отправляются вместе с запросом, который представлен в методе как **ARequest** параметр типа **TRequest**. 

### Чтение GET

Данные GET предоставляются как **ARequest.QueryFields** , который является потомком **TStrings**. Короче говоря, все, что вы обычно делаете с TStrings, применимо здесь, например, доступ к данным в стиле карты через свойство **Values**. 

Повторно используя приведенный выше код, замените тело метода на: 
    
    
    procedure TFPWebModule1.DataModuleRequest(Sender: TObject; ARequest: TRequest;
     AResponse: TResponse; var Handled: Boolean);
    var
      LName: String;
    begin
      LName := ARequest.QueryFields.Values['Name'];
      if LName = EmptyStr then
        with AResponse.Contents do
        begin
          Add('<form action="' + ARequest.URI + '" method="GET"');
          Add('<label for="name">Please tell me your name:</label>');
          Add('<input type="text" name="name" id="name" />');
          Add('<input type="submit" value="Send" />');
          Add('</form>');
        end
      else
        AResponse.Content := 'Hello, ' + LName + '!';
      Handled := true;
    end;
    

**ARequest.URI** \- это просто ссылка на текущий URI, поэтому даже когда вы меняете зарегистрированный модуль или имя действия, этот код остается прежним. 

Обратите внимание, что, как и в Паскале, обращение к данным осуществляется без учета регистра. 

Теперь вы можете попробовать запросить /, который отобразит 
    
    
     Please tell me your name
    

и /?name=<напишите здесь что угодно, например: Bob>, который отобразит 
    
    
     Hello, Bob!
    

### Чтение POST

POST на самом деле не сильно отличается от GET, отличается только тем, к какому свойству получить доступ. Если доступ к GET осуществляется через **ARequest.QueryFields** , доступ к POST осуществляется через **ARequest.ContentFields**. Стиль POST предыдущего кода: 
    
    
    procedure TFPWebModule1.DataModuleRequest(Sender: TObject; ARequest: TRequest;
     AResponse: TResponse; var Handled: Boolean);
    var
      LName: String;
    begin
      LName := ARequest.ContentFields.Values['Name'];
      if LName = EmptyStr then
        with AResponse.Contents do
        begin
          Add('<form action="' + ARequest.URI + '" method="POST"');
          Add('<label for="name">Please tell me your name:</label>');
          Add('<input type="text" name="name" id="name" />');
          Add('<input type="submit" value="Send" />');
          Add('</form>');
        end
      else
        AResponse.Content := 'Hello, ' + LName + '!';
      Handled := true;
    end;
    

### Чтение загружаемых файлов

Единственное исключение - чтение полей **multipart/form-data** , то есть файлов. Оно доступно в **ARequest.Files** как экземпляр **TUploadedFiles** , который является потомком **TCollection**. Ниже приведен общедоступный интерфейс TUploadedFiles, который вы можете использовать для доступа к файлам: 
    
    
    TUploadedFiles = Class(TCollection)
    ...
    public
      Function First: TUploadedFile;
      Function Last: TUploadedFile;
      Function IndexOfFile(AName: String) : Integer;
      Function FileByName(AName: String) : TUploadedFile;
      Function FindFile(AName: String) : TUploadedFile;
      Property Files[Index: Integer] : TUploadedFile read GetFile Write SetFile; default;
    end;
    

Каждый **TUploadedFile** сам по себе имеет несколько свойств: 
    
    
    TUploadedFile = Class(TCollectionItem)
    ...
    Public
      Destructor Destroy; override;
      Property FieldName: String Read FFieldName Write FFieldName;
      Property FileName: String Read FFileName Write FFileName;
      Property Stream: TStream Read GetStream;
      Property Size: Int64 Read FSize Write FSize;
      Property ContentType: String Read FContentType Write FContentType;
      Property Disposition: String Read FDisposition Write FDisposition;
      Property LocalFileName: String Read FLocalFileName Write FLocalFileName;
      Property Description: String Read FDescription Write FDescription;
    end;
    

Они должны быть достаточно информативными, за исключением **FileName** и **LocalFileName**. **FileName** \- это **name** исходного файла, загружаемого с клиента, **LocalFileName** \- это путь к файлу на сервере, где файл временно хранится. Обратите внимание на разницу, выделенную жирным шрифтом выше. 

Опять же, повторное использование того же обработчика запросов: 
    
    
    procedure TFPWebModule1.DataModuleRequest(Sender: TObject; ARequest: TRequest;
     AResponse: TResponse; var Handled: Boolean);
    var
      n: Integer;
      f: TUploadedFile;
      i: Integer;
    begin
      n := ARequest.Files.Count;
      if n = 0 then
        with AResponse.Contents do
        begin
          Add('<form id="form" action="' + ARequest.URI + '" method="POST" enctype="multipart/form-data">');
          Add('<label for="name">Drag n drop or click to add file:</label>');
          Add('<input type="file" name="input" />');
          Add('<input type="submit" value="Send" />');
          Add('</form>');
        end
      else
      begin
        f := ARequest.Files[0];
        AResponse.Contents.LoadFromStream(f.Stream);
      end;
      Handled := true;
    end;
    

перетащите и бросьте файл (желательно текстовый, так как он будет отображаться как текст) в поле входного файла (или нажмите соответствующую кнопку), затем нажмите кнопку _Send'_ (Отправить). Должно отобразиться содержимое файла. 

## Cookies

### Отправка

[![Концепция «cookie», изобретенная Netscape в 1994 году, позволяет HTTP-серверу идентифицировать всех своих клиентов.](https://wiki.freepascal.org/images/4/43/cookie_session_Object_Pascal.png)](</File:cookie_session_Object_Pascal.png> "Концепция «cookie», изобретенная Netscape в 1994 году, позволяет HTTP-серверу идентифицировать всех своих клиентов.")

  


Файлы cookie - небольшой фрагмент данных, отправленный веб-сервером и хранимый на компьютере пользователя. Веб-клиент (обычно веб-браузер) всякий раз при попытке открыть страницу соответствующего сайта пересылает этот фрагмент данных веб-серверу в составе HTTP-запроса ([Wiki](<https://ru.wikipedia.org/wiki/Cookie>)). **AResponse.Cookies** содержит список отправляемых файлов cookie. Это потомок **TCollection** , соответственно содержащийся **TCookie** является потомком **TCollectionItem**. Следовательно, вы можете использовать TCollection как способ управления элементами для управления ими. 

Вот пример: 
    
    
    procedure TFPWebModule1.DataModuleRequest(Sender: TObject; ARequest: TRequest;
     AResponse: TResponse; var Handled: Boolean);
    var
      C: TCookie;
    begin
      C := AResponse.Cookies.Add;
      C.Name := 'mycookie';
      C.Value := 'somevalue';
      Handled := true;
    end;
    

Вы не увидите никаких результатов в своем браузере. Но если вы используете какие-то инструменты разработчика (в Chrome есть встроенный) можно увидеть заголовок ответа: 

[![Заголовок ответа Set-Cookie в инструментах разработчика Chrome](https://wiki.freepascal.org/images/4/43/fpwebtutorial_cookie_set.png)](</File:fpwebtutorial_cookie_set.png> "Заголовок ответа Set-Cookie в инструментах разработчика Chrome")

Обратите внимание, что файл cookie имеет атрибуты, поэтому вы можете установить не только имя и значение. Просмотрите интерфейс TCookie, чтобы узнать, какие свойства поддерживаются. 

### Получение

Как только вы укажете выше заголовок **Set-Cookie** , последующий запрос на ваш сайт будет содержать дополнительный заголовок, содержащий значение, которое вы просили установить ранее: 

[![Заголовок запроса cookie в инструментах разработчика Chrome](https://wiki.freepascal.org/images/7/7d/fpwebtutorial_cookie_get.png)](</File:fpwebtutorial_cookie_get.png> "Заголовок запроса cookie в инструментах разработчика Chrome")

К счастью, способ чтения не отличается от данных GET и POST. Связанное свойство - **ARequest.CookieFields**. Чтобы прочитать ранее отправленный файл cookie: 
    
    
    procedure TFPWebModule1.DataModuleRequest(Sender: TObject; ARequest: TRequest;
     AResponse: TResponse; var Handled: Boolean);
    begin
      AResponse.Contents.Add('<p>Cookie get: ' + ARequest.CookieFields.Values['mycookie'] + '</p>');
      Handled := true;
    end;
    

## Сессии

TFPWebModule является потомком TSessionHTTPModule, поэтому имеет возможность управления сессией. Сессия основана на модулях, поэтому каждый модуль может выбирать, использовать или не использовать управление сессией. 

Сессия реализована абстрактно. По умолчанию реализация не предусмотрена. Один пример реализации с использованием файлов .ini приведен в модуле **iniwebsession'_. Вы должны иметь этот модуль в своем проекте или реализовать его, чтобы управление сессией работало. Если вы решите реализовать один из них, в основном вам необходимо расширить и реализовать абстрактные методы в классах_** _TCustomSession**и** TSessionFactory' _. 

### Активация

Чтобы активировать управление сессией, установите для свойства **CreateSession** в значение true. Сессия будет запущена до обработки запроса. В случае новой сессией будет вызвано **OnNewSession**. Здесь инициализируйте переменные сессии. 

### Управление переменными сессии

Переменные сессии представлены как **Session.Variables** (примечание: объект сессии является эквивалентом массива $ _SESSION, используемого в Php). Это строка для сопоставления строк, подобная структуре, поэтому вы можете читать / писать так: 
    
    
    Session.Variables['myvar'] := myvar; // записываем
    ...
    myvar := Session.Variables['myvar']; // читаем
    

Задание переменной пустой строки **НЕ** удаляет ее. Если вы действительно хотите удалить переменную, вместо этого вызовите **Session.RemoveVariable**. 

### Завершение

Вызывайте **Session.Terminate** всякий раз, когда вы хотите завершить сессию (например, выход пользователя из системы). Сессия также автоматически истечет, если следующий запрос поступит после **Session.TimeOutMinutes** с момента последнего запроса. Когда сессия завершается, будет вызван **OnSessionExpired**. Сделайте там все, что вам нужно. 

## Маршрутизация

Начиная с FPC 3.0.4, был реализован новый механизм маршрутизации. Вместо сохранения обратной совместимости решено, что по умолчанию будет использоваться новая маршрутизация. Таким образом, любой старый код (или новый код в зависимости от старой маршрутизации) необходимо перенести, добавив: 
    
    
    Application.LegacyRouting := true;
    

в *.lpr файле. 

### Старый способ

#### Использование нескольких модулей

В вашем приложении может быть несколько модулей. Щелкните меню "File", затем щелкните "New...". В появившемся диалоговом окне выберите "Web Module" в дереве выбора модулей. 

[![Add new web module](https://wiki.freepascal.org/images/8/80/fpwebtutorial_new_module.png)](</File:fpwebtutorial_new_module.png> "Add new web module")

затем нажмите OK. 

Поскольку в вашем приложении существует несколько модулей, вы больше не можете делать запрос (request) только с использованием `/`. Платформа не сможет волшебным образом выбрать, какой модуль должен обслуживать ответ (response), поэтому есть два способа указать, какой модуль вы хотите вызвать: 

  * /<module name>
  * /?module=<module name>



Во втором формате вы можете изменить "module" (который является значением по умолчанию) на любой допустимый ключ строки запроса, изменив **Application.ModuleVariable**. 

#### С использованием Actions

До сих пор мы использовали только веб-модули с обработчиком единого запроса. Это не сильно масштабируется, поскольку ваше веб-приложение становится все более и более сложным. Более того, некоторые функции могут иметь общие свойства и могут быть лучше логически сгруппированы, например: 

  * Account module 
    * Login action
    * Logout action
    * Register action
  * Product module 
    * Create action
    * Update action
    * Delete action
    * Details action



##### Поток обработки запросов (request)

Прежде, чем использовать действие (action), важно знать, как обрабатывается запрос fpWeb. В противном случае ваше действие (action) может оказаться бесполезным, потому что ваш модуль данных всегда обрабатывает запрос. Как такое могло случится? Возвращаясь немного назад, вспомните **Handled := true** , которое мы всегда делали раньше? Теперь в игру вступает параметр **Handled**. 

**Каждый** запрос сначала будет проходить через модуль **OnRequest** , независимо от запрошенного действия. Только если для этого **не** установлено значение Handled, выполняется OnRequest веб-действия. 

В общем, поток запросов: 

[![fpWeb request flow](https://wiki.freepascal.org/images/c/c1/fpwebtutorial_request_flow.png)](</File:fpwebtutorial_request_flow.png> "fpWeb request flow")

Обратите внимание на окошко "Our Concern" (наша озабоченность), это то, на что мы собираемся обратить наше внимание. 

##### Добавление действий в веб-модули

Чтобы добавить действие, выберите веб-модуль и перейдите в инспектор объектов. На вкладке свойств выберите "Actions" и нажмите кнопку во втором столбце. 

[![Manage actions button in object inspector](https://wiki.freepascal.org/images/6/66/fpwebtutorial_manage_actions.png)](</File:fpwebtutorial_manage_actions.png> "Manage actions button in object inspector")

Появится всплывающее окно, в котором вы можете добавлять, удалять и изменять порядок действий. 

[![Manage actions button in popup window](https://wiki.freepascal.org/images/4/47/fpwebtutorial_edit_actions.png)](</File:fpwebtutorial_edit_actions.png> "Manage actions button in popup window")

Нажмите "Add", в списке появится новое действие. Выберите его, затем перейдите в инспектор объектов. В настоящее время он будет показывать свойства и события этого вновь созданного действия. Переименуйте свойство Name (это будет имя, которое вы напишете в URL-адрес, поэтому дайте ему короткое, простое, но описательное имя) как вы желаете, я выберу "Hello". Двигаемся дальше на вкладку событий, сделайте то же самое, что и OnRequest для модуля, нажмите кнопку справа от строки OnRequest, чтобы создать обработчик запросов. 

[![Creating web action's OnRequest handler in the object inspector](https://wiki.freepascal.org/images/9/93/fpwebtutorial_action_onrequest.png)](</File:fpwebtutorial_action_onrequest.png> "Creating web action's OnRequest handler in the object inspector")

Вы будете представлены в том же интерфейсе OnRequest, но этот обрабатывает веб-действие вместо веб-модуля. Все, что вы можете сделать в OnRequest веб-модуля, можно сделать и здесь. Скопируйте тело метода из 'Hello, World!' раздела. 

Не забудьте удалить **Handled := true** из тела **OnRequest** предыдущего веб-модуля (или полностью удалить событие), чтобы действие позаботилось об обработке запроса. 

Запустите свой проект и запустите браузер. Теперь, поскольку обработка запросов делегирована веб-действию, вы больше не можете просто запрашивать /, но вам нужно / <action name> или <свойство ActionVar модуля>=<action name>. Обратите внимание, что <свойство ActionVar модуля> по умолчанию имеет значение пустой строки, в отличие от **Application.ModuleVariable** , для которого "модуль" является значением по умолчанию. Итак, по умолчанию вы можете использовать только /<action name> формы. 

Если у вас несколько модулей, то у вас есть множество вариантов: 

  * /<module name>/<action name>
  * /<module name>?action=<action name>
  * /<action name>?module=<module name>
  * /?module=<module name>&action=<action name>



Обратите внимание, что как только у модуля есть хотя бы одно действие, /<module или action name> по умолчанию сопоставляется с /<action name>. Чтобы изменить поведение таким образом, чтобы по умолчанию оно отображалось как /<module name>, установите для параметра **Application.PreferModuleName** значение **true**. В случае нескольких модулей, если имя модуля не указано, модуль по умолчанию будет обрабатывать данное действие. Чтобы изменить поведение таким образом, что имя модуля задавалось явно, установите для **Application.AllowDefaultModule** значение **false**. 

Следующие таблицы обобщают то, что произойдет на основе двух свойств: 

/<module or action name> | Application.PreferModuleName   
---|---  
true  | false   
Application.AllowDefaultModule  | true  | /<module name> | /<default module>/<action name>  
false  | /<module name> | ERROR   
  
##### Действие по умолчанию

Помните предыдущую диаграмму, "Delegate request handling to actions" (Делегировать обработку запросов действиям) на самом деле не так просто, но если мы расширим эту диаграмму, изображение будет слишком большим, чтобы поместиться. Итак, вот схема этой части: 

[![Request delegation to action flow](https://wiki.freepascal.org/images/5/5e/fpwebtutorial_request_delegation_flow.png)](</File:fpwebtutorial_request_delegation_flow.png> "Request delegation to action flow")

Две важные вещи из потока: DefActionWhenUnknown и действие по умолчанию. Первое является свойством веб-модуля, а второе соответствует свойству Default действия. В последнем случае, если имеется более двух действий, для которых свойство Default установлено в значение `true`, порядок действий (то, как он отображается во всплывающем окне управления действиями) будет учитываться для определения того, какое действие является действием по умолчанию. Эти два свойства определяют, что должно делать приложение, если для данного запроса не найдено подходящего действия. 

Следующие таблицы обобщают то, что произойдет на основе двух свойств: 

Запрос с недопустимым названием действия  | DefActionWhenUnknown   
---|---  
true  | false   
Action.Default  | true  | Запрос обрабатывается действием по умолчанию  | Ошибка: действие не найдено для действия: <action name>  
false  | Ошибка: неверное имя действия и нет действия по умолчанию  | Ошибка: действие не найдено для действия: <action name>  
  
  


Запрос без имени действия, т.е.: /  | DefActionWhenUnknown   
---|---  
true  | false   
Action.Default  | true  | Запрос обрабатывается действием по умолчанию  | Запрос обрабатывается действием по умолчанию   
false  | Ошибка: нет имени действия и действия по умолчанию.  | Ошибка: нет имени действия и действия по умолчанию.   
  
В случае приведенного выше ответа об ошибке должна следовать трассировка стека с информацией о номере строки, если вы создаете свое приложение с параметром `-gl`. Позже мы увидим, как создать собственный обработчик для этого (трассировка стека не годится для производства). Но пока убедитесь, что вы понимаете концепцию веб-модуля и веб-действия, особенно поток запросов. Играйте, пока не решите, что готовы к следующему разделу. 

### Новый механизм

Новый механизм чрезвычайно гибок и работает даже без модулей данных (старый механизм работает только с модулями данных). 

Выделенный модуль для этой маршрутизации предоставляется как **httproute** (добавьте его в пункт использования программы/модуля, где вы хотите зарегистрировать маршруты). Модуль содержит функцию **HTTPouter** , которая возвращает одноэлементный объект, отвечающий за управление маршрутом приложения, и имеет метод **RegisterRoute** для регистрации вашего маршрута. 

#### Синтаксис маршрута

Первый параметр **HTTPouter.RegisterRoute** — это маршрут, который будет сопоставляться с входящим запросом. Это может быть как простое ***** выше, что означает 0 или более путей или просто любые пути, так и сложное, как **/api/v1/:resource/*** , что означает, что заголовок **REQUEST_URI** должен начинаться с **/api/v1/** , за которым следует что-то еще, что будет привязано к переменной с именем resource и заканчиваться 0 или более путями. Он будет соответствовать: 

  * /api/v1/products
  * /api/v1/products/1
  * /api/v1/products/1/clone
  * /api/v1/products/something/else/that/is/really/long/and/silly



но не: 

  * /api/v1
  * /excuse/me/api/v1/products



В основном есть только 3 специальных символа: 

  * * обозначает 0 или более путей
  * :param обозначающий часть
  * / обозначающий разделитель частей



ваш маршрут будет состоять из этих 3 символов, а также всего остального, из чего состоит маршрут. 

#### Регистрация маршрута

2-й, 3-й или 4-й параметр (в зависимости от того, хотите ли вы обрабатывать конкретный метод HTTP и/или передавать ему дополнительные данные) **HTTPRouter.RegisterRoute** перегружен несколькими возможностями: 

  * Процедура обратного вызова


    
    
    TRouteCallback = Procedure(ARequest: TRequest; AResponse);
    

  * Событие обратного вызова


    
    
    TRouteEvent = Procedure(ARequest: TRequest; AResponse) of object;
    

  * Объект, удовлетворяющий интерфейсу (CORBA)


    
    
    IRouteInterface = Interface ['{10115353-10BA-4B00-FDA5-80B69AC4CAD0}']
      Procedure HandleRequest(ARequest: TRequest; AResponse: TResponse);
    end;
    

  * Объект, расширяющий абстрактный класс маршрутизатора


    
    
    TRouteObject = Class(TObject, IRouteInterface)
    Public
      Procedure HandleRequest(ARequest: TRequest; AResponse: TResponse); virtual; abstract;
    end;
    
    TRouteObjectClass = Class of TRouteObject;
    

По умолчанию, если второй параметр не является **TRouteMethod** , будут совпадать все методы HTTP. Используйте один из **rmUnknown, rmAll, rmGet, rmPost, rmPut, rmDelete, rmOptions, rmHead, rmTrace** , чтобы соответствовать только определенному методу HTTP. 

Порядок регистрации имеет значение. Если есть два или более маршрута, соответствующих текущему запросу, его обработает ранее зарегистрированный. 

В этом новом механизме автономнаяй "Hello, World!"-программа может быть такой простой, как: 
    
    
    uses
      fphttpapp, httpdefs, httproute;
    procedure DoHello(ARequest:TRequest; AResponse : TResponse);
    begin
      AResponse.Content:='<html><body><h1>Hello,World!</h1></body></html>'
    end;
    
    begin
      HTTPRouter.RegisterRoute('*', @DoHello);
      Application.Port := 9000;
      Application.Initialize;
      Application.Run;
    end.
    

#### Пример веб-сервера

Это пример простого кроссплатформенного многопоточного веб-сервера. 
    
    
    program webserver;
     
    {$mode objfpc}{$H+}
     
    uses
      {$ifdef UNIX}
        cthreads, cmem,
      {$endif} 
      fphttpapp, httpdefs, httproute;
     
    procedure route1(aReq: TRequest; aResp: TResponse);
    begin
      aResp.content:='<html><body><h1>Route 1 The Default</h1></body></html>'
    end;
     
    procedure route2(aReq: TRequest; aResp: TResponse);
    begin
      aResp.content:='<html><body><h1>Route 2</h1></body></html>'
    end;
     
    begin
      HTTPRouter.registerRoute('/', @route1, true);
      HTTPRouter.registerRoute('/route2', @route2);
      Application.port := 8080;
      Application.threaded := true;
      Application.initialize;
      Application.run;
    end.
    

## Использование шаблонов

fpWeb имеет встроенную поддержку FPTemplate, универсального движка шаблонов Free Pascal. Его не обязательно использовать из контекста веб-приложения, но с интегрированной поддержкой все будет немного проще. По крайней мере, управление памятью можно игнорировать, так как об этом позаботится модуль. 

Есть два уровня, на которых можно использовать шаблоны: действие и модуль. Полная поддержка RAD до конца не реализована, поэтому в некоторых моментах вам нужно перейти к ручному кодированию. 

Существует два режима работы: непараметризованный и параметризованный. Активный режим управляется свойством **AllowTagParams** , которое должно быть достаточно очевидным, какое значение относится к какому режиму. 

Строка шаблона может быть передана из файла через свойство **FileName** или прямая строка через свойство **Template**. `Template.Template` \- я знаю, это звучит странно :) Если оба заполнены, то FileName будет иметь приоритет. 

Два свойства: **StartDelimiter** и **EndDelimiter** определяют, как механизм должен распознавать тег шаблона. Например, если у вас есть: 

  * StartDelimiter = '{+'
  * EndDelimiter = '+}'



затем строка '{+title+}' определяет тег шаблона с именем 'title'. Обратите внимание, что пробелы важны, поэтому '{+ title +}' определяет тег шаблона с именем ' title ', а не просто 'title'. 

Специальные для параметризованного режима дополнительные три свойства: **ParamStartDelimiter** , **ParamEndDelimiter** и **ParamValueSeparator** определяют, как движок должен распознавать параметр тега шаблона. Например, если у вас есть: 

  * ParamStartDelimiter = '[-'
  * ParamEndDelimiter = '-]'
  * ParamValueSeparator = '='



тогда строка '{+data [-p=v-][-a=b-] +}' определяет тег шаблона с именем 'data' с параметром 'p' значения 'v' и параметром 'a' значения ' б'. Это можно использовать для передачи параметра уровня шаблона, такого как ожидаемый формат даты, заголовок-строка-нижний колонтитул для настраиваемого выходного представления, имя файла и т. д., как вы решите. 

Как следствие другого способа работы, основное событие, в котором работает шаблон, также отличается. Непараметризованный будет использовать **OnGetParam** , а параметризованный будет использовать **OnReplaceTag**. У них, конечно, другой интерфейс: 
    
    
    Type
      // OnGetParam: только для поддержки простого тега шаблона (напр: {Name})
      TGetParamEvent = Procedure(
        Sender: TObject;
        Const ParamName: String;
        Out AValue: String
      ) Of Object;
      // OnReplaceTag: для тегов с поддержкой параметров
      TReplaceTagEvent = Procedure(
        Sender: TObject;
        Const TagString: String;
        TagParams: TStringList;
        Out ReplaceText: String
      ) Of Object;
    

В **OnGetParam** вы проверяете **ParamName** , а затем соответственно назначаете **AValue** , т. е. если вы хотите, чтобы тег 'title' был заменен на 'My App', тогда заполните метод следующим образом: 
    
    
    // используйте Trim(), если вы хотите, чтобы пробелы вокруг тега были незначительными
    case Trim(ParamName) of
      'title': AValue := 'My App';
    else
      AValue := 'UNKNOWN';
    end;
    

В **OnReplaceTag** вы проверяете **TagString** и, возможно, **TagParams** , а затем соответственно назначаете **ReplaceText** , т.е., если вы хотите, чтобы тег 'datetime' был заменен текущим временем с параметром 'datetimeformat', чтобы указать, как следует форматировать дату и время, заполните метод следующим образом: 
    
    
    // используйте Trim(), если вы хотите, чтобы пробелы вокруг тега были незначительными
    case Trim(TagString) of
      'datetime': AValue := FormatDateTime(TagParams.Values['datetimeformat'],Now);
    else
      AValue := 'UNKNOWN';
    end;
    

### На уровне действий

Создайте/выберите действие, затем перейдите в инспектор объектов. Вы увидите свойство подкомпонента с именем **Template**(Шаблон). Этот шаблон является обычным экземпляром **TFPTemplate**. Разверните его и заполните свойства, как описано выше. Теперь перейдите на вкладку Events(События), снова разверните **Template** , вы увидите два события. Заполните тот, который основан на вашем значении свойства **AllowTagParams**. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Если ваш Lazarus не может автоматически заполнять событие, попробуйте ввести имя вручную в поле редактирования, а затем нажмите кнопку "...". Это ошибка в текущем Lazarus, которая может быть исправлена ​​в будущем.

На этом уровне шаблон с содержимым не устанавливается автоматически в качестве обработчика запроса. Это может измениться в будущем, но давайте разберемся с его текущим состоянием. Заполните событие **OnRequest** действия, затем заполните его: 
    
    
    with Actions.CurrentAction as TFPWebAction do
    begin
      AResponse.Content := Template.GetContent;
    end;
    Handled := true;
    

Приведение необходимо, поскольку **CurrentAction** имеет тип **TCustomWebAction** вместо **TFPWebAction**. Без него мы не сможем получить доступ к свойству **Template**. 

### На уровне модуля

На уровне модуля вам в настоящее время придется делать это вручную, поскольку поддержка RAD не реализована. Связанным свойством является ModuleTemplate. Однако это не обычный экземпляр **TFPTemplate** , а специальный класс **TFPWebTemplate** , являющийся его потомком. 

Идея здесь состоит в том, чтобы модуль предоставлял макет, а действия предоставляли контент с возможностью предоставления дополнительных переменных. Поэтому рекомендуется оставить **AllowTagParams** как есть и назначить событие **OnGetParam** только **модуля**. НЕ назначайте **OnGetParam** для **ModuleTemplate** , так как он **никогда** не будет вызываться. 

Тег шаблона с именем 'content' будет автоматически заменен содержимым, созданным действием, все остальное заменяется либо из внутренних переменных шаблона, либо из **OnGetParam**. 

### Использование отдельного шаблона

Несмотря на несколько неполную интеграцию, ничто не мешает вам использовать fpTemplate (или любые другие решения для шаблонов) вручную, вне встроенной поддержки. В некоторых случаях это может быть даже лучше, поскольку оно будет модульным. 

## Использольвание html Producer object

Предполагая, что существует TWebAction с именем act_get_my_html, вы должны попросить написать в AResponse возврат внутреннего потока байтов: при ответе с помощью решения html Producer objects _мы должны использовать методы писателя_. Он управляет потоком памяти с целью ускорения. Здесь нет строкового типа, как в приведенном выше решении fpTemplate, которое использует обработку текста строки, как обычно в Php. Producer записывает в AResponse рекурсивную итерацию " foreach ", которая проходит по дереву полиморфных объектов HTML Dom, состоящему из иерархии классов `THtmlCustomElement = Class(TDOMElement)`(см. модуль `htmlelements.pp`). Итак, AResponse пишется параметром aWriter, со следующим вызовом: 
    
    
    procedure TFPWebModule1.act_get_my_htmlRequest(Sender: TObject; ARequest: TRequest; AResponse: TResponse; var Handled: Boolean);
    begin
      (* спрашиваем у HTMLEntityProducer1 содержимое своих html-элементов DOM: *)
      HTMLEntityProducer1.HandleRequest(ARequest, AResponse, Handled);
      Handled := True;
    end;
    

Повторим еще раз, что работа Producer заключается в преобразовании потока внутренней памяти в текст в объекте ответа: именно так был разработан этот шаблон, хотя существует метод рендеринга text-html с именем ProduceContent только для целей отладки. Вы можете переопределить этот метод, если вы пишете или отлаживаете компонент Producer. 

Вот пример с html-объектом Producer, позволяющий создать веб-приложение «способом RAD», то есть с помощью перетаскивания компонента HTMLEntityProducer из палитры: 
    
    
    procedure TFPWebModule1.HTMLEntityProducer1WriteEntity(Sender: THTMLContentProducer; aWriter: THTMLWriter);
    begin
    
      aWriter.startHeader;
        aWriter.Meta('','Content-Type','text/html; charset=UTF-8');
        aWriter.title('My web page');
        aWriter.link('stylesheet','stylesheet.css','text/css','screen');
      aWriter.EndHeader;
    
      aWriter.Startbody;
        aWriter.Startparagraph;
          aWriter.heading2('Hello, world from inside §1:');
          aWriter.Text('Here is text written inside the current paragraph.');
        aWriter.Endparagraph;
        aWriter.paragraph('This is another text written inside a self "started and ended" paragraph.');
        aWriter.Startparagraph;
          aWriter.heading2('Hello, world from inside §2:');
          aWriter.Text('Here is the final text.');
          aWriter.Image.src := 'logo.png';
          AnotherProducer.WriteContent(aWriter);
        aWriter.Endparagraph;
      aWriter.Endbody;
    
    end;
    

Свойство TWebAction.ContentProducer позволяет связать THTTPContentProducer с его веб-действием, предоставляемым через URI в сети. 

[ToDo: нет официальной документации о компонентах «способ RAD» fpWeb (html producer, html provider, html adapter, html formatter и т.д.)] 

## Советы и хитрости

### Возврат другого кода ответа HTTP

По умолчанию fpWeb возвращает HTTP 200 OK, чтобы указать на успешную обработку запроса. Это, конечно, не всегда так, поскольку пользовательский ввод может быть не таким, как мы ожидали. Для этого установите для параметра **AResponse.Code** в обработчике запроса код, который вы хотите вернуть. 

### Перенаправление запроса на другой URL

Обычная практика после успешного входа в систему — перенаправление пользователя на страницу его учетной записи. Это можно сделать, вызвав **AResponse.SendRedirect** в обработчике запросов, указав URL-адрес для перенаправления запроса. 

### Обслуживание статических файлов (встроенный веб-сервер)

Помните диалоговое окно в разделе [Hello, World!](<fpWeb_Tutorial.md> "fpWeb Tutorial/ru") после выбора приложения HTTP-сервера? Если вы отметите "Register location to serve files from"(Зарегистрировать местоположение для обслуживания файлов), вы можете заполнить "Location"(Расположение) (сегмент URI, не должен содержать слэшей) и "Directory"(Каталог) (физический каталог на вашем компьютере, должен существовать во время выполнения), то мастер просто добавит: 
    
    
    RegisterFileLocation('<Location>','<Directory>');
    

в начало вашего *.lpr-файла и добавит модуль **fpwebfile** в раздел uses. Вы можете сделать это вручную в любое время, а также зарегистрироваться несколько раз для разных местоположений/каталогов. После этого вы можете запросить **/ <Location>/<любое имя файла в Directory>**, и оно будет предоставлено автоматически. Обратите внимание, что _mimetype_ файла определяется **fpmimetypes**. Вызовите **MimeTypes.LoadFromFile** с вашим файлом _mime.types_ , чтобы указать правильный тип _mimetype_ на основе его расширения. В противном случае файл всегда будет обслуживаться как **application/octet-stream** , что означает, что браузер загрузит его, а не интерпретирует (особенно важно для файлов JavaScript и CSS). 

Вы можете получить полные mime.types [здесь](<http://svn.apache.org/viewvc/httpd/httpd/trunk/docs/conf/mime.types?&view=co>). 

В Lazarus 2.0.6 или новее вы должны добавить в начало своей программы полный путь к файлу mime.types. 
    
    
    begin
      MimeTypesFile := Application.Location + 'mime.txt';
    

Примите во внимание, что путь по умолчанию, идущий с проектом, это - _lib\$(TargetCPU)-$(TargetOS)_

Например: _httpproject\lib\i386-win32\mime.txt_

### Централизованное управление конфигурацией и модулями

По умолчанию программный файл (.lpr) содержит модуль протокола. Это ограничивает возможность использования объекта **Application** из других контекстов, таких как веб-модули. К счастью, нетрудно провести рефакторинг, чтобы получить то, что мы хотим. Мы удаляем вызовы **RegisterHTTPModule** из веб-модулей и не будем использовать .lpr для очистки основного блока с единственным идентификатором модуля в разделе uses, назовем его: брокер. Содержимое модуля: 
    
    
    unit Brokers;
    
    {$mode objfpc}{$H+}
    
    interface
    
    { $define cgi}
    { $define fcgi}
    {$define httpapp}
    
    uses
      CustWeb;
    
    function GetApp: TCustomWebApplication; inline;
    
    implementation
    
    uses
      {$ifdef cgi}fpcgi{$endif}
      {$ifdef fcgi}fpfcgi{$endif}
      {$ifdef httpapp}fphttpapp{$endif}
      ,webmodule1
      ,webmodule2
      ;
    
    function GetApp: TCustomWebApplication;
    begin
      Result := Application;
    end;
    
    initialization
      RegisterHTTPModule('wm1', TWebModule1);
      RegisterHTTPModule('wm2', TWebModule2);
      {$ifndef cgi}
      Application.Port := 2015;
      {$endif}
      Application.Initialize;
      Application.Run;
    end.
    

Таким образом, мы можем управлять регистрацией веб-модуля, а также предоставлять API для получения объекта **Application** (представленного как **TCustomWebApplication**), в то же время легко переключаясь между реализациями протокола в одном месте. 

### Элегантное завершение (FastCGI / встроенный веб-сервер)

Вместо нажатия `Ctrl`+`C` в вашем приложении есть способ корректно завершить работу вашего приложения, выполнив необходимую очистку, вызвав **Application.Terminate**. Возможно, вам придется использовать предыдущий трюк, чтобы легко получить доступ к объекту **Application**. Распространенной реализацией является предоставление определенного защищенного паролем модуля/действия, которое вызывает метод Terminate. Впрочем, вы можете выбрать любой способ. 

### Пользовательский обработчик исключений

[отредактировано 1 мая 2020г => переход от указателя метода к простой процедуре..] 

Чтобы переопределить обработчик исключений по умолчанию, который выдает трассировку стека всякий раз, когда возникает исключение (например, по HTTP 404 или 500), и, следовательно, не подходит для производства, вы должны задать **Application.OnShowRequestException**. 

~~_Это метод, поэтому вам нужно будет предоставить свою процедуру, реализующую метод, и присвоить ее с помощью объекта, т.е.: если у вас есть**MyExceptionHandler** как объект **TMyExceptionHandler** , который имеет метод **MyShowRequestException** , вы можете назначить его посредством: _~~

~~_
    
    
    Application.OnShowRequestException := @MyExceptionHandler.MyShowRequestException;
    

_~~~~__~~

~~_не забудьте**.Create()** **MyExceptionHandler** ПЕРЕД назначением выше, иначе вы получите **EAccessViolation**!_~~

Вы должны предоставить свою глобальную процедуру, которая реализует ваш собственный обработчик исключений (в рабочей среде рекомендуется заменить стек вызовов кодом состояния HTTP и его объяснением). Затем вы можете переопределить обработчик исключений по умолчанию, назначив его следующим образом: 
    
    
    Application.OnShowRequestException := @MyShowRequestException;
    

### Чисто ручное кодирование (дизайнер форм не требуется)

Нет необходимости использовать конструктор форм Lazarus для написания fpWeb-приложения. Вы можете использовать технику чисто ручного кодирования, чтобы написать его. Секрет кроется в третьем параметре **RegisterHTTPModule** : **SkipStreaming**. Если для этого параметра установлено значение true, fpWeb не будет искать ресурс .lfm. Поэтому все должно обрабатываться вручную: настройки свойств, обработчики событий, регистрация действий и т.д. 

Обратите внимание, что логично делать то, что обычно делается через инспектор объектов в переопределенном конструкторе. Внутри него вызовите унаследованный конструктор, передав в качестве параметров AOwner и CreateMode. После этого вы можете установить свойства, назначить обработчики событий и т.д. Пример: 
    
    
    type
      THelloWorldModule = class(TFPWebModule)
        constructor CreateNew(AOwner: TComponent; CreateMode: Integer); override;
        procedure Request(Sender: TObject; ARequest: TRequest;
          AResponse: TResponse; var Handled: Boolean);
      end;
    
    constructor THelloWorldModule.CreateNew(AOwner: TComponent; CreateMode: Integer);
    begin
      inherited CreateNew(AOwner,CreateMode);
      OnRequest := @Request;
    end;
    
    procedure THelloWorldModule.Request(Sender: TObject; ARequest: TRequest;
     AResponse: TResponse; var Handled: Boolean);
    begin
      AResponse.Content := 'Hello, World!';
      Handled := true;
    end;
    

## См.также

  * [Lazarus для Интернета (учебник)](<https://www.freepascal.org/~michael/articles/#web1>)
  * [Lazarus для Интернета: сеансы и шаблоны (учебник))](<https://www.freepascal.org/~michael/articles/#web2>) В этой статье описывается класс TSQLDBSession, который управляет подключением SQL, которое наследуется от веб-сеанса (для управления историями переменных веб-сеанса в рамках сеанса подключения к базе данных, имеющей возможности блокировки строк).
  * примеры - чтение текстовых файлов - в вашем каталоге ...\fpc\x.x.x\source\packages\fcl-web\examples.  




[![Копия скриншота одного из примеров](https://wiki.freepascal.org/images/0/0b/cgi-listrecords.png)](</File:cgi-listrecords.png> "Копия скриншота одного из примеров")

  * XML или JSON на ExtJS: если вы хотите использовать более или менее сложный движок, который очень «продвинут» в отношении аспекта рендеринга объектов Javascript на стороне браузера, в дополнение к их функциональным возможностям (например, ExtJS для отображения сетка с поддержкой базы данных, динамическая графика и т. д.), для такого решения очень часто требуется файл XML или JSON. Существуют классы Adapter и Formatter (например, классы TExtJSXMLWebdataInputAdaptor, TExtJSJSONDataFormatter).



В целом, классы Adapter адаптируют узлы входящего jSon или XML к отображению своих полей базы данных TField. А классы Formatter сопоставляют каждую запись TField с ее узлом в формате jSon или XML перед отправкой в ​​браузер.  


  * [Форматы веб-данных в Lazarus/FPC](<https://www.freepascal.org/~michael/articles/#webdata>)
  * [Практическое построение GUI в ExtJS](<https://www.freepascal.org/~michael/articles/#extjs1>)
  * [Смешивание веб-приложений ExtJS и Pascal: ExtPascal](<https://www.freepascal.org/~michael/articles/#extpascal>)

---

_Source: [https://wiki.freepascal.org/fpWeb_Tutorial/ru](https://web.archive.org/web/20250122102334/https://wiki.freepascal.org/fpWeb_Tutorial/ru)_
