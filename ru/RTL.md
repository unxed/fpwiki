# RTL

│ **[English (en)](<../en/RTL.md>)** │  **русский (ru)** │

Библиотека времени выполнения (RTL) 

Библиотека времени выполнения (_Run-Time Library_ \- RTL) \- это набор файлов с [исходным кодом](<../en/Source_code.md> "Source code"), которые используются для создания той части [приложения](<../en/Application.md> "Application"), которыя генерируются или подключается [компилятором](<../en/Compiler.md> "Compiler") и используется для следующих целей: 

  * [Самоинициализация](<../en/Initialization.md> "Initialization") RTL перед активацией приложения пользователем.
  * [Инициализация](<../en/Initialization.md> "Initialization") и [запуск](</index.php?title=startup&action=edit&redlink=1> "startup \(page does not exist\)") приложения.
  * Предоставление стандартных возможностей языка Pascal приложению (например, поддержка [стандартных функций](</index.php?title=standard_function&action=edit&redlink=1> "standard function \(page does not exist\)") [Write](<../en/Write.md> "Write") и [Writeln](</index.php?title=Writeln&action=edit&redlink=1> "Writeln \(page does not exist\)")).
  * Предоставление любой [функции библиотеки](</index.php?title=library_function&action=edit&redlink=1> "library function \(page does not exist\)"), которая не определена компилятором как [inline](</index.php?title=inline&action=edit&redlink=1> "inline \(page does not exist\)"), например, математических подпрограмм.


  * Обеспечение расширенных возможностей Pascal для приложений.
  * Обеспечения преобразования между стандартными и расширенными возможностями функций. (Например, одна и та же функция Write или writeln может вывести текст в окне, если переменная указывает на окно; в окно терминала, если переменная указывает на терминал или сохранить текст в файл, если переменная указывает на внешний файл.)



## RTL модули

Для поддержки различных платформ а так же стандартов языка Pascal (TP\BP и Delphi), существуют множество функций, которые часто дублируются. Например, одна и та же функция Write или Writeln может иметь совершенно разные реализации для Windows и Linux платформ. Общий обзор классификаций модулей можете просмотреть [здесь](<../en/Unit_categorization.md> "Unit categorization"). 

  


## Использование RTL

Общие проблемы при использовании модулей [crt](<../en/crt_unit.md> "crt unit") и [video](<../en/video_unit.md> "video unit") в unix терминалах описаны здесь: [Terminal & Fonts](<../en/Terminal_&_Fonts.md> "Terminal & Fonts"). 

Узнать об API модулях (Video/Mouse/Keyboard) и Crt в Unix можете [тут](<../en/KVM_API_and_Crt_future.md> "KVM API and Crt future"). 

Модулям для ОС Windows посвящена [отдельная страница](<../en/Windows_API_units.md> "Windows API units"). 

## Развите RTL

[Статьи, посвященные разработке RTL](<../en/RTL_development_articles.md> "RTL development articles")

---

_Source: [https://wiki.freepascal.org/RTL/ru](https://web.archive.org/web/20250216030921/https://wiki.freepascal.org/RTL/ru)_
