# fcl-web

│ **[English (en)](<../en/fcl-web.md> "fcl-web")** │  **[español (es)](</fcl-web/es> "fcl-web/es")** │  **[français (fr)](</fcl-web/fr> "fcl-web/fr")** │  **русский (ru)** │    
****

## Contents

  * 1 Что такое fpWeb
  * 2 Использование fpWeb вместе с Lazarus
    * 2.1 Установка пакета weblaz fpWeb Lazarus
    * 2.2 Создание CGI-приложения
    * 2.3 Развертывание приложения CGI
    * 2.4 Форматирование HTML и читаемых полей запроса
    * 2.5 Чтение данных GET
    * 2.6 Чтение данных POST
    * 2.7 Чтение двоичных данных POST
  * 3 Специализированные модули
  * 4 Использование нескольких модулей
  * 5 Использование Actions
  * 6 Использование HTML-шаблонов
  * 7 Советы и устранение неполадок
    * 7.1 Отправка кодов результатов
    * 7.2 Отправка двоичных данных
    * 7.3 Error: Could not determine HTTP module for request
    * 7.4 Error: response code 500 Internal Server error when trying to handle DELETE method
    * 7.5 fpWeb/FCGI и Apache 2.4 (режим mod_proxy_fcgi)
    * 7.6 fpWeb/FCGI и nginx
  * 8 Примечание
  * 9 См. также



## Что такое fpWeb

fpWeb можно использовать для создания приложений веб-сервера. Приложение веб-сервера может быть одним из следующих 

  * Приложение CGI
  * Приложение FastCGI
  * Модуль Apache
  * Автономный HTTP-сервер, использующий компоненты HTTP-сервера Free Pascal.
  * Автономный сервер HTTP с использованием GNU libmicrohttp
  * Приложение Windows SysHTTP.



Во всех случаях модель разработки одинакова: вы регистрируете обработчики для набора URL-адресов (называемых маршрутами), которые будет поддерживать ваше приложение. 

Затем вы встраиваете эти маршруты в одну из указанных выше сред: Это означает, что вы выбираете объект приложения, который будете использовать. 

В lazarus IDE у вас есть выбор приложений, когда вы запускаете новый проект. После запуска приложения можно изменить тип приложения, обычно это означает просто изменение имени модуля в разделе uses проекта. 

В одном двоичном файле можно поддерживать несколько сред, хотя это требует дополнительной работы. 

fpWeb также предлагает некоторую готовую поддержку для REST-сервера базы данных или для механизмов JSON RPC 2.0. Они построены поверх вышеупомянутой конструкции. Платформа Brook также построена на основе базовой архитектуры, описанной выше. 

Подробнее об этом можно прочитать в [fpWeb Tutorial](<../en/fpWeb_Tutorial.md> "fpWeb Tutorial")

## Использование fpWeb вместе с Lazarus

### Установка пакета weblaz fpWeb Lazarus

Первым делом необходимо установить пакет, который находится по пути lazarus/components/fpweb/weblaz.lpk. Как обычно с пакетами времени разработки, вам придется пересобрать Lazarus. 

### Создание CGI-приложения

После установки пакета weblaz можно создать очень простое веб-приложение CGI, отображающее HTML-страницу, перейдя в меню Lazarus «Файл - Новый ...». Из списка возможных приложений выберите «Приложение CGI», как показано на изображении ниже, которое создаст основной файл проекта CGI и веб-модуль fpweb. 

