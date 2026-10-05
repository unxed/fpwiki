# pas2js

│ **[English (en)](<../en/pas2js.md> "pas2js")** │  **русский (ru)** │    
****

## Contents

  * 1 Pas2js: что это?
    * 1.1 Компилятор
    * 1.2 RTL
  * 2 Где его взять
    * 2.1 Снапшоты
    * 2.2 SVN
  * 3 Как использовать pas2js
    * 3.1 для браузера
    * 3.2 для NodeJS
  * 4 Supported syntax elements
  * 5 Планируемые языковые особенности
  * 6 Другие не реализованные функции
  * 7 Интеграция pas2js в Lazarus
  * 8 Импорт классов Javascript
  * 9 Создание простых объектов JS с новой функцией
  * 10 Отладка
  * 11 Баги
  * 12 Примеры
  * 13 Виджеты Lazarus
  * 14 ЧаВо
    * 14.1 Почему простая программа "hello world" такая большая?
    * 14.2 Почему [нежелательно] применение asm-вставок?
    * 14.3 Почему не парсятся(разбираются) asm-вставки?



# Pas2js: что это?

## Компилятор

Pas2js - это транспилер(транскомпилятор) [кода] [Pascal](<../en/Pascal.md> "Pascal") в [JavaScript](<JavaScript.md> "JavaScript/ru"). Он парсит Object Pascal и выдает JavaScript. JavaScript в настоящее время имеет уровень ECMAScript 5 и должен запускаться в любом браузере или в Node.js (цель "nodejs"). Доступен в 3 формах: 

  * как библиотека
  * как программа командной строки
  * как веб-сервер



Он транскомпилирует [код] из фактических исходников Pascal, у него нет промежуточных файлов .ppu. Это означает, что все исходники должны быть всегда доступны. 

Через определения внешних классов компилятор может использовать классы JavaScript: 

  * Все классы, доступные во время выполнения JavaScript и в браузере, доступны через модули импорта (сравнимо с модулями windows или unix для собственного компилятора).
  * Для Node.js доступна базовая поддержка среды выполнения nodejs.
  * Доступен модуль импорта для jQuery (libjquery)



## RTL

Для работы сгенерированного кода требуется небольшой файл JavaScript: rtl.js. Он определяет объект RTL. Этот объект запустит код Object Pascal, если вы включите вызов `rtl.run()` на странице [HTML](</index.php?title=HTML&action=edit&redlink=1> "HTML \(page does not exist\)"). 
    
    
    <script type="application/javascript">
      rtl.run()
    </script>
    

pas2js может автоматически включить этот файл в сгенерированный вывод, например так: 
    
    
    pas2js -Jc -Jirtl.js -Tbrowser hello.pas
    

Для `nodejs` компилятор вставит вызов `rtl.run()` автоматически в конец сгенерированного файла Javascript. 

Существует базовый Object Pascal RTL, также доступны несколько модулей из пакетов FPC. 

  * system
  * sysutils
  * Math
  * strutils
  * rtlconst
  * classes
  * contnrs
  * DB (да, TDataset)
  * fpcunit testsuite
  * custapp
  * restconnection
  * js (системные объекты javascript)
  * web (объекты, предоставляемые браузером)
  * libjquery (JQuery также доступен)
  * nodejs (среда выполнения базового узла)
  * typinfo
  * objpas
  * browserconsole (поддержка writeln)
  * dateutils
  * browserapp
  * nodejsapp



# Где его взять

Компилятор pas2js и RTL - естественно - с открытым исходным кодом и могут быть загружены и использованы свободно. 

## Снапшоты

Снапшоты содержат двоичные файлы для Windows (32 и 64-разрядная версия), Linux (64-разрядная версия) и MacOS. 

Снапшоты загружены в 

  * <ftp://ftpmaster.freepascal.org/fpc/contrib/pas2js>



Каждая версия имеет каталог с номером версии. Список изменений можно найти на странице журнала изменений [Pas2JS Version Changes](<../en/Pas2JS_Version_Changes.md> "Pas2JS Version Changes")

## SVN
    
    
    svn co https://svn.freepascal.org/svn/projects/pas2js/trunk pas2js
    

Вам нужен [FPC](<../en/FPC.md> "FPC") 3.0.4 или старше, чтобы скомпилировать его. 

Перейдите в каталог и соберите его с помощью: 
    
    
    make clean all
    

