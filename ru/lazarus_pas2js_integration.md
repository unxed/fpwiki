# lazarus pas2js integration

│ **[English (en)](<../en/lazarus_pas2js_integration.md>)** │  **русский (ru)** │

## Contents

  * 1 Интеграция pas2JS в Lazarus
    * 1.1 Глобальные настройки
    * 1.2 Мастера проекта
      * 1.2.1 Новое приложение веб-браузера
    * 1.3 Опции проекта
    * 1.4 Окно отладки
    * 1.5 Навигация



# Интеграция pas2JS в Lazarus

Требуется Lazarus 1.9+ и, либо снапшот pas2js, либо svn-архив ([см. здесь](<../en/pas2js.md> "pas2js")). 

Установите пакет **pas2jsdsgn** (components/pas2js/pas2jsdsgn.lpk) и перезапустите IDE. 

[![pas2jsdsgn install](https://upload.wikimedia.org/wikipedia/commons/e/ee/Pas2jsdsgn-install.png)](</File:Pas2jsdsgn-install.png> "pas2jsdsgn install")

Затем задайте путь к pas2js (_pas2jsfolder\compiler\utils\pas2js\pas2js.exe_) в _Tools / Options / Environment / Pas2JS / Path of pas2js_. 

Интеграция [pas2js](<../en/pas2js.md> "pas2js") в Lazarus состоит из 3 частей: 

  * [Глобальные настройки](<lazarus_pas2js_integration.md> "lazarus pas2js integration/ru")
  * [Мастеры проекта](<lazarus_pas2js_integration.md> "lazarus pas2js integration/ru")
  * [Окно отладки](<lazarus_pas2js_integration.md> "lazarus pas2js integration/ru")



## Глобальные настройки

В диалоговом окне Tools/Options... в разделе Pas2JS можно указать некоторые параметры, которые влияют на интеграцию IDE. Это выглядит так: 

[![pas2jsdsgn globaloptions.png](https://wiki.freepascal.org/images/0/06/pas2jsdsgn_globaloptions.png)](</File:pas2jsdsgn_globaloptions.png>)

Доступны следующие настройки: 

  * **Path of pas2js** (Путь к pas2js)  
Это двоичный файл компилятора, используемый при настройке нового проекта.   
Он будет найден в переменной PATH, если не указан абсолютный путь. Соответствует макросу IDE _Pas2JSJS_.
  * **Path of simpleserver** (Путь к простому серверу)  
Это веб-сервер, который запускается при запуске проекта, для которого требуется веб-сервер.   
По умолчанию это приложение simpleserver из проекта FPC, но оно может быть другим веб-сервером, если оно принимает параметр -p для установки пути, и он обслуживает файлы из каталога, в котором он был запущен.
  * **Port numbers to start allocating from simpleserver** (Номера портов, чтобы начать размещение от простого сервера)  
Каждый раз, когда вы запускаете новый проект веб-сервера, для него назначается новый номер порта. (вы все еще можете редактировать это в новом диалоге проекта).
  * **Browser to use when opening HTML page** (Используемый при открытии HTML-страницы браузер)  
Среда IDE будет использовать этот браузер для открытия вашей HTML-страницы.   
Будет выполнен поиск в переменной PATH, если не указан абсолютный путь. Соответствует макросу IDE _Pas2JSBrowser_.
  * **Node.js executable** (Исполняемый файл Node.js)  
Среда IDE будет использовать этот исполняемый файл Node.js для запуска Node.js проекта.   
Будет выполнен поиск в PATH, если не указан абсолютный путь. Соответствует макросу IDE _Pas2JSNodeJS_.



## Мастера проекта

Поддержка pas2js находится в пакете pas2jsdsgn.lpk, который вы можете найти в каталоге components/pas2js. Он регистрирует 2 мастера в диалоге 'New project': 

  * Приложение веб-браузера
  * Приложение Node.js



Помимо создания исходного источника проекта, оба варианта будут также 

  * Устанавливать команду компилятора для компиляции с pas2js
  * Изменять команду 'Run' [так], чтобы работала опция 'Run without debugging («Выполнять без отладки»): она откроет проект в браузере или запустит его с помощью nodejs.



### Новое приложение веб-браузера

Этот мастер запросит некоторые параметры перед созданием нового проекта. Диалог показан ниже. 

[![webbrowserappoptions.png](https://wiki.freepascal.org/images/f/fc/webbrowserappoptions.png)](</File:webbrowserappoptions.png>)

Доступны следующие опции:: 

  * **Create initial HTML page** (Создать начальную HTML-страницу)   
Это не требует пояснений, в среде IDE будет сгенерирована HTML-страница шаблона, которая включает в себя все, что необходимо для запуска кода pas2js.   
Страница включена в описание проекта, поэтому ее можно открывать и редактировать из среды IDE.
  * **Maintain HTML page** (Поддержка HTML-страницы)  
Если вы измените имя проекта, IDE обновит ссылки в файле HTML (все ваши изменения будут утеряны)
  * **Run rtl in document.onReady** (Запускать rtl в [событии] document.onReady)  
По умолчанию тег сценария, который запускает бросок мяча, будет вызывать `rtl.run()`.   
При выборе этой опции изменяется сценарий, поэтому вместо этого `rtl.run` запускается в событии `document.onReady` HTML.   
Это необходимо, если ваш код содержит код запуска, который ссылается на элементы в HTML.   
Элементы будут доступны только после [наступления] события onReady.
  * **Use Browser Application object** (Использовать объект приложения браузера)  
Это изменяет код для использования объекта `TBrowserApplication`.   
Это - потомок `TCustomApplication`, который предлагает поддержку параметров запроса и т.д., как если бы они были параметрами командной строки.
  * **Use Browser Console unit** (Использовать модуль консоли браузера)   
Пометка этого [пункта] просто включит модуль browserconsole в раздел uses.   
Это подключает к системным модулям команду writeln: любой вывод будет добавлен в HTML.   
Сгенерированный HTML имеет [тэг] div с правильным идентификатором, к которому добавляется вывод.
  * **Project needs a HTTP server** (Проекту нужен HTTP-сервер)  
Если проекту требуется HTTP-сервер, то среда IDE может запустить его для вас или вы можете указать URL-адрес, который будет использоваться при использовании команды 'Run without debugging' («Выполнить без отладки»).



## Опции проекта

Вы можете изменить параметры, выбранные в мастере нового проекта в диалоговом окне Project Settings. Это также позволяет преобразовывать любой существующий (простой) проект в проект веб-приложения. 

Параметры проекта находятся внизу [списка] 'Web Project (pas2js)' 

[![pas2jsprojectoptions.png](https://wiki.freepascal.org/images/a/a5/pas2jsprojectoptions.png)](</File:pas2jsprojectoptions.png>)

Флажок вверху помечает проект как проект веб-браузера. 

Все остальные параметры имеют те же значения, что и встречавшиеся выше в мастере 'New project' их части. 

**Reset compile command** (Сброс команды компиляции) и **Reset run command** (Сброс команды запуска) устанавливают соответствующие команды в их значения по умолчанию. 

## Окно отладки

Когда вы запускаете проекты из разных каталогов, IDE запускает веб-серверы по мере необходимости. Через некоторое время может быть запущено несколько веб-серверов. Обзор запущенных веб-серверов можно отобразить с помощью пункта меню View/Debug Windows/Pas2JS Webservers. 

Он показывает список процессов, запущенных IDE: 

[![webservers.png](https://wiki.freepascal.org/images/b/b9/webservers.png)](</File:webservers.png>)

Будет добавлена поддержка остановки/запуска процессов, а также статистика количества запросов и т.д. 

## Навигация

  * Назад к [pas2js](<../en/pas2js.md> "pas2js")
  * Назад к [Pas2JS Version Changes](<../en/Pas2JS_Version_Changes.md> "Pas2JS Version Changes")

---

_Source: [https://wiki.freepascal.org/lazarus_pas2js_integration/ru](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/lazarus_pas2js_integration/ru)_