[![New cgi.PNG](https://wiki.freepascal.org/images/e/ec/New_cgi.PNG)](</File:New_cgi.PNG>)

TFPWebModule позволяет вам управлять свойствами и событиями с помощью инспектора объектов. 

Чтобы добавить код для отображения страницы, необходимо добавить обработчик запроса. Для этого дважды щелкните свойство OnRequest в инспекторе объектов, как показано на изображении ниже: 

[![Webmodule.PNG](https://wiki.freepascal.org/images/3/3d/Webmodule.PNG)](</File:Webmodule.PNG>)

В обработчике событий нужно написать HTML-код, который будет отображаться браузером. Чтобы избежать смешивания Pascal и HTML, эту страницу можно загружать из каталога, в котором находится исполняемый файл CGI, с помощью `AResponse.Contents.LoadFromFile()`. 

Тип ответа должен быть установлен в `AResponse.ContentType`. Для HTML-страниц это должно иметь значение 'text/html;charset=utf-8'. 

Наконец, для параметра Handled должно быть установлено значение True, чтобы указать, что запрос был успешно обработан (и вернуть в браузер код состояния 200 OK). После добавления этого кода веб-модуль должен выглядеть так: 
    
    
    unit mainpage;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, HTTPDefs, websession, fpHTTP, fpWeb; 
    
    type
    
      { TFPWebModule1 }
    
      TFPWebModule1 = class(TFPWebModule)
        procedure DataModuleRequest(Sender: TObject; ARequest: TRequest;
          AResponse: TResponse; var Handled: Boolean);
      private
        { private declarations }
      public
        { public declarations }
      end; 
    
    var
      FPWebModule1: TFPWebModule1; 
    
    implementation
    
    {$R *.lfm}
    
    { TFPWebModule1 }
    
    procedure TFPWebModule1.DataModuleRequest(Sender: TObject; ARequest: TRequest;
      AResponse: TResponse; var Handled: Boolean);
    begin
      AResponse.ContentType := 'text/html;charset=utf-8';
      AResponse.Contents.LoadFromFile(ExtractFilePath(ParamStr(0)) + 'mainpage.html');
      Handled := True;
    end;
    
    begin
      RegisterHTTPModule('TFPWebModule1', TFPWebModule1);
    end.
    

### Развертывание приложения CGI

В этом разделе предполагается, что используется веб-сервер Apache. Конечно, можно использовать и другие веб-серверы, поддерживающие CGI (nginx, cherokee). 

Apache можно загрузить здесь: <http://httpd.apache.org/download.cgi> или установить с помощью диспетчера пакетов вашего дистрибутива. 

При установке Apache по умолчанию все файлы, расположенные в его каталоге cgi-bin, будут обрабатываться как программы CGI, поэтому пользователь не сможет получить доступ к размещенным там простым файлам HTML. Этот каталог можно задать в файле httpd.conf в следующем разделе: 
    
    
       #
       # ScriptAlias: определяет, какие каталоги содержат сценарии сервера.
       # ScriptAliases по сути то же, что и Aliases(псевдонимы), за исключением того, что
       # документы в целевом каталоге рассматриваются как приложения и
       # запускается сервером по запросу, а не как документы, отправленные на
       # клиент. К директивам ScriptAlias применяются те же правила для завершающего символа "/",
       # что и к Alias.
       #   
       ScriptAlias /cgi-bin/ "C:/Program Files/Apache Software Foundation/Apache2.2/cgi-bin/"
    

Если вы поместите исполняемый файл с именем «mywebpage.cgi» в этот каталог, тогда к странице можно будет получить доступ как <http://localhost/cgi-bin/mywebpage.cgi> (или удаленно с соответствующим IP-адресом или доменным именем). 

Fcl-web с Lazarus в Windows создает файлы .exe. Чтобы Apache обслуживал эти файлы, вам необходимо добавить это: 
    
    
        AddHandler cgi-script .exe
    

И чтобы обслуживать ваши исполняемые файлы из другого каталога, вы добавляете это: 
    
    
       ScriptAlias /bin/ "C:/lazarus_projects/test-fclweb/bin/"
       <Directory "C:/lazarus_projects/test-fclweb/bin/">
           AllowOverride None
           Options None
           Order allow,deny
           Allow from all
       </Directory>
    

### Форматирование HTML и читаемых полей запроса

В предыдущем примере была показана простая HTML-страница. Можно, например, пожелать 

  * динамически изменять вывод HTML-страницы и
  * при чтении переменных, которые браузер передал веб-странице в полях запроса (например, в действиях HTTP GET и HTTP POST).



Простое решение для первой проблемы - просто использовать стандартную подпрограмму Паскаля Format с добавлением %s или %d в файл HTML в соответствующие места, которые получат настраиваемое значение. 

### Чтение данных GET

Чтобы прочитать переменные GET, можно использовать `ARequest.QueryFields`, который является потомком TStrings. Каждая переменная будет находиться в отдельной строке в TStrings в формате имя_переменной = значение, аналогично тому, как они отображаются в адресе страницы браузера. Для поиска определенной переменной можно использовать `ARequest.QueryFields.Values​​[]`, передав имя переменной в скобках, чтобы получить обратно ее значение, вместо ручного анализа строки. 

Итоговый код: 
    
    
    procedure TFPWebModule1.DataModuleRequest(Sender: TObject; ARequest: TRequest;
      AResponse: TResponse; var Handled: Boolean);
    var
      HexText, AsciiText: string;
    begin
      HexText := ARequest.QueryFields.Values['hex'];
      AsciiText := HexToAnsii(HexText);
    
      AResponse.ContentType := 'text/html;charset=utf-8';
      AResponse.Contents.LoadFromFile(ExtractFilePath(ParamStr(0)) + 'mainpage.html');
      AResponse.Contents.Text := Format(AResponse.Contents.Text,
        [HexText, AsciiText]);
      Handled := True;
    end;
    

### Чтение данных POST

Данные, отправленные запросами POST, можно получить из `TRequest.Content`. Он будет без заголовков запроса - другими словами, он будет содержать тело запроса. 

К отправленным данным формы также можно получить доступ с помощью `TRequest.ContentFields`, который представляет собой содержимое, проанализированное как поля, разделенные знаком `&` и декодированные. Например, следующий набор значений формы: 
    
    
    login: dfg 345&&
    login_senha: ====
    email: dfg
    

Будут преобразованы в 'APPLICATION/X-WWW-FORM-URLENCODED' следующим образом (см. [wikipedia POST (HTTP)](<http://en.wikipedia.org/wiki/POST_%28HTTP%29>)): 
    
    
    login=dfg+345%26%26&login_senha=%3D%3D%3D%3D&email=dfg
    

И будет доступно в `TRequest.ContentFields` через индекс строки или через свойство Values, что удобнее: 
    
    
    TRequest.ContentFields[0]: login=dfg 345&&
    TRequest.ContentFields[1]: login_senha=====
    TRequest.ContentFields[2]: email=dfg
    TRequest.ContentFields.Values['email']: dfg
    

Если используются другие типы mime, кроме 'MULTIPART/FORM-DATA' и 'APPLICATION/X-WWW-FORM-URLENCODED' (типы, поддерживаемые формами HTML): 

  * контент доступен только в `TRequest.Content`, но не в `TRequest.ContentFields`
  * использование этих mime-типов вызывает исключение для FPC 2.4.2 (исправлено в FPC 2.5+).



Обратите внимание, что HTTP POST также может отправлять поля запроса (в URL-адресе), например <http://server/bla?question=how>, и к ним обращается `TRequest.QueryFields`, как описано в предыдущем разделе. 
    
    
    procedure TFPWebModule1.DataModuleRequest(Sender: TObject; ARequest: TRequest;
      AResponse: TResponse; var Handled: Boolean);
    var
      lData: String;
    begin
      lData := ARequest.Content;
    

### Чтение двоичных данных POST

Загруженные файлы сохраняются во временном расположении с временным именем файла, чтобы сохранить файл, вы должны переместить его в постоянное расположение. 

Для получения данных на сервере, который был размещен, например, как multipart/form-data используйте что-то вроде этого: 
    
    
    procedure TMainWebModule.TFPWebActions2Request(Sender: TObject;
      ARequest: TRequest; AResponse: TResponse; var Handled: Boolean);
    var
      i: Integer;
    begin  
      // Обработать все полученные файлы
      for i := 0 to ARequest.Files.Count - 1 do
      begin
        // Настоятельно рекомендуется делать что-то еще, кроме Writeln ;)
        writeln ('Полученное имя файла: '+ARequest.Files[i].LocalFileName);
      end;
          
      Handled := true;
    end;
    

Клиент может отправлять данные, например, FileFormPost. 

## Специализированные модули

Класс *TFPWebModule* (используемый ниже) - это простой пример WEb-модуля, который можно использовать для всех видов HTTP-запросов. 

Однако fpweb поставляется с некоторыми специализированными модулями, которые имеют дополнительную поддержку для специализированных задач: 

  * Класс **TSimpleFileModule** в модуле _fpwebfile.pp_ можно использовать для отправки файлов. Вы указываете ему каталог, и он делает все остальное.
  * Класс **TFPHTMLModule** в модуле _fphtml.pp_ можно использовать для создания HTML.
  * Класс **TProxyWebModule** в модуле _fpwebproxy.pp_ \- это готовый прокси для пересылки.
  * Класс **TFPWebProviderDataModule** в модуле _fpwebdata.pp_ обслуживает данные в формате JSON, которые могут использоваться хранилищами ExtJS.
  * Класс **TSQLDBRestModule** в модуле _sqldbrestmodule.pp_ реализует полноценный сервер REST, поддерживаемый SQLDB. См. Дополнительную информацию в [SQLDBRestBridge](<../en/SQLDBRestBridge.md> "SQLDBRestBridge").
  * Класс **TJSONRPCModule** в модуле _webjsonrpc.pp_ реализует службу JSON-RPC.
  * Класс **TExtDirectModule** в модуле _fpextdirect.pp_ реализует вариант Ext.Direct службы JSON-RPC.



## Использование нескольких модулей

Если в веб-приложении только один модуль, все запросы будут направлены в этот модуль. 

По мере роста вашего веб-приложения можно использовать несколько модулей. Новый модуль можно добавить, выбрав 'File - New', а затем один из 'Web module' или 'HTML Web Module'. 

FCL-web использует URL-адрес, чтобы определить, как следует обрабатывать HTTP-запрос. Поэтому он должен знать, какие веб-модули существуют в приложении. Для этого каждый модуль должен быть зарегистрирован. 

Каждый модуль регистрируется в fcl-web в разделе инициализации модуля, в котором он определен: 
    
    
    RegisterHTTPModule('location', TMyModule);
    

Затем модуль будет вызван, если используется URL-адрес формы 
    
    
    http://www.mysite.org/mycgi.cgi/location
    

или 
    
    
    http://www.mysite.org/mycgi.cgi?module=location
    

Если присутствует несколько модулей, имя модуля должно появиться в URL-адресе, в противном случае возникнет ошибка. 

Это поведение также можно принудительно настроить для приложений, которые имеют только один модуль, установив для свойства приложения AllowDefaultModule значение false: 
    
    
    Application.AllowDefaultModule := False;
    

В этом случае приложение fcl-web всегда будет требовать имя модуля в URL-адресе. 

Имя переменной запроса, определяющей имя модуля (по умолчанию это 'module'), можно задать в свойстве `Application.ModuleVariable`. Следующий код 
    
    
    Application.ModuleVariable := 'm';
    

гарантирует, что следующий URL-адрес будет направлен в TMyModule: 
    
    
    http://www.mysite.org/mycgi.cgi?m=location
    

Если всего этого недостаточно для определения модуля, которому следует передать запрос, можно использовать событие `Application.OnGetModule`. Это тип `TGetModuleEvent`: 
    
    
    type
      TGetModuleEvent = Procedure (Sender : TObject; ARequest : TRequest;
                                   Var ModuleClass : TCustomHTTPModuleClass) of object;
    

Создание обработчика событий для этого события позволяет точно контролировать модуль, созданный для обработки запроса: запрос (переданный в ARequest) может быть исследован, а для переменной 'ModuleClass' должен быть задан класс модуля, который должен обрабатывать Запрос. 

Если 'ModuleClass' при возврате равен 'Nil', в браузер будет отправлено сообщение об ошибке. 

## Использование Actions

Модуль можно использовать для группировки определенных видов действий, которые логически связаны друг с другом. Представьте себе модуль TUserModule, который используется для управления пользователями на веб-страницах. С пользователем может быть связано несколько действий: 

  * Создание
  * Удаление
  * Редактирование
  * Отображение



Эти разные действия могут обрабатываться одним и тем же модулем. Можно определить действие по URL-адресу вручную, например: 
    
    
    http://mysite/mycgi.cgi/user?action=delete&id=12
    

Это можно автоматизировать. 

Для упрощения разделения различных действий в модуле есть свойство actions: это коллекция, в которой каждый элемент связан с различным ответом на запрос. Действия имеют различные свойства: 

Name
    Название действия. URL-адрес будет проверен, чтобы определить название действия.
Content
    строка. Если задано, оно отправляется в браузер.
Contents
    список строк. Если задано, оно отправляется в браузер.
ContentProducer
    если задано, ContentProducer будет обрабатывать запрос.
Default
    если установлено значение «True», это действие является действием по умолчанию. Это означает, что если FCL-Web не может определить имя действия, будет выполнено это действие.
Template
    если установлено, этот шаблон будет обработан, а результаты отправлены в браузер.

Также есть некоторые события: 

BeforeRequest
    выполняется до обработки запроса. Может использоваться для установки 'Content' или других свойств.
AfterRequest
    выполняется после обработки запроса.
OnRequest
    обработчик событий для обработки запроса. Если задано, обработчик используется для обработки запроса.

Опять же, как и в случае нескольких модулей, URL-адрес используется для определения того, какое действие выполнить. Используется часть сразу после части модуля (в данном примере "user"): 
    
    
    http://mysite/mycgi.cgi/user/delete&id=12
    

выполнит действие под названием 'delete'. 

Свойство 'ActionVar' модуля можно использовать для установки имени используемой переменной запроса. Настройка 
    
    
    UserModule.ActionVar := 'a';
    

может использоваться для изменения указанного выше URL на 
    
    
    http://mysite/mycgi.cgi/user?a=delete&id=12
    

Если в приложении только один модуль, URL-адрес можно сократить до 
    
    
    http://mysite/mycgi.cgi/delete&id=12
    

## Использование HTML-шаблонов

Информацию об использовании шаблонов, тегов шаблонов и параметров тегов шаблонов для создания страниц ответов см. в файле fptemplate.txt в каталоге FPC в /packages/fcl-base/texts/. 

Примеры проектов, демонстрирующих использование шаблонов, можно найти в каталоге FPC в /packages/fcl-web/examples/fptemplate/ (дополнительные сведения см. в README.txt). (В более ранних версиях эти примеры находились в каталоге Lazarus в /components/fpweb/demo/fptemplate/) 

## Советы и устранение неполадок

### Отправка кодов результатов

Чтобы отправить HTTP-ответ, отличный от 200 OK, используйте **AResponse.Code** и **AResponse.CodeText** , например 
    
    
    AResponse.Code     := 404;
    AResponse.CodeText := 'Document not found';
    

### Отправка двоичных данных

Подход, который, кажется, работает для отправки, например, файл tiff с веб-сервера на клиент - адаптированный из $(fpcdirectory)\packages\fcl-web\examples\jsonrpc\demo1\wmdemo.pp - что-то вроде: 
    
    
    AResponse.ContentStream := TMemoryStream.Create;
    
    try
      AResponse.ContentStream.LoadFromFile('/tmp/sample.tiff');
      AResponse.ContentType := 'image/tiff'; //или любой другой тип MIME, который вы хотите отправить
      //что делать: есть пример fpweb, который получает тип mime из расширения файла ...
      AResponse.ContentLength:=AResponse.ContentStream.Size; //видимо не происходит автоматически?
      AResponse.SendContent;
    finally
      AResponse.ContentStream.Free;
    end;
    
    Handled := true;
    

### Error: Could not determine HTTP module for request

У вас может быть несколько модулей и несколько actions. Если вы укажете URL-адрес только с 1 элементом, например: 
    
    
    http://localhost/cgi-bin/somemodule
    

тогда fpweb предполагает, что вы указываете действие. Если у вас не установлен модуль по умолчанию, вы получите внутреннюю ошибку сервера 500 (не удалось определить модуль HTTP для запроса) 

Вы можете изменить это поведение, чтобы вместо этого fpweb отображал имя модуля, установив для свойства приложения `PreferModuleName` значение `true`. 

### Error: response code 500 Internal Server error when trying to handle DELETE method

В FPC 2.6.2 и ниже fcl-web не принимает метод DELETE и выдает ошибку. 

### fpWeb/FCGI и Apache 2.4 (режим mod_proxy_fcgi)

1, Enable following modules: setenvif_module, proxy_module and proxy_fcgi_module   
(on CentOS 7 modules are defined in file /etc/httpd/conf.modules.d/00-base.conf and /etc/httpd/conf.modules.d/00-proxy.conf). Если вы хотите развернуть приложение FCGI позади прокси-сервера Apache 2.4+, вам понадобится переменная PATH_INFO в заголовках, которые отправляются через сокет демону FCGI. 

1\. Включите следующие модули: setenvif_module, proxy_module и proxy_fcgi_module   
(в CentOS 7 модули определены в файлах /etc/httpd/conf.modules.d/00-base.conf и /etc/httpd/conf.modules.d/00-proxy.conf). 
    
    
    LoadModule setenvif_module   modules/mod_setenvif.so
    LoadModule proxy_module      modules/mod_proxy.so
    LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so
    

2\. Конфигурация обратного прокси для fpWeb/FCGI, прослушивающего порт 9000.  
$1 = module name  
$2 = action name 
    
    
    SetEnvIf Request_URI . proxy-fcgi-pathinfo=full
    ProxyPassMatch "/fcgi/(.*)/(.*)" "fcgi://127.0.0.1:9000/$1/$2"
    

### fpWeb/FCGI и nginx

Для правильной маршрутизации fcl-web требует переменную PATH_INFO в заголовках, отправленных из nginx. Для этого вы должны разделить весь URL-адрес на имя приложения FastCGI и часть информации о пути с помощью оператора конфигурации _fastcgi_split_path_info_. Оператор принимает регулярное выражение и помещает второе совпадение в конфигурационную переменную _$fastcgi_path_info_. Эту переменную можно передать вашему приложению с помощью оператора _fastcgi_param_. 

Следующий пример конфигурации включает в себя полноценный виртуальный хост для nginx, который передает все из _<http://myserver:8080/mycgiapp/>_ во внешнее приложение FastCGI. 
    
    
    server {
    
        listen      8080;
        listen      [::]:8080;
    
        server_name _;
    
        root    "/var/www";
    
        location /mycgiap/ {
            # setting up PATH_INFO
            fastcgi_split_path_info ^(/mycgiapp/)(.*)$;
            fastcgi_param  PATH_INFO          $fastcgi_path_info;
    
            # other parameters
            fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
            fastcgi_param  QUERY_STRING       $query_string;
            fastcgi_param  REQUEST_METHOD     $request_method;
            fastcgi_param  CONTENT_TYPE       $content_type;
            fastcgi_param  CONTENT_LENGTH     $content_length;
            fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;
            fastcgi_param  REQUEST_URI        $request_uri;
            fastcgi_param  DOCUMENT_URI       $document_uri;
            fastcgi_param  DOCUMENT_ROOT      $document_root;
            fastcgi_param  SERVER_PROTOCOL    $server_protocol;
            fastcgi_param  REQUEST_SCHEME     $scheme;
            fastcgi_param  HTTPS              $https if_not_empty;
            fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;
            fastcgi_param  SERVER_SOFTWARE    nginx/$nginx_version;
            fastcgi_param  REMOTE_ADDR        $remote_addr;
            fastcgi_param  REMOTE_PORT        $remote_port;
            fastcgi_param  SERVER_ADDR        $server_addr;
            fastcgi_param  SERVER_PORT        $server_port;
            fastcgi_param  SERVER_NAME        $server_name;
    
            # pass requests to your FastCGI application
            fastcgi_pass 127.0.0.1:9000;
        }
    }
    

## Примечание

  * Модуль cgiapp устарел, по возможности используйте fpcgi.


  * Модули fastcgi, custfcgi и fpfcgi не поддерживаются в Darwin в настоящее время, потому что он использует другой механизм, чем опция MSG_NOSIGNAL для recv(), чтобы указать, что SIGPIPE не должен генерироваться в случае разрыва соединения. См. <http://lists.apple.com/archives/macnetworkprog/2002/Dec/msg00091.html>, чтобы узнать, как это исправить.


  * Если вы развертываете свое приложение CGI и получаете такие ошибки, как "Error: No action name and no default action", вы должны убедиться, что для URL-адреса назначено действие, или перехватить неподдерживаемые действия с действием, отмеченным по умолчанию. В обоих случаях следует использовать обработчик события `OnRequest` для действия, которое устанавливает `Handled:=true`.



## См. также

  * [fpWeb Tutorial](<../en/fpWeb_Tutorial.md> "fpWeb Tutorial")
  * [fphttpclient](<../en/fphttpclient.md> "fphttpclient") \- Часть fcl-web, которую можно использовать отдельно в клиентских приложениях
  * _write me!_ fphttpserver - Небольшой автономный веб-сервер Object Pascal. Может использоваться для обслуживания приложений fcl-web CGI.
  * [CGI_Web_Programming#Debugging_CGI](<../en/CGI_Web_Programming.md> "CGI Web Programming") Информация об отладке приложений CGI
  * [Часть учебника Leonardo Ramé](<https://github.com/leonardorame/Ext4-MVC-Tutorial/wiki/Lazarus-Server>), в котором показано, как писать приложение fcl-web CGI; включает пример создания содержимого JSON

---

_Source: [https://wiki.freepascal.org/fcl-web/ru](https://web.archive.org/web/20250213000451/https://wiki.freepascal.org/fcl-web/ru)_