Это создаст _compiler/utils/pas2js/pas2js_ (Windows: compiler\utils\pas2js\pas2js.exe) 

# Как использовать pas2js

Аргументы командной строки в основном остаются такими же, как аргументы командной строки FPC. Сообщения об ошибках также в том же формате. 

Компилятору необходим доступ ко всем источникам, поэтому вам необходимо указать путь к источникам всех используемых модулей. 

Что касается компилятора FPC, поддерживается файл конфигурации, который имеет тот же синтаксис, что и файл конфигурации FPC. Обратите внимание, что снапшоты и версия svn уже содержат файл pas2js.cfg по умолчанию с путями поиска модулей (-Fu) для rtl и fcl. 

По сути, команда такая же, как и в любой командной строке FPC. Единственное, что отличается - это цель: '-Tbrowser' или '-Tnodeejs' 

Вот полный список аргументов командной строки: [аргументы командной строки](<../en/pas2js_command_line_arguments.md> "pas2js command line arguments"). 

## для браузера

Рассмотрим классику: 
    
    
    program hello;
    
    begin
      Writeln('Hello, world!');
    end.
    

Да, writeln поддерживается. Вот как это скомпилировать: 
    
    
    pas2js -Jc -Jirtl.js -Tbrowser hello.pas
    

После успешной компиляции код можно запустить в браузере, открыв в браузере html-файл со следующим содержимым: 
    
    
    <html>
      <head>
        <meta charset="utf-8"/>
        <script type="application/javascript" src="hello.js"></script>
      </head>
      <body>
        <script type="application/javascript">
         rtl.run();
        </script>
      </body>
    </html>
    

Необходимые файлы: 

  * hello.html
  * hello.js



Независимо от того, открыт ли hello.html двойным щелчком по нему в проводнике или помещен на сервер и открыт с помощью URL, это не имеет значения. 

Вывод отображается в консоли веб-разработчика браузера. Включив модуль browserconsole, это будет видно на странице браузера: 
    
    
    program hello;
    
    uses browserconsole;
    
    begin
      Writeln('Hello, world!');
    end.
    

## для NodeJS
    
    
    pas2js -Tnodejs hello.pas
    

После успешной компиляции код можно запустить на узле с помощью следующей команды. 
    
    
    nodejs hello.js
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** в MacOS это "node hello.js"

# Supported syntax elements

В основном, поддерживается синтаксис [Delphi](<../en/Delphi.md> "Delphi") 7\. Это включает RTTI. Более подробный список можно найти в источниках файла [translation.html](<https://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/utils/pas2js/docs/translation.html?view=co>). 

  * режим [Delphi](<../en/Mode_Delphi.md> "Mode Delphi") и [ObjFPC](<../en/Mode_ObjFPC.md> "Mode ObjFPC")
  * [Program](<../en/Program.md> "Program"), [Units](<../en/Unit.md> "Unit"), пространства имен
  * инициализация модуля, но не финализация
  * [Var](<../en/Var.md> "Var"), [Const](<../en/Const.md> "Const"), [Type](<../en/Type.md> "Type")
  * string (unicodestring), char (widechar), [Boolean](<../en/Boolean.md> "Boolean"), [Double](<../en/Double.md> "Double"), [Byte](<../en/Byte.md> "Byte"), [Shortint](<../en/Shortint.md> "Shortint"), [Word](<../en/Word.md> "Word"), [Smallint](<../en/Smallint.md> "Smallint"), longword, [Longint](<../en/Longint.md> "Longint"), nativeint(int53), nativeuint(int52), currency
  * resourcestrings
  * [Pointer](<../en/Pointer.md> "Pointer") (как ссылка на класс, массив, запись, указатель записи, интерфейс, но без арифметики указателя)
  * [Record](<../en/Record.md> "Record") (но не variant records)
  * [Functions](<../en/Function.md> "Function"), [Procedures](<../en/Procedure.md> "Procedure"), вложенные, анонимные функции
  * типы функций: of object, reference to (закрытые)
  * аргументы функций: default, const, var, out
  * [If](<../en/If.md> "If")-then-else
  * [For](<../en/For.md> "For")-do
  * [Repeat](<../en/Repeat.md> "Repeat")-until
  * [While](<../en/While.md> "While")-do
  * [With](<../en/With.md> "With")-do
  * try-finally
  * try-except
  * enums
  * sets
  * arrays static, dynamic, open, multi dimensionals
  * String like array operations: _a:=[1,2,3]+[1,1];_
  * class type, visibility, virtual, override, abstract, overload, properties, class properties, class var, class const, constructor, destructor
  * class-of
  * вложенные классы
  * интерфейсы: CORBA, COM, delegations, method resolution, reference counting, TVirtualInterface
  * внешние классы, переменные, константы
  * Энумератор for..in..do
  * Псевдоним типа, например, type TTranslateString = type string;
  * RTTI
  * asm блок для встраивания в JavaScript напрямую
  * директивы компилятора (e.g. $ifdef, $if, $define, $modeswitch, $R+)
  * проверка времени компиляции и времени выполнения



Есть некоторые конструкции, которые естественно не поддерживаются и никогда не будут поддерживаться: 

  * Все, что связано с указателями памяти и арифметикой указателей.
  * Variant records



Подробно о поддерживаемых элементах и переходе с Pascal на JavaScript: [translation](<https://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/utils/pas2js/docs/translation.html?view=co>). 

# Планируемые языковые особенности

По сути, идея состоит в том, чтобы вывести транспайлер pas2js на тот же уровень, что и FPC или Delphi. Это означает, что необходимо добавить следующее: 

  * Продвинутые записи(records)
  * Проверка во время выполнения: переполнение(overflow) -Co, $Q
  * Обобщения(дженерики)
  * Тип помощников(helpers)
  * Массив констант



Излишне говорить, что все, что требует прямого доступа к памяти, не будет поддерживаться. 

# Другие не реализованные функции

  * Атрибуты
  * Перечисления с пользовательскими значениями
  * Глобальные свойства
  * Futures
  * Помощники(helpers) для типов, классов, записей
  * Inline
  * Библиотека
  * Объекты
  * Перегрузка оператора
  * Арифметика указателей
  * Ресурсы
  * Расширение RTTI, $RTTI
  * Вариантные записи(records)
  * Варианты(variants)



# Интеграция pas2js в Lazarus

Lazarus понимает концепцию внешних классов, используемую pas2js, поэтому автозавершение кода будет работать. 

Начиная с Lazarus 1.9, среда IDE может использовать pas2js.exe в качестве обычного компилятора. 

Интеграция описана здесь: [Интеграция pas2JS в Lazarus](<lazarus_pas2js_integration.md> "lazarus pas2js integration/ru"). Он все еще находится в стадии разработки, но планируется глубокая интеграция с Lazarus. 

# Импорт классов Javascript

Чтобы импортировать класс javascript, нужно написать обычное определение класса, которое имитирует класс Javascript. Можно использовать свойства. Многие примеры можно найти в модулях JS, web, nodejs и libjquery. 

Вот простой пример: 
    
    
      TJSFunction = class external name 'Function'(TJSObject)
      private
        Flength: NativeInt external name 'length';
        Fprototyp: TJSFunction external name 'prototyp';
      public
        name: String;
        property prototyp: TJSFunction read Fprototyp;
        property length: NativeInt read Flength;
        function apply(thisArg: TJSObject; const ArgArray: TJSValueDynArray): JSValue; varargs;
        function bind(thisArg: TJSObject): JSValue; varargs;
        function call(thisArg: TJSObject): JSValue; varargs;
      end;
    

[Приведенный выше код] объявляет объект `TJSFunction`: в Javascript функции являются объектами. 

  * "`external name 'Function'`" означает, что вы объявляете класс Javascript, где именем класса Javascript является 'Function'.
  * `(TJSObject)` означает, что он наследуется от `TJSObject` и внешнего класса. Там не должно быть типа предка.
  * Поля объявлены так же, как в Паскале.
  * Чтобы объявить поля только для чтения, можно использовать хитрость: объявите поле с использованием модификатора `external name "thename"` и объявите свойство только для чтения с тем же именем (см. length declaration)
  * `Varargs` может использоваться, чтобы указать, что функция принимает любое количество аргументов.
  * `JSValue` может использоваться для указания неизвестного типа. Это более или менее эквивалентно типу Variant.



# Создание простых объектов JS с новой функцией

Некоторые функции JS-фреймворка ожидают JS-объект в качестве параметра. Вот как это сделать в Pascal, используя функцию _new_ из модуля _JS_ : 
    
    
    // JavaScript:
    DoIt({name:"Fred", id:3, size:4.3});
    
    
    
    // Pascal;
    DoIt(new(['name','Fred', 'id',3, 'size',4.3]));
    

Вы можете вложить его для создания субобъектов: 
    
    
    // JavaScript:
    DoIt({name:"Fred", size:{width:3,height:2}});
    
    
    
    // Pascal;
    DoIt(new(['name','Fred', 'size',new(['width',3, 'height',2])]));
    

Вы можете использовать `TJSArray._of` для создания JS-массивов "на лету": 
    
    
    // JavaScript:
    DoIt({numbers:[1,2,3]});
    
    
    
    // Pascal;
    DoIt(new(['numbers',TJSArray._of(1,2,3)]));
    

# Отладка

Сгенерированный исходный код Javascript, конечно, видим и доступен отладке в браузере. 

Более того, транспайлер может генерировать файл-маппер ([прим.перев](</User:Zoltanleo> "User:Zoltanleo"): это минифицированный/объединённый файл, связанный с файлами, из которых он получился), что означает, что вы сможете увидеть и отладить код Pascal в браузере (будет работать не [абсолютно] все, но многое. Это зависит [еще] и от браузера.) 

файл-маппер может быть сгенерирован с использованием параметра командной строки 
    
    
    -Jm
    

Проще всего включать исходники Pascal в файл-маппер 
    
    
    -Jminclude
    

You can tell the compiler to store all file names relative to a local base directory: Вы можете указать компилятору хранить все имена файлов в [путях] относительно локального базового каталога: 
    
    
    -Jmbasedir=DirName
    

И вы можете сохранять URL-адрес на карте, поэтому браузер будет использовать URL/вышеуказанное-относительное-имя-файла, чтобы получать исходный код: 
    
    
    -Jmsourceroot=URL
    

# Баги

Пожалуйста, сообщайте об ошибках в багтрекере FPC в категории _pas2js_ : <http://bugs.freepascal.org>

# Примеры

  * Приложение для отслеживания времени: (исходный код: <https://www.devstructor.com/demos/pas2js-time/source.zip>)
  * Рисование и анимация на холсте: <http://ragnemalm.se/images/santa/santa.html> (исходный код: <http://ragnemalm.se/images/santa/>)
  * WebGL: <https://github.com/genericptr/Pas2JS-WebGL#pas2js-webgl>



# Виджеты Lazarus

Конечная цель, конечно, - запустить LCL в Всемирной сети. Обсуждения по этой теме делегированы на отдельной странице. [pas2js_widgetsets](<../en/pas2js_widgetsets.md> "pas2js widgetsets")

# ЧаВо

## Почему простая программа "hello world" такая большая?

Это в основном связано с использованием rtl.js. Файл rtl.js содержит код для паскалевских модулей, классов, RTTI, наборов, проверок диапазона и т.д., и написан с учетом больших WebApp, а не для сценариев с несколькими строками кода. 

  1. Вы можете использовать Javascript [minifier ("минимизатор")](<../en/pas2js_minifier.md> "pas2js minifier"), чтобы уменьшить созданный Javascript
  2. Вы можете создать свой собственный "минимизатор" rtl.js, удалив все ненужные вам функции. В конечном итоге это будет сделано автоматически pas2js.



  


## Почему [нежелательно] применение asm-вставок?

Asm-вставки полезны для вещей, которые вы не можете сделать с [помощью] pas2js. Но есть некоторые недостатки: pas2js не разбирает JS. Он также не проверяет синтаксис и не знает, на какие идентификаторы Паскаля ссылается код. Это означает, что любой идентификатор, доступный только блоку asm, будет удален оптимизатором pas2js. 

Поэтому всегда старайтесь делать это на Паскале. Помните, что вы можете типизировать значения в JSValue, объекты в TJSObject, массивы в TJSArray, строки в TJSString и т.д., чтобы использовать почти все функции JS. 

## Почему не парсятся(разбираются) asm-вставки?

Любой анализатор JS времени компиляции может выполнять только проверку синтаксиса и анализировать только простые JS. Но так как простой JS может быть лучше написан на Паскале, это несколько бессмысленно и поэтому имеет низкий приоритет.

---

_Source: [https://wiki.freepascal.org/pas2js/ru](https://web.archive.org/web/20250122113214/https://wiki.freepascal.org/pas2js/ru)_
